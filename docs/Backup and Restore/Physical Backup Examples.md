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

## Full(physical) Backup Scheduling

#### One-time Full(physical) Backup Example

        curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
        --header 'Content-Type: application/json' \
        --header 'X-API-Key: $API_KEY' \
        --data '{
        "backup_type": "full",
        "schedule": "once",
        "service_id": "$SERVICE_ID"
        }'"

    
- API_KEY : SKYSQL API KEY, see [SkySQL API Keys](https://app.skysql.com/user-profile/api-keys/)
- SERVICE_ID : SkySQL service identifier, format dbtxxxxxx. You can fetch the service ID from the Fully qualified domain name(FQDN) of your service. E.g: in dbpgf17106534.sysp0000.db2.skysql.com, 'dbpgf17106534' is the service ID.You will find the FQDN in the [Connect window](https://app.skysql.com/dashboard) 

#### Cron Full(physical) Example

        curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
        --header 'Content-Type: application/json' \
        --header 'X-API-Key: $API_KEY' \
        --data '{
        "backup_type": "full",
        "schedule": "0 3 * * *",
        "service_id": "$SERVICE_ID"
        }'"

- API_KEY : SKYSQL API KEY, see [SkySQL API Keys](https://app.skysql.com/user-profile/api-keys/)
- SCHEDULE : Cron schedule, see [Cron](https://en.wikipedia.org/wiki/Cron)
- SERVICE_ID : SkySQL service identifier, format dbtxxxxxx

##### Backup status can be fetched using 'https://api.skysql.com/skybackup/v1/backups'. See the 'Backup Status' section for an example.