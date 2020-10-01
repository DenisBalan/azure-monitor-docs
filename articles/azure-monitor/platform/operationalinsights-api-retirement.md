---
title: Azure Monitor API retirement
description: Describes the retirement of older versions of the OperationalInsights resource provider API.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/01/2020

---

# OperationalInsights API version retirement
Microsoft provides notification at least 36 months in advance of retiring an API in order to smooth the transition to a newer/supported version. We have released a new version (2020-08-01) for **OperationalInsights** resource provider APIs and will retire any earlier API versions on October 31, 2023. Since new features and functionality and optimizations are only added to the current API you should upgrade to the latest API version as early as possible.

After October 31, 2023 Azure Monitor will no longer make bug fixes, add new features, and provide support to the following OperationalInsights APIs versions: 

- 2020-03-01-preview
- 2019-08-01-preview
- 2017-04-26-preview
- 2017-03-15-preview
- 2015-11-01-preview
- 2015-03-20

If you prefer not to upgrade, requests sent from earlier versions will continue to be served by the Azure Monitor service until October 31, 2023.

Depending on the configuration method you use, you should update the new version in REST requests and Resource Manager templates per the following examples:


## REST

PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01

## Azure Resource Manager

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
              "description": "Name of the workspace."
            }
        },
        "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2020-08-01",
            "location": "westus",
            "properties": {
                "sku": {
                    "name": "pergb2018"
                },
                "retentionInDays": 30,
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            }
        }
    ]
  }
}
```


## Next steps

- See the [reference for the OperationalInsights API](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/allversions).
