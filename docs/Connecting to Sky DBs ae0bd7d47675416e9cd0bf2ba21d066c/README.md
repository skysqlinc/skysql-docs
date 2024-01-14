# Connecting to Sky DBs

This page describes how to connect to a SkySQL database using a MariaDB-compatible client.

!!! Note
    The easiest way to get a SQL connection to any MariaDB database launched within your SkySQL account is through the built-in [Query Editor](https://mariadb.com/docs/skysql-dbaas/connect/nr-query-editor/) -  run queries directly in the browser to interact with data stored in SkySQL databases.


!!! Note
    💡 For Enterprise Server With Replica(s), you can also use any MongoDB client and use the [NoSQL Interface](https://mariadb.com/docs/skysql-dbaas/connect/nr-nosql-interface/) .


## Connection Parameters

Once your DB service is launched, click on the ‘Connect’ option for your service on the dashboard. This pops up all the required attributes to connect from any SQL client. 

### **Important - Whitelist your IP address first**

!!! Note
    💡 **Access to all services are protected by a firewall, by default. You need to IP whitelist your client’s (your desktop, laptop or server) IP. Just select ‘Manage —> Security Access’ and then click ‘Add my current IP’ to add the IP of your current workstation (laptop, desktop).**

!!! Note
    💡 **If you are not sure or unable to obtain the IP address, you can use 0.0.0.0/0 to effectively disable the firewall. Goes without saying — don’t do this for your production DBs.**


For more details go to the [Firewall](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/) settings page. 

Connection parameters include:

- Default username
- Default password
- Hostname (Fully Qualified Domain Name)
- TCP port
- PEM file containing the Certificate Authority Chain

To connect to a service, you must also manage the [Firewall](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/) settings and add the client's IP address or netblock to the allowlist.

![https://mariadb.com/docs/_images/screenshots/services-tx-xpand-connect-params.png](https://mariadb.com/docs/_images/screenshots/services-tx-xpand-connect-params.png)

*Service-specific Connection Parameters*



## Connecting using the MariaDB client

If using a mac, install MariaDB using `brew install mariadb`.  Go through [MariaDB Client](https://mariadb.com/docs/skysql-previous-release/connect/clients/mariadb-client/) for details on how to connect from Linux or Windows. 

Next, If you turned SSL, you need to download the ‘Certificate Authentication Chain’ from the Connect popup. 

Finally, simply copy/paste the MariaDB CLI command as displayed in the Connect window. 

## Connecting from your Application

Applications can connect to MariaDB SkySQL DBs using any of the below MariaDB supported connectors. There are several other connectors from the community too. 

- [C](Connect%20from%20%E2%80%98C%E2%80%99%20App%20b31c2f5880a74e198c61a60e74076cec.md)
- [C++](Connect%20from%20%E2%80%98C++%E2%80%99%20App%20697a64cebee24fcf996e21450f1c32f0.md)
- [Java](Connect%20from%20Java%20App%2017bf88ae787b44a0bccb8d5eaac5f8ef.md)
- [Java R2DBC](Connect%20using%20Connector%20R2DBC%206f657edcac0849cb8ab4be6649ca71bd.md)
- [Node.js (JavaScript)](Connect%20from%20Node%20js%20App%20b63d489b6fbd426ab40d7f7f3be9025a.md)
- [ODBC API](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/odbc-api/)
- [Python](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/python/)

## Connecting from SQL tools

Clients listed here have been tested to properly connect with MariaDB SkySQL and execute queries.

- [DBeaver](https://mariadb.com/docs/skysql-previous-release/connect/clients/dbeaver/)
- [dbForge Studio](https://mariadb.com/docs/skysql-previous-release/connect/clients/dbforge-studio/)
- [HeidiSQL](https://mariadb.com/docs/skysql-previous-release/connect/clients/heidisql/)
- [MariaDB Client](https://mariadb.com/docs/skysql-previous-release/connect/clients/mariadb-client/)
- [MariaDB Shell](https://mariadb.com/docs/skysql-previous-release/connect/clients/mariadb-shell/)
- [Microsoft Power BI](https://mariadb.com/docs/skysql-previous-release/connect/business-intelligence/)
- [MySQL Workbench](https://mariadb.com/docs/skysql-previous-release/connect/clients/mysql-workbench/)
- [Navicat](https://mariadb.com/docs/skysql-previous-release/connect/clients/navicat/)
- [SQLyog](https://mariadb.com/docs/skysql-previous-release/connect/clients/sqlyog/)
- [TablePlus](https://mariadb.com/docs/skysql-previous-release/connect/clients/tableplus/)

## Client Configuration

1. Manage the [Firewall](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/) settings and add the client's IP address or netblock to the allowlist.
2. Download the "Certificate Authentication Chain" PEM file by clicking the displayed PEM file link. The PEM file used for the current release of SkySQL is different from the PEM files used by SkySQL previous release.
3. Configure your client per the [Connection Parameters](https://mariadb.com/docs/skysql-dbaas/connect/nr-client-connections/#Connection_Parameters).
    - While instructions are provided in the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/) for connecting with the MariaDB Client (`mariadb`), there are many compatible [programming language connectors](https://mariadb.com/docs/skysql-dbaas/connect/nr-client-connections/#Supported_Connectors) and [client applications](https://mariadb.com/docs/skysql-dbaas/connect/nr-client-connections/#Supported_Clients).

