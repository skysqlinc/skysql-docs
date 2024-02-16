# REST API Reference

The MariaDB SkySQL DBaaS API is a REST API that can perform operations in MariaDB SkySQL using automation.

!!! Note
    The easiest way to get started with the API is to use [this swagger Docs](https://apidocs.skysql.com). Just generate your API key as described on this page, Click ‘Authorize’ and enter ‘Bearer <yourAPI Key> and try out any of the APIs.**


## **Supported Endpoints**

The supported endpoints are as follows:

| API Path | Method | Summary |
| --- | --- | --- |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_account/ | GET | Returns account record based on X-SkySQL-Org-Id header value |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_invoices/ | GET | Returns list of invoices |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_invoices_slash_ocb_invoice_uid_ccb/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_invoices_slash_ocb_invoice_uid_ccb/ | GET | Returns invoice for specified Chargify invoice uid |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_usage_slash_preview/ | GET | Returns usage allocation summary |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_usage_slash_service/ | GET | Returns usage allocation summary |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_usage_slash_service_slash_ocb_service_id_ccb/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_billing_slash_v1_slash_usage_slash_service_slash_ocb_service_id_ccb/ | GET | Returns usage allocation summary for specified service ID |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_architectures/ | GET | Read Architectures |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs,GET/ | GET | List Custom Configs |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs,POST/ | POST | Create Custom Config |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_,DELETE/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_,DELETE/ | DELETE | Delete Custom Config |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_,GET/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_,GET/ | GET | Read Custom Config |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_slash_values,GET/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_slash_values,GET/ | GET | Get Config Values |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_slash_values,PUT/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_configs_slash_ocb_config_id_ccb_slash_values,PUT/ | PUT | Batch Update Config Values |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_cpu-architectures/ | GET | Read Architectures |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_maintenance-windows/ | GET | Read Maintenance Windows |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_products/ | GET | List Products |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers/ | GET | Read Cloud Providers |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers_slash_ocb_provider_name_ccb_slash_iops/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers_slash_ocb_provider_name_ccb_slash_iops/ | GET | Read Provider IOPS |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers_slash_ocb_provider_name_ccb_slash_volume-types/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers_slash_ocb_provider_name_ccb_slash_volume-types/ | GET | Read Storage Volume Types |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers_slash_ocb_provider_name_ccb_slash_zones/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_providers_slash_ocb_provider_name_ccb_slash_zones/ | GET | Read Availability Zones for a specific provider |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_regions/ | GET | Read Regions |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_regions_slash_ocb_region_name_ccb_slash_zones/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_regions_slash_ocb_region_name_ccb_slash_zones/ | GET | Read Zones |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_service-types/ | GET | Read Service Types |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services,GET/ | GET | List Services |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services,POST/ | POST | Create a Service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_,DELETE/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_,DELETE/ | DELETE | Delete Service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_,GET/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_,GET/ | GET | Retrieve Service Info |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_config,DELETE/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_config,DELETE/ | DELETE | Remove a Custom Config from a Service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_config,POST/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_config,POST/ | POST | Update Service Config |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_endpoints/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_endpoints/ | PATCH | Modifies Service Endpoints |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_maintenance-window/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_maintenance-window/ | POST | Set Service Maintenance Window |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_nodes/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_nodes/ | POST | Update Service Nodes |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_power/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_power/ | POST | Set Service Power State |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_replicas/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_replicas/ | GET | List Replicas for a Service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_replication,DELETE/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_replication,DELETE/ | DELETE | Delete Replication on a Service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_replication,POST/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_replication,POST/ | POST | Update Replication on a Service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,DELETE/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,DELETE/ | DELETE | Remove Allowed Address |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,GET/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,GET/ | GET | Read Allowed Addresses |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,POST/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,POST/ | POST | Add Allowed Address |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,PUT/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_allowlist,PUT/ | PUT | Update Allowed Address |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_credentials/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_credentials/ | GET | Retrieve Default Credentials |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_is-ip-allowed/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_security_slash_is-ip-allowed/ | POST | Check an IP's Access |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_size/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_size/ | POST | Update Service Size |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_start/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_start/ | POST | Starts a stopped service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_stop/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_stop/ | POST | Stops a running service |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_storage/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_storage/ | PATCH | Set Service Storage |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_storage_slash_iops/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_storage_slash_iops/ | PATCH | Set Service Storage |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_storage_slash_size/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_services_slash_ocb_service_id_ccb_slash_storage_slash_size/ | PATCH | Set Service Storage |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_sizes/ | GET | Read Node Sizes |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_tiers/ | GET | Read Tiers |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies/ | GET | Read Topologies |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_configs/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_configs/ | GET | Read Topology Configuration Manager Parameters |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_nodes/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_nodes/ | GET | Read Topology Node Options |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_options/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_options/ | GET | Read Topology Options |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_storage-sizes/https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_topologies_slash_ocb_topology_name_ccb_slash_storage-sizes/ | GET | Read Topology Storage Sizes |
| https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_versions/ | GET | Read Software Versions |