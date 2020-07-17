---
title: Azure Monitor agent overview
description: T
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/11/2020

---

# Azure Monitor agent overview (preview)
The Azure Monitor Agent (AMA) is installed on virtual machines in Azure, other clouds, or on-premises to collect guest operating system and workload data into Azure Monitor. This articles describes the Azure Monitor Agent including how to install it and how to create data collection rules to define data that should be collected.


## Relationship to other agents
The Azure Monitor Agent replaces the following agents that are currently used by Azure Monitor to collect guest data from virtual machines:

- [Log Analytics agent](log-analytics-agent.md) - Sends data to Log Analytics workspace and supports Azure Monitor for VMs and monitoring solutions.
- [Diagnostic extension](diagnostics-extension-overview.md) - Sends data to Azure Monitor Metrics (Windows only), Azure Event Hubs, and Azure Storage.
- [Telegraf agent](collect-custom-metrics-linux-telegraf.md) - Send data to Azure Monitor Metrics (Linux only).

In addition to consolidating this functionality into a single agent, the Azure Monitor Agent provides the following benefits over the existing agents:

- Scoping. Collect different sets of data from different scopes of VMs.  
- Linux multi-homing. Send data from Linux VMs to multiple workspaces.
- Windows event filtering. Use XPATH queries to define which Windows events are collected.
- Improved extension management. The new agent uses a new method of handling extensibility that is more transparent and controllable than the management of Management Packs and Linux plug-ins in the current Log Analytics agents.

### Changes in data collection
The methods for defining data collection for the existing agents are distinctly different, and each have challenges that are addressed with AMA.

- Log Analytics agent gets its configuration from a Log Analytics workspace. This is easy to centrally configure, but difficult to define independent definitions for different virtual machines. It can also only send data to a Log Analytics workspace.

- Diagnostic extension has a configuration for each virtual machine. This is easy to define independent definitions for different virtual machines but difficult to centrally manage. It can only send data to Azure Monitor Metrics, Azure Event Hubs, or Azure Storage. For Linux agents, the open source Telegraf agent is required to send data to Azure Monitor Metrics.

AMA uses Data Collection Rules (DCR) to configure data to collect from each agent. DCRs enable manageability of collection settings at scale while still enabling unique, scoped configurations for subsets of machines. They are independent of the workspace and independent of the VM which allows them to be defined once and reused across machines and environments.


## Current limitations
The following limitations apply during public preview of the Azure Monitor Agent:

- The Data Collection Rule and any Log Analytics workspace used as a destination must be in the same region.
- Only Azure virtual machines are supported. On-premises virtual machines, virtual machine scale sets, Arc for Servers, Azure Kubernetes Service, and other compute resource types are not currently supported.
- The virtual machine must have access to the following HTTPS endpoints:
  - *.ods.opinsights.azure.com
  - *.ingest.monitor.azure.com
  - *.control.monitor.azure.com



## Costs
There is no cost for Azure Monitor agent, but you may incur charges for the data ingested. See [Azure Monitor pricing](https://azure.microsoft.com/pricing/details/monitor/) for details on Log Analytics data collection and retention and for customer metrics.

## Data sources and destinations
The following table lists the types of data you can collect with the Azure Monitor agent and where you can send that data. See [What is monitored by Azure Monitor?](../monitor-reference.md) for a list of insights, solutions, and other solutions that use the Azure Monitor agent to collect other kinds of data.


The Azure Monitor agent sends data to Azure Monitor Metrics or a Log Analytics workspace supporting Azure Monitor Logs.

| Data Source | Destinations | Description |
|:---|:---|:---|
| Performance        | Azure Monitor Metrics<br>Log Analytics workspace | Numerical values measuring performance of different aspects of operating system and workloads. |
| Windows Event logs | Log Analytics workspace | Information sent to the Windows event logging system. |
| Syslog             | Log Analytics workspace | Information sent to the Linux event logging system. |


## Supported operating systems
The following operating systems are currently supported byt the Azure Monitor agent.

### Windows 
  - Windows Server 2019
  - Windows Server 2016
  - Windows Server 2012
  - Windows Server 2012 R2

### Linux
  - Linux
  - CentOS 7
  - Oracle Linux 6, 7
  - RHEL 6, 7, 8
  - Debian 9 + 10
  - Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS
  - SLES 11, 12, 15

## Install the Azure Monitor agent
The Azure Monitor Agent is implemented as an [Azure VM extension](../../virtual-machines/extensions/overview.md) with the details in the following table. 

| Property | Windows | Linux |
|:---|:---|:---|
| Publisher | Microsoft.Azure.Monitor  | Microsoft.Azure.Monitor |
| Type      | AzureMonitorWindowsAgent | AzureMonitorLinuxAgent  |
| TypeHandlerVersion  | 1.0 | 0.9 |

Install the Azure Monitor agent using any of the methods to install virtual machine agents including the following using PowerShell or CLI.

## Windows

# [CLI](#tab/CLI1)

```azurecli
az vm extension set --name AzureMonitorWindowsAgent --publisher Microsoft.Azure.Monitor --version 1.0 --ids {resource ID of the VM}

```

# [PowerShell](#tab/PowerShell1)

```powershell
Set-AzVMExtension -Name AMAWindows -ExtensionType AzureMonitorWindowsAgent -Publisher Microsoft.Azure.Monitor -Version 1.0 -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---


## Linux

# [CLI](#tab/CLI2)

```azurecli
az vm extension set --name AzureMonitorLinuxAgent --publisher Microsoft.Azure.Monitor --version 0.9 --ids {resource ID of the VM}

```

# [PowerShell](#tab/PowerShell2)

```powershell
Set-AzVMExtension -Name AMALinux -ExtensionType AzureMonitorLinuxAgent -Publisher Microsoft.Azure.Monitor -Version 0.9 -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---

## Next steps

