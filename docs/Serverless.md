# Serverless
SkySQL Serverless is the first true serverless database solution for the MySQL and MariaDB ecosystem, uniquely built to meet the dynamic demands of modern applications. Unlike other “serverless” offerings, SkySQL combines instant scalability with cost efficiency, scaling down to zero when idle—saving you from paying for unused capacity. When workloads resume, Serverless service is ready in milliseconds, ensuring a seamless, uninterrupted experience for applications and users. 

## Get Started
Follow the [Quickstart](Quickstart/README.md) guide to launch a Serverless service. Make sure to select 'Serverless' as the service type.

## Key Features
- **Forever Free Database**: Each SkySQL account gets one perpetually free serverless database, ideal for development and testing environments, allowing teams to innovate without infrastructure management concerns.
- **Instantaneous Scaling**: SkySQL Serverless launches databases in milliseconds, allocating compute and memory resources precisely as needed to prevent over-provisioning.
- **Cost Efficiency**: The service scales down to zero during idle periods, eliminating costs associated with unused resources. When demand resumes, it instantly scales up to handle workloads, ensuring optimal performance without unnecessary expenses.
- **Seamless Integration**: Built without forking the trusted open-source database, SkySQL Serverless maintains full compatibility with existing MariaDB and MySQL applications, facilitating easy migration and integration.

## Technical Overview
SkySQL Serverless employs cloud-native techniques to achieve its capabilities:

- **Cloud-Native Scaling**: Utilizes Kubernetes with custom-built controllers for fine-grained resource control, enabling rapid and non-disruptive vertical and horizontal scaling.
- **Efficient Resource Management**: Maintains “hot” micro DB servers that can be restored seamlessly, facilitating efficient scaling from zero and ensuring consistent performance.
- **Persistent Connections**: Ensures always-on connections, even when database servers scale to zero, providing uninterrupted service for applications.

## Provisioned vs Serverless: What to Choose?
For consistent workloads that require steady, always-on database performance and predictable cost, launch your service with the provisioned option. Provisioned databases are ideal for production systems with predictable demand, ensuring optimal performance. Consider using Serverless option for following use cases: 

- **Development and Testing**: The free tier allows developers to build and test applications without incurring costs, focusing on innovation rather than infrastructure management.
- **Unpredictable Workloads**: Applications with variable or unpredictable demand benefit from automatic scaling, ensuring performance during peak times and cost savings during low activity periods.
- **Microservices Architectures**: Serverless databases complement microservices by providing scalable, independent data stores that align with the dynamic nature of microservices environments.

For a closer look into the technical design that enables Serverless instant autoscaling, please read the [Serverless](https://skysql.com/blog/what-sets-skysql-serverless-apart) blog.