# Frequently Asked Questions

## General

### What is Arguz?

Arguz is a Kubernetes Deployment Governance and Observability Platform. It tracks deployments, monitors for errors, provides AI-powered root cause analysis, and helps you enforce deployment policies — all from a single dashboard.

### Is Arguz a SaaS or self-hosted product?

Arguz is delivered as a SaaS platform. You install a lightweight agent in your Kubernetes clusters, and all data processing, storage, and analysis happens in the Arguz platform.

### What Kubernetes versions are supported?

Kubernetes v1.24 and later.

### Which cloud providers does Arguz support?

Arguz is cloud-agnostic. The agent works on any Kubernetes cluster regardless of the underlying infrastructure — GKE, EKS, AKS, OpenShift, or on-premises.

---

## Agent

### What does the agent install in my cluster?

A single Deployment with 2 replicas (for high availability) using a lightweight container (~128Mi memory, 100m CPU). The agent watches Kubernetes API resources and reports changes to Arguz. It does not inject sidecars or modify workloads.

### Does the agent require privileged access?

No. The agent runs as a non-root user with a read-only root filesystem and no privileged capabilities. It requires read-only access to specific Kubernetes API resources.

### Does the agent capture my application's source code?

No. The agent captures metadata about your deployments (resource names, container image references, status) — never source code, Secret values, or application data.

### Does the agent capture network traffic payloads?

No. The agent captures metadata about network connections (protocols, endpoints, latency, status codes) but not packet payloads or request bodies.

### Can I control what the agent monitors?

Yes. You can configure:
- Namespaces to ignore (`kube-system`, monitoring tools, etc.)
- Resource types to ignore (e.g., `job/*`)
- Node labels to exclude from reports

### How does the agent affect cluster performance?

The agent is designed to be minimal. It uses:
- ~128Mi memory per replica
- 100m CPU per replica (limit)
- Kubernetes Informer pattern (efficient watch-based observation)
- Leader election (only one active replica at a time)

### What happens if the agent disconnects?

The agent continues watching Kubernetes resources. It reconnects and re-syncs when connectivity is restored. The Arguz dashboard shows the cluster as disconnected. No data is lost — the agent is stateless and re-syncs from the Kubernetes API.

---

## Features

### How is this different from Datadog / Grafana / New Relic?

Arguz is purpose-built for **Kubernetes deployment governance**. While traditional observability tools focus on metrics, logs, and traces, Arguz adds the deployment layer — tracking every change, correlating errors with deployments, and providing AI-powered RCA specifically focused on deployment-related issues.

### Can I export my data?

Yes. Observability data (logs, metrics, events) can be exported from the web application.

### How long is data retained?

Data retention depends on your organization's subscription plan. Contact Arguz support for details on retention periods.

### Does Arguz support custom metrics?

Yes. The observability agent can ingest custom application metrics via the metrics API.

---

## Security

### Is my data encrypted?

Yes. All data is encrypted in transit (TLS 1.2+) and at rest (cloud provider encryption for object storage, encrypted databases).

### Can other organizations see my data?

No. Organizations are fully isolated. The platform enforces server-side authorization checks that prevent cross-organization data access.

### Where is my data stored?

Data storage location is configurable per organization. Arguz supports GCS, Azure Blob Storage, and AWS S3.

### How are cluster tokens managed?

- Tokens are generated when you register a cluster
- Tokens are unique per cluster
- Tokens can be rotated at any time
- Tokens are stored in a Kubernetes Secret in your cluster
- Tokens are validated using constant-time comparison to prevent timing attacks

---

## Troubleshooting {#troubleshooting}

### Agent pods are not starting

1. Check pod status: `kubectl get pods -n arguz-agent`
2. Check pod logs: `kubectl logs -n arguz-agent <pod-name>`
3. Verify the Cluster Token is correct
4. Verify the agent can reach `api-discover.arguz.io` on port 443
5. Check if the ServiceAccount has the required RBAC permissions

### Cluster shows as "Disconnected"

1. Check if the agent pods are running: `kubectl get pods -n arguz-agent`
2. Check agent logs for connection errors
3. Verify network connectivity to `api-discover.arguz.io`
4. Check if the cluster token is still valid (not rotated)
5. If the agent was recently upgraded, allow a few minutes for reconnection

### Deployments are not appearing

1. Verify the agent is connected (cluster status should be green)
2. Check that the namespace is not in the `ignore_namespaces` list
3. Verify the deployment type is not in the `ignore_resources` list
4. Allow a few minutes after agent startup for initial discovery
5. Check agent logs for any errors

### Notifications are not being delivered

1. Verify the notification channel is configured correctly (test it)
2. Check that the alert policy is enabled
3. Confirm the error conditions match the policy (error type, affected ratio, scope)
4. Check if the notification is within active hours/days for the channel
5. Verify throttling is not suppressing the notification

### I'm not seeing observability data (logs, events, metrics)

Observability data requires a separate observability agent (eBPF or sidecar-based) in addition to the Discovery Agent. Ensure:
- The observability agent is installed and configured
- It can reach `di-api.arguz.io` on port 443
- Exclusion filters are not filtering out the data you expect to see

---

## Billing

### How does pricing work?

Arguz offers tiered subscription plans based on **compute units (CUs)** — a measure of platform usage covering deployment tracking, observability data ingestion, and AI-powered features. Each plan includes a monthly CU quota with overage pricing for usage beyond the quota.

### What happens when my trial ends?

At the end of the trial period, access is restricted until a paid subscription is activated. Data is preserved for a limited period to allow for subscription activation.

### Can I upgrade or downgrade my plan?

Contact Arguz support to change your plan.

### How do I cancel my subscription?

Organization owners can cancel subscriptions from the billing settings page. Data retention after cancellation depends on your plan.
