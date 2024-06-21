# Connecting to Sky DBs

This page describes how to connect to a SkySQL database using a MariaDB-compatible client.

### **Important - Whitelist your IP address first**

!!! Note
    💡 **Access to all services are protected by a firewall, by default. You need to IP whitelist your client’s (your desktop, laptop or server) IP. Just select ‘Manage —> Security Access’ and then click ‘Add my current IP’ to add the IP of your current workstation (laptop, desktop).**

!!! Note
    💡 **If you are not sure or unable to obtain the IP address, you can use 0.0.0.0/0 to effectively disable the firewall. Goes without saying — don’t do this for your production DBs.**


For more details go to the [Firewall](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/) settings page. 



## Connecting using the MariaDB Client CLI

Once your DB service is launched, click on the ‘Connect’ option for your service on the dashboard. This pops up all the required attributes to connect from any SQL client. 

Connection parameters include:

- Default username
- Default password
- Hostname (Fully Qualified Domain Name)
- TCP port (3306 or 3307)
- ssl-verify-server-cert (if SSL is ON)


![Connect window example](connect_window.png)


### Install and Connect using the MariaDB client

If using a mac, install MariaDB using `brew install mariadb`.  Go through [MariaDB Client](Connect%20using%20MariaDB%20CLI.md) for details on how to connect from Linux or Windows. 

Finally, simply copy/paste the MariaDB CLI command as displayed in the Connect window. 

## Connecting from your Application

Applications can connect to SkySQL using any of the below MariaDB supported connectors. There are several other connectors from the community too. 

- [C](Connect%20from%20‘C’%20App.md)
- [C++](Connect%20from%20‘C++’%20App.md)
- [Java](Connect%20from%20Java%20App.md)
- [Java R2DBC](Connect%20using%20Connector%20R2DBC.md)
- [Node.js (JavaScript)](Connect%20from%20Node%20js%20App.md)
- [ODBC API](Connect%20using%20ODBC.md)
- [Python](Connect%20from%20Python%20App.md)
- [MongoDB Client](Connect%20from%20MongoDB%20clients.md)


!!! Note
    💡 For Enterprise Server With Replica(s), you can also use any MongoDB client and use the [NoSQL Interface](Connect%20from%20MongoDB%20clients.md)


## Connecting from SQL tools

Clients listed here have been tested to properly connect with SkySQL and execute queries.

!!! Note
    💡 Unlike previous SkySQL versions, the current version no longer requires clients to supply the Server SSL Certificate for SSL connections. Customers who migrated from MariaDB corporation to SkySQL Inc can continue to use provided certificates (when using the previous SkySQL method for connecting). But, we strongly recommend moving to the connection properties as shown in the Connect window for your service.

Most of the SQL clients and editors natively support MariaDB. Most often you can also just select 'MySQL' and connect to your SkySQL DB service. 

- [Connecting using Java clients like Squirrel SQL](https://squirrel-sql.sourceforge.io/)  
    - All you need to do is to make sure the "useSsl" property is set to 'true' if SSL is ON. 
- [TablePlus](https://tableplus.com/download) 
    - If SSL was configured, you should set the SSL Mode option to 'ENFORCE' and not 'VERIFY-SERVER-CERT'. 
    - When using the "ENFORCE" SSL mode in TablePlus or any MySQL client, the client will still verify that the SSL certificate presented by the server is valid and trusted. This includes verifying that the certificate is issued by a trusted Certificate Authority (CA) and that it has not expired or been revoked.
    - In the "ENFORCE" mode, the client requires the server to present a valid SSL certificate during the SSL handshake process. - The client will then verify the following aspects of the certificate:
    - Certificate Chain: The client will check if the server's SSL certificate is part of a valid certificate chain, leading back to a trusted root CA certificate.
    - Certificate Expiry: The client will verify that the server's SSL certificate has not expired.
    - Certificate Revocation: The client may also check if the certificate has been revoked by the issuing CA.
    - If any of these checks fail, the client will not establish the SSL connection and may display an error indicating that the certificate is not valid or trusted.
- [MariaDB CLI](Connect%20using%20MariaDB%20CLI.md) 
- [DBGate](https://dbgate.org/) 
    - When using SSL, you only have to switch to the SSL Tab in the Connection window and select 'use SSL' and click Connect. 
- [Sequel Ace](https://sequel-ace.com/) - Connect to MariaDB from MacOS
    - In the connection window, you should select 'Require SSL' if your SkySQL database has SSL turned ON (the default). 



!!! Note
    💡  The links below point to the older version of Docs. In all cases you DO NOT need to pass any Certificates. Just set 'use SSL' where available to true when using SSL. 

- [DBeaver](https://mariadb.com/docs/skysql-previous-release/connect/clients/dbeaver/)
- [dbForge Studio](https://mariadb.com/docs/skysql-previous-release/connect/clients/dbforge-studio/)
- [HeidiSQL](https://mariadb.com/docs/skysql-previous-release/connect/clients/heidisql/)
- [MariaDB Shell](https://mariadb.com/docs/skysql-previous-release/connect/clients/mariadb-shell/)
- [Microsoft Power BI](https://mariadb.com/docs/skysql-previous-release/connect/business-intelligence/)
- [MySQL Workbench](https://mariadb.com/docs/skysql-previous-release/connect/clients/mysql-workbench/)
- [Navicat](https://mariadb.com/docs/skysql-previous-release/connect/clients/navicat/)
- [SQLyog](https://mariadb.com/docs/skysql-previous-release/connect/clients/sqlyog/)
