# Clusters & Nodes

Clusters in Arguz are the connection point between your Kubernetes estate and the platform. The current cluster experience includes registration, cloud metadata, namespace inventory and a dedicated node snapshot view.

## Cluster registration

Register clusters from the Admin Console:

1. choose the project
2. create the cluster record
3. copy the credentials
4. install the `arguz-agent` chart

## Cluster data shown in Arguz

- cluster name and description
- project membership
- cloud provider context
- region and zone
- Kubernetes version
- namespace count
- deployment count
- last seen heartbeat

## Cloud metadata

The Discovery Agent attempts to populate:

- provider
- cluster scope
- project, account or subscription identifiers
- region and zone
- cluster name from metadata when available

This metadata is also used to build deep links into cloud consoles. For AWS, links are based on the cluster name stored by Arguz. For GCP, they use the cluster name reported in cloud metadata.

## Nodes page

The **Nodes** page is an inventory view designed for compact, responsive review.

It shows:

- readiness and pressure conditions
- capacity and allocatable CPU and memory
- pods schedulable capacity
- cluster, project and zone
- instance type, kubelet version and runtime
- capture timestamp

!!! note
    Pod values shown in node inventory refer to schedulable capacity, not the number of pods currently running.

## Namespaces and provider links

Arguz also supports cloud-provider links for:

- clusters
- namespaces
- workloads

These links are constructed from the cluster metadata heartbeat plus the resource scope currently shown in the frontend.
