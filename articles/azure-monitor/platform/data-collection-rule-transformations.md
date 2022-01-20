---
title: Data collection rule transformations
description: Use transformations in a data collection rule in Azure Monitor to filter and modify incoming data.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/19/2021

---

# Data collection rule transformations
[Data collection rules (DCR)](data-collection-rule-overview.md)  in Azure Monitor allow you to filter and transform data before its stored in a Log Analytics workspace. This article describes how to build transformations in a DCR, including details and limitations of the Kusto Query Language (KQL) used for the transform statement.

## Basic concepts
Transformations are applied individually to each entry in the source data. The input stream is represented by a virtual table named *source*. The transform specifies a KQL statement that accepts the data from *source* and creates output in the structure of the target table.



The columns of the table as per input data stream definition in *streamDeclarations* section of the Data Collection Rule.

## Example
Following is a typical example of a transformation. This example filters the incoming data, adds a new column, and then modifies an existing column. 

```kusto
source   // Transform statements always start with source to retrieve data from the source table.
| where severity == "Critical" // Filter statements. Any records not matching the filter are discarded.
| extend Properties = parse_json(properties)  // Parse columns containing JSON
// Form the output columns that match the structure of the target table.
| project
    TimeGenerated = todatetime(["time"]),
    Category = category,
    StatusDescription = StatusDescription,
    EventName = name,
    EventId = tostring(Properties.EventId)
```


## KQL limitations
Since the transformation is applied to each record individually, it can't use any KQL operators that act on multiple records. Only operators that take a single row as input and returns no more than one row are supported.

For example, [summarize](/azure/data-explorer/kusto/query/summarizeoperator) isn't supported since it summarizes multiple records. See [Supported KQL features](#supported-kql-features) for a complete list of supported features.

### Inline reference table
The [datatable](/azure/data-explorer/kusto/query/datatableoperator?pivots=azuremonitor) operator that would normally be used in KQL to define an inline query-time table is not supported in the subset of KQL available to use in transformations. Instead, use dynamic literals to work around this limitation.

For example, the following is not supported in a transformation:

```kusto
let galaxy = datatable (country:string,entity:string)['ES','Spain','US','United States'];
source
| join kind=inner (galaxy) on $left.Location == $right.country
| extend Galaxy_CF = ['entity']
```
But you can use the following statement which is supported and performs the same functionality:

```kusto
let galaxyDictionary = parsejson('{"ES": "Spain","US": "United States"}');
source
| extend Galaxy_CF = galaxyDictionary[Location]
```

### has operator
Transformations do not currently support [has](/azure/data-explorer/kusto/query/has-operator). Use [contains](/azure/data-explorer/kusto/query/contains-operator) which is supported and performs similar functionality.


### Handling dynamic data
Since the properties of type [dynamic](/azure/data-explorer/kusto/query/scalar-data-types/dynamic) are not supported in the input stream schema, you need alternate methods for strings containing JSON. 

Consider the following input:

```json
{
    "TimeGenerated" : "2021-11-07T09:13:06.570354Z",
    "Message": "Houston, we have a problem",
    "AdditionalContext": {
        "Level": 2,
        "DeviceID": "apollo13"
    }
}
```

In order to access the properties in *AdditionalContext*, define it as string-typed column in the input stream:

```json
"columns": [
    {
        "name": "TimeGenerated",
        "type": "datetime"
    },
    {
        "name": "Message",
        "type": "string"
    }, 
    {
        "name": "AdditionalContext",
        "type": "string"
    }
]
```

The content of *AdditionalContext* column can now be parsed and used in the KQL transformation:

```kusto
source
| extend parsedAdditionalContext = parse_json(AdditionalContext)
| extend Level = toint (parsedAdditionalContext.Level)
| extend DeviceId = tostring(parsedAdditionalContext.DeviceID)
```

## Supported KQL features

### Supported statements

####	let statement
The right-hand side of [let](/data-explorer/kusto/query/letstatement) can be a scalar expression, a tabular expression or a user-defined function. Only user-defined functions with scalar arguments are supported.

#### tabular expression statements
The only supported data sources for the KQL statement are as follows:

–	**source**, which represents the source data. For example:

```kql     
source
| where ActivityId == "383112e4-a7a8-4b94-a701-4266dfc18e41"
| project PreciseTimeStamp, Message
```

–	print](/azure/data-explorer/kusto/query/printoperator) operator, which always produces a single row. For example:
 	
```kusto
print x = 2 + 2, y = 5 | extend z = exp2(x) + exp2(y)
```


### Tabular Operators
- [extend](/azure/data-explorer/kusto/query/extendoperator)
- [project](/azure/data-explorer/kusto/query/projectoperator)
- [print](/azure/data-explorer/kusto/query/printoperator)
- [where](/azure/data-explorer/kusto/query/whereoperator)
- [parse](/azure/data-explorer/kusto/query/parseoperator)
- [project-away](/azure/data-explorer/kusto/query/projectawayoperator)
- [project-rename](/azure/data-explorer/kusto/query/projectrenameoperator)
- [columnifexists]() (use columnifexists instead of column_ifexists)

### Scalar operators

#### Numerical operators
All [Numerical operators](/azure/data-explorer/kusto/query/numoperators) are supported.

#### Datetime and Timespan arithmetic operators
All [Datetime and Timespan arithmetic operators](/azure/data-explorer/kusto/query/datetime-timespan-arithmetic) are supported.

#### String operators
The following [String operators](/azure/data-explorer/kusto/query/datatypes-string-operators) are supported.

– ==
– !=
– =~
– !~
– contains
– !contains
– contains_cs
– !contains_cs
– startswith
– !startswith
– startswith_cs
– !startswith_cs
– endswith
– !endswith
– endswith_cs
– !endswith_cs
– matches regex
– in
– !in

#### Bitwise operators

The following [Bitwise operators](/azure/data-explorer/kusto/query/binoperators) are supported.

– binary_and()
– binary_or()
– binary_xor()
– binary_not()
– binary_shift_left()
– binary_shift_right()

### Scalar functions

#### Bitwise functions

- [binary_and](/azure/data-explorer/kusto/query/binary-andfunction)
- [binary_or](/azure/data-explorer/kusto/query/binary-orfunction)
- [binary_not](/azure/data-explorer/kusto/query/binary-notfunction)
- [binary_shift_left](/azure/data-explorer/kusto/query/binary-shift-leftfunction)
- [binary_shift_right](/azure/data-explorer/kusto/query/binary-shift-rightfunction)
- [binary_xor](/azure/data-explorer/kusto/query/binary-xorfunction)

#### Conversion functions

- [tobool](/azure/data-explorer/kusto/query/toboolfunction)
- [todatetime](/azure/data-explorer/kusto/query/todatetimefunction)
- [todouble/toreal](/azure/data-explorer/kusto/query/todoublefunction)
- [toguid](/azure/data-explorer/kusto/query/toguid)
- [toint](/azure/data-explorer/kusto/query/toint)
- [tolong](/azure/data-explorer/kusto/query/tolong)
- [tostring](/azure/data-explorer/kusto/query/tostringfunction)
- [totimespan](/azure/data-explorer/kusto/query/totimespanfunction)

#### DateTime and TimeSpan functions

- [ago](/azure/data-explorer/kusto/query/agofunction)
- [datetime_add](/azure/data-explorer/kusto/query/datetime-addfunction)
- [datetime_diff](/azure/data-explorer/kusto/query/datetime-difffunction)
- [datetime_part](/azure/data-explorer/kusto/query/datetime-partfunction)
- [dayofmonth](/azure/data-explorer/kusto/query/dayofmonthfunction)
- [dayofweek](/azure/data-explorer/kusto/query/dayofweekfunction)
- [dayofyear](/azure/data-explorer/kusto/query/dayofyearfunction)
- [endofday](/azure/data-explorer/kusto/query/endofdayfunction)
- [endofmonth](/azure/data-explorer/kusto/query/endofmonthfunction)
- [endofweek](/azure/data-explorer/kusto/query/endofweekfunction)
- [endofyear](/azure/data-explorer/kusto/query/endofyearfunction)
- [getmonth](/azure/data-explorer/kusto/query/getmonthfunction)
- [getyear](/azure/data-explorer/kusto/query/getyearfunction)
- [hourofday](/azure/data-explorer/kusto/query/hourofdayfunction)
- [make_datetime](/azure/data-explorer/kusto/query/make-datetimefunction)
- [make_timespan](/azure/data-explorer/kusto/query/make-timespanfunction)
- [now](/azure/data-explorer/kusto/query/nowfunction)
- [startofday](/azure/data-explorer/kusto/query/startofdayfunction)
- [startofmonth](/azure/data-explorer/kusto/query/startofmonthfunction)
- [startofweek](/azure/data-explorer/kusto/query/startofweekfunction)
- [startofyear](/azure/data-explorer/kusto/query/startofyearfunction)
- [todatetime](/azure/data-explorer/kusto/query/todatetimefunction)
- [totimespan](/azure/data-explorer/kusto/query/totimespanfunction)
- [weekofyear](/azure/data-explorer/kusto/query/weekofyearfunction)

#### Dynamic and array functions

- [array_concat](/azure/data-explorer/kusto/query/arrayconcatfunction)
- [array_length](/azure/data-explorer/kusto/query/arraylengthfunction)
- [pack_array](/azure/data-explorer/kusto/query/packarrayfunction)
- [pack](/azure/data-explorer/kusto/query/packfunction)
- [parse_json](/azure/data-explorer/kusto/query/parsejsonfunction)
- [parse_xml](/azure/data-explorer/kusto/query/parse-xmlfunction.html)
- [zip](/azure/data-explorer/kusto/query/zipfunction)

#### Mathematical functions

- [abs](/azure/data-explorer/kusto/query/abs-function)
- [bin/floor](/azure/data-explorer/kusto/query/binfunction)
- [ceiling](/azure/data-explorer/kusto/query/ceilingfunction)
- [exp](/azure/data-explorer/kusto/query/exp-function)
- [exp10](/azure/data-explorer/kusto/query/exp10-function)
- [exp2](/azure/data-explorer/kusto/query/exp2-function)
- [isfinite](/azure/data-explorer/kusto/query/isfinitefunction)
- [isinf](/azure/data-explorer/kusto/query/isinffunction)
- [isnan](/azure/data-explorer/kusto/query/isnanfunction)
- [log](/azure/data-explorer/kusto/query/log-function)
- [log10](/azure/data-explorer/kusto/query/log10-function)
- [log2](/azure/data-explorer/kusto/query/log2-function)
- [pow](/azure/data-explorer/kusto/query/powfunction)
- [round](/azure/data-explorer/kusto/query/roundfunction)
- [sign](/azure/data-explorer/kusto/query/signfunction)

#### Conditional functions

- [case](/azure/data-explorer/kusto/query/casefunction)
- [iif](/azure/data-explorer/kusto/query/iiffunction)
- [max_of](/azure/data-explorer/kusto/query/max-offunction)
- [min_of](/azure/data-explorer/kusto/query/min-offunction)

#### String functions

- [base64_encodestring](/azure/data-explorer/kusto/query/base64_encode_tostringfunction) (use base64_encodestring instead of base64_encode_tostring)
- [base64_decodestring](/azure/data-explorer/kusto/query/base64_decode_tostringfunction) (use base64_decodestring instead of base64_decode_tostring)
- [countof](/azure/data-explorer/kusto/query/countoffunction)
- [extract](/azure/data-explorer/kusto/query/extractfunction)
- [extract_all](/azure/data-explorer/kusto/query/extractallfunction)
- [indexof](/azure/data-explorer/kusto/query/indexoffunction)
- [isempty](/azure/data-explorer/kusto/query/isemptyfunction)
- [isnotempty](/azure/data-explorer/kusto/query/isnotemptyfunction)
- [parse_json](/azure/data-explorer/kusto/query/parsejsonfunction)
- [split](/azure/data-explorer/kusto/query/splitfunction)
- [strcat](/azure/data-explorer/kusto/query/strcatfunction)
- [strcat_delim](/azure/data-explorer/kusto/query/strcat-delimfunction)
- [strlen](/azure/data-explorer/kusto/query/strlenfunction)
- [substring](/azure/data-explorer/kusto/query/substringfunction)
- [tolower](/azure/data-explorer/kusto/query/tolowerfunction)
- [toupper](/azure/data-explorer/kusto/query/toupperfunction)
- [hash_sha256](/azure/data-explorer/kusto/query/sha256hashfunction)

#### Type functions

- [gettype](/azure/data-explorer/kusto/query/gettypefunction)
- [isnotnull](/azure/data-explorer/kusto/query/isnotnullfunction)
- [isnull](/azure/data-explorer/kusto/query/isnullfunction)

### Identifier quoting
Use [Identifier quoting](/azure/data-explorer/kusto/query/schema-entities/entity-names?q=identifier#identifier-quoting) as required.

### Dynamic literals
[Dynamic literals](/azure/data-explorer/kusto/query/scalar-data-types/dynamic#dynamic-literals) are not supported, but you can use the [parse_json function](/azure/data-explorer/kusto/query/parsejsonfunction) as a workaround.

For example, the following query is not supported:

```kql
print d=dynamic({"a":123, "b":"hello", "c":[1,2,3], "d":{}})
    ```

The following query is supported and provides the same functionality:

```kql
print d=parse_json('{"a":123, "b":"hello", "c":[1,2,3], "d":{}}')
```


## Next steps

- [Create a data collection rule](data-collection-rule-azure-monitor-agent.md) and an association to it from a virtual machine using the Azure Monitor agent.
