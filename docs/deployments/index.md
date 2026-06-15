# Deployments

Deployments are the central abstraction in Arguz. Every time a Kubernetes Deployment changes in your cluster, Arguz captures a new **revision** — creating a permanent, searchable history of what ran, when, and with what effect.

## Understanding Deployments in Arguz

A Deployment in Arguz corresponds to a Kubernetes Deployment resource in your cluster. Arguz enriches it with:

- **Revision History** — Every change captured with container images, timestamps, and status
- **Error Correlation** — Errors detected after a deployment are linked to the revision
- **HPA Snapshot** — Autoscaling configuration at the time of deployment
- **Service Context** — The namespace, cluster, and project the deployment belongs to

## Browsing Deployments

Navigate to **Deployments** in the sidebar to see all deployments across your clusters.

The deployment list shows:

| Column | Description |
|---|---|
| **Service** | The deployment (service) name |
| **Namespace** | Kubernetes namespace |
| **Cluster** | Cluster where the deployment runs |
| **Project** | Project grouping |
| **Status** | Current health status |
| **Version** | Latest revision version identifier |
| **Deployed At** | Timestamp of most recent deployment |

You can filter deployments by:

- **Project** — Show deployments in a specific project
- **Cluster** — Show deployments in a specific cluster
- **Namespace** — Show deployments in a specific namespace
- **Search by name** — Free-text search for deployment names
- **Status** — Filter by health status
- **HPA configured** — Only show deployments with autoscaling

## Deployment Detail View

Click on any deployment to open its detail page. This is the **Service 360** view, providing comprehensive visibility into that service.

### Overview Tab

The Overview tab gives you a quick health summary:

- **Deployment timeline** — When each revision was deployed
- **Active errors** — Errors currently affecting this deployment
- **Recent alerts** — Alerts triggered for this service
- **Status history** — Health status over time

### Logs Tab

Search, filter, and analyze application logs from this deployment. Supports:

- Free-text search with pattern matching
- Time range filtering
- Severity level filtering
- Log pattern discovery (group similar log lines)
- Structured JSON log field filtering

### Patterns Tab

Arguz automatically extracts log patterns to reduce noise. The Patterns tab shows the most frequent log patterns for this service, helping you:

- Identify common error messages
- Spot unusual patterns after a deployment
- Filter logs by pattern for focused analysis

### Events Tab

View application events associated with this service:

- HTTP requests (method, path, status code, latency)
- Database queries (type, operation, latency)
- Cloud service interactions (GCP services)
- Redis and MongoDB operations

Filter by event type, status code, path, and time range.

### Metrics Tab

Time-series metrics for the deployment:

- CPU usage
- Memory usage
- Network I/O
- Custom application metrics
- HPA-related metrics (current vs target utilization)

Metrics can be viewed with configurable time windows and aggregation methods.

### Outbound Dependencies Tab

Shows all services that this deployment communicates with:

- Service name and namespace
- Protocol type (HTTP, gRPC, database, etc.)
- Request volume
- Error rate to each dependency
- Average latency

This is critical for understanding the impact radius of changes.

### Used By Tab (Reverse Dependencies)

Shows all services that depend on this deployment — the **blast radius** view. When this service has issues, these are the downstream services that will be affected. Essential for incident response prioritization.

## Deployment Search

The deployment search feature (also available via the MCP Server) allows complex queries:

- Find all deployments using a specific container image or tag
- Find deployments with HPA configured
- Find deployments by name, namespace, or cluster
- Find deployments by status
- Exclude system namespaces from results

## Deployment Status

Arguz tracks the following deployment statuses:

| Status | Meaning |
|---|---|
| **Healthy** | All replicas available, no recent errors |
| **Degraded** | Some replicas unavailable or errors detected |
| **Failed** | Deployment failed to roll out |
| **Unknown** | Status cannot be determined (agent disconnected) |

## Deployment and Error Correlation

When a deployment revision is captured, Arguz monitors for errors in a post-deployment window. If errors are detected, they are linked to the specific revision, enabling:

- **"Did this deployment cause problems?"** — Direct correlation between change and effect
- **Rollback confidence** — Compare error rates before and after a deployment
- **Change impact analysis** — Understand the full scope of a deployment's impact

## Container Image Tracking

Every deployment revision captures the container images in use. This enables:

- **Image search** — Find all deployments using a specific image or tag
- **Vulnerability correlation** — Quickly identify affected deployments when a CVE is announced
- **Image drift detection** — Spot deployments running unexpected image versions

## HPA Integration

For deployments with Horizontal Pod Autoscalers configured, Arguz captures:

- HPA configuration at each revision (min/max replicas, target metrics)
- Current HPA status (current vs target utilization)
- Scaling event history (scale-up and scale-down events)
- HPA audit trail for governance

## Exporting Deployment Data

Deployments can be exported from the web application in standard formats for reporting and compliance.
