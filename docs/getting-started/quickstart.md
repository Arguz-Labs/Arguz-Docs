# Quickstart

Follow this walkthrough to go from zero to full visibility into your Kubernetes deployments in under 15 minutes.

## 1. Create Your Account

1. Go to [app.arguz.io](https://app.arguz.io)
2. Sign in with your Google account or create an account with email/password
3. You will be guided through creating your first Organization
4. Name your organization (e.g., your company name)

## 2. Create a Project

1. From the dashboard, navigate to **Projects**
2. Click **Create Project**
3. Give it a name (e.g., "Production", "Staging", or a team name like "Platform")
4. Projects group clusters and namespaces — use them to organize your infrastructure logically

## 3. Register a Cluster

1. Navigate to **Clusters** in the sidebar
2. Click **Register Cluster**
3. Select the project you just created
4. Enter a cluster name and optional description
5. Choose a cloud provider label for reference (GCP, AWS, Azure, or On-Premises)
6. Copy the **Cluster ID** and **Cluster Token** — save them securely

## 4. Install the Agent

Run the following commands on a machine with `kubectl` configured to your cluster:

```bash
helm repo add arguz-agent https://Arguz-Labs.github.io/Arguz-Agent-Chart
helm repo update

helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  --version 0.2.0 \
  -n arguz-agent \
  --create-namespace \
  --set-string global.clusterCredentials.projectId=<PROJECT_ID> \
  --set-string global.clusterCredentials.clusterId=<CLUSTER_ID> \
  --set-string global.clusterCredentials.clusterToken=<CLUSTER_TOKEN>
```

Replace the placeholders with the values from step 3.

## 5. Verify Connectivity

Back in the Arguz web application:

1. Go to **Clusters** — your cluster should show a green "Connected" status within 2-3 minutes
2. Go to **Deployments** — your Kubernetes Deployments should begin appearing as the agent discovers them
3. If you don't see data after 5 minutes, check [agent troubleshooting](../faq/index.md#troubleshooting)

## 6. Explore a Service

1. Navigate to **Deployments** and click on any deployment
2. This opens the **Service 360** view with multiple tabs:
    - **Overview** — Health status, recent errors, deployment timeline
    - **Logs** — Application logs from this service
    - **Events** — HTTP requests, database queries, cloud service calls
    - **Metrics** — CPU, memory, and custom metrics
    - **Dependencies** — Services this deployment communicates with
    - **Used By** — Services that depend on this deployment (blast radius)

## 7. View a Revision

1. From the deployment detail page, click the **Revisions** tab
2. Click on any revision to see:
    - Container images and tags at that point in time
    - Errors that occurred after this revision
    - HPA configuration at revision time
    - Timestamps and status

## 8. Set Up Your First Alert

1. Navigate to **Alert Policies** in the sidebar
2. Click **Create Alert Policy**
3. Configure:
    - **Name**: e.g., "High Error Rate"
    - **Error Types**: select the error conditions to alert on
    - **Affected Ratio**: minimum percentage of pods affected
    - **Scope**: restrict to specific projects, clusters, namespaces, or deployments
    - **Notification Channel**: add Slack, Teams, or VictorOps
4. Save the policy

!!! tip "Test Your Notifications"
    Use the notification integration settings to send a test message before relying on alerts in production.

## 9. Explore Dependencies

1. Navigate to **Dependencies** in the sidebar
2. View the interactive dependency graph showing how services communicate
3. Click any node to drill into that service's details
4. Use the **Used By** tab in any service view to understand blast radius — critical for incident response

## 10. Configure Notifications

1. Navigate to **Integrations** or **Alert Policies**
2. Add notification channels:
    - **Slack**: Provide a Slack webhook URL
    - **Microsoft Teams**: Provide a Teams webhook URL
    - **VictorOps**: Provide a VictorOps webhook URL
3. Configure active hours/days per channel (optional) to prevent alerts during off-hours

## What's Next?

- Set up [alert policies](../policies/index.md) for your critical services
- Create [freeze windows](../policies/index.md#freeze-windows) for your change management process
- Configure [scaling rules](../policies/index.md#scaling-rules) governance
- Integrate with [ArgoCD](../integrations/argocd.md) for GitOps deployment tracking
- Explore the [MCP Server](../integrations/index.md#mcp-server) for AI-assisted operations
