# Setting up GCP Private Service Connect

!!! Note
    GCP Private Service Connect (PSC) is used for private connections within the same GCP region.
    
    Prior to configuring PSC on a SkySQL service, you must have created a VPC with a private subnet that will be used to communicate with private IP addresses. 
    
    For detailed information about GCP PSC, see ["Private Service Connect" (Google documentation)](https://cloud.google.com/vpc/docs/private-service-connect).


The default endpoint mechanism is "`nlb`", which supports accessing the SkySQL service via the public internet. Use of GCP PSC occurs when the endpoint mechanism is changed to "`privateconnect`".

GCP PSC can be enabled using the SkySQL Portal, SkySQL DBaaS API, or SkySQL Terraform Provider.

## **Note the following**

- When a SkySQL service has the `privateconnect` endpoint mechanism, all connections occur through private endpoints. SkySQL does offer users to setup a “secondary” public IP endpoint also (available in the Portal UI when you provision a new service).
- With Private Service Connect, projects are allowlisted and auto-accept of connections from any account is disabled.
- Access restrictions are controlled through the customer's GCP account. VPC firewall rules apply. For detailed information, see ["Cloud Firewall overview" (Google documentation)](https://cloud.google.com/vpc/docs/firewalls).
- Some customers using Private Service Connect may choose to disable TLS, but MariaDB recommends keeping TLS enabled for extra security.
- Database connection limits are independent from PSC connection limits. The limit for PSC connections is 10.
- Endpoint changes can be destructive, resulting in downtime. When you change the connection type from private to public, or public to private, the endpoint must be destroyed and recreated. Changing the SkySQL endpoint between `nlb` and `privateconnect` will result in a service interruption.
- Connections to SkySQL services by features such as SkySQL backups, and monitoring do not depend on Private Service Connect.
- Query Editor is not supported when Private Service Connect is enabled.

### **Enable GCP Private Service Connect on Service Launch**

To enable GCP Private Service Connect when launching a new service via the SkySQL Portal:

- When you get to the final "Security" section, select "Enable Private Service Connect".

For the next step, see the [GCP Endpoint Setup](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#GCP_Endpoint_Setup) sections on this page.

### **Enable GCP Private Service Connect on Existing SkySQL Service**

To enable GCP Private Service Connect for an existing service via the SkySQL Portal:

1. Log in to the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/).
2. Click the "MANAGE" button (at right) for the desired service.
3. In the context menu, choose the "Set up Private Service Connect" menu item.
4. In the popup window, add one or more GCP project IDs.
5. Click the "OK" button to confirm this operation.

For the next step, see the [GCP Endpoint Setup](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#GCP_Endpoint_Setup) sections on this page.

### **Disable GCP Private Service Connect**

To disable GCP Private Service Connect via the SkySQL Portal:

1. Visit the [SkySQL Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/)
2. Find the service that you would like to modify.
3. Click "MANAGE" on the far right side of the service listing.
4. In the context menu, select "Manage Private Service Connect".
5. In the popup window, click "I want to disconnect my Private Service Connect".
6. In the popup window, select "Disconnect".
7. After the service restarts, Private Service Connect is disabled.
8. Since the service's allowlist was cleared when GCP Private Service Connect was previously enabled, you will need to [update the allowlist](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/#Add_to_the_Allowlist) to allow clients to connect after disabling Private Service Connect.

## GCP Endpoint Setup

Endpoint setup is performed using the customer's GCP account.

### **Create a Subnet (optional)**

We recommend use of a subnet dedicated to Private Service Connect endpoints in the same VPC where the application is running.

1. In the GCP console, navigate VPC network → VPC networks → <application VPC> → SUBNETS → ADD SUBNET.
    - Replace <application VPC> with the name of the VPC where the application is running.
2. Configure the subnet:
    - Name
    - Region: select the same region as the one where the application runs
    - Purpose: None
    - IP address range: Set a CIDR block that doesn't overlap with the CIDR blocks of the existing subnets in the same VPC.
    - Optionally configure Private Google Access
    - Optionally configure Flow logs
    - Click "ADD".

### **Create a Static Internal IP Address**

1. In the GCP console, navigate VPC network → VPC networks → <application VPC> → STATIC INTERNAL IP ADDRESSES → RESERVE STATIC ADDRESS.
    - Replace <application VPC> with the name of the VPC where the application is running.
2. Configure the static internal IP address:
    - Name: set to the Database ID (dbxxxxxxxx) from SkySQL.
    - Subnet: select the subnet where to reserve the static IP address.
    - Static IP address: optionally choose the address.
    - Purpose: Non-shared
    - Click "RESERVE".

### ****Connect to the Published Service****

1. In the GCP console, navigate Network services → Private Service Connect → CONNECTED ENDPOINTS → CONNECT ENDPOINT.

2. Configure the endpoint connection:
    ◦ Target: Published service
    ◦ Target service: the `endpoint_service` value from the SkySQL DBaaS API
    ◦ Endpoint name: set to the Database ID from SkySQL (dbxxxxxxxx)
    ◦ Network: select the VPC network where the application is running
    ◦ Subnetwork: select the subnet where the static internal IP address is reserved
    ◦ IP address: select the reserved internal IP address from the prior step
    ◦ Click "ADD ENDPOINT".

## Setting up GCP PSC using SkySQL REST API

### **Enable GCP PSC on Service Launch**

To enable GCP Private Service Connect when launching a new service via the SkySQL DBaaS API:

1. Initiate service launch using the procedure at "[DBaaS API Launch Walkthrough](https://mariadb.com/docs/skysql-dbaas/nr-quickstart/dbaas-api-launch-walkthrough/)".
2. When you are creating the request, add the `"endpoint_mechanism"` and `"endpoint_allowed_accounts"` attributes to the JSON payload:
    
    ```json
    "endpoint_mechanism": "privateconnect",
    "endpoint_allowed_accounts": [
        "GCP-PROJECT-ID-1",
        "GCP-PROJECT-ID-2"
    ]
    ```
    
    - Set `"endpoint_mechanism"` to `"privateconnect"`
    - Set `"endpoint_allowed_accounts"` to a JSON array of one or more customer project IDs in GCP that will be allowed to establish a private connection to the SkySQL service

For the next step, see the [GCP Endpoint Setup](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#GCP_Endpoint_Setup) sections on this page.

### **Enable GCP Private Service Connect on Existing SkySQL Service**

To enable GCP Private Service Connect on an existing service via the SkySQL DBaaS API, create a JSON payload such as:

```json
[
  {
    "mechanism": "privateconnect",
    "allowed_accounts": [
      "GCP-PROJECT-ID-1",
      "GCP-PROJECT-ID-2"
    ]
  }
]
```

- Set `"mechanism"` to `"privateconnect"`
- Set `"allowed_accounts"` to a JSON array of one or more customer project IDs in GCP that will be allowed to establish a private connection to the SkySQL service

And then use the API to update the `endpoints` information of the service. The following example of using `curl` expects the above payload to be in a file named `data.json` and also expects the variables `API_KEY` and `SERVICE_ID` to be set:

```bash
curl -H "Authorization: Bearer ${API_KEY}" \
       -X PATCH -d @data.json \
       https://api.mariadb.com/provisioning/v1/services/${SERVICE_ID}/endpoints \
       | jq .
```

We will need the endpoint's service name later on in the setup, so use the API to query the endpoint once the service has finished its modifications and is back to "ready":

```bash
curl -H "Authorization: Bearer ${API_KEY}" -X GET \
       https://api.mariadb.com/provisioning/v1/services/${SERVICE_ID} \
       | jq '.status,.endpoints[0].ports,.endpoints[0].endpoint_service'
```

Note that the `jq` command is filtering the returned JSON data to extract three things (which are separated by commas in the command above):

- The status of the service, which must be "ready" for the other values to be available.
    - The "`ready`" status means that the Private Service Connect endpoint setup is complete.
    - A status of "`pending_upgrade`" is reflected while Private Service Connect endpoint setup is in-progress.
- The "ports" data for the first item in the `endpoints` array.
    - Some topologies include multiple port options, including ports for read/write and read-only connections, or for connections to the NoSQL listener.
    - Choose the desired port, such as "`readwrite`".
- The `endpoint_service` value for the first item in the `endpoints` array.

The output will look something like this, though your values will vary:

```json
"ready"
[
  {
    "name": "readwrite",
    "port": 3306,
    "purpose": "readwrite"
  }
]
"GCP_ENDPOINT_HOSTNAME"
```

If you are not using `jq`, scan (or parse) the full returned JSON data to ensure the service status is "ready" and find the associated values described above.

For the next step, see the [GCP Endpoint Setup](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#GCP_Endpoint_Setup) sections on this page.

### **Disable GCP PSC**

To disable GCP Private Service Connect via the SkySQL DBaaS API, create a JSON payload such as:

```json
[
  {
    "mechanism": "nlb"
  }
]
```

- Set `"mechanism"` to `"nlb"`

And then use the API to update the `endpoints` information of the service. The following example of using `curl` expects the above payload to be in a file named `data.json` and also expects the variables `API_KEY` and `SERVICE_ID` to be set:

```bash
curl -H "Authorization: Bearer ${API_KEY}" \
       -X PATCH -d @data.json \
       https://api.mariadb.com/provisioning/v1/services/${SERVICE_ID}/endpoints \
       | jq .
```

We will need the endpoint's service name later on in the setup, so use the API to query the endpoint once the service has finished its modifications and is back to "ready":

```bash
curl -H "Authorization: Bearer ${API_KEY}" -X GET \
       https://api.mariadb.com/provisioning/v1/services/${SERVICE_ID} \
       | jq '.status,.endpoints[0].ports,.endpoints[0].endpoint_service'
```

Note that the `jq` command is filtering the returned JSON data to extract three things (which are separated by commas in the command above):

- The status of the service, which must be "ready" for the other values to be available.
    - The "`ready`" status means that the endpoint modification is complete.
    - A status of "`pending_upgrade`" is reflected while Private Service Connect is being disabled.
- The "ports" data for the first item in the `endpoints` array.
    - Some topologies include multiple port options, including ports for read/write and read-only connections, or for connections to the NoSQL listener.
    - Choose the desired port, such as "`readwrite`".
- The `endpoint_service` value for the first item in the `endpoints` array.

The output will look something like this, though your values will vary:

```json
"ready"
[
  {
    "name": "readwrite",
    "port": 3306,
    "purpose": "readwrite"
  }
]
"GCP_ENDPOINT_HOSTNAME"
```

If you are not using `jq`, scan (or parse) the full returned JSON data to ensure the service status is "ready" and find the associated values described above.

Since the service's allowlist was cleared when GCP Private Service Connect was previously enabled, you will need to update the allowlist to allow clients to connect after disabling Private Service Connect.

To add an address to the allowlist via the SkySQL DBaaS API, create a JSON payload such as:

```json
**{**  **"ip_address":** "192.0.2.10/32"**}**
```

And then use the API to update the `allowlist` information of the service. The following example of using `curl` expects the above payload to be in a file named `data.json` and also expects the variables `API_KEY` and `SERVICE_ID` to be set:

```bash
curl -H "Authorization: Bearer ${API_KEY}" \
       -X POST -d @data.json \
       https://api.mariadb.com/provisioning/v1/services/${SERVICE_ID}/security/allowlist \
       | jq .
```

## **Terraform Provider**

GCP Private Service Connect can be enabled with Terraform using the SkySQL Terraform provider. 

For general instructions on using the SkySQL Terraform Provider, see "[Terraform Launch Walkthrough](https://mariadb.com/docs/skysql-dbaas/nr-quickstart/terraform-launch-walkthrough/)".

For an example Terraform configuration that enables GCP Private Service Connect, see "[https://github.com/mariadb-corporation/terraform-provider-skysql/tree/main/examples/private-service-connect](https://github.com/mariadb-corporation/terraform-provider-skysql/tree/main/examples/private-service-connect)".

### **Enable GCP Private Service Connect on Service Launch**

To enable GCP Private Service Connect when launching a new service via the SkySQL Terraform provider:

1. Initiate service launch using the procedure at "[Terraform Launch Walkthrough](https://mariadb.com/docs/skysql-dbaas/nr-quickstart/terraform-launch-walkthrough/)".
2. When you are configuring the `skysql_service` resource, add the `endpoint_mechanism` and `endpoint_allowed_accounts` attributes:
    
    For example, the attributes can be placed after `ssl_enabled`:
    
    ```bash
    ssl_enabled = var.ssl_enabled
    endpoint_mechanism = "privateconnect"
    endpoint_allowed_accounts = ["GCP-PROJECT-ID-1","GCP-PROJECT-ID-2"]
    ```
    
    - Set `endpoint_mechanism` to `"privateconnect"`
    - Set `endpoint_allowed_accounts` to a comma-separated list of one or more customer project IDs in GCP that will be allowed to establish a private connection to the SkySQL service
3. Continue the rest of the procedure.

For the next step, see the [Connectivity](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#Connectivity), [Controls](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#Controls), & [GCP Endpoint Setup](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#GCP_Endpoint_Setup) sections on this page.

### **Enable GCP Private Service Connect on Existing SkySQL Service**

To enable GCP Private Service Connect for an existing service via the SkySQL Terraform provider:

1. Alter the `skysql_service` resource configuration by adding the `endpoint_mechanism` and `endpoint_allowed_accounts` attributes.
    
    For example, the attributes can be placed after `ssl_enabled`:
    
    ```bash
    ssl_enabled = var.ssl_enabled
    endpoint_mechanism = "privateconnect"
    endpoint_allowed_accounts = ["GCP-PROJECT-ID-1","GCP-PROJECT-ID-2"]
    ```
    
    - Set `endpoint_mechanism` to `"privateconnect"`
    - Set `endpoint_allowed_accounts` to a comma-separated list of one or more customer project IDs in GCP that will be allowed to establish a private connection to the SkySQL service
2. Execute the `[terraform apply` command](https://developer.hashicorp.com/terraform/cli/commands/apply):
    
    ```bash
    $ terraform apply -var-file="skysql-nr-quickstart.tfvars"
    ```
    

For the next step, see the [GCP Endpoint Setup](https://mariadb.com/docs/skysql-dbaas/security/nr-private-connections/nr-gcp-private-service-connect/#GCP_Endpoint_Setup) sections on this page.

### **Disable GCP Private Service Connect**

To disable GCP Private Service Connect via the SkySQL Terraform provider:

1. Alter the `skysql_service` resource configuration by doing the following:
    - Changing the `endpoint_mechanism` attribute to `"nlb"`
    - Removing the `endpoint_allowed_accounts` attribute
    - Adding the `allow_list` attribute
    
    For example, if you started with the following attributes after `ssl_enabled`:
    
    ```json
    ssl_enabled = var.ssl_enabled
    endpoint_mechanism = "privateconnect"
    endpoint_allowed_accounts = ["GCP-PROJECT-ID-1","GCP-PROJECT-ID-2"]
    ```
    
    You could disable GCP Private Service Connect by changing the attributes to the following:
    
    ```json
    ssl_enabled = var.ssl_enabled
    endpoint_mechanism = "nlb"
    allow_list          = [
      {
         "ip"          : var.ip_address,
         "comment"     : var.ip_address_comment
      }
    ]
    ```
    
2. Execute the `[terraform apply` command](https://developer.hashicorp.com/terraform/cli/commands/apply):
    
    ```json
    $ terraform apply -var-file="skysql-nr-quickstart.tfvars"
    ```
    

---
