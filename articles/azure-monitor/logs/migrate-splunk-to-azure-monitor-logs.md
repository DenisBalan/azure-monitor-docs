---
title: Migrate from Splunk to Azure Monitor Logs - Getting started
description: Learn how to plan the phases of your migration from Splunk to Azure Monitor Logs and get started importing, collection, and analyzing log data. 
author: guywi-ms
ms.author: guywild
ms.reviewer: MeirMen
ms.topic: how-to 
ms.date: 11/22/2022

---

# Migrate from Splunk to Azure Monitor Logs

This article explains how to plan your migration from Splunk to Azure Monitor Logs for logging and log data analysis, including:  

> [!div class="checklist"]
> * Introduction to key concepts 
> * Set up a Log Analytics workspace
> * Collect data
> * Migrate Splunk artifacts
> * Ingest historical data
> * Analyze log data

## Introduction to key concepts

|Concept |Description|
|---|---|
|[Log Analytics workspace](../logs/log-analytics-workspace-overview.md)|An environment in which to collect log data from all Azure and non-Azure monitored resources, including log data you collect for Azure Monitor and other Azure services, such as Microsoft Sentinel and Microsoft Defender for Cloud.  |
|[Table management](../logs/manage-logs-tables.md)|Azure Monitor Logs stores log data in tables. Table configuration lets you define the table schema, how long to retain data, and whether you need the data available for occasional auditing and troubleshooting or for ongoing data analysis and regular use by features and services.|
|[Basic and Analytics log data plans](../logs/basic-logs-configure.md)| Azure Monitor Logs offers two log data plans that let you reduce log ingestion and retention costs and take advantage of Azure Monitor's advanced features and analytics capabilities based on your needs.<br/> The **Basic** log data plan provides a low-cost way to ingest and retain logs for troubleshooting, debugging, auditing, and compliance. The **Analytics** plan makes log data available for interactive queries and use by features and services. |
|[Archiving and quick access to archived data](../logs/data-retention-archive.md)| The cost-effective archive option keeps your logs in your Log Analytics workspace and lets you access archived log data immediately, when you need it. Archive configuration changes are also effective immediately because data is not physically transferred to external storage. |
|[Data transformations](../essentials/data-collection-transformations.md)|Transformations let you filter or modify incoming data before it's sent to a Log Analytics workspace. Use transformations to remove sensitive data, enrich data in your Log Analytics workspace with additional or calculated information, and filter out data you don't need to reduce data costs. |
|[Data collection rules](../essentials/data-collection-rule-overview.md)|Define which data to collect, how to transform that data, and where to send the data. |
|[Kusto Query Language (KQL)](/azure/kusto/query/)|Azure Monitor Logs uses a large subset of KQL that's suitable for simple log queries but also includes advanced functionality such as aggregations, joins, and smart analytics. Use the [Splunk to Kusto Query Language map](/azure/data-explorer/kusto/query/splunk-cheat-sheet) to translate your Splunk SPL knowledge to KQL. You can also [learn KQL with tutorials](../logs/get-started-queries.md) and [KQL training modules](/training/modules/analyze-logs-with-kql/).|
|[Log Analytics](../logs/log-analytics-overview.md)| A tool in the Azure portal that's used to edit and run log queries on data collected to Azure Monitor Logs.|
|[Cost optimization](../../azure-monitor/best-practices-cost.md)|Reduce your costs by understanding [Azure Monitor Logs cost calculations](../logs/cost-logs.md) and following [best practices for optimizing costs in Azure Monitor](../../azure-monitor/best-practices-cost.md). |

## 1. Set up a Log Analytics workspace

Your Log Analytics workspace is where you collect log data from all of your monitored resources. You can retain data in a Log Analytics workspace for up to seven years. Low-cost data archiving within the workspace lets you access archived data quickly and easily when you need it, without the overhead of managing an external data store.

We recommend collecting all of your log data in a single Log Analytics workspace for ease of management. If you are considering using multiple workspaces, see [Design a Log Analytics workspace architecture](../logs/workspace-design.md).

To set up a Log Analytics workspace for data collection:

1. [Create a Log Analytics workspace](../logs/quick-create-workspace.md).
    
    Azure Monitor Logs creates Azure tables in your workspace automatically based on Azure services you use and [data collection settings](#4-collect-data) you define for Azure resources.

1. Configure your Log Analytics workspace, including:
    1. [Pricing tier](../logs/change-pricing-tier.md).
    1. [Daily cap](../logs/daily-cap.md).
    1. [Data retention](../logs/data-retention-archive.md).
    1. [Network isolation](../logs/private-link-security.md).
    1. [Access control](../logs/manage-access.md).

1. Use [table-level configuration settings](../logs/manage-logs-tables.md) to: 
    1. Define each table's log data plan. 
    
        The default log data plan is Analytics, which lets you take advantage of Azure Monitor's rich monitoring and analytics capabilities. If youYou can 
    
    1. Set a data retention and archiving policy for specific tables, if you need them to be different form the default workspace-level data retention and archiving policy. 
    1. Modify the table schema based on your data model.

## 2. Migrate Splunk artifacts to Azure Monitor

To migrate most Splunk artifacts, you need to translate Splunk Processing Language (SPL) to Kusto Query Language (KQL). For more information, see the [SPL to KQL cheat sheet](/azure/data-explorer/kusto/query/splunk-cheat-sheet) and [Get started with log queries in Azure Monitor](../logs/get-started-queries.md).
T
:::image type="content" source="media/migrate-splunk-to-azure-monitor-logs/import-splunk-artifacts-to-azure-monitor.png" alt-text="Diagram that shows Azure Monitor capabilities related to insights, visualization, analysis, and responsive actions." lightbox="media/migrate-splunk-to-azure-monitor-logs/import-splunk-artifacts-to-azure-monitor.png":::

This table lists Splunk artifacts and provides links to guidance on how to set up the equivalent artifacts in Azure Monitor:

|Splunk artifact| Azure Monitor artifact|
|---|---|
|Alerts|[Alert rules](../alerts/alerts-create-new-alert-rule.md)|
|Alert actions|[Action groups](../alerts/action-groups.md)|
|Apps|[Azure Monitor Insights](../insights/insights-overview.md) are a set of ready-to-use, curated monitoring experiences with pre-configured data inputs, searches, alerts, and visualizations to get you started analyzing data quickly and effectively. |
|Dashboards|[Workbooks](../visualize/workbooks-overview.md)|
|Searches|[Queries](../logs/get-started-queries.md)|
|Source types|[Define your data model in your Log Analytics workspace](../logs/manage-logs-tables.md). Use [ingestion-time transformations]() to filter, format, or modify incoming data.|
|Namespaces|[Log Analytics workspaces](../logs/log-analytics-workspace-overview.md) and [Azure resource groups](../../azure-resource-manager/management/manage-resource-groups-portal.md)|
|Permissions|[Access management](../logs/manage-access.md)|
|Reports|Azure Monitor offers a range of options for analyzing, visualizing, and sharing data, including:<br>- [Insights](../insights/insights-overview.md)<br>- [Workbooks](../visualize/workbooks-overview.md)<br>- [Dashboards](../visualize/tutorial-logs-dashboards.md) <br>- [Integration with Power BI](../logs/log-powerbi.md)<br>- [Integration with Excel](../logs/log-excel.md)<br>- [Integration with Grafana](../visualize/grafana-plugin.md) |
|Universal forwarder| Azure Monitor provides a number of [data collection tools](#4-collect-data) designed for specific resources.| 

For information on migrating Splunk SIEM artifacts, including detection rules and SOAR automation for, see [Plan your migration to Microsoft Sentinel](../../sentinel/migration.md).
## 3. Ingest historical data

:::image type="content" source="media/migrate-splunk-to-azure-monitor-logs/import-data-from-splunk-to-azure-monitor.png" alt-text="Diagram that shows data streaming in from Splunk to a Log Analytics workspace in Azure Monitor Logs." lightbox="media/migrate-splunk-to-azure-monitor-logs/import-data-from-splunk-to-azure-monitor.png":::

## 4. Collect data

Azure Monitor provides tools for collecting data from log [data sources](../data-sources.md) on Azure and non-Azure resources in your environment. 

:::image type="content" source="media/migrate-splunk-to-azure-monitor-logs/azure-monitor-logs-collect-data.png" alt-text="Diagram that shows various data sources being connected to Azure Monitor Logs." lightbox="media/migrate-splunk-to-azure-monitor-logs/azure-monitor-logs-collect-data.png":::

This table lists the tools to use to collect data from various monitored resources.  

| Monitored resource | Data collection tool | Collected data |
| --- | --- | --- |
| **Azure** | [Diagnostic settings](../essentials/diagnostic-settings.md)  | **Azure tenant** - Azure Active Directory Audit Logs provide sign-in activity history and audit trail of changes made within a tenant.<br/>**Azure resources** - Logs and performance counters.<br/>**Azure subscription** - Service health records along with records on any configuration changes made to the resources in your Azure subscription. |
| **Application** | [Application insights](../app/app-insights-overview.md) | Application performance monitoring data. |
| **Container** |[Container insights](../containers/container-insights-overview.md)| Container performance data. |
| **Operating system** | [Azure Monitor Agent](../agents/agents-overview.md) | Monitoring data from the guest operating system of Azure and non-Azure virtual machines.|
| **Non-Azure source** | [Logs Ingestion API](../logs/logs-ingestion-api-overview.md) | File-based logs and any data you send to a [data collection endpoint](../essentials/data-collection-endpoint-overview.md) on a monitored resource.|
## 5. Analyze log data

[Log Analytics demo environment](https://portal.azure.com/#view/Microsoft_OperationsManagementSuite_Workspace/LogsDemo.ReactView)

[Analyze logs in Azure Monitor with KQL](/training/modules/analyze-logs-with-kql/)

## Next steps
<!-- Add a context sentence for the following links -->
- [Write how-to guides](contribute-how-to-write-howto.md)
- [Links](links-how-to.md)

