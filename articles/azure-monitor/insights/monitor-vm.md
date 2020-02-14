---
title: Monitor Azure virtual machines with Azure Monitor
description: Describes how to collect and analyze monitoring data from virtual machines in Azure using Azure Monitor.
ms.service:  azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/08/2019

---

# Monitoring Azure virtual machines with Azure Monitor
Virtual machines can be monitored for availability and performance like any [other Azure resource](monitor-azure-resource.md), but they're unique from other resources since you also need to monitor the guest operating and system and the workloads that run in it. This article describes how to use Azure Monitor to collect monitoring data from Azure VMs and to analyze and alert on this data to maintain the health and performance of your virtual machines. 

## What is Azure Monitor?
Azure Monitor is a full stack monitoring service in Azure that provides a complete set of features to monitor your Azure resources in addition to resources in other clouds and on-premises. The [Azure Monitor data platform](../platform/data-platform.md) collects data into [logs](../platform/data-platform-logs.md) and [metrics](../platform/data-platform-metrics.md) where they can be analyzed together using a complete set of monitoring tools as described in the following sections.

- [What can you do with Azure Monitor Metrics?](../platform/data-platform-metrics.md#what-can-you-do-with-azure-monitor-metrics)
- [What can you do with Azure Monitor Logs?](../platform/data-platform-logs.md#what-can-you-do-with-azure-monitor-logs)

As soon as you create an Azure resource, Azure Monitor is enabled and starts collecting metrics and activity logs which you can [view and analyze in the Azure portal](#monitoring-in-the-azure-portal). With some configuration, you can gather additional monitoring data and enable additional features. See [Monitoring Data](#monitoring-data) below for details on any configuration requirements.

## Monitoring data
Monitoring data in Azure Monitor is collected into either [Logs](../platform/data-platform-logs.md) or [Metrics](../platform/data-platform-metrics.md). Each has [unique benefits](../platform/data-platform.md#compare-azure-monitor-metrics-and-logs) and each supports a different set of Azure Monitor features. 


## Initial monitoring
As oon 




## Azure Monitor for VMs


## Agents
To collect data from the guest operating system of a virtual machine, you require an agent. The agent runs locally on each virtual machine and sends data to Azure Monitor. Multiple agents are available for Azure Monitor with each collecting different data and writing data to different locations. Which agents you use will depend on your particular requirements. Get a detailed comparison of the different agents at [Overview of the Azure Monitor agents](../platform/agents-overview/md). 

- [Azure Diagnostic extension](../platform/agents-overview#azure-diagnostic-extension.md) - Available for Azure Monitor virtual machines only. Can collect data to mulitple locations but primarily used to collect guest performance data into Azure Monitor Metrics.
- [Telegraf agent](../platform/collect-custom-metrics-linux-telegraf.md) - Collect performance data from Linux VMs into Azure Monitor Metrics.
- [Log Analytics agent](../platform/agents-overview#log-analytics-agent.md) - Available for virtual machines in Azure, other cloud environments, and on-premises. Collects data to Azure Monitor Logs. Supports Azure Monitor for VMs and monitoring solutions. This is the same agent used for System Center Operations Manager.
- [Dependency agent](../platform/agents-overview#dependency-agent.md) - Collects data about the processes running on the virtual machine and their dependencies. Relies on the Log Analytics agent and supports Azure Monitor for VMs and the Service Map solution.


## Azure virtual machines

Virtual machines in Azure generate the same Activity log and platform metrics for the host as other Azure resources as described in [Monitoring data](monitor-azure-resource.md#monitoring-data). Azure Virtual machines do not generate [resource logs](../platform/platform-logs-overview.md) which provide insight into operations that were performed within an Azure resource. To collect monitoring data from the guest operating system of a virtual machine, you require an agent as described below.

- [Platform metrics](../platform/data-platform-metrics.md) - Numerical values that are automatically collected at regular intervals and describe some aspect of a resource at a particular time. Platform metrics are collected for the virtual machine host, but you require the Azure Diagnostics extension to collect metrics for the guest operating system.
- [Activity log](../platform/platform-logs-overview.md) - Provides insight into the operations on each Azure resource in the subscription from the outside (the management plane). For a virtual machine, this includes such information as when it was started and when any configuration changes were made to the host.

Azure Monitor agents supported by Azure virtual machines include:

| Agent | Destinations | Description |
|:---|:---|:---|
| Azure Diagnostic extension | Azure Monitor Metrics<br>Azure Storage<br>Event Hubs | Virtual machine extension that collects . |
| Log Analytics agent | Windows<br>Linux | 
- Dependency agent

## Virtual machines outside of Azure
The Log Analytics agent can be installed on virtual machines in other cloud environments and on-premises. This allows you to collect 






### Deploying agents




## Collecting data


## Metrics

### Host metrics

Namespace: Virtual Machine Host

## Monitoring workloads



## Azure Monitor for VMs
[Azure Monitor for VMs](vminsights-overview.md) is an [insight](insights-overview.md) in Azure Monitor that provides the following additional value over standard Azure Monitor features.

- Pre-defined trending performance charts and workbooks that allow you to analyze core performance metrics from the guest VM operating system.
- Dependency map that displays the interconnected components with the VM from various resource groups and subscriptions.

When you enable monitoring for a VM, it will install the 


## Enabling monitoring for Azure virtual machines
Azure Monitor will collect the Activity log and platform metrics for Azure virtual machines hosts automatically. 

To enable the Diagnostics extension:

1. Select the virtual machine in the Azure portal.
2. Select **Diagnostic settings** in the **Settings** section of the menu.
3. 


## Integration with System Center Operations Manager
System Center Operations Manager 

[Cloud Monitoring Guide](https://docs.microsoft.com/azure/cloud-adoption-framework/manage/monitor/cloud-models-monitor-overview)

## Next steps

* See [Supported services, schemas, and categories for Azure Resource Logs](../platform/diagnostic-logs-schema.md) for details of resource logs for different Azure services.  
