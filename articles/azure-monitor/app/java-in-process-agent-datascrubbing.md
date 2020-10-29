---
title: Data Scrubbing with Java In Process Agent - Azure Monitor Application Insights
description: Data Scrubbing with Java In Process Agent
ms.topic: conceptual
ms.date: 10/29/2020

---

# Data Scrubbing with Java In Process Agent - private preview

Java In Process Agent has now the capabilities to pre-process telemetry data before it is exported.

### Some Use Cases:
 * Mask sensitive data.
 * Filter data to control cost.
 * Static attributes for all telemetry, e.g. applying "k8spod=abc" to all telemetry.

### Supported Processors:
 * Attribute Processor
 * Span Processor

## Quick Start

Create a configuration file named ApplicationInsights.json, and place it in the same directory as `applicationinsights-agent-***.jar`, with the following content:

```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            // your processor configuration
        ]
    }
}
```

## Processor Configuration Options:

### Include/Exclude Spans

The attribute processor and the span processor expose the option to provide a set of properties of a span to match against to determine if the span should be included or excluded from the processor. To configure this option, under `include` and/or `exclude` at least `matchType` and one of `spanNames` or `attributes` is required. It is supported to have more than one specified, but all of the specified conditions must evaluate to true for a match to occur. 

`matchType` controls how items in `spanNames` and `attributes` arrays are interpreted. Possible values are `regexp` or `strict`. This is a required field.
`spanNames` must match at least one of the items. This is an optional field.
`attributes` specifies the list of attributes to match against. All of these attributes must match exactly for a match to occur. This is an optional field.

Note: If both `include` and `exclude` are specified, the `include` properties are checked before the `exclude` properties.

#### Sample Usage

The following demonstrates specifying the set of span properties to
indicate which spans this processor should be applied to. The `include` of
properties say which ones should be included and the `exclude` properties
further filter out spans that shouldn't be processed.

Ex. The following are spans match the properties and the actions are applied.
Note this span is processed because the value type of redact_trace is a string instead of a boolean.

Span1 Name: 'svcB' Attributes: {env: production, test_request: 123, credit_card: 1234, redact_trace: "false"}

Span2 Name: 'svcA' Attributes: {env: staging, test_request: false, redact_trace: true}
The following span does not match the include properties and the
processor actions are not applied.

Span3 Name: 'svcB' Attributes: {env: production, test_request: true, credit_card: 1234, redact_trace: false}

Span4 Name: 'svcC' Attributes: {env: dev, test_request: false}

```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "attribute",
                "processorName": "attributes/selectiveProcessing",
                "include": {
                    "matchType": "strict",
                    "spanNames": [
                        "svcA",
                        "svcB"
                    ]
                },
                "exclude": {
                    "matchType": "strict",
                    "attributes": [
                        {
                            "key": "redact_trace",
                            "value": "false"
                        }
                    ]
                },
                "actions": [
                    {
                        "key": "credit_card",
                        "action": "delete"
                    },
                    {
                        "key": "duplicate_key",
                        "action": "delete"
                    }
                ]
            },
        ]
    }
}
```

### Attribute Processor 

The attributes processor modifies attributes of a span. It optionally supports the ability to include/exclude spans.
It takes a list of actions which are performed in order specified in the config. The supported actions are:

* `insert` : Inserts a new attribute in spans where the key does not already exist.
* `update` : Updates an attribute in spans where the key does exist.
* `delete` : Deletes an attribute from a span.
* `hash`   : Hashes (SHA1) an existing attribute value.

For the actions `insert` and `update`
* `key` is required
* one of `value` or `fromAttribute` is required
* `action` is required.

For the `delete` action,
* `key` is required
* `action`: `delete` is required.

For the `hash` action,
* `key` is required
* `action` : `hash` is required.

The list of actions can be composed to create rich scenarios, such as back filling attribute, copying values to a new key, redacting sensitive information.

#### Sample Usage

The following example demonstrates inserting keys/values into spans.

```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "attribute",
                "processorName": "attributes/insert",
                "actions": [
                    {
                        "key" : "attribute1",
                        "value" : "value1",
                        "action" : "insert"
                    },
                    {
                        "key" : "key1",
                        "fromAttribute" : "anotherkey",
                        "action" : "insert"
                    }
                ]
            }
        ]
    }
}
```
The following demonstrates configuring the processor to only update existing keys in an attribute.

```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "attribute",
                "processorName": "attributes/update",
                "actions": [
                    {
                        "key" : "piiattribute",
                        "value" : "redacted",
                        "action" : "update"
                    }, 
                    {
                        "key" : "credit_card",
                        "action" : "delete"
                    },
                    {
                        "key" : "user.email",
                        "action" : "hash"
                    }
                ]
            },
        ]
    }
}
```
The following demonstrates how to process spans that have a span name that match regexp patterns. This processor will remove "token" attribute and will obfuscate "password" attribute in spans where span name matches "auth.*" 
and where span name does not match "login.*".

```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "attribute",
                "processorName": "attributes/regexupdate",
                "include": {
                    "matchType": "regexp",
                    "spanNames": [
                        "auth.*"
                    ]
                },
                "exclude": {
                    "matchType": "regexp",
                    "spanNames": [
                        "login.*"
                    ]
                },
                "actions": [
                    {
                        "key" : "password",
                        "value" : "obfuscated",
                        "action" : "update"
                    }, 
                    {
                        "key" : "token",
                        "action" : "delete"
                    }
                ]
            },
        ]
    }
}
```

### Span Processors

The span processor modifies either the span name or attributes of a span based on the span name. It optionally supports the ability to include/exclude spans.

#### Name a span

The following settings are required as part of the name section:

`fromAttributes`: The attribute value for the keys are used to create a new name in the order specified in the configuration. All attribute keys needs to be specified in the span for the processor to rename it.

The following settings can be optionally configured:

`separator`: A string, which is specified will be used to split values

Note: If renaming is dependent on attributes being modified by the attributes processor, ensure the span processor is specified after the attributes processor in the pipeline specification.

##### Sample Usage

The following specifies the values of attribute "db.svc", "operation", and "id" will form the new name of the span, in that order, separated by the value "::".
```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "span",
                "processorName": "span/renameSpan",
                "name" : {
                    "fromAttributes": [
                        "db.svc", 
                        "operation", 
                        "id"
                    ],
                    "separator": "::"
                }
            },
        ]
    }
}

```

#### Extract attributes from span name

Takes a list of regular expressions to match span name against and extract attributes from it based on subexpressions. Must be specified under the `toAttributes` section.

The following settings are required:

`rules` : A list of rules to extract attribute values from span name. The values in the span name are replaced by extracted attribute names. Each rule in the list is regex pattern string. Span name is checked against the regex and if the regex matches then all named subexpressions of the regex are extracted as attributes and are added to the span. Each subexpression name becomes an attribute name and subexpression matched portion becomes the attribute value. The matched portion in the span name is replaced by extracted attribute name. If the attributes already exist in the span then they will be overwritten. The process is repeated for all rules in the order they are specified. Each subsequent rule works on the span name that is the output after processing the previous rule.

##### Sample Usage

Let's assume input span name is /api/v1/document/12345678/update. Applying the following results in output span name /api/v1/document/{documentId}/update and will add a new attribute "documentId"="12345678" to the span.
```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "span",
                "processorName": "span/extractAttributes",
                "name" : {
                    "toAttributes": {
                        "rules": [
                            "^\/api\/v1\/document\/(?P<documentId>.*)\/update$"
                        ]
                    }
                }
            },
        ]
    }
}

```

The following demonstrates renaming the span name to "{operation_website}" and adding the attribute {Key: operation_website, Value: oldSpanName } when the span has the following properties:
- The span name contains '/' anywhere in the string.
- The span name is not 'donot/change'.
```json
{
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
        "processors": [
            {
                "type": "span",
                "processorName": "span/extractAttributesWithIncludeExclude",
                "include": {
                    "matchType": "regexp",
                    "spanNames": [
                        "^(.*?)/(.*?)$"
                    ]
                },
                "exclude": {
                    "matchType": "strict",
                    "spanNames": [
                        "donot/change"
                    ]
                },
                "name" : {
                    "toAttributes": {
                        "rules": [
                            "(?P<operation_website>.*?)$"
                        ]
                    }
                }
            },
        ]
    }
}

```