# Glossary

This glossary defines key terms used throughout the Arguz documentation and platform.

## A

**Agent**
: The Arguz Agent — a lightweight, read-only component deployed in your Kubernetes clusters. It watches resources and reports changes to the Arguz platform.

**Alert Policy**
: A set of conditions that, when met, trigger notifications to configured channels. Defines what errors to alert on, at what threshold, and who to notify.

**Audit Trail**
: A chronological record of changes and events in the platform — user invitations, role changes, policy modifications, token rotations.

## B

**Billing Period**
: The monthly cycle for compute unit consumption and invoicing.

**Blast Radius**
: The set of services that depend on a given service. Understanding blast radius is critical during incident response — it tells you how far the impact of a failure extends.

## C

**Cluster**
: A Kubernetes cluster registered with Arguz. Runs the Arguz Agent and is the source of deployment and observability data.

**Cluster Token**
: A unique authentication credential generated during cluster registration. The agent uses this token to authenticate with the Arguz platform.

**Compute Unit (CU)**
: The billing metric for Arguz. A measure of platform usage across deployment tracking, observability ingestion, and AI features.

**Control Plane API**
: The main Arguz API (`api-discover.arguz.io`) that handles deployment tracking, cluster management, policy configuration, and organization administration.

## D

**Data Ingestion API**
: The high-throughput API (`di-api.arguz.io`) that receives observability telemetry (logs, events, metrics, errors) from observability agents.

**Deployment**
: A Kubernetes Deployment resource tracked by Arguz. Each deployment has a revision history, associated errors, and observability data.

**Dependency**
: A relationship between two services where one calls the other. Arguz tracks both outbound dependencies (services you call) and reverse dependencies (services that call you).

## E

**eBPF Agent**
: An observability agent that uses eBPF (extended Berkeley Packet Filter) to capture application-level telemetry from the kernel without modifying application code.

**Error**
: A detected failure condition — pod crash, HTTP 5xx, OOM kill, TCP reset, etc. Errors are linked to deployment revisions and can trigger alert policies.

**Event**
: A structured piece of application telemetry — an HTTP request, database query, cloud service call, etc. Events are distinct from errors (though errors can be a type of event).

**Exclusion Filter**
: A rule that prevents specific data (events, logs) from being ingested into the Arguz platform. Used to reduce noise and control costs.

## F

**Freeze Window**
: A defined time period during which deployments are blocked or flagged. Used for change management during critical business periods, maintenance windows, or compliance periods.

## G

**Google Identity Platform**
: The authentication service that powers Arguz user login — supports Google OAuth and email/password authentication.

## H

**HPA (Horizontal Pod Autoscaler)**
: A Kubernetes resource that automatically scales the number of pods in a deployment based on observed metrics (CPU, memory, custom).

**Helm**
: The Kubernetes package manager. Arguz agents are distributed and installed via Helm charts.

## I

**Incident**
: A period of service degradation or failure, typically triggered by one or more errors. Arguz helps detect, investigate, and resolve incidents.

**Informer**
: A Kubernetes client library pattern used by the Arguz Agent. Informers establish long-polling watches on API resources and efficiently receive incremental updates.

## L

**Leader Election**
: The mechanism by which multiple agent replicas coordinate so that only one actively watches resources at a time. Uses Kubernetes Leases.

**Log Pattern**
: The structural template of a log message with variable parts replaced. For example, "User alice@example.com logged in from 192.168.1.1" has the pattern "User &lt;\*&gt; logged in from &lt;\*&gt;". Pattern extraction helps reduce log noise.

## M

**MCP (Model Context Protocol)**
: A protocol for AI tools to interact with external services. Arguz provides an MCP Server that allows AI assistants to query deployment data.

**Mitigation**
: The resolution of an error — either automatically (error condition clears) or manually (operator marks as resolved).

**Multi-Tenancy**
: The platform architecture that ensures data isolation between organizations. Each organization's data, users, and settings are fully independent.

## N

**Namespace**
: A Kubernetes namespace within a cluster. Arguz tracks namespaces and allows scoping of deployments, alerts, and policies by namespace.

**Notification Channel**
: A configured destination for alerts — Slack, Microsoft Teams, or VictorOps.

## O

**Observability Agent**
: A separate agent from the Discovery Agent that captures application-level telemetry — logs, events, metrics, errors — often using eBPF or sidecar injection.

**Organization**
: Your company or team's top-level account in Arguz. Contains projects, clusters, users, and all configuration.

## P

**Pattern (Log Pattern)**
: See **Log Pattern**.

**Policy**
: A rule that governs behavior in the Arguz platform — alert policies, scaling rules, freeze windows.

**Project**
: A logical grouping of clusters and namespaces within an organization. Used to organize infrastructure by environment, team, or region.

## R

**RBAC (Role-Based Access Control)**
: The permission system that controls what users can do in Arguz. Roles include Owner, Admin, Editor, Viewer, and Reader.

**RCA (Root Cause Analysis)**
: AI-powered analysis that identifies the probable cause of errors based on deployment changes, error patterns, dependencies, and log data.

**Revision**
: An immutable snapshot of a deployment at a specific point in time — captures container images, HPA configuration, status, and associated errors.

## S

**Sampling**
: The process of selectively keeping a subset of log lines when volume is high. Arguz uses adaptive sampling that preserves unique patterns while reducing repeated log lines.

**Service 360**
: The unified view of a service in Arguz, with tabs for Overview, Logs, Patterns, Events, Metrics, Dependencies, and Used By.

**Sidecar Agent**
: An observability agent that runs as a sidecar container in application pods, capturing telemetry from the pod's network and processes.

**Subscription**
: The billing relationship between an organization and Arguz. Defines the plan, quota, and status (trialing, active, past due, canceled).

## T

**Throttling**
: The mechanism that prevents duplicate notifications for the same alert within a minimum interval. Prevents alert fatigue.

## U

**Used By**
: See **Blast Radius**.

## W

**Workload**
: A general term for applications running in Kubernetes — includes Deployments, StatefulSets, DaemonSets, and Jobs. Arguz primarily tracks Deployments.
