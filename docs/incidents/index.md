# Incidents & Errors

Arguz links operational failures to the changes and workloads around them so teams can investigate with rollout context instead of isolated error lists.

## What you can inspect

- active errors
- historical errors
- revisions with linked issues
- workload scope
- alert policy coverage

## Error sources in the current docs scope

- pod lifecycle failures such as `CrashLoopBackOff`
- `OOMKilled`
- restart-related instability captured from Kubernetes state
- failed CronJob executions and their log excerpts

## Why correlation matters

Arguz helps teams answer:

- which deployment or CronJob run is associated with the failure?
- is the issue localized to one namespace or cluster?
- should a notification policy fire?
- would a rollback or scaling revert help?

## Best practices

1. Review errors together with revisions.
2. Use deployment or cluster scope in policies to reduce noise.
3. Keep CronJob failure logs enabled for operational jobs that are hard to reproduce.
