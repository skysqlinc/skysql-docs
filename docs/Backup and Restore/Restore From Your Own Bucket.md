<details>
<summary>
Authentication
</summary>
<h3>
<ol>
<li>
Go to the SkySQL <a href="https://app.skysql.com/user-profile/api-keys">API Key management page</a>  and generate an API key
</li>
<li>
Export the value from the token field to an environment variable $API_KEY

  ```
  export API_KEY='... key data ...'
  ```
</li>
<li>
Use it on subsequent request, e.g:

        ```bash
        curl --request GET 'https://api.skysql.com/skybackup/v1/backups/schedules' --header "X-API-Key: ${API_KEY}"
        ```
</li>
</ol>
</details> 

## Restore From your Bucket (External Storage)

You can restore your data from external cloud storage. 
SkySQL supports restoration from both Google Cloud Storage (GCS) and Amazon S3 cloud storage buckets. 
Your backup data should be created using either `mariabackup` or `mysqldump`.

Below is a sample restore call:

```bash

curl --location 'https://api.skysql.com/skybackup/v1/restores' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-Key: ${API_KEY}' \
--data '{
    "service_id": "<SERVICE_ID>",
    "id": "<ID>",
    "external_source": {
      "bucket": "<GCS_URI> оr <S3_URI> ",
      "method": "<BACKUP_METHOD>",
      "credentials": "<GCP_SERVICE_ACCOUNT_BASE64> or AWS_ACCOUNT_ACCESS_KEY_BASE64"
    }
  }'
```

- SERVICE_ID : SkySQL serivce identifier, format dbtxxxxxx. 
  You can fetch your service ID from the Fully qualified domain name(FQDN) of your service.  
  E.g: in dbpgf17106534.sysp0000.db2.skysql.com, 'dbpgf17106534' is the service ID. You will find the FQDN in the [Connect window](https://app.skysql.com/dashboard) 
- ID : the backup data file reference, available in your GCS or S3 bucket.
  
!!! Note
    Gzip compressed file  expected.

    Example:
    ```bash
    gzip <backup file> -c > <backup file>.gz
    ```
   
- GCS_URI/S3_URI : the GCS/S3 bucket URI where the backup file is stored. 
 
 Format gs://BUCKET_NAME/ or s3://BUCKET_NAME/
!!! Note
    Make sure the BUCKET_NAME contains a trailing slash. 
  
- BACKUP_METHOD : the backup method used to create the backup file. 
  <br>Available options: ``mariabackup`` , ``mysqldump`` </br>
- GCP_SERVICE_ACCOUNT_BASE64/AWS_ACCOUNT_ACCESS_KEY_BASE64 : Your base64 encoded GCP service account or AWS account access key. 
  
  Information on how to create a GCP service account <a href="https://cloud.google.com/iam/docs/keys-create-delete">here</a>
  Storage Admin role is required for the service account attemping the restore.
  
  Sample GCP service account key and command to encode it: 

    echo -n '
    {
        "type": "service_account",
        "project_id": "XXXXXXX",
        "private_key_id": "XXXXXXX",
        "private_key": "-----BEGIN PRIVATE KEY-----XXXXX-----END PRIVATE KEY-----",
        "client_email": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX.iam.gserviceaccount.com",
        "client_id": "XXXXXXX",
        "auth_uri": "<https://accounts.google.com/o/oauth2/auth>",
        "token_uri": "<https://oauth2.googleapis.com/token>",
        "auth_provider_x509_cert_url": "<https://www.googleapis.com/oauth2/v1/certs>",
        "client_x509_cert_url": "<https://www.googleapis.com/robot/v1/metadata/x509/XXXXXXXXXXXXXX.iam.gserviceaccount.com>",
        "universe_domain": "googleapis.com"
    } ' | base64
    
  Sample AWS account access key and command to encode it: 

    
    echo -n '
    {
        [default]
        aws_access_key_id = XXXXXXXXXXXXXEXAMPLE
        aws_secret_access_key = XXXXXXXXXXXXX/XXXXXXXXXXXXX/XXXXXXXXXXXXXEXAMPLEKEY
        region = XXXXXXXXXXXXX
    } ' | base64
    

