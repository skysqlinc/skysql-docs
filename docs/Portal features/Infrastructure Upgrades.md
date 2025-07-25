# Infrastructure Upgrades

SkySQL runs on modern Kubernetes infrastructure across AWS, GCP, and Azure cloud providers. To maintain security, performance, and compliance with cloud provider requirements, periodic infrastructure upgrades are essential for all running services.  
Infrastructure upgrades are a critical component of maintaining a secure, performant, and compliant database service. By staying current with these upgrades, you ensure continued access to the latest features, security updates, and cloud provider support.

## Understanding Infrastructure vs Database Upgrades

**Infrastructure upgrades** are different from database software upgrades:

- **Infrastructure Upgrades**: Update the underlying Kubernetes nodes, container runtime, and cloud provider components that host your database
- **Database Upgrades**: Update the MariaDB database software version itself

## Service Impact During Upgrades

**Multi-node databases** (with high availability configurations) experience **no downtime** during infrastructure upgrades. The rolling upgrade process maintains continuous database availability and service connectivity.

**Single-node databases** will experience a brief downtime during the database restart, typically lasting 2-5 minutes.

Infrastructure upgrades ensure your service continues to receive:

- Security patches and vulnerability fixes
- Performance improvements and optimizations  
- Continued support from cloud providers (AWS, GCP, Azure)
- Access to the latest cloud platform features

## Why Infrastructure Upgrades Are Required

### Cloud Provider Requirements
Our cloud providers (AWS, GCP, Azure) enforce strict upgrade schedules for Kubernetes infrastructure:

- **Extended Support Fees**: Cloud providers charge additional fees for nodes running on older, end-of-life Kubernetes versions
- **Security Compliance**: Outdated infrastructure versions may not receive critical security updates
- **Support Limitations**: Cloud providers eventually discontinue support for older infrastructure versions

### SkySQL Platform Stability
Since all customer services share the same underlying control plane infrastructure, we must coordinate upgrades across all services to maintain platform stability and cost-effectiveness.

## How Infrastructure Upgrades Work

### Notification Timeline
1. **3 Months Prior**: SkySQL sends initial notification about upcoming infrastructure upgrade requirements
2. **Regular Reminders**: Follow-up notifications are sent as the deadline approaches
3. **Grace Period**: Customers have time to perform upgrades before the automatic deadline

### Upgrade Support Policy
SkySQL supports infrastructure versions up to **3 versions behind** the current control plane version. This provides flexibility in when you choose to upgrade while ensuring security and support compliance.

### Upgrade Process
1. **Customer-Initiated**: You can trigger infrastructure upgrades immediately through the SkySQL Portal
2. **Automatic Fallback**: If not upgraded by the deadline, SkySQL will automatically perform the upgrade to maintain compliance
3. **Database Restart**: For single-node databases, infrastructure upgrades require a database restart, typically taking 2-5 minutes. Multi-node databases use rolling upgrades with no downtime.

!!! warning "Service Interruption for Single-Node Databases"
    Single-node database infrastructure upgrades require a database restart. Multi-node databases experience no downtime during upgrades. Plan accordingly for single-node services and perform upgrades during low-traffic periods when possible.

## Managing Infrastructure Upgrades in the Portal

### Viewing Available Upgrades
1. Log in to the [Portal](https://app.skysql.com/dashboard).
2. Any affected service will display an **Upgrade Available** or **Upgrade Required** notification.
3. An upgrade deadline will be shown, indicating the last date to perform the upgrade before automatic enforcement.

### Performing an Upgrade
1. Click **Upgrade** next to the available infrastructure update
2. Review the upgrade details and impact
3. Confirm to begin the immediate upgrade process
4. Monitor the upgrade progress through the Portal

!!! info "Immediate Processing"
    Infrastructure upgrades begin immediately when initiated and cannot be scheduled for a future time.

### Upgrade Status Tracking
Monitor your upgrade progress through:
- Real-time status updates in the Portal
- Email notifications at key milestones
- Service monitoring panels showing restart progress

## Best Practices

### Planning Your Upgrades
- **Upgrade During Low Traffic**: For single-node databases, perform upgrades during low-traffic periods since they require a restart
- **Monitor Notifications**: Regularly check the Portal and email notifications for upgrade requirements
- **Plan for Single-Node Services**: Coordinate with your team about the brief service interruption for single-node database restarts. Multi-node databases can be upgraded anytime without service impact.

## Frequently Asked Questions

**Q: How long does an infrastructure upgrade take?**
A: Multi-node databases experience no downtime during infrastructure upgrades. Single-node databases complete upgrades within 2-5 minutes, including the database restart time.

**Q: Can I schedule an upgrade for later?**
A: No, infrastructure upgrades begin immediately when initiated. Plan to perform them during appropriate maintenance windows.

**Q: What happens if I don't upgrade by the deadline?**
A: SkySQL will automatically perform the upgrade to maintain cloud provider compliance and platform stability.

**Q: Will my data be affected during the upgrade?**
A: No, infrastructure upgrades only restart the database service. Your data remains intact and unchanged.

**Q: How often do infrastructure upgrades occur?**
A: Infrastructure upgrade requirements vary based on cloud provider schedules and your current version. SkySQL supports up to 3 versions behind the current control plane, giving you flexibility in upgrade timing.

**Q: Can I opt out of infrastructure upgrades?**
A: No, infrastructure upgrades are mandatory to maintain security, compliance, and cloud provider support.
