---
title: Deploy Azure Monitor - Alerts and automated actions
description: Recommendations for deployment of Azure Monitor alerts and automated actions.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/28/2021

---

# Deploy Azure Monitor - Alerts and automated actions
Alerts in Azure Monitor proactively notify you of important data or patterns identified in your monitoring data. Some insights will generate alerts without configuration. For other scenarios, you need to create [alert rules](alerts/alerts-overview.md) that include the data to analyze and the criteria for when to generate an alert, and action groups which define the action to take when an alert is generated. 

This article provides guidance on defining and creating alert rules in Azure Monitor.
## Alerting strategy
An alerting strategy defines your organizations standards for the types of alert rules that you'll create for different scenarios, how you'll categorize and manage alerts after they're created, and automated actions and notifications that you'll take in response to alerts. See [Successful alerting strategy](/azure/cloud-adoption-framework/manage/monitor/alerting#successful-alerting-strategy) for factors that you should consider in developing an alerting strategy.


## Alert rules
Alerts are created by alert rules. Azure Monitor does not have any alert rules by default, so you have to create them. See the monitoring documentation for each Azure service for guidance on recommended alert rules to create. 

There are multiple types of alert rules defined by the type of data that they use. Each has different capabilities and a different cost. The basic strategy you should follow is to use the alert rule type with the lowest cost that provides the logic that you require.

- [Activity log rules](alerts/activity-log-alerts.md). Creates an alert in response to a new Activity log event that matches specified conditions. There is no cost to these alerts so they should be your first choice. See [Create, view, and manage activity log alerts by using Azure Monitor](alerts/alerts-activity-log.md) for details on creating an Activity log alert.
- [Metric alert rules](alerts/alerts-metric-overview.md). Creates an alert in response to one or more metric values exceeding a threshold. Metric alerts are stateful meaning that the alert will automatically close when the value drops below the threshold, and it will only send out notifications when the state changes. There is a cost to metric alerts, but this is significantly less than log alerts. See [Create, view, and manage metric alerts using Azure Monitor](alerts/alerts-metric.md) for details on creating a metric alert.
- [Log alert rules](alerts/alerts-unified-log.md). Creates an alert when the results of a schedule query matches specified criteria. They are the most expensive of the alert rules, but they allow the most complex criteria. See [Create, view, and manage log alerts using Azure Monitor](alerts/alerts-log.md) for details on creating a log query alert.
- [Application alerts](app/monitor-web-app-availability.md) allow you to perform proactive performance and availability testing of your web application. You can perform a simple ping test at no cost, but there is a cost to more complex testing. See [Monitor the availability of any website](app/monitor-web-app-availability.md) for a description of the different tests and details on creating them.


## Action groups
Automated responses to alerts are defined in [action groups](alerts/action-groups.md). An action group is a collection of one or more notifications and actions that are fired when an alert is triggered. A single action group can be used with multiple alert rules. 

- Notifications. Messages that notify operators and administrators that an alert was created.
- Actions. Automated processes that attempt to correct the detected issue, 
## Notifications
Notifications are messages sent to one or more users to notify them that an alert has been created. Since a single action group can be used with multiple alert rules, you should design a set of action groups for different sets of administrators and users who will receive the same sets of alerts. Use any of the following types of notifications depending on the preferences of your operators and your organizational standards.

- Email
- SMS
- Push to Azure app
- Voice
- Email Azure Resource Manager Role

## Actions
Actions are automated responses to an alert. You can use the available actions for any scenario that they support, but the following sections describe how each are typically used.

### Automated remediation
Use the following actions to attempt automated remediation of the issue identified by the alert. 

- Automation runbook - Start either a built-in or custom a runbook in Azure Automation. For example, built-in runbooks are available to perform such functions as restarting or scaling up a virtual machine.
- Azure Function - Start an Azure Function.


### ITSM and on-call management

- ITSM - Use the [ITSM connector]() to create work items in your ITSM tool based on alerts from Azure Monitor. You first configure the connector and then use the **ITSM** action in alert rules.
- Webhooks - Send the alert to an incident management system that supports webhooks such as PagerDuty and Splunk On-Call.
- Secure webhook - ## ITSM connector


## Minimizing alert activity
Use the following guidelines to minimize your alert activity to ensure that critical issues are surfaced while you don't generate excess information and notifications for administrators. 

- See [Successful alerting strategy](/azure/cloud-adoption-framework/manage/monitor/alerting#successful-alerting-strategy) for principles on determining whether a symptom is an appropriate candidate for alerting.
- Use metric alerts for better performance and latency. Use log query alerts when metrics can't be used or for more complex logic.
- Use **Suppress alerts** option in log query alert rules which prevents creating multiple alerts for the same issue.

## Alert severity
Every alert has a severity level that you define in the 

## Create alert rules at scale
