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

You can restore your data from external storage. Your external storage bucket data should be created via one of the following tools: ```mariabackup, mysqldump```. Credentials to external storage access could be fetched from:

### Restore from Google Cloud Storage(GCS) bucket 

```bash

curl --location 'https://api-test.skysql.com/skybackup/v1/restores' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'X-API-Key: ${API_KEY}' \
--data '{
    "service_id": "<SERVICE_ID>",
    "encryption_key": "<ENCRYPTION_KEY>",
    "id": "<ID>",
    "external_source": {
      "bucket": "<GCS_URI>",
      "method": "<BACKUP_METHOD>",
      "credentials": "<GCP_SERVICE_ACCOUNT_BASE64>"
    }
  }'
```

- SERVICE_ID : SkySQL serivce identifier, format dbtxxxxxx. 
  You can fetch your service ID from the Fully qualified domain name(FQDN) of your service.  
  E.g: in dbpgf17106534.sysp0000.db2.skysql.com, 'dbpgf17106534' is the service ID. You will find the FQDN in the [Connect window](https://app.skysql.com/dashboard) 
- ENCRYPTION_KEY : plain text encryption password
- ID : the backup data file reference, available in your GCS bucket.
<br><b> Note !!! </b> The backup data file needs to be encryped using  OpenSSL </br> 

Sample Command to encrypt the backup file:

```bash
openssl enc -aes-256-cbc -pbkdf2 -salt -in <Backup File> -out <ID> -pass <ENCRYPTION_KEY>
```


- GCS_URI : the GCS bucket URI where the backup file is stored, format gs://BUCKET_NAME
  
- BACKUP_METHOD : the backup method used to create the backup file. 
  <br>Available options: ```mariabackup, mysqldump`` </br>
- GCP_SERVICE_ACCOUNT_BASE64 : the base64 encoded service account key.
  
  Sample account key and command to encode it: 

    ```json
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
    ```

Note:!!!


curl --location 'https://api-test.skysql.com/skybackup/v1/backups?service_id=dbtgf04198097' \
  --header 'Accept: application/json' \
  --header "X-API-Key: skysql.1zzz.mh2oe85a.5aXjdyqgef7facjgAQ6DcLlVfx8imkkybIan.87c113e7"



  encryption_key - 


    The service account key will be in the following format:

    ```json
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
    }
    ```

- For AWS, you must provide your own credentials. These include the AWS access key associated with an IAM account and the bucket region. For more information about AWS credentials, please refer to the [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html). The required credentials are *aws_access_key_id* , *aws_secret_access_key* and *region*. For example your credentials should look like:

    ```bash
    [default]
    aws_access_key_id = AKIAIOSFODNN7EXAMPLE
    aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    region = us-west-2
    ```

    You should encode your credentials base64 before passing it to the API. You can encode it directly from a command line itself. For example the execution of command ```echo '[default]\naws_access_key_id = AKIAIOSFODNN7EXAMPLE\naws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY\nregion = us-west-2' | base64``` will produce the following ```W2RlZmF1bHRdCmF3c19hY2Nlc3Nfa2V5X2lkID0gQUtJQUlPU0ZPRE5ON0VYQU1QTEUKYXdzX3NlY3JldF9hY2Nlc3Nfa2V5ID0gd0phbHJYVXRuRkVNSS9LN01ERU5HL2JQeFJmaUNZRVhBTVBMRUtFWQpyZWdpb24gPSB1cy13ZXN0LTIK```.

The following request demonstrates how to restore your data from an external storage:

```json
{
  "service_id": "dbtgf28044362",
  "key": "/backup.tar.gz",
  "external_source": {
    "bucket": "gs://my_backup_bucket",
    "method": "mariabackup",
    "credentials" "W2RlZmF1bHRdCmF3c19hY2Nlc3Nfa2V5X2lkID0gQUtJQUlPU0ZPRE5ON0VYQU1QTEUKYXdzX3NlY3JldF9hY2Nlc3Nfa2V5ID0gd0phbHJYVXRuRkVNSS9LN01ERU5HL2JQeFJmaUNZRVhBTVBMRUtFWQpyZWdpb24gPSB1cy13ZXN0LTIK"
  }
}
```

In case your backup data is encrypted you need to pass encryption key as well:

```json
{
  "service_id": "dbtgf28044362",
  "key": "/backup.tar.gz",
  "external_source": {
    "bucket": "gs://my_backup_bucket",
    "method": "mariabackup",
    "credentials": "W2RlZmF1bHRdCmF3c19hY2Nlc3Nfa2V5X2lkID0gQUtJQUlPU0ZPRE5ON0VYQU1QTEUKYXdzX3NlY3JldF9hY2Nlc3Nfa2V5ID0gd0phbHJYVXRuRkVNSS9LN01ERU5HL2JQeFJmaUNZRVhBTVBMRUtFWQpyZWdpb24gPSB1cy13ZXN0LTIK",
    "encryption_key": "my_encryption_key"
  }
}
```
