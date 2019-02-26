---
title: Getting started with Azure Metrics Explorer
description: Learn how to create your first metric chart with Azure Metrics Explorer.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
---

# Getting started with Azure Metrics Explorer

> [!NOTE] 
> This topic covers key concepts to help new users to get started with Azure Metrics Explorer. Use [this link](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts) if you are looking for detailed documentation and information about advanced chart settings and metrics.

## Where do I start?
Use **Azure Metrics Explorer** to investigate health and utilization of your resources. Start in the following order:
* Start by [picking a resource and a metric](#Creating-your-first-metric-chart) and you will see a basic chart. Then [select a time range](#Picking-time-range) that is relevant for your investigation.

* After learning basic charts, you may want to try [applying dimension filters and splitting](#Applying-dimension-filters-and-splitting). This allows analyzing which segments of the metric contribute to the overall metric value and identify possible outliers.

* Use [advanced settings](#Advanced-chart-settings-and-next-steps) to customize the chart before pinning to  dashboards. [Configure alerts](alerts-metric-overview.md) to receive notifications when the metric value exceeds or drops below a threshold.

## Creating your first metric chart

To create a metric chart, from your resource, resource group, subscription, or Azure Monitor view, open **Metrics** tab and follow these steps:

1. Using a **resource picker**, select your resource for which you want to see metrics. (The resource is pre-selected if you opened the **Metrics** in the context of a specific resource):

    > ![Select a resource](./media/metrics-getting-started/resource-picker.png)

2. For some resources, you must pick a namespace. The namespace is just a way to organize metrics so that you can easily find them. For example, storage accounts have separate namespaces for storing Files, Tables, Blobs, and Queues metrics. Many resource types only have one namespace.

3. Select a metric from a list of available metrics:

    > ![Select a metric](./media/metrics-getting-started/metric-picker.png)

4. You can optionally change metric aggregation. For example, you might want your chart to show minimum, maximum or average values of the metric.

> [!NOTE] 
> Use the **Add Metric** button and repeat these steps if you want to see multiple metrics plotted in the same chart. For multiple charts in one view, use the **Add Chart** button on top.

## Picking time range

By default, the chart shows the most recent 24 hours of metrics data. Use the **Time Picker** panel to change the time range, or to zoom in and zoom out your chart:

![Change time range panel](./media/metrics-getting-started/time-picker.png)

## Applying dimension filters and splitting

[Filtering](metrics-charts.md#Apply-filters-to-charts) and [splitting](metrics-charts.md#Segment-a-chart) are powerful diagnostic tools for the metrics that have dimensions. They bring visibility into how various metric segments (“dimension values”) impact the overall value of the metric, and allow to identify possible outliers.

* Filtering lets you choose which dimension values are included in the chart. For example, you might want to only account for successful requests when charting the “Server response time” metric. The “success of request” is a dimension on which you would need to apply the filter. 

* Splitting controls whether the chart displays separate lines for each value of a dimension, or aggregates the values into a single line. For example, you can see one line for an average response time across all server instances, or see separate lines for each server. The “server instance” is a dimension on which you would need to apply splitting.

See [examples of the charts](metric-chart-samples.md) that have filtering and splitting applied. You can find which steps were used to configure each of these charts.

## Advanced chart settings and next steps

You can customize chart style, title, and modify advanced chart settings. When done with customization, pin it to a dashboard to save your work. You can also configure metrics alerts. Follow [product documentation](metrics-charts.md) to learn about these and other advanced features of Azure Monitor metrics explorer.

## Next steps

* [See a list of available metrics for Azure services](metrics-supported.md)
* [Learn about advanced features of Metric Explorer](metrics-charts.md)
* [See examples of configured charts](metric-chart-samples.md)