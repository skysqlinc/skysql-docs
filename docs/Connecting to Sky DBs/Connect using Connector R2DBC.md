# Connect using Connector/R2DBC

Java developers can use MariaDB Connector/R2DBC to connect to SkySQL using the Reactive Relational Database Connectivity (R2DBC) API. R2DBC operations are non-blocking, which makes the R2DBC API more scalable than Java's standard JDBC API. MariaDB Connector/R2DBC is available both with a native R2DBC implementation and the Spring Data R2DBC framework.

| Connector | MariaDB Connector/R2DBC | MariaDB Connector/R2DBC |
| --- | --- | --- |
| Supported Versions | 1.0 | 1.1 |
| Programming Language | Java | Java |
| Programming Language Version | Java 8+ | Java 8+ |
| API | https://r2dbc.io/spec/0.8.5.RELEASE/spec/html/ | https://r2dbc.io/spec/1.0.0.RELEASE/spec/html |
| Supports TLS | Yes | Yes |
| Supports Connection Pools | Yes | Yes |
| License | Apache 2.0 | Apache 2.0 |

# Resources

- [Release notes](https://mariadb.com/docs/server/release-notes/mariadb-connector-r2dbc/)

### **Framework-Specific Documentation**

For details on how to use MariaDB Connector/R2DBC, choose a supported framework:

| https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/java-r2dbc/native/ | The native implementation of R2DBC can be used to connect using MariaDB Connector/R2DBC from within your Java application. |
| --- | --- |
| https://mariadb.com/docs/skysql-previous-release/connect/programming-languages/java-r2dbc/spring/ | Spring Data implementation of R2DBC allows you to connect using MariaDB Connector/R2DBC using the Spring Framework. |
