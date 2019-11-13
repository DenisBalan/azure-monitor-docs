---
title: Azure Monitor for containers health monitors configuration | Microsoft Docs
description: This article provides content describing the detailed configuration of the health monitors in Azure Monitor for containers. 
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-monitor
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 11/12/2019
ms.author: magoedte
---

# Azure Monitor for containers health monitor configuration guide

## Monitors

A monitor measures the health of some aspect of a managed object. Monitors each have either two or three health states. A monitor will be in one and only one of its potential states at any given time. When a monitor loaded by the containerized agent, it is initialized to a healthy state. The state changes only if the specified conditions for another state are detected.

The overall health of a particular object is determined from the health of each of its monitors. This hierarchy is illustrated in the Health Hierarchy pane in Azure Monitor for containers. The policy for how health is rolled up is part of the configuration of the aggregate monitors.

## Aggregate monitor

Aggregate monitors group multiple monitors to provide a single health aggregated health state. This provides an organization to all of the monitors targeted at a particular class and provides a consolidated health state for specific categories of operation.

### Health rollup policy

Each aggregate monitor defines a health rollup policy which is the logic that is used to determine the health of the aggregate monitor based on the health of the monitors under it. The possible health rollup policies for an aggregate monitor are as follows:

#### Worst state

The state of the aggregate monitor matches the state of the child monitor with the worst health state. This is the most common policy used by aggregate monitors.

#### Best state

The state of the aggregate monitor matches the state of the child monitor with the best health state.

## Unit monitor

A unit monitor measures some aspect of the resource, application, or service. This might be checking a performance counter to determine the performance of the resource, or watch for a log record that indicates an error. 

## Understand the monitoring configuration

Azure Monitor for containers includes a number of key monitoring scenarios that are configured as follows.

|**Monitor name** | **Description** | **Parameter** | **Value** |
|-------------|-------------|---------------|------|
|Node Memory Utilization |This monitor evaluates the memory utilization of a node every minute, using the cadvisor reported data. | ConsecutiveSamplesForStateTransition<br> FailIfGreaterThanPercentage<br> WarnIfGreaterThanPercentage | 3<br> 90<br> 80  | |
|Node CPU Utilization | This monitor checks the CPU utilization of the node every minute, using the cadvisor reported data. | ConsecutiveSamplesForStateTransition<br> FailIfGreaterThanPercentage<br> WarnIfGreaterThanPercentage | 3<br> 90<br> 80  | |
|Node Status | This monitor checks node conditions reported by Kubernetes.<br> Currently the following node conditions are checked: Disk Pressure, Memory Pressure, PID Pressure, Out of Disk, Network unavailable, Ready status for the node.<br> Out of the above conditions, if either *Out of Disk* or *Network Unavailable* is **true**, the monitor changes to **Fail** state.<br> If any other conditions equal **true**, other than the **Ready** status, then the monitor changes to a **Warning** state. | NodeConditionTypeForFailedState | outofdisk,networkunavailable | |
|Container memory utilization | This monitor reports combined health status of the Memory utilization(RSS) of the instances of the container.<br> It performs a simple comparison that compares each sample to a single threshold, and specified by the configuration parameter **ConsecutiveSamplesForStateTransition**. |

