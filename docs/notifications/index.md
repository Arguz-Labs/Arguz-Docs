# Notifications

Arguz sends alerts through notification channels when alert policies are triggered. This section covers configuring and managing notification channels.

## Supported Channels

| Channel | Description | Configuration Required |
|---|---|---|
| **Slack** | Block Kit formatted messages to Slack channels | Slack webhook URL |
| **Microsoft Teams** | Formatted messages to Teams channels | Teams webhook URL |
| **VictorOps** | Incident alerts to VictorOps/Splunk On-Call | VictorOps webhook URL |

## Setting Up Notifications

### Slack

1. In your Slack workspace, create an **Incoming Webhook**:
    - Go to [Slack API Apps](https://api.slack.com/apps)
    - Create a new app or use an existing one
    - Enable **Incoming Webhooks**
    - Create a webhook for your desired channel
    - Copy the webhook URL
2. In Arguz, navigate to **Alert Policies** or notification settings
3. Add a new notification channel, select **Slack**, and paste the webhook URL
4. Test the channel to confirm messages are delivered

Slack notifications use **Block Kit** formatting, providing structured, readable alert messages with error details, affected services, and direct links to the Arguz dashboard.

### Microsoft Teams

1. In your Teams channel, create an **Incoming Webhook**:
    - Open the channel, click **More options (...)**
    - Select **Connectors**, search for **Incoming Webhook**
    - Configure and copy the webhook URL
2. In Arguz, add a notification channel, select **Teams**, and paste the webhook URL
3. Test the channel

### VictorOps

1. In VictorOps/Splunk On-Call, configure a **REST endpoint** integration
2. Copy the integration URL
3. In Arguz, add a notification channel, select **VictorOps**, and paste the URL
4. Test the channel

## Notification Content

Alert notifications include:

- **Alert policy name** that triggered the notification
- **Error type and severity**
- **Affected service, namespace, and cluster**
- **Number of affected pods** and total pod count
- **Error timestamp**
- **Link to the error details** in the Arguz dashboard
- **RCA summary** (if available)

## Scheduling

Each notification channel supports **active hours/days scheduling**:

- Configure which days of the week the channel should be active
- Configure active time windows (e.g., 08:00-18:00)
- Outside active hours, notifications are suppressed

This prevents alert fatigue during off-hours and ensures alerts reach the right people at the right time.

## Throttling

To prevent alert storms, Arguz applies **throttling** per channel:

- **Minimum interval**: Alerts for the same error/policy combination are not sent more frequently than the configured interval
- This prevents a single persistent error from flooding notification channels

## Testing Notifications

Before relying on notifications in production:

1. Create an alert policy with test conditions
2. Configure the notification channel
3. Use the **Send Test** feature to verify message delivery
4. Confirm the message format and content appear as expected

## Best Practices

1. **Use different channels for different severities**: Critical alerts to VictorOps, warnings to Slack
2. **Configure active hours**: Set appropriate schedules for each team's working hours
3. **Set meaningful affected ratio thresholds**: Avoid alerting on minor transient issues
4. **Test notifications after setup**: Always send a test message
5. **Include context in alert policies**: Use clear names and descriptions so recipients understand the alert
6. **Avoid overlapping policies**: Multiple policies triggering on the same error can cause duplicate notifications
