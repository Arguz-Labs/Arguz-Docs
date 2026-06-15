# Data Collection

This document describes what data the Arguz Agent captures from your Kubernetes cluster. Understanding this helps you evaluate the agent's scope and ensure it aligns with your organization's data policies.

## Categories of Data Collected

### 1. Deployment Metadata

When a Kubernetes Deployment is created, updated, or deleted, the agent captures:

- Deployment name and namespace
- Container images and tags (all containers in the Pod spec)
- Replica count (desired and available)
- Deployment strategy (RollingUpdate, Recreate)
- Labels and annotations (metadata identifying the workload)
- Timestamp of the change

This data enables the deployment revision history and allows you to trace which images were running at any point in time.

### 2. Pod Lifecycle Events

The agent monitors pods for error states and significant lifecycle events:

- **CrashLoopBackOff** — Pod is crashing repeatedly
- **OOMKilled** — Pod was killed due to out-of-memory
- **Restart counts** — Number of container restarts
- **Pod phase transitions** — Pending, Running, Succeeded, Failed

This information feeds into the error detection system and helps identify problematic deployments.

### 3. HPA (Horizontal Pod Autoscaler) Configuration

When HPA configurations change, the agent captures:

- API version of the HPA resource
- Minimum and maximum replicas
- Metric specifications (CPU utilization, memory, custom metrics)
- Scaling behavior configuration (scale-up and scale-down policies)
- Current metrics status at the time of capture

This data enables the Scaling Rules management feature and provides HPA history for audit purposes.

### 4. Namespace Discovery

When new namespaces are created, the agent records:

- Namespace name
- Labels and annotations
- Creation timestamp

This keeps the Arguz platform synchronized with your cluster's namespace topology.

### 5. Service Discovery

When Kubernetes Services are created, the agent captures:

- Service name and namespace
- Service type (ClusterIP, NodePort, LoadBalancer)
- Port configurations
- Selector labels

### 6. RBAC Resources

For security visibility, the agent notes the creation of:

- Roles and RoleBindings
- ClusterRoles and ClusterRoleBindings

The agent captures resource names and creation timestamps — not the actual permission rules.

### 7. Additional Resources

The agent tracks creation events for:

- **ConfigMaps** — Name and namespace
- **Jobs** — Name, namespace, and creation timestamp
- **Ingresses** — Name, namespace, and host rules
- **NetworkPolicies** — Name, namespace, and creation timestamp

### 8. Node Resource Snapshots

Periodically, the agent captures a snapshot of node resource utilization:

- Node name
- Allocatable and capacity for CPU, memory, and storage
- Node conditions (Ready, MemoryPressure, DiskPressure)
- User-configured node labels (configurable filter)

### 9. Helm Release Metadata

If your deployments are managed by Helm, the agent extracts release metadata from Helm Secrets in the cluster:

- Release name and chart version
- Revision number
- Last deployed timestamp

### 10. Cluster Metadata Heartbeat

At regular intervals, the agent sends a heartbeat to the Arguz platform containing:

- Kubernetes version
- Cloud provider information
- Agent version and health status
- Total node count

This confirms cluster connectivity and provides platform-level context.

## How Data Is Collected

The agent uses the **Kubernetes Informer pattern** — a standard, efficient mechanism for watching API resources. Informers:

- Establish a watch connection to the Kubernetes API server
- Receive incremental updates when resources change
- Maintain an in-memory cache for efficiency
- Only report changes, not full resource snapshots, after initial sync

## Data Transmission

Collected data is sent to the Arguz platform via HTTPS POST requests to `https://api-discover.arguz.io`. Each request is:

- Compressed (where applicable)
- Batched for efficiency
- Authenticated with the cluster token

The agent does **not** buffer data to disk — all data is transmitted in near real-time.

## Configuration Options

You can control what the agent collects through its configuration:

| Option | Effect |
|---|---|
| `ignore_namespaces` | Exclude specified namespaces from all collection |
| `ignore_resources` | Exclude resources matching specified globs (e.g., `job/*`) |
| `ignore_node_labels` | Exclude specified node labels from snapshots |

Example configuration:

```yaml
log_level: info
ignore_namespaces:
  - kube-system
  - cert-manager
  - monitoring-stack
ignore_resources:
  - job/*
  - CronJob/*
ignore_node_labels:
  - beta.kubernetes.io/arch
```

## Data Retention in the Agent

The agent is **stateless** — it does not store historical data locally. All data is transmitted to the Arguz platform upon collection. If the agent loses connectivity to the platform, it continues collecting and retries transmission when connectivity is restored.

## Observability Agent Telemetry (Separate Component)

In addition to the Discovery Agent, Arguz supports separate observability agents (eBPF-based or sidecar) that capture deeper telemetry from your workloads:

### Application Events

- HTTP request/response metadata (method, path, status code, latency)
- Database connections and queries (type, operation, latency)
- Redis and MongoDB operations
- Google Cloud service interactions (Pub/Sub, Firestore, BigQuery, Cloud Storage, Secret Manager, Cloud Tasks, Vertex AI)

### Error Events

- HTTP 5xx responses
- TCP connection resets
- Out-of-memory (OOM) kills
- Application-level errors

### Logs

- Application log lines (text and structured JSON)
- Log severity levels
- Automatic log pattern extraction for noise reduction
- Adaptive sampling for high-volume log patterns

### Metrics

- Kubernetes pod metrics (CPU, memory, network, storage)
- Kubernetes node metrics
- Container-level metrics
- Custom application metrics

These observability agents send data to the **Data Ingestion API** (`di-api.arguz.io`) rather than the Control Plane API.

## Data Processing

Once data reaches the Arguz platform:

1. Data is validated and authenticated (cluster token verification)
2. Observability data is persisted to cloud object storage (Parquet format)
3. Metadata is stored in a relational database
4. Log patterns are extracted for noise reduction
5. Metrics are pre-aggregated for dashboard performance
6. Exclusion filters are applied to drop noisy or unwanted data

## What Is NOT Collected

For a clear list of what the agent does not collect, see [Limitations & Scope](limitations.md).
