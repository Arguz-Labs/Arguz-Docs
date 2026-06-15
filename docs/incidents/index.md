# Incidents & Errors

Arguz provides a comprehensive error tracking system that detects, correlates, and helps you resolve production incidents. Errors are linked to deployments, enabling fast identification of problematic changes.

## Error Detection

Errors are detected through multiple mechanisms:

### Agent-Detected Errors

The Arguz Discovery Agent detects pod-level errors from Kubernetes events:

- **CrashLoopBackOff** — A container is crashing repeatedly
- **OOMKilled** — A container was killed due to out-of-memory
- **Restart spikes** — Unusual container restart patterns

### Observability-Detected Errors

The observability agent detects application-level errors:

- **HTTP 5xx responses** — Server errors from your services
- **TCP connection resets** — Network-level failures
- **OOM events** — Out-of-memory events at the node level
- **Application errors** — Errors surfaced through logs or structured events

### Error Types

Arguz categorizes errors by type, making it easy to filter and alert on specific error classes:

- `CrashLoopBackOff`
- `OOMKilled`
- `HTTP 5xx`
- `TCP Reset`
- Custom error types from your applications

## Error List View

Navigate to **Errors** in the sidebar to see all detected errors:

| Column | Description |
|---|---|
| **Error Type** | Classification of the error |
| **Service** | Affected deployment |
| **Namespace** | Kubernetes namespace |
| **Cluster** | Affected cluster |
| **Severity** | Critical, High, Medium, or Low |
| **Affected Pods** | Number of pods affected / total pods |
| **Occurred At** | When the error was first detected |
| **Mitigated** | Whether the error has been resolved |

### Filtering Errors

Filter errors by:

- **Error type** — Show only specific error classes
- **Organization, Cluster, Namespace, Deployment** — Scope to specific resources
- **Revision** — Show errors associated with a specific deployment revision
- **Mitigated status** — Show active, mitigated, or all errors
- **Severity** — Filter by severity level
- **Time range** — Show errors from a specific period

## Error Detail

Click on any error to see its detail view:

- **Error message** — Detailed description of the error
- **Affected pods** — Which pods experienced the error
- **Affected ratio** — Percentage of pods affected
- **Occurred at** — Timestamp of first detection
- **Mitigated at** — When the error was resolved (if mitigated)
- **Associated revision** — The deployment revision this error is linked to
- **Service context** — Cluster, namespace, and project information

## Error and Deployment Correlation

When errors are detected after a deployment, they are linked to the specific revision. This enables:

1. **Identify the change that caused the error**: See which deployment introduced the problem
2. **Compare before and after**: Check if the error existed in the previous revision
3. **Track resolution**: Monitor whether errors are mitigated after a rollback or fix
4. **Calculate impact duration**: Time from first error to mitigation

## Active Errors View

The Active Errors view shows only errors that are currently unresolved. This is your real-time incident dashboard — showing what's broken right now across your infrastructure.

## Incident Lifecycle

```
Deployment Occurs
    │
    ▼
Agent Detects Errors
    │
    ▼
Errors Linked to Revision
    │
    ├──► Alert Policies Evaluated
    │       │
    │       ▼
    │    Notifications Sent (Slack, Teams, VictorOps)
    │
    ▼
RCA Available (if AI-enabled)
    │
    ▼
Team Investigates / Mitigates
    │
    ▼
Error Marked as Mitigated
    │
    ▼
Post-Incident Review (Revision History Available)
```

## Alert Policies for Errors

Alert policies define which errors should trigger notifications. See the [Policies](../policies/index.md) section for detailed configuration.

Key policy parameters:

- **Error type matching** — Alert only on specific error types
- **Affected ratio threshold** — Alert when > X% of pods are affected
- **Path/endpoint filtering** — Alert on specific API paths or endpoints
- **Scoping** — Limit alerts to specific projects, clusters, namespaces, or deployments
- **Silencing windows** — Suppress alerts during known maintenance periods
- **Delay** — Wait before alerting (avoid flapping from transient errors)

## Mitigation

Errors can be **mitigated** (marked as resolved) either:

1. **Automatically** — When the platform detects that the error condition has cleared (pods healthy, no new errors within a window)
2. **Manually** — Via the web application or API when an operator confirms resolution

Mitigated errors remain visible in the error history for audit and post-incident analysis.

## Error Search (MCP Server)

The Arguz MCP Server provides programmatic error search:

- `errors_search` — Flexible search across all errors
- `revision_errors_list` — Errors for a specific revision
- `revision_errors_active` — Quick summary of active errors
- `revisions_with_errors_list` — Find revisions with active errors

## Best Practices

1. **Configure alert policies for critical error types** — Catch problems before users report them
2. **Set appropriate affected ratio thresholds** — Avoid alert fatigue from minor transient errors
3. **Use error-to-revision correlation** — When triaging, check the deployment timeline first
4. **Review mitigated errors** — Periodically review error history to identify systemic issues
5. **Set up notification channels** — Ensure the right teams receive alerts promptly
