# Import CSV Data

MariaDB SkySQL customers can import data into a SkySQL service using the `LOAD DATA LOCAL INFILE` SQL statement:

- The `LOAD DATA LOCAL INFILE` statement can import data from TSV and CSV files.
- The `LOAD DATA LOCAL INFILE` statement can be executed by any client or connector.

!!! Note
    Ensure your schema is already created in the database. For importing entire databases or creating tables, consider using `mariadb-import`.

## Enable Local Infiles

Support for local infiles must be enabled on both the client side and the SkySQL service.

### Enable Local Infiles on the Client or Connector

To execute the [`LOAD DATA LOCAL INFILE`](https://mariadb.com/docs/skysql-dbaas/ref/mdb/sql-statements/LOAD_DATA_INFILE/) statement, most clients and connectors require a specific option to be enabled.

If you are using `mariadb` client, the [`--local-infile` option](https://mariadb.com/docs/skysql-dbaas/ref/mdb/cli/mariadb/local-infile/) must be specified.

### Enable Local Infiles in SkySQL

Support for local infiles must be enabled on the SkySQL service.

For SkySQL services that use MariaDB Enterprise Server and MariaDB Enterprise ColumnStore, the [`local_infile` system variable](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/) must be enabled:

- For Replicated Transactions and Single Node Transactions services, the [`local_infile` system variable](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/) is `OFF` by default.

[Configuration Manager](https://mariadb.com/docs/skysql-dbaas/service-management/nr-configuration-management/) can be used to modify the value of the [`local_infile` system variable](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/).

## Import Data

1. Determine the [connection parameters](https://mariadb.com/docs/skysql-dbaas/connect/nr-client-connections/) for your MariaDB SkySQL service.
2. Connect with the `mariadb` client and specify the [`--local-infile` option](https://mariadb.com/docs/skysql-dbaas/ref/mdb/cli/mariadb/local-infile/), which is needed by the next step:

    ```bash
    mariadb --host FULLY_QUALIFIED_DOMAIN_NAME --port TCP_PORT \
          --user DATABASE_USER --password \
          --ssl-verify-server-cert \
          --ssl-ca ~/PATH_TO_PEM_FILE \
          --default-character-set=utf8 \
          --local-infile
    ```

    After executing the command, you will be prompted for a password. Enter the default password for your user, or the password for the database user you created.

For each table that you want to import, execute the [`LOAD DATA LOCAL INFILE`](https://mariadb.com/docs/skysql-dbaas/ref/mdb/sql-statements/LOAD_DATA_INFILE/) statement to import the data from the TSV or CSV file into your MariaDB SkySQL database service.

For a TSV file:

```sql
LOAD DATA LOCAL INFILE 'contacts.tsv'
INTO TABLE accounts.contacts;

### Advanced Options and Best Practices

- **Error Handling**: Use the `IGNORE` or `REPLACE` option to manage duplicate records.

    ```sql
    LOAD DATA LOCAL INFILE 'contacts.csv'
    INTO TABLE accounts.contacts
    FIELDS TERMINATED BY ','
    IGNORE 1 LINES;
    ```

- **Performance Optimization**: For large data imports, temporarily disable indexes and foreign key checks to speed up the import process, then re-enable them afterward.

    ```sql
    SET foreign_key_checks = 0;
    ALTER TABLE accounts.contacts DISABLE KEYS;

    LOAD DATA LOCAL INFILE 'contacts.csv'
    INTO TABLE accounts.contacts
    FIELDS TERMINATED BY ',';

    ALTER TABLE accounts.contacts ENABLE KEYS;
    SET foreign_key_checks = 1;
    ```

- **Data Validation**: Validate data format and consistency before import to avoid errors during the process.

## Best Practices

- **Ensure Data Integrity**: Validate your data before loading to prevent errors and ensure consistency.
- **Optimize Performance**: Use bulk loading techniques and optimize your database configurations for large data imports.
- **Handle Errors**: Implement robust error handling mechanisms to manage any issues during the data import process.
- **Logging and Monitoring**: Enable logging and monitor the import process to identify and address issues promptly.
