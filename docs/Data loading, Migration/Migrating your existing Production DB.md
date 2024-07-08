# Migrating your existing Production DB

Databases can be migrated to MariaDB SkySQL from many different database platforms, including Oracle, MySQL, PostgreSQL, Microsoft SQL Server, IBM DB2, and more

# Lift-and-shift from a compatible version of MySQL/MariaDB to SkySQL

To perform a lift-and-shift migration to SkySQL, use the following process:

1. Identify requirements for your MariaDB SkySQL implementation including:
    - [Topology](https://mariadb.com/docs/skysql-previous-release/features-and-concepts/services/)
    - [Instance size](https://mariadb.com/docs/skysql-previous-release/features-and-concepts/selections/instance-sizes/)
    - Storage requirements
    - Desired server version
2. [Deploy the desired configuration](https://mariadb.com/docs/skysql-previous-release/service-management/launch/) on MariaDB SkySQL

<aside>
💡 When migrating from a production DB a common practice is to setup live replication to the SkySQL database.  Here is the sequence of steps you should follow for a live cutover:

</aside>

1. On the source database (MySQL or MariaDB) obtain the Binlog file name and its current position. Essentially, we want to track all DB changes from a certain point in time. See [‘Replicating data’](Migrating%20your%20existing%20Production%20DB%208ba80cbdb9b14f81a1df5b096d549522/Replicating%20data%20from%20external%20DB%20cdf15e1cd8d24880858d6cd2f50d8fd2.md) page for the command. 
2. Next, take a [dump of your source database using mysqldump or mariadb-dump](Import%20data%20from%20external%20DB%209d0a68120e404f36b9f09a5ad71796b7.md) 
3. Import into your target SkySQL DB using this logical “dump” (SQL)
4. Finally, turn ON the replication using the SkySQL ‘start_replication’ procedure as noted below. 

# Assisted Migration to SkySQL MariaDB

SkySQL customers can receive assistance from SkySQL Inc when migrating a database to SkySQL:

- SkySQL Inc's [migration process](https://mariadb.com/docs/skysql-previous-release/migration/assisted/#Migration_Process) divides a migration into multiple well-defined stages with distinct validation criteria at each stage
- SkyDBAs and SkySQL support can assist with many migration steps

For assistance with a migration:

- Existing customers can submit a [support case](https://mariadb.com/docs/skysql-previous-release/service-management/support/) to request assistance with a migration
- New customers can [contact us](https://mariadb.com/docs/skysql-previous-release/contact/) to begin the migration planning process

## Migration Process

We use a multi-step process to assist customers with migrations:

1. **Assessment** of application requirements, inventory, and identified challenges
2. **Schema Migration** including tables, constraints, indexes, and views
3. **Application Code Migration** by porting and testing SQL and application code
4. **Data Migration and Replication** with import of data, with conversion to the new schema, and ongoing inbound replication of new data
5. **Quality Assurance** to assess data validity, data integrity, performance, accuracy of query results, stored code, and running code such as client applications, APIs, and batch jobs
6. **Cutover** including final database preparation, fallback planning, switchover, and decommissioning of old databases

For additional information, see "[Whitepaper: Migrate to MariaDB from Oracle](https://go.mariadb.com/GLBL-WC2020OracleMigration_LP-Registration.html)".

---

# Migrate to MariaDB SkySQL using AWS DMS

This blog article details how to [Migrate RDS MySQL to SkySQL MariaDB Using AWS Data Migration Service](https://go.mariadb.com/21Q3-WC-GLBL-DBaaS-Amazon-RDS-to-SkySQL-Migration-DB1109_LP-Registration.html)

---

# Replicating data from an External DB
Click [here](Replicating%20data%20from%20external%20DB%20cdf15e1cd8d24880858d6cd2f50d8fd2.md) for a detailed walk through of the steps involved. 


For assistance with a migration:

- Existing customers can submit a [support case](https://mariadb.com/docs/skysql-previous-release/service-management/support/) to request assistance with a migration
- New customers can [contact us](https://mariadb.com/docs/skysql-previous-release/contact/) to begin the migration planning process

## Best Practices


### Live Replication for Minimal Downtime

To minimize downtime during migration, set up live replication from your source database to the SkySQL database. Follow these steps:

1. **Obtain Binlog File and Position**: On the source database (MySQL or MariaDB), obtain the Binlog file name and its current position to track all database changes from a specific point in time.

    ```sql
    SHOW MASTER STATUS;
    ```

2. **Dump the Source Database**: Take a dump of your source database using `mysqldump` or `mariadb-dump`, ensuring to skip the `mysql` table and handle the user dump separately to avoid issues with the default SkySQL user. Also, include triggers, procedures, views, and schedules in the dump.

    ```bash
    mysqldump -u [username] -p --all-databases --ignore-table=mysql.user --routines --triggers --events --skip-lock-tables > dump.sql
    ```

3. **Dump the User Table Separately**: Dump the `mysql.user` table separately to handle user data without affecting the default SkySQL user.

    ```bash
    mysqldump -u [username] -p mysql user > mysql_user_dump.sql
    ```

4. **Import the Dumps into SkySQL**: Import the logical dumps (SQL files) into your SkySQL database, ensuring to load the user dump after the main dump.

    ```bash
    mariadb -u [username] -p [database_name] < dump.sql
    mariadb -u [username] -p mysql < mysql_user_dump.sql
    ```

5. **Start Replication**: Turn on replication using SkySQL stored procedures. There are procedures allowing you to set and start replication. See our [documentation](https://skysqlinc.github.io/skysql-docs/Reference%20Guide/Sky%20Stored%20Procedures/) for details.

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

- **Consistency Checks**: Perform consistency checks on the source database before migration. Use a [supported SQL client](https://skysqlinc.github.io/skysql-docs/Connecting%20to%20Sky%20DBs/) to connect to your SkySQL instance and run the following.

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
    mysqlpump -u [username] -p --default-parallelism=4 --add-drop-database --databases [database_name] > dump.sql
    ```

- **Incremental Backups**: For large datasets, incremental backups can be used to minimize the amount of data to be transferred. SkyDBA Services can assist you with setting these up as part of a custom migration plan.

### Monitoring and Logging

- **Enable Detailed Logging**: Enable detailed logging while testing the migration process to monitor and troubleshoot effectively.

    ```sql
    SET GLOBAL general_log = 'ON';
    ```

!!! Note
    💡 **Enabling the general log can cause performance issues. It is recommended to enable it only during testing pre-migration, not on a production instance.**

- **Resource Monitoring**: Use monitoring tools to track resource usage (CPU, memory, I/O) during the migration to ensure system stability. See our [monitoring documentation](https://skysqlinc.github.io/skysql-docs/Portal%20features/Service%20Monitoring%20Panels/) for details.

### Additional Resources

- [Parallel Processing with mysqlpump](https://dev.mysql.com/doc/mysqlpump/en/)
- [MariaDB Backup Documentation](https://mariadb.com/kb/en/mariadb-backup-overview/)
- [Advanced Backup Techniques](https://mariadb.com/kb/en/backup-and-restore-overview/)

