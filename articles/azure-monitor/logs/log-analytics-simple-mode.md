---
title: Analyze data using Log Analytics simple mode 
description: This article explains the new Log Analytics experience and how to use simple mode to explore and analyze data in Azure Monitor Logs.
ms.topic: conceptual
author: guywi-ms
ms.author: guywild
ms.reviewer: roygal
ms.date: 09/04/2023

# Customer intent: As a troubleshooter, I want to get insights from log data without using Kusto Query Language (KQL).

---

# Analyze data using Log Analytics simple mode 

The new Log Analytics offers two ways to explore data:  

- **Simple mode** offers the most commonly used Azure Monitor Logs functionality in an intuitive spreadsheet-like experience. Just point and click to filter, sort, and aggregate data to get to the insights you need 80% of the time. 
- **Advanced mode** gives advanced users the full power of Kusto Query Language (KQL) to derive deeper insights from their logs.

You can switch seamlessly between simple and advance mode, and advanced users can share complex queries, which anyone can continue working with in simple mode. 

This article explains the new Log Analytics experience and how to use simple mode to explore and analyze data in Azure Monitor Logs.     

## Try Log Analytics simple mode

Simple mode is currently in preview and available to a limited number of customers. To try it, select **Try Log Analytics simple mode** at the top right corner of the Log Analytics query editor.

:::image type="content" source="media/log-analytics-explorer/try-new-log-analytics.png" alt-text="A GIF showing the Try new Log Analytics button.":::

## The simple mode experience

This section explains the elements of the Log Analytics simple mode experience.

:::image type="content" source="media/log-analytics-explorer/log-analytics-simple-mode-experience.png" alt-text="Screenshot that shows Log Analytics simple mode." lightbox="media/log-analytics-explorer/log-analytics-simple-mode-experience.png":::

### Top query bar

In simple mode, the top bar has controls for working with data and switching to advanced mode.

| Option | Description |
|:---|:---|
| **Time range** | Select the [time range](./scope.md) for the data available to the query. In advanced mode, if you set a different time range in your query, the time range you set in the time picker is overridden. |
|**Filter**|Filter data, and apply simple mode operators, as described in [Explore and analyze data in simple mode](#explore-and-analyze-data-in-simple-mode).|
|**Limit**|Configure the number of entries Log Analytics displays in simple mode. The default limit is 1000.|
|**Simple/Advanced**|Switch between simple and advanced mode.|
| **Search**|Search through query results to find all instances of a particular string.|
| **Queries** | Open the example queries dialog that appears when you first open Log Analytics. |

For more information about using these controls to navigate and analyze data, see [Explore and analyze data in simple mode](#explore-and-analyze-data-in-simple-mode).
### Left pane

The collapsible left pane gives you access to tables in the workspace, sample and saved queries, query history, and filter options for the current query.

Pin the left pane to keep it open while you work, or maximize your query window by selecting an icon from the left pane only when you need it.

:::image type="content" source="media/log-analytics-explorer/log-analytics-left-sidebar.png" alt-text="Screenshot that shows the left sidebar in Log Analytics.":::

| Option | Description |
|:---|:---|
| **Tables** | Lists the tables that are part of the selected scope. Select **Group by** to change the grouping of the tables. Hover over a table name to display a dialog with a description of the table and options to view its documentation and preview its data. Expand a table to view its columns. Double-click a table or column name to add it to the query. |
| **Queries** | List of example and saved queries you can open in the query window. This list is the same one that appears when you open Log Analytics. Select **Group by** to change the grouping of the queries. Double-click a query to add it to the query window or hover over it for other options. |
|**Functions**|Lists functions, which allow you to reuse predefined query logic in your log queries. For more information, see [Function](../logs/functions.md)|
|**Query history**|Lists your query history. Select a query to rerun it.|
| **Filter** | We recommend using the new filter experience in the [top query bar](#top-query-bar). However, you can still access the classic filter experience here.|

### More tools

:::image type="content" source="media/log-analytics-explorer/log-analytics-more-tools.png" alt-text="Screenshot that shows the More tools window in Log Analytics.":::

| Option | Description |
|:---|:---|
| **Save** | [Save a query to a query pack](../logs/save-query.md). |
| **Share** | Copy a link to your query, the query text, or query results. |
| **New alert rule** | [Create a new alert rule](../alerts/alerts-create-new-alert-rule.md#create-or-edit-an-alert-rule-in-the-azure-portal). |
|**Export**|[Export data to Excel](../logs/log-excel.md), CSV, or [Power BI](../logs/log-powerbi.md).|
|**Pin to**|Pin your query to a [workbook](../visualize/workbooks-overview.md), or an [Azure dashboard](../visualize/tutorial-logs-dashboards.md) or [Grafana dashboard](../visualize/grafana-plugin.md#pin-charts-from-the-azure-portal-to-azure-managed-grafana).|
| **Format query** | Format query text in advanced mode. |
| **Search job mode** | [Run a search job](../logs/search-jobs.md). |
| **Switch back to classic Logs** | Switch back to the [classic Log Analytics user interface](../logs/log-analytics-overview.md). |

## Get started in simple mode

When you select a table or a predefined query or function in simple mode, Log Analytics automatically retrieves the relevant data for you to explore and analyze. 

This lets you retrieve logs with one click whether you open Log Analytics in resource or workspace context.

To get started:

- Click **Select a table** and select a table from the **Tables** tab to view table data.

    :::image type="content" source="media/log-analytics-explorer/log-analytics-select-table.png" alt-text="Screenshot that shows the Select a table button in Log Analytics.":::

    Alternatively, select **Tables** from the left pane to view the list of tables in the workspace.

    :::image type="content" source="media/log-analytics-explorer/log-analytics-tables.png" alt-text="Screenshot that shows the Tables tab in Log Analytics.":::

- Use an existing query, such as a shared or [saved query](../logs/save-query.md), or an example query.

    :::image type="content" source="media/log-analytics-explorer/log-analytics-simple-mode-example-query.png" alt-text="Screenshot that shows an example query in Log Analytics.":::

- Select a query from your query history.

    :::image type="content" source="media/log-analytics-explorer/log-analytics-query-history.png" alt-text="Screenshot that shows the query history in Log Analytics.":::

- Select a [function](../logs/functions.md).

    :::image type="content" source="media/log-analytics-explorer/log-analytics-functions.png" alt-text="Screenshot that shows the functions tab in Log Analytics."::: 

    > [!IMPORTANT]
    > Functions let you reuse query logic and often require input parameters or additional context. In such cases, the function won't run until you switch to advanced mode and provide the required input.  
  
## Explore and analyze data in simple mode 

After you [get started in simple mode](#get-started-in-simple-mode), you can explore and analyze data using the [top query bar](#top-query-bar). 

> [!NOTE]
> The order in which you apply filters and operators affects your inquiry and results. For example, if you apply a filter and then aggregate, Log Analytics applies the aggregation to the filtered data. If you aggregate and then filter, the aggregation is applied to the unfiltered data.

**Change time range and number of records displayed**

By default, simple mode lists the last 1000 entries in the table from the last 24 hours. 

To change the time range and number of records displayed, use the **Time range** and **Limit** selectors.
    
:::image type="content" source="media/log-analytics-explorer/log-analytics-time-range-limit.png" alt-text="Screenshot that shows the time range and limit selectors in Log Analytics.":::

**Filter by column**

1. Select **Filter** and choose a column.
1. Select an operator and a value to filter by, or enter text or numbers in the **Search** box.

    If you filter by selecting values from a list, you can select multiple values. If the list is long, you'll see a **Not all results are shown** message. Scroll to the bottom of the list and select **Load more** to retrieve more values.  
    
    :::image type="content" source="media/log-analytics-explorer/log-analytics-filter.png" alt-text="Screenshot that shows the filter menu that opens when you select Filter in Log Analytics simple mode.":::

    > [!NOTE]
    > Simple mode doesn't support filtering a dynamic column. To filter on data in a dynamic column, [extend the column](https://azure.microsoft.com/updates/azure-monitor-log-analytics-ui-updates-june-2019/). 

**Search for entries that have a specific value in the table**

1. Select **Search**.

    :::image type="content" source="media/log-analytics-explorer/log-analytics-search.png" alt-text="Screenshot that shows the Search option in Log Analytics simple mode.":::

1. Enter a value in the **Search this table** box and select **Apply**.

    Log Analytics filters the table to show only entries that contain the value you entered.

> [!IMPORTANT]
> We recommend using **Filter** if you know which column holds the data you're searching for. The [search operator is substantially less performant](../logs/query-optimization.md#avoid-unnecessary-use-of-search-and-union-operators) than filtering, and might not function well on large volumes of data.

**Aggregate**

1. Select **Aggregate**.
1. Select a column to aggregate by and select an operator to aggregate by, as described in [Use aggregation operators](#use-aggregation-operators).

    :::image type="content" source="media/log-analytics-explorer/log-analytics-aggregate.png" alt-text="Screenshot that shows the aggregation operators in the Aggregate table window in Log Analytics.":::

**Show or hide columns**

1. Select **Show columns**.
1. Select or clear columns to show or hide them, then select **Apply**.

    :::image type="content" source="media/log-analytics-explorer/log-analytics-show-column.png" alt-text="Screenshot that shows the Show columns window in Log Analytics.":::

> [!NOTE]
> Showing or hiding columns in simple mode does not add the [project](/azure/data-explorer/kusto/query/projectoperator) or [project-away](/azure/data-explorer/kusto/query/projectawayoperator) operators to your query when you switch to advanced mode.


**Sort by column**

1. Select **Sort**.
1. Select a column to sort by.
1. Select **Ascending** or **Descending**, then select **Apply**.  

    :::image type="content" source="media/log-analytics-explorer/log-analytics-sort.png" alt-text="Screenshot that shows the Sort by column window in Log Analytics.":::

1. Select **Sort** again to sort by another column.

### Use aggregation operators

| Operator | Description |
|:---|:---|
|`count`|Counts the number of times each distinct value exists in the column.|
|`dcount`|For the `dcount` operator, you select two columns. The operator counts the total number of distinct values in the second column correlated to each value in the first column. For example, this shows the distinct number of result codes for successful and failed operations:<br/> :::image type="content" source="media/log-analytics-explorer/log-analytics-dcount.png" alt-text="Screenshot that shows the result of an aggregation using the dcount operator in Azure Monitor Log Analytics.":::  |
|`sum`<br/>`avg`<br/>`max`<br/>`min`|For these operators, you select two columns. The operators calculate the sum, average, maximum, or minimum of all values in the second column for each value in the first column. For example, this shows the total duration of each operation in milliseconds for the past 24 hours:<br/>:::image type="content" source="media/log-analytics-explorer/log-analytics-sum.png" alt-text="Screenshot that shows the results of an aggregation using the sum operator in Azure Monitor Log Analytics.":::  |

> [!IMPORTANT]
> Basic logs tables don't support aggregation using the `avg` and `sum` operators. 
## Switch modes

To switch modes, select **Simple mode** or **Advanced mode** from the dropdown in the top right corner of the query editor.

:::image type="content" source="media/log-analytics-explorer/log-analytics-switch-modes-simple.png" alt-text="Screenshot that shows how to toggle between simple mode and advanced mode in Log Analytics.":::

When you begin to query logs in simple mode and then switch to advanced mode, the query editor is prepopulated with the KQL query related to your simple mode analysis. You can then edit and continue working with the query.

:::image type="content" source="media/log-analytics-explorer/log-analytics-switch-modes-advanced.png" alt-text="Screenshot that shows a query in Log Analytics advanced mode.":::

For simple queries on a table, simple mode displays the table name at the right of the top query bar. For more complex queries, displays **User Query** at the right of the top query bar. Click **User Query** to return to the query editor and continue working on your query at any time.

:::image type="content" source="media/log-analytics-explorer/log-analytics-switch-modes-user-query.png" alt-text="Screenshot that shows the User Query button, which lets you return to the query editor when you're in simple mode.":::

## Next steps
- Walk through a [tutorial on writing queries](../logs/log-analytics-tutorial.md).
- Access the complete [reference documentation for KQL](/azure/kusto/query/).
