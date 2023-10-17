---
title: Configure agent authentication for the Container Insights agent
description: This article describes how to configure authentication for the containerized agent used by Container insights.
ms.topic: conceptual
ms.date: 07/31/2023
ms.reviewer: aul
---

# Legacy authentication for Container Insights 

Container Insights defaults to managed identity authentication. This authentication model has a monitoring agent that uses the [cluster's managed identity](../../aks/use-managed-identity.md) to send data to Azure Monitor. It replaced the legacy certificate-based local authentication and removed the requirement of adding a Monitoring Metrics Publisher role to the cluster.

This article describes how to migrate to managed identity authentication if you enabled Container insights using legacy authentication method and also how to enable legacy authentication if you have that requirement.

> [!Note] 
> [ContainerLogV2](container-insights-logging-v2.md) is the default schema for customers who will be onboarding container insights with Managed Identity Auth using ARM, Bicep, Terraform, Policy and Portal onboarding. ContainerLogV2 can be explicitly enabled through CLI version 2.51.0 or higher using Data collection settings.

## Migrate to managed identity authentication

If you enabled Container insights before managed identity authentication was available, you can use the following methods to migrate your clusters.

## [Azure portal](#tab/portal-azure-monitor)

For existing clusters, you can switch to Managed Identity authentication from the *Monitor settings* panel: Navigate to your AKS cluster, scroll through the menu on the left till you see the **Monitoring** section, there click on the **Insights** tab. In the Insights tab, click on the *Monitor Settings* option and check the box for *Use managed identity*

:::image type="content" source="./media/container-insights-authentication/monitor-settings.png" alt-text="Screenshot that shows the settings panel." lightbox="media/container-insights-authentication/monitor-settings.png":::

If you don't see the *Use managed identity* option, you are using an SPN cluster. In that case, you must use command line tools to migrate. See other tabs for migration instructions and templates.

## [Azure CLI](#tab/cli)

### AKS

**Existing clusters with a service principal**

AKS clusters with a service principal must first disable monitoring and then upgrade to managed identity. Only Azure public cloud, Microsoft Azure operated by 21Vianet cloud, and Azure Government cloud are currently supported for this migration.

> [!NOTE]
> Minimum Azure CLI version 2.49.0 or higher.

1. Get the configured Log Analytics workspace resource ID:

    ```cli
    az aks show -g <resource-group-name> -n <cluster-name> | grep -i "logAnalyticsWorkspaceResourceID"
    ```

1. Disable monitoring with the following command:

      ```cli
      az aks disable-addons -a monitoring -g <resource-group-name> -n <cluster-name> 
      ```

1. Upgrade cluster to system managed identity with the following command:

      ```cli
      az aks update -g <resource-group-name> -n <cluster-name> --enable-managed-identity
      ```

1. Enable the monitoring add-on with the managed identity authentication option by using the Log Analytics workspace resource ID obtained in step 1:

      ```cli
      az aks enable-addons -a monitoring -g <resource-group-name> -n <cluster-name> --workspace-resource-id <workspace-resource-id>
      ```


**Existing clusters with system or user-assigned identity**

AKS clusters with system-assigned identity must first disable monitoring and then upgrade to managed identity. Only Azure public cloud, Azure operated by 21Vianet cloud, and Azure Government cloud are currently supported for clusters with system identity. For clusters with user-assigned identity, only Azure public cloud is supported.

> [!NOTE]
> Minimum Azure CLI version 2.49.0 or higher.

1. Get the configured Log Analytics workspace resource ID:

      ```cli
      az aks show -g <resource-group-name> -n <cluster-name> | grep -i "logAnalyticsWorkspaceResourceID"
      ```

1. Disable monitoring with the following command:

      ```cli
      az aks disable-addons -a monitoring -g <resource-group-name> -n <cluster-name>
      ```

1. Enable the monitoring add-on with the managed identity authentication option by using the Log Analytics workspace resource ID obtained in step 1:

      ```cli
      az aks enable-addons -a monitoring -g <resource-group-name> -n <cluster-name> --workspace-resource-id <workspace-resource-id>
      ```

### Arc-enabled Kubernetes

>[!NOTE]
> Managed identity authentication is not supported for Arc-enabled Kubernetes clusters with **ARO**.

## [CLI](#tab/migrate-cli)
First retrieve the Log Analytics workspace configured for Container insights extension.

```cli
az k8s-extension show --name azuremonitor-containers --cluster-name \<cluster-name\> --resource-group \<resource-group\> --cluster-type connectedClusters -n azuremonitor-containers 
```

Enable Container insights extension with managed identity authentication option using the workspace returned in the first step. 

```cli
az k8s-extension create --name azuremonitor-containers --cluster-name \<cluster-name\> --resource-group \<resource-group\> --cluster-type connectedClusters --extension-type Microsoft.AzureMonitor.Containers --configuration-settings amalogs.useAADAuth=true logAnalyticsWorkspaceResourceID=\<workspace-resource-id\> 
```

## [Resource Manager template](#tab/arm)

See instructions for migrating 

**Arc-enabled clusters**



1. Download the template at [https://aka.ms/arc-k8s-azmon-extension-msi-arm-template](https://aka.ms/arc-k8s-azmon-extension-msi-arm-template) and save it as **arc-k8s-azmon-extension-msi-arm-template.json**.

2. Download the parameter file at [https://aka.ms/arc-k8s-azmon-extension-msi-arm-template-params](https://aka.ms/arc-k8s-azmon-extension-msi-arm-template) and save it as **arc-k8s-azmon-extension-msi-arm-template-params.json**.
 
3. Edit the values in the parameter file.

  - For **workspaceDomain**, use *opinsights.azure.com* for Azure public cloud and *opinsights.azure.us* for Azure Government cloud.
  - Specify the tags in the **resourceTagValues** parameter if you want to use any Azure tags on the Azure resources that will be created as part of the Container insights extension.

4. Deploy the template to create Container Insights extension. 

```cli
az login 
az account set --subscription "Subscription Name" 
az deployment group create --resource-group <resource-group> --template-file ./arc-k8s-azmon-extension-msi-arm-template.json --parameters @./arc-k8s-azmon-extension-msi-arm-template-params.json 
```

## [Bicep](#tab/bicep)

**Enable Monitoring with MSI without syslog** 

1.	Download Bicep templates and Parameter files 

```
curl  -L https://aka.ms/enable-monitoring-msi-bicep-template -o existingClusterOnboarding.bicep 
curl  -L https://aka.ms/enable-monitoring-msi-bicep-parameters -o existingClusterParam.json
```

2.	Edit the values in the parameter file
 
 - **aksResourceId**: Use the values on the AKS Overview page for the AKS cluster.
 - **aksResourceLocation**: Use the values on the AKS Overview page for the AKS cluster.
 - **workspaceResourceId**: Use the resource ID of your Log Analytics workspace.
 - **workspaceRegion**: Use the location of your Log Analytics workspace.
 - **resourceTagValues**: Match the existing tag values specified for the existing Container insights extension data collection rule (DCR) of the cluster and the name of the DCR. The name will match `MSCI-<clusterName>-<clusterRegion>` and this resource is created in the same resource group as the AKS clusters. For first time onboarding, you can set the arbitrary tag values.
 - **enabledContainerLogV2**: Set this parameter value to be true to use the default recommended ContainerLogV2 schema
 - Other parameters are for cost optimization, refer to [this guide](container-insights-cost-config.md?tabs=create-CLI#data-collection-parameters)

3.	Onboard with the following commands:

```
az login
az account set --subscription "Subscription Name"
az deployment group create --resource-group <ClusterResourceGroupName> --template-file ./existingClusterOnboarding.bicep --parameters ./existingClusterParam.json
```

**Enable Monitoring with MSI with syslog**

1.	Download Bicep templates and Parameter files 

```
 	curl  -L https://aka.ms/enable-monitoring-msi-syslog-bicep-template  -o existingClusterOnboarding.bicep 
 	curl  -L https://aka.ms/enable-monitoring-msi-syslog-bicep-parameters -o existingClusterParam.json
```

2.	Edit the values in the parameter file

- **aksResourceId**: Use the values on the AKS Overview page for the AKS cluster.
- **aksResourceLocation**: Use the values on the AKS Overview page for the AKS cluster.
- **workspaceResourceId**: Use the resource ID of your Log Analytics workspace.
- **workspaceRegion**: Use the location of your Log Analytics workspace.
- **resourceTagValues**: Match the existing tag values specified for the existing Container insights extension data collection rule (DCR) of the cluster and the name of the DCR. The name match `MSCI-<clusterName>-<clusterRegion>` and this resource is created in the same resource group as the AKS clusters. For first time onboarding, you can set the arbitrary tag values.
- - **enabledContainerLogV2**: Set this parameter value to be true to use the default recommended ContainerLogV2 

3.	Onboarding with the following commands:

```
az login
az account set --subscription "Subscription Name"
az deployment group create --resource-group <ClusterResourceGroupName> --template-file ./existingClusterOnboarding.bicep --parameters ./existingClusterParam.json
```

For new AKS cluster:
Replace and use the managed cluster resources in this [guide](../../aks/learn/quick-kubernetes-deploy-bicep.md?tabs=azure-cli)


## [Terraform](#tab/terraform)

**Enable Monitoring with MSI without syslog for new AKS cluster**

1.	Download Terraform template for enable monitoring msi with syslog enabled:
https://aka.ms/enable-monitoring-msi-terraform
2.	Adjust the azurerm_kubernetes_cluster resource in main.tf based on what cluster settings you're going to have
3.	Update parameters in variables.tf to replace values in "<>"
 - **aks_resource_group_name**: Use the values on the AKS Overview page for the resource group.
 - **resource_group_location**: Use the values on the AKS Overview page for the resource group.
 - **cluster_name**: Define the cluster name that you would like to create
 - **workspace_resource_id**: Use the resource ID of your Log Analytics workspace.
 - **workspace_region**: Use the location of your Log Analytics workspace.
 - **resource_tag_values**: Match the existing tag values specified for the existing Container insights extension data collection rule (DCR) of the cluster and the name of the DCR. The name match `MSCI-<clusterName>-<clusterRegion>` and this resource is created in the same resource group as the AKS clusters. For first time onboarding, you can set the arbitrary tag values.
 - - **enabledContainerLogV2**: Set this parameter value to be true to use the default recommended ContainerLogV2 
 - Other parameters are for cluster settings or cost optimization, refer to [this guide](container-insights-cost-config.md?tabs=create-CLI#data-collection-parameters)
4.	Run `terraform init -upgrade` to initialize the Terraform deployment.
5.	Run `terraform plan -out main.tfplan` to initialize the Terraform deployment.
6.	Run `terraform apply main.tfplan` to apply the execution plan to your cloud infrastructure.

**Enable Monitoring with MSI with syslog for new AKS cluster** 
1.	Download Terraform template for enable monitoring msi with syslog enabled:
https://aka.ms/enable-monitoring-msi-syslog-terraform
2.	Adjust the azurerm_kubernetes_cluster resource in main.tf based on what cluster settings you're going to have
3.	Update parameters in variables.tf to replace values in "<>"
 - **aks_resource_group_name**: Use the values on the AKS Overview page for the resource group.
 - **resource_group_location**: Use the values on the AKS Overview page for the resource group.
 - **cluster_name**: Define the cluster name that you would like to create
 - **workspace_resource_id**: Use the resource ID of your Log Analytics workspace.
 - **workspace_region**: Use the location of your Log Analytics workspace.
 - **resource_tag_values**: Match the existing tag values specified for the existing Container insights extension data collection rule (DCR) of the cluster and the name of the DCR. The name match `MSCI-<clusterName>-<clusterRegion>` and this resource is created in the same resource group as the AKS clusters. For first time onboarding, you can set the arbitrary tag values.
 - Other parameters are for cluster settings, refer [to guide](container-insights-cost-config.md?tabs=create-CLI#data-collection-parameters)
4.	Run `terraform init -upgrade` to initialize the Terraform deployment.
5.	Run `terraform plan -out main.tfplan` to initialize the Terraform deployment.
6.	Run `terraform apply main.tfplan` to apply the execution plan to your cloud infrastructure.

**Enable Monitoring with MSI for existing AKS cluster:**
1.	Import the existing cluster resource first with this command: ` terraform import azurerm_kubernetes_cluster.k8s <aksResourceId>`
2.	Add the oms_agent add-on profile to the existing azurerm_kubernetes_cluster resource.
```
oms_agent {
    log_analytics_workspace_id = var.workspace_resource_id
    msi_auth_for_monitoring_enabled = true
  }
```
3.	Copy the dcr and dcra resources from the Terraform templates
4.	Run `terraform plan -out main.tfplan` and make sure the change is adding the oms_agent property. Note: If the azurerm_kubernetes_cluster resource defined is different during terraform plan, the existing cluster will get destroyed and recreated.
5.	Run `terraform apply main.tfplan` to apply the execution plan to your cloud infrastructure.

> [!TIP]
> - Edit the `main.tf` file appropriately before running the terraform template
> - Data will start flowing after 10 minutes since the cluster needs to be ready first
> - WorkspaceID needs to match the format `/subscriptions/12345678-1234-9876-4563-123456789012/resourceGroups/example-resource-group/providers/Microsoft.OperationalInsights/workspaces/workspaceValue`
> - If resource group already exists, run `terraform import azurerm_resource_group.rg /subscriptions/<Subscription_ID>/resourceGroups/<Resource_Group_Name>` before terraform plan

## [Azure Policy](#tab/policy)

1. Download Azure Policy templates and parameter files using the following commands: 

```
curl  -L https://aka.ms/enable-monitoring-msi-azure-policy-template -o azure-policy.rules.json 
curl  -L https://aka.ms/enable-monitoring-msi-azure-policy-parameters -o azure-policy.parameters.json
```


2. Activate the policies: 

You can create the policy definition using a command:
```
az policy definition create --name "AKS-Monitoring-Addon-MSI" --display-name "AKS-Monitoring-Addon-MSI" --mode Indexed --metadata version=1.0.0 category=Kubernetes --rules azure-policy.rules.json --params azure-policy.parameters.json
```
You can create the policy assignment with the following command like:
```
az policy assignment create --name aks-monitoring-addon --policy "AKS-Monitoring-Addon-MSI" --assign-identity --identity-scope /subscriptions/<subscriptionId> --role Contributor --scope /subscriptions/<subscriptionId> --location <location> --role Contributor --scope /subscriptions/<subscriptionId> -p "{ \"workspaceResourceId\": { \"value\":  \"/subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/microsoft.operationalinsights/workspaces/<workspaceName>\" } }"
```

> [!TIP]
> - Make sure when performing remediation task, the policy assignment has access to workspace you specified.
> - Download all files under AddonPolicyTemplate folder before running the policy template.
> - For assign policy, parameters and remediation task from portal, use the following guides:
>  o	After creating the policy definition through the above command, go to Azure portal -> Policy -> Definitions and select the definition you created.
>  o	Click on 'Assign' and then go to the 'Parameters' tab and fill in the details. Then click 'Review + Create'.
>  o	Once the policy is assigned to the subscription, whenever you create a new cluster, the policy will run and check if Container Insights is enabled. If not, it will deploy the resource. If you want to apply the policy to existing AKS cluster, create a 'Remediation task' for that resource after going to the 'Policy Assignment'.

---

## Migrate to managed identity authentication

This section explains two methods for migrating to managed identity authentication.

### Existing clusters with a service principal

AKS clusters with a service principal must first disable monitoring and then upgrade to managed identity. Only Azure public cloud, Microsoft Azure operated by 21Vianet cloud, and Azure Government cloud are currently supported for this migration.

> [!NOTE]
> Minimum Azure CLI version 2.49.0 or higher.

1. Get the configured Log Analytics workspace resource ID:

    ```cli
    az aks show -g <resource-group-name> -n <cluster-name> | grep -i "logAnalyticsWorkspaceResourceID"
    ```

1. Disable monitoring with the following command:

      ```cli
      az aks disable-addons -a monitoring -g <resource-group-name> -n <cluster-name> 
      ```

1. Upgrade cluster to system managed identity with the following command:

      ```cli
      az aks update -g <resource-group-name> -n <cluster-name> --enable-managed-identity
      ```

1. Enable the monitoring add-on with the managed identity authentication option by using the Log Analytics workspace resource ID obtained in step 1:

      ```cli
      az aks enable-addons -a monitoring -g <resource-group-name> -n <cluster-name> --workspace-resource-id <workspace-resource-id>
      ```

### Existing clusters with system or user-assigned identity

AKS clusters with system-assigned identity must first disable monitoring and then upgrade to managed identity. Only Azure public cloud, Azure operated by 21Vianet cloud, and Azure Government cloud are currently supported for clusters with system identity. For clusters with user-assigned identity, only Azure public cloud is supported.

> [!NOTE]
> Minimum Azure CLI version 2.49.0 or higher.

1. Get the configured Log Analytics workspace resource ID:

      ```cli
      az aks show -g <resource-group-name> -n <cluster-name> | grep -i "logAnalyticsWorkspaceResourceID"
      ```

1. Disable monitoring with the following command:

      ```cli
      az aks disable-addons -a monitoring -g <resource-group-name> -n <cluster-name>
      ```

1. Enable the monitoring add-on with the managed identity authentication option by using the Log Analytics workspace resource ID obtained in step 1:

      ```cli
      az aks enable-addons -a monitoring -g <resource-group-name> -n <cluster-name> --workspace-resource-id <workspace-resource-id>
      ```


### Private link Without managed identity authentication
Use the following procedure if you're not using managed identity authentication. This requires a [private AKS cluster](../../aks/private-clusters.md).

1. Create a private AKS cluster following the guidance in [Create a private Azure Kubernetes Service cluster](../../aks/private-clusters.md).

2. Disable public Ingestion on your Log Analytics workspace. 

    Use the following command to disable public ingestion on an existing workspace.

    ```cli
    az monitor log-analytics workspace update --resource-group <azureLogAnalyticsWorkspaceResourceGroup> --workspace-name <azureLogAnalyticsWorkspaceName>  --ingestion-access Disabled
    ```

    Use the following command to create a new workspace with public ingestion disabled.

    ```cli
    az monitor log-analytics workspace create --resource-group <azureLogAnalyticsWorkspaceResourceGroup> --workspace-name <azureLogAnalyticsWorkspaceName>  --ingestion-access Disabled
    ```

3. Configure private link by following the instructions at [Configure your private link](../logs/private-link-configure.md). Set ingestion access to public and then set to private after the private endpoint is created but before monitoring is enabled. The private link resource region must be same as AKS cluster region. 

4. Enable monitoring for the AKS cluster.

    ```cli
    az aks enable-addons -a monitoring --resource-group <AKSClusterResourceGorup> --name <AKSClusterName> --workspace-resource-id <workspace-resource-id>
    ```


## Limitations 
1.	Ingestion Transformations are not supported: See [Data collection transformation](../essentials/data-collection-transformations.md) to read more.    
2.	Dependency on DCR/DCRA for region availability - For new AKS region, there might be chances that DCR is still not supported in the new region. In that case, onboarding Container Insights with MSI will fail. One workaround is to onboard to Container Insights through CLI with the old way (with the use of Container Insights solution)

## Timeline  
Any new clusters being created or being onboarded now default to Managed Identity authentication. However, existing clusters with legacy solution-based authentication are still supported.  

## Next steps
If you experience issues when you upgrade the agent, review the [troubleshooting guide](container-insights-troubleshoot.md) for support.
