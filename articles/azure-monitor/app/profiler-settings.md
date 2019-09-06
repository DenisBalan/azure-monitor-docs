---
title: Use the Azure Application Insights Profiler settings pane | Microsoft Docs
description: See Profiler status and start profiling sessions
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
---

# Configure Application Insights Profiler

## Profiler settings page

To open the Azure Application Insights Profiler settings pane, go to the Application Insights Performance pane, and then select the **Configure Profiler** button.

![Link to open Profiler settings page][configure-profiler-entry]

That opens a page that looks like this:

![Profiler settings page][configure-profiler-page]

The **Configure Application Insights Profiler** page has these features:

| | |
|-|-|
Profile Now | Starts profiling sessions for all apps that are linked to this instance of Application Insights.
Triggers | Allows you to configure triggers that cause the profiler to run 
Recent profiling sessions | Displays information about past profiling sessions.

## Profile Now
This option allows you to start a profiling session on demand. When you click this link, all profiler agents that are sending data to this Application Insights instance will start to capture a profile. After 5 to 10 minutes, the profile session will show in the list below.

For a user to manually trigger a profiler session they require at minimum "write" access on their role for the Application Insights component. In most cases you get this access automatically and no additional work is needed. If you are having issues, the subscription scope role to add would be the "Application Insights Component Contributor" role. [See more about role access control with Azure Monitoring](https://docs.microsoft.com/azure/azure-monitor/app/resources-roles-access-control).

## Trigger Settings
![Trigger Settings Flyout][trigger-settings-flyout]

Clicking the Triggers button on the the menu bar opens the trigger settings box. You can setup a CPU trigger or Memory trigger to start profiling when the percentage of CPU or Memory use hits the level you set.
| | |
|-|-|
On / Off Button | On: profiler can be started by this trigger; Off: profiler won't be started by this trigger
Memory threshold | When this percentage of memory is in use, the profiler will be started.
Duration | This sets the length of time the profiler will run when triggered.
Cooldown | The sets the length of time the profiler will wait before checking for the memory or CPU usage again after it is triggered.

## Recent Profiling Sessions
This section of the page shows information about recent profiling sessions. A profiling session represents the period of time when the profiler agent was taking a profile on one of the machines hosting your application. You can open the profiles from a session by clicking on one of the rows. For each session, we show:
| | |
|-|-|
Triggered by | How the session was started, either by a trigger, Profile Now, or default sampling 
App Name | Name of the application that was profiled
Machine Instance | Name of the machine the profiler agent ran on
Timestamp | When the profile was taken
Tracee | Number of traces that were attached to individual requests
CPU % | Percentage of CPU that was being used while the profiler was running
Memory % | Percentage of memory that was being used while the profiler was running

## <a id="profileondemand"></a> Use web performance tests to generate traffic to your application

You can trigger Profiler manually with a single click. Suppose you're running a web performance test. You'll need traces to help you understand how your web app is performing under load. Having control over when traces are captured is crucial, because you know when the load test will be running. But the random sampling interval might miss it.

The next sections illustrate how this scenario works:

### Step 1: Generate traffic to your web app by starting a web performance test

If your web app already has incoming traffic or if you just want to manually generate traffic, skip this section and continue to Step 2.

1. In the Application Insights portal, select **Configure** > **Performance Testing**. 

1. To start a new performance test, select the **New** button.

   ![create new performance test][create-performance-test]

1. In the **New performance test** pane, configure the test target URL. Accept all default settings, and then select **Run test** to start running the load test.

    ![Configure load test][configure-performance-test]

    The new test is queued first, followed by a status of *in progress*.

    ![Load test is submitted and queued][load-test-queued]

    ![Load test is running in progress][load-test-in-progress]

### Step 2: Start a Profiler on-demand session

1. When the load test is running, start Profiler to capture traces on the web app while it's receiving load.

1. Go to the **Configure Profiler** pane.


### Step 3: View traces

After Profiler finishes running, follow the instructions on notification to go to Performance pane and view traces.

## App Service Environment
Depending on how your Azure App Service Environment is configured, the call to check on the agent status might be blocked. The pane might display a message that the agent isn't running even when it is running. To make sure that it is, check the webjob on your application. If all the app settings values are correct and the Application Insights site extension is installed on your application, Profiler is running. If your application is receiving enough traffic, recent profiling sessions should be displayed in a list.

## Next steps
[Enable Profiler and view traces](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[configure-profiler-page]: ./media/profiler-settings/configureBlade.png
[trigger-settings-flyout]: ./media/profiler-settings/CPUTrigger.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png
