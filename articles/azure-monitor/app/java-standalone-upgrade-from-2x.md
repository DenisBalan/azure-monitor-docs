---
title: Upgrading from 2.x - Azure Monitor Application Insights Java
description: Upgrading from Azure Monitor Application Insights Java 2.x
ms.topic: conceptual
ms.date: 11/25/2020

---

# Upgrading from Application Insights Java SDK 2.x

If you're already using Application Insights Java SDK 2.x in your application, there is no need to remove it.
The Java 3.0 agent will detect it, and capture and correlate any custom telemetry you're sending via the Java SDK 2.x,
while suppressing any auto-collection performed by the Java SDK 2.x to prevent duplicate telemetry.

If you were using Application Insights 2.x agent, you need to remove the `-javaagent:` JVM arg
that was pointing to the 2.x agent.

> [!NOTE]
> Java SDK 2.x TelemetryInitializers and TelemetryProcessors will not be run when using the 3.0 agent.
> Many of the use cases that previously required these can be solved in 3.0
> by configuring [custom dimensions](./java-standalone-config.md#custom-dimensions)
> or configuring [telemetry processors](./java-standalone-telemetry-processors.md).

> [!NOTE]
> 3.0 does not support multiple instrumentation keys in a single JVM yet.

## HTTP request telemetry names
 
HTTP request telemetry names have changed.

The new name has lower cardinality, and provides better aggregation than the previous telemetry names.

However, for some applications and use-cases, the previous telemetry names may be optimal,
in which case you can configure telemetry processors in 3.0 to maintain the same behavior.

### To prefix the telemetry name with the http method (`GET`, `POST`, etc):

```
{
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "attributes": [
            { "key": "http.method", "value": "" }
          ],
          "spanNames": [ "^/" ]
        },
        "name": {
          "toAttributes": {
            "rules": [ "^(?<tempName>.*)$" ]
          }
        }
      },
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempName" }
          ]
        },
        "name": {
          "fromAttributes": [ "http.method", "tempName" ],
          "separator": " "
        }
      },
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempName" }
          ]
        },
        "actions": [
          { "key": "tempName", "action": "delete" }
        ]
      }
    ]
  }
}
```

### To set the telemetry name to the full URL path

```
{
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "http.method" },
            { "key": "http.url" }
          ]
        },
        "name": {
          "fromAttributes": [ "http.url" ]
        }
      },
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "http.method" },
            { "key": "http.url" }
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [ "https?://[^/]+(?<tempPath>/[^?]*)" ]
          }
        }
      },
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempPath" }
          ]
        },
        "name": {
          "fromAttributes": [ "tempPath" ]
        }
      },
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempPath" }
          ]
        },
        "actions": [
          { "key": "tempPath", "action": "delete" }
        ]
      }
    ]
  }
}
```
