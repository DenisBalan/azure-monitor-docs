---
title: Monitor Java applications on any environment 
description: Application performance monitoring for Java applications running in any environment without instrumenting the app. Distributed tracing and application map.
ms.topic: conceptual
ms.date: 03/29/2020

---

# Java codeless application monitoring - public preview

Java codeless application monitoring is all about simplicity - there are no code changes, teh Java agent will be enabled through just a couple of configuration changes.

The best news ever is that the Java agent work in any environment, and allows to monitor all of your Java applications.

Adding the Application Insights Java SDK to your application is no longer required, as the 3.0 agent auto-collects requests, dependencies and logs all on its own.

But don't worry, you can still send custom telemetry from your application if you wish, and the 3.0 agent will track and correlate it along with all of the auto-collected telemetry.

## Quick start

**1. Download the agent**

Download [applicationinsights-agent-3.0.0-PREVIEW.jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.0-PREVIEW/applicationinsights-agent-3.0.0-PREVIEW.jar)

**2. Point the JVM to the agent**

Add `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` to your application's JVM args

If you run into any trouble adding this to your application's JVM args, please see [3.0 Preview: Tips for updating your JVM args](https://github.com/microsoft/ApplicationInsights-Java/wiki/3.0-Preview:-Tips-for-updating-your-JVM-args).

**3. Point the agent to your Application Insights resource**

Create a configuration file named `ApplicationInsights.json`, and place it in the same directory as `applicationinsights-agent-3.0.0-PREVIEW.jar`, with the following content:

```
{
  "instrumentationSettings": {
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000"
  }
}
```

You can find your connection string in your Application Insights resource, e.g.

<img src="https://github.com/trask/docs-work-in-progress/raw/master/connectionstring.png" alt="Application Insights Connection String" width="75%"/><br>


**4. That's it!**

Now start up your application and go to your Application Insights resource in the Azure portal to see your monitoring data. Note: it may take a couple of minutes for your monitoring data to show up in the portal.

## Configuration options

In the `ApplicationInsights.json` file, you can additionally configure:

* Cloud role name
* Cloud role instance
* JMX metrics
* Sampling
* Logging capture threshold
* HTTP Proxy
* Heartbeat interval
* Self diagnostics

See details at [3.0 Public Preview: Configuration Options](https://github.com/microsoft/ApplicationInsights-Java/wiki/3.0-Preview:-Configuration-Options).

## Auto-collected requests, dependencies and logs

### Requests

* JMS Consumers
* Kafka Consumers
* Netty/WebFlux
* Servlets
* Spring Scheduling

### Dependencies with distributed trace propagation

* Apache HttpClient and HttpAsyncClient
* gRPC
* java.net.HttpURLConnection
* JMS
* Kafka
* Netty client
* OkHttp

### Other dependencies

* Cassandra
* JDBC
* MongoDB (async and sync)
* Redis (Lettuce and Jedis)

### Logs

* java.util.logging
* Log4j
* SLF4J/Logback

## Sending custom telemetry from your application

Our goal in 3.0+ is to allow you to send your custom telemetry using standard APIs.

We plan to initially support Micrometer, OpenTelemetry API, and the popular logging frameworks.

Application Insights Java 3.0 will automatically capture telemetry that is sent to these APIs, and correlate it along with all of the auto-collected telemetry.

For this reason, we are not planning to release an SDK with Application Insights 3.0 at this time.

Application Insights Java 3.0 is already listening for telemetry that is sent to the Application Insights Java SDK 2.x. This is an important part of the upgrade story for existing 2.x users, and it fills an important gap in our custom telemetry support until OpenTelemetry API is GA.

## Sending custom telemetry using Application Insights Java SDK 2.x

Add `applicationinsights-core-2.6.0.jar` to your application (all 2.x versions are supported by Application Insights Java 3.0, but it's worth using the latest if you have a choice), e.g.

```xml
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-core</artifactId>
    <version>2.6.0</version>
  </dependency>
```

Create a TelemetryClient, e.g.

  ```java
private static final TelemetryClient telemetryClient = new TelemetryClient();
```

and use that for sending custom telemetry.

### Events

  ```java
telemetryClient.trackEvent("WinGame");
```
### Metrics

```java
  telemetryClient.trackMetric("queueLength", 42.0);
```

### Dependencies

```java
  boolean success = false;
  long startTime = System.currentTimeMillis();
  try {
      success = dependency.call();
  } finally {
      long endTime = System.currentTimeMillis();
      RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
      telemetry.setTimestamp(new Date(startTime));
      telemetry.setDuration(new Duration(endTime - startTime));
      telemetryClient.trackDependency(telemetry);
  }
```

### Logs
You can send custom log telemetry via your favorite logging framework.

Or you can also use Application Insights Java SDK 2.x:

```java
  telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

### Exceptions
You can send custom exception telemetry via your favorite logging framework.

Or you can also use Application Insights Java SDK 2.x:

```java
  try {
      ...
  } catch (Exception e) {
      telemetryClient.trackException(e);
  }
```

## Upgrading from Application Insights Java SDK 2.x

If you are already using Application Insights Java SDK 2.x in your application, there is no need to remove it, as the 3.0 agent will detect it, and capture and correlate any custom telemetry you are sending via the Java SDK 2.x, while suppressing any auto-collection performed by the Java SDK 2.x in order to prevent duplicate capture.

Please note: Java SDK 2.x TelemetryInitializers and TelemetryProcessors will not be run when using the 3.0 agent. If this functionality is important for you, please reach out to the product team [asw-node-java-pr@microsoft.com](asw-node-java-pr@microsoft.com). We are still designing the replacement for this functionality in 3.0, and we want to make sure it will cover your use case(s).
