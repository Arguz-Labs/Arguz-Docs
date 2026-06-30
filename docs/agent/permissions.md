# Required Permissions

The public chart intentionally splits permissions by agent. Discovery is mostly read-only. Scaling needs mutation rights for HPAs and some Deployment replica updates.

## Discovery Agent RBAC scope

The current chart grants the Discovery Agent:

- `get`, `list`, `watch` on `namespaces`
- `get`, `list`, `watch` on `nodes`
- `get`, `list`, `watch` on `pods` and `pods/log`
- `get`, `list`, `watch` on `services`
- `get`, `list`, `watch` on `configmaps`
- `get`, `list`, `watch` on `secrets`
- `get`, `list`, `watch` on `events`
- `get`, `list`, `watch` on `deployments` and `replicasets`
- `get`, `list`, `watch` on `horizontalpodautoscalers`
- `get`, `list`, `watch` on `jobs` and `cronjobs`
- `get`, `list`, `watch` on `ingresses` and `networkpolicies`
- `get`, `list`, `watch` on selected RBAC objects
- `get`, `list`, `watch`, `create`, `update`, `patch` on `leases` for leader election

### Why `pods/log` access exists

The Discovery Agent reads failed Job pod logs so Arguz can attach failure context to CronJob execution history. It does not archive full cluster logs.

## Scaling Rules Agent RBAC scope

The current chart grants the Scaling Rules Agent:

- `get`, `list`, `watch` on `pods`
- `get`, `list`, `watch`, `update`, `patch` on `deployments`
- `get`, `list`, `watch`, `create`, `update`, `patch`, `delete` on `horizontalpodautoscalers`

### Why Deployment mutation is required

The Scaling Rules Agent may raise or restore Deployment replica counts while applying or reverting an HPA-based scaling window. This keeps the workload aligned with the requested minimum or the stored baseline.

## Permission boundaries

- Discovery does not need permission to mutate workloads.
- Scaling does not need broad inventory permissions beyond the resources involved in HPA execution.
- The chart is the recommended source of truth for RBAC because the exact permissions are tied to the current supported agent behavior.

## Practical validation after install

After installing the chart, validate both permission sets by confirming that:

- namespaces and Deployments appear in Arguz
- node snapshots and CronJob executions populate correctly
- scaling templates can apply and revert without `Forbidden` errors on HPAs or Deployments
