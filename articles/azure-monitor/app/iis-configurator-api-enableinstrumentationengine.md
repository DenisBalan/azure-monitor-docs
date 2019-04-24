---
title: IIS Configurator | Microsoft Docs
description: Monitor a website's performance without re-deploying it. Works with ASP.NET web apps hosted on-premises, in VMs or on Azure.
services: application-insights
documentationcenter: .net
author: tilee
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
---
# IISConfigurator API Reference

## Disclaimer
This module is a prototype application, and is not recommended for your production environments.

# Enable-InstrumentationEngine (v0.2.0-alpha)

**IMPORTANT**: This cmdlet must be run in a PowerShell Session with Administrator permissions.

**NOTE**: This cmdlet will require you to review and accept our license and privacy statement.

## Description

Enable the Instrumentation Engine. 
This will set some registry keys.
You will need to restart IIS for your changes to take effect.

The Instrumentation Engine is used in addition to the .NET SDKs and will collect events and messages of what is happening to during the execution of a managed process. Including but not limited to Dependency Result Codes, HTTP Verbs, and SQL Command Text. 

You should enable the Instrumentation Engine if..
- You've already enabled monitoring using the IISConfigurator but didn't enable the InstrumentationEngine, this will execute that step.
- You've manually instrumented your application with the .NET SDKs and want to collect additional telemetry.

**NOTE:** The Instrumentation Engine adds additional overhead and is therefore off by default.

## Examples

```powershell
PS C:\> Enable-InstrumentationEngine
```

## Parameters 

### -AcceptLicense
**Optional.** Use this switch to accept the license and privacy statement in headless installations.

### -Verbose
**Common Parameter.** Use this switch to output detailed logs.

## Output


#### Example output from successfully enabling the instrumentation engine

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```
