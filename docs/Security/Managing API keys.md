# Managing API keys

Users with MariaDB ID accounts are able to request SkySQL API Keys that are used to securely access automated interfaces in MariaDB SkySQL:

- The SkySQL DBaaS API requires a SkySQL API Key to create, delete, and modify database services
- The SkySQL Observability service requires a SkySQL API key to monitor on-premises database services

## **SkySQL API Key Scopes**

Each SkySQL API Key is associated with one or more key scopes that determines what the API key can be used for.

Multiple scopes are currently available:

| Scope | Description |
| --- | --- |
| Observability API: Read | Allows GET requests to the https://mariadb.com/docs/skysql-dbaas/service-management/nr-monitoring/observability/ |
| Observability API: Write | Allows write requests to the https://mariadb.com/docs/skysql-dbaas/service-management/nr-monitoring/observability/ |
| SkySQL API: Databases: Read | Allows GET requests to the https://mariadb.com/docs/skysql-dbaas/nr-quickstart/dbaas-api-launch-walkthrough/ |
| SkySQL API: Databases: Write | Allows write requests to the https://mariadb.com/docs/skysql-dbaas/nr-quickstart/dbaas-api-launch-walkthrough/ |

## **List SkySQL API Keys**

To list your SkySQL API Keys, go to the [API Keys](https://id.mariadb.com/account/api/) page.

## **Generate a SkySQL API Key**

To generate a SkySQL API Key:

1. Go to the [Generate API Key](https://id.mariadb.com/account/api/generate-key) page.
2. Fill out the API key details:
    - In the "Description" field, describe the purpose of the API key.
    - In the "Scopes" field, select one or more [API key scopes](https://mariadb.com/docs/skysql-dbaas/security/nr-api-key/#SkySQL_API_Key_Scopes).
3. Click the "Generate API Key" button.
4. After the page refreshes, click the "Copy to clipboard" button to copy the API key.
5. Paste the API key somewhere safe and do not lose it.

## **Verify a SkySQL API Key**

To verify a SkySQL API Key, use the `keyinfo` endpoint:

- A `GET` request must be sent to the MariaDB ID URL: https://id.mariadb.com/api/v1/keyinfo/
- The `Authorization` header must be in the format `Authorization: Token SKYSQL_API_KEY`, where `SKYSQL_API_KEY` refers to the API key.
- The `Content-length` header must set the content length to `0`.

For example, to verify a SkySQL API Key using cURL with the output piped to `jq` for readability:

```bash
curl --location \
   --header 'Authorization: Token SKYSQL_API_KEY' \
   --header 'Content-length: 0' \
   https://id.mariadb.com/api/v1/keyinfo/ \
   | jq .
```

```json
{
  "jti": "JTI",
  "email": "example_user@example.com",
  "full_name": "Example User",
  "key_prefix": "YpnHA",
  "description": "My new API key",
  "exp": null,
  "iat": "2021-11-10T17:45:00.823041Z",
  "scopes": [
    "skysql::database::write"
  ],
  "iss": "id.mariadb.com",
  "entitlements": [
    "enterprise",
    "xpand",
    "monitoring",
    "skysql"
  ]
}
```

The full set of fields in the output are:

| Field | Description |
| --- | --- |
| entitlements | A list of entitlements this user has access to |
| exp | Time when this API key will expire |
| iat | Time when this API key was issued |
| iss | The server that issued this API key |
| sub | The email address of the user who generated this API key |
| jti | A unique GUID for this API key |
| kid | The key ID used to sign this API key |
| scopes | The https://mariadb.com/docs/skysql-dbaas/security/nr-api-key/#SkySQL_API_Key_Scopes of the API key |

## **Request a Bearer Token**

Most automated interfaces in SkySQL do not directly use the SkySQL API Key directly. Instead, the SkySQL API Key is exchanged for a short-lived bearer token that expires after 1 hour.

To request a short-lived bearer token using a SkySQL API Key, use the `token` endpoint:

- A `POST` request must be sent to the MariaDB ID URL: https://id.mariadb.com/api/v1/token/
- The `Authorization` header must be in the format `Authorization: Token SKYSQL_API_KEY`, where `SKYSQL_API_KEY` refers to the API key.
- The `Content-length` header must set the content length to `0`.

For example, to request a bearer token using cURL with the output piped to `jq` for readability:

```bash
curl --location --request POST \
   --header 'Authorization: Token SKYSQL_API_KEY' \
   --header 'Content-length: 0' \
   https://id.mariadb.com/api/v1/token/ \
   | jq .
```

```json
{
  "jti": "JTI",
  "email": "example_user@example.com",
  "full_name": "Example User",
  "exp": "2021-11-03T23:04:28.834462Z",
  "iat": "2021-11-03T22:04:28.835032Z",
  "scopes": [
    "skysql::database::write"
  ],
  "iss": "id.mariadb.com",
  "parent": "PARENT_ID",
  "token": "SKYSQL_BEARER_TOKEN",
  "entitlements": [
    "enterprise",
    "xpand",
    "monitoring",
    "skysql"
  ],
  "tenant_id": "TENANT_ID"
}
```

In the output, the bearer token is returned as the value of the `"token"` key. This value is used by most automated interfaces in MariaDB SkySQL.

The full set of fields in the output are:

| Field | Description |
| --- | --- |
| entitlements | A list of entitlements this user has access to |
| exp | Time when this bearer token will expire |
| iat | Time when this bearer token was issued |
| iss | The server that issued this bearer token |
| sub | The email address of the user who generated this bearer token |
| jti | A unique GUID for this bearer token |
| kid | The key ID used to sign this bearer token |
| parent | The unique ID of the SkySQL API Key that issued this bearer token |
| scopes | The https://mariadb.com/docs/skysql-dbaas/security/nr-api-key/#SkySQL_API_Key_Scopes of the SkySQL API Key that issued this bearer token |
| tenant_id | The SkySQL Billing ID of the user who generated this bearer token |

## **Expiration**

SkySQL API Keys expire.

The expiration interval is chosen when the API key is generated. SkySQL API Keys can expire at the following intervals:

- 2 years
- 1 year
- 9 months
- 6 months
- 3 months

## **Revocation**

SkySQL API Keys can be revoked.

To revoke a SkySQL API Key:

1. Go to the [API Keys](https://id.mariadb.com/account/api/) page
2. Click the "Revoke" button for the API key