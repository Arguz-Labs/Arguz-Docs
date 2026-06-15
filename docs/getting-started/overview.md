# Overview

Arguz is a **Kubernetes Deployment Governance and Observability Platform** that bridges the gap between deployment activity and operational reality. It tells you what changed in your infrastructure, whether that change caused problems, and helps you understand why.

## What Problems Does Arguz Solve?

### Problem 1: You Don't Know What Changed

When an incident occurs, the first question is always: "What changed?" Without a unified view of deployments across clusters, teams waste precious time correlating Git commits, CI/CD logs, and cluster state.

**Arguz solves this** by automatically tracking every deployment across all your registered clusters, capturing revision history, and linking deployments to errors and service health.

### Problem 2: Errors Are Hard to Triage

A service starts returning 500 errors. Is it related to the deployment from 10 minutes ago? Is it an infrastructure issue? Is it a dependency failure?

**Arguz solves this** by correlating errors with deployment revisions, providing service dependency graphs, and offering AI-powered root cause analysis that surfaces the most likely cause.

### Problem 3: No Governance Over Deployments

Without a centralized view, it's impossible to enforce deployment policies — change freezes during critical periods, scaling rules to control costs, or alert policies to catch regressions early.

**Arguz solves this** with policy management: freeze windows, alert policies with notification channels (Slack, Teams, VictorOps), and HPA scaling rule governance.

### Problem 4: Observability Is Fragmented

Logs are in one tool, metrics in another, deployment history in a third, and dependencies are guesswork.

**Arguz solves this** with Service 360 — a unified view per service that combines logs, metrics, events, patterns, dependencies, and deployment history.

## Key Features

### Deployment Tracking & Revision History

Every time a Kubernetes Deployment changes, Arguz captures the revision. Each revision includes:

- Container images and tags
- Deployment timestamp
- Status (successful, failed, in-progress)
- Associated errors detected after deployment
- HPA configuration at the time of deployment

### Service 360 Observability

For every service in your cluster, Arguz provides:

- **Overview** — Health summary, recent deployments, active errors
- **Logs** — Search, filter, and analyze application logs
- **Patterns** — Automatically detected log patterns for noise reduction
- **Events** — Application events (HTTP, DB queries, cloud service calls)
- **Metrics** — CPU, memory, custom metrics with time-series visualization
- **Dependencies** — Outbound service dependencies and reverse dependencies (who depends on you)

### AI-Powered Root Cause Analysis

When errors are detected after a deployment, Arguz's AI engine analyzes error patterns, deployment changes, and service dependencies to provide a probable root cause with supporting evidence.

### Alert Policies & Notifications

Define conditions that trigger alerts:

- Error type matching
- Minimum affected pod ratio
- Path/endpoint filtering
- Project, cluster, namespace, and deployment scoping
- Active hours/days scheduling per notification channel

Notifications are delivered to Slack, Microsoft Teams, and VictorOps.

### Scaling Rule Management

View and manage HPA (Horizontal Pod Autoscaler) configurations across your clusters. Track scaling rule changes with an audit trail.

### Change Freeze Windows

Define time windows during which deployments are blocked — useful for critical business periods, holidays, or maintenance windows.

### Multi-Tenant Organization Management

Each organization has isolated data, users, and configuration:

- Role-based access control (Owner, Admin, Editor, Viewer)
- Fine-grained capabilities (view/export logs, metrics, events, errors, RCA, dependencies)
- Project-level cluster and namespace scoping

### MCP Server Integration

A read-only Model Context Protocol (MCP) server allows AI tools and assistants to query your Arguz deployment data programmatically — search for images, list errors, find deployments, and more.

## Core Concepts

### Organization

Your company's top-level account. All clusters, projects, users, and settings live within an organization. Organizations are fully isolated from each other.

### Project

A logical grouping of related clusters and namespaces. For example, you might create a "payments" project containing the production and staging clusters for your payments infrastructure.

### Cluster

A Kubernetes cluster registered with Arguz. Each cluster runs the Arguz Agent, which collects metadata and observability data.

### Deployment

A Kubernetes Deployment resource tracked by Arguz. Arguz monitors the deployment lifecycle and links each revision to errors, metrics, and events.

### Revision

A snapshot of a deployment at a point in time. Revisions capture which container images were running, when the deployment occurred, and what errors followed.

### Namespace

Kubernetes namespaces within a cluster. Arguz tracks namespace membership and allows scoping of deployments, alerts, and policies.

## Platform Access

The Arguz web application is available at **[app.arguz.io](https://app.arguz.io)**.

Authentication is handled via **Google OAuth** or **email/password** through Google Identity Platform.

An admin interface for organization management is available at **[app-admin.arguz.io](https://app-admin.arguz.io)** (admin access required).
