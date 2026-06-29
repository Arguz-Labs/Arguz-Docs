# Limitations & Scope

This section describes the intentional boundaries of the current Arguz agent bundle.

## Discovery Agent limits

- node snapshots are not live runtime usage like `kubectl top`
- cloud metadata detection is best effort and depends on metadata or node labels being available
- CronJob execution history depends on Jobs being visible and not filtered out
- failed execution logs are stored as the last 100 lines, not full pod log archives
- excluded namespaces and resources are fully ignored by discovery

## Scaling Rules Agent limits

- only rules targeting supported HPA-backed workloads can be applied
- rollback depends on the previously stored HPA baseline
- external HPA changes made outside the rule window may alter the rollback target if they happen after the baseline is captured

## What the bundle does not do

- it does not proxy user traffic
- it does not store full secret values
- it does not expose long-lived local state
- it does not act as a cluster-wide observability pipeline configuration guide in this public documentation set
