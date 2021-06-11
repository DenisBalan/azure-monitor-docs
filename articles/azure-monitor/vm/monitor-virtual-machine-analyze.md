---
title: Monitor Azure virtual machines with Azure Monitor - Analyze monitoring data
description: Describes how to collect and analyze monitoring data from virtual machines in Azure using Azure Monitor.
ms.service:  azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/11/2021

---

# Monitoring virtual machines with Azure Monitor - Analyze monitoring data
Once you’ve enabled VM insights on your virtual machines, data will be available for analysis. This article describes the different features of Azure Monitor that allow you to analyze the health and performance of your virtual machines. Several of these features provide a different experience depending on whether you're analyzing a single machine or multiple. Each experience is described here with any unique behavior of each feature depending on which experience is being used.

> [!NOTE]
> This article is part of the [Monitoring virtual machines and their workloads in Azure Monitor scenario](monitor-virtual-machine.md). It describes how to analyze monitoring data for your virtual machines after you've completed their configuration.


> [!NOTE]
> This article includes guidance on analyzing data that's collected by Azure Monitor and VM insights. For data that you configure to monitor workloads running on virtual machines, see [Monitor workloads](monitor-virtual-machine-workloads.md).



## Single machine experience
Access the single machine analysis experience from the **Monitoring** section of the menu in the Azure portal for each Azure virtual machine and Azure Arc enabled server. These options either limit the data that you're viewing to that machine or at least sets an initial filter for it. This allows you to focus on that particular machine, viewing its current performance and its trending over time, helping to identify any issues it maybe experiencing.

![Monitoring in the Azure portal](media/monitor-vm-azure/monitor-menu.png)

- **Overview page.**  Click the **Monitoring** tab to display [platform metrics](../essentials/data-platform-metrics.md) for the virtual machine host. This gives you a quick view of the trend over different time periods for important metrics such as CPU, network, and disk. Since these are host metrics though, counters from the guest operating system such as memory aren't included. Click on a graph to work with this data in [metrics explorer](../essentials/metrics-getting-started.md) where you can perform different aggregations and add additional counters for analysis.


- **Activity log.** [Activity log](../essentials/activity-log.md#view-the-activity-log) entries filtered for the current virtual machine. Use this to view the recent activity of the machine such as any configuration changes and when it's been stopped and started. 


- **Insights.**  Open [VM insights](../vm/vminsights-overview.md) with the map for the current virtual machine selected. This shows you running processes on the machine, dependencies on other machines and external processes. See [Use the Map feature of VM insights to understand application components](vminsights-maps.md#view-a-map-from-a-vm) for details on using the map view for a single machine. 

    Click on the **Performance** tab to view trends of critical performance counters over different periods of time. When you open VM insights from the virtual machine menu, you also have a table with detailed metrics for each disk. See [How to chart performance with VM insights](vminsights-performance.md#view-performance-directly-from-an-azure-vm) for details on using the map view for a single machine. 

- **Alerts.**  Views [alerts](../alerts/alerts-overview.md) for the current virtual machine. These are only alerts that use the machine as the target resource, so there may be other alerts associated with it. You may need to use the **Alerts** option in the Azure Monitor menu to view alerts for all resources. See [Monitoring virtual machines with Azure Monitor - Alerts](monitor-virtual-machine-alerts.md) for details.

- **Metrics.**  Open metrics explorer with the scope set to the machine. This is the same as selecting one of the performance charts from the **Overview** page except that the metric isn't already added.

- **Diagnostic settings.** Enable and configure [diagnostics extension](../agents/diagnostics-extension-overview.md) for the current virtual machine. Note that this option is different than the **Diagnostic settings** option for other Azure resources. See [Monitor virtual machines with Azure Monitor - Configure monitoring](monitor-virtual-machine-onboard.md#send-guest-performance-data-to-metrics-optional) for more information on the diagnostic extension.  


- **Advisor recommendations.** Recommendations for the current virtual machine from [Azure Advisor](../../advisor/index.yml).

- **Logs.**  Open [Log Analytics](../logs/log-analytics-overview.md) with the [scope](../logs/scope.md) set to the current virtual machine. This allows you to select from a variety of existing queries to drill into  log and performance data for only this machine. 


- **Connection monitor.**  Open [Network Watcher Connection Monitor](../../network-watcher/connection-monitor-overview.md) to monitor connections between the current virtual machine and other virtual machines. 


- **Workbooks.** Open the workbook gallery with the VM insights workbooks for single machines. See [VM insights workbooks](vminsights-workbooks.md#vm-insights-workbooks) for a list of the VM insights workbooks designed for individual machines.  |

### Multiple machine experience
Access the mulitple machine analysis experience from the **Monitor** menu in the Azure portal for each Azure virtual machine and Azure Arc enabled server. These options provide access to all data so that you can select the virtual machines that you're interested in comparing.


![Monitoring in the Azure portal](media/monitor-vm-azure/monitor-menu.png)

- **Activity log.**  [Activity log](../essentials/activity-log.md#view-the-activity-log) entries filtered for all resources. Create a filter for a **Resource Type** of Virtual Machines or Virtual Machine Scale Sets to view events for all of your machines.

- **Alerts.** View [alerts](../alerts/alerts-overview.md) for all resources this includes alerts related to virtual machines but that are associated with the workspace. Create a filter for a **Resource Type** of Virtual Machines or Virtual Machine Scale Sets to view alerts for all of your machines. 


- **Metrics.**  Open [metrics explorer](../essentials/metrics-getting-started.md) with no scope selected. This is particularly useful when you want to compare trends across multiple machines. Select a subscription or a resource group to quickly add a group of machines to analyze together. 

- **Logs** Open [Log Analytics](../logs/log-analytics-overview.md) with the [scope](../logs/scope.md) set to the workspace.  This allows you to select from a variety of existing queries to drill into log and performance data for all machines. Or create a custom query to perform additional analysis.


- **Workbooks**  Open the workbook gallery with the VM insights workbooks for multiple machines. See [VM insights workbooks](vminsights-workbooks.md#vm-insights-workbooks) for a list of the VM insights workbooks designed for multiple machines. 

- **Virtual Machines.**  Open [VM insights](../vm/vminsights-overview.md) with the **Get Started** tab open. This displays all machines in your Azure subscription,identifying which are being monitored. Use this view to onboard individual machines that aren't already being monitored.

    Click on the **Performance** tab to compare trends of critical performance counters for multiple machines over different periods of time. Select all machines in a subscription or resource groupto include in the view. See [How to chart performance with VM insights](vminsights-performance.md#view-performance-directly-from-an-azure-vm) for details on using the map view for a single machine.

    Click on the Map tab to view running processes on machines, dependencies between machines and external processes. Select all machines in a subscription or resource group, or inspect the data for a single machine. See [Use the Map feature of VM insights to understand application components](vminsights-maps.md#view-a-map-from-azure-monitor) for details on using the map view for multiple machines.  |
 
## Compare Metrics and Logs
For many features of Azure Monitor, you don't need to understand the different types of data it uses and where it's stored. You can use VM insights, for example, without any understanding of what data is being used to populate the Performance view, Map view, and workbooks. You just focus on the logic that you're analyzing. As you dig deeper though, there are cases where you will need to understand the difference between [Metrics](../essentials/data-platform-metrics.md) and [Logs](../logs/data-platform-logs.md)  since different features of Azure Monitor use different kinds of data, and the type of alerting that you use for a particular scenario will depend on having that data available in a particular location.


This can confusing if you're new to Azure Monitor, but the following details should help you understand the differences between the types of data. 

- Any non-numeric data such as events is stored in Logs. Metrics can only include numeric data that's sampled at regular intervals.
- Numeric data can be stored in both Metrics and Logs so it can be analyzed in different ways and support different types of alerts.
- Performance data from the guest operating system will be sent to Logs by VM insights using the Log Analytics agent and Dependency agent.
- Performance data from the guest operating system will only be sent to Metrics if the diagnostic extension is installed. The diagnostic extension is only available for Azure virtual machines. See [Send guest performance data to Metrics (optional)](monitor-virtual-machine-onboard.md#send-guest-performance-data-to-metrics-optional)
- A specific set of performance counters is available for metric alerts even though the data is stored in Logs. These counters cannot be analyzed with Metrics explorer. See [Monitoring virtual machines with Azure Monitor - Alerts](monitor-virtual-machine-alerts.md#types-of-alert-rules) for details.

> [!NOTE]
> The Azure Monitor Agent, currently in public preview, will replace the Log Analytics agent and have the ability to send client performance data to both Logs and Metrics. When this agent becomes generally available with VM insights, then all performance data will sent to both Logs and Metrics significantly simplifying this logic. 


## Analyze data with VM insights
VM insights includes multiple performance charts that help you quickly get a status of the operation of your monitored machines, their trending performance over time, and  dependencies between machines and processes. It also offers a consolidated view of different aspects of any monitored machine such as its properties and events collected in the Log Analytics workspace.

The **Get Started** tab displays all machines in your Azure subscription,identifying which are being monitored. Use this view to quickly identify which machines aren't being monitored and to onboard individual machines that aren't already being monitored.

The **Performance** view includes multiple charts with several key performance indicators (KPIs) to help you determine how well machines are performing. The charts show resource utilization over a period of time so you can identify bottlenecks, anomalies, or switch to a perspective listing each machine to view resource utilization based on the metric selected. See [How to chart performance with VM insights](vminsights-performance.md) for details on using the performance view.

![Performance view in VM insights]()

Use the **Map** view to see running processes on machines and their dependencies on other machines and external processes. You can change the time window for the view to determine if these dependencies have changed from another time period.  See [Use the Map feature of VM insights to understand application components](vminsights-maps.md) for details on using the map view.

![Map view in VM insights]()

## Analyze metric data with metrics explorer
Metrics explorer allows you plot charts, visually correlate trends, and investige spikes and dips in metrics' values. See [Getting started with Azure Metrics Explorer](../essentials/metrics-getting-started.md) for details on using this tool. 

> [!NOTE]
> You will only have host metrics for a machine unless the diagnostic extension is installed. This is the same data collected by VM insights, but that data is stored in Logs and note available in metrics explorer. For machines without the diagnostic extension installed, you'll see an option in the **Metric Namespace** dropdown called **Enable new guest memory metrics**. This option describes how to add the diagnostic extension to the machine to collect guest metrics. These same values are collected in Logs when VM insights is enabled.


There are three namespaces used by virtual machines:

| Namespace | Description | Requirement |
|:---|:---|:---|
| Virtual Machine Host | Host metrics automatically collected for all Azure virtual machines. Detailed list of metrics at [Microsoft.Compute/virtualMachines](../essentials/metrics-supported.md#microsoftcomputevirtualmachines). | Collected automatically with no configuration required. |
| Guest (classic) | Limited set of guest operating system and application performance data. Available in metrics explorer but not other Azure Monitor features such as metric alerts.  | [Diagnostic extension](../agents/diagnostics-extension-overview.md) installed. Data is read from Azure storage.  |
| Virtual Machine Guest | Guest operating system and application performance data available to all Azure Monitor features using metrics. | For Windows, [diagnostic extension installed](../agents/diagnostics-extension-overview.md) installed with Azure Monitor sink enabled. For Linux, [Telegraf agent installed](../essentials/collect-custom-metrics-linux-telegraf.md). |

> [!NOTE]
> For machines without the diagnostic extension installed, you'll see an option in the **Metric Namespace** dropdown called **Enable new guest memory metrics**. This option describes how to add the diagnostic extension to the machine to collect guest metrics. These same values are collected in Logs when VM insights is enabled.

## Analyze log data with log queries
Log Analytics allows you to perform custom analysis of your log data. Use Log Analytics when you want to dig deeper into the data used to create the views in VM insights. You may want to analyze different logic and aggregations of that data, correlate security data collected by Azure Security Center and Azure Sentinel with your health and availability data, or work with data collected for your [workloads](monitor-virtual-machine-workloads.md).

![Log query in Log Analytics]()

You don't necessarily need to understand how to write a log query to use Log Analytics. There are multiple prebuilt queries that you can select and either run without modification or use as a start to a custom query. Click **Queries** at the top of the Log Analytics screen and view queries with a **Resource type** of **Virtual machines** or **Virtual machine Scale Sets**. See [Using queries in Azure Monitor Log Analytics](../logs/queries.md) for information on using these queries and [Log Analytics tutorial](../logs/log-analytics-tutorial.md) for a complete tutorial on using Log Analytics to run queries and work with their results.

![Sample virtual machine queries]()

When you launch the Launch Log Analytics from VM insights using the properties pane in either the **Performance** or **Map** view, it lists the tables that have data for the selected computer. Click on a table to open Log Analytics with a simple query that returns all records in that table for the selected computer. Work with these results or modify the query for more complex analysis. The [scope](../log/../logs/scope.md) set to the workspace meaning that you have access data for all computers using that workspace. 


## Analyze metric data with metrics explorer
Select  **Metrics** from the virtual machine's menu to open metrics explorer which allows you to analyze metric data for virtual machines. See [Getting started with Azure Metrics Explorer](../essentials/metrics-getting-started.md) for details on using this tool. 

There are three namespaces used by virtual machines:

| Namespace | Description | Requirement |
|:---|:---|:---|
| Virtual Machine Host | Host metrics automatically collected for all Azure virtual machines. Detailed list of metrics at [Microsoft.Compute/virtualMachines](../essentials/metrics-supported.md#microsoftcomputevirtualmachines). | Collected automatically with no configuration required. |
| Guest (classic) | Limited set of guest operating system and application performance data. Available in metrics explorer but not other Azure Monitor features such as metric alerts.  | [Diagnostic extension](../agents/diagnostics-extension-overview.md) installed. Data is read from Azure storage.  |
| Virtual Machine Guest | Guest operating system and application performance data available to all Azure Monitor features using metrics. | For Windows, [diagnostic extension installed](../agents/diagnostics-extension-overview.md) installed with Azure Monitor sink enabled. For Linux, [Telegraf agent installed](../essentials/collect-custom-metrics-linux-telegraf.md). |


## Workbooks
[Workbooks](../visualize/workbooks-overview.MD) provide interactive reports in the Azure portal, combining different kinds of data into a single view. Workbooks combine text, [log queries](/azure/data-explorer/kusto/query/), metrics, and parameters into rich interactive reports. Workbooks are editable by any other team members who have access to the same Azure resources.

Workbooks are helpful for scenarios such as:

* Exploring the usage of your virtual machine when you don't know the metrics of interest in advance: CPU utilization, disk space, memory, network dependencies, etc. Unlike other usage analytics tools, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.
* Explaining to your team how a recently provisioned VM is performing, by showing metrics for key counters and other log events.
* Sharing the results of a resizing experiment of your VM with other members of your team. You can explain the goals for the experiment with text, then show each usage metric and analytics queries used to evaluate the experiment, along with clear call-outs for whether each metric was above- or below-target.
* Reporting the impact of an outage on the usage of your VM, combining data, text explanation, and a discussion of next steps to prevent outages in the future.

VM insights includes the following workbooks. You can use these workbooks or use them as a start to create custom workbooks to address your particular requirements.

### Single virtual machine

| Workbook | Description |
|----------|-------------|
| Performance | Provides a customizable version of the Performance view that leverages all of the Log Analytics performance counters that you have enabled. | 
| Connections | Provides an in-depth view of the inbound and outbound connections from your VM. | 

### Multiple virtual machines

| Workbook | Description |
|----------|-------------|
| Performance | Provides a customizable version of the Top N List and Charts view in a single workbook that leverages all of the Log Analytics performance counters that you have enabled.|
| Performance counters | A Top N chart view across a wide set of performance counters. |
| Connections | Connections provides an in-depth view of the inbound and outbound connections from your monitored VMs. |
| Active Ports | Provides a list of the processes that have bound to the ports on the monitored VMs and their activity in the chosen timeframe. |
| Open Ports | Provides the number of ports open on your monitored VMs and the details on those open ports. |
| Failed Connections | Display the count of failed connections on your monitored VMs, the failure trend, and if the percentage of failures is increasing over time. |
| Security and Audit | An analysis of your TCP/IP traffic that reports on overall connections, malicious connections, where the IP endpoints reside globally.  To enable all features, you will need to enable Security Detection. |
| TCP Traffic | A ranked report for your monitored VMs and their sent, received, and total network traffic in a grid and displayed as a trend line. |
| Traffic Comparison | Compare network traffic trends for a single machine or a group of machines. |
| Log Analytics agent | Analyze the health of your agents including the number of agents connecting to a workspace, which are unhealthy, and the affect of the agent on the performance of the machine. This workbook isn't available from VM insights like the other workbooks. Go to **Workbooks** in the Azure Monitor menu and select **Public Templates**. |

See [Create interactive reports VM insights with workbooks](vminsights-workbooks.md) for detailed instructions on creating your own custom workbooks.



## Next steps

* [Learn how to analyze data in Azure Monitor logs using log queries.](../logs/get-started-queries.md)
* [Learn about alerts using metrics and logs in Azure Monitor.](../alerts/alerts-overview.md)





## Analyze log data with Log Analytics
VM insights provides 

VM insights collects a predefined set of  set of performance counters that are written to the *InsightsMetrics* table. 


[How to query logs from VM insights](../vm/vminsights-log-search.md) for details.

| Table | Description | Source|
|:---|:---|:---|
| [VMBoundPort](/azure/azure-monitor/reference/tables/vmboundport) | Traffic for open server ports on the monitored machine. | VM Insights |
| [VMComputer](/azure/azure-monitor/reference/tables/vmcomputer) | Inventory data for servers collected by the Service Map and VM Insights solutions using the Dependency agent and Log analytics agent. | VM insights |
| [VMConnection](/azure/azure-monitor/reference/tables/vmconnection) | Traffic for inbound and outbound connections to and from monitored computers. | VM insights |
| [VMProcess](/azure/azure-monitor/reference/tables/vmprocess) | Process data for servers collected by the Service Map and VM Insights solutions using the Dependency agent and Log analytics agent. | VM insights |
| ActivityLog | Configuration changes and history of when each virtual machine is stopped and started. | Activity Log |
| InsightsMetrics |  |

> [!NOTE]
> Performance data collected by the Log Analytics agent writes to the *Perf* table while VM insights will collect it to the *InsightsMetrics* table. This is the same data, but the tables have a different structure. If you have existing queries based on *Perf*, the will need to be rewritten to use *InsightsMetrics*.

