---
title: Overview of Azure Platform logs | Microsoft Docs
description: Overview of diagnostic logs in Azure which provide rich, frequent data about the operation of an Azure resource.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: bwren
ms.subservice: logs
---
# Overview of Platform logs in Azure
Platform logs provide detailed diagnostic and auditing information for Azure resources and the Azure platform they depend on. They are automatically generated although you need to configure certain platform logs to be forwarded to one or more destinations to be retained. This article provides an overview of platform logs including what information they provide and how you can configure them for collection and analysis.

## Types of platform logs
The following table lists the specific platform logs that are available at different layers of Azure.

| Layer | Logs | Description |
|:---|:---|:---|
| Azure Resources | [Resource logs](diagnostic-logs-schema.md) | Provide insight into operations that were performed within an Azure resource (the *data plane*), for example getting a secret from a Key Vault or making a request to a database. The content of resource logs varies by the Azure service and resource type. |
| Azure Subscription | [Activity log](activity-logs-overview.md) | Provides insight into the operations on each Azure resource in the subscription from the outside (*the management plane*) in addition to updates on Service Health events. There is a single Activity log for each Azure subscription.   |
| Azure Tenant | [Azure Active Directory logs](../active-directory/reports-monitoring/overview-reports.md)  | Contains the history of sign-in activity and audit trail of changes made in the Azure Active Directory for a particular tenant.   |


![Platform logs overview](media/diagnostic-logs-overview/logs-overview.png)

## Viewing platform logs
You can view the [Activity log](activity-log-view.md) and [Azure Active Directory logs](../active-directory/reports-monitoring/overview-reports.md) in the Azure portal. You must send Resource logs to a [destination](#destinations) to view them.


## Destinations
You can send platform logs to one or more of the destinations in the following table depending on your monitoring requirements. 

| Destination | Scenario | References |
|:---|:---|:---|:---|
| Log Analytics workspace | Analyze the logs with other monitoring data and leverage Azure Monitor features such as log queries and alerts. | [Resource logs](diagnostic-logs-stream-log-store.md)<br>[Activity log](activity-log-collect.md)<br>[Azure Activity Directory logs](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure storage | Archive the logs for audit, static analysis, or backup. |[Resource logs](archive-diagnostic-logs.md)<br>[Activity log](activity-log-export.md)<br>[Azure Activity Directory logs](../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Event hub | Stream the logs to third-party logging and telemetry systems.  |[Resource logs](diagnostic-logs-stream-event-hubs.md)<br>[Activity log](activity-log-export.md)<br>[Azure Activity Directory logs](../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |


## Diagnostic settings and log profiles
Configure destinations for Resource logs and Azure Active Directory logs by [creating a Diagnostic setting](diagnostic-settings.md). Configure destinations for the Activity log by [creating a log profile](activity-log-export.md) or by [connecting it to a Log Analytics workspace](activity-log-collect.md).

The diagnostic setting and log profile define the following:

- One or more destinations to send selected logs and metrics.
- Which log categories and metrics from the resource are sent to the destinations.
- If a storage account is selected as a destination, how long each log category should be retained.



## Next steps

* [Stream resource diagnostic logs to **Event Hubs**](diagnostic-logs-stream-event-hubs.md)
* [Change resource diagnostic settings using the Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/)
* [Analyze logs from Azure storage with Azure Monitor](collect-azure-metrics-logs.md)
