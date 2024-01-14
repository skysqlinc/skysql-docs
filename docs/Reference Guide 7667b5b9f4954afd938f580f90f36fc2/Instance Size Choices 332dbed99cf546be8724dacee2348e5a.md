# Instance Size Choices

Instance size choices are specific to the [cloud provider](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/providers/), [topology](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/topologies/), [region](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/regions/), and [hardware architecture](https://mariadb.com/docs/skysql-dbaas/ref/skynr/selections/architectures/).

<aside>
💡 the list below provides available sizes as of Jan 2024. Likely to evolve over time. The [SkySQL portal](http://app.skysql.com) is the best place for accurate information.

</aside>

### MariaDB Server

For Foundation tier:

| Instance Size | Cloud Provider | CPU | Memory |
| --- | --- | --- | --- |
| sky-2x4 | aws | 2 vCPU | 4 GB |
| sky-2x8 | aws | 2 vCPU | 8 GB |
| sky-2x8 | gcp | 2 vCPU | 8 GB |
| sky-4x16 | aws | 4 vCPU | 16 GB |
| sky-4x16 | gcp | 4 vCPU | 16 GB |
| sky-4x32 | aws | 4 vCPU | 32 GB |
| sky-4x32 | gcp | 4 vCPU | 32 GB |
| sky-8x32 | aws | 8 vCPU | 32 GB |
| sky-8x32 | gcp | 8 vCPU | 32 GB |
| sky-8x64 | aws | 8 vCPU | 64 GB |
| sky-8x64 | gcp | 8 vCPU | 64 GB |

For Power tier:

| Instance Size | Cloud Provider | CPU | Memory |
| --- | --- | --- | --- |
| sky-2x4 | aws | 2 vCPU | 4 GB |
| sky-2x8 | aws | 2 vCPU | 8 GB |
| sky-2x8 | gcp | 2 vCPU | 8 GB |
| sky-4x16 | aws | 4 vCPU | 16 GB |
| sky-4x16 | gcp | 4 vCPU | 16 GB |
| sky-4x32 | aws | 4 vCPU | 32 GB |
| sky-4x32 | gcp | 4 vCPU | 32 GB |
| sky-8x32 | aws | 8 vCPU | 32 GB |
| sky-8x32 | gcp | 8 vCPU | 32 GB |
| sky-8x64 | aws | 8 vCPU | 64 GB |
| sky-16x64 | aws | 16 vCPU | 64 GB |
| sky-16x64 | gcp | 16 vCPU | 64 GB |
| sky-16x128 | aws | 16 vCPU | 128 GB |
| sky-16x128 | gcp | 16 vCPU | 128 GB |
| sky-32x128 | aws | 32 vCPU | 128 GB |
| sky-32x128 | gcp | 32 vCPU | 128 GB |
| sky-32x256 | aws | 32 vCPU | 256 GB |
| sky-32x256 | gcp | 32 vCPU | 256 GB |
| sky-64x256 | aws | 64 vCPU | 256 GB |
| sky-64x256 | gcp | 64 vCPU | 256 GB |
| sky-64x512 | aws | 64 vCPU | 512 GB |
| sky-64x512 | gcp | 64 vCPU | 512 GB |

### **MaxScale**

With Power tier, the following instance sizes can be selected for MaxScale nodes:

| Instance Size | Cloud Provider | CPU | Memory |
| --- | --- | --- | --- |
| sky-2x4 | aws | 2 vCPU | 4 GB |
| sky-2x8 | gcp | 2 vCPU | 8 GB |
| sky-4x16 | aws | 4 vCPU | 16 GB |
| sky-4x16 | gcp | 4 vCPU | 16 GB |
| sky-8x32 | aws | 8 vCPU | 32 GB |
| sky-8x32 | gcp | 8 vCPU | 32 GB |
| sky-16x64 | aws | 16 vCPU | 64 GB |
| sky-16x64 | gcp | 16 vCPU | 64 GB |
| sky-32x128 | aws | 32 vCPU | 128 GB |
| sky-32x128 | gcp | 32 vCPU | 128 GB |
| sky-64x256 | aws | 64 vCPU | 256 GB |
| sky-64x256 | gcp | 64 vCPU | 256 GB |

## REST Client

A REST client can use the SkySQL DBaaS API to query instance size selections and choose an instance size for a new service.

### **Query Database Node Options with REST Client**

A REST client can query the SkySQL DBaaS API for the database node instance size selections for a specific cloud provider, architecture, and topology.

To see the available database node instance sizes for a topology, use `curl` to call the `[/provisioning/v1/sizes` API endpoint](https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_sizes/) with `type=server` set:

```bash
curl -sS --location \
   --header "Authorization: Bearer ${SKYSQL_API_KEY}" \
   'https://api.mariadb.com/provisioning/v1/sizes?architecture=amd64&service_type=transactional&provider=gcp&topology=es-replica&type=server' \
   | jq .
```

```json
[
  {
    "id": "37629543-65d2-11ed-8da6-2228d0ae81af",
    "name": "sky-2x8",
    "display_name": "Sky-2x8",
    "service_type": "transactional",
    "provider": "gcp",
    "tier": "foundation",
    "architecture": "amd64",
    "cpu": "2 vCPU",
    "ram": "8 GB",
    "type": "server",
    "default_maxscale_size_name": "sky-2x8",
    "updated_on": "2022-11-16T17:15:06Z",
    "created_on": "2022-11-16T17:15:06Z",
    "is_active": true,
    "topology": "es-replica"
  },
  {
    "id": "37629489-65d2-11ed-8da6-2228d0ae81af",
    "name": "sky-4x16",
    "display_name": "Sky-4x16",
    "service_type": "transactional",
    "provider": "gcp",
    "tier": "foundation",
    "architecture": "amd64",
    "cpu": "4 vCPU",
    "ram": "16 GB",
    "type": "server",
    "default_maxscale_size_name": "sky-2x8",
    "updated_on": "2022-11-16T17:15:06Z",
    "created_on": "2022-11-16T17:15:06Z",
    "is_active": true,
    "topology": "es-replica"
  },
....

]
```

### **Query MaxScale Node Options with REST Client**

A REST client can query the SkySQL DBaaS API for the MaxScale node instance size selections for a specific cloud provider, architecture, and topology.

To see the default MaxScale instance size for a topology, cloud, and architecture, use `curl` to call the `[/provisioning/v1/sizes` API endpoint](https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_sizes/):

```bash
curl -sS --location \
   --header "Authorization: Bearer ${SKYSQL_API_KEY}" \
   'https://api.mariadb.com/provisioning/v1/sizes?provider=gcp&architecture=amd64&topology=es-replica' \
   | jq .
```

```json
[
   {
     "id": "c0666ab8-4a3b-11ed-8853-b278760e6ab5",
     "name": "sky-2x8",
     "display_name": "Sky-2x8",
     "service_type": "transactional",
     "provider": "gcp",
     "tier": "foundation",
     "architecture": "amd64",
     "cpu": "2 vCPU",
     "ram": "8 GB",
     "type": "server",
     "default_maxscale_size_name": "sky-2x8",
     "updated_on": "2022-10-12T14:40:00Z",
     "created_on": "2022-10-12T14:40:00Z",
     "is_active": true,
     "topology": "es-replica"
   }
]
```

The `default_maxscale_size_name` attribute shows the default MaxScale instance size.

To see the available MaxScale node instance sizes for a topology, use `curl` to call the `[/provisioning/v1/sizes` API endpoint](https://mariadb.com/docs/skysql-dbaas/ref/skynr/api/slash_provisioning_slash_v1_slash_sizes/) with `type=proxy` set:

```bash
curl -sS --location \
   --header "Authorization: Bearer ${SKYSQL_API_KEY}" \
   'https://api.mariadb.com/provisioning/v1/sizes?architecture=amd64&service_type=transactional&provider=gcp&topology=es-replica&type=proxy' \
   | jq .
```

The output can show different instance sizes, depending on whether your SkySQL account is Foundation tier or Power tier.