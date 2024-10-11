# **Introduction to SkySQL**


MariaDB stands as one of the world's leading open-source relational databases, renowned for its maturity and widespread adoption. Born as a fork of MySQL, MariaDB maintains high compatibility with its predecessor while offering enhanced features and performance.

SkySQL is a multi-cloud, fully managed Database-as-a-Service (DBaaS), designed to orchestrate MariaDB across diverse environments including multiple data centers, regions, and cloud providers. 

Originally developed by [MariaDB](http://mariadb.com), SkySQL aimed to be the most comprehensive cloud platform for MariaDB. Its robust feature set is the result of years of insights gathered from hundreds of customers running mission-critical workloads. In late 2023, the core team behind SkySQL formed an independent company, named **SkySQL**, to further advance the platform.

SkySQL elevates MariaDB to enterprise-grade with production-ready features:

    . Automated complex database configurations
    . Cloud-native capabilities including VPC integration and auto-scaling
    . Global replication
    . Automated backups
    . Advanced security with end-to-end encryption
    . Compliance and governance
    . And numerous other powerful features.

[![architecture](architecture.png)](architecture.png)

## **Key Features of SkySQL**

### **Global Multi-Cloud Presence**
Available in 30+ regions across AWS, GCP, and Azure, offering unparalleled flexibility and reach.

### **Robust High Availability (HA)**
- **Resilient Infrastructure:** Multi-layered protection across disks, compute, zones/regions, network, and load balancing.
- **Intelligent Proxy:** Ensures seamless failover and monitors replication lag, maintaining service continuity.
- **Rapid Failover:** Employs real-time health checks for swift, automated recovery.

### **Advanced Scalability**
- **Intelligent Load Balancing:** Maintains consistency via smart proxy-based load balancing and read-write splitting.
- **Flexible Consistency:** Offers both causal and strong global consistency models.
- **Custom Query Routing:** Allows fine-tuned performance optimization through customizable routing rules.

### **Disaster Recovery**
- **Multi-Region Replication:** Spans across regions and cloud providers for maximum resilience.
- **Flexible External Replication:** Supports custom external replica setups for enhanced redundancy and disaster recovery.

### **State-of-the-Art Data Protection**
- **Versatile Backup Solutions:** Goes beyond nightly backups with incremental, snapshot, and binlog backup options to both SkySQL and customer-managed storage.
- **Near-Zero RPO:** Achieves minimal data loss with continuous binlog streaming.

### **Dynamic Auto-scaling**
- **Predictive Scaling:** Anticipates and adjusts storage needs based on usage patterns.
- **Cost-Efficient Scaling:** Proactively scales resources up and down to match demand and optimize costs.

### **Fractional DBA Service**
- **SkyDBA Service:** Provides access to skilled DBAs for proactive database optimization.
- **Continuous Performance Monitoring:** Ensures peak efficiency through ongoing monitoring and rapid issue resolution.

### **Versatile Interoperability**
- **Multi-Protocol Support:** Supports both JSON and SQL operations from a single data source.
- **Native MongoDB/MariaDB/MySQL Support:** Seamlessly integrates with existing applications via native MongoDB and MariaDB/MySQL protocol support.


For more detailed information, you can refer to the original blog on the [SkySQL website](https://skysql.com/2024/03/12/optimizing-database-resilience-and-cost-a-deep-dive-into-skysqls-unique-features/).
