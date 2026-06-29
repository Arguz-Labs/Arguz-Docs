# Installation

Arguz installation has two parts:

1. Register the cluster in the **Admin Console**
2. Install the **Arguz Agent Helm chart** in that cluster

The current public chart bundles both in-cluster components:

- `Discovery-Agent`
- `Scaling-Rules-Agent`

## 1. Register the cluster

From `app-admin.arguz.io`:

1. Open the target organization and project
2. Go to **Clusters**
3. Create a new cluster record
4. Copy:
   - `PROJECT_ID`
   - `CLUSTER_ID`
   - `CLUSTER_TOKEN`

These values are stored in the shared chart credentials secret.

## 2. Install the chart

```bash
helm repo add arguz-agent https://Arguz-Labs.github.io/Arguz-Agent-Chart
helm repo update
helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  --version 0.4.0 \
  -n arguz-agent \
  --create-namespace \
  --set-string global.credentialsSecretName=arguz-credentials \
  --set-string global.clusterCredentials.projectId=<PROJECT_ID> \
  --set-string global.clusterCredentials.clusterId=<CLUSTER_ID> \
  --set-string global.clusterCredentials.clusterToken=<CLUSTER_TOKEN>
```

## Optional settings

### Disable the scaling rules agent

```bash
helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  -n arguz-agent \
  --create-namespace \
  --set Scaling-Rules-Agent.enabled=false
```

### Scaling Rules Agent defaults

```yaml
Scaling-Rules-Agent:
  enabled: true
  env:
    API_URL: https://api-scaling-rules.arguz.io
    LOG_LEVEL: info
    IGNORE_RESOURCES: ""
```

## What the agents do after installation

### Discovery Agent

- Sends heartbeats
- Syncs namespaces, deployments, images and revisions
- Captures node snapshots
- Detects cluster cloud metadata
- Syncs CronJobs and Job executions
- Stores the last 100 lines of failed job logs in the platform

### Scaling Rules Agent

- Watches active scaling templates
- Applies temporary HPA changes
- Reverts them when the schedule ends
- Reverts them again if the rule is disabled before expiration

## Verify a healthy install

```bash
kubectl get pods -n arguz-agent
kubectl logs deploy/arguz-agent-discovery-agent -n arguz-agent
```

Look for:

- leader election success
- namespace and deployment synchronization
- node snapshot loop startup
- cluster metadata heartbeat
- CronJob synchronization

## Provider metadata detection

Arguz attempts to detect provider context from cloud metadata and Kubernetes labels. It currently supports:

- GKE
- EKS
- AKS
- On-prem or unknown environments

If metadata endpoints are restricted, the agent falls back to node labels when possible.
