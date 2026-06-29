# Required Permissions

The agent chart needs read access to cluster resources for discovery, plus HPA update permissions when the Scaling Rules Agent is enabled.

## Discovery Agent permissions

Read access is required for:

- Deployments
- ReplicaSets
- Pods
- Jobs
- CronJobs
- HPAs
- Namespaces
- Services
- Nodes
- Ingresses
- ConfigMaps
- selected RBAC resources for metadata-only discovery

## Scaling Rules Agent permissions

The Scaling Rules Agent needs to:

- read HPAs
- patch HPAs
- restore previous HPA values after rule expiration or disablement

## Recommended approach

Install the official chart and keep the provided RBAC manifests aligned with the chart version. Avoid hand-editing permissions unless you are intentionally narrowing the resource scope.

## Practical validation

After installation, confirm that:

- deployments and namespaces sync correctly
- node snapshots appear in the frontend
- CronJobs show schedules and executions
- scaling templates can be applied and reverted
