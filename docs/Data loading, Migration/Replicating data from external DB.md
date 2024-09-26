i# Replicating data from external DB

# From a MySQL Database

MariaDB SkySQL customers can configure inbound replication from MySQL 5.7 to a compatible MariaDB running in SkySQL.

For additional information about the stored procedures used to configure replication with Replicated Transactions services, see "[SkySQL Replication Helper Procedures for Replicated Transactions](https://mariadb.com/docs/skysql-previous-release/ref/replication-procedures/replicated-transactions/)".

<aside>
💡 To configure inbound replication from an external primary server using MySQL 5.7 to your Replicated Transactions service in SkySQL, the following requirements must be met

GTID (Global Transaction ID) is not supported. Inbound replication must be configured using the binary log file and position.

</aside>

## 1) Obtain Binary Log File and Position

**From you logical dump or mariabackup xtrabackup_binlog_info file**, obtain the binary log file and position from which to start replication.

If source database is completly idle the current binary log file position can be obtained by executing the `SHOW MASTER STATUS` statement:

## 2) Configure Binary Log File and Position

**On the SkySQL service**, configure the binary log file and position from which to start replication.

The binary log file and position can be configured using the `[sky.change_external_primary()` stored procedure](https://mariadb.com/docs/skysql-previous-release/ref/replication-procedures/replicated-transactions/#change_external_primary):

```sql
CALL sky.change_external_primary('mysql1.example.com', 3306, 'mysql-bin.000001', 154, false);

```

######
## This procedure will return the GRANT COMMAND you must run on the source DB
######
## Run_this_grant_on_your_external_primary with data stored in sky.replication_credentials
`GRANT REPLICATION SLAVE ON *.* TO 'skysql_replication'@'%' IDENTIFIED BY '<password_hash>';`


## 3) Start Replication

**On the SkySQL service**, start replication.

Replication can be started using the `[sky.start_replication()` stored procedure](https://skysqlinc.github.io/skysql-docs/Reference%20Guide/Sky%20Stored%20Procedures/#start_replication):

```sql
CALL sky.start_replication();
+----------------------------------------+
| Message                                |
+----------------------------------------+
| External replication running normally. |
+----------------------------------------+
```

## 4) Check Replication Status

**On the SkySQL service**, check replication status.

Replication status can be checked using the `[sky.replication_status()` stored procedure](https://skysqlinc.github.io/skysql-docs/Reference%20Guide/Sky%20Stored%20Procedures/#replication_status):

```sql
**CALL** sky**.**replication_status**()\G**
```

```
*************************** 1. row ***************************
                Slave_IO_State: Waiting for master to send event
                   Master_Host: mariadb1.example.com
                   Master_User: skysql_replication
                   Master_Port: 3306
                 Connect_Retry: 60
               Master_Log_File: mysql-bin.000001
           Read_Master_Log_Pos: 462
                Relay_Log_File: mariadb-relay-bin.000002
                 Relay_Log_Pos: 665
         Relay_Master_Log_File: mysql-bin.000001
              Slave_IO_Running: Yes
             Slave_SQL_Running: Yes
               Replicate_Do_DB:
           Replicate_Ignore_DB:
            Replicate_Do_Table:
        Replicate_Ignore_Table:
       Replicate_Wild_Do_Table:
   Replicate_Wild_Ignore_Table:
                    Last_Errno: 0
                    Last_Error:
                  Skip_Counter: 0
           Exec_Master_Log_Pos: 462
               Relay_Log_Space: 985
               Until_Condition: None
                Until_Log_File:
                 Until_Log_Pos: 0
            Master_SSL_Allowed: No
            Master_SSL_CA_File:
            Master_SSL_CA_Path:
               Master_SSL_Cert:
             Master_SSL_Cipher:
                Master_SSL_Key:
         Seconds_Behind_Master: 0
 Master_SSL_Verify_Server_Cert: No
                 Last_IO_Errno: 0
                 Last_IO_Error:
                Last_SQL_Errno: 0
                Last_SQL_Error:
   Replicate_Ignore_Server_Ids:
              Master_Server_Id: 200
                Master_SSL_Crl:
            Master_SSL_Crlpath:
                    Using_Gtid: No
                   Gtid_IO_Pos:
       Replicate_Do_Domain_Ids:
   Replicate_Ignore_Domain_Ids:
                 Parallel_Mode: conservative
                     SQL_Delay: 0
           SQL_Remaining_Delay: NULL
       Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
              Slave_DDL_Groups: 0
Slave_Non_Transactional_Groups: 0
    Slave_Transactional_Groups: 0

```

# From a MariaDB Database

When replicating from another MariaDB DB you can directly use GTID based replication. The first two steps are different than MySQL.

## 1) Obtain GTID Position

**From you logical dump or mariabackup xtrabackup_binlog_info file**, obtain the binary log file and position from which to start replication.

If source database is completly idle the current binary log file position can be obtained by executing the `select @@current_gtid_pos` statement


## 2) Configure GTID Position

**On the SkySQL service**, configure the GTID position from which to start replication.

The GTID position can be configured using the `[sky.change_external_primary_gtid()` stored procedure](https://skysqlinc.github.io/skysql-docs/Reference%20Guide/Sky%20Stored%20Procedures/#change_external_primary_gtid):

**`CALL** sky**.**change_external_primary_gtid**(**'mariadb1.example.com'**,** 3306**,** '0-100-1'**,** **false);**`

`+--------------------------------------------------------------------------------------------------------------+
| Run_this_grant_on_your_external_primary                                                                      |
+--------------------------------------------------------------------------------------------------------------+
| GRANT REPLICATION SLAVE ON *.* TO 'skysql_replication'@'%' IDENTIFIED BY '<password_hash>';                  |
+--------------------------------------------------------------------------------------------------------------+`

The stored procedure returns a `[GRANT] statement` that is used in the next step.

<aside>
💡 The remaining steps (3) thru (5) are the same as described for MySQL above.

</aside>

## Compatibility

To configure inbound replication from an external primary server using MariaDB Server to your Replicated Transactions service in SkySQL, the following requirements must be met:

- The external primary server must use a supported version of MariaDB Server, and the external primary server must use a version in the same or older release series as the version used by the SkySQL service.
- When the SkySQL service uses **ES 10.6**, the following versions are supported for the external primary server:
    - MariaDB Server 10.2
    - MariaDB Server 10.3
    - MariaDB Server 10.4
    - MariaDB Server 10.5
    - MariaDB Server 10.6
- When the SkySQL service uses **ES 10.5**, the following versions are supported for the external primary server:
    - MariaDB Server 10.2
    - MariaDB Server 10.3
    - MariaDB Server 10.4
    - MariaDB Server 10.5
- When the SkySQL service uses **ES 10.4**, the following versions are supported for the external primary server:
    - MariaDB Server 10.2
    - MariaDB Server 10.3
    - MariaDB Server 10.4

