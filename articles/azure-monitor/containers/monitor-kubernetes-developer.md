---
title: Monitor Kubernetes clusters using Azure services and cloud native tools - developer
description: Describes how to monitor the health and performance of the layers of your Kubernetes environment managed by the developer using Azure Monitor and cloud native services in Azure.
ms.service:  azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/07/2023
---

# Monitor Kubernetes environment - developer
This article is part of the guide [Monitor Kubernetes clusters using Azure services and cloud native tools](monitor-kubernetes.md). It describes the role of the developer, including choice and configuration of monitoring tools and how to use those tools for common monitoring scenarios.

In addition to developing the application, the developer maintains the application running on the cluster. They're responsible for application specific traffic including application performance and failures and maintain reliability of the application according to company-defined SLAs.


## Azure services

Azure provides a complete set of services for monitoring the different layers of your Kubernetes infrastructure and the applications that depend on it. These services work in conjunction with each other to provide a complete monitoring solution, or you may integrate them with your existing monitoring tools. The following table lists the services that are commonly used by the network engineer to monitor the health and performance of the Kubernetes cluster and its components.  

| Service | Description |
|:---|:---|
| [Application insights](../app/app-insights-overview.md) |  Feature of Azure Monitor that provides application performance monitoring (APM) to monitor applications running on your Kubernetes cluster from development, through test, and into production. Quickly identify and mitigate latency and reliability issues using distributed traces. Supports [OpenTelemetry](../app/opentelemetry-overview.md#opentelemetry) for vendor-neutral instrumentation. |

## Enable Application insights to monitor your application

See [Data Collection Basics of Azure Monitor Application Insights](../app/opentelemetry-overview.md) for options on configuring data collection from the application running ion your cluster and decision criteria on the best method for your particular requirements. Once you have your application instrumented, create an [Availability test](../app/availability-overview.md) to create a recurring test to monitor its availability and responsiveness.


## Additional scenarios

**How do I get started with Application insights?**

- See [Data Collection Basics of Azure Monitor Application Insights](../app/opentelemetry-overview.md) for options on configuring data collection from your application and decision criteria on the best method for your particular requirements.

**Where are my application logs stored?**

- Container insights sends stdout/stderr logs to a Log Analytics workspace. See [Resource logs](../../aks/monitor-aks-reference.md#resource-logs) for a description of the different logs and [Kubernetes Services](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/tables-resourcetype#kubernetes-services) for a list of the tables each is sent to.
- App insights for additional logging including iLogger.

**How do I identify poor performing apis or database queries?**

- Use [Profiler](../profiler/profiler-overview.md) to capture and view performance traces for your application.

**How do I identify my application bottleneck?**

- Use [Application Map](../app/app-map.md) to view the dependencies between your application components and identify any bottlenecks.
- Use [Profiler](../profiler/profiler-overview.md) to capture and view performance traces for your application.
- Use the **Performance** view in Application insights to view the performance of different operations in your application.

**How do I determine if my application is meeting its service-level agreement (SLA)?**

- Use the [SLA report](../app/sla-report.md) to calculate and report SLA for web tests.
- Use [annotations](../app/annotations.md) to identify when a new build is deployed so that you can visually inspect any change in performance after the update.

**How do I determine if my application is generating errors?**

- See the **Failures** tab of Application insights to view the number of failed requests and the most common exceptions.
- Ensure that alerts for [failure anomalies](../alerts/proactive-failure-diagnostics.md) identified with [smart detection](../alerts/proactive-diagnostics.md) are configured properly.


**How can I to gain better observability into the interaction between services?**

- Enable [distributed tracing](../app/distributed-tracing-telemetry-correlation.md), which provides a performance profiler that works like call stacks for cloud and microservices architectures.

**How should I set up health monitoring for my application?**

- Create an [Availability test](../app/availability-overview.md) in Application insights to create a recurring test to monitor the availability and responsiveness of your application.



## Next steps

- See [Monitor Kubernetes clusters with Azure services](monitor-kubernetes-analyze.md) for details on how to use the tools described in this article to monitor your Kubernetes clusters.

