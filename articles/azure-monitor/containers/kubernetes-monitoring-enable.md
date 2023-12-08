---
title: Enable monitoring for Azure Kubernetes Service (AKS) cluster
description: Learn how to enable Container insights and Managed Prometheus on an Azure Kubernetes Service (AKS) cluster.
ms.topic: conceptual
ms.date: 11/14/2023
ms.custom: ignite-2022
ms.reviewer: aul
---

# Enable monitoring for Kubernetes clusters

This article describes how to enable these features on for complete monitoring of your Kubernetes clusters using the following Azure Monitor services:

- [Managed Prometheus](../essentials/prometheus-metrics-overview.md) for metric collection
- [Container insights](./container-insights-overview.md) for log collection
- [Managed Grafana](../../managed-grafana/overview.md) for visualization.

Using the Azure portal, you can enable all of the features at the same time. You can also enable them individually by using the Azure CLI, Azure Resource Manager template, Bicep, Terraform, or Azure Policy. Each of these methods is described in this article.

> [!IMPORTANT]
> This article describes onboarding using default configuration settings including managed identity authentication. See [Configure agent data collection for Container insights](container-insights-agent-config.md) and [Customize scraping of Prometheus metrics in Azure Monitor managed service for Prometheus](prometheus-metrics-scrape-configuration.md) to customize your configuration to ensure that you aren't collecting more data than you require. See [Authentication for Container Insights](container-insights-authentication.md) for guidance on migrating from legacy authentication models.


## Prerequisites

- See [Supported configurations](./container-insights-overview.md#supported-configurations) for the list of supported configurations.
- Permissions
  - You require at least [Contributor](../../role-based-access-control/built-in-roles.md#contributor) access to the cluster for onboarding.
  - To view data after monitoring is enabled, you must have [Monitoring Reader](../roles-permissions-security.md#monitoring-reader) or [Monitoring Contributor](../roles-permissions-security.md#monitoring-contributor) role.
- Managed Prometheus prerequisites
  - The cluster must use [managed identity authentication](../../aks/use-managed-identity.md).
  - The following resource providers must be registered in the subscription of the AKS cluster and the Azure Monitor workspace:
    - Microsoft.ContainerService
    - Microsoft.Insights
    - Microsoft.AlertsManagement
- Arc-Enabled Kubernetes clusters prerequisites
  - Verify the [firewall requirements](kubernetes-monitoring-firewall.md) in addition to the [Azure Arc-enabled Kubernetes network requirements](../../azure-arc/kubernetes/network-requirements.md).
  - If you previously installed monitoring for AKS, ensure that you have [disabled monitoring](./container-insights-optout.md) before proceeding to avoid issues during the extension install.
  - If you previously installed monitoring on this cluster using script without cluster extensions, follow the instructions at [Disable Container insights on your hybrid Kubernetes cluster](container-insights-optout-hybrid.md) to delete this Helm chart.

## Workspaces
    - Each of the features requires a workspace. You can create each workspace as part of the onboarding process or use an existing workspace. 

        | Feature | Workspace | Notes |
        |:---|:---|:---|
        | Managed Prometheus | [Azure Monitor workspace](../essentials/azure-monitor-workspace-overview.md) | `Contributor` permission is enough for enabling the addon to send data to the Azure Monitor workspace. You will need `Owner` level permission to link your Azure Monitor Workspace to view metrics in Azure Managed Grafana. This is required because the user executing the onboarding step, needs to be able to give the Azure Managed Grafana System Identity `Monitoring Reader` role on the Azure Monitor Workspace to query the metrics. |
        | Container insights | [Log Analytics workspace](../logs/log-analytics-workspace-overview.md) | You can attach an AKS cluster to a Log Analytics workspace in a different Azure subscription in the same Microsoft Entra tenant, but you must use the Azure CLI or an Azure Resource Manager template. You can't currently perform this configuration with the Azure portal.<br><br>If you're connecting an existing AKS cluster to a Log Analytics workspace in another subscription, the *Microsoft.ContainerService* resource provider must be registered in the subscription with the Log Analytics workspace. For more information, see [Register resource provider](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).<br><br>For a list of the supported mapping pairs to use for the default workspace, see [Region mappings supported by Container insights](container-insights-region-mapping.md). |


## Enable Prometheus and Grafana
Use one of the following methods to enable scraping of Prometheus metrics from your cluster and enable Managed Grafana to visualize the metrics.

### [CLI](#tab/cli)

 If you don't specify an existing Azure Monitor workspace in the following commands, the default workspace for the resource group will be used. If a default workspace doesn't already exist in the cluster's region, one with a name in the format `DefaultAzureMonitorWorkspace-<mapped_region>` will be created in a resource group with the name `DefaultRG-<cluster_region>`.



#### AKS cluster
Use the `-enable-azure-monitor-metrics` option `az aks create` or `az aks update` (depending whether you're creating a new cluster or updating an existing cluster) to install the metrics add-on that scrapes Prometheus metrics.

**Prerequisites**

- The aks-preview extension must be uninstalled by using the command `az extension remove --name aks-preview`. For more information on how to uninstall a CLI extension, see [Use and manage extensions with the Azure CLI](/cli/azure/azure-cli-extensions-overview).
- Az CLI version of 2.49.0 or higher is required. Check the aks-preview version by using the `az version` command.

**Sample commands**

```azurecli
### Use default Azure Monitor workspace
az aks create/update --enable-azure-monitor-metrics -n <cluster-name> -g <cluster-resource-group>

### Use existing Azure Monitor workspace
az aks create/update --enable-azure-monitor-metrics -n <cluster-name> -g <cluster-resource-group> --azure-monitor-workspace-resource-id <workspace-name-resource-id>

### Use an existing Azure Monitor workspace and link with an existing Grafana workspace
az aks create/update --enable-azure-monitor-metrics -n <cluster-name> -g <cluster-resource-group> --azure-monitor-workspace-resource-id <azure-monitor-workspace-name-resource-id> --grafana-resource-id  <grafana-workspace-name-resource-id>

### Use optional parameters
az aks create/update --enable-azure-monitor-metrics -n <cluster-name> -g <cluster-resource-group> --ksm-metric-labels-allow-list "namespaces=[k8s-label-1,k8s-label-n]" --ksm-metric-annotations-allow-list "pods=[k8s-annotation-1,k8s-annotation-n]"
```

#### Arc-enabled cluster

**Prerequisites**

- The k8s-extension extension must be installed. Install the extension using the command az extension add --name k8s-extension.
- The k8s-extension version 1.4.1 or higher is required. Check the k8s-extension version by using the `az version` command.


**Sample commands**

```azurecli
### Use default Azure Monitor workspace
az k8s-extension create --name azuremonitor-metrics --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers.Metrics

## Use existing Azure Monitor workspace
az k8s-extension create --name azuremonitor-metrics --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers.Metrics --configuration-settings azure-monitor-workspace-resource-id=<workspace-name-resource-id>

### Use an existing Azure Monitor workspace and link with an existing Grafana workspace
az k8s-extension create --name azuremonitor-metrics --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers.Metrics --configuration-settings azure-monitor-workspace-resource-id=<workspace-name-resource-id> grafana-resource-id=<grafana-workspace-name-resource-id>

### Use optional parameters
az k8s-extension create --name azuremonitor-metrics --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers.Metrics --configuration-settings azure-monitor-workspace-resource-id=<workspace-name-resource-id> grafana-resource-id=<grafana-workspace-name-resource-id> AzureMonitorMetrics.KubeStateMetrics.MetricAnnotationsAllowList="pods=[k8s-annotation-1,k8s-annotation-n]" AzureMonitorMetrics.KubeStateMetrics.MetricsLabelsAllowlist "namespaces=[k8s-label-1,k8s-label-n]"
```

Any of the commands can use the following optional parameters:

- AKS: `--ksm-metric-annotations-allow-list`<br>Arc: `--AzureMonitorMetrics.KubeStateMetrics.MetricAnnotationsAllowList`
  - Comma-separated list of Kubernetes annotations keys used in the resource's kube_resource_annotations metric. For example, kube_pod_annotations is the annotations metric for the pods resource. By default, this metric contains only name and namespace labels. To include more annotations, provide a list of resource names in their plural form and Kubernetes annotation keys that you want to allow for them. A single `*` can be provided for each resource to allow any annotations, but this has severe performance implications. For example, `pods=[kubernetes.io/team,...],namespaces=[kubernetes.io/team],...`.
- AKS: `--ksm-metric-labels-allow-list`<br>Arc: `--AzureMonitorMetrics.KubeStateMetrics.MetricsLabelsAllowlist` 
  - Comma-separated list of more Kubernetes label keys that is used in the resource's kube_resource_labels metric kube_resource_labels metric. For example, kube_pod_labels is the labels metric for the pods resource. By default this metric contains only name and namespace labels. To include more labels, provide a list of resource names in their plural form and Kubernetes label keys that you want to allow for them A single `*` can be provided for each resource to allow any labels, but i this has severe performance implications. For example, `pods=[app],namespaces=[k8s-label-1,k8s-label-n,...],...`. |
- AKS: `--enable-windows-recording-rules` Lets you enable the recording rule groups required for proper functioning of the Windows dashboards.



### [Azure Resource Manager](#tab/arm)

Both ARM and Bicep templates are provided in the following sections.

#### Prerequisites

- The Azure Monitor workspace and Azure Managed Grafana instance must already be created.
- The template must be deployed in the same resource group as the Azure Managed Grafana instance.
- If the Azure Managed Grafana instance is in a subscription other than the Azure Monitor workspace subscription, register the Azure Monitor workspace subscription with the `Microsoft.Dashboard` resource provider using the guidance at [Register resource provider](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).
- Users with the `User Access Administrator` role in the subscription of the AKS cluster can enable the `Monitoring Reader` role directly by deploying the template.

> [!NOTE]
> Currently in Bicep, there's no way to explicitly scope the `Monitoring Reader` role assignment on a string parameter "resource ID" for an Azure Monitor workspace like in an ARM template. Bicep expects a value of type `resource | tenant`. There is also no REST API [spec](https://github.com/Azure/azure-rest-api-specs) for an Azure Monitor workspace.
> 
> Therefore, the default scoping for the `Monitoring Reader` role is on the resource group. The role is applied on the same Azure Monitor workspace by inheritance, which is the expected behavior. After you deploy this Bicep template, the Grafana instance is given `Monitoring Reader` permissions for all the Azure Monitor workspaces in that resource group.


#### Retrieve required values for Grafana resource
If the Azure Managed Grafana instance is already linked to an Azure Monitor workspace, then you must include this list in the template. On the **Overview** page for the Azure Managed Grafana instance in the Azure portal, select **JSON view**, and copy the value of `azureMonitorWorkspaceIntegrations` which will look similar to the sample below. If it doesn't exist, then the instance hasn't been linked with any Azure Monitor workspace.

```json
"properties": {
    "grafanaIntegrations": {
        "azureMonitorWorkspaceIntegrations": [
            {
                "azureMonitorWorkspaceResourceId": "full_resource_id_1"
            },
            {
                "azureMonitorWorkspaceResourceId": "full_resource_id_2"
            }
        ]
    }
}
```

#### Download and edit template and parameter file

1. Download the template and parameter file:

    **AKS cluster ARM**

    - Template file: [https://aka.ms/azureprometheus-enable-arm-template](https://aka.ms/azureprometheus-enable-arm-template)
    - Parameter file: [https://aka.ms/azureprometheus-enable-arm-template-parameters](https://aka.ms/azureprometheus-enable-arm-template-parameters)

    **AKS cluster Bicep**

    - Template file: [https://aka.ms/azureprometheus-enable-bicep-template](https://aka.ms/azureprometheus-enable-bicep-template)
    - Parameter file: [https://aka.ms/azureprometheus-enable-bicep-template-parameters](https://aka.ms/azureprometheus-enable-arm-template-parameters)
    - DCRA module: [https://aka.ms/nested_azuremonitormetrics_dcra_clusterResourceId](https://aka.ms/nested_azuremonitormetrics_dcra_clusterResourceId)
    - Profile module: [https://aka.ms/nested_azuremonitormetrics_profile_clusterResourceId](https://aka.ms/nested_azuremonitormetrics_profile_clusterResourceId)

    **Arc-Enabled cluster ARM**

    - Template file: [https://aka.ms/azureprometheus-arc-arm-template](https://aka.ms/azureprometheus-arc-arm-template)
    - Parameter file: [https://aka.ms/azureprometheus-arc-arm-template-parameters](https://aka.ms/azureprometheus-arc-arm-template-parameters)



2. Edit the following values in the parameter file.

    | Parameter | Value |
    |:---|:---|
    | `azureMonitorWorkspaceResourceId` | Resource ID for the Azure Monitor workspace. Retrieve from the **JSON view** on the **Overview** page for the Azure Monitor workspace. |
    | `azureMonitorWorkspaceLocation` | Location of the Azure Monitor workspace. Retrieve from the **JSON view** on the **Overview** page for the Azure Monitor workspace. |
    | `clusterResourceId` | Resource ID for the AKS cluster. Retrieve from the **JSON view** on the **Overview** page for the cluster. |
    | `clusterLocation` | Location of the AKS cluster. Retrieve from the **JSON view** on the **Overview** page for the cluster. |
    | `metricLabelsAllowlist` | Comma-separated list of Kubernetes labels keys to be used in the resource's labels metric. |
    | `metricAnnotationsAllowList` | Comma-separated list of more Kubernetes label keys to be used in the resource's annotations metric. |
    | `grafanaResourceId` | Resource ID for the managed Grafana instance. Retrieve from the **JSON view** on the **Overview** page for the Grafana instance. |
    | `grafanaLocation`   | Location for the managed Grafana instance. Retrieve from the **JSON view** on the **Overview** page for the Grafana instance. |
    | `grafanaSku`        | SKU for the managed Grafana instance. Retrieve from the **JSON view** on the **Overview** page for the Grafana instance. Use the **sku.name**. |



3. Open the template file and update the `grafanaIntegrations` property at the end of the file with the values that you retrieved from the Grafana instance. This will look similar to the following samples. In these samples, `full_resource_id_1` and `full_resource_id_2` were already in the Azure Managed Grafana resource JSON. The final `azureMonitorWorkspaceResourceId` entry is already in the template and is used to link to the Azure Monitor workspace resource ID provided in the parameters file.

    **ARM**

    ```json
    {
        "type": "Microsoft.Dashboard/grafana",
        "apiVersion": "2022-08-01",
        "name": "[split(parameters('grafanaResourceId'),'/')[8]]",
        "sku": {
            "name": "[parameters('grafanaSku')]"
        },
        "location": "[parameters('grafanaLocation')]",
        "properties": {
            "grafanaIntegrations": {
            "azureMonitorWorkspaceIntegrations": [
                {
                    "azureMonitorWorkspaceResourceId": "full_resource_id_1"
                },
                {
                    "azureMonitorWorkspaceResourceId": "full_resource_id_2"
                },
                {
                    "azureMonitorWorkspaceResourceId": "[parameters('azureMonitorWorkspaceResourceId')]"
                }
            ]
            }
        }
    }
    ```

    **Bicep**

    ```json
    {
        resource grafanaResourceId_8 'Microsoft.Dashboard/grafana@2022-08-01' = {
          name: split(grafanaResourceId, '/')[8]
          sku: {
            name: grafanaSku
          }
          identity: {
            type: 'SystemAssigned'
          }
          location: grafanaLocation
          properties: {
            grafanaIntegrations: {
              azureMonitorWorkspaceIntegrations: [
                {
                  azureMonitorWorkspaceResourceId: azureMonitorWorkspaceResourceId
                }
              ]
            }
          }
        }
    }
    ```


### [Terraform](#tab/terraform)

#### Prerequisites

- If the Azure Managed Grafana instance is in a subscription other than the Azure Monitor Workspaces subscription, register the Azure Monitor Workspace subscription with the `Microsoft.Dashboard` resource provider by following [this documentation](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).
- The Azure Monitor workspace and Azure Managed Grafana workspace must already be created.
- The template needs to be deployed in the same resource group as the Azure Managed Grafana workspace.
- Users with the User Access Administrator role in the subscription of the AKS cluster can enable the Monitoring Reader role directly by deploying the template.

#### Retrieve required values for a Grafana resource

On the **Overview** page for the Azure Managed Grafana instance in the Azure portal, select **JSON view**.

If you're using an existing Azure Managed Grafana instance that's already linked to an Azure Monitor workspace, you need the list of Grafana integrations. Copy the value of the `azureMonitorWorkspaceIntegrations` field. If it doesn't exist, the instance hasn't been linked with any Azure Monitor workspace. Update the azure_monitor_workspace_integrations block(shown here) in main.tf with the list of grafana integrations.

```.tf
  azure_monitor_workspace_integrations {
    resource_id  = var.monitor_workspace_id[var.monitor_workspace_id1, var.monitor_workspace_id2]
  }
```

#### Download and edit the templates

If you're deploying a new AKS cluster using Terraform with managed Prometheus addon enabled, follow these steps:

1. Download all files under [AddonTerraformTemplate](https://aka.ms/AAkm357).
2. Edit the variables in variables.tf file with the correct parameter values.
3. Run `terraform init -upgrade` to initialize the Terraform deployment.
4. Run `terraform plan -out main.tfplan` to initialize the Terraform deployment.
5. Run `terraform apply main.tfplan` to apply the execution plan to your cloud infrastructure.


Note: Pass the variables for `annotations_allowed` and `labels_allowed` keys in main.tf only when those values exist. These are optional blocks.

> [!NOTE]
> Edit the main.tf file appropriately before running the terraform template. Add in any existing azure_monitor_workspace_integrations values to the grafana resource before running the template. Else, older values gets deleted and replaced with what is there in the template during deployment. Users with 'User Access Administrator' role in the subscription  of the AKS cluster can enable 'Monitoring Reader' role directly by deploying the template. Edit the grafanaSku parameter if you're using a nonstandard SKU and finally run this template in the Grafana Resource's resource group.

### [Azure Policy](#tab/policy)

#### Download Azure Policy rules and parameters and deploy

1. Download Azure Policy template and parameter files.

   - Template file: [https://aka.ms/enable-monitoring-msi-azure-policy-template](https://aka.ms/enable-monitoring-msi-azure-policy-template)
   - Parameter file: [https://aka.ms/enable-monitoring-msi-azure-policy-parameters](https://aka.ms/enable-monitoring-msi-azure-policy-parameters)

1. Create the policy definition using the following command:

      `az policy definition create --name "Prometheus Metrics addon" --display-name "Prometheus Metrics addon" --mode Indexed --metadata version=1.0.0 category=Kubernetes --rules AddonPolicyMetricsProfile.rules.json --params AddonPolicyMetricsProfile.parameters.json`

1. After you create the policy definition, in the Azure portal, select **Policy** and then **Definitions**. Select the policy definition you created.
1. Select **Assign** and fill in the details on the **Parameters** tab,. Select **Review + Create**.
1. After the policy is assigned to the subscription, whenever you create a new cluster without Prometheus enabled, the policy will run and deploy to enable Prometheus monitoring. If you want to apply the policy to an existing cluster, create a **Remediation task** for that cluster resource from **Policy Assignment**.

Afterwards, if you create a new Managed Grafana instance, you can link it with the corresponding Azure Monitor workspace from the **Linked Grafana Workspaces** tab of the relevant **Azure Monitor Workspace** page. The `Monitoring Reader` role must be assigned to the managed identity of the Managed Grafana instance with the scope as the Azure Monitor workspace, so that Grafana has access to query the metrics. Use the following instructions to do so:

1. On the **Overview** page for the Azure Managed Grafana instance in the Azure portal, select **JSON view**.

1. Copy the value of the `principalId` field for the `SystemAssigned` identity.

    ```json
    "identity": {
            "principalId": "00000000-0000-0000-0000-000000000000",
            "tenantId": "00000000-0000-0000-0000-000000000000",
            "type": "SystemAssigned"
        },
    ```
1. On the **Access control (IAM)** page for the Azure Managed Grafana instance in the Azure portal, select **Add** > **Add role assignment**.
1. Select `Monitoring Reader`.
1. Select **Managed identity** > **Select members**.
1. Select the **system-assigned managed identity** with the `principalId` from the Grafana resource.
1. Choose **Select** > **Review+assign**.

##### Limitations during enablement/deployment

- Ensure that you update the `kube-state metrics` annotations and labels list with proper formatting. There's a limitation in the ARM template deployments that require exact values in the `kube-state` metrics pods. If the Kubernetes pod has any issues with malformed parameters and isn't running, the feature might not run as expected.
- A data collection rule and data collection endpoint are created with the name `MSProm-\<short-cluster-region\>-\<cluster-name\>`. Currently, these names can't be modified.
- You must get the existing Azure Monitor workspace integrations for a Grafana instance and update the ARM template with it. Otherwise, the ARM deployment gets over-written, which removes existing integrations.
---



## Enable Container insights
Use one of the following methods to enable Container insights on your cluster. Once this is complete, see [Configure agent data collection for Container insights](container-insights-agent-config.md) to customize your configuration to ensure that you aren't collecting more data than you require.


### [CLI](#tab/cli)

Use one of the following commands to enable monitoring of your AKS and Arc-enabled clusters. If you don't specify an existing Log Analytics workspace, the default workspace for the resource group will be used. If a default workspace doesn't already exist in the cluster's region, one will be created with a name in the format `DefaultWorkspace-<GUID>-<Region>`.

> [!NOTE]
> Managed identity authentication will be default in CLI version 2.49.0 or higher. For CLI version 2.54.0 or higher the logging schema will be configured to [ContainerLogV2](container-insights-logs-schema.md) via the ConfigMap.

#### AKS cluster
Use one of the following commands to enable monitoring of your AKS or Arc-enabled Kubernetes cluster. If you don't specify an existing Log Analytics workspace, the default workspace for the resource group will be used. If a default workspace doesn't already exist in the cluster's region, one will be created with a name in the format `DefaultWorkspace-<GUID>-<Region>`.

```azurecli
### Use default Log Analytics workspace
az aks enable-addons -a monitoring -n <cluster-name> -g <cluster-resource-group-name>

### Use existing Log Analytics workspace
az aks enable-addons -a monitoring -n <cluster-name> -g <cluster-resource-group-name> --workspace-resource-id <workspace-resource-id>
```

**Example**

```azurecli
az aks enable-addons -a monitoring -n <cluster-name> -g <cluster-resource-group-name> --workspace-resource-id "/subscriptions/my-subscription/resourceGroups/my-resource-group/providers/Microsoft.OperationalInsights/workspaces/my-workspace"
```


#### Arc-enabled cluster

>[!NOTE]
> Managed identity authentication is the default in k8s-extension version 1.43.0 or higher.
> 
> Managed identity authentication is not supported for Arc-enabled Kubernetes clusters with **ARO**.

```azurecli
### Use default Log Analytics workspace
az k8s-extension create --name azuremonitor-containers --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers

### Use existing Log Analytics workspace
az k8s-extension create --name azuremonitor-containers --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers --configuration-settings logAnalyticsWorkspaceResourceID=<armResourceIdOfExistingWorkspace>

### Use advanced configuration settings
az k8s-extension create --name azuremonitor-containers --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers --configuration-settings  amalogs.resources.daemonset.limits.cpu=150m amalogs.resources.daemonset.limits.memory=600Mi amalogs.resources.deployment.limits.cpu=1 amalogs.resources.deployment.limits.memory=750Mi

### On Azure Stack Edge
az k8s-extension create --name azuremonitor-containers --cluster-name <cluster-name> --resource-group <resource-group> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers --configuration-settings amalogs.logsettings.custommountpath=/home/data/docker

```

**Example**

```azurecli
az aks enable-addons -a monitoring -n <cluster-name> -g <cluster-resource-group-name> --workspace-resource-id "/subscriptions/my-subscription/resourceGroups/my-resource-group/providers/Microsoft.OperationalInsights/workspaces/my-workspace"
```



### [Azure Resource Manager](#tab/arm)

Both ARM and Bicep templates are provided in the following sections.

#### Prerequisites
 
- The template must be deployed in the same resource group as the cluster.

#### Download and install template

1. Download the template and parameter file.

    **AKS cluster ARM**
   - Template file: [https://aka.ms/aks-enable-monitoring-msi-onboarding-template-file](https://aka.ms/aks-enable-monitoring-msi-onboarding-template-file)
   - Parameter file: [https://aka.ms/aks-enable-monitoring-msi-onboarding-template-parameter-file](https://aka.ms/aks-enable-monitoring-msi-onboarding-template-parameter-file)

    **AKS  cluster Bicep**
    - Template file (Syslog): [https://aka.ms/enable-monitoring-msi-syslog-bicep-template](https://aka.ms/enable-monitoring-msi-syslog-bicep-template)
    - Parameter file (No Syslog): [https://aka.ms/enable-monitoring-msi-syslog-bicep-parameters](https://aka.ms/enable-monitoring-msi-syslog-bicep-parameters)
    - Template file (No Syslog): [https://aka.ms/enable-monitoring-msi-bicep-template](https://aka.ms/enable-monitoring-msi-bicep-template)
    - Parameter file (No Syslog): [https://aka.ms/enable-monitoring-msi-bicep-parameters](https://aka.ms/enable-monitoring-msi-bicep-parameters)

    **Arc-enabled cluster ARM**
    - Template file: [https://aka.ms/arc-k8s-azmon-extension-arm-template](https://aka.ms/arc-k8s-azmon-extension-arm-template)
    - Parameter file: [https://aka.ms/arc-k8s-azmon-extension-arm-template-params](https://aka.ms/arc-k8s-azmon-extension-arm-template-params)

2. Edit the following values in the parameter file. Retrieve the resource ID of the resources from the **JSON View** of their **Overview** page.

    | Parameter | Description |
    |:---|:---|
    | AKS: `aksResourceId`<br>Arc: `clusterResourceId`  | Resource ID of the cluster. |
    | AKS: `aksResourceLocation`<br>Arc: `clusterRegion` | Location of the cluster. |
    | AKS: `workspaceResourceId`<br>Arc: `workspaceResourceId` | Resource ID of the Log Analytics workspace. |
    | Arc: `workspaceRegion` | Region of the Log Analytics workspace. |
    | Arc: `workspaceDomain` | Domain of the Log Analytics workspace.<br>`opinsights.azure.com` for Azure public cloud<br>`opinsights.azure.us for AzureUSGovernment.` |
    | AKS: `resourceTagValues` | Tag values specified for the existing Container insights extension data collection rule (DCR) of the cluster and the name of the DCR. The name will be `MSCI-<clusterName>-<clusterRegion>` and this resource created in an AKS clusters resource group. For first time onboarding, you can set arbitrary tag values. |


3. Deploy the template with the parameter file by using any valid method for deploying Resource Manager templates. For examples of different methods, see [Deploy the sample templates](../resource-manager-samples.md#deploy-the-sample-templates).



### [Terraform](#tab/terraform)

#### New AKS cluster

1.	Download Terraform template file depending on whether you want to enable Syslog collection.

    **Syslog**
    - [https://aka.ms/enable-monitoring-msi-syslog-terraform](https://aka.ms/enable-monitoring-msi-syslog-terraform)

    **No Syslog** 
    - [https://aka.ms/enable-monitoring-msi-terraform](https://aka.ms/enable-monitoring-msi-terraform)

2.	Adjust the `azurerm_kubernetes_cluster` resource in *main.tf* based on your cluster settings.
3.	Update parameters in *variables.tf* to replace values in "<>"

    | Parameter | Description |
    |:---|:---|
    | `aks_resource_group_name` | Use the values on the AKS Overview page for the resource group. |
    | `resource_group_location` | Use the values on the AKS Overview page for the resource group. |
    | `cluster_name` | Define the cluster name that you would like to create. |
    | `workspace_resource_id` | Use the resource ID of your Log Analytics workspace. |
    | `workspace_region` | Use the location of your Log Analytics workspace. |
    | `resource_tag_values` | Match the existing tag values specified for the existing Container insights extension data collection rule (DCR) of the cluster and the name of the DCR. The name will match `MSCI-<clusterName>-<clusterRegion>` and this resource is created in the same resource group as the AKS clusters. For first time onboarding, you can set the arbitrary tag values. |
    | `enabledContainerLogV2` | Set this parameter value to be true to use the default recommended ContainerLogV2. |
    | Cost optimization parameters | Refer to [Data collection parameters](container-insights-cost-config.md#data-collection-parameters) |


4.	Run `terraform init -upgrade` to initialize the Terraform deployment.
5.	Run `terraform plan -out main.tfplan` to initialize the Terraform deployment.
6.	Run `terraform apply main.tfplan` to apply the execution plan to your cloud infrastructure.


#### Existing AKS cluster
1.	Import the existing cluster resource first with the command: ` terraform import azurerm_kubernetes_cluster.k8s <aksResourceId>`
2.	Add the oms_agent add-on profile to the existing azurerm_kubernetes_cluster resource.
    ```
    oms_agent {
        log_analytics_workspace_id = var.workspace_resource_id
        msi_auth_for_monitoring_enabled = true
      }
    ```
3.	Copy the DCR and DCRA resources from the Terraform templates
4.	Run `terraform plan -out main.tfplan` and make sure the change is adding the oms_agent property. Note: If the `azurerm_kubernetes_cluster` resource defined is different during terraform plan, the existing cluster will get destroyed and recreated.
5.	Run `terraform apply main.tfplan` to apply the execution plan to your cloud infrastructure.

> [!TIP]
> - Edit the `main.tf` file appropriately before running the terraform template
> - Data will start flowing after 10 minutes since the cluster needs to be ready first
> - WorkspaceID needs to match the format `/subscriptions/12345678-1234-9876-4563-123456789012/resourceGroups/example-resource-group/providers/Microsoft.OperationalInsights/workspaces/workspaceValue`
> - If resource group already exists, run `terraform import azurerm_resource_group.rg /subscriptions/<Subscription_ID>/resourceGroups/<Resource_Group_Name>` before terraform plan

### [Azure Policy](#tab/policy)

1. Download Azure Policy template and parameter files.

   - Template file: [https://aka.ms/enable-monitoring-msi-azure-policy-template](https://aka.ms/enable-monitoring-msi-azure-policy-template)
   - Parameter file: [https://aka.ms/enable-monitoring-msi-azure-policy-parameters](https://aka.ms/enable-monitoring-msi-azure-policy-parameters)

2. Create the policy definition and assignment using the following CLI commands:

    ```
    ### Create policy definition    
    az policy definition create --name "AKS-Monitoring-Addon-MSI" --display-name "AKS-Monitoring-Addon-MSI" --mode Indexed --metadata version=1.0.0 category=Kubernetes --rules azure-policy.rules.json --params azure-policy.parameters.json
    
    ### Create policy assignment
    az policy assignment create --name aks-monitoring-addon --policy "AKS-Monitoring-Addon-MSI" --assign-identity --identity-scope /subscriptions/<subscriptionId> --role Contributor --scope /subscriptions/<subscriptionId> --location <location> --role Contributor --scope /subscriptions/<subscriptionId> -p "{ \"workspaceResourceId\": { \"value\":  \"/subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/microsoft.operationalinsights/workspaces/<workspaceName>\" } }"
    ```

1. After you create the policy definition, in the Azure portal, select **Policy** and then **Definitions**. Select the policy definition you created.

2. Select **Assign** and fill in the details on the **Parameters** tab,. Select **Review + Create**.

3. After the policy is assigned to the subscription, whenever you create a new cluster without Prometheus enabled, the policy will run and deploy to enable Prometheus monitoring. If you want to apply the policy to an existing cluster, create a **Remediation task** for that cluster resource from **Policy Assignment**.

> [!TIP]
> - Make sure when performing remediation task, the policy assignment has access to workspace you specified.
> - Download all files under *AddonPolicyTemplate* folder before running the policy template.

---





## Enable full monitoring with Azure portal
Using the Azure portal, you can enable both Managed Prometheus and Container insights at the same time. 

> [!NOTE]
> If you want to enabled Managed Prometheus without Container insights, then [enable it from the Azure Monitor workspace](./prometheus-metrics-enable.md) as described below.

### New AKS cluster

When you create a new AKS cluster in the Azure portal, you can enable Prometheus, Container insights, and Grafana from the **Integrations** tab. In the Azure Monitor section, select either **Default configuration** or **Custom configuration** if you want to specify which workspaces to use. You can perform additional configuration once the cluster is created.

:::image type="content" source="media/prometheus-metrics-enable/aks-integrations.png" lightbox="media/prometheus-metrics-enable/aks-integrations.png" alt-text="Screenshot of integrations tab for new AKS cluster.":::

### Full monitoring on existing cluster

This option enables Container insights and optionally Prometheus and Grafana on an existing AKS cluster. 

1. Either select **Insights** from the cluster's menu OR select **Containers** from the **Monitor** menu, **Unmonitored clusters** tab, and click **Enable** next to a cluster.
   1. If Container insights isn't enabled for the cluster, then you're presented with a screen identifying which of the features have been enabled. Click **Configure monitoring**.

    :::image type="content" source="media/aks-onboard/configure-monitoring-screen.png" lightbox="media/aks-onboard/configure-monitoring-screen.png" alt-text="Screenshot that shows the configuration screen for a cluster.":::

   2. If Container insights has already been enabled on the cluster, select the **Monitoring Settings** button to modify the configuration.

    :::image type="content" source="media/aks-onboard/monitor-settings-button.png" lightbox="media/aks-onboard/monitor-settings-button.png" alt-text="Screenshot that shows the monitoring settings button for a cluster.":::

2. **Container insights** will be enabled. **Select** the checkboxes for **Enable Prometheus metrics** and **Enable Grafana** if you also want to enable them for the cluster. If you have existing Azure Monitor workspace and Grafana workspace, then they're selected for you. 

    :::image type="content" source="media/prometheus-metrics-enable/configure-container-insights.png" lightbox="media/prometheus-metrics-enable/configure-container-insights.png" alt-text="Screenshot that shows the dialog box to configure Container insights with Prometheus and Grafana.":::

3. Click **Advanced settings** to select alternate workspaces or create new ones. The **Cost presets** setting allows you to modify the default collection details to reduce your monitoring costs. See [Enable cost optimization settings in Container insights](./container-insights-cost-config.md) for details.

    :::image type="content" source="media/aks-onboard/advanced-settings.png" lightbox="media/aks-onboard/advanced-settings.png" alt-text="Screenshot that shows the advanced settings dialog box.":::

4. Click **Configure** to save the configuration.

### Prometheus only on existing cluster

This option enables Prometheus metrics on a cluster without enabling Container insights.

1. Open the **Azure Monitor workspaces** menu in the Azure portal and select your workspace.
1. Select **Monitored clusters** in the **Managed Prometheus** section to display a list of AKS clusters.
1. Select **Configure** next to the cluster you want to enable.

    :::image type="content" source="media/prometheus-metrics-enable/azure-monitor-workspace-configure-prometheus.png" lightbox="media/prometheus-metrics-enable/azure-monitor-workspace-configure-prometheus.png" alt-text="Screenshot that shows an Azure Monitor workspace with a Prometheus configuration.":::

### Add Prometheus to cluster with Container insights

1. Select **Containers** from the **Monitor** menu, **Monitored clusters** tab, and click **Configure** next to a cluster in the **Managed Prometheus** column.


## Enable Windows metrics collection

> [!NOTE]
> There is no CPU/Memory limit in windows-exporter-daemonset.yaml so it may over-provision the Windows nodes  
> For more details see [Resource reservation](https://kubernetes.io/docs/concepts/configuration/windows-resource-management/#resource-reservation)
>   
> As you deploy workloads, set resource memory and CPU limits on containers. This also subtracts from NodeAllocatable and helps the cluster-wide scheduler in determining which pods to place on which nodes.
> Scheduling pods without limits may over-provision the Windows nodes and in extreme cases can cause the nodes to become unhealthy.


As of version 6.4.0-main-02-22-2023-3ee44b9e of the Managed Prometheus addon container (prometheus_collector), Windows metric collection has been enabled for the AKS clusters. Onboarding to the Azure Monitor Metrics add-on enables the Windows DaemonSet pods to start running on your node pools. Both Windows Server 2019 and Windows Server 2022 are supported. Follow these steps to enable the pods to collect metrics from your Windows node pools.

1. Manually install windows-exporter on AKS nodes to access Windows metrics.
   Enable the following collectors:

   * `[defaults]`
   * `container`
   * `memory`
   * `process`
   * `cpu_info`

   Deploy the [windows-exporter-daemonset YAML](https://github.com/prometheus-community/windows_exporter/blob/master/kubernetes/windows-exporter-daemonset.yaml) file:

   ```
       kubectl apply -f windows-exporter-daemonset.yaml
   ```

1. Apply the [ama-metrics-settings-configmap](https://github.com/Azure/prometheus-collector/blob/main/otelcollector/configmaps/ama-metrics-settings-configmap.yaml) to your cluster. Set the `windowsexporter` and `windowskubeproxy` Booleans to `true`. For more information, see [Metrics add-on settings configmap](./prometheus-metrics-scrape-configuration.md#metrics-add-on-settings-configmap).
1. Enable the recording rules that are required for the out-of-the-box dashboards:

   * If onboarding using the CLI, include the option `--enable-windows-recording-rules`.
   * If onboarding using an ARM template, Bicep, or Azure Policy, set `enableWindowsRecordingRules` to `true` in the parameters file.
   * If the cluster is already onboarded, use [this ARM template](https://github.com/Azure/prometheus-collector/blob/kaveesh/windows_recording_rules/AddonArmTemplate/WindowsRecordingRuleGroupTemplate/WindowsRecordingRules.json) and [this parameter file](https://github.com/Azure/prometheus-collector/blob/kaveesh/windows_recording_rules/AddonArmTemplate/WindowsRecordingRuleGroupTemplate/WindowsRecordingRulesParameters.json) to create the rule groups.





## Verify deployment
You can verify that the agent is deployed properly using the [kubectl command line tool](../../aks/learn/quick-kubernetes-deploy-cli.md#connect-to-the-cluster).


**Verify that the DaemonSets were deployed properly on the Linux node pools**

```
kubectl get ds ama-metrics-node --namespace=kube-system
kubectl get ds ama-logs --namespace=kube-system
```

The number of pods should be equal to the number of Linux nodes on the cluster. The output should resemble the following example:

```
User@aksuser:~$ kubectl get ds ama-logs --namespace=kube-system
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
ama-logs   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
 
User@aksuser:~$ kubectl get ds ama-metrics-node --namespace=kube-system
NAME               DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
ama-metrics-node   1         1         1       1            1           <none>          10h
```

**Verify that Windows nodes were deployed properly**

```
kubectl get ds ama-logs-windows --namespace=kube-system
kubectl get ds ama-metrics-win-node --namespace=kube-system
```

The number of pods should be equal to the number of Windows nodes on the cluster. The output should resemble the following example:

```output
User@aksuser:~$ kubectl get ds ama-logs-windows --namespace=kube-system
NAME                   DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                   AGE
ama-logs-windows           2         2         2         2            2           beta.kubernetes.io/os=windows   1d

User@aksuser:~$ kubectl get ds ama-metrics-node --namespace=kube-system
NAME                   DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
ama-metrics-win-node   3         3         3       3            3           <none>          10h
```


**Verify deployment of the Container insights solution**

```
kubectl get deployment ama-logs-rs -n=kube-system
```

The output should resemble the following example:

```output
User@aksuser:~$ kubectl get deployment ama-logs-rs -n=kube-system
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
ama-logs-rs   1         1         1            1            3h
```

**Verify that the two ReplicaSets were deployed nfor Prometheus**

```
kubectl get rs --namespace=kube-system
```

The output should resemble the following example:

```
User@aksuser:~$kubectl get rs --namespace=kube-system
NAME                            DESIRED   CURRENT   READY   AGE
ama-metrics-5c974985b8          1         1         1       11h
ama-metrics-ksm-5fcf8dffcd      1         1         1       11h
```


## View configuration with CLI

Use the `aks show` command to find out whether the solution is enabled or not, what the Log Analytics workspace resource ID is, and summary information about the cluster.

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

The command will return JSON-formatted information about the solution. The `addonProfiles` section should include information on the `omsagent` as in the following example:

```output
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```


## Resources provisioned

When you enable metrics addon, the following resources are provisioned:

| Resource Name | Resource Type | Resource Group | Region/Location | Description |
    |:---|:---|:---|:---|:---|
    | `MSCI-<aksclusterregion>-<clustername>` | **Data Collection Rule** | Same Resource group as AKS cluster resource | Same region as Azure Monitor Workspace | This data collection rule is for log collection by Azure Monitor agent, which uses the Log Analytics workspace as destination, and is associated to the AKS cluster resource. |
    | `MSPROM-<aksclusterregion>-<clustername>` | **Data Collection Rule** | Same Resource group as AKS cluster resource | Same region as Azure Monitor Workspace | This data collection rule is for prometheus metrics collection by metrics addon, which has the chosen Azure monitor workspace as destination, and also it is associated to the AKS cluster resource |
    | `MSPROM-<aksclusterregion>-<clustername>` | **Data Collection endpoint** | Same Resource group as AKS cluster resource | Same region as Azure Monitor Workspace | This data collection endpoint is used by the above data collection rule for ingesting Prometheus metrics from the metrics addon|
    
When you create a new Azure Monitor workspace, the following additional resources are created as part of it

| Resource Name | Resource Type | Resource Group | Region/Location | Description |
    |:---|:---|:---|:---|:---|
    | `<azuremonitor-workspace-name>` | **System Data Collection Rule** | MA_\<azuremonitor-workspace-name>_\<azuremonitor-workspace-region>_managed | Same region as Azure Monitor Workspace | This is **system** data collection rule that customers can use when they use OSS Prometheus server to Remote Write to Azure Monitor Workspace |
    | `<azuremonitor-workspace-name>` | **System Data Collection endpoint** | MA_\<azuremonitor-workspace-name>_\<azuremonitor-workspace-region>_managed | Same region as Azure Monitor Workspace | This is **system** data collection endpoint that customers can use when they use OSS Prometheus server to Remote Write to Azure Monitor Workspace |
    




## Limitations

- Dependency on DCR/DCRA for region availability. For new AKS region, there might be chances that DCR is still not supported in the new region. In that case, onboarding Container Insights with MSI will fail. One workaround is to onboard to Container Insights through CLI with the old way (with the use of Container Insights solution)
- You must be on a machine on the same private network to access live logs from a private cluster.

## Differences between Windows and Linux clusters

The main differences in monitoring a Windows Server cluster compared to a Linux cluster include:

- Windows doesn't have a Memory RSS metric. As a result, it isn't available for Windows nodes and containers. The [Working Set](/windows/win32/memory/working-set) metric is available.
- Disk storage capacity information isn't available for Windows nodes.
- Only pod environments are monitored, not Docker environments.
- With the preview release, a maximum of 30 Windows Server containers are supported. This limitation doesn't apply to Linux containers.

>[!NOTE]
> Container insights support for the Windows Server 2022 operating system is in preview.


The containerized Linux agent (replicaset pod) makes API calls to all the Windows nodes on Kubelet secure port (10250) within the cluster to collect node and container performance-related metrics. Kubelet secure port (:10250) should be opened in the cluster's virtual network for both inbound and outbound for Windows node and container performance-related metrics collection to work.

If you have a Kubernetes cluster with Windows nodes, review and configure the network security group and network policies to make sure the Kubelet secure port (:10250) is open for both inbound and outbound in the cluster's virtual network.



## Troubleshooting
If you registered your cluster and/or configured HCI Insights before November 2023, features that use the AMA agent on HCI, such as Arc for Servers Insights, VM Insights, Container Insights, Defender for Cloud or Sentinel might not be collecting logs and event data properly. See [Repair AMA agent for HCI](/azure-stack/hci/manage/monitor-hci-single?tabs=22h2-and-later) for steps to reconfigure the AMA agent and HCI Insights.


## Next steps

* If you experience issues while you attempt to onboard the solution, review the [Troubleshooting guide](container-insights-troubleshoot.md).
* With monitoring enabled to collect health and resource utilization of your AKS cluster and workloads running on them, learn [how to use](container-insights-analyze.md) Container insights.
