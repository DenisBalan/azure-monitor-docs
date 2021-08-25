---
title: Azure Monitor OpenTelemetry for Python | Microsoft Docs
description: Provides guidance on how to enable Azure Monitor on Python Applications using OpenTelemetry
ms.topic: conceptual
ms.date: 08/24/2021
author: mattmccleary
ms.author: mmcc
---

# Azure Monitor OpenTelemetry Exporter for Python Applications **(Preview)**

This article describes how to enable and configure the OpenTelemetry-based Azure Monitor Preview offering. When you complete the instructions in this article, you’ll be able to use Azure Monitor Application Insights to monitor your application.

> [!WARNING]
> Please consider carefully whether this preview is right for you. It enables distributed tracing only and excludes:
> - custom metrics
> - [Pre-aggregated metrics](pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics)
> - [Live Metrics](live-stream.md)
> - logging (e.g., console logs)
> - [Profiler](profiler-overview.md)
> - [Snapshot Debugger](snapshot-debugger.md)
> - offline disk storage
> - [AAD Authentication](azure-ad-authentication.md)
> - Azure Monitor compatible sampling
>
> Those who require a full-feature experience should use the existing Application Insights SDKs until the OpenTelemetry-based offering matures.

> [!WARNING]
> Enabling sampling will result in broken traces if used alongside the existing Application Insights SDKs, and it will make standard and log-based metrics extremely inaccurate which will adversely impact all Application Insights experiences.

## Get Started
### Prerequisites
- Python Application using version 3.6+
- Azure Subscription (Free to [create](https://azure.microsoft.com/free/))
- Application Insights Resource (Free to [create](create-workspace-resource.md#create-workspace-based-resource))

### Enable Azure Monitor Application Insights
**1. Install package**

Add code to xyz.file in your application

```python
Placeholder
```

**2. Add connection string**

Replace placeholder connection string with YOUR connection string.

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights Connection String":::

Find the connection string on your Application Insights Resource.

//screenshot

**3. Confirm Data is Flowing**

Generate requests in your application and open your Application Insights Resource.

> [!NOTE]
> It may take a couple minutes for data to show up in the Portal.

//screenshot

## Enable OTLP Exporter
You may want to enable the OTLP Exporter alongside your Azure Monitor Exporter to send your telemetry to two locations.

Add the code to xyz.file in your application.

```python
Placeholder
```

> [!NOTE]
> OTLP exporter is shown for convenience only. We do not officially support the OTLP Exporter or any components or third-party experiences downstream of it. We suggest you open an issue with the OpenTelemetry community for OpenTelemetry issues outside the Azure Support Boundary.

## Sampling
OpenTelemetry SDKs provide built-in sampling as a way to control data volume and ingestion costs. To learn how to enable built-in sampling, see [OpenTelemetry Python SDK on trace sampling](https://opentelemetry-python.readthedocs.io/en/latest/sdk/trace.sampling.html).

> [!WARNING]
> We do not recommend enabling sampling in the preview release because it will result in broken traces if used alongside the existing Application Insights SDKs and it will make standard and log-based metrics extremely inaccurate which will adversely impact all application insights experiences.

## Instrumentation Libraries
Microsoft has tested and validated that the following instrumentation libraries will work with the **Preview** Release.

> [!NOTE]
> Instrumentation libraries are based on experimental OpenTelemetry specifications. Microsoft’s **preview** support commitment is to ensure the libraries listed below emit data to Azure Monitor Application Insights, but it’s possible that breaking changes or experimental mapping will block some data elements.

### HTTP
- XYZ (version X.X)

### Database
- XYZ (version X.X)

> [!NOTE]
> The **preview** offering only includes instrumentations that handle HTTP and Database requests. In the future, we plan to support other request types. See [OpenTelemetry Semantic Conventions](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/trace/semantic_conventions) to learn more.

## Modify Telemetry
### Add Span Attributes
You may use X to add attributes to spans. This may include adding a custom business dimension to your telemetry or setting optional fields in the Application Insights Schema such as User ID or Client IP. We provide three common examples below.

#### Add Custom Dimension
Adding one or more custom dimensions will populate the _customDimensions_ field in the requests, dependencies, and/or exceptions table.

```python
Placeholder
```

#### Set User ID or Authenticated User ID
Setting user id will populate the _user_Id_ field in the requests, dependencies, and/or exceptions table.

```python
Placeholder
```

#### Set User IP
Setting the user IP populate the _client_IP_ field in the requests, dependencies, and/or exceptions table.

```python
Placeholder
```

For more information, see [GitHub Repo](link).

### Override Span Name
You may use X to override span name. This updates Operation Name from its default value to something that makes sense to your team. It will surface on the Failures and Performance Blade when you pivot by Operations.

```python
Placeholder
```

For more information, see [GitHub Repo](link).

### Set or Override Cloud Role Name
You may use the Resource API to set or override Cloud Role Name. This updates Cloud Role Name from its default value to something that makes sense to your team. It will surface on the Application Map as the name underneath a node.

```python
Placeholder
```

For more information, see [GitHub Repo](link).

### Filter Telemetry
You may use a Span Processor to filter out telemetry before leaving your application. This may be done to mask telemetry for privacy reasons or block unneeded telemetry to reduce ingestion costs.

```python
Placeholder
```

For more information, see [GitHub Repo](link).

### Get Trace ID or Span ID
You may use X or Y to get trace ID or span ID. This may be done to add these identifiers to existing logging telemetry to improve correlation when debugging and diagnosing issues.

```python
Placeholder
```

For more information, see [GitHub Repo](link).


## Troubleshooting
### Enable Diagnostic Logging
Placeholder

## Support
- Review Troubleshooting steps in this article
- For Azure support issues, file an Azure SDK GitHub issue or open a CSS Ticket.
- For OpenTelemetry issues, contact the [OpenTelemetry community](https://opentelemetry.io/community/) directly.

## Feedback
- Fill out the OpenTelemetry community’s [customer feedback survey](https://docs.google.com/forms/d/e/1FAIpQLScUt4reClurLi60xyHwGozgM9ZAz8pNAfBHhbTZ4gFWaaXIRQ/viewform).
- Tell Microsoft a bit about yourself by joining our [OpenTelemetry Early Adopter Community](https://aka.ms/AzMonOTel/).
- Add your feature requests to the [Azure Monitor Application Insights UserVoice](https://feedback.azure.com/forums/357324-azure-monitor-application-insights).
- Open a GitHub issue against this official documentation page.

## Next Steps
- [Azure Monitor Exporter GitHub Repository](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/monitor/azure-monitor-opentelemetry-exporter/README.md)
- [Azure Monitor Exporter  PyPI Package](https://pypi.org/project/azure-monitor-opentelemetry-exporter/)
- [Azure Monitor Sample Application](https://github.com/Azure-Samples/azure-monitor-opentelemetry-python)
- [OpenTelemetry Python GitHub Repository](https://github.com/open-telemetry/opentelemetry-python)
- [Enable web/browser user monitoring](./javascript.md)
