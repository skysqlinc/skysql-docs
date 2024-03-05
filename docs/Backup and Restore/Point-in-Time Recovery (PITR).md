# Point-in-Time Recovery (PITR)

By default, available backups can be restored with full and complete backup restoration.

Available as a feature for [Power Tier](https://mariadb.com/docs/skysql-dbaas/nr-power-tier/) customers, point-in-time recovery (PITR) can be configured.

When enabled, additional steps are taken to preserve binary logs, enabling point-in-time recovery to be used at a later date. Customers are responsible for additional storage costs.

For Replicated Transactions service, when enabled, point-in-time recovery is available only to points after the last failover.

# Configure for Point-in-Time Recovery

Point-in-time recovery must be configured in advance of need to ensure the required data is available to support point-in-time recovery at a later date.

[Contact SkySQL Support](https://mariadb.com/docs/skysql-dbaas/working/nr-contact-support/) to for point-in-time recovery configuration. This feature is available for Power Tier customers.