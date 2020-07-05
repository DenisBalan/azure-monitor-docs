---
title: Enable Azure Monitor for VMs with PowerShell or templates
description: This article describes how you enable Azure Monitor for VMs for one or more Azure virtual machines or virtual machine scale sets by using Azure PowerShell or Azure Resource Manager templates.
ms.subservice:
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/14/2019

---

# Enable Azure Monitor for VMs using Resource Manager templates
This article describes how to enable Azure Monitor for VMs for a virtual machine or virtual machine scale set using Resource Manager templates. This procedure can be used for the following:

- Azure virtual machine
- Azure virtual machine scale set
- Hybrid virtual machine connected with Azure Arc

## Prerequisites

- [Create and configure a Log Analytics workspace](vminsights-configure-workspace.md). 
- See [Supported operating systems](vminsights-enable-overview#supported-operating-systems) to ensure that the operating system of the VM or VMSS you're enabling is supported. 

## Resource Manager templates

We have created example Azure Resource Manager templates for onboarding your virtual machines and virtual machine scale sets. These templates include scenarios you can use to enable monitoring on an existing resource and create a new resource that has monitoring enabled.

>[!NOTE]
>The template needs to be deployed in the same resource group as the VM or VMSS being enabled.


The Azure Resource Manager templates are provided in an archive file (.zip) that you can [download](https://aka.ms/VmInsightsARMTemplates) from our GitHub repo. Contents of the file include folders that represent each deployment scenario with a template and parameter file. Before you run them, modify the parameters file and specify the values required. Don't modify the template file unless you need to customize it to support your particular requirements. After you have modified the parameter file, you can deploy it by using the following methods described later in this article.

The download file contains the following templates for different scenarios:

- **ExistingVmOnboarding** template enables Azure Monitor for VMs if the virtual machine already exists.
- **NewVmOnboarding** template creates a virtual machine and enables Azure Monitor for VMs to monitor it.
- **ExistingVmssOnboarding** template enables Azure Monitor for VMs if the virtual machine scale set already exists.
- **NewVmssOnboarding** template creates virtual machine scale sets and enables Azure Monitor for VMs to monitor them.
- **ConfigureWorkspace** template configures your Log Analytics workspace to support Azure Monitor for VMs by enabling the solutions and collection of Linux and Windows operating system performance counters.

>[!NOTE]
>If virtual machine scale sets were already present and the upgrade policy is set to **Manual**, Azure Monitor for VMs won't be enabled for instances by default after running the **ExistingVmssOnboarding** Azure Resource Manager template. You have to manually upgrade the instances.



# [CLI](#tab/CLI2)

```azurecli
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```

# [PowerShell](#tab/PowerShell2)

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```

---


## Next steps

Now that monitoring is enabled for your virtual machines, this information is available for analysis with Azure Monitor for VMs.

- To view discovered application dependencies, see [View Azure Monitor for VMs Map](vminsights-maps.md).

- To identify bottlenecks and overall utilization with your VM's performance, see [View Azure VM Performance](vminsights-performance.md).
