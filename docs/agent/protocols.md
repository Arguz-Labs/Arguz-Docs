# Communication Protocols

Arguz agents communicate with the platform over HTTPS using cluster-scoped credentials.

## Authentication

- header-based cluster token authentication
- one token per registered cluster
- token rotation supported from the Admin Console

## Main request families

| Flow | Destination | Purpose |
|---|---|---|
| Inventory sync | `api-discover.arguz.io` | deployments, namespaces, revisions, images, CronJobs, nodes |
| Cluster heartbeat | `api-discover.arguz.io` | cluster health and cloud metadata |
| Scaling reconciliation | `api-scaling-rules.arguz.io` | active rules, rollback state and execution updates |

## Transmission model

- JSON payloads
- batched submission when possible
- near real-time change reporting
- retry behavior on transient failures

## Cloud metadata lookups

Before sending cluster metadata, the Discovery Agent may call:

- GCP metadata endpoints
- AWS IMDS
- Azure metadata endpoints
- Kubernetes node labels as a fallback source

## Frontend access pattern

The browser does not call the platform internals directly. `app.arguz.io` and `app-admin.arguz.io` proxy the requests server-side and enforce user scope there.
