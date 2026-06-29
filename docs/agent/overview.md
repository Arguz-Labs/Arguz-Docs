# Discovery Agent Overview

The Discovery Agent is a long-running Kubernetes controller optimized for inventory and change detection, not for traffic interception. It relies on informers, periodic loops and authenticated batch submissions to keep the Arguz control plane updated.

## Runtime behavior

### High availability

- leader election in the `arguz-agent` namespace
- two replicas recommended
- stateless controller design

### Main loops

| Loop | Purpose |
|---|---|
| Heartbeat | Marks the cluster as connected and updates basic health |
| Cluster metadata heartbeat | Refreshes provider, region, zone, cluster name and Kubernetes version |
| Node snapshot loop | Captures node capacity, allocatable values and conditions |
| Deployment sync | Maintains deployment, image and revision inventory |
| CronJob sync | Maintains schedules and captures live Job execution results |

## Kubernetes resources it watches

- Deployments
- Pods
- Jobs
- CronJobs
- HPAs
- Namespaces
- Services
- Ingresses
- ConfigMaps
- RBAC resource creation metadata

## Provider detection

The agent first tries cloud metadata endpoints and then falls back to Kubernetes node labels when metadata endpoints are not reachable from the pod. This is especially important for restricted EKS environments.

Fields sent to Arguz include:

- provider
- cluster scope
- region and zone
- project, account or subscription identifiers
- cluster name and cluster id when available
- Kubernetes version

## CronJob capture model

Arguz stores two layers of CronJob information:

1. the CronJob definition
2. the execution history produced by the child Jobs

For each failed execution, the agent stores the last 100 log lines so operators can review failure context directly from the frontend.

## Helm-aware revision metadata

When workloads are managed by Helm, the agent extracts release metadata from Helm Secrets and sends a sanitized representation to Arguz. Sensitive values are redacted, but resource identities remain readable so teams can still understand which Secret, ConfigMap or workload is involved.
