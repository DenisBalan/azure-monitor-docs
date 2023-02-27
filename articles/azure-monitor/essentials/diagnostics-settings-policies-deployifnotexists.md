---
title: Enable Diagnostics settings by category group using built-in policies.
description: Use Azure builtin policies to create diagnostic settings in Azure Monitor.
author: EdB-MSFT
ms.author: edbaynash
services: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2023
ms.reviewer: lualderm
--- 

# Built-in policies for Azure Monitor
Policies and policy initiatives provide a simple method to enable logging at-scale via diagnostics settings for Azure Monitor. Using a policy initiative, you can turn on audit logging for all [supported resources](#supported-resources) in your Azure environment.  

Enable resource logs to track activities and events that take place on your resources and give you visibility and insights into any changes that occur.
Assign policies to enable resource logs and to send them to destinations according to your needs. Send logs to Event Hubs for third-party SIEM systems, enabling continuous security operations. Send logs to storage accounts for longer term storage or the fulfillment of regulatory compliance. 

A set of built-in policies and initiatives exists to direct resource logs to Log Analytics Workspaces, Event Hubs, and Storage Accounts.

The policies enable audit logging, sending logs belonging to the **audit** log category group to an Event Hub, Log Analytics workspace or Storage Account.

The policies' `effect` is  `DeployIfNotExists`, which deploys the policy as a default if there aren't other settings defined.


## Deploy policies.
Deploy the policies and initiatives using the Portal, CLI, PowerShell, or Azure Resource Management templates
### [Azure portal](#tab/portal)

The following steps show how to apply the policy to send audit logs to for key vaults to a log analytics workspace.

1. From the Policy page, select **Definitions**.

1. Select your scope. You can apply a policy to the entire subscription, a resource group, or an individual resource.
1. From the **Definition type** dropdown, select **Policy**.
1. Select **Monitoring** from the Category dropdown
1. Enter *keyvault* in the **Search** field.
1. Select the **Enable logging by category group for Key vaults (microsoft.keyvault/vaults) to Log Analytics** policy,
    :::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/policy-definitions.png" alt-text="A screenshot of the policy definitions page":::
1. From the policy definition page, select **Assign**
1. Select the **Parameters** tab.
1. Select the Log Analytics Workspace that you want to send the audit logs to.
1. Select the **Remediation** tab.
 :::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/assign-policy-parameters.png" alt-text="A screenshot of the assign policy page, parameters tab.":::
1. On the remediation tab, select the keyvault policy from the **Policy to remediate** dropdown.
1. Select the **Create a Managed Identity** checkbox.
1. Under **Type of Managed Identity**, select **System assigned Managed Identity**.
1. Select **Review + create**, then select **Create** .
  :::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/assign-policy-remediation.png" alt-text="A screenshot of the assign policy page, remediation tab.":::

The policy visible in the resources' diagnostic setting after approximately 30 minutes.

### [CLI](#tab/cli)
To apply a policy using the CLI, use the following commands:

1. Create a policy assignment using 
```azurecli

  az policy assignment create --name <policy assignment name>  --policy "6b359d8f-f88d-4052-aa7c-32015963ecc1"  --scope </subsciption/12345687-abcf-....> --params "{\"logAnalytics\": {\"value\": \"<log analytics workspace resource ID"}}" --mi-system-assigned --location <location>
```
For example, to apply the policy to send audit logs to a log analytics workspace

```azurecli
  az policy assignment create --name "policy-assignment-1"  --policy "6b359d8f-f88d-4052-aa7c-32015963ecc1"  --scope /subscriptions/12345678-aaaa-bbbb-cccc-1234567890ab/resourceGroups/rg-001 --params "{\"logAnalytics\": {\"value\": \"/subscriptions/12345678-aaaa-bbbb-cccc-1234567890ab/resourcegroups/rg-001/providers/microsoft.operationalinsights/workspaces/workspace001\"}}" --mi-system-assigned --location eastus
```

2. Assign the required role to the identity created for the policy assignment.
Find the role in the policy definition by searching for *role*

```json
       ...},
          "roleDefinitionIds": [
            "/providers/Microsoft.Authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293"
          ],
          "deployment": {
            "properties": {...
```

```azurecli
az policy assignment identity assign --system-assigned -g <resource group name> --role <role name or ID> --identity-scope </scope> -n <policy assignment name>
```
For example:

```azurecli
az policy assignment identity assign --system-assigned -g rg-001  --role 92aaf0da-9dab-42b6-94a3-d43ce8d16293 --identity-scope /subscriptions/12345678-aaaa-bbbb-cccc-1234567890ab/resourceGroups/rg001 -n policy-assignment-1
```

3. Create a remediation task to apply the policy to existing resources.

```azurecli
az policy remediation create -g <resource group name> --policy-assignment <policy assignment name> --name <remediation name> 
```

For example,
```azurecli
az policy remediation create -g rg-001 -n remediation-001 --policy-assignment  policy-assignment-1
```

For more information on policy assignment using CLI, see [Azure CLI reference - az policy assignment](https://learn.microsoft.com/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create)
### [PowerShell](#tab/Powershell)

To apply a policy using the PowerShell, use the following commands:

1. Set up your environment.
Select your subscription and set your resource group
```azurepowershell
    Select-AzSubscription <subscriptionID>
    $rg = Get-AzResourceGroup -Name <resource groups name>    
```

1. Get the policy defintiion and configure the parameters for the policy. In the example below we assign the policy to send keyVault logs to a Log Analytics workspace
```azurepowershell
  $definition = Get-AzPolicyDefinition |Where-Object Name -eq 6b359d8f-f88d-4052-aa7c-32015963ecc1
  $params =  @{"logAnalytics"="/subscriptions/<subscriptionID/resourcegroups/<resourcgroup>/providers/microsoft.operationalinsights/workspaces/<log anlaytics workspace name>"}  
 ```

1. Assign the policy 
```azurepowershell
$policyAssignment=New-AzPolicyAssignment -Name <assignment name> -DisplayName "assignment display name" -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyparameterObject $params -IdentityType 'SystemAssigned' -Location <location>
 
#To get your assignemnt use:
$policyAssignment=Get-AzPolicyAssignment -Name '<assignment name>' -Scope '/subscriptions/<subscriptionID>/resourcegroups/<resource group name>'

```

1. Assign the required role or roles to the system assigned Managed Identity
 ```azurepowershell
    $principalID=$policyAssignment.Identity.PrincipalId
    $roleDefinitionIds=$definition.Properties.policyRule.then.details.roleDefinitionIds
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $rg.ResourceId -ObjectId $policyAssignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
```

 Start-AzPolicyComplianceScan -ResourceGroupName $rg.ResourceGroupName
1. Scan for compliance, then  create a remediation task to force compliance for existing resources.
```azurepowershell
    Start-AzPolicyComplianceScan -ResourceGroupName $rg.ResourceGroupName
    Start-AzPolicyRemediation -Name $policyAssignment.Name -PolicyAssignmentId $policyAssignment.PolicyAssignmentId  -ResourceGroupName $rg.ResourceGroupName
```

1. Check compliance 
```azurepowershell
Get-AzPolicyState -PolicyAssignmentName  $policyAssignment.Name -ResourceGroupName $policyAssignment.ResourceGroupName|select-object IsCompliant , ResourceID
```



---
## Assign initiatives

Initiatives are collections of policies. There are three initiatives for Azure Monitor Diagnostics settings:
+ [Enable audit category group resource logging for supported resources to Event Hub](https://portal.azure.com/?feature.customportal=false&feature.canmodifystamps=true&Microsoft_Azure_Monitoring_Logs=stage1&Microsoft_OperationsManagementSuite_Workspace=stage1#view/Microsoft_Azure_Policy/InitiativeDetailBlade/id/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F1020d527-2764-4230-92cc-7035e4fcf8a7/scopes~/%5B%22%2Fsubscriptions%2Fd0567c0b-5849-4a5d-a2eb-5267eae1bbc7%22%5D)
+ [Enable audit category group resource logging for supported resources to Log Analytics](https://portal.azure.com/?feature.customportal=false&feature.canmodifystamps=true&Microsoft_Azure_Monitoring_Logs=stage1&Microsoft_OperationsManagementSuite_Workspace=stage1#view/Microsoft_Azure_Policy/InitiativeDetailBlade/id/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Ff5b29bc4-feca-4cc6-a58a-772dd5e290a5/scopes~/%5B%22%2Fsubscriptions%2Fd0567c0b-5849-4a5d-a2eb-5267eae1bbc7%22%5D)
+ [Enable audit category group resource logging for supported resources to storage](https://portal.azure.com/?feature.customportal=false&feature.canmodifystamps=true&Microsoft_Azure_Monitoring_Logs=stage1&Microsoft_OperationsManagementSuite_Workspace=stage1#view/Microsoft_Azure_Policy/InitiativeDetailBlade/id/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F8d723fb6-6680-45be-9d37-b1a4adb52207/scopes~/%5B%22%2Fsubscriptions%2Fd0567c0b-5849-4a5d-a2eb-5267eae1bbc7%22%5D)

In this example, we assign an initiative for sending audit logs to a Log Analytics workspace.

### [Azure portal](#tab/portal)

1. From the policy **Definitions** page, select your scope.

1. Select *Initiative* in the **Definition type** dropdown.
1. Select *Monitoring* in the **Category** dropdown.
1. Enter *audit* in the **Search** field.
1. Select thee *Enable audit category group resource logging for supported resources to Log Analytics* initiative.
1. On the following page, select **Assign**
:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/initiatives-definitions.png" alt-text="A screenshot showing the initiatives definitions page.":::

1. On the **Basics** tab of the **Assign initiative** page, select a **Scope** that you want the initiative to apply to.
1. Enter a name in the **Assignment name** field.
1. Select the **Parameters** tab.
:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/assign-initiatives-basics.png" alt-text="A screenshot showing the assign initiatives basics tab":::  

    The **Parameters** contains the parameters defined in the policy. In this case, we need to select the Log Analytics workspace that we want to send the logs to. For more information in the individual parameters for each policy, see [Policy-specific parameters](#policy-specific-parameters).

1. Select the **Log Analytics workspace** to send your audit logs to.

1. Select **Review + create** then **Create**
:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/assign-initiatives-parameters.png" alt-text="A screenshot showing the assign initiatives parameters tab":::

To verify that your policy or initiative assignment is working, create a resource in the subscription or resource group scope that you defined in your policy assignment.

After 10 minutes, select the **Diagnostics settings** page for your resource.
Your diagnostic setting appears in the list with the default name *setByPolicy-LogAnalytics and the workspace name that you configured in the policy.

:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/diagnostics-settings.png" alt-text="A screenshot showing the Diagnostics setting page for a resource.":::

Change the default name in the **Parameters** tab of the **Assign initiative** or policy page by unselecting the **Only show parameters that need input or review** checkbox.

:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/edit-initiative-assignment.png" alt-text="A screenshot showing the edit-initiative-assignment page with the checkbox unselected.":::

### [CLI](#tab/cli)
TBD

### [PowerShell](#tab/Powershell)

$subscriptionId = "d0567c0b-5849-4a5d-a2eb-5267eae1bbc7";
Select-AzSubscription $subscriptionId;
$groupName= "ed-ps-initiative-03";
$rg = Get-AzResourceGroup -Name $groupName;
$definition = Get-AzPolicySetDefinition |Where-Object ResourceID -eq /providers/Microsoft.Authorization/policySetDefinitions/f5b29bc4-feca-4cc6-a58a-772dd5e290a5;
$assignmentName="assign-ps-initiative-03-03";
$params =  @{"logAnalytics"="/subscriptions/$subscriptionId/resourcegroups/$($rg.ResourceGroupName)/providers/microsoft.operationalinsights/workspaces/ed-psi-02-workspace"}  
$policyAssignment=Get-AzPolicyAssignment -Name $assignmentName -Scope "/subscriptions/$subscriptionId/resourcegroups/$($rg.ResourceGroupName)";


$policyAssignment=New-AzPolicyAssignment -Name $assignmentName  -Scope $rg.ResourceId -PolicySetDefinition $definition -PolicyparameterObject $params -IdentityType 'SystemAssigned' -Location eastus;


New-AzRoleAssignment -Scope $rg.ResourceId -ObjectId $policyAssignment.Identity.PrincipalId -RoleDefinitionName Contributor;


Start-AzPolicyComplianceScan -ResourceGroupName $rg.ResourceGroupName;
#$policyAssignment=Get-AzPolicyAssignment -Name $assignmentName -Scope "/subscriptions/$subscriptionId/resourcegroups/$($rg.ResourceGroupName)";

$assignmentState=Get-AzPolicyState -PolicyAssignmentName  $assignmentName -ResourceGroupName $rg.ResourceGroupName   

$policyAssignmentId=$assignmentState.PolicyAssignmentId[0]

$policyDefinitionReferenceIds=$assignmentState.PolicyDefinitionReferenceId 

$policyDefinitionReferenceIds | ForEach-Object {
  $referenceId = $_
  Start-AzPolicyRemediation -ResourceGroupName $rg.ResourceGroupName  -PolicyAssignmentId $policyAssignmentId   -PolicyDefinitionReferenceId $referenceId -Name "$($rg.ResourceGroupName) remediation $referenceId"
}


Get-AzPolicyState -PolicyAssignmentName  $assignmentName -ResourceGroupName $rg.ResourceGroupName|select-object IsCompliant , ResourceID



## Remediation tasks

Policies are applied to new resources when they're created. To apply a policy to existing resources, create a remediation task. Remediation tasks bring resources into compliance with a policy.

Remediation tasks act for specific policies. For initiatives that contain multiple policies, create a remediation task for each policy in the initiative where you have resources that you want to bring into compliance.

 Define remediation tasks when you first assign the policy, or at any stage after assignment. 

To create a remediation task for policies during the policy assignment, select the **Remediation** tab on **Assign policy** page and select the **Create remediation task** checkbox.

To create a remediation task after the policy has been assigned, select your assigned policy from the list on the Policy Assignments page.
 
:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/remediation-after-assignment.png" alt-text="A screenshot showing the edit-initiative-assignment page with the checkbox unselected.":::

Select **Remediate**.
Track the status of your remediation task in the **Remediation tasks** tab of the Policy Remediation page.

:::image type="content" source="./media/diagnostics-settings-policies-deployifnotexists/new-remediation-task-after-assignment.png" alt-text="A screenshot showing the new remediation task page.":::




For more information on remediation tasks, see [Remediate non-compliant resources](../../governance/policy/how-to/remediate-resources.md)





## Common parameters

The following table describes the common parameters for each set of policies.

|Parameter| Description| Valid Values|Default|
|---|---|---|---|
|effect| Enable or disable the execution of the policy|DeployIfNotExists,<br>AuditIfNotExists,<br>Disabled|DeployIfNotExists|
|diagnosticSettingName|Diagnostic Setting Name||setByPolicy-LogAnalytics|
|categoryGroup|Diagnostic category group|none,<br>audit,<br>allLogs|audit|

## Policy-specific parameters
### Log Analytics policy parameters
 This policy deploys a diagnostic setting using a category group to route logs to a Log Analytics workspace.

|Parameter| Description| Valid Values|Default|
|---|---|---|---|
|resourceLocationList|Resource Location List to send logs to nearby Log Analytics. <br>"*" selects all locations|Supported locations|\*|
|logAnalytics|Log Analytics Workspace|||

### Event Hubs policy parameters

This policy deploys a diagnostic setting using a category group to route logs to an Event Hub.

|Parameter| Description| Valid Values|Default|
|---|---|---|---|
|resourceLocation|Resource Location must be the same location as the event hub Namespace|Supported locations||
|eventHubAuthorizationRuleId|Event Hub Authorization Rule ID. The authorization rule is at event hub namespace level. For example, /subscriptions/{subscription ID}/resourceGroups/{resource group}/providers/Microsoft.EventHub/namespaces/{Event Hub namespace}/authorizationrules/{authorization rule}|||
|eventHubName|Event Hub Name||Monitoring|


### Storage Accounts policy parameters
This policy deploys a diagnostic setting using a category group to route logs to a Storage Account.

|Parameter| Description| Valid Values|Default|
|---|---|---|---|
|resourceLocation|Resource Location must be in the same location as the Storage Account|Supported locations|
|storageAccount|Storage Account resourceId|||

## Supported Resources

Built-in Audit logs policies for Log Analytics workspaces, Event Hubs, and Storage Accounts exist for the following resources:

* microsoft.agfoodplatform/farmbeats
* microsoft.apimanagement/service
* microsoft.appconfiguration/configurationstores
* microsoft.attestation/attestationproviders
* microsoft.automation/automationaccounts
* microsoft.avs/privateclouds
* microsoft.cache/redis
* microsoft.cdn/profiles
* microsoft.cognitiveservices/accounts
* microsoft.containerregistry/registries
* microsoft.devices/iothubs
* microsoft.eventgrid/topics
* microsoft.eventgrid/domains
* microsoft.eventgrid/partnernamespaces
* microsoft.eventhub/namespaces
* microsoft.keyvault/vaults
* microsoft.keyvault/managedhsms
* microsoft.machinelearningservices/workspaces
* microsoft.media/mediaservices
* microsoft.media/videoanalyzers
* microsoft.netapp/netappaccounts/capacitypools/volumes
* microsoft.network/publicipaddresses
* microsoft.network/virtualnetworkgateways
* microsoft.network/p2svpngateways
* microsoft.network/frontdoors
* microsoft.network/bastionhosts
* microsoft.operationalinsights/workspaces
* microsoft.purview/accounts
* microsoft.servicebus/namespaces
* microsoft.signalrservice/signalr
* microsoft.signalrservice/webpubsub
* microsoft.sql/servers/databases
* microsoft.sql/managedinstances

## Next Steps

* [Create diagnostic settings at scale using Azure Policy](./diagnostic-settings-policy.md)
* [Azure Policy built-in definitions for Azure Monitor](../policy-reference.md)
* [Azure Policy Overview](../../governance/policy/overview.md)
* [Azure Enterprise Policy as Code](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/azure-enterprise-policy-as-code-a-new-approach/ba-p/3607843)