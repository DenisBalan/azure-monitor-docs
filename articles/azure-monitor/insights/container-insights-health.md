---
title: Monitor AKS cluster health with Azure Monitor for containers | Microsoft Docs
description: This article describes how you can view and analyze the health of your AKS clusters with Azure Monitor for containers.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-monitor
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 11/11/2019
ms.author: magoedte
---

# Understand AKS cluster health with Azure Monitor for containers

With Azure Monitor for containers, it monitors and reports health status of the managed infrastructure components, all nodes, and workloads running on a cluster deployed on Azure Kubernetes Service (AKS) and AKS Engine running on-premises and on Azure Stack. This experience extends beyond the cluster health status calculated and reported on the [multi-cluster view](container-insights-analyze.md#multi-cluster-view-from-azure-monitor), where now you can understand if one or more nodes in the cluster are resource constrained, or a node or pod are unavailable that could impact a running application in the cluster based on curated metrics. 

For information about how to enable Azure Monitor for containers, see [Onboard Azure Monitor for containers](container-insights-onboard.md).

## Overview

In Azure Monitor for containers, the Health feature provides proactive health monitoring of your Kubernetes cluster to help you identify and diagnose issues. It gives you the ability to view significant issues detected. 

Kubernetes cluster health is based on a number of monitoring scenarios organized by the following Kubernetes objects and abstractions:

- Kubernetes infrastructure - provides a rollup of the Kubernetes API server, ReplicaSets, and DaemonSets running on nodes deployed in your cluster by evaluating CPU and memory utilization, and a Pods availability

    ![Kubernetes infrastructure health rollup view](./media/container-insights-health/health-view-kube-infra-01.png)

- Nodes - provides a rollup of the Node pools and state of individual Nodes in each pool, by evaluating CPU and memory utilization, and a Nodes availability.

    ![Nodes health rollup view](./media/container-insights-health/health-view-nodes-01.png)

All monitors are shown in a hierarchical layout, where an aggregate monitor representing the Kubernetes object or abstraction (that is, Kubernetes infrastructure or Nodes) are the top-most monitor reflecting the combined health of all dependent child monitors. The key monitoring scenarios used to derive health are:

* Evaluate CPU utilization from the node and container.
* Evaluate memory utilization from the node and container.
* Status of Pods and Nodes based on calculation of their ready state reported by Kubernetes.

The icons used to indicate state are as follows:

|Icon|Meaning|  
|--------|-----------|  
|![Green check icon indicates healthy](./media/container-insights-health/healthyicon.png)|Success, health is OK (green)|  
|![Yellow triangle and exclamation mark is warning](./media/container-insights-health/warningicon.png)|Warning (yellow)|  
|![Red button with white X indicates critical state](./media/container-insights-health/criticalicon.png)|Critical (red)|  
|![Grayed-out icon](./media/container-insights-health/grayicon.png)|Unknown (gray)|  

## Monitor configuration (put into its own article)

To understand the behavior and configuration of each monitor supporting Azure Monitor for containers Health feature, see [article name].

## Sign in to the Azure portal

Sign in to the [Azure portal](https://portal.azure.com). 

## View health of an AKS or non-AKS cluster

Access to the Azure Monitor for containers Health feature is available directly from an AKS cluster by selecting **Insights** from the left pane in the Azure portal. Under the **Insights** section, select **Containers**. From the **Cluster** page, select **Health**.

![Cluster health dashboard example](./media/container-insights-health/health-view-01.png)

To view health from a non-AKS cluster, that is an AKS Engine cluster hosted on-premises or on Azure Stack, select **Azure Monitor** from the left pane in the Azure portal. Under the **Insights** section, select **Containers**.  On the multi-cluster page, select the non-AKS cluster from the list to drill-down and view health for that specific cluster.  

## Review cluster health

When the Health page opens, by default **Kubernetes Infrastructure** is selected in the **Health Aspect** grid.  The grid summarizes current health rollup state of Kubernetes infrastructure and cluster nodes. Selecting either health aspect updates the results in the middle-pane and shows all child monitors in a hierarchical layout, displaying their current health state. To view more information about any dependent monitor, you can select one and a property pane automatically displays on the right side of the page. 

![Cluster health property pane](./media/container-insights-health/health-view-property-pane.png)

On the property pane, you can learn the following:

- On the **Overview** tab, it shows the last state change history over the last three hours.
- On the**Config** tab, it shows the default configuration parameter settings (only for unit monitors).
- On the **Knowledge** tab, it contains information explaining the behavior of the monitor and how it evaluates for the unhealthy condition.


## Next steps