# Using AWS/GCP private VPC connections

By default, connections to SkySQL cloud databases occur with TLS/SSL encryption and can be initiated only from allowlisted IP addresses.

Some customers have regulatory requirements or information security policies that prohibit database connections over the public internet, and result in a requirement for private connections.

SkySQL cloud databases can optionally be configured for private connections between your VPC (virtual private clouds) and SkySQL cloud databases:

- [AWS PrivateLink](docs/Using AWS GCP private VPC connections/Setting up AWS Private Link.md) is supported for SkySQL cloud databases on AWS
- [Private Service Connect](Setting%20up%20GCP%20Private%20Service%20Connect%2036f6d531c5514999bd464a80ac49919a.md) is supported for SkySQL cloud databases on GCP

