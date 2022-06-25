---
title: Azure Monitor workbook text visualizations
description: Learn about all the Azure Monitor workbook text visualizations.
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/04/2020
---
# Text visualizations

Workbooks allow you to include text blocks in your workbooks. The text can be human analysis of telemetry, information to help users interpret your data, and section headings.

[![Screenshot that shows the Apdex table of text.](./media/workbooks-text-visualizations/apdex.png)](./media/workbooks-text-visualizations/apdex.png#lightbox)

Text is added through a Markdown control, which provides full formatting control. Formatting includes different heading and font styles, hyperlinks, and tables.

Edit mode:

![Screenshot that shows a text step in edit mode.](./media/workbooks-text-visualizations/text-edit-mode.png)

Preview mode:

![Screenshot that shows a text step in edit mode on the Preview tab.](./media/workbooks-text-visualizations/text-edit-mode-preview.png)

## Add a text control

1. Switch the workbook to edit mode by selecting the **Edit** toolbar item.
1. Use the **Add text** link to add a text control to the workbook.
1. Add Markdown in the editor box.
1. Use the **Text Style** option to switch between plain Markdown and Markdown wrapped with the Azure portal's standard info, warning, success, and error styling.
1. Use the **Preview** tab to see how your content will look. During editing, the preview shows the content inside a scrollbar area to limit its size. At runtime, the Markdown content expands to fill whatever space it needs, with no scrollbars.
1. Select **Done Editing** to finish editing the step.

> [!TIP]
> Use this [Markdown cheat sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) to learn about different formatting options.

## Text styles

The following text styles are available for the text step:

| Style     | Explanation                                                                               |
|-----------|-------------------------------------------------------------------------------------------|
| `plain`   | No other formatting is applied.                                                      |
| `info`    | The portal's "info" style, with an  `ℹ` or similar icon and generally a blue background.      |
| `error`   | The portal's "error" style, with an `❌` or similar icon and generally a red background.     |
| `success` | The portal's "success" style, with a `✔` or similar icon and generally a green background.  |
| `upsell`  | The portal's "upsell" style, with a `🚀` or similar icon and generally a purple background. |
| `warning` | The portal's "warning" style, with a `⚠` or similar icon and generally a blue background.   |

Instead of selecting a specific style, you can also choose a text parameter as the source of the style. The parameter value must be one of the preceding text values. The absence of a value or any unrecognized value is treated as `plain` style.

Info style example:

![Screenshot that shows what the info style looks like.](./media/workbooks-text-visualizations/text-preview-info-style.png)

Warning style example:

![Screenshot that shows what the warning style looks like.](./media/workbooks-text-visualizations/text-warning-style.png)

## Next steps

* Learn how to create a [chart in workbooks](workbooks-chart-visualizations.md).
* Learn how to create a [grid in workbooks](workbooks-grid-visualizations.md).
