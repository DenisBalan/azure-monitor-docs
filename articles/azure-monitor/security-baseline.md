---
title: Azure security baseline for Azure Monitor
description: Azure security baseline for Azure Monitor
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark

# Important: This content is machine generated; do not modify this topic directly. Contact mbaldwin for more information.

---

# Azure security baseline for Azure Monitor

The Azure Security Baseline for Azure Monitor contains recommendations that will help you improve the security posture of your deployment.

The baseline for this service is drawn from the [Azure Security Benchmark version 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview), which provides recommendations on how you can secure your cloud solutions on Azure with our best practices guidance.

For more information, see the [Azure security baselines overview](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

>[!WARNING]
>This preview version of the article is for review only. **DO NOT MERGE INTO MASTER!**

## Network security

*For more information, see [Security control: Network security](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### 1.1: Protect Azure resources within virtual networks

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21325).

**Guidance**: &lt;-------NEED Additional links-------&gt;

Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If you are using Log Analytics workspaces, configure network isolation by limiting public access for ingestion and queries. Use Log Analytics gateway to send data to a Log Analytics workspace in Azure Monitor on behalf of the computers that cannot directly connect to the internet preventing need of computers to be connected to internet.

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.2: Monitor and log the configuration and traffic of virtual networks, subnets, and NICs

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21326).

**Guidance**: &lt;-------NEED Additional links-------&gt;

Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If you are using Log Analytics workspace, Log Analytics Gateway, Azure Monitor agents, use Azure Security Center and follow network protection recommendations to help secure your network resources hosting these services in Azure. Enable NSG flow logs and send logs into a Storage Account for traffic audit. You may also send NSG flow logs to a Log Analytics Workspace and use Traffic Analytics to provide insights into traffic flow in your Azure cloud. Some advantages of Traffic Analytics are the ability to visualize network activity and identify hot spots, identify security threats, understand traffic flow patterns, and pinpoint network misconfigurations.

* [Network requirements for Azure Monitor agents](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

* [How to Enable NSG Flow Logs](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [How to Enable and use Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Understand Network Security provided by Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.3: Protect critical web applications

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21327).

**Guidance**: Not applicable; Azure Monitor is part of the Azure core services and cannot be deployed as a service separately.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.4: Deny communications with known malicious IP addresses

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21328).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If you are using Log Analytics workspace, Log Analytics Gateway, Azure Monitor agents, enable DDoS Standard protection on your Azure Virtual Networks hosting these services to guard against DDoS attacks. Use Azure Security Center Integrated Threat Intelligence to deny communications with known malicious IP addresses.

Deploy Azure Firewall at each of the organization's network boundaries with Threat Intelligence enabled and configured to "Alert and deny" for malicious network traffic.

Use Azure Security Center Just In Time Network access to configure NSGs to limit exposure of endpoints to approved IP addresses for a limited period.

Use Azure Security Center Adaptive Network Hardening to recommend NSG configurations that limit ports and source IPs based on actual traffic and threat intelligence.

* [How to configure DDoS protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [How to deploy Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Understand Azure Security Center Integrated Threat Intelligence](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

* [Understand Azure Security Center Adaptive Network Hardening](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

* [Understand Azure Security Center Just In Time Network Access Control](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.5: Record network packets

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21329).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If using Azure Monitor gateway or agents, enable Network Watcher packet capture to investigate anomalous activities.

* [Network requirements for Azure Monitor agents](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

* [How to enable Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.6: Deploy network based intrusion detection/intrusion prevention systems (IDS/IPS)

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21330).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If using Azure Monitor gateway or agents, select an offer from the Azure Marketplace that supports IDS/IPS functionality with payload inspection capabilities. If intrusion detection and/or prevention based on payload inspection is not a requirement, Azure Firewall with Threat Intelligence can be used. Azure Firewall Threat intelligence-based filtering can alert and deny traffic to and from known malicious IP addresses and domains. The IP addresses and domains are sourced from the Microsoft Threat Intelligence feed.

Deploy the firewall solution of your choice at each of your organization's network boundaries to detect and/or deny malicious traffic.

* [Network requirements for Azure Monitor agents](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

* [How to deploy Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [How to configure alerts with Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.7: Manage traffic to web applications

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21331).

**Guidance**: Not applicable; Azure Policy is part of the Azure core services and cannot be deployed as a service separately.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.8: Minimize complexity and administrative overhead of network security rules

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21332).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If using Azure Monitor gateway or agents, use Virtual Network Service Tags to define network access controls on Network Security Groups or Azure Firewall. You can use service tags in place of specific IP addresses when creating security rules. By specifying the service tag name (e.g., ApiManagement) in the appropriate source or destination field of a rule, you can allow or deny the traffic for the corresponding service. Microsoft manages the address prefixes encompassed by the service tag and automatically updates the service tag as addresses change.

You may also use Application Security Groups to help simplify complex security configuration. Application security groups enable you to configure network security as a natural extension of an application's structure, allowing you to group virtual machines and define network security policies based on those groups.

* [Network requirements for Azure Monitor agents](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

* [Understand and use Service Tags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Understand and use Application Security Groups](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.9: Maintain standard security configurations for network devices

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21333).

**Guidance**: Define and implement standard security configurations for network resources with Azure Policy.

You may also use Azure Blueprints to simplify large scale Azure deployments by packaging key environment artifacts, such as Azure Resources Manager templates, RBAC controls, and policies, in a single blueprint definition. You can apply the blueprint to new subscriptions, and fine-tune control and management through versioning.

* [How to configure and manage Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Policy samples for networking](https://docs.microsoft.com/azure/governance/policy/samples/#network)

* [How to create an Azure Blueprint](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.10: Document traffic configuration rules

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21334).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If using Azure Monitor gateway or agents, use Tags for network security groups(NSG) and other resources related to network security and traffic flow. For individual NSG rules, use the "Description" field to specify business need and/or duration (etc.) for any rules that allow traffic to/from a network.

Use any of the built-in Azure policy definitions related to tagging, such as "Require tag and its value" to ensure that all resources are created with Tags and to notify you of existing untagged resources.

You may use Azure PowerShell or Azure CLI to look-up or perform actions on resources based on their Tags.

* [Network requirements for Azure Monitor agents](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

* [How to create and use Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [How to create a Virtual Network](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [How to create an NSG with a Security Config](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 1.11: Use automated tools to monitor network resource configurations and detect changes

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21335).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If using Azure Monitor gateway or agents, use Azure Activity Log to monitor network resource configurations and detect changes to your Azure network resources. Create alerts within Azure Monitor that will trigger when changes to critical resources take place.

* [Network requirements for Azure Monitor agents](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

* [Connect computers without internet access by using the Log Analytics gateway in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/gateway)

* [How to view and retrieve Azure Activity Log events](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [How to create alerts in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

## Logging and monitoring

*For more information, see [Security control: Logging and monitoring](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### 2.1: Use approved time synchronization sources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21336).

**Guidance**: Microsoft maintains time sources for Azure resources for Azure Monitor

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Microsoft

### 2.2: Configure central security log management

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21337).

**Guidance**: Azure Monitor uses Activity logs to log changes to the service. Ingest logs via Azure Monitor to aggregate security data generated by endpoint devices, network resources, and other security systems. Within Azure Monitor, use Log Analytics Workspace(s) to query and perform analytics, and use Azure Storage Accounts for long-term/archival storage.

Alternatively, you may enable and on-board data to Azure Sentinel or a third-party SIEM.

* [How to onboard Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [How to collect platform logs and metrics with Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

* [How to collect Azure Virtual Machine internal host logs with Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

* [How to get started with Azure Monitor and third-party SIEM integration](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 2.3: Enable audit logging for Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21338).

**Guidance**: Azure Monitor uses Activity logs which are automatically enabled, include event source, date, user, timestamp, source addresses, destination addresses, and other useful elements.

* [How to collect platform logs and metrics with Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

* [Understand logging and different log types in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 2.4: Collect security logs from operating systems

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21339).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 2.5: Configure security log storage retention

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21340).

**Guidance**: Within Azure Monitor, set your Log Analytics Workspace retention period according to your organization's compliance regulations. Use Azure Storage Accounts for long-term/archival storage.

* [Change the data retention period in Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

* [How to configure retention policy for Azure Storage account logs](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 2.6: Monitor and review Logs

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21341).

**Guidance**: Analyze and monitor logs for anomalous behavior and regularly review results. Use Azure Monitor's Log Analytics Workspace to review logs and perform queries on log data.Alternatively, you may enable and on-board data to Azure Sentinel or a third party SIEM.

* [How to onboard Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Understand Log Analytics Workspace](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

* [How to perform custom queries in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 2.7: Enable alerts for anomalous activities

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21342).

**Guidance**: Use Azure Security Center with Log Analytics Workspace for monitoring and alerting on anomalous activity found in Activity logs.Alternatively, you may enable and on-board data to Azure Sentinel.

* [How to onboard Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [How to manage alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

* [How to alert on log analytics log data](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 2.8: Centralize anti-malware logging

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21343).

**Guidance**: Not applicable; Azure Monitor does not produce anti-malware related logs.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 2.9: Enable DNS query logging

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21344).

**Guidance**: Implement a third-party solution from Azure Marketplace for DNS logging solution as per your organizations need.

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 2.10: Enable command-line audit logging

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21345).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

## Identity and access control

*For more information, see [Security control: Identity and access control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### 3.1: Maintain an inventory of administrative accounts

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21346).

**Guidance**: Azure AD has built-in roles that must be explicitly assigned and are queryable. Use the Azure AD PowerShell module to perform ad hoc queries to discover accounts that are members of administrative groups.

* [Roles, permissions, and security in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security)

* [How to get a directory role in Azure AD with PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [How to get members of a directory role in Azure AD with PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.2: Change default passwords where applicable

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21347).

**Guidance**: Not applicable; Azure Policy does not have the concept of default passwords.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 3.3: Use dedicated administrative accounts

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21348).

**Guidance**: Create standard operating procedures around the use of dedicated administrative accounts. Use Azure Security Center Identity and Access Management to monitor the number of administrative accounts.

You can also enable a Just-In-Time / Just-Enough-Access by using Azure AD Privileged Identity Management Privileged Roles for Microsoft Services, and Azure Resource Manager.

* [Learn more about Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.4: Use single sign-on (SSO) with Azure Active Directory

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21349).

**Guidance**: Wherever possible, use Azure Active Directory SSO instead than configuring individual stand-alone credentials per-service. Use Azure Security Center Identity and Access Management recommendations.

* [Understand SSO with Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.5: Use multi-factor authentication for all Azure Active Directory based access

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21350).

**Guidance**: Enable Azure Active Directory multi-factor authentication(MFA) and follow Azure Security Center Identity and Access Management recommendations.

* [How to enable MFA in Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [How to monitor identity and access within Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.6: Use dedicated machines (Privileged Access Workstations) for all administrative tasks

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21351).

**Guidance**: Use PAWs (privileged access workstations) with MFA configured to log into and configure Azure resources.

* [Learn about Privileged Access Workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [How to enable MFA in Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.7: Log and alert on suspicious activities from administrative accounts

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21352).

**Guidance**: Use Azure Active Directory security reports for generation of logs and alerts when suspicious or unsafe activity occurs in the environment. Use Azure Security Center to monitor identity and access activity.

* [How to identify Azure AD users flagged for risky activity](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [How to monitor users' identity and access activity in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.8: Manage Azure resources from only approved locations

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21353).

**Guidance**: Use Conditional Access Named Locations to allow access from only specific logical groupings of IP address ranges or countries/regions.

* [How to configure Named Locations in Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.9: Use Azure Active Directory

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21354).

**Guidance**: Use Azure Active Directory (Azure AD) as the central authentication and authorization system. Azure AD protects data by using strong encryption for data at rest and in transit. Azure AD also salts, hashes, and securely stores user credentials.

* [How to create and configure an Azure AD instance](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.10: Regularly review and reconcile user access

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21355).

**Guidance**: Azure AD provides logs to help discover stale accounts. In addition, use Azure Identity Access Reviews to efficiently manage group memberships, access to enterprise applications, and role assignments. User access can be reviewed on a regular basis to make sure only the right Users have continued access.

* [Understand Azure AD reporting](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [How to use Azure Identity Access Reviews](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.11: Monitor attempts to access deactivated credentials

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21356).

**Guidance**: You have access to Azure AD Sign-in Activity, Audit and Risk Event log sources, which allow you to integrate with any SIEM/Monitoring tool.You can streamline this process by creating Diagnostic Settings for Azure Active Directory user accounts and sending the audit logs and sign-in logs to a Log Analytics Workspace. You can configure desired Alerts within Log Analytics Workspace.

* [How to integrate Azure Activity Logs into Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.12: Alert on account login behavior deviation

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21357).

**Guidance**: Use Azure AD Risk and Identity Protection features to configure automated responses to detected suspicious actions related to user identities. You can also ingest data into Azure Sentinel for further investigation.

* [How to view Azure AD risky sign-ins](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [How to configure and enable Identity Protection risk policies](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [How to onboard Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 3.13: Provide Microsoft with access to relevant customer data during support scenarios

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21358).

**Guidance**: Not applicable; Customer Lockbox not supported for Azure Monitor.

* [Supported services and scenarios in general availability](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

## Data protection

*For more information, see [Security control: Data protection](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### 4.1: Maintain an inventory of sensitive Information

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21359).

**Guidance**: Use Tags when possible to assist in tracking Azure Monitor resources that store or process sensitive information.

* [How to create and use Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 4.2: Isolate systems storing or processing sensitive information

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21360).

**Guidance**: Implement isolation using separate subscriptions and management groups for individual security domains such as environment type and data sensitivity level. You can restrict the level of access to your Azure Monitor and related resources that your applications and enterprise environments demand. You can control access to Azure Monitor via Azure Active Directory role-based access control.

* [How to create additional Azure subscriptions](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [How to create Management Groups](https://docs.microsoft.com/azure/governance/management-groups/create)

* [How to create and use tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 4.3: Monitor and block unauthorized transfer of sensitive information

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21361).

**Guidance**: Azure Monitor is part of the Azure core services and cannot be deployed as a service separately. If using Azure Monitor gateway or agents, leverage a third-party solution from Azure Marketplace on network perimeters that monitors for unauthorized transfer of sensitive information and blocks such transfers while alerting information security professionals.

For the underlying platform which is managed by Microsoft, Microsoft treats all customer content as sensitive and guard against customer data loss and exposure. To ensure customer data within Azure remains secure, Microsoft has implemented and maintains a suite of robust data protection controls and capabilities.

* [Understand customer data protection in Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Shared

### 4.4: Encrypt all sensitive information in transit

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21362).

**Guidance**: Encrypt all sensitive information in transit. Ensure that any Azure Monitor agents or gateway connecting to Log Analytics are configured to negotiate TLS 1.2 or greater.

Follow Azure Security Center recommendations for encryption at rest and encryption in transit, where applicable.

* [Sending data securely using TLS 1.2](https://docs.microsoft.com/azure/azure-monitor/platform/data-security#sending-data-securely-using-tls-12)

* [Data encryption in Application Insight](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy)

* [Understand encryption in transit with Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

**Azure Security Center monitoring**: N/A

**Responsibility**: Shared

### 4.5: Use an active discovery tool to identify sensitive data

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21363).

**Guidance**: Use a third-party active discovery tool to identify all sensitive information stored in Azure Monitor and update the organization's sensitive information inventory.

* [Understand customer data protection in Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center monitoring**: N/A

**Responsibility**: Shared

### 4.6: Use Role-based access control to control access to resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21364).

**Guidance**: Use Azure Active Directory role-based access control (RBAC) to control access to Azure Monitor.

* [Roles, permissions, and security in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security)

* [How to configure RBAC in Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 4.7: Use host-based data loss prevention to enforce access control

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21365).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Shared

### 4.8: Encrypt sensitive information at rest

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21366).

**Guidance**: The Azure Monitor data-store ensures that all data encrypted at rest using Azure-managed keys while stored in Azure Storage.

* [Log Analytics data security](https://docs.microsoft.com/azure/azure-monitor/platform/data-security)

* [Data collection, retention, and storage in Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy)

* [Understand encryption at rest in Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 4.9: Log and alert on changes to critical Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21367).

**Guidance**: Use Azure Monitor with the Azure Activity Log to create alerts for when changes take place in Azure Monitor and related resources.

* [How to create alerts for Azure Activity Log events](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

## Vulnerability management

*For more information, see [Security control: Vulnerability management](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### 5.1: Run automated vulnerability scanning tools

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21368).

**Guidance**: Not applicable; Azure Monitor is part of the Azure core services and cannot be deployed as a service separately.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 5.2: Deploy automated operating system patch management solution

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21369).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 5.3: Deploy automated patch management solution for third-party software titles

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21370).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 5.4: Compare back-to-back vulnerability scans

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21371).

**Guidance**: Not applicable; Azure Monitor is part of the Azure core services and cannot be deployed as a service separately.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 5.5: Use a risk-rating process to prioritize the remediation of discovered vulnerabilities

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21372).

**Guidance**: Use a common risk scoring program (for example, Common Vulnerability Scoring System) or the default risk ratings provided by your third-party scanning tool.

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

## Inventory and asset management

*For more information, see [Security control: Inventory and asset management](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### 6.1: Use automated Asset Discovery solution

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21373).

**Guidance**: Use Azure CLI to query/discover Azure Monitor resources within your subscription(s). Ensure appropriate (read) permissions in your tenant and enumerate all Azure subscriptions as well as resources within your subscriptions.

* [az monitor](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest)

* [How to view your Azure Subscriptions](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Understand Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

* [Roles, permissions, and security in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.2: Maintain asset metadata

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21374).

**Guidance**: Apply tags to Azure Monitor resources giving metadata to logically organize them into a taxonomy.

* [How to create and use Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.3: Delete unauthorized Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21375).

**Guidance**: Use tagging, management groups, and separate subscriptions, where appropriate, to organize and track Azure Monitor related resources.  Reconcile inventory on a regular basis and ensure unauthorized resources are deleted from the subscription in a timely manner.

* [How to create additional Azure subscriptions](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [How to create Management Groups](https://docs.microsoft.com/azure/governance/management-groups/create)

* [How to create and use Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.4: <div>Define and Maintain an inventory of approved Azure resources</div>

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21376).

**Guidance**: Create an inventory of approved Azure policies and assignments as per your organizational needs.

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.5: Monitor for unapproved Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21377).

**Guidance**: Use Azure Monitor's Log Analytics workspace to monitor for unappproved policy creation and assignment in Activity logs. You may use Azure Policy to put restrictions on the type of Azure Monitor related resources that can be created in your subscription(s).

Alternatively, you may enable and on-board data to Azure Sentinel or a third party SIEM.

* [How to configure and manage Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [How to onboard Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Understand Log Analytics Workspace](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

* [How to perform custom queries in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.6: Monitor for unapproved software applications within compute resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21378).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 6.7: Remove unapproved Azure resources and software applications

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21379).

**Guidance**: Reconcile inventory on a regular basis and ensure unauthorized Azure Monitor related resources are deleted from the subscription in a timely manner.

* [Delete Azure Log Analytics workspace](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.8: Use only approved applications

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21380).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 6.9: Use only approved Azure services

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21381).

**Guidance**: Use Azure Policy to restrict which Azure Monitor related resources you can provision in your environment.

* [How to configure and manage Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [How to deny a specific resource type with Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 6.10: Maintain an inventory of approved software titles

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21382).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 6.11: Limit users' ability to interact with Azure Resource Manager

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21383).

**Guidance**: Use Azure Conditional Access to limit users' ability to interact with Azure Resources Manager by configuring "Block access" for the "Microsoft Azure Management" App.

* [How to configure Conditional Access to block access to Azure Resources Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 6.12: Limit users' ability to execute scripts within compute resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21384).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 6.13: Physically or logically segregate high risk applications

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21385).

**Guidance**: Not applicable; this recommendation is intended for web applications running on Azure App Service or compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

## Secure configuration

*For more information, see [Security control: Secure configuration](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### 7.1: Establish secure configurations for all Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21386).

**Guidance**: Use Azure Policy aliases to create custom policies to audit or enforce the configuration of your Azure Monitor related resources. You may also use built-in Azure Policy definitions.

Also, Azure Resource Manager has the ability to export the template in Java Script Object Notation (JSON), which should be reviewed to ensure that the configurations meet / exceed the security requirements for your organization.

You may also use recommendations from Azure Security Center as a secure configuration baseline for your Azure resources.

* [How to view available Azure Policy Aliases](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Tutorial: Create and manage policies to enforce compliance](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Single and multi-resource export to a template in Azure portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [Security recommendations - a reference guide](https://docs.microsoft.com/azure/security-center/recommendations-reference)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.2: Establish secure operating system configurations

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21387).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.3: Maintain secure Azure resource configurations

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21388).

**Guidance**: Use Azure policy [deny] and [deploy if not exist] to enforce secure settings across your Azure Monitor related resources. In addition, you may use Azure Resource Manager templates to maintain the security configuration of your Azure Monitor related resources required by your organization.

* [Understand Azure Policy effects](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

* [Create and manage policies to enforce compliance](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Resource Manager templates overview](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.4: Maintain secure operating system configurations

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21389).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Shared

### 7.5: Securely store configuration of Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21390).

**Guidance**: Use Azure DevOps to securely store and manage your code like custom Azure policies, Azure Resource Manager templates. To access the resources you manage in Azure DevOps, you can grant or deny permissions to specific users, built-in security groups, or groups defined in Azure Active Directory (Azure AD) if integrated with Azure DevOps, or Active Directory if integrated with TFS.

* [How to store code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [About permissions and groups in Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 7.6: Securely store custom operating system images

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21391).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.7: Deploy configuration management tools for Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21392).

**Guidance**: Define and implement standard security configurations for Azure Monitor related resources using Azure Policy. Use Azure Policy aliases to create custom policies to audit or enforce the security configuration of your Azure Monitor related resources. You may also make use of built-in policy definitions related to your specific resources.

* [How to configure and manage Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [How to use Aliases](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.8: Deploy configuration management tools for operating systems

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21393).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.9: Implement automated configuration monitoring for Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21394).

**Guidance**: Use Azure Security Center to perform baseline scans for your Azure Monitor related resources. Additionally, use Azure Policy to alert and audit Azure resource configurations.

* [How to remediate recommendations in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.10: Implement automated configuration monitoring for operating systems

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21395).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.11: Manage Azure secrets securely

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21396).

**Guidance**: Use Managed Service Identity in conjunction with Azure Key Vault to simplify and secure secret management for supported Azure monitor related resources.

* [How to integrate with Azure Managed Identities](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

* [Services that support managed identities for Azure resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

* [How to create a Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

* [How to provide Key Vault authentication with a managed identity](https://docs.microsoft.com/azure/key-vault/managed-identity)

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 7.12: Manage identities securely and automatically

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21397).

**Guidance**: &lt;--------NEED TO VERIFY--------&gt;

* [Use Managed Identities to provide Azure services with an automatically managed identity in Azure AD. Managed Identities allows you to authenticate to any service that supports Azure AD authentication, including Azure Policy, without any credentials in your code.How to configure Managed Identities](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 7.13: Eliminate unintended credential exposure

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21398).

**Guidance**: Implement Credential Scanner to identify credentials within code. Credential Scanner will also encourage moving discovered credentials to more secure locations such as Azure Key Vault.

* [How to setup Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

## Malware defense

*For more information, see [Security control: Malware defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### 8.1: Use centrally managed anti-malware software

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21399).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

### 8.2: Pre-scan files to be uploaded to non-compute Azure resources

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21400).

**Guidance**: Microsoft Antimalware is enabled on the underlying host that supports Azure services (for example, Azure Monitor related resources), however it does not run on your content.

Pre-scan any files being uploaded to applicable Azure Monitor related resources, such as Log Analytics workspace.

Use Azure Security Center's Threat detection for data services to detect malware uploaded to storage accounts.

* [Understand Microsoft Antimalware for Azure Cloud Services and Virtual Machines](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Understand Azure Security Center's Threat detection for data services](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 8.3: Ensure anti-malware software and signatures are updated

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21401).

**Guidance**: Not applicable; this recommendation is intended for compute resources.

**Azure Security Center monitoring**: Not applicable

**Responsibility**: Customer

## Data recovery

*For more information, see [Security control: Data recovery](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### 9.1: Ensure regular automated back ups

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21402).

**Guidance**: Use Azure Resource Manager to export the Azure Monitor and related resources in a Java Script Object Notation (JSON) template which can be used as backup for Azure Monitor and related configurations. Use Azure Automation to run the backup scripts automatically.

* [Manage Log Analytics workspace using Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration)

* [Single and multi-resource export to a template in Azure portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [About Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 9.2: Perform complete system backups and backup any customer managed keys

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21403).

**Guidance**: Use Azure Resource Manager to export the Azure Monitor and related resources in a Java Script Object Notation (JSON) template which can be used as backup for Azure Monitor and related configurations. Backup customer managed keys within Azure Key Vault if Azure Monitor related resources are using customer managed keys,

* [Manage Log Analytics workspace using Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration)

* [Single and multi-resource export to a template in Azure portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [How to backup key vault keys in Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 9.3: Validate all backups including customer managed keys

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21404).

**Guidance**: Ensure ability to periodically perform restoration using Azure Resource Manager backed template files. Test restoration of backed up customer managed keys.

* [Manage Log Analytics workspace using Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration)

* [How to restore key vault keys in Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 9.4: Ensure protection of backups and customer managed keys

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21405).

**Guidance**: Use Azure DevOps to securely store and manage your code like custom Azure policies, Azure Resource Manager templates. To protect resources you manage in Azure DevOps, you can grant or deny permissions to specific users, built-in security groups, or groups defined in Azure Active Directory (Azure AD) if integrated with Azure DevOps, or Active Directory if integrated with TFS.  Use role-based access control to protect customer managed keys.

Additionally, Enable Soft-Delete and purge protection in Key Vault to protect keys against accidental or malicious deletion. If Azure Storage is used to store Azure Resource Manager template backups, enable soft delete to save and recover your data when blobs or blob snapshots are deleted.

* [How to store code in Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [About permissions and groups in Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

* [How to enable Soft-Delete and Purge protection in Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

* [Soft delete for Azure Storage blobs](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

## Incident response

*For more information, see [Security control: Incident response](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### 10.1: Create an incident response guide

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21406).

**Guidance**: Build out an incident response guide for your organization. Ensure that there are written incident response plans that define all roles of personnel as well as phases of incident handling/management from detection to post-incident review.

* [Guidance on building your own security incident response process](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Security Response Center's Anatomy of an Incident](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [Leverage NIST's Computer Security Incident Handling Guide to aid in the creation of your own incident response plan](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 10.2: Create an incident scoring and prioritization procedure

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21407).

**Guidance**: Security Center assigns a severity to each alert to help you prioritize which alerts should be investigated first. The severity is based on how confident Security Center is in the finding or the analytic used to issue the alert as well as the confidence level that there was malicious intent behind the activity that led to the alert.

Additionally, clearly mark subscriptions (for ex. production, non-prod) using tags and create a naming system to clearly identify and categorize Azure resources, especially those processing sensitive data. It is your responsibility to prioritize the remediation of alerts based on the criticality of the Azure resources and environment where the incident occurred.

* [Security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

* [Use tags to organize your Azure resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 10.3: Test security response procedures

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21408).

**Guidance**: Conduct exercises to test your systems’ incident response capabilities on a regular cadence to help protect your Azure resources. Identify weak points and gaps and revise plan as needed.

* [NIST's publication - Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 10.4: Provide security incident contact details and configure alert notifications for security incidents

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21409).

**Guidance**: Security incident contact information will be used by Microsoft to contact you if the Microsoft Security Response Center (MSRC) discovers that your data has been accessed by an unlawful or unauthorized party. Review incidents after the fact to ensure that issues are resolved.

* [How to set the Azure Security Center Security Contact](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 10.5: Incorporate security alerts into your incident response system

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21410).

**Guidance**: Export your Azure Security Center alerts and recommendations using the Continuous Export feature to help identify risks to Azure resources. Continuous Export allows you to export alerts and recommendations either manually or in an ongoing, continuous fashion. You may use the Azure Security Center data connector to stream the alerts to Azure Sentinel.

* [How to configure continuous export](https://docs.microsoft.com/azure/security-center/continuous-export)

* [How to stream alerts into Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

### 10.6: Automate the response to security alerts

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21411).

**Guidance**: Use the Workflow Automation feature in Azure Security Center to automatically trigger responses via "Logic Apps" on security alerts and recommendations to protect your Azure resources.

* [How to configure Workflow Automation and Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Security Center monitoring**: N/A

**Responsibility**: Customer

## Penetration tests and red team exercises

*For more information, see [Security control: Penetration tests and red team exercises](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### 11.1: Conduct regular penetration testing of your Azure resources and ensure remediation of all critical security findings

>[!NOTE]
> To revise the text in this section, update the [underlying Work Item](https://dev.azure.com/AzureSecurityControlsBenchmark/AzureSecurityControlsBenchmarkContent/_queries/edit/21412).

**Guidance**: Follow the Microsoft Rules of Engagement to ensure your Penetration Tests are not in violation of Microsoft policies. Use Microsoft’s strategy and execution of Red Teaming and live site penetration testing against Microsoft-managed cloud infrastructure, services, and applications.

* [Penetration Testing Rules of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center monitoring**: N/A

**Responsibility**: Shared

## Next steps

- See the [Azure security benchmark](https://docs.microsoft.com/azure/security/benchmarks/overview)
- Learn more about [Azure security baselines](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
