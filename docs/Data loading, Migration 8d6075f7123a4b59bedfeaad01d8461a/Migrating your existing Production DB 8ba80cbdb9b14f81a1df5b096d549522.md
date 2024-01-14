# Migrating your existing Production DB

Databases can be migrated to MariaDB SkySQL from many different database platforms, including Oracle, MySQL, PostgreSQL, Microsoft SQL Server, IBM DB2, and more

# Lift-and-shift from a compatible version of MySQL/MariaDB to SkySQL

To perform a lift-and-shift migration to SkySQL, use the following process:

1. Identify requirements for your MariaDB SkySQL implementation including:
    - [Topology](https://mariadb.com/docs/skysql-previous-release/features-and-concepts/services/)
    - [Instance size](https://mariadb.com/docs/skysql-previous-release/features-and-concepts/selections/instance-sizes/)
    - Storage requirements
    - Desired server version
2. [Deploy the desired configuration](https://mariadb.com/docs/skysql-previous-release/service-management/launch/) on MariaDB SkySQL

<aside>
💡 When migrating from a production DB a common practice is to setup live replication to the SkySQL database.  Here is the sequence of steps you should follow for a live cutover:

</aside>

1. On the source database (MySQL or MariaDB) obtain the Binlog file name and its current position. Essentially, we want to track all DB changes from a certain point in time. See [‘Replicating data’](Migrating%20your%20existing%20Production%20DB%208ba80cbdb9b14f81a1df5b096d549522/Replicating%20data%20from%20external%20DB%20cdf15e1cd8d24880858d6cd2f50d8fd2.md) page for the command. 
2. Next, take a [dump of your source database using mysqldump or mariadb-dump](Import%20data%20from%20external%20DB%209d0a68120e404f36b9f09a5ad71796b7.md) 
3. Import into your target SkySQL DB using this logical “dump” (SQL)
4. Finally, turn ON the replication using the SkySQL ‘start_replication’ procedure as noted below. 

# Assisted Migration to SkySQL MariaDB

SkySQL customers can receive assistance from SkySQL Inc when migrating a database to SkySQL:

- SkySQL Inc's [migration process](https://mariadb.com/docs/skysql-previous-release/migration/assisted/#Migration_Process) divides a migration into multiple well-defined stages with distinct validation criteria at each stage
- SkyDBAs and SkySQL support can assist with many migration steps

For assistance with a migration:

- Existing customers can submit a [support case](https://mariadb.com/docs/skysql-previous-release/service-management/support/) to request assistance with a migration
- New customers can [contact us](https://mariadb.com/docs/skysql-previous-release/contact/) to begin the migration planning process

## Migration Process

We use a multi-step process to assist customers with migrations:

1. **Assessment** of application requirements, inventory, and identified challenges
2. **Schema Migration** including tables, constraints, indexes, and views
3. **Application Code Migration** by porting and testing SQL and application code
4. **Data Migration and Replication** with import of data, with conversion to the new schema, and ongoing inbound replication of new data
5. **Quality Assurance** to assess data validity, data integrity, performance, accuracy of query results, stored code, and running code such as client applications, APIs, and batch jobs
6. **Cutover** including final database preparation, fallback planning, switchover, and decommissioning of old databases

For additional information, see "[Whitepaper: Migrate to MariaDB from Oracle](https://go.mariadb.com/GLBL-WC2020OracleMigration_LP-Registration.html)".

---

# Migrate to MariaDB SkySQL using AWS DMS

This blog article details how to [Migrate RDS MySQL to SkySQL MariaDB Using AWS Data Migration Service](https://go.mariadb.com/21Q3-WC-GLBL-DBaaS-Amazon-RDS-to-SkySQL-Migration-DB1109_LP-Registration.html)

---

# [Replicating data from an External DB](Migrating%20your%20existing%20Production%20DB%208ba80cbdb9b14f81a1df5b096d549522/Replicating%20data%20from%20external%20DB%20cdf15e1cd8d24880858d6cd2f50d8fd2.md)

For assistance with a migration:

- Existing customers can submit a [support case](https://mariadb.com/docs/skysql-previous-release/service-management/support/) to request assistance with a migration
- New customers can [contact us](https://mariadb.com/docs/skysql-previous-release/contact/) to begin the migration planning process

[Replicating data from external DB](Migrating%20your%20existing%20Production%20DB%208ba80cbdb9b14f81a1df5b096d549522/Replicating%20data%20from%20external%20DB%20cdf15e1cd8d24880858d6cd2f50d8fd2.md)