# Connect from Java App

MariaDB Connector/J enables Java applications to connect to MariaDB database products using a native MariaDB connector.

# Download the connector ..

| Version | Latest Release | Latest Release Date | Maturity |
| --- | --- | --- | --- |
| MariaDB Connector/J 3.1 | https://mariadb.com/docs/server/release-notes/mariadb-connector-j-3-1/3-1-4/ | 2023-05-01 | General Availability |
| MariaDB Connector/J 3.0 | https://mariadb.com/docs/server/release-notes/mariadb-connector-j-3-0/3-0-10/ | 2023-01-11 | General Availability |
| MariaDB Connector/J 2.7 | https://mariadb.com/docs/server/release-notes/mariadb-connector-j-2-7/2-7-9/ | 2023-03-22 | General Availability |
| MariaDB Connector/J 1.8 | MariaDB Connector/J 1.8.0 | 2019-02-11 | GA |

# Install MariaDB Connector/J via JAR

To download the JAR file manually:

1. Go to the [MariaDB Connector/J download page](https://mariadb.com/downloads/connectors/connectors-data-access/java8-connector/)
2. Within the "Product" dropdown, choose the "Java 8 connector" or "Java 7 connector".
3. In the "Version" dropdown, choose the desired version.
4. Click the "Download" button to download the JAR file.
5. When the JAR file finishes downloading, place it into the relevant directory on your system.
6. Similarly, install dependency JAR files, if any used.

# Install MariaDB Connector/J via Maven

Maven can install MariaDB Connector/J as a dependency of your application during build. Set the `<version>` element to correspond to the version of MariaDB Connector/J that you would like to install.

To use Maven to install MariaDB Connector/J, add the dependency to your `pom.xml` file:

```xml
<dependency>
   <groupId>org.mariadb.jdbc</groupId>
   <artifactId>mariadb-java-client</artifactId>
   <version>3.0.10</version>
</dependency>
```

For additional information on available releases, see the "[Release Notes for MariaDB Connector/J](https://mariadb.com/docs/server/release-notes/mariadb-connector-j-3-1/)".

Depending on the features you plan to use, you may need to add some additional dependencies to `pom.xml`.

If you downloaded the connector JAR, place it on your CLASSPATH

```bash
export CLASSPATH="/path/to/application:/path/to/mariadb-java-client-3.0.10.jar"
```

### **Connector/J 3.0**

In MariaDB Connector/J 3.0, TLS is enabled for connections to SkySQL using the `sslMode` parameter.

```java
import java.sql.*;
import java.util.Properties;

public class App {
    public static void main(String[] argv) {
        Properties connConfig = new Properties();
        connConfig.setProperty("user", "db_user");
        connConfig.setProperty("password", "db_user_password");
        **connConfig.setProperty("sslMode", "verify-full");**
        connConfig.setProperty("serverSslCert", "/path/to/skysql_chain.pem");

        try (Connection conn = DriverManager.getConnection("jdbc:mariadb://HOST:PORT", connConfig)) {
            try (Statement stmt = conn.createStatement()) {
                try (ResultSet contact_list = stmt.executeQuery("SELECT first_name, last_name, email FROM test.contacts")) {
                    while (contact_list.next()) {
                        System.out.println(String.format("%s %s <%s>",
                            contact_list.getString("first_name"),
                            contact_list.getString("last_name"),
                            contact_list.getString("email")));
                    }
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### **Connector/J 2.7**

In MariaDB Connector/J 2.7 and before, TLS is enabled for connections to SkySQL using the `useSsl` parameter.

```java
import java.sql.*;
import java.util.Properties;

public class App {
    public static void main(String[] argv) {
        Properties connConfig = new Properties();
        connConfig.setProperty("user", "db_user");
        connConfig.setProperty("password", "db_user_password");
        **connConfig.setProperty("useSsl", "true");**
        connConfig.setProperty("serverSslCert", "/path/to/skysql_chain.pem");

        try (Connection conn = DriverManager.getConnection("jdbc:mariadb://HOST:PORT", connConfig)) {
            try (Statement stmt = conn.createStatement()) {
                try (ResultSet contact_list = stmt.executeQuery("SELECT first_name, last_name, email FROM test.contacts")) {
                    while (contact_list.next()) {
                        System.out.println(String.format("%s %s <%s>",
                            contact_list.getString("first_name"),
                            contact_list.getString("last_name"),
                            contact_list.getString("email")));
                    }
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

| Connector | MariaDB Connector/J |
| --- | --- |
| Supported Versions | https://mariadb.com/docs/server/release-notes/mariadb-connector-j-3-1/https://mariadb.com/docs/server/release-notes/mariadb-connector-j-3-0/https://mariadb.com/docs/server/release-notes/mariadb-connector-j-2-7/MariaDB Connector/J 1.8 |
| Programming Language | Java |
| Programming Language Version | Java 17, Java 11, Java 8 (Connector/J 3.1)Java 17, Java 11, Java 8 (Connector/J 3.0)Java 17, Java 11, Java 8 (Connector/J 2.7)Java 7 (Connector/J 1.8) |
| API | JDBC 4.2 (Connector/J 3.1)JDBC 4.2 (Connector/J 3.0)JDBC 4.2 (Connector/J 2.7)JDBC 4.1 (Connector/J 1.8) |
| Supports TLS | Yes |
| License | GNU Lesser General Public License v2.1 |