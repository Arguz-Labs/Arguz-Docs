# Revision History

Each time a Kubernetes Deployment changes, Arguz captures a **revision** — an immutable snapshot of the deployment at that point in time. Revisions form the backbone of deployment tracking and enable powerful capabilities like change impact analysis and rollback planning.

## What Is a Revision?

A revision records:

- **Container images and tags** — Exactly which images were running
- **Timestamp** — When the deployment change occurred
- **Status** — Whether the deployment succeeded, failed, or is in progress
- **Revision type** — Helm upgrade, kubectl apply, CI/CD triggered, etc.
- **HPA configuration** — Autoscaling settings at the time of deployment (if applicable)
- **Associated errors** — Errors that occurred after this revision was deployed

## Viewing Revisions

### From the Deployments List

Click on any deployment to open its Service 360 view, then click the **Revisions** tab to see the revision history timeline.

### Revision Timeline

Revisions are displayed chronologically, newest first:

| Field | Description |
|---|---|
| **Revision Number** | Sequential identifier within the deployment |
| **Deployed At** | When this revision was rolled out |
| **Status** | Success, Failed, or In Progress |
| **Images** | Container images and tags at this revision |
| **Errors** | Count of errors detected after this revision |
| **HPA** | Whether HPA was active at this revision |

### Revision Detail

Click on a specific revision to see:

1. **Full image list** — All containers with image references
2. **Error list** — All errors that occurred within the post-deployment monitoring window
3. **Error summary** — Active errors, mitigated errors, total errors
4. **HPA snapshot** — Full autoscaling configuration (min/max replicas, metrics, behavior)
5. **Timeline context** — What came before and after this revision

## Error Correlation

When a revision is captured, Arguz enters a monitoring window during which errors are associated with that revision. This enables:

### Change Impact Analysis

Compare error rates before and after a revision:

- Did error rates increase after this deployment?
- Did new error types appear?
- Which specific pods are affected?
- What's the affected-to-total pod ratio?

### Rollback Confidence

If a rollback is needed, the revision history shows:

- What the previous revision's images were
- Whether the previous revision had errors
- Whether rolling back is likely to resolve the issue

### Root Cause Analysis

Revisions are a key input to the [AI-powered RCA engine](../rca/index.md), which uses revision history along with error data, logs, and dependency information to identify probable causes.

## Searching Revisions

Revisions can be searched via:

- **Web Application**: Filter by deployment, namespace, cluster, organization, or revision ID
- **MCP Server**: Programmatic search for integration with AI tools and automation

Search parameters include:

| Parameter | Description |
|---|---|
| `organization_id` | Scope to an organization |
| `cluster_id` | Scope to a cluster |
| `namespace_id` | Scope to a namespace |
| `deployment_id` | Scope to a specific deployment |
| `revision_id` | Look up a specific revision |

## Revision Errors

### Active vs Mitigated Errors

Errors associated with a revision can be:

- **Active** — The error condition is still present (e.g., pods still crashing)
- **Mitigated** — The error condition has been resolved (e.g., pods recovered, rollback applied)

### Error Severity

| Severity | Meaning |
|---|---|
| **Critical** | Service is down or completely unavailable |
| **High** | Significant degradation affecting users |
| **Medium** | Partial degradation, limited impact |
| **Low** | Minor issue, no user impact |

### Affected Pods

For each error, Arguz tracks:

- How many pods are affected
- Total pod count for the deployment
- Affected ratio (affected / total)

This helps prioritize incidents — an error affecting 80% of pods is more urgent than one affecting 5%.

## Revision with Errors List

The platform provides a dedicated view showing only revisions that have active errors, enabling quick identification of problematic deployments across your organization.

## HPA History

Each revision captures the HPA configuration at that point in time, providing an audit trail of autoscaling changes:

- Who changed the HPA configuration?
- What were the min/max replicas at each revision?
- When did scaling metrics change?
- How has scaling behavior evolved over time?

## Helm Integration

If your deployments are managed by Helm, Arguz extracts Helm release metadata from the cluster's Helm Secrets:

- Release name
- Chart name and version
- Helm revision number
- Last deployed timestamp

This provides an additional layer of traceability for Helm-managed deployments.

## Retention

Revisions are retained for the duration of your organization's data retention period. Historical revisions remain queryable even after subsequent deployments.

## Export

Revision data can be exported for compliance, auditing, or integration with external systems.

## API Access (MCP Server)

The Arguz MCP Server provides read-only programmatic access to revisions:

- `revisions_list` — List revisions for a deployment
- `revisions_search` — Flexible search across revisions
- `revision_errors_list` — Get errors for a specific revision
- `revision_errors_active` — Quick summary of active errors for a revision
- `revisions_with_errors_list` — Find all revisions with active errors
