# Connect from ‘C++’ App

MariaDB Connector/C++ enables C++ applications to establish client connections to SkySQL and MariaDB databases over TLS.

# Requirement

MariaDB Connector/C++ has dependencies. You must install MariaDB Connector/C to use it.

| MariaDB Connector/C++ | MariaDB Connector/C |
| --- | --- |
| 1.1 | 3.2.3 or later |
| 1.0 | 3.1.1 or later |

For additional information, see "[MariaDB Connector/C++ Release Notes](https://mariadb.com/docs/server/release-notes/mariadb-connector-cpp-1-0/)".

# Linux Installation (Binary Tarball)

To install MariaDB Connector/C++ on Linux:

1. [Install MariaDB Connector/C](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/c/install/).
2. Go to the [MariaDB Connector C++ download page](https://mariadb.com/downloads/?tab=connectors&group=connectors_dataaccess&product=C%2B%2B+connector).
3. In the "OS" dropdown, select the Linux distribution you want to use.
4. Click the "Download" button to download the binary tarball.
5. Extract the tarball:
    
    `$ tar -xvzf mariadb-connector-cpp-*.tar.gz`
    
6. Change into the relevant directory:
    
    `$ cd mariadb-connector-cpp-*/`
    
7. Install the directories for the header files:
    
    `$ sudo install -d /usr/include/mariadb/conncpp
    $ sudo install -d /usr/include/mariadb/conncpp/compat`
    
8. Install the header files:
    
    `$ sudo install include/mariadb/* /usr/include/mariadb/
    $ sudo install include/mariadb/conncpp/* /usr/include/mariadb/conncpp
    $ sudo install include/mariadb/conncpp/compat/* /usr/include/mariadb/conncpp/compat`
    
9. Install the directories for the shared libraries:
    - On CentOS, RHEL, Rocky Linux:
        
        `$ sudo install -d /usr/lib64/mariadb
        $ sudo install -d /usr/lib64/mariadb/plugin`
        
    - On Debian, Ubuntu:
        
        `$ sudo install -d /usr/lib/mariadb
        $ sudo install -d /usr/lib/mariadb/plugin`
        
10. Install the shared libraries:
    - On CentOS, RHEL, Rocky Linux:
        
        `$ sudo install lib64/mariadb/libmariadbcpp.so /usr/lib64
        $ sudo install lib64/mariadb/plugin/* /usr/lib64/mariadb/plugin`
        
    - On Debian, Ubuntu:
        
        `$ sudo install lib/mariadb/libmariadbcpp.so /usr/lib
        $ sudo install lib/mariadb/plugin/* /usr/lib/mariadb/plugin`
        

# Windows Installation (MSI)

To install MariaDB Connector/C++ on Windows:

1. MariaDB Connector/C dependency will be installed when Connector/C++ is installed.
2. Go to the [MariaDB Connector C++ download page for MS Windows](https://mariadb.com/downloads/?tab=connectors&group=connectors_dataaccess&product=C%2B%2B+connector&os=MS+Windows+(64-bit)).
3. Click the "Download" button to download the MSI package.
4. Run the MSI package and click "Next" to start the Setup Wizard.
5. On the second screen, click the license agreement checkbox, then click "Next."
6. On the third screen, click "Typical."
7. On the fourth screen, click "Install."
8. Click "Finish."
9. Add the directory path that contains the `mariadbcpp` `LIB` file (example `"C:\Program Files\MariaDB\MariaDB C++ Connector 64-bit"`) to `PATH` environment variable.

# Latest Software Releases

| Version | Latest Release | Latest Release Date | Maturity |
| --- | --- | --- | --- |
| MariaDB Connector/C++ 1.1 | https://mariadb.com/docs/server/release-notes/mariadb-connector-cpp-1-1/1-1-2/ | 2022-11-30 | Release Candidate |
| MariaDB Connector/C++ 1.0 | https://mariadb.com/docs/server/release-notes/mariadb-connector-cpp-1-0/1-0-2/ | 2022-10-11 | General Availability |

# Connection Info

The connection is configured via the information that is initially acquired from the SkySQL Portal pages:

| What to set | Where to find it |
| --- | --- |
| Hostname in the URL | The fully Qualified Domain Name in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| Port number in the URL | The Read-Write Port or Read-Only Port in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| user parameter | The desired username, which might be the default username in the Service Credentials view |
| password parameter | The user's password, which might be the default password in the Service Credentials view if it was not yet customized |
| tlsCert parameter | The path to the skysql_chain.pem file containing the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/#Certificate_Authority_Chain
• https://supplychain.mariadb.com/skysql_chain.pem
• https://supplychain.mariadb.com/aws_skysql_chain.pem |

# Connection URL Syntax

While MariaDB Connector/C++ supports several connection styles, we are going to detail just the JDBC syntax since all connections to SkySQL use a single idiom of hostname, port, user, password, and SSL parameters.

The base URL is specified as follows:

`jdbc:mariadb://example.skysql.com:5001/dbname`

If the trailing database name is left off of the URL, the connection will start without selecting a database.

# Optional Connection Parameters

MariaDB Connector/C++ supports several optional connection parameters. These parameters can be specified using a `Properties` object, as we do in our examples, or appended to the URL in standard `name=value` query-string encoding.

In the following list, we've left out any parameters that aren't pertinent to accessing SkySQL:

| Parameter Name | Description | Type | Default | Aliases |
| --- | --- | --- | --- | --- |
| autoReconnect | Defines whether the connector automatically reconnects after a connection failure. | bool | false | • OPT_RECONNECT |
| connectTimeout | Defines the connect timeout value in milliseconds. When set to 0, there is no connect timeout. | int | 30000 |  |
| enabledTlsCipherSuites | A list of permitted ciphers or cipher suites to use for TLS. | string |  | • enabledSslCipherSuites
• enabledSSLCipherSuites |
| jdbcCompliantTruncation | This mode is enabled by default. This mode configures the connector to add STRICT_TRANS_TABLES to https://mariadb.com/docs/server/ref/mdb/system-variables/sql_mode/https://mariadb.com/docs/server/ref/mdb/system-variables/sql_mode/, which causes ES to handle truncation issues as errors instead of warnings. | bool | true |  |
| password | Defines the password of the user account to connect with. |  |  |  |
| socketTimeout | Defines the network socket timeout (SO_TIMEOUT) in milliseconds. When set to 0, there is no socket timeout. This connection parameter is not intended to set a maximum time for statements. To set a maximum time for statements, please see the https://mariadb.com/docs/server/ref/mdb/system-variables/max_statement_time/https://mariadb.com/docs/server/ref/mdb/system-variables/max_statement_time/https://mariadb.com/docs/server/ref/mdb/system-variables/max_statement_time/ system variable. | int | 0 | • OPT_READ_TIMEOUT |
| tcpRcvBuf | The buffer size for TCP/IP and socket communication. tcpSndBuf changes the same buffer value, and the biggest value of the two is selected. | int | 0x4000 | • tcpSndBuf |
| tcpSndBuf | The buffer size for TCP/IP and socket communication. tcpRcvBuf changes the same buffer value, and the biggest value of the two is selected. | int | 0x4000 | • tcpRcvBuf |
| tlsCert | Path to the X509 certificate file. | string |  | • sslCert |
| tlsCRL | Path to a PEM file that should contain one or more revoked X509 certificates. | string |  | • tlsCrl
• sslCRL |
| useCompression | Compresses network traffic between the client and server. | bool | false | • CLIENT_COMPRESS |
| user | Defines the user name of the user account to connect with. |  |  | • userName |
| useServerPrepStmts | Defines whether the connector uses server-side prepared statements using the https://mariadb.com/docs/skysql-previous-release/ref/mdb/sql-statements/PREPARE/, https://mariadb.com/docs/skysql-previous-release/ref/mdb/sql-statements/EXECUTE/, and https://mariadb.com/docs/skysql-previous-release/ref/mdb/sql-statements/DROP_PREPARE/ statements. By default, the connector uses client-side prepared statements. | bool | false |  |
| useTls | Whether to force TLS. This enables TLS with the default system settings. | bool |  | • useSsl
• useSSL |

# Connection Methods

Two categories of methods are available to to establish a connection.

### **sql::Driver::connect()**

MariaDB Connector/C++ can connect using the non-static `connect()` methods in the `sql::Driver` class.

The non-static `connect()` methods in the `sql::Driver` class have the following prototypes:

- `Connection* connect(const SQLString& url, Properties& props);`
- `Connection* connect(const SQLString& host, const SQLString& user, const SQLString& pwd);`
- `Connection* connect(const Properties& props);`

The non-static `connect()` methods in the `sql::Driver` class:

- Require an instance of the `sql::Driver` class to establish a connection.
- Return `nullptr` as the `Connection*` value when an error occurs, so applications should check the return value before use.

For example:

```cpp
// Instantiate Driver
sql::Driver* driver = sql::mariadb::get_driver_instance();

// Configure Connection, including an optional initial database name "places":
sql::SQLString url("jdbc:mariadb://example.skysql.com:5009/places");

// Use a properties map for the other connection options
sql::Properties properties({
      {"user", "db_user"},
      {"password", "db_user_password"},
      {"autocommit", false},
      {"useTls", true},
      {"tlsCert", "classpath:static/skysql_chain.pem"},
   });

// Establish Connection
// Use a smart pointer for extra safety
std::unique_ptr<sql::Connection> conn(driver->connect(url, properties));

if (!conn) {
   cerr << "Invalid database connection" << endl;
   exit (EXIT_FAILURE);
}
```

### **sql::DriverManager::getConnection()**

MariaDB Connector/C++ can connect using the static `getConnection()` methods in the `sql::DriverManager` class.

The static `getConnection()` methods in the `sql::DriverManager` class have the following prototypes:

- `static Connection* getConnection(const SQLString& url);`
- `static Connection* getConnection(const SQLString& url, Properties& props);`
- `static Connection* getConnection(const SQLString& url, const SQLString& user, const SQLString& pwd);`

The static `getConnection()` methods in the `sql::DriverManager` class:

- Do not require an instance of the `sql::DriverManager` class to establish a connection, because they are static.
- Throw an exception when an error occurs, so applications should use `try { .. } catch ( .. ) { .. }` to catch the exception.

For example:

```cpp
try {
    // Configure Connection, including an optional initial database name "places":
    sql::SQLString url("jdbc:mariadb://example.skysql.com:5009/places");

    // Use a properties map for the other connection options
    sql::Properties properties({
          {"user", "db_user"},
          {"password", "db_user_password"},
          {"autocommit", false},
          {"useTls", true},
          {"tlsCert", "classpath:static/skysql_chain.pem"},
       });

    // Establish Connection
    // Use a smart pointer for extra safety
    std::unique_ptr<sql::Connection> conn(DriverManager::getConnection(url, properties));
 } catch (...) {
    cerr << "Invalid database connection" << endl;
    exit (EXIT_FAILURE);
}
```

# Code Example: Connect

The following code demonstrates how to connect using the [example database and user account](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/cpp/example-setup/):

```cpp
// Includes
#include <iostream>
#include <mariadb/conncpp.hpp>

// Main Process
int main(int argc, char **argv)
{
   try {
      // Instantiate Driver
      sql::Driver* driver = sql::mariadb::get_driver_instance();

      // Configure Connection, including initial database name "test":
      sql::SQLString url("jdbc:mariadb://example.skysql.com:5009/test");

      // Use a properties map for the other connection options
      sql::Properties properties({
            {"user", "db_user"},
            {"password", "db_user_password"},
            {"autocommit", false},
            {"useTls", true},
            {"tlsCert", "classpath:static/skysql_chain.pem"},
         });

      // Establish Connection
      // Use a smart pointer for extra safety
      std::unique_ptr<sql::Connection> conn(driver->connect(url, properties));

      // Use Connection
      // ...

      // Close Connection
      conn->close();
   }

   // Catch Exceptions
   catch (sql::SQLException& e) {
      std::cerr << "Error Connecting to the database: "
         << e.what() << std::endl;

      // Exit (Failed)
      return 1;
   }

   // Exit (Success)
   return 0;
}
```
