# Root Cause Analysis

Arguz provides AI-powered Root Cause Analysis (RCA) to help you quickly understand why errors are occurring in production. RCA analyzes deployment changes, error patterns, service dependencies, and log anomalies to surface probable causes.

## What Is RCA in Arguz?

When errors are detected after a deployment, Arguz's RCA engine analyzes multiple signals to identify the most likely root cause:

- **Deployment changes**: What changed? Which images were updated? Were configurations modified?
- **Error patterns**: What types of errors occurred? Are they concentrated in specific pods or endpoints?
- **Dependency impact**: Did errors cascade from upstream services? Is a downstream dependency failing?
- **Timing analysis**: Did errors start immediately after the deployment, or was there a delay?
- **Log anomalies**: Did new or unusual log patterns appear after the change?

The RCA engine presents findings in natural language with supporting evidence, enabling faster incident response.

## How to Access RCA

### From Error Details

When viewing an error in the Arguz web application, if the error is associated with a deployment revision, an **RCA** tab or section will be available showing the analysis.

### RCA Requirements

RCA is available when:

- The error is linked to a specific revision
- The deployment has sufficient historical data for comparison
- There is enough data (errors, logs, metrics) for meaningful analysis

## Understanding RCA Results

An RCA analysis typically includes:

### Summary

A concise explanation of the probable root cause in plain language.

### Evidence

Supporting data points that inform the analysis:

- **Deployment diff**: What containers changed between revisions
- **Error timeline**: Error onset relative to deployment time
- **Affected scope**: Which pods, namespaces, or services are impacted
- **Dependency chain**: Whether the error correlates with upstream or downstream issues

### Confidence

The RCA engine provides a confidence indicator showing how strongly the evidence supports the conclusion. Higher confidence typically means clearer correlation between the deployment and the error.

### Related Errors

Other errors across the system that may be related to the same root cause — helping you understand the full blast radius.

## Using RCA During Incident Response

RCA is designed to accelerate incident response, not replace human judgment:

1. **Start with RCA** — Use the analysis as your starting point for investigation
2. **Verify with data** — Check the evidence (logs, metrics, events) to confirm the findings
3. **Combine with knowledge** — Apply your team's knowledge of the system and recent changes
4. **Take action** — Roll back, scale up, or fix forward based on the complete picture

## RCA Best Practices

1. **Use RCA as a starting point** — It accelerates triage but doesn't replace domain expertise
2. **Verify evidence** — Check the logs, metrics, and events that inform the analysis
3. **Correlate across services** — Check if other services show similar error patterns
4. **Document findings** — RCA provides a natural post-incident review starting point
5. **Enable for all critical services** — The more data RCA has, the better its analysis

## AI Configuration

RCA is powered by AI capabilities that operate within the Arguz platform. Organization administrators can configure AI settings for their organization, including model preferences and data synchronization.

## Limitations

- RCA analyzes correlations and patterns — it identifies probable causes, not definitive root causes
- Results depend on the quality and volume of available data
- New deployments with limited history may produce lower-confidence analyses
- RCA is a tool for acceleration, not a replacement for engineering investigation
