# Workloads, Services & CronJobs

This section groups the operational views engineers use most often after installation: services, images and CronJobs.

## Service-oriented views

From the deployments and workload pages, teams can review:

- service scope by project, cluster and namespace
- image sets and rollout history
- HPA context
- related incidents and revisions

## Image visibility

Arguz keeps image tracking close to deployment history so teams can answer:

- where is a tag running?
- which clusters still have an older version?
- which workloads are exposed to a vulnerable base image?

## CronJobs

CronJobs are first-class resources in the current product.

For each CronJob, Arguz shows:

- raw cron expression
- human-readable schedule
- timezone
- concurrency policy
- suspend state
- last schedule
- last successful execution
- recent Job executions

## Execution history

Each execution record can include:

- start and finish times
- duration
- status
- associated Job name
- failure reason
- last 100 log lines when the execution failed

This makes the CronJobs page useful for both routine review and incident triage.

## Operational interpretation

- readable schedule helps non-operators understand cadence quickly
- raw cron keeps the exact Kubernetes source of truth visible
- failure logs reduce context switching into the cluster for first-pass debugging
