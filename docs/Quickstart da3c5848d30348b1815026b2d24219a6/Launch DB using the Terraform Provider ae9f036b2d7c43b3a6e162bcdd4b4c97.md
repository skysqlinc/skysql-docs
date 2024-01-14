# Launch DB using the Terraform Provider

This walkthrough explains how to launch database services and manage the lifecycle of database services using the Terraform provider.

For users who prefer other interfaces, SkySQL offers the following alternatives:

- Use the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/) in a web browser
- Use the [DBaaS API](https://mariadb.com/docs/skysql-dbaas/nr-quickstart/dbaas-api-launch-walkthrough/) with a REST client

This walkthrough demonstrates a service configuration that is suitable for a quick test. A more customized configuration should be selected for performance testing or for alignment to the needs of production workloads.

<aside>
💡 This procedure uses Terraform. HashiCorp officially supports Terraform on several Linux distributions, but HashiCorp also provides binaries for Microsoft Windows, macOS, and other operating systems.

For a list of operating systems that are officially supported for Terraform, see "[HashiCorp Terraform Documentation: Supported Operating Systems](https://developer.hashicorp.com/terraform/enterprise/requirements/os-specific/supported-os)".

For a list of operating systems that have binaries available for Terraform, see "[HashiCorp Terraform Documentation: Install Terraform](https://developer.hashicorp.com/terraform/downloads)".

</aside>

# Dependencies

- This procedure requires Terraform to be installed. For information about how to install Terraform, see "[HashiCorp Terraform Documentation: Install Terraform](https://developer.hashicorp.com/terraform/downloads)".
- The examples in this procedure also use `jq`, a JSON parsing utility. [jq](https://stedolan.github.io/jq/download/) is available for Linux, macOS, and MS Windows. Install `jq` then proceed.
- The examples in this procedure also use `curl`, a data transfer utility. [curl](https://curl.se/) is available for Linux, macOS, and MS Windows. Install `curl` then proceed.
- The examples in this procedure also use `wget`, a file download utility. [GNU Wget](https://www.gnu.org/software/wget/) is available for Linux, macOS, and MS Windows. Install `wget` then proceed.
- The examples in this procedure also use exported environment variables that are compatible with Bourne-like shells (such as `sh`, `bash`, and `zsh`).

# Launch a Service

### **Step 1: Generate API Key**

1. Go to the [Generate API Key](https://id.mariadb.com/account/api/generate-key) page.
2. Fill out the API key details:
    - In the "Description" field, describe the purpose of the API key.
    - In the "Expiration" field, specify how long this key will be valid. If you need to revoke the key before it expires, you can revoke it from the [API Keys](https://id.mariadb.com/account/api) page.
    - In the "Scopes" field, select the "read" and "write" scopes under `SkySQL API: Databases`.
3. Click the "Generate API Key" button.
4. After the page refreshes, click the "Copy to clipboard" button to copy the API key.
5. Paste the API key somewhere safe and do not lose it.

### **Step 2: Create Terraform Project Directory**

Create a directory for your Terraform project and change to the directory:

`$ mkdir -p ~/skysql-nr-tf
$ cd ~/skysql-nr-tf`

### **Step 3: Create `main.tf`**

In the Terraform project directory, create a `main.tf` file that contains the following:

- [Provider Requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider Configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Resources](https://developer.hashicorp.com/terraform/language/resources/syntax)
- [Data Sources](https://developer.hashicorp.com/terraform/language/data-sources)

```yaml
# ---------------------
# Provider Requirements
# ---------------------
# TF Documentation: https://developer.hashicorp.com/terraform/language/providers/requirements

terraform {
  required_providers {
    skysql = {
      source          = "registry.terraform.io/mariadb-corporation/skysql"
    }
  }
}

# ----------------------
# Provider Configuration
# ----------------------
# TF Documentation: https://developer.hashicorp.com/terraform/language/providers/configuration

provider "skysql" {
   access_token       = var.api_key
}

# ---------
# Resources
# ---------
# TF Documentation: https://developer.hashicorp.com/terraform/language/resources/syntax

# Create a service
resource "skysql_service" "default" {
  service_type        = var.service_type
  topology            = var.topology
  cloud_provider      = var.cloud_provider
  region              = var.region
  availability_zone   = coalesce(var.availability_zones, data.skysql_availability_zones.default.zones[0].name)
  architecture        = var.architecture
  size                = var.size
  storage             = var.storage
  nodes               = var.nodes
  version             = coalesce(var.sw_version, data.skysql_versions.default.versions[0].name)
  name                = var.name
  ssl_enabled         = var.ssl_enabled
  deletion_protection = var.deletion_protection
  wait_for_creation   = true
  wait_for_deletion   = true
  wait_for_update     = true
  is_active           = true
  allow_list          = [
     {
        "ip"          : var.ip_address,
        "comment"     : var.ip_address_comment
     }
  ]
}

# ------------
# Data Sources
# ------------
# TF Documentation: https://developer.hashicorp.com/terraform/language/data-sources

# Retrieve the list of projects. Projects are a way to group services.
data "skysql_projects" "default" {}

# Retrieve the list of available versions for a specific topology
data "skysql_versions" "default" {
  topology            = var.topology
}

# Retrieve the service details
data "skysql_service" "default" {
  service_id          = skysql_service.default.id
}

# Retrieve the service default credentials.
# When the service is created please change the default credentials
data "skysql_credentials" "default" {
  service_id          = skysql_service.default.id
}

data "skysql_availability_zones" "default" {
  region              = var.region
  filter_by_provider  = var.cloud_provider
}
```

### **Step 4: Create `outputs.tf`**

In the Terraform project directory, create an `outputs.tf` file that contains the [output values](https://developer.hashicorp.com/terraform/language/values/outputs) used to display metadata about the SkySQL service:

```yaml
# -------------
# Output Values
# -------------
# TF Documentation: https://developer.hashicorp.com/terraform/language/values/outputs

output "skysql_projects" {
  value = data.skysql_projects.default
}

# Show the service details
output "skysql_service" {
  value = data.skysql_service.default
}

# Show the service credentials
output "skysql_credentials" {
  value     = data.skysql_credentials.default
  sensitive = true
}

# Example how you can generate a command line for the database connection
output "skysql_cmd" {
  value = "mariadb --host ${data.skysql_service.default.fqdn} --port 3306 --user ${data.skysql_service.default.service_id} -p --ssl-ca ~/Downloads/skysql_chain_2022.pem"
}

output "availability_zones" {
  value = data.skysql_availability_zones.default
}
```

### **Step 5: Create `variables.tf`**

In the Terraform project directory, create a `variables.tf` file that contains the [input variables](https://developer.hashicorp.com/terraform/language/values/variables) used to configure the SkySQL service:

```yaml
# ---------------
# Input Variables
# ---------------
# TF Documentation: https://developer.hashicorp.com/terraform/language/values/variables

variable "api_key" {
   type                 = string
   sensitive            = true
   description          = "The SkySQL API Key generated at: https://id.mariadb.com/account/api/generate-key"
}

variable "service_type" {
   type                 = string
   default              = "transactional"
   description          = "Specify \"transactional\" or \"analytical\". For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/service-types/"
}

variable "topology" {
   type                 = string
   default              = "es-single"
   description          = "Specify a topology. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/topologies/"
}

variable "cloud_provider" {
    type                 = string
    default              = "gcp"
    description          = "Specify the cloud provider. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/providers/"
}

variable "region" {
   type                 = string
   default              = "us-central1"
   description          = "Specify the region. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/regions/"
}

variable "availability_zone" {
   type                 = string
   default              = null
   description          = "Specify the availability zone for the cloud provider and region. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/availability-zones/"
}

variable "architecture" {
   type                 = string
   default              = "amd64"
   description          = "Specify a hardware architecture. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/architectures/"
}

variable "size" {
   type                 = string
   default              = "sky-2x8"
   description          = "Specify the database node instance size. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/instance-sizes/"
}

variable "storage" {
   type                 = number
   default              = 100
   description          = "Specify a transactional storage size. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/storage-sizes/"
}

variable "nodes" {
   type                 = number
   default              = 1
   description          = "Specify a node count. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/node-count/"
}

variable "sw_version" {
   type                 = string
   default              = null
   description          = "Specify a software version. For additional information, see: https://mariadb.com/docs/skysql/ref/skynr/selections/versions/"
}

variable "name" {
   type                 = string
   default              = "skysql-nr-quickstart"
   description          = "Specify a name for the service. For additional information, see: https://mariadb.com/docs/skysql/selections/nr-launch-time-service-name/"
}

variable "ssl_enabled" {
   type                 = bool
   default              = true
   description          = "Specify whether TLS should be enabled for the service. For additional information, see: https://mariadb.com/docs/skysql/selections/nr-launch-time-disable-ssltls/"
}

variable "deletion_protection" {
   type                 = bool
   default              = true
   description          = "Specify whether the service can be deleted via Terraform (false) or whether trying to do so raises an error (true)"
}

variable "ip_address" {
   type                 = string
   description          = "Specify an IP address in CIDR format to add to the service's IP allowlist. For additional information, see: https://mariadb.com/docs/skysql/security/nr-firewall/"
}

variable "ip_address_comment" {
   type                 = string
   description          = "Specify a comment describing the IP address. For additional information, see: https://mariadb.com/docs/skysql/security/nr-firewall/"
}
```

The variables are configured in the next step.

### **Step 6: Configure Service in a `.tfvars` File**

A `[.tfvars` file](https://developer.hashicorp.com/terraform/tutorials/configuration-language/variables#assign-values-with-a-file) can be used to configure the service using the input variables.

For example:

```yaml
api_key             = "... key data ..."
service_type        = "transactional"
topology            = "es-single"
cloud_provider      = "gcp"
region              = "us-central1"
availability_zone   = null
architecture        = "amd64"
size                = "sky-2x8"
storage             = 100
nodes               = 1
sw_version          = null
name                = "skysql-nr-quickstart"
ssl_enabled         = true
deletion_protection = true
ip_address          = "192.0.2.10/32"
ip_address_comment  = "Describe the IP address"
```

The input variables should be customized for your own needs:

- For `api_key`, set it to the API key previously created in "[Step 1: Generate API Key](https://mariadb.com/docs/skysql-dbaas/nr-quickstart/terraform-launch-walkthrough/#Step_1:_Generate_API_Key)".
- For `service_type`, choose a [Service Type Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/service-types/)
- For `topology`, choose a [Topology Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/topologies/)
- For `cloud_provider`, choose a [Cloud Provider Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/providers/)
- For `region`, choose a [Region Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/regions/)
- For `availability_zone`, choose a [Availability Zone Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/availability-zones/) or leave it `null` to use the default availability zone for the cloud provider and region
- For `architecture`, choose a [Hardware Architecture Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/architectures/)
- For `size`, choose an [Instance Size Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/instance-sizes/)
- For `storage`, choose a [Transactional Storage Size Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/storage-sizes/)
- For `nodes`, choose a [Node Count Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/node-count/)
- For `sw_version`, choose the [Software Version Selection](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/versions/) or leave it `null` to use the default version for the topology
- For `name`, choose a [Service Name](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-service-name/) for the new service
- For `deletion_protection`, choose whether the service can be deleted via Terraform (`false`) or whether trying to do so raises an error (`true`)
- For `ip_address`, choose an IP address to allow through the [Firewall](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/)
- For `ip_address_comment`, provide a description for the IP address

The following steps assume that the file is called `skysql-nr-quickstart.tfvars`.

### **Step 7: Run `terraform init`**

Initialize the Terraform project directory and download the Terraform provider from the [Terraform Registry](https://registry.terraform.io/providers/mariadb-corporation/skysql) by executing the `[terraform init` command](https://developer.hashicorp.com/terraform/cli/commands/init):

`$ terraform init`

If you need to download the provider manually, see "[Manually Install Provider from Binary Distribution](https://mariadb.com/docs/skysql-dbaas/nr-quickstart/terraform-launch-walkthrough/#Manually_Install_Provider_from_Binary_Distribution)".

### **Step 8: Run `terraform plan`**

Create a Terraform execution plan by executing the `[terraform plan` command](https://developer.hashicorp.com/terraform/cli/commands/plan) and specifying the path to the `.tfvars` file:

`$ terraform plan -var-file="skysql-nr-quickstart.tfvars"`

### **Step 9: Run `terraform apply`**

Execute the Terraform execution plan and create the SkySQL service by executing the `[terraform apply` command](https://developer.hashicorp.com/terraform/cli/commands/apply) and specifying the path to the `.tfvars` file:

`$ terraform apply -var-file="skysql-nr-quickstart.tfvars"`

Terraform prints the plan from the previous step again and prompts the user to confirm that the plan should be applied:

```yaml
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes
```

Then Terraform creates the objects and prints status messages:

```yaml
skysql_service.default: Creating...
skysql_service.default: Still creating... [10s elapsed]
skysql_service.default: Still creating... [20s elapsed]
skysql_service.default: Still creating... [30s elapsed]
skysql_service.default: Still creating... [40s elapsed]
skysql_service.default: Still creating... [50s elapsed]
skysql_service.default: Still creating... [1m0s elapsed]
skysql_service.default: Still creating... [1m10s elapsed]
skysql_service.default: Still creating... [1m20s elapsed]
skysql_service.default: Still creating... [1m30s elapsed]
skysql_service.default: Still creating... [1m40s elapsed]
skysql_service.default: Still creating... [1m50s elapsed]
skysql_service.default: Still creating... [2m0s elapsed]
skysql_service.default: Still creating... [2m10s elapsed]
skysql_service.default: Still creating... [2m20s elapsed]
skysql_service.default: Still creating... [2m30s elapsed]
skysql_service.default: Still creating... [2m40s elapsed]
skysql_service.default: Still creating... [2m50s elapsed]
skysql_service.default: Still creating... [3m0s elapsed]
skysql_service.default: Still creating... [3m10s elapsed]
skysql_service.default: Still creating... [3m20s elapsed]
skysql_service.default: Still creating... [3m30s elapsed]
skysql_service.default: Creation complete after 3m40s [id=dbpgf00000001]
data.skysql_credentials.default: Reading...
data.skysql_service.default: Reading...
data.skysql_service.default: Read complete after 0s [name=skysql-nr-quickstart]
data.skysql_credentials.default: Read complete after 0s

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

Then Terraform prints the outputs.

### **Step 10: Obtain Connection Credentials**

Obtain the connection credentials for the new SkySQL service by executing the following commands:

1. Download `[skysql_chain_2022.pem](https://supplychain.mariadb.com/skysql/skysql_chain_2022.pem)`, which contains the Certificate Authority chain that is used to verify the server's certificate for TLS:
    
    `$ curl https://supplychain.mariadb.com/skysql/skysql_chain_2022.pem --output ~/Downloads/skysql_chain_2022.pem`
    
2. Obtain the connection command from the `terraform.tfstate` file:
    
    `$ jq ".outputs.skysql_cmd" terraform.tfstate`
    
    `"mariadb --host dbpgf00000001.sysp0000.db.skysql.net --port 3306 --user dbpgf00000001 -p --ssl-ca ~/Downloads/skysql_chain_2022.pem"`
    
3. Obtain the user password from the `terraform.tfstate` file:
    
    `$ jq ".outputs.skysql_credentials.value.password" terraform.tfstate`
    
    `"..password string.."`
    

### **Step 11: Connect**

Connect to the SkySQL service by executing the connection command from the previous step:

`$ mariadb --host dbpgf00000001.sysp0000.db.skysql.net --port 3306 --user dbpgf00000001 -p --ssl-ca ~/Downloads/skysql_chain_2022.pem`

When prompted, type the password and press enter to connect:

```yaml
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 1059
Server version: 10.6.11-6-MariaDB-enterprise-log MariaDB Enterprise Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

### **Step 12: Run `terraform destroy`**

Delete the service by executing the `[terraform destroy` command](https://developer.hashicorp.com/terraform/cli/commands/destroy) and specifying the path to the `.tfvars` file:

`$ terraform destroy -var-file="skysql-nr-quickstart.tfvars"`

Terraform prints the plan to delete the service and prompts the user to confirm that the plan should be applied:

`Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes`

If deletion protection is enabled for the resources, the operation raises an error:

`╷
│ Error: Can not delete service
│
│ Deletion protection is enabled
╵`

If deletion protection is not enabled for the resources, Terraform deletes the resources and prints status messages:

```yaml
skysql_service.default: Destroying... [id=dbpgf00000001]
skysql_service.default: Still destroying... [id=dbpgf00000001, 10s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 20s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 30s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 40s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 50s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 1m0s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 1m10s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 1m20s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 1m30s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 1m40s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 1m50s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 2m0s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 2m10s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 2m20s elapsed]
skysql_service.default: Still destroying... [id=dbpgf00000001, 2m30s elapsed]
skysql_service.default: Destruction complete after 2m38s

Destroy complete! Resources: 1 destroyed.
```

# Manually Install Provider from Binary Distribution

The SkySQL New Release Terraform provider can be downloaded from the [GitHub releases page](https://github.com/mariadb-corporation/terraform-provider-skysql/releases) as a binary distribution and manually installed.

### **Manually Install Provider on Linux**

With **Linux**, manually install the provider on the target system by performing the following steps in the same Bash terminal:

1. Set some environment variables to configure your provider version, OS, and architecture:
    
    `$ export TF_PROVIDER_RELEASE=1.1.0
    $ export TF_PROVIDER_OS=linux
    $ export TF_PROVIDER_ARCH=amd64`
    
    For `TF_PROVIDER_ARCH`, the following architectures are supported on Linux:
    
    - `386`
    - `amd64`
    - `arm`
    - `arm64`
2. Download the provider from GitHub using `wget`:
    
    `$ wget -q https://github.com/mariadb-corporation/terraform-provider-skysql/releases/download/v1.1.0/terraform-provider-skysql_${TF_PROVIDER_RELEASE}_${TF_PROVIDER_OS}_${TF_PROVIDER_ARCH}.zip`
    
3. Create a Terraform plugin directory:
    
    `$ mkdir -p ~/.terraform.d/plugins/registry.terraform.io/mariadb-corporation/skysql`
    
4. Move the provider's binary distribution to the Terraform plugin directory:
    
    `$ mv terraform-provider-skysql_${TF_PROVIDER_RELEASE}_${TF_PROVIDER_OS}_${TF_PROVIDER_ARCH}.zip ~/.terraform.d/plugins/registry.terraform.io/mariadb-corporation/skysql/`
    
5. Verify that the provider's binary distribution is present in the Terraform plugin directory:
    
    `$ ls -l ~/.terraform.d/plugins/registry.terraform.io/mariadb-corporation/skysql/`
    

### **Manually Install Provider on macOS**

With **macOS**, manually install the provider on the target system by performing the following steps in the same macOS Terminal:

1. If [Homebrew](https://brew.sh/) is not installed, install it:
    
    `$ /bin/bash -c "**$(**curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh**)**"`
    
2. Install `wget` using Homebrew:
    
    `$ brew install wget`
    
3. Set some environment variables to configure your provider version, OS, and architecture:
    
    `$ export TF_PROVIDER_RELEASE=1.1.0
    $ export TF_PROVIDER_OS=darwin
    $ export TF_PROVIDER_ARCH=arm64`
    
    For `TF_PROVIDER_ARCH`, the following architectures are supported on macOS:
    
    - `amd64`
    - `arm64`
4. Download the provider from GitHub using `wget`:
    
    `$ wget -q https://github.com/mariadb-corporation/terraform-provider-skysql/releases/download/v1.1.0/terraform-provider-skysql_${TF_PROVIDER_RELEASE}_${TF_PROVIDER_OS}_${TF_PROVIDER_ARCH}.zip`
    
5. Create a Terraform plugin directory:
    
    `$ mkdir -p ~/.terraform.d/plugins/registry.terraform.io/mariadb-corporation/skysql`
    
6. Move the provider's binary distribution to the Terraform plugin directory:
    
    `$ mv terraform-provider-skysql_${TF_PROVIDER_RELEASE}_${TF_PROVIDER_OS}_${TF_PROVIDER_ARCH}.zip ~/.terraform.d/plugins/registry.terraform.io/mariadb-corporation/skysql/`
    
7. Verify that the provider's binary distribution is present in the Terraform plugin directory:
    
    `$ ls -l ~/.terraform.d/plugins/registry.terraform.io/mariadb-corporation/skysql/`
    

# Resources

- [GitHub Project](https://github.com/mariadb-corporation/terraform-provider-skysql)
- [Example Terraform Configuration Files](https://github.com/mariadb-corporation/terraform-provider-skysql/tree/main/examples)
- [Terraform Registry](https://registry.terraform.io/providers/mariadb-corporation/skysql)
- [API Documentation](https://docs.skysql.mariadb.com/)
- [API Reference Documentation](https://mariadb.com/docs/skysql-dbaas/ref/skynr/)