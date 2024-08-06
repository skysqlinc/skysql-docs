# Migrating your existing Production DB

Databases can be migrated to SkySQL from many different database platforms, including Oracle, MySQL, PostgreSQL, Microsoft SQL Server, IBM DB2, and more

# Lift-and-shift from a compatible version of MySQL/MariaDB to SkySQL

To perform a lift-and-shift migration to SkySQL, use the following process:

1. Identify requirements for your SkySQL implementation including:
    - Topology - Enterprise Server Single node or with Replica(s)
    - [Instance size](<../Reference Guide/Instance Size Choices.md>)
    - Storage requirements
    - Desired server version
2. [Deploy the desired configuration](<../Portal features/Launch page.md>) on SkySQL

<aside>
💡 When migrating from a production DB a common practice is to setup live replication to the SkySQL database.  Here is the sequence of steps you should follow for a live cutover:

</aside>

1. On the source database (MySQL or MariaDB) obtain the Binlog file name and its current position. Essentially, we want to track all DB changes from a certain point in time. See [Replicating data](<./Replicating data from external DB.md>) page for the command. 
2. Next, take a [dump of your source database using mysqldump or mariadb-dump](<./Import data from external DB.md>) 
3. Import into your target SkySQL DB using this logical `dump` (SQL)
4. Finally, turn ON the replication using the SkySQL `start_replication` procedure as noted below. 

# Assisted Migration to SkySQL

SkySQL customers can receive assistance from SkySQL Inc when migrating a database to SkySQL:

- SkySQL Inc's [migration process](#migration-process) divides a migration into multiple well-defined stages with distinct validation criteria at each stage
- SkyDBAs and SkySQL support can assist with many migration steps

For assistance with a migration:

- Existing customers can submit a [support case](https://support.skysql.com) to request assistance with a migration
- New customers can [contact us](mailto:support@skysql.com) to begin the migration planning process

# Assisted Migration to SkySQL

Our [SkyDBA team](https://skysqlinc.github.io/skysql-docs/FractionalDBA/) can help design a migration plan to suit your needs. We use a multi-step process to assist customers with migrations:

1. **Assessment** of application requirements, inventory, and identified challenges
2. **Schema Migration** including tables, constraints, indexes, and views
3. **Application Code Migration** by porting and testing SQL and application code
4. **Data Migration and Replication** with import of data, with conversion to the new schema, and ongoing inbound replication of new data
5. **Quality Assurance** to assess data validity, data integrity, performance, accuracy of query results, stored code, and running code such as client applications, APIs, and batch jobs
6. **Cutover** including final database preparation, fallback planning, switchover, and decommissioning of old databases

For additional information, see "[Migrate to MariaDB from Oracle](https://mariadb.com/resources/blog/a-typical-journey-migrating-to-mariadb-from-oracle/)".

---

# Migrate to SkySQL using AWS DMS

This blog article details how to [Migrate RDS MySQL to SkySQL MariaDB Using AWS Data Migration Service](<./migrate-rds-mysql-to-skysql-using-amazon-data-migration-service_whitepaper_1109.pdf>)
---

# Replicating data from an External DB
Click [here](<./Replicating data from external DB.md>) for a detailed walk through of the steps involved. 


For assistance with a migration:

- Existing customers can submit a [support case](https://support.skysql.com) to request assistance with a migration
- New customers can [contact us](mailto:support@skysql.com) to begin the migration planning process

## Best Practices

---

### Live Replication for Minimal Downtime

To minimize downtime during migration, set up live replication from your source database to the SkySQL database. Follow these steps:

1. **Obtain Binlog File and Position**: On the source database (MySQL or MariaDB), obtain the Binlog file name and its current position to track all database changes from a specific point in time.

    ```sql
    SHOW MASTER STATUS;
    ```

2. **Dump the Source Database**: Take a dump of your source database using `mysqldump` or `mariadb-dump`, ensuring to skip the `mysql` table and handle the user dump separately to avoid issues with the default SkySQL user. Also, include triggers, procedures, views, and schedules in the dump.

    ```bash
    mysqldump -u [username] -p --all-databases --ignore-table=mysql.user \
        --ignore-table=mysql.global_priv --routines --triggers --events  \
        --skip-lock-tables > dump.sql
    ```

3. **Dump the User Table Separately**: Since the `mysql.user` view aggregates information from the `global_priv` table, you should dump the `global_priv` table separately to ensure that user privileges are preserved:

    ```bash
    mysqldump -u [username] -p mysql global_priv > global_priv.sql
    ```

4. **Import the Dumps into SkySQL**: Import the logical dumps (SQL files) into your SkySQL database, ensuring to load the user dump after the main dump.

    ```bash
    mariadb -u [username] -p [database_name] < dump.sql
    mariadb -u [username] -p mysql < global_priv.sql
    ```

5. **Start Replication**: Turn on replication using SkySQL stored procedures. There are procedures allowing you to set and start replication. See our [documentation](<../Reference Guide/Sky Stored Procedures.md>) for details.

    ```sql
    CALL sky.replication_grants();
    CALL sky.start_replication();
    ```

### Performance Optimization During Migration

- **Disable Foreign Key Checks**: Temporarily disable foreign key checks during import to speed up the process.

    ```sql
    SET foreign_key_checks = 0;
    ```

- **Disable Binary Logging**: If binary logging is not required during the import process, and you are using a standalone instance, it can potentially be disabled to improve performance. SkyDBA Services can assist with this as part of a detailed migration plan.

### Data Integrity and Validation

- **Consistency Checks**: Perform consistency checks on the source database before migration. Use a [supported SQL client](<../../Connecting to Sky DBs/>) to connect to your SkySQL instance and run the following.

    ```sql
    CHECK TABLE [table_name] FOR UPGRADE;
    ```

- **Post-Import Validation**: Validate the data integrity and consistency after the import.

    ```sql
    CHECKSUM TABLE [table_name];
    ```

### Advanced Migration Techniques

- **Adjust Buffer Sizes**: Temporarily increase buffer sizes to optimize the import performance. This can be done via the Configuration Manager in the portal.

    ```sql
    innodb_buffer_pool_size = 2G
    innodb_log_file_size = 512M
    ```

- **Parallel Dump and Import**: Use tools that support parallel processing for dumping and importing data.

    ```bash
    mysqlpump -u [username] -p --default-parallelism=4 --add-drop-database \
        --databases [database_name] > dump.sql
    ```

- **Incremental Backups**: For large datasets, incremental backups can be used to minimize the amount of data to be transferred. SkyDBA Services can assist you with setting these up as part of a custom migration plan.

### Monitoring and Logging

- **Enable Detailed Logging**: Enable detailed logging while testing the migration process to monitor and troubleshoot effectively. The slow_log can be enabled in the SkySQL configuration manager.

- **Resource Monitoring**: Use monitoring tools to track resource usage (CPU, memory, I/O) during the migration to ensure system stability. See our [monitoring documentation](<../Portal features/Service Monitoring Panels.md>) for details.

### Additional Resources

- [Backup with mariadb-dump](https://mariadb.com/kb/en/mariadb-dump/)
- [MariaDB Backup Documentation](https://mariadb.com/kb/en/mariadb-backup-overview/)
- [Advanced Backup Techniques](https://mariadb.com/kb/en/backup-and-restore-overview/)

