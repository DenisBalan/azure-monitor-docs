---
title: "Tutorial: Get started with log queries - Azure Monitor"
description: Learn from this tutorial how to write and manage Azure Monitor log queries in a Log Analytics workspace in the Azure portal.
ms.subservice: logs
ms.topic: tutorial
author: v-thepet
ms.author: v-thepet
ms.date: 03/02/2020

---

# Tutorial: Get started with Azure Monitor log queries in a Log Analytics workspace

In this tutorial, you learn how to use a Log Analytics workspace to write, execute, and manage Azure Monitor log queries in the Azure portal. You can use Log Analytics queries to search for terms, identify trends, analyze patterns, and provide many other insights based on your data. 

The tutorial shows you how to use a Log Analytics workspace to:

> [!div class="checklist"]
> * Understand Azure Monitor log data schema and tables
> * Write and run simple queries, and modify the time range for queries
> * Filter, sort, and group query results
> * View, modify, and present visuals of query results
> * Save, load, export, and share queries and results

For more details about log queries, see [Overview of log queries in Azure Monitor](log-query-overview.md).
For a detailed tutorial on writing log queries, see [Get started with log queries in Azure Monitor](get-started-queries.md).

To complete this tutorial, you can use [this demo environment](https://portal.loganalytics.io/demo), which includes plenty of sample data.

You can also use your own environment, if you're using Azure Monitor to collect log data for at least one Azure resource. To open the Log Analytics workspace, select **Logs** in your Azure Monitor left navigation. 

## Understand the schema
A data *schema* is a collection of tables grouped under logical categories on the **Tables** tab of the Log Analytics workspace. 

The **Demo** schema has several monitoring categories. For example, the **LogManagement** category contains Windows and Syslog events, performance data, and agent heartbeats.

Each table contains columns with different data types shown by icons next to the column names. For example, the **Event** table contains columns such as **Computer**, which is text, and **EventCategory**, a number.

![Schema](media/get-started-portal/schema.png)

## Write and run basic queries

A Log Analytics workspace opens with a new blank query in the **Query editor**.

![Log Analytics workspace](media/get-started-portal/homepage.png)

Azure Monitor log queries use a version of the Kusto query language. Queries can begin with either a table name or a [search](/azure/kusto/query/searchoperator) command. 

The following query retrieves all records from the **Event** table:

```Kusto
Event
```

The pipe (|) character separates commands, so the output of the first command serves as the input of the next command. You can add any number of commands to a single query. The following query retrieves the records from the **Event** table, and then searches them for the term **error** in any property:

```Kusto
Event 
| search "error"
```

A single line break makes queries easier to read. More than one line break splits the query into separate queries.

Another way to write the same query is:

```Kusto
search in (Event) "error"
```

In the second example, the **search** command searches only records in the **Events** table for the term **error**.

To run a query, place your cursor somewhere inside the query, and select **Run** in the top bar or press **Shift**+**Enter**. The query runs until it finds a blank line.

Log Analytics limits results to a maximum of 10,000 records, and by default, limits queries to a time range of the past 24 hours. To set a different time range, you can add an explicit **TimeGenerated** filter to the query, or use the **Time range** control.

To use the **Time range** control, select it in the top bar, and then select a value from the dropdown list, or select **Custom** to create a custom time range.

![Time picker](media/get-started-portal/time-picker.png)

- Time range values are in UTC, which could be different than your local time zone.
- If the query explicitly sets a filter for **TimeGenerated**, the time picker control shows **Set in query**, and is disabled to prevent a conflict.

## Filter results
A general query like `Event` returns too many results to be useful. You can filter the query results either through retricting the table elements in the query, or by explicitly adding a filter to the results. Filtering through the table elements returns a new result set, while an explicit filter applies to the existing result set.

To filter `Event` query results to **Error** events by restricting the table elements in the query:

1. In the query results, select the dropdown arrow to the left of any result that has **Error** in the **EventLevelName** column. 
   
1. In the expanded details, hover over and select the **...** next to **EventLevelName**, and then select **Include "Error"**. 
   
   ![Add filter to query](media/get-started-portal/add-filter.png)
   
1. Notice that the query in the **Query editor** has now changed to:
   
   ```Kusto
   Event
   | where EventLevelName == "Error"
   ```
   
1. Select **Run** to run the new query.

To filter the `Event` query results to **Error** events by filtering the query results:

1. In the query results, select the **Filter** icon next to the column heading **EventLevelName**. 
   
1. In the first field of the pop-up window, select **Is equal to**, and in the next field, type *error*. 
   
1. Select **Filter**.
   
   ![Filter](media/get-started-portal/filter.png)

## Sort, group, and show columns
To sort query results by a specific column, such as **TimeGenerated (UTC)**, select the column heading. Select again to toggle between ascending and descending order.

![Sort column](media/get-started-portal/sort-column.png)

Another way to organize results is by groups. To group results by a specific column, drag the column header up to the bar above the results table labeled **Drag a column header and drop it here to group by that column**. To create subgroups, drag other columns to the upper bar. You can rearrange the hierarchy and sorting of the groups and subgroups in the bar.

![Groups](media/get-started-portal/groups.png)

To add or remove columns that show in the results, select **Columns** above the table, and then select or deselect desired columns in the dropdown list.

![Select columns](media/get-started-portal/select-columns.png)

## Charts
You can also see query results in visual formats. Enter the following query as an example:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

By default, results appear in a table. Select **Chart** above the table to see the results in a graphic view.

![Bar chart](media/get-started-portal/bar-chart.png)

The results appear in a stacked bar chart. Select other options like **Stacked Column** or **Pie** to show other views of the results.

![Pie chart](media/get-started-portal/pie-chart.png)

You can change properties of the view, such as x and y axes, or grouping and splitting preferences, manually from the control bar.

You can also set the preferred view in the query itself, using the `render` operator.

## Pin results to a shared dashboard
To pin a results chart or table from your Log Analytics workspace to a shared Azure dashboard, select **Pin to dashboard** on the top bar. 

![Pin to dashboard](media/get-started-portal/pin-dashboard.png)

In the **Pin to another dashboard** pane, select the shared dashboard to pin the results to, or create a new one, and select **Apply**. 

![Chart pinned to dashboard](media/get-started-portal/pin-dashboard2.png)

A chart or table that you pin to a shared dashboard updates with the latest data. The chart or table on the dashboard has the following simplifications: 

- Data is limited to the past 14 days.
- A table shows only up to four columns and the top seven rows.
- Charts with many discrete categories automatically group less populated categories into a single **others** bin.

## Save, load, export, or share queries
Once you've created a useful query, you can save it or share with others. 

To save a query:

1. Select **Save** on the top bar.
   
1. In the **Save** dialog, give the query a **Name**, using the characters a–z, A–Z, 0-9, space, hyphen -, underscore _, period ., parenthesis ( or ), or pipe |. 
   
1. Select whether to save the query as a **Query** or a **Function**. Functions are queries that other queries can reference. 
   
   To save a query as a function, provide a function alias, which is a short name for other queries to use to call this query.
   
1. Provide a **Category** name for the query to be saved under in **Query Explorer**.
   
1. Select **Save**.
   
   ![Save function](media/get-started-portal/save-function.png)

To load a saved query, select **Query Explorer** at top right. The **Query Explorer** pane opens, listing all saved queries by category. Expand the categories or enter a query name in the search bar, then select the query to load it in the **Query editor**. You can mark a query as a **Favorite** by selecting the star next to the query name, or select the **...** to rename or delete the query.

![Query explorer](media/get-started-portal/query-explorer.png)

To export a query, select **Export** on the top bar, and then select **Export to CSV - all columns**, **Export to CSV - displayed columns**, or **Export to Power BI (M query)** from the dropdown list.
To share a link to a query, select **Copy link** on the top bar, and then select **Copy link to query**, **Copy query text**, or **Copy query results** to copy to the clipboard. You can send the query link to others who have access to the same workspace.

## Next steps

In this tutorial, you learned to write and manage simple queries in a Log Analytics workspace in the Azure portal. Advance to the next tutorial to learn more about writing Azure Monitor log queries.
> [!div class="nextstepaction"]
> [Next steps button](get-started-queries.md)
