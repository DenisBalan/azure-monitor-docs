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
ms.date: 10/14/2019
ms.author: magoedte
---

# Understand AKS cluster health with Azure Monitor for containers

With Azure Monitor for containers, it monitors and reports health status of the managed infrastructure components, all nodes, and workloads running on a cluster deployed on Azure Kubernetes Service (AKS) and AKS Engine running on-premises and on Azure Stack. This experience extends beyond the cluster health status calculated and reported on the [multi-cluster view](container-insights-analyze.md#multi-cluster-view-from-azure-monitor), where now you can understand if one or more nodes in the cluster are resource constrained, or a node or pod are unavailable that could impact a running application in the cluster based on curated metrics. 

For information about how to enable Azure Monitor for containers, see [Onboard Azure Monitor for containers](container-insights-onboard.md).

## Overview

In Azure Monitor for containers, the Health feature provides proactive health monitoring of your Kubernetes cluster to help you identify and diagnose issues. It gives you the ability to view these significant issues detected, and define [alerts](../platform/alerts-unified-log.md) to notify you when these state changes occur. 

Kubernetes cluster health is cateogrized by the following Kubernetes objects and abstractions :

- Kubernetes infrastructure - provides a rollup of the Kubernetes API server, ReplicaSets, and DaemonSets running on nodes deployed in your cluster.

    ![Kubernetes infrastructure health rollup view](./media/container-insights-health/health-view-kube-infra-01.png)

- Nodes - provides a rollup of the node pools and state of individual nodes in each pool, by evaluating CPU and memory utilization, and a nodes availability.

    ![Nodes health rollup view](./media/container-insights-health/health-view-nodes-01.png)

- Workloads - provides a rollup of overall utilization of the cluster and for each containerized application or other workload running across all namespaces in the cluster.

    ![Workloads health rollup view](./media/container-insights-health/health-view-workloads-01.png)

All monitors are shown in a hierarchical layout, where an aggregate monitor represents the Kubernetes object or abstraction (Kubernetes infrastructure, Node, Workload) and reflects combined health of all dependent monitors based on their current health state. 

The icons used to indicate state are as follows:

|Icon|Meaning|  
|--------|-----------|  
|![Green check icon indicates healthy](./media/container-insights-health/healthyicon.png)|Success, health is OK (green)|  
|![Yellow triangle and exclamation mark is warning](./media/container-insights-health/warningicon.png)|Warning (yellow)|  
|![Red button with white X indicates critical state](./media/container-insights-health/criticalicon.png)|Critical (red)|  
|![Grayed-out icon](./media/container-insights-health/grayicon.png)|Out of contact (gray)|  




## Monitor configuration (put into its own article)

Describe the behavior and configuration of each monitor (describe difference between unit monitor and aggregate monitor, the config of each

## Sign in to the Azure portal

Sign in to the [Azure portal](https://portal.azure.com). 

## View health from an AKS cluster

Access to the Azure Monitor for containers health feature is available directly from an AKS cluster by selecting **Insights** from the left pane in the Azure portal. From the **Cluster** performance tab, select **Health**.  

![Azure Monitor health dashboard example](./media/container-insights-health/health-view-01.png)



Selecting 

## View health from non-AKS clusters

Go to the multi-cluster view and then select the non-AKS cluster from the list to drill-down and view health for that specific cluster.

## Alerting

## Next steps