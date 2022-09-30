# Private preview – Container Insights Extension support for LCM clusters

## Overview

This private preview supports onboarding the Container Insights extension onto provisioned cluster.

Provisioning K8s clusters into environments is complex and error prone for many customers leading to multiple customer asking for simplifying provisioning (day 0) and maintaining (day1) clusters, namely 'k8s cluster lifecycle management' (LCM).

For Arc Agent Onboarding Provisioned Clusters details:

[Project Haiku - Cluster Lifecycle Management - All Documents (sharepoint.com)](https://microsoft.sharepoint.com/teams/ProjectHaiku/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2FProjectHaiku%2FShared%20Documents%2FCluster%20Lifecycle%20Management&FolderCTID=0x012000FF58C9350911CA4EBC5A6983E11990B6)

Provisioned cluster operations:

[azure-arc-kubernetes-preview/2. Create and access AKS-HCI clusters.md at master · Azure/azure-arc-kubernetes-preview (github.com)](https://github.com/Azure/azure-arc-kubernetes-preview/blob/master/docs/aks-hci/2.%20Create%20and%20access%20AKS-HCI%20clusters.md)

CLI examples:

- az hybridaks create –name
- az hybridaks show
- az hybridaks nodepool add

## Pre-requisites

- azure-cli: 2.39.0+

- azure-cli-core: 2.39.0+

- k8s-extension: 1.3.2+

- hybridaks (for provisioned cluster operation, optional):0.1.3+

- Resource-graph: 2.1.0+

## Onboarding

For testing provisioned cluster with onboarding extensions:

Sample environment setup:

```azcli 
export k8sClusterName="akscluster0915"

export resourceGroup="provCluGrpDs01"

export location="eastus"

export targetNamespace="arcextensions"

export clusterRP="microsoft.hybridcontainerservice"

export clusterType="provisionedclusters"


$k8sClusterName="sampleClusterName"

$resourceGroup="sampleResourceGroup"

$location="sampleLocation"

$targetNamespace="arcextensions"

$clusterRP="microsoft.hybridcontainerservice"

$clusterType="provisionedclusters"
```


## Extension Onboarding with ARM Template using Managed Identity Auth

1.1 Download the Azure Resource Manager Template and Parameter files

```bash
curl -L [https://github.com/microsoft/Docker-Provider/tree/longw/lcm-private-preview/scripts/onboarding/templates/arc-k8s-extension-provisionedcluster-msi-auth](https://github.com/microsoft/Docker-Provider/tree/longw/lcm-private-preview/scripts/onboarding/templates/arc-k8s-extension-provisionedcluster-msi-auth) -o existingClusterOnboarding.json
```

```bash
curl -L [https://github.com/microsoft/Docker-Provider/tree/longw/lcm-private-preview/scripts/onboarding/templates/arc-k8s-extension-provisionedcluster-msi-auth](https://github.com/microsoft/Docker-Provider/tree/longw/lcm-private-preview/scripts/onboarding/templates/arc-k8s-extension-provisionedcluster-msi-auth) -o existingClusterParam.json
```

1.2 Edit the values in the parameter file.

  - For clusterResourceIdand clusterRegion, use the values on the Overview page for the LCM cluster
  - For workspaceResourceId, use the resource ID of your Log Analytics workspace
  - For workspaceRegion, use the Location of your Log Analytics workspace
  - For workspaceDomain, use the domain of your Log Analytics workspace, this is optional
  - For resourceTagValues, leave as empty if not specific

1.3 Deploy the ARM template

```acli
az login

az account set --subscription "Cluster Subscription Name"

az deployment group create --resource-group $resourceGroup --template-file ./existingClusterOnboarding.json --parameters existingClusterParam.json
```

## Extension Onboarding with CLI using Managed Identity Auth

```acli
az login

az account set --subscription "Cluster Subscription Name"

az k8s-extension create --name azuremonitor-containers --cluster-name $k8sClusterName --resource-group $resourceGroup --cluster-type provisionedclusters --cluster-resource-provider microsoft.hybridcontainerservice --extension-type Microsoft.AzureMonitor.Containers --configuration-settings omsagent.useAADAuth=true
```

## Validation

### Extension details

Showing the extension details:

```azcli
az k8s-extension list --cluster-name $k8sClusterName --resource-group $resourceGroup --cluster-type $clusterType --cluster-resource-provider $clusterRP
```

### Extension status

Querying the Azure Resource graph:

```KQL
Resources | where type =~ 'Microsoft.HybridContainerService/provisionedClusters'
| project clusterResourceId = id, name, location, arcproperties = parse\_json(tolower(properties))
| project clusterResourceId, name, location, kubernetesVersion = tostring(arcproperties.kubernetesversion)
| join kind=leftouter(KubernetesConfigurationResources
| where type =~'Microsoft.KubernetesConfiguration/extensions'
| project extensionResourceId = id, extensionProperties = parse\_json(tolower(properties)), subscriptionId, resourceGroup | extend clusterResourceId = '\<your cluster resource id\>' ) on clusterResourceId
```

### Log Analytics data flow

Validate all the Container Insights data types are getting ingested to your configured Azure Log analytics

### Sample Query for checking in Azure Log analytics

```KQL
let SearchValue = "";//Please update term you would like to find in the table.
KubePodInventory
| where \* contains tostring(SearchValue)
| take 1000
Table name can be: ContainerInventory, ContainerLog, ContainerNodeInventory, InsightsMetrics, KubeEvents, KubeMonAgentEvents, KubeNodeInventory, KubePodInventory, KubeServices
```

### Delete extension

The command for deleting the extension:

```azcli
az k8s-extension delete --cluster-name $k8sClusterName --resource-group $resourceGroup --cluster-type $clusterType --cluster-resource-provider $clusterRP --name azuremonitor-containers --yes
```

## Known Issues/Limitations

- Currently we do not support the onboarding & insights (single and multi-cluster) experience through azure portal, this capability will be available at public preview.
- The metrics session does not support the provisioned resource type, the hybridaks team is working to onboard the provisioned cluster resource type to metrics
