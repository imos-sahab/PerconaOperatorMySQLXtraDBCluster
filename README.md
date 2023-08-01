# PerconaOperatorMySQLXtraDBCluster

Kubernetes and the OpenShift platform, based on Kubernetes, have added a way to manage containerized systems, including database clusters. This management is achieved by controllers, declared in configuration files. These controllers provide automation with the ability to create objects, such as a container or a group of containers called pods, to listen for an specific event and then perform a task.

This automation adds a level of complexity to the container-based architecture and stateful applications, such as a database. A Kubernetes Operator is a special type of controller introduced to simplify complex deployments. The Operator extends the Kubernetes API with custom resources.

Percona XtraDB Cluster is an open-source enterprise MySQL solution that helps you to ensure data availability for your applications while improving security and simplifying the development of new applications in the most demanding public, private, and hybrid cloud environments.

Following our best practices for deployment and configuration, Percona Operator for MySQL based on Percona XtraDB Cluster contains everything you need to quickly and consistently deploy and scale Percona XtraDB Cluster instances in a Kubernetes-based environment on-premises or in the cloud.

Requirements¶
System Requirements

Design and architecture

Comparison with other solutions

Quickstart guides¶
Install with Helm

Install with kubectl

Advanced Installation Guides¶
Install on Minikube

Install on Google Kubernetes Engine (GKE)

Install on Amazon Elastic Kubernetes Service (AWS EKS)

Install on Microsoft Azure Kubernetes Service (AKS)

Install on OpenShift

Generic Kubernetes installation

Multi-cluster and multi-region deployment

Configuration¶
Application and system users

Changing MySQL Options

Anti-affinity and tolerations

Labels and annotations

Local Storage support

Defining environment variables

Load Balancing with HAProxy

Load Balancing with ProxySQL

Transport Encryption (TLS/SSL)

Data at rest encryption

Telemetry

Management¶
Backup and restore

About backups

Configure storage for backups

Make scheduled backups

Make on-demand backup

Store operations logs for point-in-time recovery

Enable compression for backups

Restore from a previously saved backup

Copy backup to a local machine

Delete the unneeded backup

Upgrade Database and Operator

Horizontal and vertical scaling

Monitor with Percona Monitoring and Management (PMM)

Add sidecar containers

Restart or pause the cluster

Crash recovery

Troubleshooting¶
Initial troubleshooting

Exec into the container

Check the logs

Special debug images

HOWTOs¶
How to install Percona XtraDB Cluster in multi-namespace (cluster-wide) mode

How to upgrade Percona XtraDB Cluster manually

How to use private registry

How to restore backup to a new Kubernetes-based environment

How to use backups and asynchronous replication to move an external database to Kubernetes

