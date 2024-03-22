# High availability

My homelab is designed to tolerate at least one failure of a servers. It utilises an Elastic IP (EIP) so that the Kubernetes API (kube-apiserver) will remain accessible if one (or more of the servers) becomes unavailable.

## Highly available persistent storage

Production Kubernetes clusters typically rely on robust, external persistent storage solutions, integrated through CSI drivers (e.g. AWS Elastic Block Store, GCE Persistent Disk, Azure Disk). In contrast, my homelab employs the built-in [etcd](https://docs.k3s.io/datastore/ha-embedded) of k3s for storage, suitable for learning and initial scaling. Future hardware enhancements will allow for the adoption of a dedicated storage system like [Longhorn](https://longhorn.io/), offering improved stability and features such as easy backup and restore, which are crucial for production use.

**Upgrade Path**: The transition from embedded etcd to a more resilient storage solution will be a critical step in maturing the cluster's capability to handle stateful workloads reliably.


## Highly available GitOps

In an HA setup, ArgoCD's components are deployed in a way that allows them to withstand node failures, network partitions, and other types of outages. The key components of ArgoCD that can be scaled for HA include:

- API Server: The server that exposes the ArgoCD API.
- Repo Server: Handles Git repository operations.
- Application Controller: Monitors and manages the state of applications.
- Redis: Used for caching and message queuing.

### Data consistency
ArgoCD stores its state in a database (by default, this is a built-in instance of PostgreSQL starting from ArgoCD v2.1, or otherwise it uses Kubernetes Custom Resources). Running multiple ArgoCD instances without a shared, consistent database could lead to inconsistencies and split-brain scenarios, where different instances of ArgoCD have different views of the system state.

### Configuration
- Load balancing: I'd need to configure a load balancer in front of the ArgoCD instances to distribute traffic appropriately (requiring ingress controllers to be configured prior to ArgoCD in my case?). This setup must also account for health checks to ensure traffic is only routed to healthy instances.
- Storage: Ensure that all instances can access the same configuration data and repositories. This might involve shared volumes or consistent configurations that point to the same external services (e.g., Git repositories).