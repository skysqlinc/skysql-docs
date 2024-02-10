# Portal features

From the SkySQL Portal, you can launch, query, monitor, and manage your SkySQL services.

## Access the Portal

You can access the Portal [here](https://app.skysql.com/dashboard)

## Dashboard

![https://mariadb.com/docs/_images/screenshots/services-tx-xpand-healthy.png](https://mariadb.com/docs/_images/screenshots/services-tx-xpand-healthy.png)


From the Dashboard, you can see a list of your SkySQL services and status information for each service.

From a different view, the Dashboard can be accessed by clicking the "Dashboard" link in the main menu (left navigation in the Portal).

## Launch

To launch a new service, click the "+ Launch New Service" button on the Dashboard.

See "[Service Launch](https://mariadb.com/docs/skysql-dbaas/service-management/nr-launch/)" for details on the service launch process and launch-time selections.

## Service-Specific Interfaces

Service-specific interfaces are available from the Dashboard by clicking on the service name for the desired service.

Service-specific interfaces will vary by topology.

Service-specific interfaces are provided to:

- [Connect](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/#Connect)
- [Manage](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/#Manage)
- [Monitor](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/#Monitoring)
- [Alerts](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/#Alerts)
- [Logs](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/#Logs)
- [Query Editor](https://mariadb.com/docs/skysql-dbaas/connect/nr-query-editor/)
- [Service Details](https://mariadb.com/docs/skysql-dbaas/service-management/nr-service-details/)

## Connect

From the Dashboard, the details needed to connect to your SkySQL service can be seen by clicking on the "CONNECT" button for the desired service.

See "[Client Connections](https://mariadb.com/docs/skysql-dbaas/connect/nr-client-connections/)" for details on how to connect to a service.

## Manage

From the Dashboard, the "MANAGE" button for a service provides access to:

- [Self-Service Operations](https://mariadb.com/docs/skysql-dbaas/service-management/nr-self-service/) to stop/start, delete, or scale your service
- [Security access](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/) to manage the firewall
- [Autonomous](https://mariadb.com/docs/skysql-dbaas/service-management/nr-autonomous/) settings for auto-scale of nodes and auto-scale of storage
- [Apply custom configuration](https://mariadb.com/docs/skysql-dbaas/service-management/nr-configuration-management/)

## Billing

The Dashboard includes a Spending gauge to indicate current charges. More detailed billing information can be accessed by clicking on the Spending gauge.

Alternatively, you can access detailed billing and invoice information by clicking on your name in the upper-right corner of the interface, then select "Billing" from the menu.

See "[Billing](https://mariadb.com/docs/skysql-dbaas/working/nr-billing/)" for additional details.

## Monitoring

The Dashboard includes monitoring gauges for Current SQL Commands, CPU Load, and QPS (Queries Per Second). More detailed monitoring can be accessed by clicking on one of these gauges.

Alternatively, you can access detailed server and service monitoring by clicking on the service name from the Dashboard, then accessing the Monitoring tab (the default view).

See "[Monitoring](https://mariadb.com/docs/skysql-dbaas/service-management/nr-monitoring/)" for additional details.

## Alerts

The Dashboard includes the count of active monitoring alerts for your service. More detailed alert information can be accessed by clicking on the Alerts gauge.

Alternatively, you can access monitoring alerts by clicking the "Alerts" link in the main menu (left navigation in the Portal).

See "[Alerts](https://mariadb.com/docs/skysql-dbaas/service-management/nr-alerts/)" for additional details.

## Logs

Server log files can be accessed by clicking the "Logs" link in the main menu (left navigation in the Portal).

See "[Logs](https://mariadb.com/docs/skysql-dbaas/service-management/nr-logs/)" for additional details.

## Workspace

From the Portal, you can connect to and query your SkySQL databases.

These features can be accessed by clicking the "Workspace" link in the main menu (left navigation in the Portal):

- Queries can be run directly from the browser using the [Query Editor](https://mariadb.com/docs/skysql-dbaas/connect/nr-query-editor/).
- For interactive analytics with built-in visualization and forms, [Serverless Analytics](https://mariadb.com/docs/skysql-dbaas/connect/nr-serverless-analytics/) can be used with [Notebook](https://mariadb.com/docs/skysql-dbaas/connect/nr-notebook/).

## Settings

These settings can be accessed by clicking the "Settings" link in the main menu (left navigation in the Portal):

- [User Management](https://mariadb.com/docs/skysql-dbaas/security/nr-user-management/)
- [Configuration Manager](https://mariadb.com/docs/skysql-dbaas/service-management/nr-configuration-management/)
- [Firewall](https://mariadb.com/docs/skysql-dbaas/security/nr-firewall/)
- [Policies](https://mariadb.com/docs/skysql-dbaas/service-management/nr-alerts/) for monitoring alerts
- [Notification Channels](https://mariadb.com/docs/skysql-dbaas/service-management/nr-alerts/) for delivery of monitoring alerts by email

## Notifications

Actions performed through the Portal will generate a notification.

To view current notifications, click the bell icon in the upper-right corner of the interface.

See "[Notifications](https://mariadb.com/docs/skysql-dbaas/working/nr-notifications/)" for additional details.

## User Preferences

To customize your email notification preferences, click your name in the upper-right corner of the interface, then choose "User preferences".

See "[Notifications](https://mariadb.com/docs/skysql-dbaas/working/nr-notifications/)" for additional details.

## Logout

To log out from SkySQL, click your name in the upper-right corner of the interface, then choose "Logout" from the menu.

## Key Features

!!! Note
    💡 TODO - most of these features are described elsewhere. To be removed later.


| https://mariadb.com/docs/skysql-previous-release/service-management/options/aws-privatelink/ | Connects your AWS VPC network to MariaDB SkySQL with connectivity over Amazon's network |
| --- | --- |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/cross-region-replicas/ | For disaster recovery, data is replicated to a different supported region |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/custom-configuration/ | Changes the default service design and configuration |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/custom-instance-sizes/ | Changes the available instance sizes |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/database-account-2fa/ | Enables two-factor authentication (2FA) for database user accounts |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/database-account-ldap/ | Enables LDAP authentication for database user accounts |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/disable-ssltls/ | Disables default data-in-transit encryption; typically paired with AWS PrivateLink or GCP VPC Peering |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/maxscale-redundancy/ | Enables highly available (HA) active-active load balancers and ability to choose load balancer instance size |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/point-in-time-recovery/ | Configures additional retention of binary logs to enable point-in-time recovery at a later date |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/replication/ | Inbound (to MariaDB SkySQL) and outbound (from MariaDB SkySQL) replication |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/single-zone-deployment/ | Reduced latency for Distributed Transactions |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/skysql-portal-sso/ | Authenticate to SkySQL Portal using SAML 2.0 IDP |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/skysql-teams/ | Multiple SkySQL user accounts jointly maintain services under a single billing profile |
| https://mariadb.com/docs/skysql-previous-release/service-management/options/vpc-peering/ | Connects your GCP VPC network to MariaDB SkySQL with connectivity over Google's network |

[Notifications](Portal%20features%20a48bd73b584b4a9db9b84ca94f958adc/Notifications%2047a5af40b9ee4be4970dfde8a92ef4a2.md)

[Launch page](Portal%20features%20a48bd73b584b4a9db9b84ca94f958adc/Launch%20page%20014bc19a417c4d169df7ae3f80b5f684.md)

[Service Details page](Portal%20features%20a48bd73b584b4a9db9b84ca94f958adc/Service%20Details%20page%20e98710a1c3444f3388548f782ff856f9.md)

[Manage your Service](Portal%20features%20a48bd73b584b4a9db9b84ca94f958adc/Manage%20your%20Service%20a647ad5005f74122bc5fa86fa8d7fde6.md)