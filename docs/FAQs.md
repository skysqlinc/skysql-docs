# FAQs

## About

### **What is the history of SkySQL?**

SkySQL is a database-as-a-service (DBaaS) that was originally developed and managed by MariaDB Corporation. The cloud division (SkySQL) was later spun out of MariaDB into a independent company -  [SkySQL Inc](https://skysql.com).  The team that developed SkySQL transitioned over to the new company.

### **How do I sign up for SkySQL?**

1. Go to [https://app.skysql.com](https://app.skysql.com/)
2. Log in or register using social login or email.
3. Once registered, you can get started right away by [launching a service](../Quickstart), [connecting](<../Connecting to Sky DBs>), and [loading data](<../Data loading, Migration>).

### **Why would I choose a DBaaS over an on-premises deployment?**

Our platform and experts handle the infrastructure needs, allowing you to focus on your core business.

When you choose SkySQL, a full-featured DBaaS, you forego the capital expenditure of buying hardware, the delay of waiting for new systems to ship every time you need to scale-up or scale-out, and the overhead and opportunity-cost of tuning, monitoring, and upgrading your database. SkySQL also handles routine tasks such as nightly backups and infrastructure maintenance.

And if you need ultimate control, and have the necessary skills and resources to hand-pick instances and tune configurations, we offer SkySQL [Power Tier](<../Billing and Power Tier>) to deliver all of the on-premises benefits without the capex (capital expense) and operational overhead.

### **How long do deployments on SkySQL take?**

In Foundation Tier smaller databases launch in 2-4 mins. Power Tier deployments with isolated, dedicated Kubernetes environments can take up to 25 minutes. Subsequent database deployments will use the same Kubernetes environment and usually launch in 2-4 minutes.

The real time benefits come every day after, when you're operating at scale. A failed database node can recover in a matter of seconds using Kubernetes self-healing, or instantly failover to an alternate replicated server. Instead of an eight-hour bare metal rebuild as you might see on-premises or on other cloud platforms.

### **What version of MariaDB does SkySQL support?**

SkySQL provides services backed by:

- MariaDB Community Server 11.4
- MariaDB Community Server 10.11
- MariaDB Enterprise Server 10.6
- MariaDB Enterprise Server 10.5
- MariaDB MaxScale

### **Are other databases supported?**

No, SkySQL is dedicated to being the top choice for MariaDB. Our goal is to provide the best price-performance of any DBaaS, offer significant productivity improvements through automation, and serve as the most comprehensive end-to-end platform for all your database needs.

We offer “fractional DBAs” - expert-maintained multi-cloud databases. We're glad to help with your move to SkySQL, whether you're [migrating](<../Data loading, Migration>) from another database platform or looking to [lift-and-shift](<../Data loading, Migration>) a MariaDB implementation to SkySQL.

### **What hardware does SkySQL run on?**

SkySQL is multi-cloud and as a full-featured DBaaS we handle all of the hardware and infrastructure needs.

Services are currently available with a range of [instance sizes](<../Reference Guide/Instance Size Choices>) running on the following cloud service providers:

- Amazon AWS (Amazon Web Services)
- Google GCP (Google Cloud Platform)
- Microsoft Azure

Transactional services (such as our Replicated Transactions topology) operate on:

- AWS with [EBS Provisioned SSD (gp2, gp3, io1 type)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html) with [Amazon EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html)
- GCP with [Zonal SSD Persistent Disk](https://cloud.google.com/compute/docs/disks/performance) with [Google's default encryption](https://cloud.google.com/security/encryption-at-rest/default-encryption)
- Azure with [Azure Standard and Premium Disks](https://docs.microsoft.com/en-us/azure/virtual-machines/managed-disks-overview) with [Azure's default encryption](https://docs.microsoft.com/en-us/azure/virtual-machines/disk-encryption)

### **What software stack does SkySQL run on?**

SkySQL runs on Amazon Elastic Kubernetes Service (EKS), Google Kubernetes Engine (GKE), and Azure Kubernetes Service (AKS). MariaDB database products - MariaDB Server, MaxScale.

### **How long has SkySQL existed?**

SkySQL [officially launched](https://mariadb.com/newsroom/press-releases/mariadb-skysql-launches-delivers-next-generation-cloud-database/) as a production-ready enterprise-grade DBaaS in 2020, after extensive pre-release testing.

In December 2023, MariaDB completed the spinoff of its SkySQL business to SkySQL Inc., as a new independent entity founded by the former MariaDB team that built and 
supported the SkySQL product. 

### **Is SkySQL ready for production use?**

Yes. SkySQL delivers enterprise-grade cloud database services for mission-critical applications. SkySQL is built to make MariaDB enterprise ready - optimized for security, stability, and reliability in demanding production environments. Multi-node database deployments feature a comprehensive [SLA](<../Autonomously scale Compute, Storage/Uptime SLA>), High Availability (HA) features, and operations features. [Enterprise support](https://skysql.com/support-policy/) options extend support to 24x7, with the additional option of [SkyDBA](<../FractionalDBA>) for reactive and proactive assistance from a team of expert DBAs. [Security features](../Security) are designed to meet the GRC and infosec challenges faced by modern applications. [//]: # , and [DPA (GDPR) and BAA (HIPAA)](https://skysql.com/trust/) are available.

## **SkySQL Features**

### **What services are available on SkySQL?**

SkySQL is primarily designed for online applications and offers two topologies -

- Replicated: Useful for mission-critical, production workloads requiring read scaling. Replicated services feature 1 primary and up to 4 replicas and uses MariaDB MaxScale for load balancing and automatic zero-interruption failover.
- Single Node: Useful for low-cost development and test transactional workloads. Single Node services cannot be scaled to Replicated topologies.

### **What options are available for scaling and right-sizing SkySQL?**

You can choose [topologies to match your workload requirements, cloud regions to match your latency and operating requirements, instance sizes](<../Portal features/Launch page>), and [support plan](Support.md).

Our platform features:

- Availability in a range of database instance sizes and storage sizes
- Availability from multiple AWS (Amazon Web Services), GCP (Google Cloud Platform), and Azure (Cloud Computing Services) [regions](<../Reference Guide/Region Choices>).
- Load Balancing features included with Replicated Transactions topologies allow for read-scaling through read-write splitting.
- Custom instance sizes (for [Power Tier](<../Billing and Power Tier>) customers)
- Range of [support options](Support.md)

### **What reliability features are available on SkySQL?**

- SkySQL is operated by a global team of Site Reliability Engineers (SRE), and expert DBAs. Platform problems are escalated to our team 24x7.
- [Service Level Agreement](https://skysql.com/sla/), including an elevated SLA for [Power Tier](<../Billing and Power Tier>) customers
- Kubernetes self-healing - Databases run in containers in kubernetes clusters and auto-heal.
- Load balancing for multi-node configurations using MariaDB MaxScale
- High Availability (HA) for multi-node configurations
- MaxScale Redundancy option
- Inbound and outbound replication - you can replicate to your self managed MariaDB anywhere.

### **What operations features are available on SkySQL?**

- Support from SkySQL Inc, including Enterprise and Platinum tiers optionally with SkyDBA for reactive and proactive assistance
- Vendor managed infrastructure and platform
- SkySQL Portal and [SkySQL DBaaS API](https://apidocs.skysql.com/) for instance management
- Compatibility with most programming languages and clients that work with MariaDB or MySQL, for off-the-shelf integration to your stack
- Scheduled upgrades to database software
- Automated nightly backups
- Configuration management
- On-demand backups and Snapshots
- Monitoring
- Ability to deploy additional services to support application migrations and testing on the same configuration used in production
- On-demand tear-down of unneeded services

### **What governance, risk, compliance, and information security features are available on SkySQL?**

- Firewall protection, including dedicated IP allowlists to access databases and to access monitoring features
- Data-at-rest encryption
- Data-in-transit encryption by default
- VPC peering, AWS PrivateLink, GCP Private Service Connect and Azure Private Link options
- Standard or enterprise authentication for management portal
- Business Associate Addendum (BAA) for HIPAA
- Data Processing Addendum (DPA) for GDPR

## **Pricing**

### **What does SkySQL cost? How is SkySQL priced?**

Estimated SkySQL pricing is available from the SkySQL portal. SkySQL pricing is very competitive and starts at about $100 per month for production grade databases.

SkySQL pricing varies based on the selections made when you [launch a service](<../Portal features/Launch page>). Examples of selections include provider, topology, instance and storage size, and region.

The pricing shown is not a quote and does not guarantee the cost for your actual use of SkySQL services, as is shown on monthly invoices. The cost estimate can vary from your actual costs for [several reasons](<./Billing and Power Tier/Pricing.md>).

### **Do I need to purchase a MariaDB Server license or subscription to use SkySQL?**

No. Purchase of SkySQL service includes [support](Support.md) and access to MariaDB database products on SkySQL.

### **I have an existing contract with Google. Can I leverage this for SkySQL?**

Yes. SkySQL is listed in the Google Cloud Marketplace. Customers have the ability to retire their GCP commitment with a SkySQL subscription via the Marketplace.

[See the Marketplace listing](https://console.cloud.google.com/marketplace/details/skysql-public/skysql-gcp-mp).

[Contact us](mailto:info@skysql.com) if you have further questions.

### **I have an existing contract with AWS. Can I leverage this for SkySQL?**

Yes. SkySQL is an AWS partner network. Customers can retire their AWS commitment with a SkySQL subscription via the AWS Marketplace.

[See the AWS Marketplace listing](https://aws.amazon.com/marketplace/pp/prodview-txge2dgyj4ieg?sr=0-1&ref_=beagle&applicationId=AWSMPContessa)

[Contact us](mailto:info@skysql.com) if you have further questions.

### **Do you have a pricing calculator?**

Estimated SkySQL pricing information is shown when you [create a service](<./Portal features/Launch page.md>) based on the selections you make at launch time, such as topology, region, and instance size. Please [contact us](mailto:info@skysql.com) for assistance in cost estimation, including [support](Support.md) and [Power Tier](<../Billing and Power Tier>).

### **What is included in SkySQL pricing?**

SkySQL pricing includes instances for a specific service topology, and monitoring, and also includes management features, e.g., backups, upgrades, patch installs, etc. [Some factors](<./Billing and Power Tier/Pricing.md>), such as object storage and network egress which are variable and usage-dependent, are not included in estimated pricing. We typically pass-thru the cloud provider costs with no additional markup.

### **What is optional in SkySQL pricing?**

Add-ons are available to optimize your SkySQL experience:

- [SkySQL Power Tier](<../Billing and Power Tier>) is a premium service offering for SkySQL customers who have the most critical requirements for uptime, availability, performance, and support.
- While all Foundation Tier services include Standard Support, Power Tier customers are offered the  [Enterprise support plan](Support.md).
- An optional add-on, [SkyDBA](../FractionalDBA), further extends the premium support experience and the capabilities of your in-house DBAs with the backing from a global team of expert MariaDB DBAs, available 24/7 for the most severe (P1) issues. SkySQL's SkyDBAs manage your  SkySQL databases both proactively and reactively so you can focus on your core business.

### **Is discounted pricing available for a longer-term commitment?**

Yes. Discounts are typically offered for one-year and three-year commitments. Please [contact us](https://skysql.com/contact/) for more information.

## **Payment**

### **What forms of payment does SkySQL accept?**

SkySQL accepts payment by [major credit card and through remittance accounts](<./Portal features/Billing.md>)

### **Which credit cards does SkySQL accept?**

SkySQL accepts all major credit cards. Specifically, we accept Visa, Mastercard, American Express, Discover, and Diners Club payments from customers worldwide.

!!! Note
    SkySQL does not store any of your credit card information. We use Stripe to manage all credit card transactions. [Stripe](https://stripe.com) is a widely used payment processing platform that enables businesses to accept credit card payments securely

### **How do I set up my account to pay by wire transfer/ACH?**

[Contact us](https://skysql.com/contact/) to have your account set up for payment by wire transfer or ACH.

### **Can I pre-fund my account?**

SkySQL contract customers can pre-fund their account. [Contact us](https://skysql.com/contact/) for more information.

### **How do I pay my bill?**

SkySQL charges are paid using a credit card, or via wire transfer/ACH upon invoice in the case of remittance accounts.

### **Can I buy SkySQL in the AWS Marketplace?**

Yes. We offer direct purchase through the AWS Marketplace or we can craft a "private offer" to customize a subscription.

[See the AWS Marketplace listing](https://aws.amazon.com/marketplace/pp/prodview-txge2dgyj4ieg?sr=0-1&ref_=beagle&applicationId=AWSMPContessa)

[Contact us](https://skysql.com/contact/) if you have further questions.

### **I have an existing contract with AWS. Can I leverage this for SkySQL?**

Yes. SkySQL is an AWS partner network. Customers can retire their AWS commitment with a SkySQL subscription via the AWS Marketplace.

[See the AWS Marketplace listing](https://aws.amazon.com/marketplace/pp/prodview-txge2dgyj4ieg?sr=0-1&ref_=beagle&applicationId=AWSMPContessa)

[Contact us](https://skysql.com/contact/) if you have further questions.

### **Can I buy SkySQL in the Google Marketplace?**

Yes. We offer direct purchase through the Google Marketplace or we can craft a "private offer" to customize a subscription.

[See the Marketplace listing](https://console.cloud.google.com/marketplace/details/skysql-public/skysql-gcp-mp).

[Contact us](https://skysql.com/contact/) if you have further questions.

### **I have an existing contract with Google. Can I leverage this for SkySQL?**

Yes. SkySQL is listed in the Google Cloud Marketplace. Customers have the ability to retire their GCP commitment with a SkySQL subscription via the Marketplace.

[See the Marketplace listing](https://console.cloud.google.com/marketplace/details/skysql-public/skysql-gcp-mp).

[Contact us](https://skysql.com/contact/) if you have further questions.

## **Billing and Invoices**

### **Am I charged for deleted or stopped databases? What database states are billable?**

If you stop a SkySQL service, you will continue to be charged for storage, since your data is not deleted. Instance and egress charges will stop until the instance is started again.

### **How can I see my current charges?**

Current month's estimated charges can be viewed on the SkySQL portal dashboard. Detailed information is also available under ‘Billing’ where you can see the breakdown for all your current charges - you can see resource usage by Service Name (your individual DB clusters) or by resource type. Variable charges such as object storage and network egress are updated the day prior to the last day of the month and are available in the invoice. You can also use the [SkySQL REST API](https://apidocs.skysql.com/) to fetch usage and billing data.

### **How can I see detailed billing reports?**

SkySQL invoices are sent monthly and include a detailed breakdown of usage, pricing, and taxes. For Team accounts, only the Team owner has access to Account Information.

### **When will I be billed?**

Invoices for  SkySQL are sent by email on subscription renewal. Subscription renewal occurs on the last day of the month. Accounts using a credit card are charged at this time.

### **Will I be charged VAT or taxes?**

MariaDB will bill for VAT and/or taxes in applicable jurisdictions. Customers are responsible for paying all applicable taxes and fees. See the [SkySQL Terms of Use](https://skysql.com/tos/) for additional information.

### **How can I see discounts and service credits?**

Current month's estimated charges, including coupons and service credits, can be viewed on the Account Information page and are updated six times per day.

In the event of service credits issued based on [SLA](https://skysql.com/sla/), service credits will be included in coupons and service credits on the Account Information page.

### **Can I stop or pause my instance to save money?**

Instances can be stopped to save money. While stopped, additional instance and egress charges will not accrue, but you will continue to be charged for storage.

### **Who do I contact with billing questions?**

Contact [info@skysql.com](mailto:info@skysql.com) with billing questions.

## Backup and Restore

### **How do I backup my data on SkySQL?**

SkySQL runs full backups automatically each night. SkySQL also allows on-demand backups (a Preview feature).

### **Can I set the frequency or schedule of automated backups?**

Yes. Backup frequency and schedule can be customized using the [API](https://apidocs.skysql.com/) or [Portal](https://app.skysql.com/backups/service-backups). See [Backup and Restore](../Backup and Restore) for more information.

### **Are automated backups sent offsite? Will my data be sent to another country?**

No. Data is not sent to another country. All data is managed within the same region by default where your database is running for data sovereignty.

### **Does SkySQL guarantee an RTO and RPO?**

No.

### **Do backup operations impact application performance?**

No.

MariaDB Backup (mariabackup) is used for Replicated Transactions and Single Node Transactions service backups. MariaDB Backup breaks up backups into non-blocking stages so writes and schema changes can occur during backups.

### **Are incremental backups available?**

The backup service provides support for incremental backups. It is in preview state (October 2024).

### **How long are backups retained?**

Backups for running and stopped services are retained for 7 days.

### **Can I set the retention window for automated backups?**

No. Currently, backup retention is not customer configurable. SkySQL Power Tier customers should contact us if an alternate retention schedule is required.

### **How do I restore my data from a SkySQL backup?**

Request data restore by creating a support case in the [Customer Support Portal](https://support.skysql.com/). Please state what you need restored, and the desired restore point. Self service Restore functionality is available using the Backup service and [API](https://apidocs.skysql.com/) (in Preview as of Oct 2024)

### **Can I request a partial restore of data from backup?**

Yes, by [support case](https://support.skysql.com).

### **Does SkySQL support Point-in-Time Recovery (PITR)?**

By default, full and complete backup restoration is available. To enable point-in-time recovery, services must be configured in advance for additional binary log retention. [Point-in-time recovery (PITR)](<../Backup and Restore>) configuration is available to Power Tier customers.

### **Can I request a restore of my data to a different region?**

Yes, self service restore is available using the Backup service and [API](https://apidocs.skysql.com/) (in Preview as of Oct 2024).

### **Can I request a restore of my data to a different topology?**

Yes, self service restore is available using the Backup service and [API](https://apidocs.skysql.com/) (in Preview as of Oct 2024).

### **Can I retrieve my database backup from SkySQL? Is there vendor lock?**

Yes, you can retrieve your database. No, there is no vendor lock. Your data is your data. Create a [support case](https://support.skysql.com) for access to a backup.

## **Encryption**

### **Does SkySQL support data-at-rest encryption (on-disk encryption)?**

Yes. All SkySQL data is encrypted on disk.

- SkySQL on Amazon AWS benefits from [Amazon EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html).
- SkySQL on Google GCP benefits from [encryption by default](https://cloud.google.com/security/encryption-at-rest/default-encryption).
- SkySQL on Microsoft Azure leverages Azure's [default encryption](https://docs.microsoft.com/en-us/azure/virtual-machines/disk-encryption).

### **Does SkySQL support data-in-transit encryption (over the network encryption)?**

By default,  SkySQL services feature data-in-transit encryption for client connections.

By default, server-to-server communications between the nodes of a SkySQL service are protected with data-in-transit encryption.

For additional information, see "[Data-in-Transit Encryption](./Security/Encryption.md)".

### **Does SkySQL support encrypted client connections?**

Yes. By default, SkySQL requires client connections via TLS (TLS 1.2, TLS 1.3).

### **Does SkySQL support unencrypted client connections?**

SkySQL supports disabling SSL/TLS via the Portal or using the [API](https://apidocs.skysql.com/).

### **What encryption algorithms are used for on-disk encryption?**

SkySQL on Amazon AWS benefits from [Amazon EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html), which is AES256.

SkySQL on Google GCP leverages Google's [default encryption](https://cloud.google.com/security/encryption-at-rest/default-encryption), which is AES256 or AES128.

SkySQL on Microsoft Azure leverages Azure's [default encryption](https://docs.microsoft.com/en-us/azure/virtual-machines/disk-encryption), which is AES256.

### **What versions of SSL or TLS are supported?**

TLS 1.2, and TLS 1.3 are supported.

### **When do TLS certificates expire?**

TLS certificates expire every 90 days but are automatically renewed without customer action and without service interruption.

### **How are TLS certificates and encryption keys managed?**

SkySQL leverages HashiCorp Vault for certificate and key management. Certificates and keys are not customer-configurable at this time.

### **Are client certificates supported?**

No. SkySQL supports server-side certificates. Database users are authenticated by standard password authentication.

### **Is ed25519 authentication supported?**

No.

### **How frequently are cryptography libraries (like OpenSSL) updated?**

Cryptography libraries are included in our standard release process, and vulnerability scanning is conducted for each release.

## **Data Deletion**

### **Can I delete my running or stopped SkySQL service?**

Yes. The decision to delete your running service rests with you and your business. Please consider production impacts before deleting a service. SkySQL permits the on-demand deletion of running and stopped services.

### **Can I delete my pending SkySQL service?**

No. The launch process must complete before deletion is permitted.

### **How long do you keep my data when I delete a service?**

All data residing on a service's storage is deleted at time of service deletion. Backups for deleted services are purged after 7 days.

### **Can I get my data back if I delete a service by accident?**

Maybe. If you contact us before the system completes data deletion, yes, we can recover. Backups for deleted services are purged after 7 days.

### **Is it possible for SkySQL to retain my data when I delete a service?**

No. You should download your data so you have a local copy before you delete the service.

### **Will my data be retained if a hard drive gets swapped-out?**

No. SkySQL is hosted on public cloud provider systems.

- SkySQL on Amazon AWS benefits from [Amazon EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) and [Amazon's data destruction controls](https://aws.amazon.com/compliance/data-center/data-layer/).
- SkySQL on Google Cloud benefits from [encryption by default](https://cloud.google.com/security/encryption-at-rest/default-encryption) and [Google's data destruction controls](https://www.google.com/about/datacenters/data-security/).
- SkySQL on Microsoft Azure leverages Azure's [default encryption](https://docs.microsoft.com/en-us/azure/virtual-machines/disk-encryption), and Azure's [data destruction controls](https://learn.microsoft.com/en-us/azure/security/fundamentals/protection-customer-data).

### **Can data be purged from backups?**

Yes. If you would like backups purged, please [create a support case](https://support.skysql.com)

## **Monitoring**

### **How do I access monitoring?**

You can SkySQL Monitoring after launching a service, then clicking the "Monitoring" link in the SkySQL main menu (left navigation).

### **What is monitored?**

SkySQL Monitoring covers status and metrics specific to a service and its servers. A [complete list of charts](<./Reference Guide/Monitoring Metrics Reference.md>) is provided.

### **Who is alerted if a service goes down?**

SkySQL Inc’s Support and SRE teams are alerted if a SkySQL service becomes unavailable or when serious issues are detected (e.g. disk is 90% utilized).

Additionally, SkySQL automatically turns on several sensible alerting rules so the customer can also be alerted.

### **How can I get Alerted on DB events?**

SkySQL Monitoring includes alerting features, which allow configurable alerting rules, notification channels, and notification criteria. These settings are managed from the SkySQL Monitoring interface. You can SkySQL Monitoring after launching a service by clicking the "Monitoring" link in the SkySQL main menu (left navigation).

## **Support**

### **How do I contact support?**

SkySQL customers can contact us via the [Customer Support Portal](https://support.skysql.com/).

If you are not yet a SkySQL customer, please [contact us](https://skysql.com/contact/) with questions.

### **What support options are available for SkySQL?**

Included with Foundation Tier services:

- Standard Support, 24x5

Available to Power Tier customers:

- Enterprise Support, 24x7

See [full details of our support options](./Support.md).

### **Is 24x7x365 support available for mission-critical applications?**

Yes. [Enterprise Support](./Support.md) levels are available for customers requiring 24x7x365 support (24 hours per day, 7 days per week, 365 (or 366) days per year).

### **Is SkySQL fully managed?**

Yes. SkySQL infrastructure is fully managed, including many typical operations features such as automated nightly backups and monitoring.

Standard support is included with Foundation Tier services. Activities like performance tuning and assistance with schema change are not included in standard support. Power Tier customers choose between Enterprise and Platinum support options, which include consultative support.

Our optional [SkyDBA](FractionalDBA.md) service is available for Enterprise and Platinum support customers, and SkyDBA customers receive both reactive (break/fix) and proactive (analyze/enhance) assistance.

### **What professional services are available for SkySQL?**

SkySQL offers a full range of professional services, including:

- [SkyDBA](./FractionalDBA.md) for proactive and reactive support
- [Migration](<./Data loading, Migration/nr-support-assisted.md>) assistance
- Assistance with your SkySQL proof-of-concept ([contact us for more information](https://skysql.com/contact/))

## **Inquiries**

### **Who do I contact if I have questions about SkySQL?**

[Contact SkySQL Inc](https://skysql.com/contact/) or email us at [info@skysql.com](mailto:info@skysql.com)

### **How do I contact sales to buy services on SkySQL?**

SkySQL is available for immediate use. [Get started now](https://app.skysql.com). If you would like assistance from sales, [contact us](https://skysql.com/contact/).

### **Who do I contact with billing questions?**

Billing questions can be directed to the [Billing Team](mailto:billing@skysql.com).

### **How do I provide feedback about SkySQL Documentation?**

To aid our continuous improvement efforts, we encourage you to provide feedback on our documentation and your experiences using it via emailing [info@skysql.com](mailto:info@skysql.com)

### **I am a SkySQL customer. How do I get support?**

SkySQL customers can contact us via the [Support Portal](https://support.skysql.com/). Customers can also use the [Support email](mailto:support@skysql.com).
