# **Back up and instance**
<!-- Commenting the backups pricing until it's finalzied
## **Pricing**
> While daily automated backups are provided, using the SkySQL Backup and Restore API may incur nominal additional charges. For more information, please contact info@skysql.com."


The following documentation describes the API for the SkySQL Backup Service. This can be used directly with any HTTP client.

The Backup and Restore service provides SkySQL customers with a comprehensive list of features through a secure API and a user-friendly portal. The service extends the automated nightly backups with a number of self-service features. Users can automatically create and store backups of their databases to ensure additional data safety or provide a robust disaster recovery solution. The backups are stored on reliable and secure cloud storage, ensuring they are readily available when needed. The backup process is seamless and does not affect the database performance. SkySQL also offers the flexibility to customize backup schedule according to your specific needs. Backups on large data sets can take time.

You instruct the creation of a backup using a "schedule". You can either schedule a one-time backup (schedule now) or set up automatic backups using a cron schedule. A backup schedule results in a backup job which can be tracked using the status API. We support the following types of backups: snapshot, full (physical), incremental (physical), binary log, and dump (logical).
-->
### **SkySQL Snapshot Backups**

<details>
<summary>
Overview
</summary>

<h3>
<li>
SkySQL database snapshots create a point-in-time copy of the database persistent volume. Compared to full backups, snapshots provide a faster method for restoring your database with the same data. 
</li>

<li>
Snapshots are incremental in nature. After the initial full snapshot of a database persistent volumes, subsequent snapshots only capture and store the changes made since the last snapshot. This approach saves a lot of storage space and reduces the time it takes to create a snapshot database backup and the related cloud storage cost. 
</li>
<li>
Users have the flexibility to trigger a snapshot as per their scheduling requirements - either on-demand or according to a pre-defined schedule. 
</li>
<li>
The SkySQL snapshots benefit from MariaDB's [backup stage flush](https://mariadb.com/kb/en/backup-stage/#:~:text=active%20DDL%20commands.-,BACKUP%20STAGE%20FLUSH,as%20closed%20for%20the%20backup.) to create a consistent backup of the database - database lock temporarily suspends write operations and replication for just a few seconds. In a Primary/Replica topology, snapshot backups are prioritized and performed on the replica node. This is to ensure that the primary server can continue to operate in read/write mode, as the backup process is carried out on the replica node. After the backup process on the replica is completed, replication resumes automatically.
</li>
</h3>
</details>

##### Snapshot Backup Examples 

SkySQL supports database snapshot backups either on-demand or according to a pre-established schedule. 
Below are examples of how to schedule a snapshot backup using the SkySQL API.

- [Examples](Snapshot Backup Examples.md)

***Important:*** Database snapshots are deleted immediately upon service deletion. 

<details>
<summary>
References
</summary>
<h3>
    <ul>
        <li><a href="https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html">Amazon EBS snapshots</a></li>
        <li><a href="https://cloud.google.com/kubernetes-engine/multi-cloud/docs/aws/how-to/snapshot-persistentvolume">Google Cloud Kubernetes Engine - Snapshot Persistent Volume</a></li>
        <li><a href="https://kubernetes.io/docs/concepts/storage/volume-snapshots/">Kubernetes - Volume Snapshots</a></li>
        <li><a href="https://mariadb.com/kb/en/how-mariabackup-works/#create-a-consistent-backup-point">MariaDB - How Mariabackup Works/create-a-consistent-backup-point</a></li>
    </ul>
</h3>
</details>

### **Full (physical) Backups** 

<details>
<summary>
Overview
</summary>

<h3>
<li>
Full backups create a complete backup of the database server into a new backup folder. It uses [mariabackup](https://mariadb.com/kb/en/full-backup-and-restore-with-mariabackup/) under the hood. Physical backups are performed by copying the individual data files or directories.
</li>

<li>
The physical backup uses backup stages to create a consistent backup of the database without requiring a global read lock for the entire duration of the backup, while allowing the database to continue processing transactions. Instead, the server read lock is only needed briefly during the [BACKUP STAGE FLUSH](https://mariadb.com/kb/en/backup-stage/#:~:text=active%20DDL%20commands.-,BACKUP%20STAGE%20FLUSH,as%20closed%20for%20the%20backup.) stage, which flushes the tables to ensure that all of them are in a consistent state at the exact same point in time, independent of storage engine. The database lock temporarily suspends write operations and replication; the duration of the lock is typically just a few seconds. In a Primary/Replica topology, backups are prioritized and performed on the replica node. This approach ensures that the primary server can continue to operate in read/write mode, as the backup process is carried out on the replica node. After the backup process on the replica is completed, replication resumes automatically.
</li>
</h3>
</details>
#### Full (physical) Backup Examples

SkySQL supports database physical backups either on-demand or according to a pre-established schedule. Below are examples of how to schedule a physical backup using the SkySQL API.

- [Examples](Physical Backup Examples.md)

<details>
<summary>
References
</summary>
<h3>
    <ul>
        <li><a href="https://mariadb.com/kb/en/full-backup-and-restore-with-mariabackup/">mariabackup</a></li>
    </ul>
</h3>
</details>

### **Incremental Backups**

<details>
<summary>
Overview
</summary>

<h3>
Incremental backups update a previous backup with any changes to the data that have occurred since the initial backup was taken.

InnoDB pages contain log sequence numbers, or LSN's. Whenever you modify a row on any InnoDB table in the database, the storage engine increments this number. When performing an incremental backup, Mariabackup checks the most recent LSN for the backup against the LSN's contained in the database. It then updates any of the backup files that have fallen behind.

</h3>
</details>

##### Incremental Backup Examples
SkySQL supports database incremental backups either on-demand or according to a pre-established schedule. 
Below are examples of how to schedule an incremental backup using the SkySQL API.

- [Examples](Incremental Backup Examples.md)
    
### **Logical (Mariadb-dump) Backups** 

<details>
<summary>
Overview
</summary>

<h3>
Logical backups consist of the SQL statements necessary to restore the data, such as CREATE DATABASE, CREATE TABLE, and INSERT. This is done using mariadb-dump ([mariadb-dump](https://mariadb.com/kb/en/mariadb-dump/)) and is the most flexible way to perform a backup and restore, and a good choice when the data size is relatively small.
</h3>
</details>

#### Logical Backup Examples

SkySQL supports database logical backups either on-demand or according to a pre-established schedule. Below are examples of how to schedule a logical backup using the SkySQL API.

- [Examples](Logical Backup Examples.md)

<details>
<summary>
References
</summary>

<h3>
[mariadb-dump](https://mariadb.com/kb/en/mariadb-dump/)

</h3>
</details>

### **BinaryLog Backups** 

<details>
<summary>
Overview
</summary>

<h3>
Binlogs record database changes (data modifications, table structure changes) in a sequential, binary format. You can preserve binlogs for setting up replication or to recover to a certain point-in-time.

</h3>
</details>

#### BinaryLog Backup Examples

- [Examples](<./Binarylog Backup Examples.md>)

### **Additional Backup Options (with Examples)**
  
<ul>
<li> <b> Replication as Backup </b>: In situations where the service cannot be locked or stopped, or is under heavy load, performing backups directly on a primary server may not be the preferred option. Using a replica database instance for backups allows the replica to be shut down or locked, enabling backup operations without impacting the primary server.
    
    The approach is commonly implemented in the following manner:
    - The primary server replicates data to a replica.
    - Backups are then initiated from the replica, ensuring no disruption to the primary server.
  
    Details on how to set up replication with your SkySQL instance can be found [here](../Data%20loading%2C%20Migration/Replicating%20data%20from%20external%20DB/).
</li>
<li> <b> Automatic Nightly Backups </b>: Automated nightly backups include a full backup of every database in the service to ensure that your SkySQL Database service is backed up regularly. Nightly backups are running for every SkySQL database by default.
</li>
<li> <b> Bring Your Own Bucket (BYOB) </b>: You can backup or restore data to/from your own bucket in either GCP or AWS. Sample GCP and AWS scripts can be found [here](../Backup%20and%20Restore/Bring%20Your%20Own%20Bucket%20Examples/).
</li>
<li> <b> Point-in-time Recovery </b>: You can restore from a full or a logical backup and then use a binlog backup to restore to a point-in-time.
</li>
<li> <b> Secure Backup/Restores </b>: Control backup/restore privileges by granting roles to users in SkySQL.
</li>

<li> <b> Other Backup API Examples </b>: Various API scripts providing examples of listing backups, checking backup statuses, and working with backup schedules can be found [here](../Backup%20and%20Restore/Other%20backup%20API%20examples/).
</li>
</ul>

## Restores

### Warning
> Restoring from a backup will, by default, erase all data in your target DB service. If you are uncertain, it is advisable to first create a backup of the DB service before initiating the restore process. Consider restoring to a new database instance as a preferred approach. For restores other than logical dumps, the database being restored will be temporarily stopped during the restoration.

Users can instruct the restore of their SkySQL Database from their own SkySQL storage or from an external storage they own. The restore API provides options for listing, adding, and deleting a scheduled restore operation.

### **List Restore Schedules** 

SkySQL Users can fetch their already existing database restore schedules using the backup API. Check the provided API examples for details.

#### Restore List Examples

- [Examples](Restore Listing Examples.md)

### **Create a Restore**

SkySQL Users can restore their databases using their own SkySQL managed backup storage or using an external storage they own. Check the provided service API examples for details.

#### Database Restore Examples

- [Examples](Restore Examples.md)
    
### **Delete Restore Schedule**

SkySQL Users can delete their already defined database restore schedules with the provided service API.

#### Delete Restore Examples

- [Examples](Restore Delete Examples.md)

### **GTID Considerations for Logical Dumps**

When restoring from a logical dump in MariaDB:

- The GTID state is not preserved by default in logical dumps
- If you're using replication, you'll need to:
    1. Either reset the GTID state after restore
    2. Or use `mysqldump --dump-slave` to include GTID information in the backup
- Consider using physical backups (mariabackup) instead, as they preserve GTID state

## Limitations

- Currently, SkySQL services deployed in Azure can only be backed up and restored using [SkySQL Snapshots](Snapshot Backup Examples.md).
- SkySQL Managed backups can only be restored within the same cloud provider. If you need to restore to a SkySQL service hosted on a different cloud provider, you must export your backup to S3 or GCS storage and follow the steps described [here](Restore From Your Own Bucket.md).