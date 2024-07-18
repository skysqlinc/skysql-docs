# Import CSV data

SkySQL customers can import data into a SkySQL service using the `LOAD DATA LOCAL INFILE` SQL statement:

- The `LOAD DATA LOCAL INFILE` statement can import data from TSV and CSV files
- The `LOAD DATA LOCAL INFILE` statement can be executed by any client or connector

!!! Note
    Make sure your schema is already created in the database. If you need to import entire databases or create tables, you should use mariab-import.


## **Enable Local Infiles**

Support for local infiles must be enabled on the client side and on the SkySQL service.

### **Enable Local Infiles on the Client or Connector**

To execute the [`LOAD DATA LOCAL INFILE`](https://mariadb.com/docs/skysql-dbaas/ref/mdb/sql-statements/LOAD_DATA_INFILE/) statement, most clients and connectors require a specific option to be enabled.

If you are using `mariadb` client, the [`--local-infile` option](https://mariadb.com/docs/skysql-dbaas/ref/mdb/cli/mariadb/local-infile/) must be specified.

### **Enable Local Infiles in SkySQL**

Support for local infiles must be enabled on the SkySQL service.

For SkySQL services that use MariaDB Enterprise Server and MariaDB Enterprise ColumnStore, the [local_infile system variable](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/) must be enabled:

- For Replicated Transactions and Single Node Transactions services, the [local_infile system variable](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/) is `OFF` by default

[Configuration Manager](https://mariadb.com/docs/skysql-dbaas/service-management/nr-configuration-management/) can be used to modify the value of the [local_infile system variable](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/).

## **Import Data**

1. Determine the [connection parameters](https://mariadb.com/docs/skysql-dbaas/connect/nr-client-connections/) for your SkySQL service.
2. Connect with the `mariadb` client and specify the [-local-infile option](https://mariadb.com/docs/skysql-dbaas/ref/mdb/cli/mariadb/local-infile/), which is needed by the next step:

```bash
mariadb --host FULLY_QUALIFIED_DOMAIN_NAME --port TCP_PORT \
      --user DATABASE_USER --password \
      --ssl-verify-server-cert \
      --default-character-set=utf8 \
      --local-infile
```

After the command is executed, you will be prompted for a password. Enter the default password for your default user, the password you set for the default user, or the password for the database user you created.

For each table that you want to import, execute the [LOAD DATA LOCAL INFILE](https://mariadb.com/docs/skysql-dbaas/ref/mdb/sql-statements/LOAD_DATA_INFILE/) statement to import the data from the TSV or CSV file into your SkySQL database service.

For a TSV file:

```bash
LOAD DATA LOCAL INFILE 'contacts.tsv'
INTO TABLE accounts.contacts;
```

For a CSV file:

```bash
LOAD DATA LOCAL INFILE 'contacts.csv'
INTO TABLE accounts.contacts
FIELDS TERMINATED BY ',';
```

## **Using a Connector**

If you are using a MariaDB Connector, then you must select the method for the specific connector from the list below.

If you are using MariaDB Connector/C, the `MYSQL_OPT_LOCAL_INFILE` option can be set with the `mysql_optionsv()` function:

```bash
/* enable local infile */
unsigned int enable_local_infile = 1;
mysql_optionsv(mysql, MYSQL_OPT_LOCAL_INFILE, (void *) &enable_local_infile);
```

If you are using MariaDB Connector/J, the `allowLocalInfile` parameter can be set for the connection:

```bash
Connection connection = DriverManager.getConnection("jdbc:mariadb://FULLY_QUALIFIED_DOMAIN_NAME:TCP_PORT/test?user=DATABASE_USER&password=DATABASE_PASSWORD&allowLocalInfile=true");
```

If you are using MariaDB Connector/Node.js, the `permitLocalInfile` parameter can be set for the connection:

```bash
mariadb.createConnection({
   host: 'FULLY_QUALIFIED_DOMAIN_NAME',
   port: 'TCP_PORT',
   user:'DATABASE_USER',
   password: 'DATABASE_PASSWORD',
   permitLocalInfile: 'true'
 });
```

If you are using MariaDB Connector/Python, the `local_infile` parameter can be set for the connection:

```bash
conn = mariadb.connect(
   user="DATABASE_USER",
   password="DATABASE_PASSWORD",
   host="FULLY_QUALIFIED_DOMAIN_NAME",
   port=TCP_PORT,
   local_infile=true)
```