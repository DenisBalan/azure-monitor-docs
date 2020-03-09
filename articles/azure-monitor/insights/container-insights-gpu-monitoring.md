---
title: Configure GPU monitoring with Azure Monitor for containers | Microsoft Docs
description: This article describes how you can configure monitoring Kubernetes clusters with NVIDIA and AMD GPU enabled nodes with Azure Monitor for containers.
ms.topic: conceptual
ms.date: 03/09/2020
---

# Configure GPU monitoring with Azure Monitor for containers

Starting with agent version *ciprod03022019*, Azure monitor for containers integrated agent now supports monitoring GPU (graphical processing units) usage on GPU-aware Kubernetes cluster nodes, and monitor pods/containers requesting and using GPU resources.

## Supported GPU vendors

Azure Monitor for Containers supports monitoring GPU clusters from following GPU vendors:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Azure Monitor for containers automatically starts monitoring GPU usage on nodes, and GPU requesting pods and workloads by collecting the following metrics at 60sec intervals and storing them in the **InsightMetrics** table:

|Metric name |Metric dimension (tags) |Description |
|------------|------------------------|------------|
|containerGpuDutyCycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor|Percentage of time over the past sample period (60 seconds) during which GPU was busy/actively processing for a container. Duty cycle is a number between 1 and 100. |
|containerGpuLimits |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Each container can specify limits as one or more GPUs. It is not possible to request or limit a fraction of a GPU. |
|containerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Each container can request one or more GPUs. It is not possible to request or limit a fraction of a GPU.|
|containerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Amount of GPU Memory in bytes available to use for a specific container. |
|containerGpumemoryUsedBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Amount of GPU Memory in bytes used by a specific container. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Number of GPUs in a node that can be used by Kubernetes. |
|nodeGpuCapacity |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Total Number of GPUs in a node. |

## GPU performance charts 

Azure Monitor for containers includes pre-configured charts for the metrics listed earlier in the table as a GPU workbook for every cluster. You can find the GPU workbook directly from an AKS cluster by selecting **Workbooks** from the left-hand pane, and from the **View Workbooks** drop-down list in the Insight.

## Next steps

Use GPUs for compute-intensive workloads on Azure Kubernetes Service (AKS) 

GPU Optimized VM SKUs in Microsoft Azure 

GPU support in Kubernetes  