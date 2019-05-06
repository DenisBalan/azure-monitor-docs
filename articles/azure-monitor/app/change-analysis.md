---
title: Azure Monitor application change analysis - Discover changes that may impact live site issues/outages with Azure Monitor application change analysis  | Microsoft Docs
description: Troubleshoot application live site issues on Azure App Services with Azure Monitor application change analysis
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: cawa@microsoft.com
---

# Azure Monitor application change analysis (public preview)

When a live site issue/outage occurs, determining root cause quickly is critical. Standard monitoring solutions may help you rapidly identify that there is a problem, and often even which component is failing. But this won't always lead to an immediate explanation for why the failure is occurring. Your site worked five minutes ago, now it's broken. What changed in the last five minutes? This is the question that the new feature Azure Monitor application change analysis is designed to answer. By building on the power of the [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) application change analysis provides insight into your Azure App Service application changes to increase observability and reduce MTTR (Mean Time To Repair).

> [!IMPORTANT]
> Azure Monitor application change analysis is currently in public preview.
> This preview version is provided without a service level agreement, and it's not recommended for production workloads. Certain features might not be supported or might have constrained capabilities. 
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## How do I use application change analysis?

Azure Monitor application change analysis is currently built into the self-service **Diagnose and solve problems** experience, which can be accessed from the **Overview** section of your Azure App Service application:

![Screenshot of Azure App Service overview page with red boxes around overview button and diagnose and solve problems button](./media/change-analysis/change-analysis.png)

### Enable change analysis

When the detector for a particular symptom (such as web app down) runs, follow the instructions to enable change analysis. You will arrive at a pane with the following content:

![Screenshot of the Azure App Service enable change analysis screen](./media/change-analysis/change-analysis-on.png)

If **Change Analysis** is enabled, you will be able to detect resource level changes. If **Scan for code changes** is enabled, you will also see deployment files and site configuration changes. Enabling **Always on** will optimize the change scanning performance, but may incur additional costs from a billing perspective.

1. Once **Change Analysis** is enabled, choose **Diagnose and solve problems**:

2. Select **Availability and Performance** as an example problem type:

    ![screenshot of availability and performance troubleshooting options](./media/change-analysis/availability-and-performance.png)

3. Select **Web App Down** as the problem symptom.

     ![screenshot of web app down problem symptom](./media/change-analysis/web-app-down.png)

4. Open the Insights group to see the number of changes the web app has experienced recently. You will see a graph describing the type of changes that happened over time and details on those changes:

     ![screenshot of change diff view](./media/change-analysis/change-view.png)

## Next Steps

- Improve your monitoring of Azure App Services [by enabling the Application Insights features](azure-web-apps.md) of Azure Monitor.
- Enhance your understanding of the [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) which helps power Azure Monitor application change analysis. 
