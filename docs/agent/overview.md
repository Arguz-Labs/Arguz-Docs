# Agent Overview

The Arguz Agent is a lightweight, read-only Kubernetes controller that connects your cluster to the Arguz platform. It observes cluster state and reports changes, enabling deployment tracking, error detection, and governance features.

## What the Agent Does

In simple terms, the agent:

1. **Watches** your Kubernetes cluster for changes to specific resources
2. **Detects** new deployments, pod errors, HPA changes, and cluster metadata
3. **Reports** these changes to the Arguz platform
4. **Heartbeats** regularly to confirm cluster connectivity

## How It Helps

### Deployment Visibility

Without the agent, you would need to manually track what was deployed, when, and by whom across multiple clusters. The agent automates this by:

- Capturing every deployment change in real time
- Recording the container images and tags in use
- Noting whether the deployment succeeded or failed
- Linking Helm release metadata when applicable

This gives you a complete, searchable deployment history across all your clusters.

### Incident Analysis

When an incident occurs, the agent provides critical context:

- What was the last deployment before the error started?
- Which pods are affected?
- Are there CrashLoopBackOff or OOMKilled events?
- What HPA configuration was active?

All of this information is captured automatically and available in the Arguz dashboard, reducing the time spent correlating data from multiple sources.

### Understanding Your Infrastructure

The agent helps you understand your infrastructure at a glance:

- Which services are running in each namespace?
- What container images are deployed across your fleet?
- Which deployments have autoscaling configured?
- How are nodes being utilized?

### Governance

The agent enables governance features by providing the data foundation:

- **Alert policies** need to know when errors occur and which deployments are affected
- **Freeze windows** need to know about deployment attempts
- **Scaling rules** need current and historical HPA configurations

## How the Agent Operates

### Deployment Model

The agent is deployed as a Kubernetes Deployment via Helm. It runs 2 replicas by default and uses **Leader Election** (via Kubernetes Leases) to ensure only one replica actively watches resources at a time.

```
Replica 1 (Leader)  ─── Watches K8s API ─── Sends to Arguz
Replica 2 (Standby) ─── Idle ─────────────── Ready to take over
```

If the leader fails, the standby replica becomes the new leader within ~10 seconds.

### Resource Footprint

The agent is designed to be minimal:

| Resource | Limit |
|---|---|
| CPU | 100m per replica |
| Memory | 128Mi per replica |
| Disk | None (stateless) |

### Configuration

The agent supports per-cluster configuration for:

- **Namespace exclusion**: Skip system namespaces like `kube-system`, `cert-manager`, or monitoring tools
- **Resource exclusion**: Ignore specific resource types (e.g., `job/*` for short-lived Jobs)
- **Node label filtering**: Exclude specific node labels from snapshots

## What the Agent Tracks

The agent watches the following Kubernetes resources:

- **Deployments** — Creation, updates, and deletion
- **Pods** — Lifecycle events including CrashLoopBackOff, OOMKilled, and restarts
- **HorizontalPodAutoscalers** — Configuration changes (min/max replicas, metrics, behavior)
- **Namespaces** — New namespace creation
- **Services** — Service creation
- **ConfigMaps** — ConfigMap creation
- **RBAC Resources** — Roles, RoleBindings, ClusterRoles, ClusterRoleBindings
- **Jobs** — Job creation
- **Ingresses and NetworkPolicies** — Creation events
- **Nodes** — Resource usage snapshots

## What the Agent Does NOT Do

Equally important is what the agent does **not** do:

- **Does not modify** your workloads, deployments, or cluster configuration
- **Does not execute** commands or changes in your cluster
- **Does not capture** application source code or secrets
- **Does not capture** data from PersistentVolumes or ConfigMaps
- **Does not alter** network traffic or inject sidecars
- **Does not require** privileged containers or host-level access
- **Does not collect** data outside of Kubernetes API resources
