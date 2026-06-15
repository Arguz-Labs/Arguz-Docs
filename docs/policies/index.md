# Policies & Governance

Arguz provides a suite of policy and governance features that help you maintain control over deployments, autoscaling, and alerting across your Kubernetes infrastructure.

## Alert Policies

Alert policies define the conditions under which the platform sends notifications. They are the bridge between detected errors and your team's awareness.

### Creating an Alert Policy

1. Navigate to **Alert Policies** in the sidebar
2. Click **Create Alert Policy**
3. Configure the following:

| Setting | Description |
|---|---|
| **Name** | A descriptive name for the policy |
| **Error Types** | Which error types trigger this policy (CrashLoopBackOff, OOMKilled, HTTP 5xx, etc.) |
| **Min Affected Ratio** | Minimum percentage of pods that must be affected (e.g., 0.5 = 50%, 1.0 = 100%) |
| **Path Globs** | Filter by API path patterns (e.g., `/api/*`, `/checkout`) |
| **Scope** | Limit the policy to specific projects, clusters, namespaces, or deployments |
| **Notification Channels** | One or more channels (Slack, Teams, VictorOps) to notify |
| **Silencing Window** | Time window during which alerts are suppressed (e.g., maintenance windows) |
| **Delay** | Wait period before alerting (prevents flapping) |

### Policy Evaluation

Alert policies are evaluated by the Arguz Notification Engine, which runs as a background worker. When a new error is detected:

1. The error is matched against active alert policies
2. Matching policies trigger notifications to configured channels
3. Throttling prevents duplicate notifications for the same error

### Managing Policies

- **Edit**: Modify any policy parameter at any time
- **Enable/Disable**: Temporarily suppress a policy without deleting it
- **Delete**: Remove policies that are no longer needed
- **View History**: See which policies triggered and when

### Event Notification Policies

In addition to error-based alert policies, Arguz supports **event notification policies** that trigger on specific events in the platform (e.g., cluster registration, deployment events, configuration changes).

## Scaling Rules

Scaling Rules provide governance over Horizontal Pod Autoscaler (HPA) configurations across your clusters.

### What Are Scaling Rules?

Scaling rules track and manage HPA configurations for your Kubernetes Deployments. They provide:

- **Visibility**: See HPA configurations across all deployments
- **Audit Trail**: Track who changed HPA settings and when
- **Configuration History**: View historical HPA settings at each revision
- **Policy Enforcement**: Ensure HPA configurations comply with organizational standards

### Viewing HPA Configurations

From the Service 360 view for any deployment, the HPA section shows:

- API version of the HPA resource
- Minimum and maximum replicas
- Target metrics (CPU utilization, memory, custom metrics)
- Scaling behavior (scale-up and scale-down policies)

### Scaling Rule Audit Trail

Every change to an HPA configuration is tracked:

- What was changed (min/max replicas, metrics, behavior)
- When it was changed
- Which revision it is associated with

This audit trail is essential for compliance and incident investigation.

### Searching by HPA

You can search for deployments that have (or don't have) HPA configured:

- In the **Deployments** view, filter by `hpa_present: true`
- Via the **MCP Server**, use the `deployment_hpa` tool to retrieve HPA details

## Freeze Windows

Freeze windows allow you to define periods during which deployments are blocked — a critical governance feature for change management.

### Use Cases

- **Critical business periods**: Black Friday, holiday shopping season
- **Maintenance windows**: Scheduled infrastructure maintenance
- **Compliance periods**: Audit periods, quarter-end financial close
- **Team moratoriums**: Code freezes before major releases

### Creating a Freeze Window

1. Navigate to **Freeze Windows** (under Policies or Clusters)
2. Click **Create Freeze Window**
3. Configure:
    - **Name and description**: What is this freeze for?
    - **Start and end time**: When the freeze is active
    - **Scope**: Which projects, clusters, namespaces, or deployments are affected
    - **Recurrence**: One-time or recurring (e.g., every Saturday 02:00-06:00)

### Freeze Window Behavior

During an active freeze window:

- The platform tracks deployment attempts
- Depending on configuration, deployments may be blocked or flagged
- Notifications can alert administrators about attempted deployments during a freeze

## Policy Scoping

All policies (alert policies, freeze windows) support hierarchical scoping:

| Scope Level | Description |
|---|---|
| **Organization** | Policy applies to all projects and clusters |
| **Project** | Policy applies to all clusters within the project |
| **Cluster** | Policy applies to a specific cluster |
| **Namespace** | Policy applies to a specific namespace |
| **Deployment** | Policy applies to a specific deployment |

## Best Practices

### Alert Policies

1. Start with **broad alert policies** and refine over time
2. Set **realistic affected ratio thresholds** — too low = alert fatigue, too high = miss incidents
3. Use **delays** for transient errors that self-resolve
4. Configure **silencing windows** during known maintenance
5. **Test notifications** after creating or modifying policies

### Scaling Rules

1. **Review HPA configurations** regularly for cost optimization
2. Use the **audit trail** to understand who changed scaling parameters
3. Set **organizational standards** for min/max replicas
4. **Correlate scaling events** with incidents to identify capacity-related issues

### Freeze Windows

1. **Plan ahead** — create freeze windows before critical periods
2. **Communicate** freeze windows to all teams
3. **Use scoping** to apply freezes only where needed
4. **Monitor attempts** — understand what people tried to deploy during a freeze
