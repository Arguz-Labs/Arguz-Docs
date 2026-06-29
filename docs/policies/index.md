# Policies & Governance

Arguz governance is currently centered on three workflows:

1. alert policies
2. event notification policies
3. scaling rules

## Alert policies

Alert policies route operational failures to the correct channels with scope and thresholds.

Typical fields include:

- policy name
- enabled state
- project, cluster, namespace or deployment scope
- error types
- delay
- threshold or affected ratio
- notification channels
- optional channel schedules

## Event notification policies

Event notification policies are used for non-error operational events such as deployment or platform lifecycle changes.

They support:

- scoped resource path selection
- event type selection
- notification channels
- enabled and silenced states

## Scaling rules

Scaling rules let teams apply temporary HPA changes with auditability.

Arguz records:

- template scope
- requested scaling values
- execution history
- rollback state

### Automatic rollback

Rollback happens when:

- the scaling window ends
- the rule is manually disabled before the end

That behavior is critical for returning the workload to its previous HPA baseline automatically.

## Silences

Both alert and event-notification workflows support temporary suppression, reducing noise during maintenance or controlled rollout windows.
