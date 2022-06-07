---
title: Azure Workbooks templates
description: Learn how to use workbooks templates.
services: azure-monitor
author: AbbyMSFT
ms.author: abbyweisberg
ms.topic: conceptual
ms.date: 05/30/2022
ms.reviewer: gardnerjr 
---

# Azure Workbook templates

## Workbooks versus workbook templates

You can see a _workbook_ in green and a number of _workbook templates_ in purple. Templates serve as curated reports that are designed for flexible reuse by multiple users and teams. Opening a template creates a transient workbook populated with the content of the template.

You can adjust the template-based workbook's parameters and perform analysis without fear of breaking the future reporting experience for colleagues. If you open a template, make some adjustments, and then select the save icon you will be saving the template as a workbook which would then show in green leaving the original template untouched.

Under the hood, templates also differ from saved workbooks. Saving a workbook creates an associated Azure Resource Manager resource, whereas the transient workbook created when just opening a template has no unique resource associated with it. [Click here](workbooks-overview.md#access-control) to learn more about how access control is managed in workbooks.

## Explore a workbook template

Select **Application Failure Analysis** to see one of the default application workbook templates.

:::image type="content" source="./media/workbooks-overview/failure-analysis.png" alt-text="Screenshot of application failure analysis template." border="false" lightbox="./media/workbooks-overview/failure-analysis.png":::

As stated previously, opening the template creates a temporary workbook for you to be able to interact with. By default, the workbook opens in reading mode which displays only the information for the intended analysis experience that was created by the original template author.

In the case of this particular workbook, the experience is interactive. You can adjust the subscription, targeted apps, and the time range of the data you want to display. Once you have made those selections the grid of HTTP Requests is also interactive whereby selecting an individual row will change what data is rendered in the two charts at the bottom of the report.

## Editing a template

To understand how this workbook template is put together you need to swap to editing mode by selecting **Edit**.

:::image type="content" source="./media/workbooks-overview/edit.png" alt-text="Screenshot of edit button in workbooks." border="false" :::

Once you have switched to editing mode you will notice a number of **Edit** boxes appear to the right corresponding with each individual aspect of your workbook.

:::image type="content" source="./media/workbooks-overview/edit-mode.png" alt-text="Screenshot of Edit button." border="false" lightbox="./media/workbooks-overview/edit-mode.png":::

If we select the edit button immediately under the grid of request data we can see that this part of our workbook consists of a Kusto query against data from an Application Insights resource.

:::image type="content" source="./media/workbooks-overview/kusto.png" alt-text="Screenshot of underlying Kusto query." border="false" lightbox="./media/workbooks-overview/kusto.png":::

Selecting the other **Edit** buttons on the right will reveal a number of the core components that make up workbooks like markdown-based [text boxes](../visualize/workbooks-text-visualizations.md), [parameter selection](../visualize/workbooks-parameters.md) UI elements, and other [chart/visualization types](workbooks-visualizations.md).

Exploring the pre-built templates in edit-mode and then modifying them to fit your needs and save your own custom workbook is an excellent way to start to learn about what is possible with Azure Monitor workbooks.