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