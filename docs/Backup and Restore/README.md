# Backup and Restore

The SkySQL Backup service provides comprehensive Backup and Restore features through a secure API. We extend the automated nightly backups with a number of self service features. You can automatically create and store backups of your databases to ensure additional data safety or provide a robust disaster recovery solution. The backups are stored on reliable and secure cloud storage, ensuring they are readily available when needed. The backup process is seamless and does not affect the performance of your databases. SkySQL also offers the flexibility to customize your backup schedule according to your specific needs. 

Here is the list of features offered:
- **Automatic nightly backups** : Automated nightly backups include a full backup of every database in the service. 
  
- **Secure On-demand or scheduled backups**
  
-  **Full (physical) backups** : Full backups create a complete backup of the database server into a new backup folder. It uses [mariabackup](https://mariadb.com/kb/en/full-backup-and-restore-with-mariabackup/) under the hood. Physical backups are performed by copying the individual data files or directories and the fastest way to do a backup.
  
- **Incremental backups** : Incremental backups update a previous backup with whatever changes to the data have occurred since the backup.
  
- **Logical backups** : Logical backups consist of the SQL statements necessary to restore the data, such as CREATE DATABASE, CREATE TABLE and INSERT. This is done using mariadb-dump(https://mariadb.com/kb/en/mariadb-dump/) and is the most flexible way to perform a backup and restore, and a good choice when the data size is relatively small.

- **Bring your own Bucket (BYOB)** : you can backup or restore data to/from your own bucket in either GCP or AWS.

- **Backup your binlogs** : Binlogs record database changes (data modifications, table structure changes) in a sequential, binary format. You can preserve binlogs for setting up replication or to recover to a certain point-in-time.

- **Point-in-time recovery** : you can restore from a full or a logical backup and then use a binlog backup to restore to a point-in-time.

-  **Secure backup/restores** : Control backup/restore privileges by granting roles to users in SkySQL. 

## Pricing 
while the daily automated backups are included the use of this API will incur nominal additional charges. Please contact info@skysql.com for details. 

The following documentation describes the API for the SkySQL Backup Service. This can be used directly with any HTTP client.

## Authentication

To authenticate with the API, do the following:

1. Go to MariaDB ID: <https://id.mariadb.com/account/api/> and generate an API key
2. Add the value from the token field to an environment variable $JWT_TOKEN
3. Use it on subsequent request, e.g:

   ```bash
    curl --request GET 'https://api.skysql.com/skybackup/v1/backups/schedules' \\
    --header "Authorization: Bearer $JWT_TOKEN"
   ```

## Scheduling backups

Backups on large data sets can take time. You instruct the creation of a backup using a "schedule". You can either schedule a one-time backup (schedule now) or set up automatic backups using a cron schedule. A backup schedule results in a backup job which can be tracked using the status API. We support the following types of backups : full(physical), incremental(physical), binary log, and dump(logical).

### Create a backup

To create a backup schedule, you need to have the "administrator" role. You can add members and configure roles using the [SkySQL portal](https://app.skysql.com/settings/user-management). 

#### Full Backup

##### One-time Full

To create a *full* backup you need to make the following API call:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "full",
    "schedule": "once",
    "service_id": "dbtgf28044362"
}'
```
**NOTE**
> note that each launched database is tracked with a service ID in SkySQL. You can fetch the service ID from the Fully qualified domain name(FQDN) of your service. For instance in dbpgf17106534.sysp0000.db2.skysql.com, 'dbpgf17106534' is the service ID. 
> You will find the FQDN in the [Connect window](https://app.skysql.com/dashboard) 

Typical API server response should look like:

```json
{
    "id": 253,
    "service_id": "dbtgf28044362",
    "schedule": "once",
    "type": "full",
    "status": "Scheduled",
    "message": "Backup is scheduled."
}
```
> you can fetch the Status of the backup using 'https://api.skysql.com/skybackup/v1/backups'. See the 'Backup Status' section for an example. The 'status' field will report Success or failure. 

##### Schedule a backup job using Cron

To set up an automatic periodic *full* backup at 3 am UTC, you need to make the following API call:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "full",
    "schedule": "0 3 * * *",
    "service_id": "dbtgf28044362"
}'
```

For more information about cron schedules take a look at this [document](https://en.wikipedia.org/wiki/Cron).

#### Incremental Backup

Incremental backups can be taken once you have full backup. Read [here](https://mariadb.com/kb/en/incremental-backup-and-restore-with-mariabackup/) for more details. 

##### One-time Incremental

To set up an one-time *incremental* backup, you need to make the following API call:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "incremental",
    "schedule": "once",
    "service_id": "dbtgf28044362"
}'
```

##### Cron Incremental

To set up an cron *incremental* backup, you need to make the following API call:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "incremental",
    "schedule": "0 3 * * *",
    "service_id": "dbtgf28044362"
}'
```

#### Binarylog Backup

##### One-time Binarylog

To set up an one-time *binarylog* backup:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "binarylog",
    "schedule": "once",
    "service_id": "dbtgf28044362"
}'
```

##### Schedule Binarylog backup 

To set up an cron *incremental* backup:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "binarylog",
    "schedule": "0 3 * * *",
    "service_id": "dbtgf28044362"
}'
```

#### Take a Logical backup (Mariadb-dump)

##### One-time Dump

To set up an one-time *dump* backup:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "dump",
    "schedule": "once",
    "service_id": "dbtgf28044362"
}'
```

##### Schedule a Logical backup (Dump)

To set up an cron *dump* backup:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
    "backup_type": "dump",
    "schedule": "0 3 * * *",
    "service_id": "dbtgf28044362"
}'
```

#### Scheduling Backups to your own bucket (external storage)

To set up an external storage backup, you need to make the following API call:

- For *GCP* you need to create an service account key. Please follow the steps from this [documentation](https://cloud.google.com/iam/docs/keys-create-delete). Once you have created the service account key you will need to base64 encode it. You can encode it directly from a command line itself. For example the execution of command ```echo -n 'service-account-key' | base64``` will produce something like ```c2VydmljZS1hY2NvdW50LWtleQ==```

    ```bash
    curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer $JWT_TOKEN' \
    --data '{
        "backup_type": "full",
        "schedule": "0 2 * * *",
        "service_id": "dbtgf28044362",
        "external_storage": {
            "bucket": {
                "path": "s3://my_backup_bucket",
                "credentials": "c2VydmljZS1hY2NvdW50LWtleQ=="
            }
        }
    }'
    ```

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
    Using encoded credentials you will be able to pass it to the API server. To initiate a new backup to your external storage you need to execute an API call to the backup service:

    ``````bash
    curl --location '<https://api.skysql.com/skybackup/v1/backups/schedules>' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer $JWT_TOKEN' \
    --data '{
        "backup_type": "full",
        "schedule": "0 2 ** *",
        "service_id": "dbtgf28044362",
        "external_storage": {
            "bucket": {
                "path": "s3://my_backup_bucket",
                "credentials": "W2RlZmF1bHRdCmF3c19hY2Nlc3Nfa2V5X2lkID0gQUtJQUlPU0ZPRE5ON0VYQU1QTEUKYXdzX3NlY3JldF9hY2Nlc3Nfa2V5ID0gd0phbHJYVXRuRkVNSS9LN01ERU5HL2JQeFJmaUNZRVhBTVBMRUtFWQpyZWdpb24gPSB1cy13ZXN0LTIK"
            }
        }
    }'
    ```

### Working with Backup Schedules

To get backup schedules inside the Organization :

```bash
curl --location '<https://api.skysql.com/skybackup/v1/backups/schedules>' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

#### Get all Backup Schedules per service

To get backup schedules for specific service :

```bash
curl --location '<https://api.skysql.com/skybackup/v1/backups/schedules?service_id=dbtgf28044362>' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

#### Get Backup Schedule by ID

To get specific backup schedule by id :

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules/200' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

#### Update Backup Schedule

In the following example, we update the backup schedule to 9 AM UTC. Remember, you cannot change the schedules for one-time backups.
To update specific backup schedule you need to make the following API call:

```bash
curl --location --request PATCH '<https://api.skysql.com/skybackup/v1/backups/schedules/215>' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
  "schedule": "0 9 ** *"
}'
```

#### Delete Backup Schedule

To delete a backup schedule you need to provide the backup schedule id. Example of the api call below:

```bash
curl --location --request DELETE 'https://api.skysql.com/skybackup/v1/backups/schedules/215' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

## Backup Status 

The following API illustrates how to get the available backups and status of backup jobs .

### List all backups inside the organization

Here is an example to fetch all the available Backups in your org:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

### List all backups by service

To list all backups available for your service :

```bash
curl --location 'https://api.skysql.com/skybackup/v1/backups?service_id=dbtgf28216706' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

The typical response of either of two calls should look like:

```json
{
    "backups": [
        {
            "id": "eda3b72460c8c0d9d61a7f01b6a22e32:dbtgf28216706:tx-filip-mdb-ms-0",
            "service_id": "dbtgf28216706",
            "type": "full",
            "method": "skybucket",
            "server_pod": "tx-filip-mdb-ms-0",
            "backup_size": 5327326,
            "reference_full_backup": "",
            "point_in_time": "2024-03-26 17:18:21",
            "start_time": "2024-03-26T17:18:57Z",
            "end_time": "2024-03-26T17:19:01Z",
            "status": "Succeeded"
        }
    ],
    "backups_count": 1,
    "pages_count": 1
}
```

> The ** Backup id is the most important part of this data as you need to provide it in the restore api call** to schedule restore execution.

## Restores

**WARNING**
> restoring from a Backup will likely wipe out all data in your target DB service. If you aren't sure, first take a backup of the Db service where a restore is being performed. The DB being restored will also be stopped during a Restore. You will need restart it. 

### Creating Restore Job

#### Restore From Managed Storage

You can restore your database from the backup located in the default SkySQL managed backup storage. In this case, you need to provide the backup ID when making the restore API call. Here is an example:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/restores' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN' \
--data '{
  "key": "eda3b72460c8c0d9d61a7f01b6a22e32:dbtgf28216706:tx-filip-mdb-ms-0",
  "service_id": "dbtgf28044362"
}'
```

Inside the service_id parameter of your restore API request, you need to provide the id of the service, where you want to restore your data.

#### Restore From your Bucket (External Storage)

You can restore your data from external storage. Your external storage bucket data should be created via one of the following tools: ```mariabackup, mysqldump```. Credentials to external storage access could be fetched from:

- For *GCP* you need to create an service account key. Please follow the steps from this [documentation](https://cloud.google.com/iam/docs/keys-create-delete). Once you have created the service account key you will need to base64 encode it. You can encode it directly from a command line itself. For example the execution of command ```echo -n 'service-account-key' | base64``` will produce the following ```c2VydmljZS1hY2NvdW50LWtleQ==```

    ```bash
    curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer $JWT_TOKEN' \
    --data '{
        "backup_type": "full",
        "schedule": "0 2 * * *",
        "service_id": "dbtgf28044362",
        "external_storage": {
            "bucket": {
                "path": "s3://my_backup_bucket",
                "credentials": "c2VydmljZS1hY2NvdW50LWtleQ=="
            }
        }
    }'
    ```

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

### Fetching Restore Job information

#### Get all Restores Schedules

In order to get all Restores scheduled in the past you need to make api call:

```bash
curl --location 'https://api.skysql.com/skybackup/v1/restores' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

#### Get Restore by ID

```bash
curl --location 'https://api.skysql.com/skybackup/v1/restores/12' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```

Typical response of those two apis should look like:

In case restore is in progress:

```json
[
    {
        "id": 12,
        "service_id": "dbtgf28216706",
        "bucket": "gs://sky-syst0000-backup-us-84e9d84ecf265a/orgpxw1x",
        "key": "eda3b72460c8c0d9d61a7f01b6a22e32:dbtgf28216706:tx-filip-mdb-ms-0",
        "type": "physical",
        "status": "Running",
        "message": "server is not-ready"
    }
]
```

In case restore completed:

```json
[
    {
        "id": 13,
        "service_id": "dbtgf28216706",
        "bucket": "gs://sky-syst0000-backup-us-84e9d84ecf265a/orgpxw1x",
        "key": "dda9b72460c9c0d9d61a7f01b6a33e39:dbtgf28216706:tx-filip-mdb-ms-0",
        "type": "physical",
        "status": "Succeeded",
        "message": "Restore has succeeded!"
    }
]
```

#### Delete Restore Schedules

You can delete older completed restore schedules. To clean up your auditing history you need to execute the following api call:


```bash
curl --location --request DELETE 'https://api.skysql.com/skybackup/v1/restores/12' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $JWT_TOKEN'
```
