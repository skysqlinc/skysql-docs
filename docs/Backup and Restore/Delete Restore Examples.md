details>
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
In order to delete an already scheduled Restore, users  need to make the following API call:


```bash
curl --location --request DELETE 'https://api.skysql.com/skybackup/v1/restores/12' \
--header 'Accept: application/json' \
--header 'X-API-Key: ${API_KEY}'
```