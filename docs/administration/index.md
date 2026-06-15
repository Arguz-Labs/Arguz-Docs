# Administration

This section covers administration tasks for managing your Arguz organization, users, and settings.

## Organization Management

### Creating an Organization

Organizations are created during initial sign-up. You can create additional organizations if needed.

### Organization Settings

Administrators can manage:

- **Organization name** and **slug** (URL identifier)
- **AI settings**: Configure AI-powered features including Root Cause Analysis
- **Notification defaults**: Set organization-wide notification preferences
- **User management**: Invite, remove, and manage user roles

### Projects

Create and manage projects to organize your infrastructure:

1. Navigate to **Projects**
2. Click **Create Project**
3. Name the project (e.g., "Production", "Staging", "Platform")
4. Projects group clusters and namespaces logically

## User Management

### Roles

Arguz uses role-based access control (RBAC):

| Role | Description |
|---|---|
| **Owner** | Full access to the organization, including billing and user management |
| **Admin** | Full access to all features and settings |
| **Editor** | Can view and edit deployments, policies, and configurations |
| **Viewer** | Read-only access to observability data |
| **Reader** | Read-only access to observability data |

### Capabilities

Fine-grained capabilities control access to specific features:

| Capability | Description |
|---|---|
| `observability.logs.view` | View application logs |
| `observability.logs.export` | Export log data |
| `observability.metrics.view` | View metrics dashboards |
| `observability.metrics.export` | Export metrics data |
| `observability.events.view` | View application events |
| `observability.events.export` | Export event data |
| `observability.errors.view` | View errors and incidents |
| `observability.errors.export` | Export error data |
| `observability.dependencies.view` | View service dependencies |
| `observability.dependencies.export` | Export dependency data |
| `observability.rca.view` | View root cause analysis |
| `observability.rca.export` | Export RCA results |
| `observability.patterns.view` | View log patterns |
| `observability.patterns.export` | Export pattern data |

### Inviting Users

1. Go to **Organization Settings** -> **Users**
2. Click **Invite User**
3. Enter their email address
4. Select their role
5. They will receive an invitation email with setup instructions

### Managing Groups

Users can be organized into groups for easier permission management. Groups can be assigned roles just like individual users.

## Billing

Arguz offers tiered subscription plans based on usage. Billing is managed at the organization level.

### Plans

Each plan includes:

- Monthly compute units (CUs) quota
- Base price per month
- Overage pricing for usage beyond the quota
- Organization and user limits
- Trial period (for new organizations)

### Viewing Usage

From organization settings, administrators can view:

- Current plan and subscription status
- Monthly usage summary
- Compute unit consumption
- Billing history

### Subscription Status

| Status | Meaning |
|---|---|
| **Trialing** | In trial period, full access |
| **Active** | Paid subscription, full access |
| **Past Due** | Payment overdue, 7-day grace period before restriction |
| **Canceled** | Subscription canceled, access restricted |

### Payment Enforcement

Organizations with expired trials, past-due subscriptions (beyond grace period), or canceled subscriptions have restricted access:

- **Read access** may remain available for a limited period
- **Write operations** (creating deployments, managing policies) are blocked
- **API access** returns 402 Payment Required
- Users see a banner indicating the subscription status

## Audit

Organization owners and admins can view an audit trail of significant events:

- User invitations and role changes
- Cluster registrations and token rotations
- Policy changes (alert policies, freeze windows, scaling rules)
- Configuration changes

## Access Tokens

For programmatic access (MCP Server, API), users can generate personal access tokens:

1. Go to **User Settings** -> **Access Tokens**
2. Click **Generate Token**
3. Name the token (describing its purpose)
4. Copy the token value (it will not be shown again)

Treat access tokens like passwords — they grant access to your organization's data.
