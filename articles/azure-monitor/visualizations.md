---
title: Deploy Azure Monitor - Analysis and visualizations
description: Guidance and recommendations for customizing visualizations beyond standard analysis features in Azure Monitor.
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 07/28/2021

---

# Deploying Azure Monitor - Analyze and visualize data
This article is part of the scenario [Recommendations for configuring Azure Monitor](deploy.md). It describes builtin features for analyzing collected data and for creating custom visualizations to meet the requirements of different users in the organization. Visualizations such as charts and graphs can help you analyze your monitoring data to drill down on issues and identify patterns. Depending on the tool that you use, you might also have the option to share visualizations with other users inside and outside your organization.


## Builtin analysis features
The following sections describe Azure Monitor features that provide analysis of collected data without configuration.
### Overview page
Most Azure services will have an **Overview** page in the Azure portal that includes a **Monitor** section with charts showing recent charts for critical metrics. This is intended for owners of individual services to quickly assess the performance of the resource. Since this page is based on platform metrics that are collected automatically, there's no configuration required for this feature.

## Metrics explorer
Metrics explorer allows users to interactively work with metric data and create metric alerts. Most users will be able to use metrics explorer with minimal training but must be familiar with the metrics they want to analyze. There's no configuration required for this feature once data collection has been configured. Platform metrics for Azure resources will automatically be available. Guest metrics for virtual machines will be available when Azure Monitor agent has been deployed to them, and application metrics will be available when Application Insights has been configured.


## Log Analytics
Log Analytics allows users to create log queries to interactively work with log data and create log query alerts. There is some training required for users to become familiar with the query language, although they can use prebuilt queries provided by Microsoft. You can also add [query packs](logs/query-packs.md) with queries that are unique to your organization. This allows users who are familiar with the query language to make queries available to others in the organization.


## Workbooks
[Workbooks](./visualize/workbooks-overview.md) are the visualization platform of choice for Azure providing a flexible canvas for data analysis and creation of rich visual reports. Workbooks enable you to tap into multiple data sources from across Azure and combine them into unified interactive experiences. They are especially useful to prepare E2E monitoring views across multiple resources 

Insights use prebuilt workbooks to present users with critical health and performance information for a particular service. You can access a gallery of additional workbooks in the **Workbooks** tab of the Azure Monitor menu and create custom workbooks to meet requirements of your different users.

![Diagram that shows screenshots of three pages from a workbook, including Analysis of Page Views, Usage, and Time Spent on Page.](media/visualizations/workbook.png)

Common scenarios for workbooks include the following:

- Create an interactive report with parameters where selecting an element in a table will dynamically update associated charts and visualizations.
- Share a report with other users in your organization.
- Collaborate with other workbook authors in your organization using a public GitHub-based template gallery.



## Azure Dashboards
[Azure dashboards](../azure-portal/azure-portal-dashboards.md) are useful in providing a single pane of glass over your Azure infrastructure and services. While a workbook provides richer functionality, a dashboard can combine Azure Monitor data with data from other Azure services.

![Screenshot that shows an example of an Azure dashboard with customizable information.](media/visualizations/dashboard.png)

Here's a video walkthrough on creating dashboards:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4AslH]

Common scenarios for dashboards include the following:

- Create a dashboard combining a metrics graph and the results of a log query with operational data for related services.
- Share a dashboard with service owners through integration with [Azure role-based access control (Azure RBAC)](../role-based-access-control/overview.md).
  

See [Create and share dashboards of Log Analytics data](visualize/tutorial-logs-dashboards.md) for details on creating a dashboard that includes data from Azure Monitor Logs. See [Create custom KPI dashboards using Azure Application Insights](app/tutorial-app-dashboards.md) for details on creating a dashboard that includes data from Application Insights. 



## Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is useful for creating business-centric dashboards and reports, along with reports that analyze long-term KPI trends. You can [import the results of a log query](visualize/powerbi.md) into a Power BI dataset and then take advantage of its features, such as combining data from different sources and sharing reports on the web and mobile devices.

![Screenshot that shows an example Power B I report for I T operations.](media/visualizations/power-bi.png)

Common scenarios for Power BI include the following:

- Rich visualizations.
- Extensive interactivity, including zoom-in and cross-filtering.
- Ease of sharing throughout your organization.
- Integration with other data from multiple data sources.
- Better performance with results cached in a cube.



## Grafana
[Grafana](https://grafana.com/) is an open platform that excels in operational dashboards. It's useful for detecting, isolating, and triaging operational incidents. You can add the [Azure Monitor data source plug-in for Grafana](visualize/grafana-plugin.md) to your Azure subscription to have it visualize your Azure metric data.

![Screenshot that shows Grafana visualizations.](media/visualizations/grafana.png)


Common scenarios for Grafana include the following:

- Rich visualizations.
- A rich ecosystem of data sources.
- Data interactivity, including zoom-in.
- Support for parameters.

## Azure Monitor partners
Some Azure Monitor partners provide visualization functionality. For a list of partners that Microsoft has evaluated, see [Azure Monitor partner integrations](./partners.md). An Azure Monitor partner might provide out-of-the-box visualizations to save you time, although these solutions may have an additional cost.


## Custom application
You can then build your own custom websites and applications using metric and log data in Azure Monitor accessed through a REST API. This gives you complete flexibility in UI, visualization, interactivity, and features.


## Next steps
- [Define alerts and automated actions from Azure Monitor data](deploy-alerts.md)