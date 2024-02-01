# Connect using ODBC

## Overview

Application developers can use MariaDB Connector/ODBC to establish a data source for client connections with MariaDB database products.

The method for configuring the data source varies between operating systems.

## Configuring a Data Source on Linux

1. Configure `unixODBC` to recognize the driver by creating a file called `MariaDB_odbc_driver_template.ini`with the relevant driver definition.
    
    For example, on CentOS / RHEL / Rocky Linux:
    
    ```
    [MariaDB ODBC 3.1 Driver]
    Description = MariaDB Connector/ODBC v.3.1
    Driver      = /usr/lib64/libmaodbc.so
    ```
    
    On Debian / Ubuntu:
    
    ```
    [MariaDB ODBC 3.1 Driver]
    Description = MariaDB Connector/ODBC v.3.1
    Driver      = /usr/lib/libmaodbc.so
    ```
    
2. Install the driver using the `odbcinst` command.
    
    For example:
    
    ```bash
    $ sudo odbcinst -i -d -f MariaDB_odbc_driver_template.ini
    odbcinst: Driver installed. Usage count increased to 1.
    Target directory is /etc
    ```
    
3. Determine the connection parameters for your database.
4. Configure `unixODBC` to connect to the data source by creating a file called `MariaDB_odbc_data_source_template.ini`with the relevant data source parameters.
    
    For example:
    
    ```
    # Data Source for unixODBC
    [My-Test-Server]
    Description = Describe your database setup here
    Driver      = MariaDB ODBC 3.1 Driver
    Trace       = Yes
    TraceFile   = /tmp/trace.log
    SERVER      = localhost
    SOCKET      = /var/run/mysqld/mysqld.sock
    USER        = db_user
    PASSWORD    = db_user_password
    DATABASE    = test
    ```
    
    ```
    # Data Source for unixODBC
    [My-Test-Server]
    Description = Describe your database setup here
    Driver      = MariaDB ODBC 3.1 Driver
    Trace       = Yes
    TraceFile   = /tmp/trace.log
    SERVER      = example.skysql.com
    PORT        = 3306
    SSLVERIFY   = 1
    USER        = db_user
    PASSWORD    = db_user_password
    DATABASE    = test
    ```
    
    - Customize the values of the parameters with the relevant information for your environment.
5. Install the `unixODBC` data source template file:
    
    ```bash
    $ sudo odbcinst -i -s -h -f MariaDB_odbc_data_source_template.ini
    ```
    
6. Test the data source `My-Test-Server`configured in the `MariaDB_odbc_data_source_template.ini`
file using the `isql` command:
    
    ```
    $ isql My-Test-Server
    +-------------------------+
    | Connected!              |
    | sql-statement           |
    | help[tablename]         |
    | quit                    |
    +-------------------------+
    SQL>
    ```
    
7. To select your new data source in your application, select the data source with the name that you configured, which is `My-Test-Server` in the above example.

## Configuring a Data Source on macOS

1. Confirm that MariaDB Connector/ODBC has been registered with`iODBC` by confirming that the following options are set in the `iODBC`configuration file at `/Library/ODBC/odbcinst.ini`:
    
    ```
    [ODBC]
    Trace     = no
    TraceFile = /tmp/iodbc_trace.log
    
    [ODBC Drivers]
    MariaDB ODBC 3.1 Unicode Driver = Installed
    
    [MariaDB ODBC 3.1 Unicode Driver]
    Driver      = /Library/MariaDB/MariaDB-Connector-ODBC/libmaodbc.dylib
    Description = MariaDB Connector/ODBC(Unicode) 3.1 64bit
    Threading   = 0
    ```
    
2. Determine the connection parameters for your database.
3. Add a data source for your database to `iODBC` by adding the following options to the `iODBC` configuration file at `/Library/ODBC/odbc.ini`:
    
    ```
    [ODBC Data Sources]
    My-Test-Server = MariaDB ODBC 3.1 Unicode Driver
    
    [My-Test-Server]
    Driver   = /Library/MariaDB/MariaDB-Connector-ODBC/libmaodbc.dylib
    SERVER   = 192.0.2.1
    DATABASE = test
    USER     = db_user
    PASSWORD = db_user_password
    ```
    
    - Substitute the values of the `SERVER`, `SOCKET`, `DATABASE`, `PORT`, `USER`, and `PASSWORD` parameters with the relevant value for your environment.
4. Test the data source using the `iodbctest`command:
    
    ```
    $ iodbctest "DSN=My-Test-Server"
    ```
    
5. To select your new data source in your application, select the data source with the name that you configured, which is `My-Test-Server` in the above example.

## Configuring a Data Source on Windows

MariaDB Connector/ODBC requires at least Windows 8.

Windows 10 was used to prepare these instructions. When using other versions of Windows, these instructions may require adjustment.

1. In the start menu, search for "ODBC Data Sources".
2. In the search results, open the application called "ODBC Data Sources (32-bit)" or "ODBC Data Sources (64-bit)", depending on whether you need a data source for a 32-bit or 64-bit application.
3. In the [ODBC Data Source Administrator](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/open-the-odbc-data-source-administrator), click the "Add" button on the right side.
4. In the "Create New Data Source" window:
    - Click on "MariaDB ODBC 3.1 Driver" in the list.
    - Click the "Finish" button.
5. In the "Create a new Data Source to MariaDB" window:
    - In the "Name" text box, enter a name for the data source.
    - In the "Description" test box, enter a description for the data source.
    - Click the "Next" button.
6. In the next window, provide the connection credentials:
    - In the "Server Name" field, provide the IP address or domain name for the Server.
    - In the "User name" field, provide the username for the database user account.
    - In the "Password" field, provide the password for that user.
    - In the "Database" field, provide the the default database to use.
    - Then, click the "Next" button.
7. Continue configuring the data source using the wizard:
    - The wizard provides a series of windows for configuring various aspects of the connection, such as enabling TLS encryption. Enable settings you want to use.
    - Click the "Next" button to move onto the next window in the wizard.
    - Click the "Finish" on the last window to exit the wizard and save your data source.
8. To select your new data source in your application, select the data source with the name that you configured for the "Name" field.

## Failover

MariaDB Connector/ODBC supports failover in case one or more hosts are not available.

The failover feature requires using MariaDB Connector/ODBC 3.1.16 or greater with MariaDB Connector/C 3.3 or greater.

MariaDB Connector/ODBC 3.1.16 and greater is statically linked for Windows and macOS with MariaDB Connector/C 3.3.1. MariaDB Connector/ODBC 3.1.16 and greater is dynamically linked for Linux with MariaDB Connector/C.

The failover feature is enabled by providing a comma separated list of hosts as a server name.

The failover host string is the `SERVER` string. If the `SERVER` string does not include a port, the default port will be used.

The following syntax is required:

- IPv6 addresses must be enclosed within square brackets `"[]"`
- hostname and port must be separated by a colon `":"`
- `hostname:port` pairs must be be separated by a comma `","`
- If only one `hostname:port` is specified, the host string must end with a comma
- If no port is specified, the default port will be used

An example of a failover host string:

`[::1]:3306,192.168.0.1:3307,test.example.com`

## Connection Parameters

| Connection Parameter | Description | Default Value |
| --- | --- | --- |
| DRIVER | • On Linux, the name of the driver, which is configured in the
unixODBC driver template file.
• On macOS, the path to the driver's shared library, which is
installed at /Library/MariaDB/MariaDB-Connector-ODBC/libmaodbc.dylib
by default. |  |
| SERVER | Host name, IPv4 address, or IPv6 address of the database
server. | localhost |
| SOCKET | • The path to the socket file. On Linux, MariaDB Enterprise Server
uses different default socket files on different Linux
distributions.
• On Debian / Ubuntu, the default socket file is /var/run/mysqld/mysqld.sock
or /run/mysqld/mysqld.sock.
• On CentOS / RHEL / Rocky Linux, the default socket file is /var/lib/mysql/mysql.sock. | /tmp/mysql.sock |
| DATABASE | Database name to select upon successful connection. The database
must already exist, and the user account must have privileges to select
it. |  |
| PORT | TCP port of the database server. | 3306 |
| USER | The username to use for authentication. |  |
| PASSWORD | User password. |  |
| FORWARDONLY | When enabled, cursors are created as SQL_CURSOR_FORWARD_ONLY,
so they can only move forward. Starting in Connector/ODBC 3.2, cursors
are SQL_CURSOR_FORWARD_ONLY
by default. In previous releases, cursors are created as SQL_CURSOR_STATIC by
default. |  |
| NO_CACHE | When enabled, result set streaming is enabled, which enables the
application to fetch result sets from the server row-by-row instead of
caching the entire result set on the client side. Since the application
is not caching the entire result set, the application is less likely to
run out of memory when working with large result sets. |  |
| STREAMRS | Alias for the NO_CACHE connection
parameter. |  |
| OPTIONS | See about:blank#OPTIONS_Bitmaskabout:blank#OPTIONS_Bitmask. |  |
| PREPONCLIENT | When enabled, the SQLPrepare ODBC API
function uses the text protocol and client-side prepared statements
(CSPS). |  |
| ATTR | Sets connection attributes that can be queried via the https://www.notion.so../../../../ref/mdb/performance-schema/session_account_connect_attrs/https://www.notion.so../../../../../server/ref/mdb/performance-schema/session_account_connect_attrs/
and https://www.notion.so../../../../ref/mdb/performance-schema/session_connect_attrs/https://www.notion.so../../../../../server/ref/mdb/performance-schema/session_connect_attrs/
tables when https://www.notion.so../../../../ref/mdb/system-variables/performance_schema/https://www.notion.so../../../../../server/ref/mdb/system-variables/performance_schema/
is enabled. Specify attributes in the format ATTR={<attrname1>=<attrvalue1>[,<attrname2=attrvalue2,...]} |  |

| What | Where to find it |
| --- | --- |
| DRIVER | • On Linux, the name of the driver, which is configured in the
unixODBC driver template file.
• On macOS, the path to the driver's shared library, which is
installed at /Library/MariaDB/MariaDB-Connector-ODBC/libmaodbc.dylib
by default. |
| SERVER | Fully Qualified Domain Name in the https://www.notion.so../../../connection-parameters-portal/ |
| PORT | Read-Write Port or Read-Only Port in the https://www.notion.so../../../connection-parameters-portal/ |
| USER | Default username in the Service Credentials view, or the username
you created |
| PASSWORD | Default password in the Service Credentials view, the password
you set on the default user, or the password for the user you
created |
| SSLVERIFY | Set to 1 to connect with SSL |
| FORCETLS | Set to 1 to enable TLS |
| FORWARDONLY | When enabled, cursors are created as SQL_CURSOR_FORWARD_ONLY,
so they can only move forward. Starting in Connector/ODBC 3.2, cursors
are SQL_CURSOR_FORWARD_ONLY
by default. In previous releases, cursors are created as SQL_CURSOR_STATIC by
default. |
| NO_CACHE | When enabled, result set streaming is enabled, which enables the
application to fetch result sets from the server row-by-row instead of
caching the entire result set on the client side. Since the application
is not caching the entire result set, the application is less likely to
run out of memory when working with large result sets. |
| STREAMRS | Alias for the NO_CACHE connection
parameter. |
| OPTIONS | See about:blank#OPTIONS_Bitmaskabout:blank#OPTIONS_Bitmask. |
| PREPONCLIENT | When enabled, the SQLPrepare ODBC API
function uses the text protocol and client-side prepared statements
(CSPS). |
| ATTR | Sets connection attributes that can be queried via the https://www.notion.so../../../../ref/mdb/performance-schema/session_account_connect_attrs/
and https://www.notion.so../../../../ref/mdb/performance-schema/session_connect_attrs/
tables when https://www.notion.so../../../../ref/mdb/system-variables/performance_schema/
is enabled. Specify attributes in the format ATTR={<attrname1>=<attrvalue1>[,<attrname2=attrvalue2,...]} |

## `OPTIONS` Bitmask

The `OPTIONS` bitmask
contains the following bits:

| Bit Number | Bit Value | Description |
| --- | --- | --- |
| 0 | 1 | Unused |
| 1 | 2 | Tells connector to return the number of matched rows instead of
number of changed rows |
| 4 | 16 | Same as NO_PROMPT connection
parameter |
| 5 | 32 | Forces all cursors to be dynamic |
| 6 | 64 | Forbids the DATABASE_NAME.TABLE_NAME.COLUMN_NAME
syntax |
| 11 | 2048 | Enables compression in the protocol |
| 13 | 8192 | Same as the NAMEDPIPE connection
parameter |
| 16 | 65536 | Same as the USE_MYCNF connection
parameter |
| 20 | 1048576 | Same as the NO_CACHE connection
parameter |
| 21 | 2097152 | Same as the FORWARDONLY
connection parameter |
| 22 | 4194304 | Same as the AUTO_RECONNECT
connection parameter |
| 26 | 67108864 | Enables multi-statement queries |
