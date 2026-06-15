# Clusters

Clusters represent the Kubernetes clusters you have registered with Arguz. This section covers cluster management, connectivity monitoring, and cluster-level operations.

## Cluster Registration

Before Arguz can track deployments in a cluster, you must register it. Registration creates a unique **Cluster Token** that the Arguz Agent uses to authenticate.

### Registering a Cluster

1. Navigate to **Clusters** in the sidebar
2. Click **Register Cluster**
3. Select the **Project** this cluster belongs to
4. Enter a **cluster name** and optional **description**
5. Select a **provider** label (GCP, AWS, Azure, On-Premises) for organizational purposes
6. Click **Register** — the platform generates a Cluster ID and Cluster Token

Copy the credentials immediately — the token will not be shown again (though it can be rotated later).

### After Registration

After registering the cluster:

1. [Install the Arguz Agent](../getting-started/installation.md) using the generated credentials
2. Wait 2-3 minutes for the agent to connect and begin reporting
3. Verify the cluster status shows **Connected** (green indicator)
4. Deployments should begin appearing within minutes

## Cluster List View

Navigate to **Clusters** to see all registered clusters:

| Column | Description |
|---|---|
| **Name** | Cluster name |
| **Project** | Associated project |
| **Provider** | Cloud provider label |
| **Status** | Connectivity status (Connected, Delayed, Disconnected) |
| **Namespaces** | Number of tracked namespaces |
| **Deployments** | Number of tracked deployments |
| **Last Seen** | Timestamp of last agent heartbeat |

### Status Indicators

| Status | Indicator | Meaning |
|---|---|---|
| **Connected** | Green | Agent is sending heartbeats on schedule |
| **Delayed** | Yellow | Heartbeat is late but still within threshold |
| **Disconnected** | Red | Agent has not sent a heartbeat beyond the threshold |

## Cluster Detail View

Click any cluster to see its detail page, which includes:

- **Cluster metadata** — Kubernetes version, provider, node count, registration date
- **Namespace listing** — All namespaces in the cluster with deployment counts
- **Deployment listing** — All deployments in the cluster
- **Node overview** — Node resource utilization snapshots
- **Agent status** — Agent version, last heartbeat, connectivity history

## Token Management

### Rotating a Token

Token rotation updates the cluster's authentication credential without re-registering:

1. Go to the cluster detail page
2. Click **Rotate Token**
3. Confirm the rotation
4. Copy the new token
5. Update the agent's configuration (Helm chart or Kubernetes Secret) with the new token

The old token is invalidated immediately upon rotation.

### Token Security

- Tokens are scoped to a single cluster
- Tokens cannot access data from other clusters or organizations
- Tokens should be treated as sensitive credentials
- Store tokens in Kubernetes Secrets (the Helm chart does this automatically)

## Managing Clusters

### Editing a Cluster

You can update a cluster's name, description, and provider label from the cluster detail page. Changing these does not affect the agent or connectivity.

### Removing a Cluster

To remove a cluster from Arguz:

1. Uninstall the agent from the cluster: `helm uninstall arguz-agent -n arguz-agent`
2. From the cluster detail page in Arguz, click **Delete Cluster**
3. Confirm the deletion

!!! warning
    Deleting a cluster removes it from the Arguz platform. Historical deployment and observability data associated with the cluster will no longer be accessible. Consider exporting data before deletion if you need to retain records.

## Projects

Projects are organizational groupings that contain clusters and namespaces. They help you organize your infrastructure logically:

- **By environment**: Production, Staging, Development
- **By team**: Platform, Payments, Frontend
- **By region**: US, EU, APAC

A cluster belongs to exactly one project. Namespaces within a cluster are scoped to the cluster's project.

## Namespaces

Namespaces within a cluster are discovered automatically by the agent and appear in the Arguz interface. You can:

- **Browse** namespaces per cluster
- **Filter** deployments by namespace
- **Search** for namespaces by name across clusters
- **Scope alert policies** to specific namespaces

## Node Overview

The **Nodes** page provides a view of node resource utilization across your clusters:

- Node count per cluster
- CPU utilization per node
- Memory utilization per node
- Node conditions (Ready, MemoryPressure, DiskPressure)

Node data is captured via periodic snapshots by the Arguz Agent. Snapshot frequency is configurable.

## Multi-Cluster Search

The search functionality in Arguz works across all clusters in your organization:

- **Image search** — Find all deployments using a specific container image
- **Service search** — Find services by name across clusters
- **Dependency search** — Find services that depend on a specific service

## Integration with CI/CD

Clusters in Arguz integrate with CI/CD tools to provide deployment tracking context:

- **ArgoCD** — Link deployments to ArgoCD applications
- **GitHub** — Correlate deployments with Git commits and pull requests

See the [Integrations](../integrations/index.md) section for setup details.
