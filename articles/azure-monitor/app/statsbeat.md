---
title: Statsbeat in Azure Application Insights | Microsoft Docs
description: Statstics about Application Insights SDKs and Auto-Instrumentation
ms.topic: conceptual
ms.date: 09/20/2021

---

# Statsbeat in Azure Application Insights

Statsbeat is a new feature being added to the Application Insights SDKs and Auto-Instrumentation. Statsbeat collects essential and non-essential [custom metric](../essentials/metrics-custom-overview.md) about our SDKs and auto-instrumentation. Statsbeat serves three benefits for Azure Monitor Application insights customers:
-	Service Health and Reliability (outside-in monitoring of ingestion endpoint)
-	Support Diagnostics (self-help insights and CSS Insights)
-	Product Improvement (insights for design optimizations)

Statsbeat data is stored in a Microsoft data store.  It doesn't impact customers' overall monitoring volume and cost. 

## What data does Statsbeat collect?

Statsbeat collects essential and non-essential metrics.

### Essential Statsbeat

#### Network Statsbeat

|Metric Name|Unit|Supported dimensions|
|-----|-----|-----|
|Request Success Count|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Requests Failure Count|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Request Duration|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Retry Count|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Throttle Count|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Exception Count|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|

#### Attach Statsbeat

|Metric Name|Unit|Supported dimensions|
|-----|-----|-----|
|Attach|Count| `Resource Provider`, `Resource Provider Identifier`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`|

#### Feature Statsbeat

|Metric Name|Unit|Supported dimensions|
|-----|-----|-----|
|Feature|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Feature`, `Type`, `Operating System`, `Language`, `Version`|

### Non-Essential Statsbeat

- Track the success and failure of disk persistence
- Live metrics network statsbeat
- Azure metadata service network statsbeat
- Profiler network statsbeat

### Configure Statsbeat

#### [Java](#tab/java)

To disable non-essential Statsbeat, add the below configuration to your config file.

```json
{
  "preview": {
    "statsbeat": {
        "disabled": "true"
    }
  }
}
```

You can also disable this feature by setting the environment variable `APPLICATIONINSIGHTS_STATSBEAT_DISABLED` to true (which will then take precedence over disabled specified in the json configuration).

#### [Node](#tab/node)

It's not supported yet.

#### [Python](#tab/python)

It's not supported yet.

---
