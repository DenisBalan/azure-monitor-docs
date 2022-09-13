---
title: Migrate from Service Map to Azure Monitor VM insights
description: Migrate from Service Map is a solution in Azure that automatically discovers application components on Windows and Linux systems and maps the communication between services. This article provides details for deploying Service Map in your environment and using it in a variety of scenarios.
ms.topic: conceptual
author: guywi-ms
ms.author: guywild
ms.date: 09/13/2022
ms.reviewer: xpathak

---

# Migrate from Service map to Azure Monitor VM insights

[Service map](../vm/service-map.md) will be retired on 30 September 2025. To monitor connections between servers, processes, inbound and outbound connection latency, and ports across any TCP-connected architecture make sure to transition to [Azure Monitor VM insights](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-overview) before this date.

VM insights monitors the performance and health of your virtual machines and virtual machine scale sets, including their running processes and dependencies on other resources. The map feature visualizes the VM dependencies by discovering running processes that have active network connection between servers, inbound and outbound connection latency or ports across any TCP-connected architecture over a specified time range. [Learn more about the benefits of Map feature over Service map](https://docs.microsoft.com/en-us/azure/azure-monitor/faq#how-is-vm-insights-map-feature-different-from-service-map-). 

## Enable Azure Monitor VM insights using Azure Monitor agent
Migrate from service map to Azure Monitor VM insights using Azure Monitor agent and data collection rules. Azure Monitor agent is meant to replace the Log Analytics agent which was used by service map. Refer to our documentation to enable VM insights for Azure VMs and on-prem machines.
- [How to enable VM insights using Azure Monitor agent for Azure VMs?](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-enable-overview#agents)

If you have an on-prem machine, we recommend enabling [Azure Arc for servers](https://docs.microsoft.com/en-us/azure/azure-arc/servers/overview) so that the VMs can be enabled for VM insights using processes similar to Azure VMs.

VM insights includes additional functionality of collecting additional per-VM performance counters that provides visibility into the health of your VMs. These performance counters are ingested every minute and will slightly increase monitoring costs per VM. [Learn more about the pricing.](https://docs.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-overview#pricing)

Once you migrate to VM insights, remove the ServiceMap solution from the workspace to avoid data duplication and incurring additional costs.

## Remove ServiceMap solution from the workspace

1.	Sign in to the [Azure portal](https://portal.azure.com/).
1.	In the search bar, type **Log Analytics workspaces**. As you begin typing, the list filters suggestions based on your input. Select **Log Analytics workspaces**.
1.	In your list of Log Analytics workspaces, select the workspace you chose when you enabled ServiceMap.
1.	On the left, select **Solutions**.
1.	In the list of solutions, select **ServiceMap(workspace name)**. On the Overview page for the solution, select Delete. When prompted to confirm, select **Yes**.

You will not be able to onboard new subscriptions to service map after 31 August, 2024. The service map UI will not be available after the retirement - 30 September 2025.
