# Revision History

Arguz stores revisions as immutable rollout checkpoints for each deployment.

## What a revision contains

- image list
- rollout timestamp
- status
- deployment scope
- linked incidents
- HPA snapshot when available
- Helm context for Helm-managed workloads

## Why revisions matter

### Compare changes over time

Teams can see exactly what changed between revisions and whether a new image or configuration coincided with a problem window.

### Review deployment impact

Errors and operational context can be tied back to a specific revision to answer:

- did the new rollout introduce the issue?
- was the previous revision healthier?
- should we revert to the previous HPA or image state?

### Preserve sanitized Helm context

When the revision includes Helm-derived metadata, Arguz preserves useful resource identity while redacting sensitive fields.

## Typical revision workflow

1. open a deployment
2. inspect the latest revision
3. compare images and timestamps
4. review errors that started after the rollout
5. decide whether to rollback, silence or alert
