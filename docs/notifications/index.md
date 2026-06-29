# Notifications

Arguz separates **notification channel management** from **policy usage**.

## Where channels are created

Notification channels are created and maintained in the **Admin Console**, then selected from:

- **Alert Policies**
- **Event Notification Policies**

Use the Admin Console route pattern:

`https://app-admin.arguz.io/admin/organizations/<organization-id>/notification-channels`

## Supported usage in the main app

Once channels exist, operators can:

- attach them to alert policies
- attach them to event notification policies
- schedule active time windows
- silence policies temporarily

## Practical model

| Step | Where it happens |
|---|---|
| Create channel | Admin Console |
| Edit webhook or connector details | Admin Console |
| Select channel for alerts | Main app |
| Select channel for event notifications | Main app |
| Configure channel schedules in a policy | Main app |

## What gets notified

Depending on the policy type:

- workload or rollout failures
- deployment-related incidents
- cluster or platform events
- policy or automation signals

## Helpful note for operators

If a channel dropdown is empty in the main app, first confirm that the organization already has channels configured in the Admin Console.
