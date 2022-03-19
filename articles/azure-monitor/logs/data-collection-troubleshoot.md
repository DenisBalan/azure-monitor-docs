---
title: Troubleshoot why data is no longer being collected in Azure Monitor
description: Steps to take if data is no longer being collected in Log Analytics workspace in Azure Monitor.
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/18/2022
---
 
# Troubleshoot why data is no longer being collected in Azure Monitor
This article provides steps that you can take to determine why data is no longer being collected by Azure Monitor.


## Data collection status
When data collection in a Log Analytics workspace stops, an event with a type of **Operation** is created in the workspace. Run the following query to check whether you're reaching the daily limit and missing data: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

When data collection stops, the **OperationStatus** is **Warning**. When data collection starts, the **OperationStatus** is **Succeeded**.

To be notified when data collection stops, use the steps described in the [Alert when daily cap is reached](daily-cap.md#alert-when-daily-cap-is-reached) section. To configure an e-mail, webhook, or runbook action for the alert rule, use the steps described in [create an action group](../alerts/action-groups.md). 

## Legacy free pricing tier
If your Log Analytics workspace is on the [legacy Free pricing tier]() and has collected more than 500 MB of data in a day, data collection stops for the rest of the day. Reaching the daily limit is a common reason that Log Analytics stops collecting data, or data appears to be missing. 
 The following table lists reasons that data collection stops and a suggested action to resume data collection.


## Daily cap was reached
The [daily cap](daily-cap.md) limits the amount of data that a Log Analytics workspace can collect in a day. If the daily cap is reached, then data collection will stop until the reset time that you defined. Either wait for collection to automatically restart, or increase the daily data volume limit.


## Workspace reached the data ingestion volume rate
The [default ingestion volume rate limit](../service-limits.md#log-analytics-workspaces)  for data sent from Azure resources using diagnostic settings is approximately 6 GB/min per workspace. This is an approximate value because the actual size can vary between data types, depending on the log length and its compression ratio. This limit doesn't apply to data that's sent from agents or the [Data Collector API](data-collector-api.md). 

If you send data at a higher rate to a single workspace, some data is dropped, and an event is sent to the Operation table in your workspace every 6 hours while the threshold continues to be exceeded. If your ingestion volume continues to exceed the rate lim#it or you are expecting to reach it sometime soon, you can request an increase to your workspace by sending an email to LAIngestionRate@microsoft.com or by opening a support request. 

The event to look for that indicates a data ingestion rate limit can be found by the following query.

```kusto
Operation | where OperationCategory == "Ingestion" | where Detail startswith "The rate of data crossed the threshold"
```

## Azure subscription is in a suspended state 
You Azure subscription could be in a suspended state for one of the following reasons:

- Free trial ended
- Azure pass expired
- Monthly spending limit reached (such as on an MSDN or Visual Studio subscription)


## Limits summary

There are additional Log Analytics limits, some of which depend on the Log Analytics pricing tier. These are documented at [Azure subscription and service limits, quotas, and constraints](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces).


## Next steps

