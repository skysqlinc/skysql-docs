# Autonomously scale Compute, Storage

Autonomous features enable automatic scaling in response to changes in workload.

Auto-scale of nodes enables scaling based on load:

- In/Out auto-scaling performs horizontal scaling, decreasing (In) or increasing (Out) the node count.
- Up/Down auto-scaling performs vertical scaling, increasing (Up) or decreasing (Down) the instance size.

Auto-scale of storage enables expansion of capacity based on usage.

Autonomous features can be enabled at time of [service launch](https://mariadb.com/docs/skysql-dbaas/service-management/nr-launch/). Autonomous features can be enabled or disabled after launch.

![https://mariadb.com/docs/_images/screenshots/services-tx-xpand-autonomous-dialog.png](https://mariadb.com/docs/_images/screenshots/services-tx-xpand-autonomous-dialog.png)



## **Enable Auto-Scaling of Nodes**

Auto-scaling of nodes can be enabled either at time of service launch or after service launch.

During [service launch](https://mariadb.com/docs/skysql-dbaas/service-management/nr-launch/):

- Check the "Enable auto-scale nodes" checkbox and set the desired scaling parameters.

After service launch, [manage Autonomous settings](https://mariadb.com/docs/skysql-dbaas/service-management/nr-autonomous/#Manage_Autonomous_Settings), and enable the desired auto-scaling features.

## **Enable Auto-Scaling of Storage**

Auto-scaling of storage can be enabled either at time of service launch or after service launch.

During [Service Launch](https://mariadb.com/docs/skysql-dbaas/service-management/nr-launch/):

- Check the "Enable auto-scale storage" checkbox and set the desired maximum transactional data storage.

After service launch, [manage Autonomous settings](https://mariadb.com/docs/skysql-dbaas/service-management/nr-autonomous/#Manage_Autonomous_Settings), and enable the desired auto-scaling features.

For ColumnStore Data Warehouse, object storage adjusts automatically. Autonomous scaling features for storage are not used by this topology.

## **Manage Autonomous Settings**

To manage Autonomous settings:

- From the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/), click the "MANAGE" button for the desired service, then choose "Autonomous" from the menu.
- Update settings as desired.
- Click "Apply Changes" when complete.

## **Scaling Rules**

Automatic scaling occurs based on rules.

| Policy             | Condition                                                          | Action                                                                                             |
|--------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Auto-Scale Disk   | Disk utilization > 90% sustained for 5 minutes                     | The disk is expected to run out of capacity in the next 24 hours (predicted based on the last 6 hours of service usage). Upgrade storage to the next available size in 100GB increments Note: you cannot downgrade storage, the upgrade is irreversible |
| Auto-Scale Nodes Out | CPU utilization > 75% over all replicas sustained for 30 minutes. <br> Number of concurrent sessions > 90% over all replicas sustained for 1 hour. <br> Number of concurrent sessions is expected to hit the maximum within 4 hours (predicted based on the last 2 hours of service usage) | Add new replica or node Additional nodes will be of the same size and configuration as existing nodes |
| Auto-Scale Nodes In | CPU utilization < 50% over all replicas sustained for 1 hour. <br> Number of concurrent sessions < 50% over all replicas sustained for 1 hour | Remove replica or node Node count will not decrease below the initial count set at launch |
| Auto-Scale Nodes Up | Number of concurrent sessions is expected to hit the maximum within 4 hours (predicted based on the last 2 hours of service usage) | Upgrade all nodes to the next available size |
| Auto-Scale Nodes Down | CPU utilization < 50% over all replicas sustained for 1 hour. <br> Number of concurrent sessions < 50% over all replicas sustained for 1 hour | Downgrade nodes Node size will not decrease below the initial node size set at launch |


Autonomous actions are not instantaneous.

Cooldown periods may apply. A cooldown period is the time period after a scaling operation is completed before another scaling operation can occur. The cooldown period for storage scaling is 6 hours.

[Uptime SLA](Uptime%20SLA%20ea985d9f82fc48b8bc0476cd359f48ce.md)