# Import Data from External DB

## Options to Import/Load Data

Data can be loaded to SkySQL services with the assistance of SkySQL Support or by using self-service tools. When importing large data sets, we recommend working with SkySQL Support.

Instructions are provided for the following data import methods:

| Data Import Method | Use Case |
| --- | --- |
| [Coordinate data import with SkySQL Support](https://mariadb.com/docs/skysql-dbaas/data-operations/data-import/nr-support-assisted/) | Coordinate data import with SkySQL Support |
| [Import TSV or CSV file data using LOAD DATA LOCAL INFILE](https://mariadb.com/docs/skysql-dbaas/data-operations/data-import/nr-load-data-local-infile/) | Import TSV (tab-delimited) or CSV (comma-delimited) file data using the `LOAD DATA LOCAL INFILE` statement with your database client |
| [Import TSV or CSV file data using mariadb-import](https://mariadb.com/docs/skysql-dbaas/data-operations/data-import/nr-mariadb-import/) | Import TSV (tab-delimited) or CSV (comma-delimited) file data using the `mariadb-import` utility |

## Loading Using mariadb-dump Output

To export data from a MySQL/MariaDB database and import it into MariaDB SkySQL, follow these steps:

1. Start by using the `mysqldump` command to export the data from the source database. This command creates a backup of your database in a SQL file:

    ```bash
    mysqldump -u [username] -p [database_name] > dump.sql
    ```

    Replace `[username]` with your MySQL/MariaDB username and `[database_name]` with the name of the database you want to export. The `>` symbol redirects the output to a file named `dump.sql`.
    
2. Transfer the `dump.sql` file to the destination server where you can efficiently connect to your MariaDB SkySQL database. This is typically the same cloud provider region where your SkySQL DB is running.

3. Connect to the MariaDB SkySQL server using the command-line client or a GUI tool to interact with the server and perform administrative tasks.

4. If you haven't already created a database in the MariaDB SkySQL server, you can do so using the `CREATE DATABASE` statement. Skip this step if you already have a database into which you want to import the data.

5. Import the data from the `dump.sql` file into the destination database using the `mariadb` or `mysql` command:

    ```bash
    mariadb --host FULLY_QUALIFIED_DOMAIN_NAME --port 3306 --user [username] -p [database_name] < dump.sql
    ```

    Replace `[FULLY_QUALIFIED_DOMAIN_NAME]`, `[username]`, and `[database_name]` with your MariaDB SkySQL hostname, username, and destination database name, respectively. The `<` symbol redirects the input from the file `dump.sql`.

By following these steps, you can export data from your MySQL/MariaDB database and import it into MariaDB SkySQL, ensuring a smooth data transition.

💡 **Note**: `mariadb-dump` uses a single thread to create a logical backup (SQL) of your database. While this approach can work well on a loaded production DB, there are better/faster approaches to dumping and loading data in parallel. You can use the `mysqlpump` or the `mariadb-backup` (or `mysqlbackup`) command to parallelize the export of larger databases.

💡 **TODO**: Describe how users can use the SkySQL backup service to perform a fast backup and restore of a compatible external database.

### mariadb-dump Options

- [mariadb-dump Options for 23.08 ES](https://mariadb.com/docs/skysql-dbaas/ref/es23.08/cli/mariadb-dump/)
- [mariadb-dump Options for 23.07 ES](https://mariadb.com/docs/skysql-dbaas/ref/es23.07/cli/mariadb-dump/)
- [mariadb-dump Options for 10.6 ES](https://mariadb.com/docs/skysql-dbaas/ref/es10.6/cli/mariadb-dump/)
- [mariadb-dump Options for 10.5 ES](https://mariadb.com/docs/skysql-dbaas/ref/es10.5/cli/mariadb-dump/)
- [mariadb-dump Options for 10.4 ES](https://mariadb.com/docs/skysql-dbaas/ref/es10.4/cli/mysqldump/)
