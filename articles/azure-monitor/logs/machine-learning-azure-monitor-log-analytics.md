---
title: Analyze log data with machine learning in Azure Monitor Log Analytics
description: #Required; article description that is displayed in search results. 
ms.service: azure-monitor
ms.topic: tutorial 
author: guywild
ms.author: guywild
ms.reviewer: ilanawaitser
ms.date: 07/01/2022

# Customer intent:  As a data analyst, I want to use the native machine learning capabilities of Log Analytics to gain insights from my log data without having to export data outside of Azure Monitor.

---

# Tutorial: Analyze log data with machine learning in Azure Monitor Log Analytics 

Azure Monitor provides advanced machine learning capabilities, powered by Kusto, Microsoft's big data analytics cloud platform.

The Kusto Query Language (KQL) lets you analyze logs in Azure Monitor and includes a set of machine learning operators and plugins for time series analysis, anomaly detection and forecasting, and pattern recognition for root cause analysis. 

Using the KQL machine learning operators with Log Anayltic's native tools for extracting insights from logs - including queries, workbooks and dashboards, and integration with Excel - provides you with: 

- Savings on the costs and overhead of exporting data to external machine learning tools.
- The power of Kusto’s distributed database, running at high scales.
- Greater flexibility and deeper insights than out-of-the-box insights tools.

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Conduct time series analysis using the `make-series` operator
> * Identify usage anomalies using the `series_decompose_anomalies()` function
> * Analyze the root cause of anomalies

## Prerequisites

- <!-- An Azure account with an active subscription. [Create an account for free]
  (https://azure.microsoft.com/free/?WT.mc_id=A261C142F). -->
- <!-- prerequisite 2 -->
- <!-- prerequisite n -->

<!-- 5. H2s
Required. Give each H2 a heading that sets expectations for the content that follows. 
Follow the H2 headings with a sentence about how the section contributes to the whole.
-->

## Analyze usage anomalies using the make-series operator

We'll start by running a query using the time series operator on all billable data types in the `Usage` table. We'll be able to see major anomalies just by looking at the chart. 

1. Create a time series chart for all billable data types:

    ```kusto
    let starttime = 21d; // # of days back from the current date to start the time series
    let endtime = 0d; // # of days back from the current date to end the time series
    let timeframe = 1d; // How often to sample data
    Usage // The table we’re analyzing
    | where TimeGenerated between (startofday(ago(starttime))..startofday(ago(endtime))) // Time range for the query, beginning at 12:00 am of the first day and ending at 11:59 of the last day in the time range
    | where IsBillable == "true" // IsBillable is a standard column in Log Analytics, indicating billable events
    | make-series ActualCount=sum(Quantity) default = 0 on TimeGenerated from startofday(ago(starttime)) to startofday(ago(endtime)) step timeframe by DataType // TODO
    | render timechart // Renders results in a timechart
    ``` 

    We can see anomalies in the resulting chart - for example, in the `AzureDiagnostics` and `SecurityEvent` data types.

2. Let's modify our query to return all anomalies for all data types. 
    
    Replace the `render` operator in the last line of our previous query with the following lines of KQL:

    ```kusto
    | extend(anomalies, anomalyScore, expectedCount) = series_decompose_anomalies(ActualCount) // Extracts anomalous points with scores, the only parameter we pass here is the output of make-series, other parameters are default 
    | mv-expand ActualCount to typeof(double), TimeGenerated to typeof(datetime), anomalies to typeof(double),anomalyScore to typeof(double), expectedCount to typeof(long) // TODO
    | where anomalies != 0  // Return all positive and negative usage deviations from the expected count
    | project TimeGenerated, ActualCount, expectedCount,DataType,anomalyScore,anomalies // TODO check casing of column names
    | sort by abs(anomalyScore) desc;
    ```
1. Filter the `DataType` column for `AzureDiagnostics` 

    We see only two anomalies - on June 14 and June 15 - while in the time series, we also saw deviations on May 27 and May 28.

 
## Teach the Log Analytics machine learning algorithm to identify anomalies using the series_decompose_anomalies() function

To get more accurate results, exclude the last day of June 15 with major outlier from the learning (training) process.

For that replace series_decompose_anomalies(ActualCount) in the query with the following:

series_decompose_anomalies(ActualCount,1.5,-1,'avg',1) // 1.5 is the threshold (anomalyscore threshold) [default], -1 is Autodetect seasonality [default], avg -  Define trend component as average of the series [default], 1 – number of points  at the end of the series to exclude from the learning process [default]


Get all usage anomalies for all data types.
3.	Focus in on analyzing anomalies of the Azure Diagnostics data type. We’ll run anomaly detection and pattern recognition functions on Azure Diagnostics table to find out which resource caused anomalous usage. 


<!-- 6. Clean up resources
Required. If resources were created during the tutorial. If no resources were created, 
state that there are no resources to clean up in this section.
-->

## Next steps

Advance to the next article to learn how to create...
> [!div class="nextstepaction"]
> [Next steps button](contribute-how-to-mvc-tutorial.md)

<!--
Remove all the comments in this template before you sign-off or merge to the 
main branch.
-->
