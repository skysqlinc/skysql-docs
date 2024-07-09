
## Authentication

To authenticate with the API, do the following:

1. Go to SkySQL API Key management page: https://app.skysql.com/user-profile/api-keys and generate an API key

2. Export the value from the token field to an environment variable $API_KEY

  ```bash
  export API_KEY='... key data ...'
  ```
3. Use it on subsequent request, e.g:

   ```bash
    curl --request GET 'https://api.skysql.com/skybackup/v1/backups/schedules' \\
    --header "X-API-Key: ${API_KEY}"
   ```
## Snapshot Backup Scheduling



1. Go to SkySQL API Key management page: https://app.skysql.com/user-profile/api-keys and generate an API key

2. Export the value from the token field to an environment variable $API_KEY
    
  ```bash
  export API_KEY='... key data ...'
  ```
    
  The `API_KEY` environment variable will be used in the subsequent steps.

3. Use it on subsequent request, e.g:
  ```bash 
  curl --request GET 'https://api.skysql.com/provisioning/v1/services' \\
  --header "X-API-Key: $API_KEY"
  ```

#### One-time Snapshot Example

        curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules' \
        --header 'Content-Type: application/json' \
        --header 'Accept: application/json' \
        --header "X-API-Key: $API_KEY" \
        --data "{
            \"backup_type\": \"snapshot\",
            \"schedule\": \"once\",
            \"service_id\": \"$SERVICE_ID\"
            }"

    
- API_KEY : SKYSQL API KEY, see [SkySQL API Keys](https://app.skysql.com/user-profile/api-keys/)
- SERVICE_ID : SkySQL serivce identifier, format dbtxxxxxx

#### Cron Snapshot Example


        curl --location 'https://api.skysql.com/skybackup/v1/backups/schedules'
        --header 'Content-Type: application/json' \
        --header 'Accept: application/json' \
        --header "X-API-Key: $API_KEY" \
        --data "{
        \"backup_type\": \"snapshot\",
        \"schedule\": \"0 3 * * *\",
        \"service_id\": \"$SERVICE_ID\"
        }"

- API_KEY : SKYSQL API KEY, see [SkySQL API Keys](https://app.skysql.com/user-profile/api-keys/)
- SCHEDULE : Cron schedule, see [Cron](https://en.wikipedia.org/wiki/Cron)
- SERVICE_ID : SkySQL serivce identifier, format dbtxxxxxx