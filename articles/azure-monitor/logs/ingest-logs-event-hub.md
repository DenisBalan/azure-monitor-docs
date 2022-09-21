---
title: Send events from Azure Event Hubs to Azure Monitor Logs with a data collection rule
description: Ingest logs from Event Hub into Azure Monitor Logs 
services: azure-monitor
author: guywi-ms
ms.author: guywild
ms.reviewer: ilanawaitser
ms.topic: tutorial 
ms.date: 09/20/2022
ms.custom: template-tutorial 

# customer-intent: As a workspace administrator, I want to collect data from an event hub into a Log Analytics workspace so that I can monitor application logs that I ingest using Azure Event Hubs.
---


# Tutorial: Send events from Azure Event Hubs to Azure Monitor Logs with a data collection rule   

[Azure Event Hubs](../../event-hubs/event-hubs-about.md) is a big data streaming platform that collects events from multiple sources to be ingested by Azure and external services.This article explains how to ingest data directly from an event hub into a Log Analytics workspace.


In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create a destination table for event hub data in your Log Analytics workspace
> * Create a data collection endpoint
> * Create a data collection rule
> * Grant the data collection rule permissions to the event hub
> * Associate the data collection rule with the event hub

## Prerequisites

- [Log Analytics workspace](../logs/quick-create-workspace.md) where you have at least [contributor rights](../logs/manage-access.md#azure-rbac).
- Your Log Analytics workspace needs to be [linked to a dedicated cluster](../logs/logs-dedicated-clusters.md#link-a-workspace-to-a-cluster).
- [Event hub](/azure/event-hubs/event-hubs-create) with events.
    
    Send events to your event hub by following the steps in the [Send and receive events in Azure Event Hubs tutorials](../../event-hubs/event-hubs-create.md#next-steps) or by [configuring the diagnostic settings of Azure resources](../essentials/diagnostic-settings.md#create-diagnostic-settings).


## Create a destination table for event hub data in your Log Analytics workspace

Before you can ingest data, you need to set up a destination table. 

To create a custom table into which to ingest events, in the Azure portal:  

1. Collect workspace data:

    1. Navigate to your workspace in the **Log Analytics workspaces** menu and select **Properties** to find your subscription ID, resource group name, and workspace name.
    
        :::image type="content" source="media/ingest-logs-event-hub/create-custom-table-prepare.png" lightbox="media/ingest-logs-event-hub/create-custom-table-prepare.png" alt-text="Screenshot showing Log Analytics workspace overview screen with subscription ID, resource group name, and workspace name highlighted.":::

    1. Select **JSON** to open the **Resource JSON** screen and copy the workspace's **Resource ID**. You'll need the workspace resource ID to create a data collection rule. 
  
        :::image type="content" source="media/ingest-logs-event-hub/log-analytics-workspace-id.png" lightbox="media/ingest-logs-event-hub/log-analytics-workspace-id.png" alt-text="Screenshot showing Resource JSON screen with the workspace resource ID highlighted.":::

1. Select the **Cloud Shell** button and ensure the environment is set to **PowerShell**.

    :::image type="content" source="media/ingest-logs-event-hub/create-custom-table-open-cloud-shell.png" lightbox="media/ingest-logs-event-hub/create-custom-table-open-cloud-shell.png" alt-text="Screenshot showing how to open Cloud Shell.":::


1. Run this PowerShell command to create the table, providing the table name (`<table_name>`) in the JSON, and setting the `<subscription_id>`, `<resource_group_name>`, `<workspace_name>`, and `<table_name>` values in the `Invoke-AzRestMethod -Path`:

    ```PowerShell
    $tableParams = @'
    {
        "properties": {
            "schema": {
                "name": "<table_name>",
                "columns": [
                    {
                        "name": "TimeGenerated",
                        "type": "datetime",
                        "description": "The time at which the data was generated"
                    },
                    {
                        "name": "RawData",
                        "type": "string",
                        "description": "Body of the event"
                    },
                    {
                        "name": "Properties",
                        "type": "dynamic",
                        "description": "Additional message properties"
                    }
                ]
            }
        }
    }
    '@
    
    Invoke-AzRestMethod -Path "/subscriptions/<subscription_id>/resourcegroups/<resource_group_name>/providers/microsoft.operationalinsights/workspaces/<workspace_name>/tables/<table_name>?api-version=2021-12-01-preview" -Method PUT -payload $tableParams
    ```

> [!IMPORTANT]
> When creating a custom table, follow these naming guidelines:
> * Custom table names must have the `_CL` suffix.
> * Column names can consist of alphanumeric characters and the characters `_` and `-`. They must start with a letter. 

## Create a data collection endpoint

To collect data with a data collection rule, you need [create a data collection endpoint (DCE)](../essentials/data-collection-endpoint-overview.md#create-data-collection-endpoint). The DCE must be located in the same region as the Log Analytics Workspace where the data will be sent.

## Create a data collection rule

Azure Monitor uses [data collection rules](../essentials/data-collection-rule-overview.md) to define what data should be collected, how to transform that data, and where to send the data you collect.

To create a data collection rule in the Azure portal:

1. In the Azure portal's search box, type in *template* and then select **Deploy a custom template**.

    :::image type="content" source="media/tutorial-workspace-transformations-api/deploy-custom-template.png" lightbox="media/tutorial-workspace-transformations-api/deploy-custom-template.png" alt-text="Screenshot to deploy custom template.":::

1. Select **Build your own template in the editor**.

    :::image type="content" source="media/tutorial-workspace-transformations-api/build-custom-template.png" lightbox="media/tutorial-workspace-transformations-api/build-custom-template.png" alt-text="Screenshot to build template in the editor.":::

1. Paste the Resource Manager template below into the editor and then select **Save**.

    :::image type="content" source="media/tutorial-workspace-transformations-api/edit-template.png" lightbox="media/tutorial-workspace-transformations-api/edit-template.png" alt-text="Screenshot to edit Resource Manager template.":::

    Notice the following details in the DCR defined in this template:

    - `identity` - Defines which type of [managed identity](../../active-directory/managed-identities-azure-resources/overview.md) to use. In our example, we use system-assigned identity. You can also [configure user-assigned managed identity](#configure-user-assigned-managed-identity-optional).
    
    - `dataCollectionEndpointId` - Resource ID of the data collection endpoint.
    - `streamDeclarations` - Defines which data to ingest from event hub (incoming data). The stream declaration cannot be modified. 
       
       - `TimeGenerated` - The time at which the data was ingested from event hub to Azure Monitor Logs.
       - `RawData` - Body of the event. For more information, see [Read events](../../event-hubs/event-hubs-features.md#read-events).
       - `Properties` - User properties from the event. For more information, see [Read events](../../event-hubs/event-hubs-features.md#read-events).
    
    - `datasources` - Specifies the [event hub consumer group](../../event-hubs/event-hubs-features.md#consumer-groups) and the stream to which you ingest the data (optional).
    - `destinations` - Specifies the destination workspace.
    - `dataFlows` - Matches the stream with the destination workspace and specifies the transformation query and the destination table.
    - `transformKql` - Specifies a transformation to apply to the incoming data (stream declaration) before it's sent to the workspace. In our example, we set `transformKql` to `source`, which does not modify the data from the source in any way, because we're mapping incoming data to a custom table we've created specifically with the corresponding schema. If you're ingesting data to a table with a different schema or to filter data before ingestion, [define a data collection transformation](../essentials/data-collection-transformations.md).

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "dataCollectionRuleName": {
                "type": "string",
                "metadata": {
                    "description": "Specifies the name of the Data Collection Rule to create."
                }
            },
            "location": {
                "type": "string",
                "metadata": {
                    "description": "Specifies the location in which to create the Data Collection Rule."
                }
            },
            "workspaceName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the Log Analytics workspace to use."
                }
            },
            "workspaceResourceId": {
                "type": "string",
                "metadata": {
                    "description": "Specifies the Azure resource ID of the Log Analytics workspace to use."
                }
            },
            "endpointResourceId": {
                "type": "string",
                "metadata": {
                    "description": "Specifies the Azure resource ID of the Data Collection Endpoint to use."
                }
            },
            "tableName": { 
                "type": "string", 
                "metadata": { 
                    "description": "Specifies the name of the table in the workspace." 
                } 
            },
            "consumerGroup": {
                "type": "string",
                "metadata": {
                    "description": "Specifies consumer group of event hub. If not filled, default consumer group will be used"
                },
                "defaultValue": ""
            }
        },
        "resources": [
            {
                "type": "Microsoft.Insights/dataCollectionRules",
                "name": "[parameters('dataCollectionRuleName')]",
                "location": "[parameters('location')]",
                "apiVersion": "2022-06-01",
                "identity": {
                                 "type": "systemAssigned"
                  },
                "properties": {
                    "dataCollectionEndpointId": "[parameters('endpointResourceId')]",
                    "streamDeclarations": {
                        "Custom-MyEventHubStream": {
                            "columns": [
                    {
                        "name": "TimeGenerated",
                        "type": "datetime"
                    },
                    {
                        "name": "RawData",
                        "type": "string"
                    },
                    {
                        "name": "Properties",
                        "type": "dynamic"
                    }
                ]
                        }
                    },
                    "dataSources": {
                        "dataImports": {
                             "eventHub": {
                                        "consumerGroup": "[if(not(empty(parameters('consumerGroup')))), parameters('consumerGroup'), json('null')]",
                                        "stream": "Custom-MyEventHubStream",
                                        "name": "myEventHubDataSource1"
                                                              }
                                               }
    
                    },
                    "destinations": {
                        "logAnalytics": [
                            {
                                "workspaceResourceId": "[parameters('workspaceResourceId')]",
                                "name": "[parameters('workspaceName')]"
                            }
                        ]
                    },
                    "dataFlows": [
                        {
                            "streams": [
                                "Custom-MyEventHubStream"
                            ],
                            "destinations": [
                                "[parameters('workspaceName')]"
                            ],
                            "transformKql": "source",
                            "outputStream": "[concat('Custom-', parameters('tableName'))]" 
                        }
                    ]
                }
            }
        ]
    }
    ```
1. On the **Custom deployment** screen, specify a **Subscription** and **Resource group** to store the data collection rule and then provide values for the parameters defined in the template, including: 

    - **Region** - Region for the data collection rule. Populated automatically based on the subscription you select. 
    - **Data Collection Rule Name** - Give the rule a name.
    - **Location** - Set to be the same location as the workspace.
    - **Workspace Name** and **Workspace Resource ID** - The same values you use to [create a destination table for event hub data in your Log Analytics workspace](#create-a-destination-table-for-event-hub-data-in-your-log-analytics-workspace). 
    - **Endpoint Resource ID** - As set when you [create the data collection endpoint](#create-a-data-collection-endpoint).
    - **Table Name** - The name of the destination table. In our example, and whenever you use a custom table, the table name ends with the suffix *_CL*. 
    - **Consumer Group** - When left blank, Azure sets the [event hub consumer group](../../event-hubs/event-hubs-features.md#consumer-groups) to `$Default`. 

    :::image type="content" source="media/ingest-logs-event-hub/data-collection-rule-custom-template-deployment.png" lightbox="media/ingest-logs-event-hub/data-collection-rule-custom-template-deployment.png" alt-text="Screenshot showing the Custom Template Deployment screen with the deployment values for the data collection rule set up in this tutorial.":::

1. Select **Review + create** and then **Create** when you review the details.

1. When the deployment is complete, expand the **Deployment details** box and select your data collection rule to view its details. Select **JSON View**.

    :::image type="content" source="media/tutorial-workspace-transformations-api/data-collection-rule-details.png" lightbox="media/tutorial-workspace-transformations-api/data-collection-rule-details.png" alt-text="Screenshot for data collection rule details.":::

1. Copy the **Resource ID** for the data collection rule. You'll use this in the next step.

    :::image type="content" source="media/tutorial-workspace-transformations-api/data-collection-rule-json-view.png" lightbox="media/tutorial-workspace-transformations-api/data-collection-rule-json-view.png" alt-text="Screenshot for data collection rule JSON view.":::

    > [!NOTE]
    > All of the properties of the DCR, such as the transformation, may not be displayed in the Azure portal even though the DCR was successfully created with those properties.

### Configure user-assigned managed identity (optional)

To configure your data collection rule to support [user-assigned identity](../../active-directory/managed-identities-azure-resources/how-manage-user-assigned-managed-identities.md), in the example above, replace:

```json
                "identity": {
                                 "type": "systemAssigned"
                  },
``` 

with:

```json
                "identity": {
                        "type": "userAssigned",
                        "userAssignedIdentities": {
                            "<identity_resource_Id>": {
                                "principalId": "<principal_id>",
                                "clientId": "<client_id>"
                            }
                        }
                  },
```

In the Azure portal, go to your user-assigned managed identity resource and select **Overview** to find the `<principal_id>` and `<client_id>` values:

:::image type="content" source="media/ingest-logs-event-hub/user-assigned-managed-id.png" lightbox="media/ingest-logs-event-hub/user-assigned-managed-id.png" alt-text="Screenshot the user-assigned managed identity resource Overview screen with the Principal ID and Client ID fields highlighted.":::

## Grant the event hub permission to the data collection rule

With [managed identity](../../active-directory/managed-identities-azure-resources/overview.md), you can give any event hub permission to send events to the data collection rule and data collection endpoint you created:

1. From the event hub in the Azure portal, select **Access Control (IAM)** and then **Add role assignment**. 

    :::image type="content" source="media/tutorial-logs-ingestion-portal/add-role-assignment.png" lightbox="media/tutorial-logs-ingestion-portal/custom-log-create.png" alt-text="Screenshot for adding custom role assignment to DCR.":::

2. Select **Azure Event Hubs Data Receiver** and select **Next**.  You could instead create a custom action with the `Microsoft.Insights/Telemetry/Write` data action. 

    :::image type="content" source="media/tutorial-logs-ingestion-portal/add-role-assignment-select-role.png" lightbox="media/tutorial-logs-ingestion-portal/add-role-assignment-select-role.png" alt-text="Screenshot for selecting role for DCR role assignment.":::

3. Select **User, group, or service principal** for **Assign access to** and click **Select members**. Select your DCR and click **Select**.

    :::image type="content" source="media/tutorial-logs-ingestion-portal/add-role-assignment-select-member.png" lightbox="media/tutorial-logs-ingestion-portal/add-role-assignment-select-member.png" alt-text="Screenshot for selecting members for DCR role assignment.":::


4. Click **Review + assign** and verify the details before saving your role assignment.

    :::image type="content" source="media/tutorial-logs-ingestion-portal/add-role-assignment-save.png" lightbox="media/tutorial-logs-ingestion-portal/add-role-assignment-save.png" alt-text="Screenshot for saving DCR role assignment.":::


## Associate the data collection rule with the Event Hub

The final step is to associate the data collection rule to the event hub from which you want to collect events. 

You can associate a single data collection rule with multiple event hubs that share the same [consumer group](../../event-hubs/event-hubs-features.md#consumer-groups) and ingest data to the same stream; otherwise, create a separate rule for consumer group and stream.

To create a data collection rule association in the Azure portal:

1. In the Azure portal's search box, type in *template* and then select **Deploy a custom template**.

1. Select **Build your own template in the editor**.

1. Paste the Resource Manager template below into the editor and then select **Save**.

    ```JSON
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "eventHubResourceId": {
          "type": "string",
          "metadata": {
            "description": "Specifies the Azure resource ID of the event hub to use."
          }
        },
        "associationName": {
          "type": "string",
          "metadata": {
            "description": "The name of the association."
          }
        },
        "dataCollectionRuleId": {
          "type": "string",
          "metadata": {
            "description": "The resource ID of the data collection rule."
          }
        }
      },
      "resources": [
        {
          "type": "Microsoft.Insights/dataCollectionRuleAssociations",
          "apiVersion": "2021-09-01-preview",
          "scope": "[parameters('eventHubResourceId')]",
          "name": "[parameters('associationName')]",
          "properties": {
            "description": "Association of data collection rule. Deleting this association will break the data collection for this event hub.",
            "dataCollectionRuleId": "[parameters('dataCollectionRuleId')]"
          }
        }
      ]
    }


