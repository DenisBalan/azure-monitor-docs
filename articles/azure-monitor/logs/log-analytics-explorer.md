---
title: Analyze log data in Azure Monitor using the new Log Analytics 
description: This article explains the new Log Analytics interface and how to use simple and advanced modes to explore and analyze data in Azure Monitor Logs.
ms.topic: conceptual
author: guywild
ms.author: guywild
ms.reviewer: roygal
ms.date: 09/04/2023

---

# Analyze log data in Azure Monitor using the new Log Analytics 

The new Log Analytics features a simplified user interface and two modes for working with log data: simple and advanced. 

Simple mode provides a spreadsheet-like experience to get you started quickly. Similar to working in Excel, you can navigate your data and apply a set of operators without writing Kusto Query Language (KQL). For more complex analysis, switch to advanced mode and take advantage of Azure Monitor's full data analysis capabilities.

This article explains the new Log Analytics interface and how to use simple and advanced modes to explore and analyze data in Azure Monitor Logs.     

## Try the new Log Analytics

The new Log Analytics is currently in preview and available to a limited number of customers. To try it, select **Try the new Log Analytics** in the top right corner of the Log Analytics query editor.

:::image type="content" source="media/log-analytics-explorer/try-new-log-analytics.png" alt-text="A GIF showing the Try new Log Analytics button.":::

## Switch modes

To switch modes, select **Simple mode** or **Advanced mode** from the dropdown in the top right corner of the query editor.

When you begin to query logs in simple mode and then switch to advanced mode, the query editor is pre-populated with the KQL query related to your simple mode analysis. You can then edit and continue working with the query.

:::image type="content" source="media/log-analytics-explorer/log-analytics-switch-modes.gif" alt-text="A GIF showing two Log Analytics query tabs, one in simple mode and one in advanced mode.":::

## Query in simple mode 

**Select a table to view log data**

1. Click **Select a table** and select a table from the **Tables** tab.

    By default, simple mode lists the last 1000 entries in the table from the last 24 hours. 

1. To change the time range and number of records displayed, use the **Time range** and **Limit** cards.

**Filter by column**

1. Select **Filter** and choose a column.
1. Select an operator and enter a value for the filter, or enter text or numbers in the **Search** box.
    
**Search for entries that have a specific value in the table**

1. Select **Search**.
1. Enter a value in the **Search this table** box and select **Apply**.

    Log Analytics filters the table to show only entries that contain the value you entered.

**Aggregate**

1. Select **Aggregate**.
1. Select a column to aggregate by.
1. Select an operator to aggregate by, as described in the table below.

| Operator | Description |
|:---|:---|
|`count`|Counts the number of times each distinct value exists in the column.|
|`dcount`|For the `dcount` operator, you select two columns. The operator counts the total number of distinct values in the second column correlated to each value in the first column. For example, this shows the distinct number of result codes for successful and failed operations:<br/> :::image type="content" source="media/log-analytics-explorer/log-analytics-dcount.png" alt-text="Screenshot that shows the result of an aggregation using the dcount operator in Azure Monitor Log Analytics.":::  |
|`sum`<br/>`avg`<br/>`max`<br/>`min`|For these operators, you select two columns. The operators calculate the sum, average, maximum, or minimum of all values in the second column for each value in the first column. For example, this shows the total duration of each operation in milliseconds for the past 24 hours:<br/>:::image type="content" source="media/log-analytics-explorer/log-analytics-sum.png" alt-text="Screenshot that shows the results of an aggregation using the sum operator in Azure Monitor Log Analytics.":::  |

**Show or hide columns**

1. Select **Show columns**.
1. Select or clear columns to show or hide them, then select **Apply**.
1. Select an operator to aggregate by.


**Sort by column**

1. Select **Sort**.
1. Select a column to sort by.
1. Select **Ascending** or **Descending**, then select **Apply**.  
1. Select **Sort** again to sort by another column.

## Log Analytics interface

This image shows the Log Analytics simple mode components.

:::image type="content" source="media/log-analytics-explorer/new-log-analytics-user-interface.png" alt-text="Screenshot that shows the Log Analytics simple mode interface." lightbox="media/log-analytics-explorer/new-log-analytics-user-interface.png":::

### Top action bar

The top bar has controls for working with a query in the query window.

| Option | Description |
|:---|:---|
| Time picker | Select the time range for the data available to the query. This action is overridden if you include a time filter in the query. See [Log query scope and time range in Azure Monitor Log Analytics](./scope.md). |
|Filter|Filter the results by selecting the funnel next to a column name. Clear the filters and reset the sorting by running the query again.|
|Limit|Limit the number of records displayed in the table.|
|Simple/Advanced|Switch between simple and advanced mode.|
| Example queries button | Open the example queries dialog that appears when you first open Log Analytics. |
| Query Explorer button | Open **Query Explorer**, which provides access to saved queries in the workspace. |

### Left sidebar

The sidebar on the left lists tables in the workspace, sample queries, and filter options for the current query.

| Tab | Description |
|:---|:---|
| Tables | Lists the tables that are part of the selected scope. Select **Group by** to change the grouping of the tables. Hover over a table name to display a dialog with a description of the table and options to view its documentation and preview its data. Expand a table to view its columns. Double-click a table or column name to add it to the query. |
| Queries | List of example queries that you can open in the query window. This list is the same one that appears when you open Log Analytics. Select **Group by** to change the grouping of the queries. Double-click a query to add it to the query window or hover over it for other options. |
| Filter | Creates filter options based on the results of a query. After you run a query, columns appear with different values from the results. Select one or more values, and then select **Apply & Run** to add a **where** command to the query and run it again. |

### Query window

The query window is where you edit your query. IntelliSense is used for KQL commands and color coding enhances readability. Select **+** at the top of the window to open another tab.

A single window can include multiple queries. A query can't include any blank lines, so you can separate multiple queries in a window with one or more blank lines. The current query is the one with the cursor positioned anywhere in it.

To run the current query, select the **Run** button or select **Shift+Enter**.

### Results window

The results of a query appear in the results window. By default, the results are displayed as a table. To display the results as a chart, select **Chart** in the results window. You can also add a **render** command to your query.

#### Results view

The results view displays query results in a table organized by columns and rows. Click to the left of a row to expand its values. Select the **Columns** dropdown to change the list of columns. Sort the results by selecting a column name. Filter the results by selecting the funnel next to a column name. Clear the filters and reset the sorting by running the query again.

Select **Group columns** to display the grouping bar above the query results. Group the results by any column by dragging it to the bar. Create nested groups in the results by adding more columns.

#### Chart view

The chart view displays the results as one of multiple available chart types. You can specify the chart type in a **render** command in your query. You can also select it from the **Visualization Type** dropdown.

| Option | Description |
|:---|:---|
| Visualization type | Type of chart to display. |
| X-axis | Column in the results to use for the x-axis.
| Y-axis | Column in the results to use for the y-axis. Typically, this is a numeric column. |
| Split by | Column in the results that defines the series in the chart. A series is created for each value in the column. |
| Aggregation | Type of aggregation to perform on the numeric values in the y-axis. |

 
## Next steps
- Walk through a [tutorial on writing queries](/azure/data-explorer/kusto/query/tutorial?pivots=azuremonitor).
- Access the complete [reference documentation for KQL](/azure/kusto/query/).
