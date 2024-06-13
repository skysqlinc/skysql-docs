# Connect from Node.js App

Node.js developers can connect to MariaDB database products through a native MariaDB Connector. Using MariaDB Connector/Node.js you can connect to MariaDB database products to use and administer databases from within your Node.js application.

# Install MariaDB Connector/Node.js

MariaDB Connector/Node.js is usually installed either from the Node.js repository or manually from the source code package.

# Install MariaDB Connector/Node.js via Repository

To install MariaDB Connector/Node.js from the Node.js repository, use NPM:

`$ npm install mariadb`

NPM connects to the Node.js repository and downloads MariaDB Connector/Node.js and all relevant dependencies into the `node_modules/` directory.

# Install MariaDB Connector/Node.js via Source Code

To download and install the MariaDB Connector/Node.js manually from source code:

1. Go to the MariaDB Connectors download page:
    - [https://mariadb.com/downloads/#connectors](https://mariadb.com/downloads/#connectors)
2. In the "Product" dropdown, select the Node.js connector.
3. Click the "Download" button to download the source code package
4. When the source code package finishes downloading, install it with NPM:
    
    `$ npm install mariadb-connector-nodejs-*.tar.gz`
    

NPM untars the download and installs MariaDB Connector/Node.js in the `node_modules/` directory.

---

# Connect with MariaDB Connector/Node.js (Callback API)

Node.js developers can use MariaDB Connector/Node.js to establish client connections with MariaDB database products.

# Require Callback API

MariaDB Connector/Node.js provides two different connection implementations: one built on the [Promise API](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/nodejs/promise/) and the other built on the Callback API.

To use the Callback API, use the following module:

**`const** mariadb = require**(**'mariadb/callback'**);**`

# Connect

`createConnection(options) -> Connection` is the base function used to create a `Connection` object.

The `createConnection(options)` function returns a `Connection` object.

Determine the [connection information](https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/) for your MariaDB SkySQL database service:

| Option | Description |
| --- | --- |
| host | The fully Qualified Domain Name in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| port | The Read-Write Port or Read-Only Port in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| user | The desired username, which might be the default username in the Service Credentials view |
| password | The user's password, which might be the default password in the Service Credentials view if it was not yet customized |
| database | Database name to establish a connection to. No default is configured. |
| connectTimeout | Connection timeout in milliseconds. In Connector/Node.js 2.5.6, the default value changed to 1000. The default value for earlier versions is 10000. |
| rowsAsArray | A boolean value to indicate whether to return result sets as array instead of the default JSON. Arrays are comparatively faster. |

# Code Example: Connect

The following code example connects using the database and user account created in the [example setup](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/nodejs/example-setup/):

```jsx
const mariadb = require('mariadb/callback');

// Certificate Authority (CA)",
var serverCert = [fs.readFileSync(process.env.SKYSQL_CA_PEM, "utf8")];

// Declare async function
function main() {
   let conn;

   try {
      conn = mariadb.createConnection({
         host: "example.skysql.com",
         port: 5009,
         ssl: { ca: serverCert },
         user: "db_user",
         password: "db_user_password",
         database: "test",
      });

      // Use Connection
      // ...
   } catch (err) {
      // Manage Errors
      console.log("SQL error in establishing a connection: ", err);
   } finally {
      // Close Connection
      if (conn) conn.end(err => {if(err){
         console.log("SQL error in closing a connection: ", err);}
      });
   }
}

main();
```

- A `try...catch...finally` statement is used for exception handling.
- New connections are created in auto-commit mode by default.
- When you are done with a connection, close it to free resources. Close the connection using the `connection.end([callback])` function.
- The script calls the `connection.end([callback])` function to close/end the connection in the `finally` block after the queries that are running have completed.
- The `end()` function takes a callback function that defines one implicit argument for the `Error` object if thrown in closing the connection as argument. If no error is generated in closing a connection the `Error` object is `null`.

---

# Connect with MariaDB Connector/Node.js (Promise API)

Node.js developers can use MariaDB Connector/Node.js to establish client connections with MariaDB database products.

# Require Promise API

MariaDB Connector/Node.js provides two different connection implementations: one built on the Promise API and the other built on the [Callback API](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/nodejs/callback/). Promise is the default.

To use the Promise API, use the `mariadb` module:

**`const** mariadb = require**(**'mariadb'**);**`

# Connect

`createConnection(options) -> Promise` is the base function used to create a `Connection` object.

The `createConnection(options)` returns a `Promise` that resolves to a `Connection` object if no error occurs, and rejects with an `Error` object if an error occurs.

Determine the [connection information](https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/) for your MariaDB SkySQL database service:

| Option | Description |
| --- | --- |
| host | The fully Qualified Domain Name in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| port | The Read-Write Port or Read-Only Port in the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/ |
| user | The desired username, which might be the default username in the Service Credentials view |
| password | The user's password, which might be the default password in the Service Credentials view if it was not yet customized |
| ssl.ca | The contents of the skysql_chain.pem file containing the https://mariadb.com/docs/skysql-previous-release/connect/connection-parameters-portal/#Certificate_Authority_Chain
• https://supplychain.mariadb.com/skysql_chain.pem
• https://supplychain.mariadb.com/aws_skysql_chain.pem |
| database | Database name to establish a connection to. No default is configured. |
| connectTimeout | Connection timeout in milliseconds. In Connector/Node.js 2.5.6, the default value changed to 1000. The default value for earlier versions is 10000. |
| rowsAsArray | A boolean value to indicate whether to return result sets as array instead of the default JSON. Arrays are comparatively faster. |

Create a file named `.env` to store your database credentials:

```jsx
MDB_HOST = 192.0.2.50
MDB_PORT = 3306
MDB_USER = db_user
MDB_PASS = db_user_password
MDB_HOST = example.skysql.com
MDB_PORT = 5001
MDB_CA_PEM = /path/to/skysql_chain.pem
MDB_USER = db_user
MDB_PASS = db_user_password
```

# Code Example: Connect

The following code example connects using the database and user account created in [Setup for Examples](https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/nodejs/example-setup/):

```jsx
// Required Modules
const fs = require("fs");
const mariadb = require("mariadb");
require("dotenv").config()

// Certificate Authority (CA)
const serverCert = [fs.readFileSync(process.env.MDB_CA_PEM, "utf8")];

// Declare async function
async function main() {
   let conn;

   try {
      conn = await mariadb.createConnection({
         host: process.env.MDB_HOST,
         port: process.env.MDB_PORT,
         user: process.env.MDB_USER,
         password: process.env.MDB_PASS,
         ssl: { ca: serverCert },
         database: "test",
      });

      // Use Connection
      // ...
   } catch (err) {
      // Manage Errors
      console.log("SQL error in establishing a connection: ", err);
   } finally {
      // Close Connection
      if (conn) conn.close();
   }
}

main();
```

- Load the `mariadb` module using the `require()` function.
- Declare an async function called `main()` using the `async` keyword.
- An async function provides asynchronous, Promise-based code behavior.
- Async functions may declare `await` expressions using the `await` keyword.
- Await expressions yield control to a promise-based asynchronous operation.
- Await expressions resume control after the awaited operation is either fulfilled or rejected.
- The return value of an `await` expression is the resolved value of the `Promise`.
- The async function name `main` is arbitrary and does not have special meaning as in some other programming languages.
- Declare a variable called `conn` for the connection to be created using a `let` statement with the async function `main`.
- A `try...catch...finally` statement is used for exception handling.
- New connections are by default created in auto-commit mode.
- In the `try` block, create a new connection using the `mariadb#createConnection(options)` function in the Promise API.
- Send error messages if any to the console in the `catch` block.
- When you are done with a connection, close it to free resources. Close the connection using the `close()` function.

---

| Connector | MariaDB Connector/Node.js |
| --- | --- |
| Supported Versions | https://mariadb.com/docs/server/release-notes/mariadb-connector-nodejs-2-5/https://mariadb.com/docs/server/release-notes/mariadb-connector-nodejs-3-2/ |
| Programming Language | JavaScript |
| Programming Language Version | • Connector/Node.js 2.5: Node.js 16
• Connector/Node.js 3.2: Node.js 16, 18, 20 |
| API | Promise APICallback API |
| Supports TLS | Yes |
| Supports Connection Pools | Yes |
| License | GNU Lesser General Public License v2.1 |
