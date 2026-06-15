# Limitations & Scope

This document clearly defines what the Arguz Agent **does NOT** capture or do. Understanding these boundaries helps you set appropriate expectations and integrate Arguz effectively with your existing toolchain.

## What the Agent Does NOT Capture

### Source Code

The agent does **not** capture, store, or transmit any application source code. It does not access Git repositories, source files, or code artifacts.

### Container Image Contents

The agent captures container image **references** (registry, repository name, and tag) — not the image contents. It does not pull, scan, or inspect container images.

### Secret Values

The agent does **not** capture the values of Kubernetes Secrets. If your deployment references a Secret, only the fact that a Secret is referenced may be noted — never the Secret's data.

### ConfigMap Data

The agent captures ConfigMap **names** and **creation events**, but does **not** capture the data within ConfigMaps. Your configuration data stays in your cluster.

### PersistentVolume Contents

The agent does not access or capture data from PersistentVolumes, PersistentVolumeClaims, or any storage volumes.

### Application Business Data

The agent does not capture application-level business data — user records, financial transactions, personal information, or any data processed by your workloads.

### Pod Exec Output

The agent does **not** execute commands inside pods or capture the output of running processes. It only observes pod status and lifecycle events.

### Network Packet Payloads

The Discovery Agent does not capture network traffic. The separate observability agent (eBPF/sidecar) captures **metadata** about network connections (protocols, endpoints, latency, status codes) but does **not** capture packet payloads or request bodies.

### Arbitrary Log Files

The Discovery Agent does not read pod logs from disk. Application log collection (when enabled) is handled by a separate observability agent that streams logs from the container runtime — not from filesystem access.

## What the Agent Does NOT Do

### Does Not Modify Workloads

The agent never modifies your Deployments, Pods, Services, or any other workload resources. It is strictly read-only with respect to workloads.

### Does Not Execute Changes

The agent does not execute commands, apply configurations, or trigger actions in your cluster. It observes and reports — nothing more.

### Does Not Alter Traffic

The agent does not inject itself into the network path, modify traffic, add sidecars, or change routing behavior.

### Does Not Enforce Policies

The agent does not enforce Kubernetes policies or admission control. It reports state to the Arguz platform, where policies are evaluated — but enforcement happens in the platform (alerts, notifications, freeze windows), not in the agent.

### Does Not Require Cluster Admin

The agent does not require cluster-admin privileges. It operates with a scoped set of read permissions (see [Required Permissions](permissions.md)).

### Does Not Use Privileged Containers

The agent runs as a standard, non-privileged container. It does not require `privileged: true`, host network, host PID, or any elevated capabilities.

### Does Not Modify RBAC

The agent does not create, modify, or delete RBAC resources in your cluster (with the exception of the Lease used for leader election, which is standard practice).

## Architectural Limitations

### Kubernetes API Dependency

The agent depends on the Kubernetes API server being available. If the API server is unreachable, the agent cannot detect changes. The agent does not buffer events locally — it re-syncs from the API when connectivity is restored.

### Cluster Scope

Each agent instance monitors a single Kubernetes cluster. To monitor multiple clusters, install the agent in each cluster separately.

### Polling-Based Observation

The agent uses Kubernetes Informers (long-polling watches), not real-time push notifications from the API server. There may be a small delay (typically under 1 second) between a resource change and its detection by the agent.

### No Cross-Cluster Correlation in the Agent

The agent does not correlate data across clusters. Cross-cluster correlation and dependency mapping happen in the Arguz platform, not in the agent.

## Functional Boundaries

### No Alerting from the Agent

The agent does not trigger alerts. Alert policies are evaluated by the Arguz Notification Engine on the platform side. The agent provides the data foundation but is not part of the alerting pipeline.

### No Metric Threshold Evaluation in the Agent

The agent does not evaluate metric thresholds or trigger scaling actions. Threshold evaluation and scaling rule management happen on the platform side.

### No User Authentication

The agent does not authenticate human users. It uses machine-to-machine authentication (cluster token). User authentication is handled by the Arguz web application.

## Observability Agent Limitations

The separate observability (eBPF/sidecar) agent has additional limitations:

### Sampling

In high-volume log scenarios, the platform may apply **adaptive sampling** to reduce data volume. This means not every log line may be preserved. Sampling preserves:

- 100% of unique/new log patterns
- A configurable percentage of repeating patterns

Exact counts are preserved through extrapolation, so metric accuracy is not affected.

### Exclusion Filters

You can configure **exclusion filters** to prevent specific types of data from being ingested. For example:

- Exclude health probe requests
- Exclude specific namespaces
- Exclude log patterns matching a regex

Data matching exclusion filters is discarded before ingestion and is not stored or queryable.

### Resource Limits

Observability data collection has resource limits to protect platform stability:

- Maximum 2 MiB per request payload
- Rate limiting per cluster (default 10,000 requests per minute)
- Backpressure when storage systems are degraded (returns 503)
- Maximum log message size (configurable, default 1 MB)

## Customizing Scope

You can configure what the agent observes:

| Configuration | Effect | File |
|---|---|---|
| Excluded namespaces | Namespaces not monitored | `config.yaml` — `ignore_namespaces` |
| Excluded resources | Resource types ignored | `config.yaml` — `ignore_resources` |
| Excluded node labels | Labels not captured in snapshots | `config.yaml` — `ignore_node_labels` |
| Exclusion filters | Data not ingested by observability pipeline | Configured via API/web app |
| Log sampling rate | Percentage of repeated log lines preserved | Managed automatically by platform |

## Summary

| Capability | Supported? |
|---|---|
| Track deployments | Yes |
| Track pod errors (CrashLoopBackOff, OOMKilled) | Yes |
| Track HPA configurations | Yes |
| Track namespace and service topology | Yes |
| Capture node resource utilization | Yes |
| Capture Helm release metadata | Yes |
| Capture application logs (separate agent) | Yes |
| Capture application events (separate agent) | Yes |
| Capture application metrics (separate agent) | Yes |
| Capture source code | No |
| Capture Secret data | No |
| Capture ConfigMap data | No |
| Capture network packet payloads | No |
| Modify workloads | No |
| Execute cluster changes | No |
| Enforce policies at admission | No |
| Require cluster admin | No |
| Require privileged containers | No |
