# Administration

The Admin Console is where organizations prepare the shared resources that the main Arguz app consumes.

## Main administrative tasks

- manage organizations and users
- create and organize projects
- register clusters and rotate tokens
- create notification channels
- review billing and subscription state

## Notification channels

Notification channels belong to the organization and are created in the Admin Console. Product-side policy editors then consume those channels as selectable options.

Route pattern:

`https://app-admin.arguz.io/admin/organizations/<organization-id>/notification-channels`

## Cluster onboarding flow

1. create or choose a project
2. register the cluster
3. copy cluster credentials
4. install the `arguz-agent` chart
5. confirm heartbeat and inventory sync from the main app

## User and billing responsibilities

Owners and admins typically manage:

- role assignments
- organization settings
- billing visibility
- channel governance
