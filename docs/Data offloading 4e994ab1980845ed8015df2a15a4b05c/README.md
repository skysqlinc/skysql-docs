# Data offloading

!!! Note
    TODO - this section needs detailed review


## **Using Mariadb-dump utility**

The simplest way to offload your database is to use the `mariadb-dump` utility:

- The `mariadb-dump` utility provides a command-line interface (CLI)
- The `mariadb-dump` utility is available for Linux and Windows
- The `mariadb-dump` utility supports [many command-line options](https://mariadb.com/docs/skysql-dbaas/ref/mdb/cli/mariadb-dump/)
- Egress charges may apply for customer-initiated backups

`mariadb-dump` usage is fully described [here](
'Data loading, Migration 8d6075f7123a4b59bedfeaad01d8461a'/'Install Mariadb-dump 14d92b62ce0f42cd83557aa80dd8c049'.md). 

## **Using MariaDB client**

Use [MariaDB Client](https://mariadb.com/docs/skysql-previous-release/connect/clients/mariadb-client/) with the connection information to export your schema from your MariaDB SkySQL database service. Here is an example to export all rows from a single table:

```sql
mariadb --host FULLY_QUALIFIED_DOMAIN_NAME --port TCP_PORT \
      --user DATABASE_USER --password \
      --ssl-verify-server-cert \
      --ssl-ca ~/PATH_TO_PEM_FILE \
      --default-character-set=utf8 \
      --batch \
      --skip-column-names \
      --execute='SELECT * FROM accounts.contacts;' \
      > contacts.tsv
```

- Replace `FULLY_QUALIFIED_DOMAIN_NAME` with the Fully Qualified Domain Name of your service.
- Replace `TCP_PORT` with the read-write or read-only port of your service.
- Replace `DATABASE_USER` with the default username for your service, or the username you created.
- Replace `~/PATH_TO_PEM_FILE` with the path to the certificate authority chain (.pem) file.
- Optionally, for large tables, specify the [-quick command-line option](https://mariadb.com/docs/skysql-previous-release/ref/mdb/cli/mariadb/quick/) to disable result caching and reduce memory usage.

## **Select into Outfile**

you can use the **`SELECT INTO OUTFILE`** statement to export query results directly to a file. This is useful for exporting specific data from tables.

- **Usage:**
    
    ```sql
    SELECT * INTO OUTFILE '/path/to/file.csv' FIELDS TERMINATED BY ',' FROM your_table;
    ```
    

!!! Note
    Note that this file system export is not directly accessible to SkySQL users. To generate CSV files, you should contact SkySQL support.


## Replicating data from SkySQL to a compatible external DB

Click [here](Replicating data from SkySQL to external database 65a9d8aa0dc040c09548c042ba6582ae.md)
