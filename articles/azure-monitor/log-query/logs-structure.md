---
title: Structure of Azure Monitor Logs | Microsoft Docs
description: You require a log query to retrieve log data from Azure Monitor.  This article describes how new log queries are used in Azure Monitor and provides concepts that you need to understand before creating one.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: bwren
---

# Structure of Azure Monitor Logs
When you write a [log query](log-query-overview.md) in Azure Monitor, you need to understand where the data you want is located and at least a basic understanding of how that data is structured. Many queries will only require data from a single table, but others may use a variety of options to include data from multiple tables.

This article describes the structure of log data stored in Azure Monitor Logs for the purposes of writing log queries to retrieve and analyze data.

## Overview
Data in Azure Monitor Logs is stored in either a Log Analytics workspace or an Application Insights application. Both are based on [Azure Data Explorer](/azure/data-explorer/) with data accessible with [Kusto Query Language (KQL)](/azure/kusto/query/).

Data in both workspaces and applications are organized into tables, each of which has its own unique set of properties. Most data sources will write to their own tables a Log Analytics workspace, while Application Insights will write to a predefined set of tables in an Application Insights application. You can use a cross-resource query to combine data from multiple locations.

![Tables](media/log-query-overview/queries-tables.png)

## Log Analytics workspace
A [Log Analytics workspace](../platform/manage-access.md) is a container that stores monitoring data and configuration information. You can create one or more workspaces depending on your particular requirements. [Data Sources](../platform/data-sources.md) such as Activity Logs and Diagnostic logs from Azure resources, agents on virtual machines, and data from insights and monitoring solutions will write data to one or more workspaces that you configure as part of their onboarding. Other services such as Azure Security Center and Azure Sentinel also use a Log Analytics workspace to store their data so it can be analyzed using log queries along with monitoring data from other sources.

A standard set of tables are added to a workspace when it's created. New tables are added for different data sources, solutions, and services as they're onboarded. You can also create custom tables using the [Data Collector API](../platform/data-collector-api.md).

Different kinds of data are stored in different tables in the portal, and each table has its own set of properties. New tables may be added to the workspaces as new data sources are configured to write to it. See documentation for these sources for details of the tables they create. Examples include articles for [agent data sources](../platform/agent-data-sources.md), [diagnostic logs](../platform/diagnostic-logs-schema.md), and [monitoring solutions](../insights/solutions-inventory.md).

## Application Insights application
Each application in Application Insights has one and only one corresponding application in Azure Monitor Logs. This is automatically created when you create the application. No configuration is required to collect data, and the application will automatically write monitoring data such as page views, requests, and exceptions to Azure Monitor Logs.

Unlike a Log Analytics workspace, an Application Insights application has a fixed set of tables. You can't configure other data sources to write to the application so no additional tables can be created. 

| Table | Description | 
|:---|:---|
| availabilityResults | Summary data from availability tests. |
| browserTimings      | Data about client performance, such as the time taken to process the incoming data. |
| customEvents        | Custom events created by your application. |
| customMetrics       | Custom metrics created by your application. |
| dependencies        | Calls from the application to external components. |
| exceptions          | Exceptions thrown by the application runtime. |
| pageViews           | Data about each website view with browser information. |
| performanceCounters | Performance measurements from the compute resources supporting the application. |
| requests            | Details of each application request.  |
| traces              | Results from distributed tracing. |


## Query scope
When you run a query, it will need to specify a [query scope](scope.md) which will either include one or more Log Analytics workspaces or an Application Insights application. The query will only have access to 

## Cross-resource queries
To query 




## Next steps
- Learn about using [Log Analytics to create and edit log searches](../log-query/portals.md).
- Check out a [tutorial on writing queries](../log-query/get-started-queries.md) using the new query language.
