---
title: Azure Monitor best practices - Analysis and visualizations
description: Guidance and recommendations for customizing visualizations beyond standard analysis features in Azure Monitor.
ms.topic: conceptual
author: AbbyMSFT
ms.author: abbyweisberg
ms.date: 06/07/2023
ms.reviewer: bwren
---

# Analyze and visualize monitoring data

This article describes built-in features for visualizing and analyzing collected data in Azure Monitor. Visualizations like charts and graphs can help you analyze your monitoring data to drill down on issues and identify patterns. You can create custom visualizations to meet the requirements of different users in your organization. 

## Built-in analysis features

This table describes Azure Monitor features that provide analysis of collected data without any configuration.

| Component | Description | Required training and/or configuration |
|---------|---------|--------|
| Overview page |Most Azure services have an **Overview** page in the Azure portal that includes a **Monitor** section with charts that show recent critical metrics. This information is intended for owners of individual services to quickly assess the performance of the resource. | This page is based on platform metrics that are collected automatically. No configuration is required. |
| [Metrics Explorer](essentials/metrics-getting-started.md)| You can use Metrics Explorer to interactively work with metric data and create metric alerts. You need minimal training to use Metrics Explorer, but you must be familiar with the metrics you want to analyze. | • Once data collection is configured, no other configuration is required.<br>• Platform metrics for Azure resources are automatically available.<br>• Guest metrics for virtual machines are available after an Azure Monitor agent is deployed to the virtual machine.<br>• Application metrics are available after Application Insights is configured. |
| [Log Analytics](logs/log-analytics-overview.md) | With Log Analytics, you can create log queries to interactively work with log data and create log search alerts.| Some training is required for you to become familiar with the query language, although you can use prebuilt queries for common requirements. You can also add [query packs](logs/query-packs.md) with queries that are unique to your organization. Then if you're familiar with the query language, you can build queries for others in your organization. |

## Built-in visualization tools

### Azure workbooks

[Azure workbooks](./visualize/workbooks-overview.md) provide a flexible canvas for data analysis and the creation of rich visual reports. You can use workbooks to tap into multiple data sources from across Azure and combine them into unified interactive experiences. They're especially useful to prepare end-to-end monitoring views across multiple Azure resources. Insights use prebuilt workbooks to present you with critical health and performance information for a particular service. You can access a gallery of workbooks on the **Workbooks** tab of the Azure Monitor menu and create custom workbooks to meet the requirements of your different users.

:::image type="content" source="media/visualizations/workbook.png" lightbox="media/visualizations/workbook.png" alt-text="Diagram that shows screenshots of three pages from a workbook, including Analysis of Page Views, Usage, and Time Spent on Page.":::

### Azure dashboards

[Azure dashboards](../azure-portal/azure-portal-dashboards.md) are useful in providing a "single pane of glass" of your Azure infrastructure and services. While a workbook provides richer functionality, a dashboard can combine Azure Monitor data with data from other Azure services.

:::image type="content" source="media/visualizations/dashboard.png" lightbox="media/visualizations/dashboard.png" alt-text="Screenshot that shows an example of an Azure dashboard with customizable information.":::

Here's a video about how to create dashboards:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4AslH]

### Grafana

[Grafana](https://grafana.com/) is an open platform that excels in operational dashboards. It's useful for:

* Detecting, isolating, and triaging operational incidents.
* Combining visualizations of Azure and non-Azure data sources. These sources include on-premises, third-party tools, and data stores in other clouds.

Grafana has popular plug-ins and dashboard templates for application performance monitoring(APM) tools such as Dynatrace, New Relic, and AppDynamics. You can use these resources to visualize Azure platform data alongside other metrics from higher in the stack collected by other tools. It also has AWS CloudWatch and GCP BigQuery plug-ins for multicloud monitoring in a single pane of glass.

All versions of Grafana include the [Azure Monitor datasource plug-in](visualize/grafana-plugin.md) to visualize your Azure Monitor metrics and logs.

[Azure Managed Grafana](../managed-grafana/overview.md) also optimizes this experience for Azure-native data stores such as Azure Monitor and Azure Data Explorer. In this way, you can easily connect to any resource in your subscription and view all resulting telemetry in a familiar Grafana dashboard. It also supports pinning charts from Azure Monitor metrics and logs to Grafana dashboards. Grafana includes out-of-the-box dashboards for Azure resources. [Create your first Azure Managed Grafana workspace](../managed-grafana/quickstart-managed-grafana-portal.md) to get started.

The [out-of-the-box Grafana Azure alerts dashboard](https://grafana.com/grafana/dashboards/15128-azure-alert-consumption/) allows you to view and consume Azure monitor alerts for Azure Monitor, your Azure datasources, and Azure Monitor managed service for Prometheus.

* For more information on define Azure Monitor alerts, see [Create a new alert rule](alerts/alerts-create-new-alert-rule.md).
* For Azure Monitor managed service for Prometheus, define your alerts using [Prometheus alert rules](alerts/prometheus-alerts.md) that are created as part of a [Prometheus rule group](essentials/prometheus-rule-groups.md), applied on the Azure Monitor workspace.

:::image type="content" source="media/visualizations/grafana.png" lightbox="media/visualizations/grafana.png" alt-text="Screenshot that shows Grafana visualizations.":::

### Power BI

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is useful for creating business-centric dashboards and reports, along with reports that analyze long-term KPI (Key Performance Indicator) trends. You can [import the results of a log query](./logs/log-powerbi.md) into a Power BI dataset. This will allow you to take advantage of features such as combining data from different sources and sharing reports on the web and mobile devices.

:::image type="content" source="media/visualizations/power-bi.png" lightbox="media/visualizations/power-bi.png" alt-text="Screenshot that shows an example Power BI report for IT operations." border="false":::

## Choose the right visualization tool

Azure Monitor suggests using Azure Managed Grafana for data visualizations and dashboards in cloud-native scenarios, such as Kubernetes and AKS, as well as multicloud, OSS, and third-party integrations. For other Azure scenarios, including Azure hybrid environments with Azure Arc, workbooks are the recommended option.

**Use Azure workbooks for:**

* Out of the box and customizable Azure-native reports
    * Within the Azure portal
    * Accessed exclusively via users' Azure RBAC (role-based access control) assignments
    * Managed with Azure Automation including ARM, Bicep, and Terraform
    * With no additional cost
* Most complete set of Azure datasources
* Azure-managed hybrid and edge environments
* Integrations with Azure actions and Azure Automation
* Creating custom reports based on Azure Monitor Insights
* Leveraging Azure GitHub community templates

**Use Azure Managed Grafana for:**

* Cloud-native environments monitored with Prometheus and CNCF (Cloud Native Computing Foundation) tools
* Multicloud and multi-platform environments
* Multi-tenancy and portability support
* Interoperability with open-source and third party tools
* Extensive flexibility combining data queries, query results, and performing open-ended client-side data processing
* Sharing dashboards outside of the Azure portal
* Leveraging open-source community dashboards

### Comparing Capabilities

✅ Supported<br>
☑️ Supported with requirements/limitations

| Service/Feature                                                                 | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| **Fully Azure Native Reporting**                                                | ✅                                                                 |                                             |
| In the Azure portal, no other entry points required                             | ✅                                                                 |                                             |
| Leveraged by Azure Monitor Insights                                             | ✅                                                                 |                                             |
| Visualizations are native ARM resources                                         | ✅                                                                 |                                             |
| At scale deployment and management of visualizations – ARM, Bicep, Terraform    | ✅                                                                 | ☑️<br>Only Azure CLI supported              |
| Access model Identical to Azure RBAC                                            | ✅                                                                 | ☑️<br>Not supported for Prometheus          |
| Included in Azure Cost                                                          | ✅                                                                 | ☑️<br>Additional per user pricing           |
| Reports and guided workflows in the Azure portal                                | ✅                                                                 |                                             |

| Complete set of  Azure datasources                                              | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| ARM, Azure Resource Health, Azure Service Health                                | ✅                                                                 | ☑️<br>Doesn't support ARM and Resource Health |
| Azure Monitor metrics, logs, traces, ARG, Azure Prom, ADX (Azure Data Explorer) | ✅                                                                 | ✅                                          |

| Azure Hybrid Environments                                                       | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| Azure Arc                                                                       | ✅                                                                 |                                             |
| Azure Stack Hub                                                                 | ✅                                                                 |                                             |
| Azure Edge and Azure IoT                                                        | ✅                                                                 |                                             |

| Azure Automation                                                                | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| Supports integrations actions and runbooks                                      | ✅                                                                 |                                             |

| Multi-tenancy and portability                                                   | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| Single view for multiple Azure tenants                                          | ☑️<br>Via Lighthouse                                               | ✅                                          |
| Visualizations available outside of the Azure portal                            |                                                                    | ✅                                          |
| On demand and scheduled reports outside of the Azure portal                     |                                                                    | ✅                                          |
| Azure service principal and Azure SQL Managed Instance auth                     |                                                                    | ✅                                          |

| Multicloud and multi-platform                                                   | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| AWS (Amazon Web Services), GCP (Google Cloud Platform), Oracle Cloud            | ☑️<br>Requires ARC and Azure Monitor instrumentation and ingestion | ✅                                          |
| OSS datasources – Zipkin, Jaeger, Tempo, Loki, Standalone Prometheus            |                                                                    | ✅                                          |
| Third party datasources  - AppDynamic, DataDog, New Relic, DataDog etc.         |                                                                    | ✅                                          |
| Generic database support – SQL, Mongo, Postgres, Oracle etc.                    |                                                                    | ✅                                          |
| &nbsp;                                                                          |                                                                    |                                             |

| Cloud Native                                                                    | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| Prometheus queries                                                              | ✅                                                                 | ✅                                          |
| Prometheus Builder (No Code Queries)                                            |                                                                    | ✅                                          |
| Prometheus  Community Dashboards                                                |                                                                    | ✅                                          |
| Prometheus + Exemplars                                                          |                                                                    | ✅                                          |
| Istio - Service Mesh                                                            |                                                                    | ✅                                          |

| Operational dashboards                                                          | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| Multiple queries in single pane                                                 |                                                                    | ✅                                          |
| Event and annotations combined with time series data                            |                                                                    | ✅                                          |
| Client-side Processing – Expressions & Transforms                               |                                                                    | ✅                                          |
| Dashboard playlists/rotation                                                    |                                                                    | ✅                                          |

### One table (V1)

| Service/Feature                                                                 | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| **Fully Azure Native Reporting**                                                | ✅                                                                 |                                             |
| In the Azure portal, no other entry points required                             | ✅                                                                 |                                             |
| Leveraged by Azure Monitor Insights                                             | ✅                                                                 |                                             |
| Visualizations are native ARM resources                                         | ✅                                                                 |                                             |
| At scale deployment and management of visualizations – ARM, Bicep, Terraform    | ✅                                                                 | ☑️<br>Only Azure CLI supported              |
| Access model Identical to Azure RBAC                                            | ✅                                                                 | ☑️<br>Not supported for Prometheus          |
| Included in Azure Cost                                                          | ✅                                                                 | ☑️<br>Additional per user pricing           |
| Reports and guided workflows in the Azure portal                                | ✅                                                                 |                                             |
| &nbsp; | | |
| **Complete set of  Azure datasources**                                              | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| ARM, Azure Resource Health, Azure Service Health                                | ✅                                                                 | ☑️<br>Doesn't support ARM and Resource Health |
| Azure Monitor metrics, logs, traces, ARG, Azure Prom, ADX (Azure Data Explorer) | ✅                                                                 | ✅                                          |
| &nbsp; | | |
| **Azure Hybrid Environments**                                                       | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| Azure Arc                                                                       | ✅                                                                 |                                             |
| Azure Stack Hub                                                                 | ✅                                                                 |                                             |
| Azure Edge and Azure IoT                                                        | ✅                                                                 |                                             |
| &nbsp; | | |
| **Azure Automation**                                                                | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| Supports integrations actions and runbooks                                      | ✅                                                                 |                                             |
| &nbsp; | | |
| **Multi-tenancy and portability**                                                   | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| Single view for multiple Azure tenants                                          | ☑️<br>Via Lighthouse                                               | ✅                                          |
| Visualizations available outside of the Azure portal                            |                                                                    | ✅                                          |
| On demand and scheduled reports outside of the Azure portal                     |                                                                    | ✅                                          |
| Azure service principal and Azure SQL Managed Instance auth                     |                                                                    | ✅                                          |
| &nbsp; | | |
| **Multicloud and multi-platform**                                                   | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| AWS (Amazon Web Services), GCP (Google Cloud Platform), Oracle Cloud            | ☑️<br>Requires ARC and Azure Monitor instrumentation and ingestion | ✅                                          |
| OSS datasources – Zipkin, Jaeger, Tempo, Loki, Standalone Prometheus            |                                                                    | ✅                                          |
| Third party datasources  - AppDynamic, DataDog, New Relic, DataDog etc.         |                                                                    | ✅                                          |
| Generic database support – SQL, Mongo, Postgres, Oracle etc.                    |                                                                    | ✅                                          |
| &nbsp; | | |
| **Cloud Native**                                                                    | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| Prometheus queries                                                              | ✅                                                                 | ✅                                          |
| Prometheus Builder (No Code Queries)                                            |                                                                    | ✅                                          |
| Prometheus  Community Dashboards                                                |                                                                    | ✅                                          |
| Prometheus + Exemplars                                                          |                                                                    | ✅                                          |
| Istio - Service Mesh                                                            |                                                                    | ✅                                          |
| &nbsp; | | |
| **Operational dashboards**                                                          | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| &nbsp; | | |
| Multiple queries in single pane                                                 |                                                                    | ✅                                          |
| Event and annotations combined with time series data                            |                                                                    | ✅                                          |
| Client-side Processing – Expressions & Transforms                               |                                                                    | ✅                                          |
| Dashboard playlists/rotation                                                    |                                                                    | ✅                                          |

### One table (V2)

### One table

| Service/Feature                                                                 | Workbooks                                                          | Azure Managed Grafana                       |
|:--------------------------------------------------------------------------------|:------------------------------------------------------------------:|:-------------------------------------------:|
| **Fully Azure Native Reporting**                                                | ✅                                                                 |                                             |
| In the Azure portal, no other entry points required                             | ✅                                                                 |                                             |
| Leveraged by Azure Monitor Insights                                             | ✅                                                                 |                                             |
| Visualizations are native ARM resources                                         | ✅                                                                 |                                             |
| At scale deployment and management of visualizations – ARM, Bicep, Terraform    | ✅                                                                 | ☑️<br>Only Azure CLI supported              |
| Access model Identical to Azure RBAC                                            | ✅                                                                 | ☑️<br>Not supported for Prometheus          |
| Included in Azure Cost                                                          | ✅                                                                 | ☑️<br>Additional per user pricing           |
| Reports and guided workflows in the Azure portal                                | ✅                                                                 |                                             |
| &nbsp; | | |
| &nbsp; | | |
| **Complete set of  Azure datasources**                                              | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| ARM, Azure Resource Health, Azure Service Health                                | ✅                                                                 | ☑️<br>Doesn't support ARM and Resource Health |
| Azure Monitor metrics, logs, traces, ARG, Azure Prom, ADX (Azure Data Explorer) | ✅                                                                 | ✅                                          |
| &nbsp; | | |
| &nbsp; | | |
| **Azure Hybrid Environments**                                                       | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| Azure Arc                                                                       | ✅                                                                 |                                             |
| Azure Stack Hub                                                                 | ✅                                                                 |                                             |
| Azure Edge and Azure IoT                                                        | ✅                                                                 |                                             |
| &nbsp; | | |
| &nbsp; | | |
| **Azure Automation**                                                                | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| Supports integrations actions and runbooks                                      | ✅                                                                 |                                             |
| &nbsp; | | |
| &nbsp; | | |
| **Multi-tenancy and portability**                                                   | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| Single view for multiple Azure tenants                                          | ☑️<br>Via Lighthouse                                               | ✅                                          |
| Visualizations available outside of the Azure portal                            |                                                                    | ✅                                          |
| On demand and scheduled reports outside of the Azure portal                     |                                                                    | ✅                                          |
| Azure service principal and Azure SQL Managed Instance auth                     |                                                                    | ✅                                          |
| &nbsp; | | |
| &nbsp; | | |
| **Multicloud and multi-platform**                                                   | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| AWS (Amazon Web Services), GCP (Google Cloud Platform), Oracle Cloud            | ☑️<br>Requires ARC and Azure Monitor instrumentation and ingestion | ✅                                          |
| OSS datasources – Zipkin, Jaeger, Tempo, Loki, Standalone Prometheus            |                                                                    | ✅                                          |
| Third party datasources  - AppDynamic, DataDog, New Relic, DataDog etc.         |                                                                    | ✅                                          |
| Generic database support – SQL, Mongo, Postgres, Oracle etc.                    |                                                                    | ✅                                          |
| &nbsp; | | |
| &nbsp; | | |
| **Cloud Native**                                                                    | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| Prometheus queries                                                              | ✅                                                                 | ✅                                          |
| Prometheus Builder (No Code Queries)                                            |                                                                    | ✅                                          |
| Prometheus  Community Dashboards                                                |                                                                    | ✅                                          |
| Prometheus + Exemplars                                                          |                                                                    | ✅                                          |
| Istio - Service Mesh                                                            |                                                                    | ✅                                          |
| &nbsp; | | |
| &nbsp; | | |
| **Operational dashboards**                                                          | **Workbooks**                                                          | **Azure Managed Grafana**                       |
| Multiple queries in single pane                                                 |                                                                    | ✅                                          |
| Event and annotations combined with time series data                            |                                                                    | ✅                                          |
| Client-side Processing – Expressions & Transforms                               |                                                                    | ✅                                          |
| Dashboard playlists/rotation                                                    |                                                                    | ✅                                          |

## Other options

Some Azure Monitor partners provide visualization functionality. An Azure Monitor partner might provide out-of-the-box visualizations to save you time, although these solutions might have an extra cost.

You can also build your own custom websites and applications using metric and log data in Azure Monitor using the REST API. The REST API gives you flexibility in UI, visualization, interactivity, and features.

## Next steps

* [Deploy Azure Monitor: Alerts and automated actions](best-practices-alerts.md)
* [Optimize costs in Azure Monitor](best-practices-cost.md)
