//TODO 
//Steps to follow

1. Dump using logical or physical backup
2. Upload the dump to S3/GCS bucket under your control
3. Call the migration API - ref - 
[Sky SQL Managed Migration Tutorial](../Backup and Restore/Restore From Your Own Bucket.md) 

4. To minimize downtime during migration, set up live binary-loggd based replication from your source database to the SkySQL database. 
Click [here](<./Replicating data from external DB.md>) for a detailed walk through of the steps involved. 

Verify 