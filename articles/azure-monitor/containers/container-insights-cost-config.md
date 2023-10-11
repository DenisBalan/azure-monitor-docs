---
title: Configure Container insights cost optimization data collection rules
description: This article describes how you can configure the Container insights agent to control data collection for metric counters
ms.topic: conceptual
ms.custom: devx-track-azurecli
ms.date: 07/31/2023
ms.reviewer: aul
---

# Enable cost optimization settings in Container insights

Cost optimization settings allow you to customize the data collected by Container insights for your Kubernetes cluster. These settings reduce the monitoring costs of Container insights by controlling the volume of data ingested in your Log Analytics workspace. You can modify the settings for individual tables, data collection intervals, and namespaces to exclude for the data collection through [Azure Monitor Data Collection Rules (DCR)](../essentials/data-collection-rule-overview.md).

## Cluster configurations

- AKS
- Arc-enabled Kubernetes
- AKS hybrid

## Prerequisites

- AKS clusters must use either System or User Assigned Managed Identity. If cluster is using Service Principal, you must [upgrade to Managed Identity](../../aks/use-managed-identity.md#enable-managed-identities-on-an-existing-aks-cluster).
- Minimum version required for Azure CLI is 2.51.0.
    - For AKS clusters, [aks-preview](../../aks/cluster-configuration.md#install-the-aks-preview-azure-cli-extension) version 0.5.147 or higher
    - For Arc enabled Kubernetes and AKS hybrid, [k8s-extension](../../azure-arc/kubernetes/extensions.md#prerequisites) version 1.4.3 or higher


## Enable cost settings
Following are the details for using different methods to enable cost optimization settings for a cluster. See the sections below for details about the different available settings.

## [Azure portal](#tab/portal)
1. Select the cluster in the Azure portal.
2. Select the **Insights** menu in the **Monitoring** section.
3. If Container insights has already been enabled on the cluster, select the **Monitoring Settings** button. If not, select **Configure Azure Monitor** and see []() for details on enabling monitoring. 
4. For AKS and Arc-enabled Kubernetes, select **Use managed identity** if you haven't yet migrated to [managed identity authentication](../containers/container-insights-onboard.md#authentication).
5. Select one of the cost presets described in []().

    [![Screenshot that shows the onboarding options.](media/container-insights-cost-config/cost-settings-onboarding.png)](media/container-insights-cost-config/cost-settings-onboarding.png#lightbox)

1. Click **Edit collection settings** to customize the settings. See []() for details on each setting.

    [![Screenshot that shows the collection settings.](media/container-insights-cost-config/advanced-collection-settings.png)](media/container-insights-cost-config/advanced-collection-settings.png#lightbox).

1. Click **Configure** to save the settings.

## Custom data collection
Container insights Collected Data can be customized through the Azure portal, using the following options. Selecting any options other than **All (Default)** leads to the container insights experience becoming unavailable.

| Grouping | Tables | Notes |
| --- | --- | --- |
| All (Default) | All standard container insights tables | Required for enabling the default container insights visualizations |
| Performance | Perf, InsightsMetrics | |
| Logs and events | ContainerLog or ContainerLogV2, KubeEvents, KubePodInventory | Recommended if you have enabled managed Prometheus metrics |
| Workloads, Deployments, and HPAs | InsightsMetrics, KubePodInventory, KubeEvents, ContainerInventory, ContainerNodeInventory, KubeNodeInventory, KubeServices | |
| Persistent Volumes | InsightsMetrics, KubePVInventory | |

[![Screenshot that shows the collected data options.](media/container-insights-cost-config/collected-data-options.png)](media/container-insights-cost-config/collected-data-options.png#lightbox)

### Cost presets and collection settings
Cost presets and collection settings are available for selection in the Azure portal to allow easy configuration. By default, container insights ships with the Standard preset, however, you may choose one of the following to modify your collection settings.

| Cost preset | Collection frequency | Namespace filters | Syslog collection |
| --- | --- | --- | --- |
| Standard | 1 m | None | Not enabled |
| Cost-optimized | 5 m | Excludes kube-system, gatekeeper-system, azure-arc | Not enabled |
| Syslog | 1 m | None | Enabled by default |


## [CLI](#tab/cli)

> [!NOTE]
> Minimum Azure CLI version 2.51.0 or higher.

## AKS cluster

When you use CLI to configure monitoring for your AKS cluster, you provide the configuration as a JSON file using the following format. Each of these settings is described in [Data collection parameters](#data-collection-parameters).

```json
{
  "interval": "1m",
  "namespaceFilteringMode": "Include",
  "namespaces": ["kube-system"],
  "enableContainerLogV2": true, 
  "streams": ["Microsoft-Perf", "Microsoft-ContainerLogV2"]
}
```

### New AKS cluster

Use the following command to create a new AKS cluster with monitoring enabled:

```azcli
az aks create -g <clusterResourceGroup> -n <clusterName> --enable-managed-identity --node-count 1 --enable-addons monitoring --data-collection-settings dataCollectionSettings.json --generate-ssh-keys 
```

### Existing AKS Cluster

**Cluster without the monitoring addon**

```azcli
az aks enable-addons -a monitoring -g <clusterResourceGroup> -n <clusterName> --data-collection-settings dataCollectionSettings.json
```

**Cluster with an existing monitoring addon**

```azcli    
# get the configured log analytics workspace resource id
az aks show -g <clusterResourceGroup> -n <clusterName> | grep -i "logAnalyticsWorkspaceResourceID"

# disable monitoring 
az aks disable-addons -a monitoring -g <clusterResourceGroup> -n <clusterName>

# enable monitoring with data collection settings
az aks enable-addons -a monitoring -g <clusterResourceGroup> -n <clusterName> --workspace-resource-id <logAnalyticsWorkspaceResourceId> --data-collection-settings dataCollectionSettings.json
```

## Arc-enabled Kubernetes cluster

```azcli
az k8s-extension create --name azuremonitor-containers --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers --configuration-settings amalogs.useAADAuth=true dataCollectionSettings='{"interval":"1m","namespaceFilteringMode": "Include", "namespaces": [ "kube-system"],"enableContainerLogV2": true,"streams": ["<streams to be collected>"]}'
```

>[!NOTE]
> When deploying on a Windows machine, the dataCollectionSettings field must be escaped. For example, dataCollectionSettings={\"interval\":\"1m\",\"namespaceFilteringMode\": \"Include\", \"namespaces\": [ \"kube-system\"]} instead of dataCollectionSettings='{"interval":"1m","namespaceFilteringMode": "Include", "namespaces": [ "kube-system"]}'

## AKS hybrid Cluster

```azcli
az k8s-extension create --name azuremonitor-containers --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type provisionedclusters --cluster-resource-provider "microsoft.hybridcontainerservice" --extension-type Microsoft.AzureMonitor.Containers --configuration-settings amalogs.useAADAuth=true dataCollectionSettings='{"interval":"1m","namespaceFilteringMode":"Include", "namespaces": ["kube-system"],"enableContainerLogV2": true,"streams": ["<streams to be collected>"]}'
```

>[!NOTE]
> When deploying on a Windows machine, the dataCollectionSettings field must be escaped. For example, dataCollectionSettings={\"interval\":\"1m\",\"namespaceFilteringMode\": \"Include\", \"namespaces\": [ \"kube-system\"]} instead of dataCollectionSettings='{"interval":"1m","namespaceFilteringMode": "Include", "namespaces": [ "kube-system"]}'

The collection settings can be modified through the input of the `dataCollectionSettings` field.


## [ARM](#tab/arm)


## AKS cluster

1. Download the Azure Resource Manager Template and Parameter files.

```bash
curl -L https://aka.ms/aks-enable-monitoring-costopt-onboarding-template-file -o existingClusterOnboarding.json
```

```bash
curl -L https://aka.ms/aks-enable-monitoring-costopt-onboarding-template-parameter-file -o existingClusterParam.json
```

2. Edit the values in the parameter file: existingClusterParam.json.

- For _aksResourceId_ and _aksResourceLocation_, use the values on the  **AKS Overview**  page for the AKS cluster.
- For _workspaceResourceId_, use the resource ID of your Log Analytics workspace.
- For _workspaceLocation_, use the Location of your Log Analytics workspace
- For _resourceTagValues_, use the existing tag values specified for the AKS cluster
- For _dataCollectionInterval_, specify the interval to use for the data collection interval. Allowed values are 1 m, 2 m … 30 m where m suffix indicates the minutes.
- For _namespaceFilteringModeForDataCollection_, specify if the namespace array is to be included or excluded for collection. If set to off, the agent ignores the namespaces field.
- For _namespacesForDataCollection_, specify array of the namespaces to exclude or include for the Data collection. For example, to exclude "kube-system" and "default" namespaces, you can specify the value as ["kube-system", "default"] with an Exclude value for namespaceFilteringMode.
- For _enableContainerLogV2_, specify this parameter to be true or false. By default, this parameter is set to true.
- For _streams_, select the container insights tables you want to collect. Refer to the above mapping for more details.

3. Deploy the ARM template.

```azcli
az login

az account set --subscription"Cluster Subscription Name"

az deployment group create --resource-group <ClusterResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
```

## Arc-enabled Kubernetes cluster

1. Download the Azure Resource Manager Template and Parameter files.

```bash
curl -L https://aka.ms/existingClusterOnboarding.json -o existingClusterOnboarding.json
```

```bash
curl -L https://aka.ms/existingClusterParam.json -o existingClusterParam.json
```

2. Edit the values in the parameter file: existingClusterParam.json.

- For _clusterResourceId_ and _clusterResourceLocation_, use the values on the  **Overview**  page for the AKS hybrid cluster.
- For _workspaceResourceId_, use the resource ID of your Log Analytics workspace.
- For _workspaceLocation_, use the Location of your Log Analytics workspace
- For _resourceTagValues_, use the existing tag values specified for the AKS hybrid cluster
- For _dataCollectionInterval_, specify the interval to use for the data collection interval. Allowed values are 1 m, 2 m … 30 m where m suffix indicates the minutes.
- For _namespaceFilteringModeForDataCollection_, specify if the namespace array is to be included or excluded for collection. If set to off, the agent ignores the namespaces field.
- For _namespacesForDataCollection_, specify array of the namespaces to exclude or include for the Data collection. For example, to exclude "kube-system" and "default" namespaces, you can specify the value as ["kube-system", "default"] with an Exclude value for namespaceFilteringMode.
- For _enableContainerLogV2_, specify this parameter to be true or false. By default, this parameter is set to true.
- For _streams_, select the container insights tables you want to collect. Refer to the above mapping for more details.


3. Deploy the ARM template.

```azcli
az login

az account set --subscription"Cluster Subscription Name"

az deployment group create --resource-group <ClusterResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
```
## AKS hybrid Cluster

1. Download the Azure Resource Manager Template and Parameter files.

```bash
curl -L https://aka.ms/arc-k8s-enable-monitoring-costopt-onboarding-template-file -o existingClusterOnboarding.json 
```

```bash
curl -L https://aka.ms/arc-k8s-enable-monitoring-costopt-onboarding-template-parameter-file -o existingClusterParam.json 
```

2. Edit the values in the parameter file: existingClusterParam.json.

- For _clusterResourceId_ and  _clusterRegion_, use the values on the  **Overview**  page for the Arc enabled Kubernetes cluster.
- For _workspaceResourceId_, use the resource ID of your Log Analytics workspace.
- For _workspaceLocation_, use the Location of your Log Analytics workspace
- For _resourceTagValues_, use the existing tag values specified for the Arc cluster
- For _dataCollectionInterval_, specify the interval to use for the data collection interval. Allowed values are 1 m, 2 m … 30 m where m suffix indicates the minutes.
- For _namespaceFilteringModeForDataCollection_, specify if the namespace array is to be included or excluded for collection. If set to off, the agent ignores the namespaces field.
- For _namespacesForDataCollection_, specify array of the namespaces to exclude or include for the Data collection. For example, to exclude "kube-system" and "default" namespaces, you can specify the value as ["kube-system", "default"] with an Exclude value for namespaceFilteringMode.
- For _enableContainerLogV2_, specify this parameter to be true or false. By default, this parameter is set to true.
- For _streams_, select the container insights tables you want to collect. Refer to the above mapping for more details.

3. Deploy the ARM template.

```azcli
az login

az account set --subscription "Cluster's Subscription Name"

az deployment group create --resource-group <ClusterResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
```
---







## Data collection parameters

The container insights agent periodically checks for the data collection settings, validates and applies the applicable settings to applicable container insights Log Analytics tables and Custom Metrics. The data collection settings should be applied in the subsequent configured Data collection interval.

The following table describes the supported data collection settings:

| Data collection setting | Azure portal | Description |
|:---|:---|:---|
| interval | Collection frequency | Determines how often the agent collects data.  Valid values are 1m - 30m in 1m intervals The default value is 1m. If the value is outside the allowed range, then it defaults to *1 m*. |
| namespaceFilteringMode | Namespace filtering | *Include*: Collects only data from the values in the *namespaces* field.<br>*Exclude*: Collects data from all namespaces except for the values in the *namespaces* field.<br>*Off*: Ignores any *namespace* selections and collect data on all namespaces.
| namespaces | Namespace filtering | Array of comma separated Kubernetes namespaces to collect inventory and perf data based on the _namespaceFilteringMode_.<br>For example, *namespaces = \["kube-system", "default"]* with an _Include_ setting collects only these two namespaces. With an _Exclude_ setting, the agent collects data from all other namespaces except for _kube-system_ and _default_. With an _Off_ setting, the agent collects data from all namespaces including _kube-system_ and _default_. Invalid and unrecognized namespaces are ignored. |
| enableContainerLogV2 | Enable ContainerLogV2 | Boolean flag to enable ContainerLogV2 schema. If set to true, the stdout/stderr Logs are ingested to [ContainerLogV2](container-insights-logging-v2.md) table. If not, the container logs are ingested to **ContainerLog** table, unless otherwise specified in the ConfigMap. When specifying the individual streams, you must include the corresponding table for ContainerLog or ContainerLogV2. |
| streams | | An array of container insights table streams. See the supported streams above to table mapping. |




## Log Analytics data collection

The settings allow you specify which tables you want to collect using streams. The following table indicates the stream to table mapping to be used in the data collection settings.

| Stream | Container insights table |
| --- | --- |
| Microsoft-ContainerInventory | ContainerInventory |
| Microsoft-ContainerNodeInventory | ContainerNodeInventory |
| Microsoft-Perf | Perf |
| Microsoft-InsightsMetrics | InsightsMetrics |
| Microsoft-KubeMonAgentEvents | KubeMonAgentEvents |
| Microsoft-KubeServices | KubeServices |
| Microsoft-KubePVInventory | KubePVInventory |
| Microsoft-KubeNodeInventory | KubeNodeInventory |
| Microsoft-KubePodInventory | KubePodInventory |
| Microsoft-KubeEvents | KubeEvents |
| Microsoft-ContainerLogV2 | ContainerLogV2 |
| Microsoft-ContainerLog | ContainerLog |

This table outlines the list of the container insights Log Analytics tables for which data collection settings are applicable.

>[!NOTE]
>This feature configures settings for all container insights tables (excluding ContainerLog), to configure settings on the ContainerLog please update the ConfigMap listed in documentation for [agent data Collection settings](../containers/container-insights-agent-config.md).

| ContainerInsights Table Name | Is Data collection setting: interval applicable? | Is Data collection setting: namespaces applicable? | Remarks |
| --- | --- | --- | --- |
| ContainerInventory | Yes | Yes | |
| ContainerNodeInventory | Yes | No | Data collection setting for namespaces is not applicable since Kubernetes Node is not a namespace scoped resource |
| KubeNodeInventory | Yes | No | Data collection setting for namespaces is not applicable Kubernetes Node is not a namespace scoped resource |
| KubePodInventory | Yes | Yes ||
| KubePVInventory | Yes | Yes | |
| KubeServices | Yes | Yes | |
| KubeEvents | No | Yes | Data collection setting for interval is not applicable for the Kubernetes Events |
| Perf | Yes | Yes\* | \*Data collection setting for namespaces is not applicable for the Kubernetes Node related metrics since the Kubernetes Node is not a namespace scoped object. |
| InsightsMetrics| Yes\*\* | Yes\*\* | \*\*Data collection settings are only applicable for the metrics collecting the following namespaces: container.azm.ms/kubestate, container.azm.ms/pv and container.azm.ms/gpu |

## Custom metrics

| Metric namespace | Is Data collection setting: interval applicable? | Is Data collection setting: namespaces applicable? | Remarks |
| --- | --- | --- | --- |
| Insights.container/nodes| Yes | No | Node is not a namespace scoped resource |
|Insights.container/pods | Yes | Yes| |
| Insights.container/containers | Yes | Yes | |
| Insights.container/persistentvolumes | Yes | Yes | |

## Impact on default visualizations and existing alerts

The default Container insights experience is depends on all the existing data streams. Removing one or more of the default streams makes the Container insights experience unavailable.

If you are currently using the above tables for other custom alerts or charts, then modifying your data collection settings may degrade those experiences. If you are excluding namespaces or reducing data collection frequency, review your existing alerts, dashboards, and workbooks using this data.

To scan for alerts that may be referencing these tables, run the following Azure Resource Graph query:

```Kusto
resources
| where type in~ ('microsoft.insights/scheduledqueryrules') and ['kind'] !in~ ('LogToMetric')
| extend severity = strcat("Sev", properties["severity"])
| extend enabled = tobool(properties["enabled"])
| where enabled in~ ('true')
| where tolower(properties["targetResourceTypes"]) matches regex 'microsoft.operationalinsights/workspaces($|/.*)?' or tolower(properties["targetResourceType"]) matches regex 'microsoft.operationalinsights/workspaces($|/.*)?' or tolower(properties["scopes"]) matches regex 'providers/microsoft.operationalinsights/workspaces($|/.*)?'
| where properties contains "Perf" or properties  contains "InsightsMetrics" or properties  contains "ContainerInventory" or properties  contains "ContainerNodeInventory" or properties  contains "KubeNodeInventory" or properties  contains"KubePodInventory" or properties  contains "KubePVInventory" or properties  contains "KubeServices" or properties  contains "KubeEvents" 
| project id,name,type,properties,enabled,severity,subscriptionId
| order by tolower(name) asc
```

Reference the [Limitations](./container-insights-cost-config.md#limitations) section for information on migrating your Recommended alerts.



## Data Collection Settings Updates

To update your data collection Settings, modify the values in parameter files and redeploy the Azure Resource Manager Templates to your corresponding AKS or Azure Arc Kubernetes cluster. Or select your new options through the Monitoring Settings in the portal.

## Troubleshooting

- Only clusters using [managed identity authentication](../containers/container-insights-onboard.md#authentication), are able to use this feature.
- Missing data in your container insights charts is an expected behavior for namespace exclusion, if excluding all namespaces

## Limitations

- Recommended alerts will not work as intended if the Data collection interval is configured more than 1-minute interval. To continue using Recommended alerts, please migrate to the [Prometheus metrics addon](../essentials/prometheus-metrics-overview.md)
- There may be gaps in Trend Line Charts of Deployments workbook if configured Data collection interval more than time granularity of the selected Time Range.



[![Screenshot that shows the custom experience.](media/container-insights-cost-config/container-insights-cost-custom.png)](media/container-insights-cost-config/container-insights-cost-custom.png#lightbox)
