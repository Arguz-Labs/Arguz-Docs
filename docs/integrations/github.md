# GitHub Integration

Arguz integrates with GitHub to correlate deployments with Git activity, providing full traceability from Git commit to production deployment.

## Overview

The GitHub integration enables:

- **Commit-to-deployment traceability**: See which Git commits are included in each deployment revision
- **Pull request context**: Link deployments to the PRs that triggered them
- **Change context for incidents**: When investigating an error, see the associated code changes

## How It Works

GitHub integrates with Arguz via webhooks:

1. You configure a webhook in your GitHub repository
2. When events occur (push, pull request), GitHub sends a webhook to Arguz
3. Arguz associates the Git metadata with deployments tracked in your clusters
4. The deployment revision history shows Git context for each change

## Configuration

### Prerequisites

- Admin access to the GitHub repository
- An Arguz organization with at least one registered cluster

### Setup Steps

1. In Arguz, navigate to **Integrations** -> **GitHub**
2. Follow the setup wizard to connect your GitHub organization or repository
3. Configure the webhook in your GitHub repository settings:
    - Payload URL: Provided by Arguz during setup
    - Content type: `application/json`
    - Events: Select `push` and `pull_request` events
    - Secret: Generated during setup for webhook validation

### Webhook Events

| Event | Purpose |
|---|---|
| `push` | Track commits pushed to branches |
| `pull_request` | Track PR creation, merge, and close events |

## Usage

Once configured, GitHub context appears in:

- **Deployment Revisions**: Each revision shows associated commits and PRs
- **Service 360 View**: Deployment timeline includes Git commit references
- **Error Investigation**: When errors correlate with a deployment, you can trace back to specific code changes

## Security

- Webhook payloads are validated using a shared secret
- The integration does not access your source code — it captures metadata (commit SHAs, PR numbers, branch names)
- The integration requires minimal GitHub permissions (read-only access to commit metadata)
