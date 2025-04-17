# Backup Support

## **MariaDB Server Versions and Backup Support**

| Server Version | Full Backup | Incremental Backup | Dump(mariadb-dump) Backup | Snapshot Backup |
| --- | --- | --- | --- | --- |
| 10.5.25 | ✓ | ✓ | ✓ | ✓ |
| 10.6.20 | ✓ | ✓ | ✓ | ✓ |
| 10.11.11 | ✓ | ✓ | ✓ | ✓ |
| 11.4.5 | ✓ | ✓ | ✓ | ✓ |
| 11.6.2 (Vector Preview) | ✗ | ✗ | ✗ | ✓ |
| 11.7.1 (Release Candidate) | ✗ | ✗ | ✗ | ✓ |

### Notes:
- Versions 11.6.2 and 11.7.1 support only snapshot backups
- All other versions support all backup types: Full, Incremental, Dump, and Snapshot

Please [contact us](../Support.md) if you have any questions about backup support for specific MariaDB versions. 