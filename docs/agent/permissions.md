# Required Permissions

This document describes the Kubernetes RBAC permissions required by the Arguz Agent and how to configure them.

## RBAC Overview

The Arguz Agent requires **read-only** access to specific Kubernetes API resources. It uses a dedicated ServiceAccount with a ClusterRole and ClusterRoleBinding to limit its access to only what is needed.

## Required Permissions

The agent watches the following resources and requires `get`, `list`, and `watch` verbs for each:

### Core Resources (apiGroups: `""`)

| Resource | Verbs | Purpose |
|---|---|---|
| `pods` | get, list, watch | Pod lifecycle and error detection |
| `services` | get, list, watch | Service discovery |
| `configmaps` | get, list, watch | ConfigMap tracking |
| `namespaces` | get, list, watch | Namespace discovery |
| `nodes` | get, list, watch | Node resource snapshots |
| `events` | get, list, watch | Kubernetes event monitoring |

### Apps Resources (apiGroups: `apps`)

| Resource | Verbs | Purpose |
|---|---|---|
| `deployments` | get, list, watch | Deployment tracking and revision capture |

### Autoscaling Resources (apiGroups: `autoscaling`, `autoscaling/v2`)

| Resource | Verbs | Purpose |
|---|---|---|
| `horizontalpodautoscalers` | get, list, watch | HPA configuration tracking |

### Batch Resources (apiGroups: `batch`)

| Resource | Verbs | Purpose |
|---|---|---|
| `jobs` | get, list, watch | Job creation monitoring |

### Networking Resources (apiGroups: `networking.k8s.io`)

| Resource | Verbs | Purpose |
|---|---|---|
| `ingresses` | get, list, watch | Ingress creation tracking |
| `networkpolicies` | get, list, watch | NetworkPolicy creation tracking |

### RBAC Resources (apiGroups: `rbac.authorization.k8s.io`)

| Resource | Verbs | Purpose |
|---|---|---|
| `roles` | get, list, watch | Role creation tracking |
| `rolebindings` | get, list, watch | RoleBinding creation tracking |
| `clusterroles` | get, list, watch | ClusterRole creation tracking |
| `clusterrolebindings` | get, list, watch | ClusterRoleBinding creation tracking |

### Coordination Resources (apiGroups: `coordination.k8s.io`)

| Resource | Verbs | Purpose |
|---|---|---|
| `leases` | get, list, watch, create, update | Leader election for agent HA |

!!! note "Leader Election Leases"
    The Lease resource requires `create` and `update` verbs because the agent uses it for leader election. This is a standard Kubernetes pattern and does not modify your workloads.

### Discovery Resources (apiGroups: `discovery.k8s.io`)

| Resource | Verbs | Purpose |
|---|---|---|
| `endpointslices` | get, list, watch | Service endpoint discovery |

## Example ClusterRole

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: arguz-agent
rules:
  - apiGroups: [""]
    resources: ["pods", "services", "configmaps", "namespaces", "nodes", "events"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["apps"]
    resources: ["deployments"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["autoscaling", "autoscaling/v2"]
    resources: ["horizontalpodautoscalers"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["networking.k8s.io"]
    resources: ["ingresses", "networkpolicies"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["rbac.authorization.k8s.io"]
    resources: ["roles", "rolebindings", "clusterroles", "clusterrolebindings"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["coordination.k8s.io"]
    resources: ["leases"]
    verbs: ["get", "list", "watch", "create", "update"]
  - apiGroups: ["discovery.k8s.io"]
    resources: ["endpointslices"]
    verbs: ["get", "list", "watch"]
```

## Example ServiceAccount and Binding

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: arguz-agent
  namespace: arguz-agent
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: arguz-agent
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: arguz-agent
subjects:
  - kind: ServiceAccount
    name: arguz-agent
    namespace: arguz-agent
```

## What Permissions the Agent Does NOT Need

The agent does **not** require:

- **Write access** to any workload resource (Deployments, Pods, Services, etc.) — except Leases
- **Delete access** to any resource
- **Exec access** to pods
- **Port-forward access**
- **Access to Secrets** — the agent does not read Kubernetes Secrets (except the Helm release metadata secret, if configured)
- **Access to ConfigMap/Secret data** — only metadata (names, creation timestamps)
- **Privileged containers** — the agent container runs as a standard non-root user
- **Host network** or **host PID** access
- **Volume mount** access to host paths

## Helm Chart Defaults

When you install the agent via the Helm chart, the required ServiceAccount, ClusterRole, and ClusterRoleBinding are created automatically with the minimum required permissions. No manual RBAC configuration is needed in most cases.

If your cluster enforces restrictive policies (e.g., OPA Gatekeeper, Kyverno), you may need to allow the ClusterRole above explicitly.

## Customizing Permissions

If you want to restrict the agent further, you can manage RBAC manually and set:

```yaml
Discovery-Agent:
  serviceAccount:
    create: false
    name: "my-custom-sa"
```

Then create the ServiceAccount and ClusterRole with only the resources you want to monitor. Note that reducing permissions may cause some Arguz features (such as pod error detection or node snapshots) to stop working for those resources.
