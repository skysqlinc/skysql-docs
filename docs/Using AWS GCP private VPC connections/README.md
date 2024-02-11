# Using AWS/GCP private VPC connections

By default, connections to SkySQL cloud databases occur with TLS/SSL encryption and can be initiated only from allowlisted IP addresses.

Some customers have regulatory requirements or information security policies that prohibit database connections over the public internet, and result in a requirement for private connections.

SkySQL cloud databases can optionally be configured for private connections between your VPC (virtual private clouds) and SkySQL cloud databases:

- [AWS PrivateLink](https://skysqlinc.github.io/skysql-docs/Using%20AWS%20GCP%20private%20VPC%20connections/Setting%20up%20AWS%20Private%20Link/) is supported for SkySQL cloud databases on AWS
- [Private Service Connect](Setting%20up%20GCP%20Private%20Service%20Connect%2036f6d531c5514999bd464a80ac49919a.md) is supported for SkySQL cloud databases on GCP

