# Launch page

![https://mariadb.com/docs/_images/screenshots/launch-tx-xpand-gcp-choose-instance.png](https://mariadb.com/docs/_images/screenshots/launch-tx-xpand-gcp-choose-instance.png)

*Launch*

https://skysql.mariadb.com/launch-service

While making launch-time selections, your selections and estimated costs are shown at right.

If you require AWS PrivateLink or Google Private Service Connect, please [Contact Support](https://mariadb.com/docs/skysql-dbaas/working/nr-contact-support/).

To launch a SkySQL service from the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/):

1. From the Dashboard, click the "+ Launch New Service" button.
2. Choose the desired [Workload Type](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-workload-type/).
3. Choose the desired [Topology](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-topology/).
4. Choose the desired [Cloud Provider](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-cloud-provider/).
5. Choose the desired [Region](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-region/).
    - Each region has a scheduled maintenance window.
6. Choose the desired [Hardware Architecture](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-hardware-architecture/).
    - Hardware architecture choice is provided on select topologies.
7. Choose the desired [Instance Size](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-instance-size/).
    - If your workload requires a larger instance size, contact us regarding [Power Tier](https://mariadb.com/docs/skysql-dbaas/nr-power-tier/).
    - When launching a service in the Serverless Analytics topology, dedicated capacity is not provisioned so there is no instance size selection.
8. If needed, enable [Auto-Scaling of Nodes](https://mariadb.com/docs/skysql-dbaas/service-management/nr-autonomous/).
    - This feature is provided on select topologies.
9. Choose the desired [Storage Configuration](https://mariadb.com/docs/skysql-dbaas/selections/nr-storage-configuration/).
    - When launching a service in the Serverless Analytics topology, dedicated capacity is not provisioned so there is no storage configuration.
10. If needed, enable [Auto-Scaling of Storage](https://mariadb.com/docs/skysql-dbaas/service-management/nr-autonomous/).
    - This feature is provided on select topologies.
11. Choose number of nodes to deploy.
    - This selection is provided on select topologies.
    - For Enterprise Server With Replica(s) topology, this is [Replica Count](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-replica-count/).
    - For Xpand Distributed SQL topology, this is [Xpand Node Count](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-xpand-node-count/).
12. Choose the desired [Software Version](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-software-version/).
    - This selection is provided on select topologies.
13. Enter the desired Service Name. Service name must align to [Service Name](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-service-name/) rules.
14. Enable topology-specific features, if desired:
    - [Disable SSL/TLS](https://mariadb.com/docs/skysql-dbaas/selections/nr-launch-time-disable-ssltls/)
    - [NoSQL Interface](https://mariadb.com/docs/skysql-dbaas/connect/nr-nosql-interface/)

After initiating service launch, the service will be shown on the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/) Dashboard.

A [notification](https://mariadb.com/docs/skysql-dbaas/working/nr-notifications/) will be sent at time of service launch initiation and when service launch completes.