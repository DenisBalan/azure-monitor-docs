---
title: Enable VM insights overview
description: Learn how to deploy and configure VM insights. Find out the system requirements.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/24/2022
ms.custom: references_regions

---

# Enable VM insights overview

This article provides an overview of the options available to enable VM insights to monitor health and performance of the following:

- Azure virtual machines 
- Azure virtual machine scale sets
- Hybrid virtual machines connected with Azure Arc
- On-premises virtual machines
- Virtual machines hosted in another cloud environment.  

## Installation options and supported machines
The following table shows the installation methods available for enabling VM insights on supported machines.

| Method | Scope |
|:---|:---|
| [Azure portal](vminsights-enable-portal.md) | Enable individual machines with the Azure portal. |
| [Azure Policy](vminsights-enable-policy.md) | Create policy to automatically enable when a supported machine is created. |
| [Resource Manager templates](../vm/vminsights-enable-resource-manager.md) | Enable multiple machines using any of the supported methods to deploy a Resource Manager template such as CLI and PowerShell. |
| [PowerShell](vminsights-enable-powershell.md) | Use a PowerShell script to enable multiple machines. Log Analytics agent only. |
| [Manual install](vminsights-enable-hybrid.md) | Virtual machines or physical computers on-premises other cloud environments. Log Analytics agent only |


## Supported Azure Arc machines
VM insights is available for Azure Arc-enabled servers in regions where the Arc extension service is available. You must be running version 0.9 or above of the Arc Agent.

## Supported operating systems

VM insights supports any operating system that supports the Dependency agent and either the Azure Monitor agent (preview) or Log Analytics agent. See [Overview of Azure Monitor agents
](../agents/agents-overview.md#supported-operating-systems) for a complete list.

> [!IMPORTANT]
> If the ethernet device for your virtual machine has more than nine characters, then it won’t be recognized by VM insights and data won’t be sent to the InsightsMetrics table. The agent will collect data from [other sources](../agents/agent-data-sources.md).


### Linux considerations
See the following list of considerations on Linux support of the Dependency agent that supports VM insights:

- Only default and SMP Linux kernel releases are supported.
- Nonstandard kernel releases, such as Physical Address Extension (PAE) and Xen, aren't supported for any Linux distribution. For example, a system with the release string of *2.6.16.21-0.8-xen* isn't supported.
- Custom kernels, including recompilations of standard kernels, aren't supported.
- For Debian distros other than version 9.4, the map feature isn't supported, and the Performance feature is available only from the Azure Monitor menu. It isn't available directly from the left pane of the Azure VM.
- CentOSPlus kernel is supported.

The Linux kernel must be patched for the Spectre and Meltdown vulnerabilities. Please consult your Linux distribution vendor for more details. Run the following command to check for available if Spectre/Meltdown has been mitigated:

```
$ grep . /sys/devices/system/cpu/vulnerabilities/*
```

Output for this command will look similar to the following and specify whether a machine is vulnerable to either issue. If these files are missing, the machine is unpatched.

```
/sys/devices/system/cpu/vulnerabilities/meltdown:Mitigation: PTI
/sys/devices/system/cpu/vulnerabilities/spectre_v1:Vulnerable
/sys/devices/system/cpu/vulnerabilities/spectre_v2:Vulnerable: Minimal generic ASM retpoline
```


## Log Analytics workspace
VM insights requires a Log Analytics workspace. See [Configure Log Analytics workspace for VM insights](vminsights-configure-workspace.md) for details and requirements of this workspace.

> [!NOTE]
> VM Insights does not support sending data to more than one Log Analytics workspace (multi-homing).
> 
## Agents
When you enable VM insights for a machine, the following agents are installed. See [Network requirements](../agents/log-analytics-agent.md#network-requirements) for the network requirements for these agents.

- [Azure Monitor agent](../agents/azure-monitor-agent-overview.md) or [Log Analytics agent](../agents/log-analytics-agent.md). Collects data from the virtual machine or virtual machine scale set and delivers it to the Log Analytics workspace (both agents) and Azure Monitor Metrics (Azure Monitor agent only). VM insights support for Azure Monitor agent is currently in public preview. See [VM insights with Azure Monitor agent (Preview)](vminsights-azure-monitor-agent.md) for details.
- Dependency agent. Collects discovered data about processes running on the virtual machine and external process dependencies, which are used by the [Map feature in VM insights](../vm/vminsights-maps.md). The Dependency agent relies on the Azure Monitor agent or Log Analytics agent to deliver its data to Azure Monitor.


## Network requirements

- See [Network requirements](../agents/log-analytics-agent.md#network-requirements) for the network requirements for the Log Analytics agent.
- The dependency agent requires a connection from the virtual machine to the address 169.254.169.254. This is the Azure metadata service endpoint. Ensure that firewall settings allow connections to this endpoint.

## Data collection rule (Azure Monitor agent)
When you enable VM insights on a machine with the Azure Monitor agent you must specify a [data collection rule](../essentials/data-collection-ruleo-overview.m) to use. The DCR specifies the data to collect and the workspace to use. VM insights creates a default DCR if one doesn't already exist. You can edit this DCR to collect additional data such as Windows and Syslog events, or you can create additional DCRs and associate with the machine.

It's not recommended to create your own DCR to support VM insights. The DCR created by VM insights includes a special data stream required for its operation. If you want to customize the configuration, modify the DCR created by VM insights instead of creating a new one.

## Management packs (Log Analytics agent)
When a Log Analytics workspace is configured for VM insights, two management packs are forwarded to all the Windows computers connected to that workspace. The management packs are named *Microsoft.IntelligencePacks.ApplicationDependencyMonitor* and *Microsoft.IntelligencePacks.VMInsights* and are written to *%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs*. 

The data source used by the *ApplicationDependencyMonitor* management pack is **%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*. The data source used by the *VMInsights* management pack is *%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\ Microsoft.VirtualMachineMonitoringModule.dll*.

## Diagnostic and usage data

Microsoft automatically collects usage and performance data through your use of the Azure Monitor service. Microsoft uses this data to improve the quality, security, and integrity of the service. 

To provide accurate and efficient troubleshooting capabilities, the Map feature includes data about the configuration of your software. The data provides information such as the operating system and version, IP address, DNS name, and workstation name. Microsoft doesn't collect names, addresses, or other contact information.

For more information about data collection and usage, see the [Microsoft Online Services Privacy Statement](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## Next steps

To learn how to use the Performance monitoring feature, see [View VM insights Performance](../vm/vminsights-performance.md). To view discovered application dependencies, see [View VM insights Map](../vm/vminsights-maps.md).
