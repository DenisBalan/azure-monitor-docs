---
title: Autoscale flapping
description: "Flapping in Autoscale "
author: EdB-MSFT
ms.author: edbaynash
ms.service: azure-monitor
ms.subservice: autoscale
ms.topic: conceptual
ms.date: 09/15/2022
ms.reviewer: 
---

# Flapping in Autoscale

This article describes flapping in autoscale and how to avoid it.

Flapping is a term that refers to a loop condition causing a series of opposing scale events. Flapping happens when a scale event will result in the opposite scale event being triggered. To avoid the loop condition, autoscale evaluates the effect of a pending scale action to see if it would cause flapping. In cases where flapping could occur, autoscale may scale by less than the specified number of resource instances or, may skip the scale action and reevaluate at the next run. The autoscale evaluation process occurs each time the autoscale engine runs, which is every 30 to 60 seconds, depending on the resource type.

For example, let's assume the following rules:

* Scale out increasing by 1 instance when average CPU usage is above 50%.
* Scale in decreasing the instance count by 1 instance when average CPU usage is lower than 30%.

When usage is at 56% for a single instance, a scale-out action would result in 56% CPU usage across 2 instances, or an average of 28% for the scale set. As 28% is less than the scale-in threshold, autoscale should scale back in. Scaling in would return the scale set to 56% CPU usage, which requires a scale-out. In this situation, the autoscale engine will defer the scale-out event and reevaluate during the next autoscale run. The scale-out will only happen once the average CPU usage is above 60%.

The following scenarios show what can cause, and how to avoid flapping.

## Keep a margin between thresholds

It's important to have an adequate margin between scaling thresholds. For example, the following rules would cause flapping.

* Scale out when thread count >=600
* Scale in when thread count < 600

:::image type="content" source="./media/autoscale-flapping/autoscale-flapping-example2.png" alt-text="A screenshot showing an autoscale default scale condition with rules configured for the example":::

|Time| Instance count| Thread count|Thread count per instance| Scale event| Resulting instance count
|---|---|---|---|---|---|
T0|2|1250|625|Scale out|3|
T1|3|1250|417|Scale in|2|

1. At time T0, there are two instances handling 1250 threads, or 625 treads per instance. Autoscale scales out to three instances.
1. Following the scale-out, at T1, we have the same 1250 threads, but with three instances, only 417 threads per instance. A scale-in event should be triggered.
1. Before scaling-in, autoscale estimates the final state, if it scaled in. In this example, 1250 / 2 = 625 threads per instance. This means autoscale would have to immediately scale out again after it scaled in, however, if it scaled up again, the whole process would repeat, leading to flapping loop.
1. To avoid this situation, autoscale doesn't scale in. Instead, it skips the current scale event and reevaluates the condition in the next execution cycle.

In this case, it would appear that autoscale isn't working as no scale event would take place.

Setting an adequate margin between thresholds solves the above condition. For example,

* Scale out when thread count >=600
* Scale in when thread count < 400

:::image type="content" source="./media/autoscale-flapping/autoscale-flapping-example3.png" alt-text="A screenshot showing  autoscale rules configured for the example":::

If the scale-in thread count is 400, the total thread count would have to drop to below 1200 before a scale event would take place. See the table below.

|Time| Instance count| Thread count|Thread count per instance| Scale event| Resulting instance count
|---|---|---|---|---|---|
T0|2|1250|625|Scale out|3|
T1|3|1250|417|no scale event|3|
T2|3|1180|394|scale in|2|
T3|3|1180|590|no scale event|2|

Estimation during a scale-in is intended to avoid "flapping" situations, where scale-in and scale-out actions continually go back and forth. Keep this behavior in mind when setting thresholds for scaling out and in.

## Scaling by more than one instance

To avoid flapping, when scaling in or out by more than one instance, autoscale may scale by less than the number of instances specified in the rule.

For example, given the following rules:

* Scale out by 20 when the request count >=200 per instance.
* OR when CPU > 70% per instance.
* Scale in by 10 when the request count <=50 per instance.

:::image type="content" source="./media/autoscale-flapping/autoscale-flapping-example1.png" alt-text="A screenshot showing an autoscale default scale condition with rules configured for the example":::

|Time|Number of instances|CPU |Request count| Scale event| Resulting instances|Comments|
|---|---|---|---|---|---|---|
|T0|30|65%|3000, or 100 per instance.|No scale event|30|
|T1|30|65|1500| Scale in by 3 instances |27|Scaling-in by 10 would cause an estimated CPU rise above 70%, leading to a scale-out event.

At time T0, the app is running with 30 instances, a total request count of 3000, and a CPU usage of 65% per instance.

At T1, when the request count drops to 1500 requests, or 50 requests per instance, autoscale will try to scale in by 10 instances to 20. However, autoscale estimates that the CPU load for 20 instances will be above 70%, causing a scale-out event.

To avoid flapping, the autoscale engine will estimate the CPU usage for instance counts above 20, until it finds an instance count that satisfies all of the rules:

  1. Keep the CPU below 70%.
  1. Reduce the number of instances below 30.
  1. Keep the number of requests per instance is above 50.

In this situation, autoscale may scale in by 3, from 30 to 27 instances in order to satisfy both rules, even though the rule specifies a decrease of 10.

If autoscale can't find a suitable number of instances, it will skip the scale in event and reevaluate during the next cycle.

> [!NOTE]
> If the autoscale engine detects that flapping could occur as a result of scaling to the target number of instances, it will also try to scale to a lower number of instances between the current count and the target count. If flapping does not occur within this range, autoscale will continue the scale operation with the new target.
