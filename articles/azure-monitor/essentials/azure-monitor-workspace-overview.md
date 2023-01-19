---
title: Azure Monitor workspace overview (preview)
description: Overview of Azure Monitor workspace, which is a unique environment for data collected by Azure Monitor.
author: EdB-MSFT
ms.author: edbaynash 
ms.topic: conceptual
ms.date: 19/01/2023
---

# Azure Monitor workspace (preview)
An Azure Monitor workspace is a unique environment for data collected by Azure Monitor. Each workspace has its own data repository, configuration, and permissions.


## Contents of Azure Monitor workspace
Azure Monitor workspaces will eventually contain all metric data collected by Azure Monitor. Currently, the only data hosted by an Azure Monitor workspace is Prometheus metrics.

The following table lists the contents of Azure Monitor workspaces. This table will be updated as other types of data are stored in them.

| Current contents | Future contents |
|:---|:---|
| Prometheus metrics | Native platform metrics<br>Native custom metrics<br>Prometheus metrics |


## Azure Monitor workspace architecture 

While a single Azure Monitor workspace may be sufficient for many use cases using Azure Monitor, many organizations create multiple workspaces to better meet their needs. This article presents a set of criteria for deciding whether to use a single Azure Monitor workspace, multiple Azure Monitor workspaces, and the configuration and placement of those workspaces. 

### Design criteria 

As you identify the right criteria to create additional Azure Monitor workspaces, your design should use the lowest number of workspaces that will match your requirements, while optimizing for minimal administrative management overhead. 

The following table presents criteria to consider when designing an Azure Monitor workspace architecture.  

|Criteria|Description|
|---|---|
|Segregate by logical boundaries |Create separate Azure Monitor workspaces for operational data based on logical boundaries, for example, a role, application type, type of metric etc.|
|Azure tenants | For multiple Azure tenants, create an Azure Monitor workspace in each tenant. Data sources can only send monitoring data to an Azure Monitor workspace in the same Azure tenant. |
|Azure regions |Each Azure Monitor workspace resides in a particular Azure region. Regulatory or compliance requirements may dictate the storage of data in particular locations. |
|Data ownership |Create separate Azure Monitor workspaces to define data ownership, for example by subsidiaries or affiliated companies.| 

### Growing account capacity  

Azure Monitor workspaces have default quotas and limitations for metrics. As your product grows and you need more metrics, you can request an increase to 50 million events or active time series. If your capacity needs grow exceptionally large, and your data ingestion needs can no longer be met by a single Azure Monitor workspace, consider creating multiple Azure Monitor workspaces. 

### Multiple Azure Monitor workspaces  

When an Azure Monitor workspace reaches 80% of its maximum capacity, or depending on your current and forecasted metric volume, it's recommended to split the Azure Monitor workspace into multiple workspaces. Split the workspace based on how the data in the workspace is used by your applications and business processes,  and how you want to access that data in the future.  For example, a company using Azure cloud service can logically separate its metrics in Azure Monitor workspaces by grouping them by application. By doing this, all the telemetric data can be managed and queried in an efficient way. 

In certain circumstances, splitting Azure Monitor workspace into multiple workspaces can be necessary. For example: 
1. Monitoring data in sovereign clouds – Create Azure Monitor workspace(s) in each sovereign cloud.  

1. Compliance or regulatory requirements that mandate storage of data in specific regions – Create an Azure Monitor workspace per region as per requirements. There may be a need to manage the scale of metrics for large services or financial institutions with regional accounts. 
1. Separating metric data in test, pre-production, and production environments 

>[!Note] 
> A single query cannot access multiple Azure Monitor workspaces. Keep data that you want to retrieve in a single query in same workspace. For presentation purposes, setting up Grafana with each workspace as a dedicated data source will allow for querying multiple workspaces in a single Grafana panel. 


## Limitations
See [Azure Monitor service limits](../service-limits.md#prometheus-metrics) for performance related service limits for Azure Monitor managed service for Prometheus.
- Private Links aren't supported for Prometheus metrics collection into Azure monitor workspace.
- Azure monitor workspaces are currently only supported in public clouds.
- Azure monitor workspaces don't currently support being moved into a different subscription or resource group once created.



## Link a Grafana workspace
Connect an Azure Monitor workspace to an [Azure Managed Grafana](../../managed-grafana/overview.md) workspace to authorize Grafana to use the Azure Monitor workspace as a resource type in a Grafana dashboard. An Azure Monitor workspace can be connected to multiple Grafana workspaces, and a Grafana workspace can be connected to multiple Azure Monitor workspaces.

> [!NOTE]
> When you add the Azure Monitor workspace as a data source to Grafana, it will be listed in form `Managed_Prometheus_<azure-workspace-name>`.

### [Azure portal](#tab/azure-portal)

1. Open the **Azure Monitor workspace** menu in the Azure portal.
2. Select your workspace.
3. Select **Linked Grafana workspaces**.
4. Select a Grafana workspace.

### [CLI](#tab/cli)
To be completed.

If the Azure Monitor workspace is linked to one or more Grafana workspaces, then the data will be available in Grafana.
az aks update --enable-azuremonitormetrics -n <cluster-name> -g <cluster-resource-group> --azure-monitor-workspace-resource-id <workspace-name-resource-id>

This creates a link between the Azure Monitor workspace and the Grafana workspace.
```azurecli
az aks update --enable-azuremonitormetrics -n <cluster-name> -g <cluster-resource-group> --azure-monitor-workspace-resource-id 
<azure-monitor-workspace-name-resource-id> --grafana-resource-id  <grafana-workspace-name-resource-id>
```
Output
```JSON
"azureMonitorProfile": {
    "metrics": {
        "enabled": true,
        "kubeStateMetrics": {
            "metricAnnotationsAllowList": "",
            "metricLabelsAllowlist": ""
        }
    }
}
```


### [Resource Manager](#tab/resource-manager)
To be completed.

---


## Next steps

- Learn more about the [Azure Monitor data platform](../data-platform.md).
