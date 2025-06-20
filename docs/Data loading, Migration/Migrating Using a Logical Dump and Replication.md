# Migrating Using a Logical Dump and Replication

To minimize downtime during migration, you can set up live replication from your source database to the SkySQL database.

# Prerequisites

1. An active SkySQL account. Identify requirements for your SkySQL implementation prior to [deployment](<../Portal features/Launch page.md>), including:
- Topology - Mariadb Server Single node or with Replica(s)
- [Instance size](<../Reference Guide/Instance Size Choices.md>)
- Storage requirements
- Desired server version
2. An existing source database with the IP added to your SkySQL allowlist.

# Steps

1. **Dump the Source Database**: Take a dump of your source database using `mysqldump` or `mariadb-dump`. Include triggers, procedures, views, and schedules in the dump, and ignore the system databases to avoid conflicts with the existing SkySQL schemas.

    ```bash
    mysqldump -u [username] -p -h [hostname] --single-transaction --master-data=2 --routines --triggers --all-databases --ignore-database=mysql --ignore-database=information_schema --ignore-database=performance_schema --ignore-database=sys > dump.sql
    ```

2. **Create the Users and Grants Separately**: To avoid conflicts with the existing SkySQL users, use `SELECT CONCAT` on your source database to create users and grants in separate files. Note that you may need to create the schema and table grants separately as well.

    ```bash
    mysql -u [username] -p -h [hostname] --silent --skip-column-names -e "SELECT CONCAT('CREATE USER \'', user, '\'@\'', host, '\' IDENTIFIED BY PASSWORD \'', authentication_string, '\';') FROM mysql.user;" > users.sql

    mysql -h [hostname] -u [username] -p --silent --skip-column-names -e "SELECT CONCAT('GRANT ', privilege_type, ' ON ', table_schema, '.* TO \'', grantee, '\';') FROM information_schema.schema_privileges;" > grants.sql

    mysql -h [hostname] -u [username] -p --silent --skip-column-names -e "SELECT CONCAT('GRANT ', privilege_type, ' ON ', table_schema, '.', table_name, ' TO \'', grantee, '\';') FROM information_schema.table_privileges;" >> grants.sql
    ```

3. **Import the Dumps into SkySQL**: Import the logical dumps (SQL files) into your SkySQL database, ensuring to load the user and grant dumps after the main dump.

    ```bash
    mariadb -u [SkySQL username] -p -h [SkySQL hostname] --port 3306 --ssl-verify-server-cert < dump.sql
    mariadb -u [SkySQL username] -p -h [SkySQL hostname] --port 3306 --ssl-verify-server-cert < users.sql
    mariadb -u [SkySQL username] -p -h [SkySQL hostname] --port 3306 --ssl-verify-server-cert < grants.sql
    ```

If you encounter an error while importing your users, you may need to uninstall the `simple_password_check` plugin on your SkySQL instance.

    ```sql
    UNINSTALL PLUGIN simple_password_check;
    ```

4. **Start Replication**: Turn on replication using SkySQL stored procedures. There are procedures allowing you to set and start replication. See our [documentation](<../Reference Guide/Sky Stored Procedures.md>) for details. The `dump.sql` file you created in step 1 will contain the GTID and binary log information needed for the `change_external_primary` procedure.

    ```sql
    CALL sky.change_external_primary(
   host VARCHAR(255),
   port INT,
   logfile TEXT,
   logpos LONG ,
   use_ssl_encryption BOOLEAN
    );
    
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
    mysqldump -u [username] -p --default-parallelism=4 --add-drop-database \
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
- [Migrate RDS MySQL to SkySQL using the AWS Data Migration Service (DMS)](<./migrate-rds-mysql-to-skysql-using-amazon-data-migration-service_whitepaper_1109.pdf>)
