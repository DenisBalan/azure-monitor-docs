---
title: 'Monitor virtual machines with Azure Monitor: Configure monitoring'
description: Learn how to configure virtual machines for monitoring in Azure Monitor. Monitor virtual machines and their workloads with an Azure Monitor scenario.
ms.service: azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/21/2021
ms.reviewer: Xema Pathak

---

# Monitor virtual machines with Azure Monitor: Configure monitoring
This article is part of the set of content [Monitor virtual machines and their workloads in Azure Monitor](monitor-virtual-machine.md). It describes how to configure monitoring of your Azure and hybrid virtual machines in Azure Monitor.

> [!NOTE]
> This scenario describes how to implement complete monitoring of your Azure and hybrid virtual machine environment. To get started monitoring your first Azure virtual machine, see [Monitor Azure virtual machines](../../virtual-machines/monitor-vm.md).

This article discusses the most common Azure Monitor features to monitor the virtual machine host and its guest operating system. Depending on your particular environment and business requirements, you might not want to implement all features enabled by this configuration. Each section describes what features are enabled by that configuration and whether it potentially results in additional cost. This information will help you assess whether to perform each step of the configuration. For detailed pricing information, see [Azure Monitor pricing](https://azure.microsoft.com/pricing/details/monitor/).

A general description of each feature enabled by this configuration is provided in the [overview for the scenario](monitor-virtual-machine.md). That article also includes links to content that provides a detailed description of each feature to further help you assess your requirements.

> [!NOTE]
> The features enabled by the configuration support monitoring workloads running on your virtual machine. But depending on your particular workloads, you'll typically require additional configuration. For details on this additional configuration, see [Workload monitoring](monitor-virtual-machine-workloads.md).


## Configuration overview
The following table lists the steps that must be performed for this configuration. Each one links to the section with the detailed description of that configuration step.

| Step | Description |
|:---|:---|
| [No configuration](#no-configuration) | Activity log and platform metrics for the Azure virtual machine hosts are automatically collected with no configuration. |
| [Create Log Analytics workspace](#create-and-prepare-a-log-analytics-workspace) | Create a Log Analytics workspace to support log collection and VM insights if you choose to use it. Depending on your particular requirements, you might configure multiple workspaces. |
| [Send Activity log to Log Analytics workspace](#send-an-activity-log-to-a-log-analytics-workspace) | Send the Activity log to the workspace to analyze it with other log data. |
| [Prepare hybrid machines](#prepare-hybrid-machines) | Hybrid machines either need the server agents enabled by Azure Arc installed so they can be managed like Azure virtual machines or must have their agents installed manually. |
| [Deploy Azure Monitor agent](#deploy-azure-monitor-agent) | Optionally, onboard machines to VM insights, which deploys required agents and begins collecting data from the guest operating system. |
| Configure data collection | 
| [Create alert rules](#create-alert-rules) |



## No configuration
Azure Monitor provides a basic level of monitoring for Azure virtual machines at no cost and with no configuration.

- Platform metrics for Azure virtual machines include important metrics such as CPU, network, and disk utilization. They can be viewed on the [Overview page](monitor-virtual-machine-analyze.md#single-machine-experience) for the machine in the Azure portal and support metric alerts. See [Enable recommended alert rules for Azure virtual machine](../azure-monitor/vm/tutorial-monitor-vm-alert-recommended.md).
- The Activity log is collected automatically and includes the recent activity of the machine, such as any configuration changes and when it was stopped and started.

## Create a Log Analytics workspace
You require at least one Log Analytics workspace to collect telemetry from the Azure Monitor agent. There's no cost for the workspace, but you do incur ingestion and retention costs when you collect data. For more information, see [Azure Monitor Logs pricing details](../logs/cost-logs.md).

Many environments use a single workspace for all their virtual machines and other Azure resources they monitor. You can even share a workspace used by [Microsoft Defender for Cloud and Microsoft Sentinel](monitor-virtual-machine-security.md), although many customers choose to segregate their availability and performance telemetry from security data. If you're getting started with Azure Monitor, start with a single workspace and consider creating more workspaces as your requirements evolve.

For complete details on logic that you should consider for designing a workspace configuration, see [Design a Log Analytics workspace configuration](../logs/workspace-design.md).

## Workspace permissions
The access mode of the workspace defines which users can access different sets of data. For details on how to define your access mode and configure permissions, see [Manage access to log data and workspaces in Azure Monitor](../logs/manage-access.md). If you're just getting started with Azure Monitor, consider accepting the defaults when you create your workspace and configure its permissions later.

### Multihoming agents
Multihoming refers to a virtual machine that connects to multiple workspaces. There's typically little reason to multihome agents for Azure Monitor alone. Having an agent send data to multiple workspaces most likely creates duplicate data in each workspace, which increases your overall cost. You can combine data from multiple workspaces by using [cross-workspace queries](../logs/cross-workspace-query.md) and [workbooks](../visualizations/../visualize/workbooks-overview.md).

One reason you might consider multihoming, though, is if you have an environment with Microsoft Defender for Cloud or Microsoft Sentinel stored in a workspace that's separate from Azure Monitor. A machine being monitored by each service needs to send data to each workspace. 

## Send Activity log to a Log Analytics workspace
You can view the platform metrics and Activity log collected for each virtual machine host in the Azure portal. Send this data into the same Log Analytics workspace as VM insights to analyze it with the other monitoring data collected for the virtual machine. You might have already done this task when you configured monitoring for other Azure resources because there's a single Activity log for all resources in an Azure subscription.

There's no cost for ingestion or retention of Activity log data. For details on how to create a diagnostic setting to send the Activity log to your Log Analytics workspace, see [Create diagnostic settings](../essentials/diagnostic-settings.md).


## Prepare hybrid machines
A hybrid machine is any machine not running in Azure. It's a virtual machine running in another cloud or hosted provider or a virtual or physical machine running on-premises in your datacenter. Use [Azure Arc-enabled servers](../../azure-arc/servers/overview.md) on hybrid machines so you can manage them similarly to your Azure virtual machines. You can use VM insights in Azure Monitor to use the same process to enable monitoring for Azure Arc-enabled servers as you do for Azure virtual machines. For a complete guide on preparing your hybrid machines for Azure, see [Plan and deploy Azure Arc-enabled servers](../../azure-arc/servers/plan-at-scale-deployment.md). This task includes enabling individual machines and using [Azure Policy](../../governance/policy/overview.md) to enable your entire hybrid environment at scale.

There's no additional cost for Azure Arc-enabled servers, but there might be some cost for different options that you enable. For details, see [Azure Arc pricing](https://azure.microsoft.com/pricing/details/azure-arc/). There is a cost for the data collected in the workspace after your hybrid machines are onboarded, but this is the same as for an Azure virtual machine.

## Network requirements
The Azure Monitor agent for both Linux and Windows communicates outbound to the Azure Monitor service over TCP port 443. The Dependency agent uses the Azure Monitor agent for all communication, so it doesn't require any another ports. For details on how to configure your firewall and proxy, see [Network requirements](../agents/log-analytics-agent.md#network-requirements).

:::image type="content" source="media/monitor-virtual-machines/network-diagram.png" alt-text="Diagram that shows the network." lightbox="media/monitor-virtual-machines/network-diagram.png":::

### Gateway
With the Log Analytics gateway, you can channel communications from your on-premises machines through a single gateway. You can't use the Azure Arc-enabled server agents with the Log Analytics gateway though. If your security policy requires a gateway, you'll need to manually install the agents for your on-premises machines. For details on how to configure and use the Log Analytics gateway, see [Log Analytics gateway](../agents/gateway.md).

### Azure Private Link
By using Azure Private Link, you can create a private endpoint for your Log Analytics workspace. After it's configured, any connections to the workspace must be made through this private endpoint. Private Link works by using DNS overrides, so there's no configuration requirement on individual agents. For details on Private Link, see [Use Azure Private Link to securely connect networks to Azure Monitor](../logs/private-link-security.md). For specific guidance on configuring private link for you virtual machines, see [Enable network isolation for the Azure Monitor agent](../agents/azure-monitor-agent-data-collection-endpoint.md).
### Machines that can't use Azure Arc-enabled servers
If you have any hybrid machines that match the following criteria, they won't be able to use Azure Arc-enabled servers:

- The operating system of the machine isn't supported by the server agents enabled by Azure Arc. For more information, see [Supported operating systems](../../azure-arc/servers/prerequisites.md#supported-operating-systems).
- Your security policy doesn't allow machines to connect directly to Azure. The Azure Monitor agent can use the [Log Analytics gateway](../agents/gateway.md) whether or not Azure Arc-enabled servers are installed. The server agents enabled by Azure Arc though must connect directly to Azure.

You still can monitor these machines with Azure Monitor, but you need to manually install their agents. To manually install the Log Analytics agent and Dependency agent on those hybrid machines, see [Enable VM insights for a hybrid virtual machine](vminsights-enable-hybrid.md).

> [!NOTE]
> The private endpoint for Azure Arc-enabled servers is currently in public preview. The endpoint allows your hybrid machines to securely connect to Azure by using a private IP address from your virtual network.

## Deploy Azure Monitor agent
There are multiple methods for deploying the Azure Monitor agent top your virtual machines depending on whether you use the [agent extension](../agents/azure-monitor-agent-manage.md) or the [client installer](../agents/azure-monitor-agent-windows-client.md). The client installer is only required for machines outside of Azure that don't use Azure Arc. For different options deploying the agent on a single machine or as part of a script, see [Install](../agents/azure-monitor-agent-manage?tabs=azure-portal.md#install).


### VM insights
VM insights provides simplified onboarding in the Azure portal. With a single click for a particular machine, it installs the Azure Monitor agent and Dependency agent, connects to a workspace, and starts collecting performance data.


### Azure Policy

If you have a significant number of virtual machines, you should deploy the agent using Azure Policy as described in [Use Azure Policy](../agents/azure-monitor-agent-manage?tabs=azure-portal.md#use-azure-policy). This will ensure that the agent is automatically added to existing virtual machines and any new ones that you deploy.


## Enable VM insights

 You can start using performance views and workbooks to analyze trends for a variety of guest operating system metrics, enable the map feature of VM insights for analyzing running processes and dependencies between machines, and collect the data required for you to create a variety of alert rules.

The dependency agent uses the Azure Monitor agent to deliver the data that it collects, so there are no additional network or firewall considerations.

> [!NOTE]
> VM insights gives you an option to install either the Azure Monitor agent or Log Analytics agent, but the Azure Monitor agent is recommended. It only installs the Dependency agent if you choose to enable the Map feature.

You can enable VM insights on individual machines by using the same methods for Azure virtual machines and Azure Arc-enabled servers. These methods include onboarding individual machines with the Azure portal or Azure Resource Manager templates or enabling machines at scale by using Azure Policy. There's no direct cost for VM insights, but there is a cost for the ingestion and retention of data collected in the Log Analytics workspace.

For different options to enable VM insights for your machines, see [Enable VM insights overview](vminsights-enable-overview.md). To create a policy that automatically enables VM insights on any new machines as they're created, see [Enable VM insights by using Azure Policy](vminsights-enable-policy.md).


## Next steps

* [Analyze monitoring data collected for virtual machines](monitor-virtual-machine-analyze.md)
* [Create alerts from collected data](monitor-virtual-machine-alerts.md)
* [Monitor workloads running on virtual machines](monitor-virtual-machine-workloads.md)



The performance charts in VM insights depend on the VM insights DCR that sends client performance data to a Log Analytics workspace. This allows you to use use KQL queries to analyze the data, which is what the performance charts are based on. You may choose to send thr performance data to Azure Metrics either instead of or in addition to the workspace. This saves you the cost of sending data to the workspace and allows you to use metrics explorer and metric alerts with the data.