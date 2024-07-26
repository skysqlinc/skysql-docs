# Setup Disaster Recovery Site
SkySQL offers a robust platform for managing databases in the cloud, but setting up an effective disaster recovery (DR) site requires careful planning and execution. In this guide, we’ll explore how to automate the creation and restoration of SkySQL database services for disaster recovery purposes using the SkySQL API. We will use the following SkySQL resources for the setup:

- Provisioning APIs: To launch a primary and disaster recovery SkySQL service.
- Backup APIs: To backup the primary SkySQL service and restore to the DR service.
- Replication Procedures: To setup active replication between the primary and the DR SkySQL services.

### **Step 1: Generate SkySQL API Key**
1. Go to the [User Profile](https://app.skysql.com/user-profile/api-keys/) page of the SkySQL Portal to generate an API key.
2. Export the value from the token field to an environment variable $API_KEY
    
    ```bash
    $ export API_KEY='... key data ...'
    ```
    
    The `API_KEY` environment variable will be used in the subsequent steps.

### **Step 2: Launch SkySQL Services**
1. Launch two SkySQL services - a primary that your application will connect to and a standby that will act as a disaster recovery service. Following API requests will create two services in Google Cloud - 'skysql-primary' in the Virginia region and 'skysql-standby' in the Oregon region. 

```bash
curl --location --request POST 'https://api.skysql.com/provisioning/v1/services' \
   --header "X-API-Key: ${SKYSQL_API_KEY}" --header "Content-type: application/json" \
   --data '{
  "service_type": "transactional",
  "topology": "standalone",
  "provider": "gcp",
  "region": "us-east4",
  "architecture": "amd64",
  "size": "sky-2x8",
  "storage": 100,
  "nodes": 1,
  "name": "skysql-primary",
  "ssl_enabled": true,
  }'
```

```bash
curl --location --request POST 'https://api.skysql.com/provisioning/v1/services' \
   --header "X-API-Key: ${SKYSQL_API_KEY}" --header "Content-type: application/json" \
   --data '{
  "service_type": "transactional",
  "topology": "standalone",
  "provider": "gcp",
  "region": "us-west1",
  "architecture": "amd64",
  "size": "sky-2x8",
  "storage": 100,
  "nodes": 1,
  "name": "skysql-standby",
  "ssl_enabled": true,
  }'
```
2. Each SkySQL service has a unique identifier. Please make note of the identifier shown in the API response. We will need it later.

### **Step 3: Backup the Primary and Restore to the Standby Service**
1. In a real world scenario, the Primary service will have some data which will need to be restored to the Standby service before the replication can be set up. SkySQL performs full backup of your services every night. You can either use existing latest nightly backup or create a schedule to perform a new full backup.

Use the following API to list backups associated with the Primary service. Replace {id} with the id of the Primary service.

```bash
curl --location --request GET 'https://api.skysql.com/skybackup/v1/backups?service_id={id}' \
   --header "X-API-Key: ${SKYSQL_API_KEY}" --header "Content-type: application/json" \
```

Use the following API to create a one-time schedule to perform a new full backup. Replcate {id} with the id of the Primary service.

```bash
curl --location --request POST 'https://api.skysql.com/skybackup/v1/backups/schedules' \
   --header "X-API-Key: ${SKYSQL_API_KEY}" --header "Content-type: application/json" \
   --data '{
  "backup_type": "full",
  "schedule": "once",
  "service_id": "{id}"
  }'
```

2. Each backup also has a unique identified. Make note of the identifier shown in the API response. Now use the following API to restore the backup to the Standby service. Please note that restoring the backup on a SkySQL service will stop the service if it is running and will wipe out all existing data. Replcate {backup-id} with the backup id that you want to restore and {service-id} with the id of the Standby service.

```bash
curl --location --request POST 'https://api.skysql.com/skybackup/v1/restores' \
   --header "X-API-Key: ${SKYSQL_API_KEY}" --header "Content-type: application/json" \
   --data '{
  "id": "{backup-id}}",
  "service_id": "{service-id}"
  }'
```

3. Once the restore is complete, the original default username and password will not work. You will have to use Primary service's username and password to connect to the Standby service.

### **Step 4: Set up Replication between the Primary and the Standby**
1. Since we want to set up replication between the two SkySQL services, the Standby service should be able to connect to the Primary service. Add the Outbound IP address of the Standby service to the Allowlist of the Primary service. (TODO - Add APIs for retrieving outbound IP and adding it to allowlist)

2. Next, obtain the GTID position from which to start the replication and configure it on the Standby service by calling the following stored procedures. Replace 'host' and 'port' with the Primary service's hostname and port. Replace 'gtid' with the GTID position from which to start replication. Use true/false for whether to use SSL.

```bash
CALL sky.gtid_status();
CALL sky.change_external_primary_gtid(host,port,gtid,use_ssl_encryption);
```

3. Start replication and check status on the Standby service using the following procedures:

```bash
CALL sky.start_replication();
CALL sky.replication_status();
```

4. Once the replication is setup, verify the status of the new database service in the SkySQL console. Ensure that the service is replicating for use post failover in case of a disaster.

