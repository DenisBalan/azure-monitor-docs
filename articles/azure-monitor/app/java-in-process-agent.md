---
title: Azure Monitor Application Insights Java
description: Application performance monitoring for Java applications running in any environment without requiring code modification. Distributed tracing and application map.
ms.topic: conceptual
ms.date: 06/24/2021
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
---

# Azure Monitor OpenTelemetry-based Auto-Instrumentation for Java Applications

This article describes how to enable and configure the OpenTelemetry-based Azure Monitor Java offering. When you complete the instructions in this article, you’ll be able to use Azure Monitor Application Insights to monitor your application.

Java auto-instrumentation can be enabled without any code changes, and it works in any environment.

> [!NOTE]
> For most scenarios, Java 3.X auto-instrumentation is all you need. However, to enable some types of [custom telemetry](#supported-custom-telemetry), you'll also need to add the [Java 2.x SDK](./java-2x-get-started.md).

## Get Started
### Prerequisites
- Java Application using version 8+
- Azure Subscription (Free to [create](https://azure.microsoft.com/free/))
- Application Insights Resource (Free to [create](create-workspace-resource.md#create-workspace-based-resource))

### Enable Azure Monitor Application Insights
**1. Download the auto-instrumentation jar file**

> [!WARNING]
> **If you are upgrading from 3.0 Preview**
>
> Please review all the [configuration options](./java-standalone-config.md) carefully,
> as the json structure has completely changed, in addition to the file name itself which went all lowercase.

> [!WARNING]
> **If you are upgrading from 3.0.x**
>
> The operation names and request telemetry names are now prefixed by the http method (`GET`, `POST`, etc.).
> This can affect custom dashboards or alerts if they relied on the previous unprefixed values.
> See the [3.1.0 release notes](https://github.com/microsoft/ApplicationInsights-Java/releases/tag/3.1.0)
> for more details.

Download [applicationinsights-agent-3.1.1.jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.1.1/applicationinsights-agent-3.1.1.jar)

**2. Point the JVM to the jar file**

Add `-javaagent:path/to/applicationinsights-agent-3.1.1.jar` to your application's JVM args. 

For help with configuring your application's JVM args, see [Tips for updating your JVM args](./java-standalone-arguments.md).

**3. Point the jar file to your Application Insights resource**

Point the jar file to your Application Insights resource, either by setting an environment variable:

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=...
```

Or by creating a configuration file named `applicationinsights.json`, and placing it in the same directory as `applicationinsights-agent-3.1.1.jar`, with the following content:

```json
{
  "connectionString": "InstrumentationKey=..."
}
```

You can find your connection string in your Application Insights resource:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights Connection String":::

**4. That's it!**

Now start up your application and go to your Application Insights resource in the Azure portal to see your monitoring data.

> [!NOTE]
> It may take a couple minutes for data to show up in the Portal.

> [!IMPORTANT]
> If you have two or more micro-services using the same connection string, you are required to set cloud role names to represent them properly on the Application Map.

## Configuration options

In the `applicationinsights.json` file, you can additionally configure:

* Cloud role name
* Cloud role instance
* Sampling
* JMX metrics
* Custom dimensions
* Telemetry processors (preview)
* Auto-collected logging
* Auto-collected Micrometer metrics (including Spring Boot Actuator metrics)
* Heartbeat
* HTTP Proxy
* Self-diagnostics

See [configuration options](./java-standalone-config.md) for full details.

## Instrumentation Libraries

Java 3.X includes the following instrumentation libraries.

### Auto-collected requests

* JMS Consumers
* Kafka Consumers
* Netty/WebFlux
* Servlets
* Spring Scheduling

### Auto-collected dependencies

Auto-collected dependencies plus downstream distributed trace propagation:

* Apache HttpClient and HttpAsyncClient
* gRPC
* java.net.HttpURLConnection
* JMS
* Kafka
* Netty client
* OkHttp

Auto-collected dependencies (without downstream distributed trace propagation):

* Cassandra
* JDBC
* MongoDB (async and sync)
* Redis (Lettuce and Jedis)

### Auto-collected logs

* java.util.logging
* Log4j (including MDC properties)
* SLF4J/Logback (including MDC properties)

### Auto-collected metrics

* Micrometer (including Spring Boot Actuator metrics)
* JMX Metrics

### Azure SDKs (preview)

See the [configuration options](./java-standalone-config.md#auto-collected-azure-sdk-telemetry-preview)
to enable this preview feature and auto-collect the telemetry emitted by these Azure SDKs:

* [App Configuration](/java/api/overview/azure/data-appconfiguration-readme) 1.1.10+
* [Cognitive Search](/java/api/overview/azure/search-documents-readme) 11.3.0+
* [Communication Chat](/java/api/overview/azure/communication-chat-readme) 1.0.0+
* [Communication Common](/java/api/overview/azure/communication-common-readme) 1.0.0+
* [Communication Identity](/java/api/overview/azure/communication-identity-readme) 1.0.0+
* [Communication Phone Numbers](/java/api/overview/azure/communication-phonenumbers-readme) 1.0.0+
* [Communication SMS](/java/api/overview/azure/communication-sms-readme) 1.0.0+
* [Cosmos DB](/java/api/overview/azure/cosmos-readme) 4.13.0+
* [Digital Twins - Core](/java/api/overview/azure/digitaltwins-core-readme) 1.1.0+
* [Event Grid](/java/api/overview/azure/messaging-eventgrid-readme) 4.0.0+
* [Event Hubs](/java/api/overview/azure/messaging-eventhubs-readme) 5.6.0+
* [Event Hubs - Azure Blob Storage Checkpoint Store](/java/api/overview/azure/messaging-eventhubs-checkpointstore-blob-readme) 1.5.1+
* [Form Recognizer](/java/api/overview/azure/ai-formrecognizer-readme) 3.0.6+
* [Identity](/java/api/overview/azure/identity-readme) 1.2.4+
* [Key Vault - Certificates](/java/api/overview/azure/security-keyvault-certificates-readme) 4.1.6+
* [Key Vault - Keys](/java/api/overview/azure/security-keyvault-keys-readme) 4.2.6+
* [Key Vault - Secrets](/java/api/overview/azure/security-keyvault-secrets-readme) 4.2.6+
* [Service Bus](/java/api/overview/azure/messaging-servicebus-readme) 7.1.0+
* [Storage - Blobs](/java/api/overview/azure/storage-blob-readme) 12.11.0+
* [Storage - Blobs Batch](/java/api/overview/azure/storage-blob-batch-readme) 12.9.0+
* [Storage - Blobs Cryptography](/java/api/overview/azure/storage-blob-cryptography-readme) 12.11.0+
* [Storage - Common](/java/api/overview/azure/storage-common-readme) 12.11.0+
* [Storage - Files Data Lake](/java/api/overview/azure/storage-file-datalake-readme) 12.5.0+
* [Storage - Files Shares](/java/api/overview/azure/storage-file-share-readme) 12.9.0+
* [Storage - Queues](/java/api/overview/azure/storage-queue-readme) 12.9.0+
* [Text Analytics](/java/api/overview/azure/ai-textanalytics-readme) 5.0.4+

[//]: # "the above names and links scraped from https://azure.github.io/azure-sdk/releases/latest/java.html"
[//]: # "and version sync'd manually against the oldest version in maven central built on azure-core 1.14.0"
[//]: # ""
[//]: # "var table = document.querySelector('#tg-sb-content > div > table')"
[//]: # "var str = ''"
[//]: # "for (var i = 1, row; row = table.rows[i]; i++) {"
[//]: # "  var name = row.cells[0].getElementsByTagName('div')[0].textContent.trim()"
[//]: # "  var stableRow = row.cells[1]"
[//]: # "  var versionBadge = stableRow.querySelector('.badge')"
[//]: # "  if (!versionBadge) {"
[//]: # "    continue"
[//]: # "  }"
[//]: # "  var version = versionBadge.textContent.trim()"
[//]: # "  var link = stableRow.querySelectorAll('a')[2].href"
[//]: # "  str += '* [' + name + '](' + link + ') ' + version + '\n'"
[//]: # "}"
[//]: # "console.log(str)"

## Modify Telemetry
### Add Span Attributes
You may use X to add attributes to spans. These attributes may include adding a custom business dimension to your telemetry. You may also use attributes to set optional fields in the Application Insights Schema such as User ID or Client IP. Below are three examples that show common scenarios.

For more information, see [GitHub Repo](link).

#### Add Custom Dimension
Adding one or more custom dimensions will populate the _customDimensions_ field in the requests, dependencies, and/or exceptions table.

> [!NOTE]
> This feature is only in 3.0.2 and later

Add `applicationinsights-web-2.6.3.jar` to your application
(all 2.x versions are supported by Application Insights Java 3.x, but it's worth using the latest if you have a choice):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.3</version>
</dependency>
```

and add custom dimensions in your code:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getProperties().put("mydimension", "myvalue");
```

#### Set User ID or Authenticated User ID
Populate the _user_Id_ or _user_Authenticatedid_ field in the requests, dependencies, and/or exceptions table. User ID is an anonymous user identifier and Authenticated User ID is a known user identifier.

> [!TIP]
> Instrument with the the [JavaScript SDK](javascript.md) to automatically populate User ID.

> [!IMPORTANT]
> Consult applicable privacy laws before setting Authenticated User ID.

> [!NOTE]
> This feature is only in 3.0.2 and later

Add `applicationinsights-web-2.6.3.jar` to your application
(all 2.x versions are supported by Application Insights Java 3.x, but it's worth using the latest if you have a choice):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.3</version>
</dependency>
```

and set the `user_Id` in your code:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getContext().getUser().setId("myuser");
```

#### Set User IP
Populate the _client_IP_ field in the requests, dependencies, and/or exceptions table. Application Insights uses the IP address to generate user location attributes and then [discards it by default](ip-collection.md#default-behavior).

> [!TIP]
> Instrument with the the [JavaScript SDK](javascript.md) to automatically populate User IP.

```java
Placeholder
```

### Override Span Name
You may use X to override span name. This updates Operation Name from its default value to something that makes sense to your team. It will surface on the Failures and Performance Blade when you pivot by Operations.

> [!NOTE]
> This feature is only in 3.0.2 and later

Add `applicationinsights-web-2.6.3.jar` to your application
(all 2.x versions are supported by Application Insights Java 3.x, but it's worth using the latest if you have a choice):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.3</version>
</dependency>
```

and set the name in your code:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.setName("myname");
```

### Get Trace ID or Span ID
You may use X or Y to get trace ID or span ID. This may be done to add these identifiers to existing logging telemetry to improve correlation when debugging and diagnosing issues.

> [!NOTE]
> If you are manually creating spans for log-based metrics and alerting, you will need to update them to use the metrics API (after it is released) to ensure accuracy.

> [!NOTE]
> This feature is only in 3.0.3 and later

Add `applicationinsights-web-2.6.3.jar` to your application
(all 2.x versions are supported by Application Insights Java 3.x, but it's worth using the latest if you have a choice):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.3</version>
</dependency>
```

and get the request telemetry ID and the operation ID in your code:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
String requestId = requestTelemetry.getId();
String operationId = requestTelemetry.getContext().getOperation().getId();
```
## Custom Telemetry

Our goal in Application Insights Java 3.x is to allow you to send your custom telemetry using standard APIs.

We support Micrometer, popular logging frameworks, and the Application Insights Java 2.x SDK so far.
Application Insights Java 3.x automatically captures the telemetry sent through these APIs,
and correlates it with auto-collected telemetry.

### Supported custom telemetry

The table below represents currently supported custom telemetry types that you can enable to supplement the Java 3.x agent. To summarize, custom metrics are supported through micrometer, custom exceptions and traces can be enabled through logging frameworks, and any type of the custom telemetry is supported through the [Application Insights Java 2.x SDK](#send-custom-telemetry-using-the-2x-sdk).

|                     | Micrometer | Log4j, logback, JUL | 2.x SDK |
|---------------------|------------|---------------------|---------|
| **Custom Events**   |            |                     |  Yes    |
| **Custom Metrics**  |  Yes       |                     |  Yes    |
| **Dependencies**    |            |                     |  Yes    |
| **Exceptions**      |            |  Yes                |  Yes    |
| **Page Views**      |            |                     |  Yes    |
| **Requests**        |            |                     |  Yes    |
| **Traces**          |            |  Yes                |  Yes    |

We're not planning to release an SDK with Application Insights 3.x at this time.

Application Insights Java 3.x is already listening for telemetry that is sent to the Application Insights Java 2.x SDK. This functionality is an important part of the upgrade story for existing 2.x users, and it fills an important gap in our custom telemetry support until the OpenTelemetry API is GA.

### Send custom metrics using Micrometer

Add Micrometer to your application:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-core</artifactId>
  <version>1.6.1</version>
</dependency>
```

Use the Micrometer [global registry](https://micrometer.io/docs/concepts#_global_registry) to create a meter:

```java
static final Counter counter = Metrics.counter("test_counter");
```

and use that to record metrics:

```java
counter.increment();
```

### Send custom traces and exceptions using your favorite logging framework

Log4j, Logback, and java.util.logging are auto-instrumented,
and logging performed via these logging frameworks is auto-collected as trace and exception telemetry.

By default, logging is only collected when that logging is performed at the INFO level or above.
See the [configuration options](./java-standalone-config.md#auto-collected-logging) for how to change this level.

If you want to attach custom dimensions to your logs, you can use
[Log4j 1.2 MDC](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/MDC.html),
[Log4j 2 MDC](https://logging.apache.org/log4j/2.x/manual/thread-context.html),
or [Logback MDC](http://logback.qos.ch/manual/mdc.html),
and Application Insights Java 3.x will automatically capture those MDC properties as custom dimensions
on your trace and exception telemetry.

### Send custom telemetry using the 2.x SDK

Add `applicationinsights-core-2.6.3.jar` to your application
(all 2.x versions are supported by Application Insights Java 3.x, but it's worth using the latest if you have a choice):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-core</artifactId>
  <version>2.6.3</version>
</dependency>
```

Create a TelemetryClient:

  ```java
static final TelemetryClient telemetryClient = new TelemetryClient();
```

and use that to send custom telemetry:

##### Events

```java
telemetryClient.trackEvent("WinGame");
```

##### Metrics

```java
telemetryClient.trackMetric("queueLength", 42.0);
```

##### Dependencies

```java
boolean success = false;
long startTime = System.currentTimeMillis();
try {
    success = dependency.call();
} finally {
    long endTime = System.currentTimeMillis();
    RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
    telemetry.setSuccess(success);
    telemetry.setTimestamp(new Date(startTime));
    telemetry.setDuration(new Duration(endTime - startTime));
    telemetryClient.trackDependency(telemetry);
}
```

##### Logs

```java
telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

##### Exceptions

```java
try {
    ...
} catch (Exception e) {
    telemetryClient.trackException(e);
}
```

## Enable OTLP Exporter
You may want to enable the OTLP Exporter alongside your Azure Monitor Exporter to send your telemetry to two locations.

Add the code to xyz.file in your application.
```Java
Placeholder
```

> [!NOTE]
> OTLP exporter is shown for convenience only. We do not officially support the OTLP Exporter or any components or third-party experiences downstream of it. We suggest you open an issue with the OpenTelemetry community for OpenTelemetry issues outside the Azure Support Boundary.

## Troubleshooting
See [Troubleshooting](java-standalone-troubleshoot.md).

## Support
- Review [Troubleshooting steps](java-standalone-troubleshoot.md).
- For Azure support issues, file an Azure SDK GitHub issue or open a CSS Ticket.
- For OpenTelemetry issues, contact the [OpenTelemetry community](https://opentelemetry.io/community/) directly.

## OpenTelemetry Feedback
- Fill out the OpenTelemetry community’s [customer feedback survey](https://docs.google.com/forms/d/e/1FAIpQLScUt4reClurLi60xyHwGozgM9ZAz8pNAfBHhbTZ4gFWaaXIRQ/viewform).
- Tell Microsoft a bit about yourself by joining our [OpenTelemetry Early Adopter Community](https://aka.ms/AzMonOTel/).
- Add your feature requests to the [Azure Monitor Application Insights UserVoice](https://feedback.azure.com/forums/357324-azure-monitor-application-insights).

## Next Steps
- [Azure Monitor Java Auto-Instrumentation GitHub Repository](https://github.com/Microsoft/ApplicationInsights-Java)
- [Azure Monitor Example Application]()
- [OpenTelemetry Community Language GitHub Repository](https://github.com/open-telemetry/opentelemetry-java-instrumentation)
- [Enable web/browser user monitoring](javascript.md)
