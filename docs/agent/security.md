# Security Model

Arguz is designed so cluster-side components remain narrowly scoped and auditable.

## Cluster credentials

- one token per cluster
- shared Kubernetes Secret for the bundled agents
- rotation supported without re-registering the cluster

## Transport security

- HTTPS for agent-to-platform communication
- server-side proxying from the web applications
- no internal API tokens exposed to browsers

## Manifest sanitization

When Arguz stores manifest-derived metadata:

- sensitive values are redacted
- resource identities remain visible

This includes preserving names such as:

- Secret names
- ConfigMap names
- workload names
- namespaces

That balance keeps troubleshooting practical without leaking secret payloads.

## Operational boundaries

- Discovery Agent does not mutate workloads
- Scaling Rules Agent only changes HPA state within its rule scope
- rollback data is used to restore previous HPA values when required
