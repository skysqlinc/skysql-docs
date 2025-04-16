# Point-in-Time Restore

<details>
<summary>
Authentication
</summary>
<h3>
<ol>
<li>
Go to the SkySQL <a href="https://app.skysql.com/user-profile/api-keys">API Key management page</a>  and generate an API key
</li>
<li>
Export the value from the token field to an environment variable $API_KEY

```
export API_KEY='... key data ...'
```
</li>
<li>
Use it on subsequent request, e.g:

```bash
curl --request GET 'https://api.skysql.com/skybackup/v1/backups/schedules' --header "X-API-Key: ${API_KEY}"
```
</li>
</ol>
</details>

### Important Note

For Point-in-Time Restore to work, you must have a pre-configured backup schedule that ensures:

* Your backup schedule creates snapshot backups with a time gap shorter than your `expire_logs_days` database configuration setting (required for binary log availability)
* Your selected restore point must be between two consecutive snapshot backups from this schedule
* By default, SkySQL sets `expire_logs_days` to 4 days, but you can configure this value to match your backup schedule requirements

## Usage Examples

### API Example

```bash
curl --location 'https://api.skysql.com/skybackup/v1/restores' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header "X-API-Key: ${API_KEY}" \
--data '{
  "service_id": "<SERVICE_ID>",
  "id":"<BACKUP_SOURCE_SERVICE_ID>",
  "point_in_time":"<RESTORE_POINT_IN_TIME, UTC, FORMAT: YYYY-MM-DD HH:MM:SS>"
}'
```

- API_KEY : SKYSQL API KEY, see [SkySQL API Keys](https://app.skysql.com/user-profile/api-keys/)
- SERVICE_ID : SkySQL service identifier, format dbtxxxxxx. This is your restore target service
- BACKUP_SOURCE_SERVICE_ID: SkySQL service identifier, format dbtxxxxxx. This is your backup source service id
- You can fetch the SkySQL service identifier from the Fully Qualified Domain Name (FQDN) of your service. For example: in dbpgf17106534.sysp0000.db2.skysql.com, 'dbpgf17106534' is the service ID. You will find the FQDN in the [Connect window](https://app.skysql.com/dashboard)

### SkySQL Portal Example

To perform a Point-in-Time Restore through the SkySQL Portal:

<ol>
  <li>Navigate to Backups→Restores</li>
  <li>Click the "Point-in-Time Restore" Button</li>
  <li>In the restore form, provide:
    <ol>
      <li>Database restore target service</li>
      <li>Backup source service</li>
      <li>Selected restoration point in time</li>
    </ol>
  </li>
  <li>Click the "Restore" button to start the restore process</li>
</ol>

## Limitations

- Cross-cloud restore is not supported. Your restore target service must be in the same cloud provider as your backup source service.
- Only SkySQL native snapshots can be used as restore source. External backups are not supported for Point-in-Time Restore.
- Point-in-Time Restore requires [MariaDB 10.8](https://mariadb.com/kb/en/changes-improvements-in-mariadb-108/#mysqlbinlog-gtid-support) or later, which introduced the binary log search functionality needed for this feature.
- Support for Serverless databases as Point-in-Time Restore sources is coming soon.
