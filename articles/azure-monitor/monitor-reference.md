---
title: What is monitored by Azure Monitor
description: Reference of all services and other resources monitored by Azure Monitor.
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 07/15/2021

---

# What is monitored by Azure Monitor?

This article describes the different applications and services that are monitored by Azure Monitor. 

## Insights

<<<<<<< HEAD
Some services have a dedicated customized monitoring experience called an "insight". Insights are a feature of Azure Monitor which provide a starting point for common services.  They collect and analyze a subset of logs and metrics for that resource. Depending on the insight, it may also provide out-of-the-box alerting. Insights are considered part of Azure Monitor and follow the support and service level agreements for Azure. They are supported in all Azure regions where Azure Monitor is available.
=======
| Insight | Description |
|:---|:---|
| [Application Insights](app/app-insights-overview.md) | Extensible Application Performance Management (APM) service to monitor your live web application on any platform. |
| [Container insights](containers/container-insights-overview.md) | Monitors the performance of container workloads deployed to either Azure Container Instances or managed Kubernetes clusters hosted on Azure Kubernetes Service (AKS). |
| [Cosmos DB insights](insights/cosmosdb-insights-overview.md) | Provides a view of the overall performance, failures, capacity, and operational health of all your Azure Cosmos DB resources in a unified interactive experience. |
| [Networks insights (preview)](insights/network-insights-overview.md) | Provides a comprehensive view of health and metrics for all your network resource. The advanced search capability helps you identify resource dependencies, enabling scenarios like identifying resource that are hosting your website, by simply searching for your website name. |
[Resource Group insights (preview)](insights/resource-group-insights.md) |  Triage and diagnose any problems your individual resources encounter, while offering context as to the health and performance of the resource group as a whole. |
| [Storage insights](../storage/common/storage-insights-overview.md?toc=%2fazure%2fazure-monitor%2ftoc.json) | Provides comprehensive monitoring of your Azure Storage accounts by delivering a unified view of your Azure Storage services performance, capacity, and availability. |
| [VM insights](vm/vminsights-overview.md) | Monitors your Azure virtual machines (VM) and virtual machine scale sets at scale. It analyzes the performance and health of your Windows and Linux VMs, and monitors their processes and dependencies on other resources and external processes. |
| [Key Vault insights (preview)](./insights/key-vault-insights-overview.md) | Provides comprehensive monitoring of your key vaults by delivering a unified view of your Key Vault requests, performance, failures, and latency. |
| [Azure Cache for Redis insights (preview)](insights/redis-cache-insights-overview.md) |  Provides a unified, interactive view of overall performance, failures, capacity, and operational health. |
>>>>>>> 5f0bd44e00905371e9b0f84c839d2a8c279649f8

For a list of available insights, see [Overview of Azure Monitor - Insights](overview.md#insights)

## Product integrations

The services and solutions in the following table store their data in a Log Analytics workspace so that it can be analyzed with other log data collected by Azure Monitor.

| Product/Service | Description |
|:---|:---|
| [Azure Automation](../automation/index.yml) | Manage operating system updates and track changes on Windows and Linux computers. See [Change Tracking](../automation/change-tracking/overview.md) and [Update Management](../automation/update-management/overview.md). |
| [Azure Information Protection ](/azure/information-protection/) | Classify and optionally protect documents and emails. See [Central reporting for Azure Information Protection](/azure/information-protection/reports-aip#configure-a-log-analytics-workspace-for-the-reports). |
| [Azure Security Center](../security-center/index.yml) | Collect and analyze security events and perform threat analysis. See [Data collection in Azure Security Center](../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](../sentinel/index.yml) | Connects to different sources including Office 365 and Amazon Web Services Cloud Trail. See [Connect data sources](../sentinel/connect-data-sources.md). |
| [Microsoft Intune](/intune/) | Create a diagnostic setting to send logs to Azure Monitor. See [Send log data to storage, event hubs, or log analytics in Intune (preview)](/intune/fundamentals/review-logs-using-azure-monitor).  |
| Network  | [Network Performance Monitor](insights/network-performance-monitor.md) - Monitor network connectivity and performance to service and application endpoints.<br>[Azure Application Gateway](insights/azure-networking-analytics.md#azure-application-gateway-analytics) - Analyze logs and metrics from Azure Application Gateway.<br>[Traffic Analytics](../network-watcher/traffic-analytics.md) - Analyzes Network Watcher network security group (NSG) flow logs to provide insights into traffic flow in your Azure cloud. |
| [Office 365](insights/solution-office-365.md) | Monitor your Office 365 environment. Updated version with improved onboarding available through Azure Sentinel. |
| [SQL Analytics](insights/azure-sql.md) | Monitor performance of Azure SQL Databases and SQL Managed Instances at scale and across multiple subscriptions. |
| [Surface Hub](insights/surface-hubs.md) | Track the health and usage of Surface Hub devices. |
| [System Center Operations Manager](/system-center/scom) | Collect data from Operations Manager agents by connecting their management group to Azure Monitor. See [Connect Operations Manager to Azure Monitor](agents/om-agents.md)<br> Assess the risk and health of your System Center Operations Manager management group with [Operations Manager Assessment](insights/scom-assessment.md) solution. |
| [Microsoft Teams Rooms](/microsoftteams/room-systems/azure-monitor-deploy) | Integrated, end-to-end management of Microsoft Teams Rooms devices. |
| [Visual Studio App Center](/appcenter/) | Build, test, and distribute applications and then monitor their status and usage. See [Start analyzing your mobile app with App Center and Application Insights](app/mobile-center-quickstart.md). |
| Windows | [Windows Update Compliance](/windows/deployment/update/update-compliance-get-started) - Assess your Windows desktop upgrades.<br>[Desktop Analytics](/configmgr/desktop-analytics/overview) - Integrates with Configuration Manager to provide insight and intelligence to make more informed decisions about the update readiness of your Windows clients. |

## List of Azure Monitor supported services

The following table lists Azure services and the data they collect into Azure Monitor. 

- Metrics - The service automatically collects metrics into Azure Monitor Metrics. 
- Logs - The service supports diagnostic settings which can collect platform logs and metrics to Azure Monitor Logs.
- Insight - There is an insight available for the service which provides a customized monitoring experience for the service.

| Service | Resourse Provider Namespace | Has Metrics | Has Logs | Insight
|---------|-----------------------------|-------------|----------|-----------|
 | Application Gateway | Microsoft.Network/applicationGateways | **Yes** | **Yes** | [Application Gateway](/azure/application-gateway/) | 
 | Azure Active Directory | Microsoft.Aadiam/azureADMetrics | **Yes** | No | [Azure Active Directory Domain Services](/azure/active-directory-domain-services/)   | 
 | Azure Active Directory Domain Services | Microsoft.AAD/DomainServices | No | **Yes** | [Azure Active Directory Domain Services](/azure/active-directory-domain-services/)   | 
 | Azure Analysis Services | Microsoft.AnalysisServices/servers | **Yes** | **Yes** | [Azure Analysis Services](/azure/analysis-services/)   | 
 | Azure API for Fast Healthcare Interoperability Resources (FHIR) | Microsoft.HealthcareApis/services | **Yes** | **Yes** | [Azure API for FHIR](/azure/healthcare-apis/) | 
 | Azure API for Fast Healthcare Interoperability Resources (FHIR) | Microsoft.HealthcareApis/workspaces/iotconnectors | **Yes** | No | [Azure API for FHIR](/azure/healthcare-apis/) | 
 | Azure API Management | Microsoft.ApiManagement/service | **Yes** | **Yes** | [API Management](/azure/api-management/)   | 
 | Azure App Service & Functions | Microsoft.Web/connections | **Yes** | No | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/hostingEnvironments | **Yes** | **Yes** | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/hostingEnvironments/multiRolePools | **Yes** | No | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/hostingEnvironments/workerPools | **Yes** | No | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/serverFarms | **Yes** | No | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/sites | **Yes** | **Yes** | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/sites/slots | **Yes** | **Yes** | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure App Service & Functions | Microsoft.Web/staticSites | **Yes** | No | [App Service](/azure/app-service/)<br />[Azure Functions](/azure/azure-functions/)   | 
 | Azure AppConfig | Microsoft.AppConfiguration/configurationStores | **Yes** | **Yes** | [Azure App Configuration](/azure/azure-app-configuration/)   | 
 | Azure Attestation Service | Microsoft.Attestation/attestationProviders | No | **Yes** | [Azure Attestation overview](/azure/attestation/overview) | 
 | Azure Automation | Microsoft.Automation/automationAccounts | **Yes** | **Yes** | [Automation](/azure/automation/)   | 
 | Azure Bastion | Microsoft.Network/bastionHosts | **Yes** | **Yes** | [Azure Bastion](/azure/bastion/) | 
 | Azure Batch | Microsoft.Batch/batchAccounts | **Yes** | **Yes** | [Batch](/azure/batch/)   | 
 | Azure Batch | Microsoft.BatchAI/workspaces | No | No | [Batch](/azure/batch/)   | 
 | Azure Blockchain Service | Microsoft.Blockchain/blockchainMembers | **Yes** | **Yes** | [Azure Blockchain Service](/azure/blockchain/workbench/)   | 
 | Azure Blockchain Service | Microsoft.Blockchain/cordaMembers | No | **Yes** | [Azure Blockchain Service](/azure/blockchain/workbench/)   | 
 | Azure Bot Service | Microsoft.BotService/botServices | **Yes** | **Yes** | [Azure Bot Service](/azure/bot-service/)   | 
 | Azure Cache for Redis | Microsoft.Cache/Redis | **Yes** | **Yes** | [Azure Cache for Redis](/azure/azure-cache-for-redis/)   | 
 | Azure Cache for Redis | Microsoft.Cache/redisEnterprise | **Yes** | No | [Azure Cache for Redis](/azure/azure-cache-for-redis/)   | 
 | Azure Cognitive Services | Microsoft.CognitiveServices/accounts | **Yes** | **Yes** | [Cognitive Services](/azure/cognitive-services/)   | 
 | Azure Communication Services | Microsoft.Communication/CommunicationServices | **Yes** | **Yes** | [Azure Communication Services](/azure/communication-services/) | 
 | Azure Container Instances | Microsoft.ContainerInstance/containerGroups | **Yes** | No | [Container Instances](/azure/container-instances/)   | 
 | Azure Container Registry | Microsoft.ContainerRegistry/registries | **Yes** | **Yes** | [Container Registry](/azure/container-registry/)   | 
 | Azure Content Delivery Network | Microsoft.Cdn/CdnWebApplicationFirewallPolicies | **Yes** | **Yes** | [Content Delivery Network](/azure/cdn/)   | 
 | Azure Content Delivery Network | Microsoft.Cdn/profiles | **Yes** | **Yes** | [Content Delivery Network](/azure/cdn/)   | 
 | Azure Content Delivery Network | Microsoft.Cdn/profiles/endpoints | No | **Yes** | [Content Delivery Network](/azure/cdn/)   | 
 | Azure Cosmos DB | Microsoft.DocumentDB/databaseAccounts | **Yes** | **Yes** | [Azure Cosmos DB](/azure/cosmos-db/)   | 
 | Azure Custom Providers | Microsoft.CustomProviders/resourceProviders | **Yes** | **Yes** | [Azure Custom Providers](../custom-providers/overview.md)   | 
 | Azure Data Explorer | Microsoft.Kusto/clusters | **Yes** | **Yes** | [Azure Data Explorer](/azure/data-explorer/)   | 
 | Azure Data Lake Storage Gen2 | Microsoft.DataLakeStore/accounts | **Yes** | **Yes** | [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction.md)   | 
 | Azure Data Share | Microsoft.DataShare/accounts | **Yes** | **Yes** | [Azure Data Share](/azure/data-share/)   | 
 | Azure Database for MariaDB | Microsoft.DBforMariaDB/servers | **Yes** | **Yes** | [Azure Database for MariaDB](/azure/mariadb/)   | 
 | Azure Database for MySQL | Microsoft.DBforMySQL/flexibleServers | **Yes** | **Yes** | [Azure Database for MySQL](/azure/mysql/)   | 
 | Azure Database for MySQL | Microsoft.DBforMySQL/servers | **Yes** | **Yes** | [Azure Database for MySQL](/azure/mysql/)   | 
 | Azure Database for PostgreSQL | Microsoft.DBforPostgreSQL/flexibleServers | **Yes** | **Yes** | [Azure Database for PostgreSQL](/azure/postgresql/)   | 
 | Azure Database for PostgreSQL | Microsoft.DBforPostgreSQL/serverGroupsv2 | **Yes** | **Yes** | [Azure Database for PostgreSQL](/azure/postgresql/)   | 
 | Azure Database for PostgreSQL | Microsoft.DBforPostgreSQL/servers | **Yes** | **Yes** | [Azure Database for PostgreSQL](/azure/postgresql/)   | 
 | Azure Database for PostgreSQL | Microsoft.DBforPostgreSQL/serversv2 | **Yes** | **Yes** | [Azure Database for PostgreSQL](/azure/postgresql/)   | 
 | Azure Databricks | Microsoft.Databricks/workspaces | No | **Yes** | [Azure Databricks](/azure/azure-databricks/)   | 
 | Azure Digital Twins | Microsoft.DigitalTwins/digitalTwinsInstances | **Yes** | **Yes** | [Azure Digital Twins](/azure/digital-twins/about-digital-twins.md)   | 
 | Azure DNS | Microsoft.Network/dnszones | **Yes** | No | [Azure DNS](/azure/dns/) | 
 | Azure DNS Private Zones | Microsoft.Network/privateDnsZones | **Yes** | No | Azure DNS Private Zones | 
 | Azure Event Grid | Microsoft.EventGrid/domains | **Yes** | **Yes** | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Grid | Microsoft.EventGrid/eventSubscriptions | **Yes** | No | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Grid | Microsoft.EventGrid/extensionTopics | **Yes** | No | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Grid | Microsoft.EventGrid/partnerNamespaces | **Yes** | **Yes** | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Grid | Microsoft.EventGrid/partnerTopics | **Yes** | **Yes** | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Grid | Microsoft.EventGrid/systemTopics | **Yes** | **Yes** | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Grid | Microsoft.EventGrid/topics | **Yes** | **Yes** | [Event Grid](/azure/event-grid/)   | 
 | Azure Event Hubs | Microsoft.EventHub/clusters | **Yes** | No | [Event Hubs](/azure/event-hubs/)   | 
 | Azure Event Hubs | Microsoft.EventHub/namespaces | **Yes** | **Yes** | [Event Hubs](/azure/event-hubs/)   | 
 | Azure ExpressRoute | Microsoft.Network/expressRouteCircuits | **Yes** | **Yes** | [Azure ExpressRoute](azure/expressroute/) | 
 | Azure ExpressRoute | Microsoft.Network/expressRouteGateways | **Yes** | No | [Azure ExpressRoute](azure/expressroute/) | 
 | Azure ExpressRoute | Microsoft.Network/expressRoutePorts | **Yes** | No | [Azure ExpressRoute](azure/expressroute/) | 
 | Azure Firewall | Microsoft.Network/azureFirewalls | **Yes** | **Yes** | [Azure Firewall](/azure/firewall/) | 
 | Azure Front Door | Microsoft.Network/frontdoors | **Yes** | **Yes** | [Azure Frontdoor](/azure/frontdoor/) | 
 | Azure HDInsight | Microsoft.HDInsight/clusters | **Yes** | No | [HDInsight](/azure/hdinsight/)   | 
 | Azure HPC Cache | Microsoft.StorageCache/caches | **Yes** | No | [Azure HPC Cache](/azure/hpc-cache/) | 
 | Azure IoT Central | Microsoft.IoTCentral/IoTApps | **Yes** | No | [Azure IoT Central](/azure/iot-central/)   | 
 | Azure IoT Hub | Microsoft.Devices/ElasticPools | **Yes** | No | [Azure IoT Hub](/azure/iot-hub/) | 
 | Azure IoT Hub | Microsoft.Devices/ElasticPools/IotHubTenants | **Yes** | **Yes** | [Azure IoT Hub](/azure/iot-hub/) | 
 | Azure IoT Hub | Microsoft.Devices/IotHubs | **Yes** | **Yes** | [Azure IoT Hub](/azure/iot-hub/) | 
 | Azure IoT Hub | Microsoft.Devices/ProvisioningServices | **Yes** | **Yes** | [Azure IoT Hub Device Provisioning Service](/azure/iot-dps/)   | 
 | Azure Key Vault | Microsoft.KeyVault/managedHSMs | **Yes** | **Yes** | [Key Vault](/azure/key-vault/)   | 
 | Azure Key Vault | Microsoft.KeyVault/vaults | **Yes** | **Yes** | [Key Vault](/azure/key-vault/)   | 
 | Azure Kubernetes Service (AKS) | Microsoft.ContainerService/managedClusters | **Yes** | **Yes** | [Azure Kubernetes Service (AKS)](/azure/aks/)   | 
 | Azure Kubernetes Service (AKS) | Microsoft.Kubernetes/connectedClusters | **Yes** | No | [Azure Kubernetes Service (AKS)](/azure/aks/)   | 
 | Azure Load Balancer | Microsoft.Network/loadBalancers | **Yes** | **Yes** | Azure Load Balancer | 
 | Azure Logic Apps | Microsoft.Logic/integrationAccounts | No | **Yes** | [Logic Apps](/azure/logic-apps/)   | 
 | Azure Logic Apps | Microsoft.Logic/integrationServiceEnvironments | **Yes** | No | [Logic Apps](/azure/logic-apps/)   | 
 | Azure Logic Apps | Microsoft.Logic/workflows | **Yes** | **Yes** | [Logic Apps](/azure/logic-apps/)   | 
 | Azure Machine Learning | Microsoft.MachineLearningServices/workspaces | **Yes** | **Yes** | [Azure Machine Learning](/azure/machine-learning/)   | 
 | Azure Maps | Microsoft.Maps/accounts | **Yes** | No | [Azure Maps](/azure/azure-maps/)   | 
 | Azure Media Services | Microsoft.Media/mediaservices | **Yes** | **Yes** | [Media Services](/azure/media-services/)   | 
 | Azure Media Services | Microsoft.Media/mediaservices/liveEvents | **Yes** | No | [Media Services](/azure/media-services/)   | 
 | Azure Media Services | Microsoft.Media/mediaservices/streamingEndpoints | **Yes** | No | [Media Services](/azure/media-services/)   | 
 | Azure Media Services | Microsoft.Media/videoAnalyzers | **Yes** | **Yes** | [Media Services](/azure/media-services/)   | 
 | Azure Monitor Application Insights | microsoft.insights/components | **Yes** | **Yes** | [Azure Monitor](/azure/azure-monitor/)   | 
 | Azure Monitor Autoscale | microsoft.insights/autoscalesettings | **Yes** | **Yes** | [Azure Monitor](/azure/azure-monitor/)   | 
 | Azure Monitor Log Analytics | Microsoft.OperationalInsights/workspaces | **Yes** | **Yes** | [Azure Monitor](/azure/azure-monitor/)   | 
 | Azure NetApp Files | Microsoft.NetApp/netAppAccounts/capacityPools | **Yes** | No | [Azure NetApp Files](/azure/azure-netapp-files/)   | 
 | Azure NetApp Files | Microsoft.NetApp/netAppAccounts/capacityPools/volumes | **Yes** | No | [Azure NetApp Files](/azure/azure-netapp-files/)   | 
 | Azure Network Watcher | Microsoft.Network/networkWatchers/connectionMonitors | **Yes** | No | Azure Network Watcher | 
 | Azure Peering Service | Microsoft.Peering/peerings | **Yes** | No | Microsoft Azure Peering Service | 
 | Azure Peering Service | Microsoft.Peering/peeringServices | **Yes** | No | Microsoft Azure Peering Service | 
 | Azure Private Link | Microsoft.Network/privateEndpoints | **Yes** | No | Azure Private Link | 
 | Azure Private Link | Microsoft.Network/privateLinkServices | **Yes** | No | Azure Private Link | 
 | Azure Purview | Microsoft.Purview/accounts | **Yes** | **Yes** | [Azure Purview](/azure/purview/) | 
 | Azure Relay | Microsoft.Relay/namespaces | **Yes** | **Yes** | [Azure Relay](/azure/service-bus-relay/relay-what-is-it.md)   | 
 | Azure Resource Manager | Microsoft.Resources/subscriptions | **Yes** | No | [Azure Resource Manager](../)   | 
 | Azure Search | Microsoft.Search/searchServices | **Yes** | **Yes** | [Azure Cognitive Search](/azure/search/)   | 
 | Azure Service Bus | Microsoft.ServiceBus/namespaces | **Yes** | **Yes** | [Service Bus](/azure/service-bus/)   | 
 | Azure SignalR Service | Microsoft.SignalRService/SignalR | **Yes** | **Yes** | [Azure SignalR Service](/azure/azure-signalr/)   | 
 | Azure SignalR Service | Microsoft.SignalRService/WebPubSub | **Yes** | **Yes** | [Azure SignalR Service](/azure/azure-signalr/)   | 
 | Azure Site Recovery | Microsoft.RecoveryServices/vaults | **Yes** | **Yes** | [Azure Site Recovery](/azure/site-recovery/)   | 
 | Azure Software Load Balancer | Microsoft.Network/natGateways | **Yes** | No | Azure Software Load Balancer | 
 | Azure Spatial Anchors | Microsoft.MixedReality/remoteRenderingAccounts | **Yes** | No | [Azure Spatial Anchors](/azure/spatial-anchors/)   | 
 | Azure Spatial Anchors | Microsoft.MixedReality/spatialAnchorsAccounts | **Yes** | No | [Azure Spatial Anchors](/azure/spatial-anchors/)   | 
 | Azure Spring Cloud | Microsoft.AppPlatform/Spring | **Yes** | **Yes** | [Azure Spring Cloud](/azure/spring-cloud/spring-cloud-overview.md)   | 
 | Azure SQL Database | Microsoft.Sql/servers/databases | **Yes** | No | [Azure SQL Database](/azure/azure-sql/database/) | 
 | Azure SQL Database | Microsoft.Sql/servers/elasticpools | **Yes** | No | [Azure SQL Database](/azure/azure-sql/database/) | 
 | Azure SQL Managed Instance | Microsoft.Sql/managedInstances | **Yes** | **Yes** | [Azure SQL Managed Instance](/azure/azure-sql/database/monitoring-tuning-index) | 
 | Azure Stack Edge | Microsoft.DataBoxEdge/DataBoxEdgeDevices | **Yes** | No | [Azure Stack Edge](/azure/databox-online/azure-stack-edge-overview.md)   | 
 | Azure Storage (Classic) | Microsoft.ClassicStorage/storageAccounts | **Yes** | No | [Azure Storage (Classic)](/azure/storage/) | 
 | Azure Storage (Classic) | Microsoft.ClassicStorage/storageAccounts/blobServices | **Yes** | No | [Azure Storage Blobs (Classic)](/azure/storage/blobs/) | 
 | Azure Storage (Classic) | Microsoft.ClassicStorage/storageAccounts/fileServices | **Yes** | No | Azure Storage Files (Classic)(/azure/storage/files/) | 
 | Azure Storage (Classic) | Microsoft.ClassicStorage/storageAccounts/queueServices | **Yes** | No | Azure Storage Queues (Classic)(/azure/storage/queue/) | 
 | Azure Storage (Classic) | Microsoft.ClassicStorage/storageAccounts/tableServices | **Yes** | No | Azure Storage Tables (Classic)(/azure/storage/tables/) | 
 | Azure Storage | Microsoft.Storage/storageAccounts | **Yes** | No | [Storage](/azure/storage/)   | 
 | Azure Storage | Microsoft.StorageSync/storageSyncServices | **Yes** | No | [Storage](/azure/storage/)   | 
 | Azure Storage Blobs | Microsoft.Storage/storageAccounts/blobServices | **Yes** | **Yes** | [Azure Storage Blobs](/azure/storage/blobs/) | 
 | Azure Storage Files | Microsoft.Storage/storageAccounts/fileServices | **Yes** | **Yes** | [Azure Storage Files](/azure/storage/files/) | 
 | Azure Storage Queue Services | Microsoft.Storage/storageAccounts/queueServices | **Yes** | **Yes** | [Azure Storage Queue Services](/azure/storage/queues/)   | 
 | Azure Storage tables | Microsoft.Storage/storageAccounts/tableServices | **Yes** | **Yes** | [Azure Table Services](/azure/storage/tables/)   | 
 | Azure Stream Analytics | Microsoft.StreamAnalytics/streamingjobs | **Yes** | **Yes** | [Azure Stream Analytics](/azure/stream-analytics/)   | 
 | Azure Synapse Analytics | Microsoft.Synapse/workspaces | **Yes** | **Yes** | [Azure Synapse Analytics](/azure/sql-data-warehouse/)   | 
 | Azure Synapse Analytics | Microsoft.Synapse/workspaces/bigDataPools | **Yes** | **Yes** | [Azure Synapse Analytics](/azure/sql-data-warehouse/)   | 
 | Azure Synapse Analytics | Microsoft.Synapse/workspaces/sqlPools | **Yes** | **Yes** | [Azure Synapse Analytics](/azure/sql-data-warehouse/)   | 
 | Azure Time Series Insights | Microsoft.TimeSeriesInsights/environments | **Yes** | **Yes** | [Azure Time Series Insights](/azure/time-series-insights/)   | 
 | Azure Time Series Insights | Microsoft.TimeSeriesInsights/environments/eventsources | **Yes** | **Yes** | [Azure Time Series Insights](/azure/time-series-insights/)   | 
 | Azure Traffic Manager | Microsoft.Network/trafficmanagerprofiles | **Yes** | **Yes** | Azure Traffic Manager | 
 | Azure Virtual Machine Scale Sets | Microsoft.Compute/virtualMachineScaleSets | **Yes** | No | [Virtual Machines](/azure/virtual-machines/)<br />[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)   | 
 | Azure Virtual Machine Scale Sets | Microsoft.Compute/virtualMachineScaleSets/virtualMachines | **Yes** | No | [Virtual Machines](/azure/virtual-machines/)<br />[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)   | 
 | Azure Virtual Machines - Classic | Microsoft.ClassicCompute/domainNames/slots/roles | **Yes** | No | [Virtual Machines - Classic](/azure/virtual-machines/) | 
 | Azure Virtual Machines - Classic | Microsoft.ClassicCompute/virtualMachines | **Yes** | No | [Virtual Machines - Classic](/azure/virtual-machines/) | 
 | Azure Virtual Machines | Microsoft.Compute/cloudServices | **Yes** | No | [Virtual Machines](/azure/virtual-machines/)<br />[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)   | 
 | Azure Virtual Machines | Microsoft.Compute/cloudServices/roles | **Yes** | No | [Virtual Machines](/azure/virtual-machines/)<br />[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)   | 
 | Azure Virtual Machines | Microsoft.Compute/disks | **Yes** | No | [Virtual Machines](/azure/virtual-machines/)<br />[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)   | 
 | Azure Virtual Machines | Microsoft.Compute/virtualMachines | **Yes** | No | [Virtual Machines](/azure/virtual-machines/)<br />[Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/)   | 
 | Azure Virtual Network (Classic) | Microsoft.ClassicNetwork/networkSecurityGroups | No | **Yes** | [Virtual Network (Classic)](/azure/virtual-network/network-security-groups-overview) | 
 | Azure Virtual Network | Microsoft.Network/networkInterfaces | **Yes** | No | Azure Virtual Network | 
 | Azure Virtual Network | Microsoft.Network/networkSecurityGroups | No | **Yes** | Azure Virtual Network | 
 | Azure Virtual Network | Microsoft.Network/publicIPAddresses | **Yes** | **Yes** | Azure Virtual Network | 
 | Azure Virtual Network | Microsoft.Network/virtualNetworks | **Yes** | **Yes** | Azure Virtual Network | 
 | Azure Virtual Network Gateway | Microsoft.Network/virtualNetworkGateways | **Yes** | **Yes** | Azure Virtual Network Gateway | 
 | Azure Virtual Routers | Microsoft.Network/virtualRouters | **Yes** | No | Azure Virtual Routers | 
 | Azure Virtual WAN (This is a VWAN setting for VPN gateways) | Microsoft.Network/vpnGateways | **Yes** | **Yes** | Azure VPN Gateway - Virtual WAN | 
 | Azure Virtual WAN | Microsoft.Network/p2sVpnGateways | **Yes** | **Yes** | Azure Virtual WAN | 
 | Azure Virtual WAN | Microsoft.Network/virtualHubs | **Yes** | No | Azure Virtual WAN | 
 | Azure VMware Solution | Microsoft.AVS/privateClouds | **Yes** | **Yes** | [Azure VMware Solution](/azure/azure-vmware/)   | 
 | Azure VMware Solution by CloudSimple | Microsoft.VMwareCloudSimple/virtualMachines | **Yes** | No | [Azure VMware Solution](/azure/azure-vmware/)   | 
 | Azure VPN Gateway | Microsoft.Network/connections | **Yes** | No | [VPN Gateway](/azure/vpn-gateway/) | 
 | Cray ClusterStor in Azure | Microsoft.ClusterStor/nodes | **Yes** | No | Cray on Azure | 
 | Data Factory | Microsoft.DataFactory/dataFactories | **Yes** | No | [Data Factory](/azure/data-factory/)   | 
 | Data Factory | Microsoft.DataFactory/factories | **Yes** | **Yes** | [Data Factory](/azure/data-factory/)   | 
 | Data Lake Analytics | Microsoft.DataLakeAnalytics/accounts | **Yes** | **Yes** | [Data Lake Analytics](/azure/data-lake-analytics/)   | 
 | Microsoft Bing | Microsoft.Bing/accounts | **Yes** | No | [Bing Search API documentation - Azure Cognitive Services/azure/cognitive-services/bing-web-search/) | 
 | Microsoft Cloud Test Platform | Microsoft.Cloudtest/hostedpools | **Yes** | No | Cloud Test | 
 | Microsoft Cloud Test Platform | Microsoft.Cloudtest/pools | **Yes** | No | Cloud Test | 
 | Microsoft Connected Vehicle Platform | Microsoft.ConnectedVehicle/platformAccounts | **Yes** | **Yes** | Connected vehicle program | 
 | Microsoft Dynamics 365 Customer Insights | Microsoft.D365CustomerInsights/instances | No | **Yes** | Dynamics 365 | 
 | Microsoft Experimentation Platform | microsoft.experimentation/experimentWorkspaces | **Yes** | **Yes** | [Microsoft Experimentation Platform](https://www.microsoft.com/research/group/experimentation-platform-exp/) | 
 | Notification Hubs | Microsoft.NotificationHubs/namespaces/notificationHubs | **Yes** | No | [Notification Hubs](/azure/notification-hubs/)   | 
 | Power BI | Microsoft.PowerBI/tenants | No | **Yes** | [Power BI](/power-bi/power-bi-overview)   | 
 | Power BI | Microsoft.PowerBI/tenants/workspaces | No | **Yes** | [Power BI](/power-bi/power-bi-overview)   | 
 | Power BI Embedded | Microsoft.PowerBIDedicated/capacities | **Yes** | **Yes** | [Power BI Embedded](/azure/power-bi-embedded/)   | 
 | Project CI | Microsoft.DataCollaboration/workspaces | **Yes** | **Yes** | Project CI | 
 | StorSimple | microsoft.hybridnetwork/networkfunctions | **Yes** | No | [StorSimple](/azure/storsimple/)   | 
 | StorSimple | microsoft.hybridnetwork/virtualnetworkfunctions | **Yes** | No | [StorSimple](/azure/storsimple/)   | 
 | Windows Virtual Desktop | Microsoft.DesktopVirtualization/applicationgroups | No | **Yes** | [Windows Virtual Desktop](/azure/virtual-desktop/)   | 
 | Windows Virtual Desktop | Microsoft.DesktopVirtualization/hostpools | No | **Yes** | [Windows Virtual Desktop](/azure/virtual-desktop/)   | 
 | Windows Virtual Desktop | Microsoft.DesktopVirtualization/workspaces | No | **Yes** | [Windows Virtual Desktop](/azure/virtual-desktop/)   | 


## Virtual machine agents

The following table lists the agents that can collect data from the guest operating system of virtual machines and send data to Monitor. Each agent can collect different data and send it to either Metrics or Logs in Azure Monitor. The Azure Monitor agent is meant to be a replacement for the other agents.  

See [Overview of Azure Monitor agents](agents/agents-overview.md) for details on the data that each agent can collect and advice on their usage. 

| Agent |  Metrics | Logs |
|:---|:---|:---|:---|
| [Azure Monitor agent](agents/azure-monitor-agent-overview.md) | Yes | Yes |
| [Log Analytics agent](agents/log-analytics-agent.md) | No | Yes|
| [Diagnostic extension](agents/diagnostics-extension-overview.md) | Yes | No |
| [Telegraf agent](essentials/collect-custom-metrics-linux-telegraf.md) | Yes | No |
| [Dependency agent](vm/vminsights-enable-overview.md) | No | Yes |

## Other solutions

Other solutions are available for monitoring different applications and services, but active development has stopped and they may not be available in all regions. They are covered by the Azure Log Analytics data ingestion service level agreement.

| Solution | Description |
|:---|:---|
| [Active Directory health check](insights/ad-assessment.md) | Assess the risk and health of your Active Directory environments. |
| [Active Directory replication status](insights/ad-replication-status.md) | Regularly monitors your Active Directory environment for any replication failures. |
| [Activity log analytics](essentials/activity-log.md#activity-log-analytics-monitoring-solution) | View Activity Log entries. |
| [DNS Analytics (preview)](insights/dns-analytics.md) | Collects, analyzes, and correlates Windows DNS analytic and audit logs and other related data from your DNS servers. |
| [Cloud Foundry](../cloudfoundry/cloudfoundry-oms-nozzle.md) | Collect, view, and analyze your Cloud Foundry system health and performance metrics, across multiple deployments. |
| [Containers](containers/containers.md) | View and manage Docker and Windows container hosts. |
| [On-Demand Assessments](/services-hub/health/getting_started_with_on_demand_assessments) | Assess and optimize the availability, security, and performance of your on-premises, hybrid, and cloud Microsoft technology environments. |
| [SQL health check](insights/sql-assessment.md) | Assess the risk and health of your SQL Server environments.  |
| [Wire Data](insights/wire-data.md) | Consolidated network and performance data collected from Windows-connected and Linux-connected computers with the Log Analytics agent. |

## Third party integration

| Solution | Description |
|:---|:---|
| [ITSM](alerts/itsmc-overview.md) | The IT Service Management Connector (ITSMC) allows you to connect Azure and a supported IT Service Management (ITSM) product/service.  |


## Resources outside of Azure

Azure Monitor can collect data from resources outside of Azure using the methods listed in the following table.

| Resource | Method |
|:---|:---|
| Applications | Monitor web applications outside of Azure using Application Insights. See [What is Application Insights?](./app/app-insights-overview.md). |
| Virtual machines | Use agents to collect data from the guest operating system of virtual machines in other cloud environments or on-premises. See [Overview of Azure Monitor agents](agents/agents-overview.md). |
| REST API Client | Separate APIs are available to write data to Azure Monitor Logs and Metrics from any REST API client. See [Send log data to Azure Monitor with the HTTP Data Collector API](logs/data-collector-api.md) for Logs and [Send custom metrics for an Azure resource to the Azure Monitor metric store by using a REST API](essentials/metrics-store-custom-rest-api.md) for Metrics. |

## Next steps

- Read more about the [Azure Monitor data platform which stores the logs and metrics collected by insights and solutions](data-platform.md).
- Complete a [tutorial on monitoring an Azure resource](essentials/tutorial-resource-logs.md).
- Complete a [tutorial on writing a log query to analyze data in Azure Monitor Logs](essentials/tutorial-resource-logs.md).
- Complete a [tutorial on creating a metrics chart to analyze data in Azure Monitor Metrics](essentials/tutorial-metrics-explorer.md).
 

