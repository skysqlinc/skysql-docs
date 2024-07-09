# On-demand or scheduled Snapshot backups

## Snapshot Backup Overview
SkySQL database snapshots create a point-in-time copy of the database persistent volume. Compared to full backups, snapshots provide a faster method for restoring your database with the same data. 

Snapshots are incremental in nature. This means that after the initial full snapshot of a database persistent volumes, subsequent snapshots only capture and store the changes made since the last snapshot. This approach saves a lot storage space and reduce the time it takes to create a snapshot and the overall total cost. 

You have the flexibility to trigger a snapshot as per your requirements - either on-demand or according to a pre-established schedule. 

The snapshots use backup stages to create a consistent backup of the database without requiring a global read lock for the entire duration of the backup, while allowing the database to continue processing transactions. Instead, the server read lock is only needed briefly during the BACKUP STAGE FLUSH stage, which flushes the tables to ensure that all of them are in a consistent state at the exact same point in time, independent of storage engine. The database lock temporarily suspends write operations and replication, the duration of the lock is typically just a few seconds. In a Primary/Replica topology, backups are prioritized and performed on the replica node. This approach ensures that the primary server can continue to operate in read/write mode, as the backup process is carried out on the replica node. After the backup process on the replica is completed, replication resumes automatically.
  
  References:
- <https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html>
- <https://cloud.google.com/kubernetes-engine/multi-cloud/docs/aws/how-to/snapshot-persistentvolume>
- <https://kubernetes.io/docs/concepts/storage/volume-snapshots/>
- <https://mariadb.com/kb/en/how-mariabackup-works/#create-a-consistent-backup-point>

 **Note** : Database snapshots are deleted immedatily upon serivce deletion.

 ## Snapshot Backup Scheduling

1. Go to SkySQL API Key management page: https://app.skysql.com/user-profile/api-keys and generate an API key

2. Export the value from the token field to an environment variable $API_KEY
    
  ```bash
  export API_KEY='... key data ...'
  ```
    
  The `API_KEY` environment variable will be used in the subsequent steps.

3. Use it on subsequent request, e.g:
  ```bash 
  curl --request GET 'https://api.skysql.com/provisioning/v1/services' \\
  --header "X-API-Key: $API_KEY"
  ```

!!! Note
    You can use the Skysql Backup and Restore API documentation [here](https://api.skysql.com/public/services/dbs/docs/swagger/index.html) and directly try out the backup APIs in your browser. 

### One-time Snapshot

To set up an one-time snapshot backup:

```json
curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header "X-API-Key: $API_KEY" \
--data "{
      \"backup_type\": \"snapshot\",
      \"schedule\": \"once\",
      \"service_id\": \"$SERVICE_ID\"
    }"
```

Typical API server response should look like:

```json
{
  "id": 253,
  "service_id": "dbtgf28044362",
  "schedule": "snapshot",
  "type": "full",
  "status": "Scheduled",
   "message": "Backup is scheduled."
}
```

You can fetch the Status of the backup using 'https://api.skysql.com/skybackup/v1/backups/schedules'. See the 'Backup Status' section for an example. The 'status' field will report Success or failure.

###Cron Snapshot

To set up an cron snapshot backup:

```json
  curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules'
  --header 'Content-Type: application/json' \
  --header 'Accept: application/json' \
  --header "X-API-Key: $API_KEY" \
  --data "{
  \"backup_type\": \"snapshot\",
  \"schedule\": \"0 3 * * *\",
  \"service_id\": \"$SERVICE_ID\"
}"
```

