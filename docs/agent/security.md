# Agent Security

This document describes the security model of the Arguz Agent, including how it authenticates, what access it requires, and privacy considerations.

## Security Design Principles

The Arguz Agent is built on these security principles:

1. **Least Privilege** — The agent requires only read access to Kubernetes API resources
2. **No Modification** — The agent never modifies workloads, configurations, or cluster state
3. **Encryption in Transit** — All communication with the Arguz platform uses HTTPS (TLS 1.2+)
4. **Token-Based Authentication** — Unique per-cluster tokens, not shared credentials
5. **No Persistent Local Storage** — The agent does not buffer or cache data to disk
6. **Container Hardening** — Non-root user, read-only root filesystem, no privileged access

## Authentication

### Cluster Token

Each cluster registered with Arguz receives a unique **Cluster Token**. This token:

- Is generated during cluster registration in the Arguz web app
- Is unique to that cluster — no two clusters share a token
- Is stored in a Kubernetes Secret in your cluster
- Is sent as the `x-cluster-token` HTTP header on every API request
- Can be rotated at any time from the Clusters management page
- Cannot be used to access data from other clusters or organizations

### Token Validation

The Arguz platform validates tokens using:

- **Constant-time comparison** to prevent timing attacks — the comparison takes the same amount of time regardless of whether the token matches
- **Parameterized database queries** to prevent SQL injection
- **Cache-based validation** — valid tokens are cached for 60 seconds, reducing database load while allowing prompt revocation

### Token Rotation

To rotate a cluster token:

1. Go to **Clusters** in the Arguz web app
2. Click on the cluster you want to rotate
3. Click **Rotate Token**
4. Copy the new token
5. Update the Kubernetes Secret in your cluster or redeploy the agent Helm chart with the new token

Old tokens are invalidated immediately upon rotation. The agent will reconnect automatically with the new token.

## Network Security

### TLS

All communication between the agent and the Arguz platform uses HTTPS (TLS 1.2 or higher). The platform enforces TLS on all public endpoints.

### Egress Only

The agent requires only **outbound** HTTPS connectivity (port 443). It does not expose any ports or accept inbound connections. No ingress rules are needed for the agent.

### DNS Resolution

The agent connects to the Arguz platform via public DNS hostnames:

- `api-discover.arguz.io`
- `di-api.arguz.io`

These resolve to load balancer IPs managed by the Arguz platform.

## Container Security

The agent container is hardened following Kubernetes security best practices:

| Control | Configuration |
|---|---|
| User | Non-root (UID not 0) |
| Root Filesystem | Read-only (`readOnlyRootFilesystem: true`) |
| Privilege Escalation | Disabled (`allowPrivilegeEscalation: false`) |
| Capabilities | All dropped (`capabilities.drop: [ALL]`) |
| Host Network | Not used |
| Host PID | Not used |
| Privileged | No |

## Data Privacy

### Data in Transit

All data sent from the agent to the Arguz platform is encrypted in transit via HTTPS (TLS 1.2+).

### Data at Rest

Observability data (logs, events, metrics) is stored in cloud object storage with encryption at rest provided by the cloud provider (GCS, Azure Blob, or S3). Metadata is stored in encrypted databases.

### What Data Leaves Your Cluster

The agent sends only **metadata and telemetry** to the Arguz platform. Specifically:

- Kubernetes resource names, labels, and annotations
- Container image references (names and tags)
- Deployment and pod status information
- HPA configuration parameters
- Node resource utilization metrics
- Error events and counts
- Cluster connectivity information

### What Data Does NOT Leave Your Cluster

The agent does **not** send:

- Application source code
- Container image contents
- Kubernetes Secret values
- ConfigMap data values
- PersistentVolume contents
- Application business data (user records, transactions, etc.)
- ServiceAccount tokens (the agent uses its own, which never leaves the cluster)
- Pod logs (logs are captured by a separate observability agent if configured)

### Data Isolation

Data from each cluster is scoped to its organization within the Arguz platform. Cross-organization data access is prevented by:

- Organization-level data partitioning
- Server-side authorization checks on every API call
- Token-scoped data access (a cluster token can only access its own cluster's data)

## Incident Response

### If a Token Is Compromised

1. Immediately rotate the token from the Arguz web app
2. Update the agent configuration with the new token
3. Review recent data submissions in the platform for anomalies
4. Contact Arguz support if you need assistance

### Agent Crashes or Restarts

If the agent pod crashes:

1. Kubernetes restarts the pod automatically
2. A new leader is elected (if running multiple replicas)
3. The agent performs a fresh sync of cluster state
4. No data is lost — the agent is stateless and re-syncs from the Kubernetes API

### Connectivity Loss

If the agent loses connectivity to the Arguz platform:

1. The agent continues watching Kubernetes resources
2. Data is not buffered (agent is stateless) — it's sent when connectivity is restored
3. The Arguz dashboard shows the cluster as disconnected after the heartbeat threshold
4. No alerts are triggered based on agent disconnection (this is configurable in alert policies)

## Compliance Considerations

- The agent does not process or access **customer data** (as defined by GDPR, CCPA, etc.) from your applications
- The agent collects **operational metadata** about your Kubernetes infrastructure
- Data residency is configurable per organization — each organization can choose its storage backend region
- The agent is **SOC 2 aware** — it operates with minimal permissions and does not persist data locally

## Reporting Security Issues

If you discover a security vulnerability, please report it to the Arguz security team through your customer support channel. Do not disclose potential vulnerabilities publicly.
