# Setting up AWS Private Link


AWS PrivateLink is an AWS service that enables secure and private connectivity between Virtual Private Clouds (VPCs) and third-party services. By using PrivateLink with SkySQL services, traffic does not traverse the public internet, which enhances security and reduces exposure to potential threats.
For detailed information about AWS PrivateLink, see ["AWS PrivateLink" (Amazon documentation)](https://aws.amazon.com/privatelink/).


## **Considerations**

- AWS PrivateLink is used for private connections within the same AWS region.  The SkySQL service and the connection VPC must be in the same region.
- When using SkySQL with AWS PrivateLink, all connections occur through private endpoints.  If you need to connect to the service from outside your VPC, you will need to use a VPN or other mechanism to go through the connected VPC.  Alternatively, SkySQL can be configured to provide a second, public endpoint for an additional fee.
- A list of AWS Account IDs that will be allowed to connect to the SkySQL service must be provided when enabling AWS PrivateLink.  This list can be updated at any time.
- The SkySQL IP Allowlist is not used with AWS PrivateLink connections.  Access to the SkySQL service will be controlled by Security Groups in the connecting VPC. For detailed information, see ["Control traffic to resources using security groups"](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) (Amazon documentation).
- Connections to SkySQL services by features such as SkySQL backups, and monitoring do not depend on AWS PrivateLink.
- Query Editor is not supported when AWS PrivateLink is enabled.


### **Enable AWS PrivateLink on Service Launch**

<details>
<summary>Enable Privatelink via the SkySQL Portal</summary>
<br>

To enable AWS PrivateLink when launching a new service via the SkySQL Portal select the 'Enable Private link' option in the 'Security' section.
After the service completes provisioning, you will see a new option to "Set up Private Link" in the service's context menu. Click this option to add one or more AWS account IDs to the allowlist.

For the next step, see the [AWS Endpoint Setup](#aws-endpoint-setup) section on this page.

</details>

<details>
<summary>Enable Privatelink via the SkySQL DBaaS API</summary>
<br>

To enable AWS PrivateLink when launching a new service via the SkySQL DBaaS API, add the `endpoint_mechanism` and `endpoint_allowed_accounts` attributes to the JSON payload.  The `endpoint_mechanism` field must be set to `privateconnect` to enable PrivateLink:

```json
{
  "name": "my-skysql-service",
  ...
  "endpoint_mechanism": "privateconnect",
  "allowed_accounts": [
    "123456789012"
  ]
}
```

For more information on using the SkySQL DBaaS API, see ["SkySQL DBaaS API"](https://apidocs-test.skysql.com/#/Services/post_provisioning_v1_services).
</details>

<details>
<summary>Enable Privatelink via the SkySQL Terraform Provider</summary>
<br>

To enable AWS PrivateLink when launching a new service via the SkySQL DBaaS API, set the `endpoint_mechanism` and `endpoint_allowed_accounts` attributes on the `skysql_service` resource.  The `endpoint_mechanism` field must be set to `privateconnect` to enable PrivateLink:

```hcl
resource "skysql_service" "example" {
  name                      = "my-skysql-service"
  ...
  endpoint_mechanism        = "privateconnect"
  endpoint_allowed_accounts = ["123456789012"]
}
```

A complete example Terraform template that creates a new SkySQL service with AWS PrivateLink enabled can be found in the [terraform provider examples](https://github.com/skysqlinc/terraform-provider-skysql/tree/main/examples/privateconnect).


For more information on using the SkySQL Terraform Provider, see ["SkySQL Terraform Provider"](https://registry.terraform.io/providers/skysqlinc/skysql/latest/docs).

</details>



### **Enable AWS PrivateLink on Existing SkySQL Service**

To enable AWS PrivateLink for an existing service via the SkySQL Portal:

1. Log in to the SkySQL Portal
2. Click the "MANAGE" button (at right) for the desired service.
3. In the context menu, choose the "Set up Private Link" menu item.
4. In the popup window, add one or more AWS account IDs.
5. Click the "OK" button to confirm this operation.

For the next step, see the [AWS Endpoint Setup](#aws-endpoint-setup) section on this page.

### **Disabling AWS PrivateLink**

To disable AWS PrivateLink via the SkySQL Portal:

1. Visit the [SkySQL Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/)
2. Find the service that you would like to modify.
3. Click "MANAGE" on the far right side of the service listing.
4. In the context menu, select "Manage PrivateLink".
5. In the popup window, click "I want to disconnect my Private Link".
6. In the popup window, select "Disconnect".
7. After the service restarts, PrivateLink is disabled.
8. Since the service's allowlist was cleared when AWS PrivateLink was previously enabled, you will need to [update the allowlist](../Security/Configuring%20Firewall.md) to allow clients to connect after disabling PrivateLink.

## AWS Endpoint Setup

In addition to switching the SkySQL service to the `privateconnect` endpoint mechanism, an AWS Endpoint must be created using the customer's AWS account in order for the SkySQL service to become accessible.

1. Log in to the AWS console.
2. Confirm the correct region is selected.
3. Navigate to the "VPC" page, then the "Endpoint" section.
4. Click the "Create Endpoint" button.
5. Fill in the name to help you to identify it. (This is optional.)
6. Set the Service category to "Other endpoint services".
7. The value for the "Service name" field must be set to the value of the "Fully Qualified Domain Name" in the "Connect" window from SkySQL portal.
8. Click "Verify service". AWS should find the service and auto-populate the rest of the form.
9. In the VPC search field, find the VPC that you want to use for the interconnect between the clients and the SkySQL service.
10. In the Subnets section, it is suggested that you select all the Availability Zones in the list, entering the proper subnet ID for each one. If you are unsure, view the details of your running instances to see the Subnet ID that they have configured.
11. Select IPv4 for "IP address type".
12. For the "Security Groups" section, we suggest that you create a new security group that will regulate which instances can make a connection to the database.
    - In a new browser tab, use the AWS console to visit the "Security Groups" settings (under Network & Security).
    - Click on the "Create security group" button.
    - Fill in the group's name and (optionally) its description.
    - Under "Inbound rules" click the "Add rule" button.
    - Set the value for the "Port range" to be the port number of the "Read-write port" in the "Connect" window of the SkySQL portal.
    - Set the Source to either a list of private (internal) IPv4 addresses that you want to authorize (adding a "/32" suffix to each one), or set it to an existing security group name that can be used to authorize all instances that have that security group in their configuration.
    - Press the "Create security group" button.
13. Back on the endpoint tab, click the refresh button on the "Security Groups" section and choose the newly created security group.
14. Optionally add a "Name" tag for easy identification.
15. Press the "Create endpoint" button. Endpoint creation may take several minutes. When complete, status will change from "Pending" to "Available".
16. In the details of the new endpoint will be a list of DNS names. Copy the first name in the list and use that as the hostname for your clients to use when they connect.

The newly created endpoint now authorizes the internal IPs or security groups that you specified in the Source values to access the SkySQL service's connection port. When testing a client connection, ensure that the client host is authorized by the security group's Source settings and that you're using the "`readwrite`" port plus the appropriate username and password (either the default values or the value for any user you have created).

## **Setting up AWS Private link using SkySQL REST API**

### **Enable AWS PrivateLink on Service Launch**

To enable AWS PrivateLink when launching a new service via the SkySQL DBaaS API:

1. Initiate service launch using the procedure at "[DBaaS API Launch Walkthrough](../Quickstart/Launch%20DB%20using%20the%20REST%20API.md). 
2. When you are creating the request, add the `"endpoint_mechanism"` and `"endpoint_allowed_accounts"` attributes to the JSON payload:
    1. 
    
    ```json
    "endpoint_mechanism": "privateconnect",
    "endpoint_allowed_accounts": [
        "AWS-ACCOUNT-ID-1",
        "AWS-ACCOUNT-ID-2"
    ]
    ```
    
    - Set `"endpoint_mechanism"` to `"privateconnect"`
    - Set `"endpoint_allowed_accounts"` to a JSON array of one or more customer account IDs in AWS that will be allowed to establish a private connection to the SkySQL service

For the next step, go through the AWS Endpoint setup section. 

### **Enable AWS PrivateLink on Existing SkySQL Service**

To enable AWS PrivateLink on an existing service via the SkySQL DBaaS API, create a JSON payload such as:

```json
[
  {
    "mechanism": "privateconnect",
    "allowed_accounts": [
      "AWS-ACCOUNT-ID-1",
      "AWS-ACCOUNT-ID-2"
    ]
  }
]
```

- Set `"mechanism"` to `"privateconnect"`
- Set `"allowed_accounts"` to a JSON array of one or more customer account IDs in AWS that will be allowed to establish a private connection to the SkySQL service.

And then use the API to update the `endpoints` information of the service. The following example of using `curl` expects the above payload to be in a file named `data.json` and also expects the variables `API_KEY` and `SERVICE_ID` to be set:

```json
curl -H "X-API-Key:  ${API_KEY}" \
       -X PATCH -d @data.json \
       https://api.skysql.com/provisioning/v1/services/${SERVICE_ID}/endpoints \
       | jq .
```

We will need the endpoint's service name later on in the setup, so use the API to query the endpoint once the service has finished its modifications and is back to "ready":

```json
curl -H "X-API-Key:  ${API_KEY}" -X GET \
       https://api.skysql.com/provisioning/v1/services/${SERVICE_ID} \
       | jq '.status,.endpoints[0].ports,.endpoints[0].endpoint_service'
```

Note that the `jq` command is filtering the returned JSON data to extract three things (which are separated by commas in the command above):

- The status of the service, which must be "ready" for the other values to be available.
    - The "`ready`" status means that the PrivateLink endpoint setup is complete.
    - A status of "`pending_upgrade`" is reflected while PrivateLink endpoint setup is in-progress.
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
"com.amazonaws.vpce.AWS_REGION.vpce-svc-AWS_SERVICE_ID"
```

If you are not using `jq`, scan (or parse) the full returned JSON data to ensure the service status is "ready" and find the associated values described above.

For the next step, go through the AWS Endpoint Setup section. 

### **Disable AWS PrivateLink**

To disable AWS PrivateLink via the SkySQL DBaaS API, create a JSON payload such as:

```json
[
  {
    "mechanism": "nlb"
  }
]
```

Set `"mechanism"` to `"nlb"`

And then use the API to update the `endpoints` information of the service. The following example of using `curl` expects the above payload to be in a file named `data.json` and also expects the variables `API_KEY` and `SERVICE_ID` to be set:

```json
curl -H "X-API-Key:  ${API_KEY}" \
       -X PATCH -d @data.json \
       https://api.skysql.com/provisioning/v1/services/${SERVICE_ID}/endpoints \
       | jq .
```

We will need the endpoint's service name later on in the setup, so use the API to query the endpoint once the service has finished its modifications and is back to "ready":

```json
curl -H "X-API-Key:  ${API_KEY}" -X GET \
       https://api.skysql.com/provisioning/v1/services/${SERVICE_ID} \
       | jq '.status,.endpoints[0].ports,.endpoints[0].endpoint_service'
```

Note that the `jq` command is filtering the returned JSON data to extract three things (which are separated by commas in the command above):

- The status of the service, which must be "ready" for the other values to be available.
    - The "`ready`" status means that the endpoint modification is complete.
    - A status of "`pending_upgrade`" is reflected while PrivateLink is being disabled.
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
"com.amazonaws.vpce.AWS_REGION.vpce-svc-AWS_SERVICE_ID"
```

If you are not using `jq`, scan (or parse) the full returned JSON data to ensure the service status is "ready" and find the associated values described above.

Since the service's allowlist was cleared when AWS PrivateLink was previously enabled, you will need to update the allowlist to allow clients to connect after disabling PrivateLink.

To add an address to the allowlist via the SkySQL DBaaS API, create a JSON payload such as:

```json
**{**  **"ip_address":** "192.0.2.10/32"**}**
```

And then use the API to update the `allowlist` information of the service. The following example of using `curl` expects the above payload to be in a file named `data.json` and also expects the variables `API_KEY` and `SERVICE_ID` to be set:

```bash
curl -H "X-API-Key:  ${API_KEY}" \
       -X POST -d @data.json \
       https://api.skysql.com/provisioning/v1/services/${SERVICE_ID}/security/allowlist \
       | jq .
```

## Setup AWS Private link using Terraform Provider

For general instructions on using the SkySQL Terraform Provider, see "[Terraform Launch Walkthrough](../Quickstart/Launch%20DB%20using%20the%20Terraform%20Provider.md) 

For an example Terraform configuration that enables AWS PrivateLink, see Resources section [here](../Quickstart/Launch%20DB%20using%20the%20Terraform%20Provider.md). 


### **Enable AWS PrivateLink on Service Launch**

To enable AWS PrivateLink when launching a new service via the SkySQL Terraform provider:

1. Initiate service launch using the procedure at "[Terraform Launch Walkthrough](../Quickstart/Launch%20DB%20using%20the%20Terraform%20Provider.md) .
2. When you are configuring the `skysql_service` resource, add the `endpoint_mechanism` and `endpoint_allowed_accounts` attributes.
    
    For example, the attributes can be placed after `ssl_enabled`:
    
    ```bash
    ssl_enabled = var.ssl_enabled
    endpoint_mechanism = "privateconnect"
    endpoint_allowed_accounts = ["AWS-ACCOUNT-ID-1","AWS-ACCOUNT-ID-2"]
    ```
    
    - Set `endpoint_mechanism` to `"privateconnect"`
    - Set `endpoint_allowed_accounts` to a comma-separated list of one or more customer account IDs in AWS that will be allowed to establish a private connection to the SkySQL service
3. Continue the rest of the procedure.

For the next step, see the AWS Endpoint Setup section on this page.

### **Enable AWS PrivateLink on Existing SkySQL Service**

To enable AWS PrivateLink for an existing service via the SkySQL Terraform provider:

1. Alter the `skysql_service` resource configuration by adding the `endpoint_mechanism` and `endpoint_allowed_accounts` attributes.
    
    Follow the same instructions as above. 
    
2. Execute the `[terraform apply` command](https://developer.hashicorp.com/terraform/cli/commands/apply):
    
    ```bash
    $ terraform apply -var-file="skysql-nr-quickstart.tfvars"
    ```
    

For the next step, see the AWS Endpoint Setup section on this page.

### **Disable AWS PrivateLink**

To disable AWS PrivateLink via the SkySQL Terraform provider:

1. Alter the `skysql_service` resource configuration by doing the following:
    - Changing the `endpoint_mechanism` attribute to `"nlb"`
    - Removing the `endpoint_allowed_accounts` attribute
    - Adding the `allow_list` attribute
    
    For example, if you started with the following attributes after `ssl_enabled`:
    
    ```bash
    ssl_enabled = var.ssl_enabled
    endpoint_mechanism = "privateconnect"
    endpoint_allowed_accounts = ["AWS-ACCOUNT-ID-1","AWS-ACCOUNT-ID-2"]
    ```
    
    You could disable AWS PrivateLink by changing the attributes to the following:
    
    ```bash
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
