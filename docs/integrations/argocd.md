# ArgoCD Integration

Arguz integrates with ArgoCD to provide deployment tracking for GitOps-managed workloads. When your deployments are managed by ArgoCD, Arguz correlates ArgoCD sync events with the deployment revisions it captures.

## Overview

The ArgoCD integration enables:

- **Sync-to-deployment correlation**: See which ArgoCD sync operation triggered a deployment change
- **Application context**: Know which ArgoCD application manages each deployment
- **GitOps audit trail**: Trace deployments back through ArgoCD to the Git commit that triggered the sync

## How It Works

ArgoCD integrates with Arguz via webhooks:

1. You configure a webhook in your ArgoCD instance
2. When ArgoCD syncs an application, it sends a webhook notification to Arguz
3. Arguz associates the ArgoCD sync event with the deployment revision detected by the agent
4. The deployment history shows ArgoCD context alongside Kubernetes-level changes

## Configuration

### Prerequisites

- ArgoCD installed and managing your cluster's deployments
- Arguz Agent installed in the same cluster
- Admin access to ArgoCD configuration

### Setup Steps

1. In Arguz, navigate to **Integrations** -> **ArgoCD**
2. Follow the setup wizard to configure the connection
3. In ArgoCD, add a webhook notification:
    - Configure the Arguz webhook URL as the notification target
    - Select relevant triggers (typically `on-sync`, `on-health-degraded`)

## Usage

Once configured, ArgoCD context appears in:

- **Deployment Revisions**: Each revision shows the associated ArgoCD application and sync operation
- **Service 360 View**: The deployment overview includes ArgoCD application status
- **Error Correlation**: When investigating errors, you can see whether they followed an ArgoCD sync

## How It Complements the Agent

The Arguz Discovery Agent detects Kubernetes-level deployment changes. The ArgoCD integration adds the GitOps context layer:

- **Agent alone**: Knows a deployment changed and what images are running
- **Agent + ArgoCD**: Knows the deployment changed because ArgoCD synced application `my-app` from Git commit `abc123`

This provides complete traceability: **Git Commit** -> **ArgoCD Sync** -> **Kubernetes Deployment** -> **Error Monitoring**.
