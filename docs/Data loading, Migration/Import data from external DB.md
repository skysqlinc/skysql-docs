# Import data from external DB

## Options to import/load data

Data can be loaded to SkySQL services with the assistance of SkySQL Support or by using self-service tools. When importing large data sets, we recommend working with SkySQL Support.

Instructions are provided for the following data import methods:

| Data Import Method | Use Case |
| --- | --- |
| https://mariadb.com/docs/skysql-dbaas/data-operations/data-import/nr-support-assisted/ | Coordinate data import with SkySQL Support |
| https://mariadb.com/docs/skysql-dbaas/data-operations/data-import/nr-load-data-local-infile/ | Import TSV (tab-delimited) or CSV (comma-delimited) file data using the LOAD DATA LOCAL INFILE statement with your database client |
| https://mariadb.com/docs/skysql-dbaas/data-operations/data-import/nr-mariadb-import/ | Import TSV (tab-delimited) or CSV (comma-delimited) file data using the mariadb-import utility |

## Loading using mariadb-dump output

To export data from a MySQL/MariaDB database and import it into MariaDB SkySQL, you can follow these steps:

1. Start by using the `mysqldump` command to export the data from the source database. This command allows you to create a backup of your database in a SQL file. For example, you can use the following command:
    
    ```
    mysqldump -u [username] -p [database_name] > dump.sql
    
    ```
    
    In this command, replace `[username]` with your MySQL/MariaDB username and `[database_name]` with the name of the database you want to export. The `>` symbol redirects the output to a file named `dump.sql`.
    
2. An optional step - Once you have exported the data, you need to transfer the `dump.sql` file to the destination server where you can efficiently connect to your MariaDB SkySQL database. This is typically the same cloud provider region where your Sky DB is running. 
3. After transferring the file, connect to the MariaDB SkySQL server using the command-line client or a GUI tool. This will allow you to interact with the server and perform administrative tasks.
4. If you haven't already created a database in the MariaDB SkySQL server, you can do so using the `CREATE DATABASE` statement. This step is necessary if you want to import the data into a new database. If you already have a database in which you want to import the data, you can skip this step.
5. Finally, import the data from the `dump.sql` file into the destination database using the `mariadb or mysql` command. This command reads the SQL statements in the file and executes them in the specified database. Here's an example command:
    
    ```bash
    ## mariadb -u [username] -p [database_name] < dump.sql
    
    mariadb --host dbpwp27784332.orgtd5j0.db1.skysql.mariadb.com --port 3306 --user dbpwp27784332 -p [database_name] < dump.sql
    ```
    
    Replace `[hostname], username]` with your MariaDB SkySQL username and `[database_name]` with the name of the destination database. The `<` symbol is used to redirect the input from the file `dump.sql`.
    

By following these steps, you will be able to export data from your MySQL/MariaDB database and import it into MariaDB SkySQL, ensuring a smooth transition of your data.

<aside>
💡 Note that mariadb-dump uses a single thread to create a logical backup (SQL) of your database. While this approach can work well on a loaded production DB there are better/faster approaches to dumping and loading data in parallel. You can use the mysqlpump or the mariadb-backup (or mysqlbackup) command to parallelize the export of larger DBs.

</aside>

<aside>
💡 TODO - describe how users can use the SkySQL backup service to do a fast backup and restore of a compatible external database.

</aside>

### Mariadb-dump options

- [mariadb-dump Options for 23.08 ES](https://mariadb.com/docs/skysql-dbaas/ref/es23.08/cli/mariadb-dump/)
- [mariadb-dump Options for 23.07 ES](https://mariadb.com/docs/skysql-dbaas/ref/es23.07/cli/mariadb-dump/)
- [mariadb-dump Options for 10.6 ES](https://mariadb.com/docs/skysql-dbaas/ref/es10.6/cli/mariadb-dump/)
- [mariadb-dump Options for 10.5 ES](https://mariadb.com/docs/skysql-dbaas/ref/es10.5/cli/mariadb-dump/)
- [mariadb-dump Options for 10.4 ES](https://mariadb.com/docs/skysql-dbaas/ref/es10.4/cli/mysqldump/)

## Advanced Options and Best Practices

### Parallel Data Export and Import

For large databases, consider using tools that support parallel processing to speed up the export and import process.

- **mysqlpump**: This utility performs parallel processing and includes features to help with consistent backups.

    ```bash
    mysqlpump -u [username] -p --default-parallelism=4 --add-drop-database --databases [database_name] > dump.sql
    ```

- **mariadb-backup**: This tool allows for non-blocking backups and can be used to parallelize the backup process.

    ```bash
    mariadb-backup --backup --target-dir=/path/to/backup --parallel=4
    ```

### Compression for Efficient Storage and Transfer

Use compression to reduce the size of your backup files, making them faster to transfer and store.

- **Using gzip**:

    ```bash
    mysqldump -u [username] -p [database_name] | gzip > dump.sql.gz
    ```

- **Using mariadb-backup with compression**:

    ```bash
    mariadb-backup --backup --target-dir=/path/to/backup --compress
    ```

### Incremental Backups

For very large datasets, consider using incremental backups to minimize the amount of data to be transferred and imported.

- **mariadb-backup incremental backup**:

    ```bash
    mariadb-backup --backup --target-dir=/path/to/backup --incremental-basedir=/path/to/previous/backup
    ```

### Best Practices for Data Consistency and Integrity

- **Consistent Snapshots**: Use filesystem snapshots (e.g., LVM snapshots) to ensure data consistency during the backup process.

    ```bash
    lvcreate --size 1G --snapshot --name db-snapshot /dev/vg0/db
    ```

- **Data Integrity Checks**: Validate your backups by performing regular restores and checksums.

    ```bash
    md5sum dump.sql
    ```

### Advanced Import Techniques

- **Batch Inserts**: Optimize the import process by using batch inserts to reduce the overhead of individual insert operations.

    ```sql
    LOAD DATA LOCAL INFILE 'data.csv' INTO TABLE my_table
    FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    (column1, column2, column3);
    ```

- **Adjusting Buffer Sizes**: Increase the buffer sizes temporarily to enhance the import performance.

    ```sql
    SET GLOBAL innodb_buffer_pool_size = 2G;
    SET GLOBAL innodb_log_file_size = 512M;
    ```

### Monitoring and Logging

- **Enable Detailed Logging**: Enable detailed logging to monitor the import process and troubleshoot issues effectively.

    ```sql
    SET GLOBAL general_log = 'ON';
    ```

- **Resource Monitoring**: Use monitoring tools to track resource usage (CPU, memory, I/O) during the import process to ensure system stability.

    ```bash
    top
    iostat -x 5
    ```

### Additional Resources

- [Parallel Processing with mysqlpump](https://dev.mysql.com/doc/mysqlpump/en/)
- [MariaDB Backup Documentation](https://mariadb.com/kb/en/mariadb-backup-overview/)
- [Advanced Backup Techniques](https://mariadb.com/kb/en/backup-and-restore-overview/)
