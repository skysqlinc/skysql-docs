# SkySQL Architecture

SkySQL cloud infrastructure is composed of a Control plane and a Data plane. The Control Plane is built using event driven micro-services architecture 
and provides a RESTful API for managing the lifecycle of databases as well as performs orchestration tasks on behalf of the RESTful APIs. The Data Plane is an isolated 
enviromnment where the databases are deployed via the orchestrator. All the components use managed kubernetes from the cloud provider.

The following diagram shows the architecture of SkySQL.

![SkySQL Architecture](skysql-architecture.png)

The major components of the SkySQL architecture are:

## SkySQL Core Services

Core services cluster runs all the necessary control plane services and the orchestrator. The orchestrator consists of workflow engine that invokes various workflows
to manage the database lifecycle actions. The Monitoring infrastructure provides the metrics that drive the dashboards in the customer portal. This cluster is deployed in an isolated environment with a very strict security perimeter.

## Customer Clusters

The databases are deployed in the customer clusters. The orchestrator engine is responsible for building and maintaining the databases. Each cluster runs in its own VPC with
a strict role-based access controls. The SkySQL Kubernetes Operator builds and maintains the desired database state and takes remediation actions when needed.
