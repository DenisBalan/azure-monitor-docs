---
title: Data export in Log Analytics
description: 
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/09/2020

---

# Data export in Log Analytics (preview)
Log Analytics data export allows you to continuously export data of selected tables from your Log Analytics workspace to Azure Storage Account or Event Hub as it's collected. This article provides background information and steps to configure data export in your workspaces.

## Current limitations

- Configuration may only be performed using REST requests. You cannot use the Azure portal or with PowerShell or CLI.
- Supported tables are currently limited to *Heartbeat*, *SecurityEvent*, and *Perf*. 
- Your Log Analytics workspace must in one of the following regions:
  - East US
  - West Europe
  - Central Canada
- Sending data to Azure Data Lake Storage Gen2 is not currently supported.
- The destination storage account or event hub must be in the same region as the Log Analytics workspace.
- Names of tables to be exported can be no longer than 60 characters for a storage account and no more than 47 characters to an event hub. Tables with longer names will not be exported.

## Prerequisites

- The storage account and event hub must already be created and must be in the same region as the Log Analytics workspace.
- The storage account must be StorageV1 or StorageV2. Classic storage is not supported  
- Export to storage requires that you register your subscription ID where your storage account is located with the *Microsoft.Insights* resource provider namespace. 
- If you have configured your storage account to allow access from selected networks, you need to add an exception in your storage account settings to allow Azure Monitor to write to your storage.

Azure Monitor lets you export data of selected tables from your Log Analytics workspace as it reaches ingestion, and continuously send it to Azure Storage Account and Event Hub. Log Analytics data export can write append blobs to immutable storage accounts when time-based retention policies have the allowProtectedAppendWrites setting enabled. This allows writing new blocks to an append blob, while maintaining immutability protection and compliance.

See [Allow protected append blobs writes](../storage/blobs/storage-blob-immutable-storage.md#allow-protected-append-blobs-writes)

## Overview
Once data export is configured for your workspace, any new data sent to the selected tables in your Log Analytics workspace is exported to your storage account hourly or to event hub in near-real-time. Export rules can be disabled to let you stop the export when you don’t need to retain data during certain time, for example, during testing. 

There is no a way to filter data or to limit the export to certain events. For example, when you configure a data export rule for *SecurityEvent* table, all data sent to the *SecurityEvent* table is exported starting from the configuration time.

When exporting to Storage, each table is kept under a separate container – example: “am-SecurityEvent”. Similarly, when exporting to Event Hub, each table is exported to a new Event Hub instance – for example: am-securityevent. 

## Regions
The Log Analytics workspace and destination storage account and event hub must be located in the same region. If you need to replicate your data to other storage accounts, you can use any of the [Azure Storage redundancy options](../../storage/common/storage-redundancy.md).  


