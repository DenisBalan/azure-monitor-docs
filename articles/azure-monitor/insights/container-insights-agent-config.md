---
title: Configure Azure Monitor for containers agent data collection | Microsoft Docs
description: This article describes how you can configure the Azure Monitor for containers agent to control stdout/stderr and environment variables log collection.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/17/2019
ms.author: magoedte
---

# Configure agent data collection for Azure Monitor for containers

Azure Monitor for containers collects stdout, stderr, and environmental variables from container workloads deployed to managed Kubernetes clusters hosted on Azure Kubernetes Service (AKS) from the containerized agent. You can configure agent data collection settings by creating a custom Kubernetes ConfigMaps to control this experience. This article demonstrates how to create ConfigMaps and configure data collection based on your requirements.

## Configure your cluster with custom data collection settings

A sample ConfigMap file is provided that allows you to easily edit it with your customizations without having to create it from scratch. Before starting, you should review the Kubernetes documentation about [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) and familiarize yourself with how to create, configure, and deploy ConfigMaps. This will allow you to filter stderr and stdout per namespace or across the entire cluster, and environment variables for any container running across all pods/nodes in the cluster.  

### Overview of configurable data collection settings 

The following are the settings that can be configured to control data collection.

|Key |Data type |Value |Description |
|----|-----------|------|------------|
|`schema-version` |String (case sensitive) |v1 |This is the schema version used by the agent when parsing this configmap. Currently supported schema-version is v1. Modifying this value is not supported and will be rejected when ConfigMaps is evaluated.|
| `config-version` |String |Supports ability to keep track of this config file's version in your source control system/repository. Maximum allowed characters is 10, and all other characters are truncated. |
| [log_collection_settings.stdout] | | |
| |Boolean | `enabled=true or false` | This controls if stdout container log collection is enabled. When set to `true` and no namespaces are excluded for stdout log collection (`log_collection_settings.stdout.exclude_namespaces` setting below), stdout logs will be collected from all containers across all pods/nodes in the cluster. If not specified in ConfigMaps, the default value is `enabled = true`. |
| |String, comma separated array | `exclude_namespaces` |Array of kubernetes namespaces for which stdout logs will not be collected. This setting is effective only if `log_collection_settings.stdout.enabled` is set to `true`. If not specified in ConfigMaps, the default value is `exclude_namespaces = ["kube-system"]`.|
| [log_collection_settings.stderr] | | |
| |Boolean | `enabled=true or false` |This controls if stderr container log collection is enabled. When set to `true` and no namespaces are excluded for stdout log collection (`log_collection_settings.stderr.exclude_namespaces` setting), stderr logs will be collected from all containers across all pods/nodes in the cluster. If not specified in ConfigMaps, the default value is `enabled = true`. |
| |String, comma separated array |Array of kubernetes namespaces for which stderr logs will not be collected. This setting is effective only if `log_collection_settings.stdout.enabled` is set to `true`. If not specified in ConfigMaps, the default value is `exclude_namespaces = ["kube-system"]`. |
| [log_collection_settings.env_var] |Boolean | `enabled=true or false` | This controls if environment variable collection is enabled. When set to `false`, no environment variables are collected for any container running across all pods/nodes in the cluster. If not specified in ConfigMaps, the default value is `enabled = true`. |

### Configure and deploy ConfigMaps

Perform the following steps to configure and deploy your ConfigMaps configuration file to your cluster.

1. [Download](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) the sample ConfigMaps yaml file and save it and save it as container-azm-ms-agentconfig.yaml.  
1. Edit the ConfigMaps yaml file with your customizations.  
1. Create ConfigMaps by running the following kubectl command: `kubectl apply -f <configmap_yaml_file>`.
    
    Example: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    The configuration change can take a few minutes to finish before taking effect, and all omsagent pods in the cluster will restart. When it's finished, a message is displayed that's similar to the following and includes the result: `configmap "container-azm-ms-agentconfig" created`

