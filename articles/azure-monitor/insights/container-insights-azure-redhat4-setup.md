---
title: Configure Azure Red Hat OpenShift v4.x with Azure Monitor for containers | Microsoft Docs
description: This article describes how to configure monitoring of a Kubernetes cluster with Azure Monitor hosted on Azure Red Hat OpenShift version 4 and higher.
ms.topic: conceptual
ms.date: 04/16/2020
---

# Configure Azure Red Hat OpenShift v4.x with Azure Monitor for containers

Azure Monitor for containers provides rich monitoring experience for the Azure Kubernetes Service (AKS) and AKS Engine clusters. This article describes how to enable monitoring of Kubernetes clusters hosted on [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) version 4.x to achieve a similar monitoring experience.

>[!NOTE]
>Support for Azure Red Hat OpenShift is a feature in public preview at this time.
>

Azure Monitor for containers can be enabled for one or more existing deployments of Azure Red Hat OpenShift using the following supported methods:

- For an existing cluster using the provided bash script and running in the [Azure CLI](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create).

## Supported and unsupported features

Azure Monitor for containers supports monitoring Azure Red Hat OpenShift as described in the [Overview](container-insights-overview.md) article, except for the following features:

- Live Data (preview)
- [Collect metrics](container-insights-update-metrics.md) from cluster nodes and pods and storing them in the Azure Monitor metrics database

## Prerequisites

- [Helm 3](https://helm.sh/docs/intro/install/) CLI tool

- [Bash version 4](https://www.gnu.org/software/bash/)

- To enable and access the features in Azure Monitor for containers, at a minimum you need to be a member of the Azure *Contributor* role in the Azure subscription, and a member of the [*Log Analytics Contributor*](../platform/manage-access.md#manage-access-using-azure-permissions) role of the Log Analytics workspace configured with Azure Monitor for containers.

- To view the monitoring data, you are a member of the [*Log Analytics reader*](../platform/manage-access.md#manage-access-using-azure-permissions) role permission with the Log Analytics workspace configured with Azure Monitor for containers.

## Enable for an existing cluster

Perform the following steps to enable monitoring of an Azure Red Hat OpenShift version 4 and higher cluster deployed in Azure using the provided bash script.

1. Sign into Azure

    ```azurecli
    az login
    ```

2. Download and save the script to a local folder that configures your cluster with the monitoring add-on using the following commands:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aroV4/onboarding_azuremonitor_for_containers.sh.`


3. The following step enables monitoring on the specified cluster by using Azure CLI. Specify your Log Analytics workspace resource ID and context of Kubernetes cluster. Before running the script, first identify the full resource ID of your Log Analytics workspace required for the `workspaceResourceId` parameter value.

    1. List all the subscriptions that you have access to using the following command:

        ```azurecli
        az account list --all -o table
        ```

        The output will resemble the following:

        ```azurecli
        Name                                  CloudName    SubscriptionId                        State    IsDefault
        ------------------------------------  -----------  ------------------------------------  -------  -----------
        Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
        ```

        Copy the value for **SubscriptionId**.

    2. Switch to the subscription hosting the Log Analytics workspace using the following command:

        ```azurecli
        az account set -s <subscriptionId of the workspace>
        ```

    3. The following example displays the list of workspaces in your subscriptions in the default JSON format.

        ```
        az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
        ```

        In the output, find the workspace name, and then copy the full resource ID of that Log Analytics workspace under the field **ID**.

    4. To identify the **kube-context** of your cluster, after successful `oc login` on to your cluster, run the command `kubectl config current-context` and copy the value.

4. To deploy with Azure CLI, run the following command:

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId> <LogAnalyticsWorkspaceId>`

    For example:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4 /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourcegroups/test-la-workspace-rg/providers/microsoft.operationalinsights/workspaces/test-la-workspace`

After you've enabled monitoring, it might take about 15 minutes before you can view health metrics for the cluster.

### From the Azure portal

1. Sign in to the [Azure portal](https://portal.azure.com).

2. On the Azure portal menu or from the Home page, select **Azure Monitor**. Under the **Insights** section, select **Containers**.

3. On the **Monitor - containers** page, select **Non-monitored clusters**.

4. From the list of non-monitored clusters, find the cluster in the list and click **Enable**. You can identify the results in the list by looking for the value **ARO** under the column **CLUSTER TYPE**.

## Next steps

- With monitoring enabled to collect health and resource utilization of your RedHat OpenShift version 4.x cluster and workloads running on them, learn [how to use](container-insights-analyze.md) Azure Monitor for containers.

- By default, the containerized agent collects the stdout/ stderr container logs of all the containers running in all the namespaces except kube-system. To configure container log collection specific to particular namespace or namespaces, review [Container Insights agent configuration](container-insights-agent-config.md) to configure desired data collection settings to your ConfigMap configurations file.

- To scrape and analyze Prometheus metrics from your cluster, review [Configure Prometheus metrics scraping](container-insights-prometheus-integration.md)

- To learn how to stop monitoring your cluster with Azure Monitor for containers, see [How to Stop Monitoring Your Azure Red Hat OpenShift cluster](container-insights-optout-openshift.md).
