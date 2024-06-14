# Connect from ‘C’ App

MariaDB Connector/C enables C and C++ applications to establish client connections to SkySQL over TLS. MariaDB Connector/C is a native connector that is written in C.

## First [Install MariaDB Connector/C](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/c/install/)

MariaDB Connector/C enables C and C++ applications to establish client connections to SkySQL and MariaDB database products over TLS.

Additional information on MariaDB Connector/C is available in the [MariaDB Knowledge Base](https://mariadb.com/kb/en/mariadb-connector-c/).

# Connection Info

The connection is configured via the information that is initially acquired from the SkySQL Portal pages:

| Function | Option/Argument | Where to find it |
| --- | --- | --- |
| mysql_optionsv() | MYSQL_OPT_SSL_CA option | The path to the skysql_chain.pem file containing the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/#Certificate_Authority_Chain
• https://supplychain.mariadb.com/skysql_chain.pem
• https://supplychain.mariadb.com/aws_skysql_chain.pem |
| mysql_real_connect() | host argument | The fully Qualified Domain Name in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| mysql_real_connect() | user argument | The desired username, which might be the default username in the Service Credentials view |
| mysql_real_connect() | passwd argument | The user's password, which might be the default password in the Service Credentials view if it was not yet customized |
| mysql_real_connect() | port argument | The Read-Write Port or Read-Only Port in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |

# Code Example

The following code demonstrates how to use MariaDB Connector/C to connect to SkySQL. This example uses the [example database and user account](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/c/example-setup/):

```c
#include <stdio.h>
#include <stdlib.h>
#include <mysql.h>

int main (int argc, char* argv[])
{

   // Initialize Connection
   MYSQL *conn;
   if (!(conn = mysql_init(0)))
   {
      fprintf(stderr, "unable to initialize connection struct\n");
      exit(1);
   }

   // Connect to the database
   if (!mysql_real_connect(
         conn,                 // Connection
         "example.skysql.com", // Host
         "db_user",            // User account
         "db_user_password",   // User password
         "test",               // Default database
         3006,                 // Port number
         NULL,                 // Path to socket file
         0                     // Additional options
      ))
   {
      // Report the failed-connection error & close the handle
      fprintf(stderr, "Error connecting to Server: %s\n", mysql_error(conn));
      mysql_close(conn);
      exit(1);
   }

   // Use the Connection
   // ...

   // Close the Connection
   mysql_close(conn);

   return 0;
}
```
