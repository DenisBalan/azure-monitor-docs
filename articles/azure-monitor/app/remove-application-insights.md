---
title: Remove Application Insights in Visual Studio - Azure Monitor 
description: How to remove Application Insights SDK for ASP.NET and ASP.NET Core in Visual Studio. 
ms.topic: conceptual
ms.date: 03/25/2020

---

# How to remove Application Insights in Visual Studio

This article will show you how to remove the ASP.NET and ASP.NET Core Application Insights SDK in Visual Studio.

To remove Application Insights, you'll need to remove the NuGet packages and references from the API in your application. You can remove NuGet packages by using the Package Management Console or Manage NuGet Solution in Visual Studio. The following sections will show you how to remove NuGet Packages and what was automatically added in your project. Be sure to confirm the files added and areas with in your own code in which you made calls to the API are removed.

## What is created when you add Application Insights

# [.NET](#tab/net)

When you add Application Insights Telemetry to a Visual Studio ASP.NET template project, it adds the following files:

- ApplicationInsights.config
- AiHandleErrorAttribute.cs

The following pieces of code are added:

- [Your project's name].vbproj

```C#
 <ApplicationInsightsResourceId>/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4</ApplicationInsightsResourceId>
```

- Packages.config

```xml
<packages>
...

  <package id="Microsoft.ApplicationInsights" version="2.12.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.12.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.12.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Web" version="2.12.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.12.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.12.0" targetFramework="net472" />

  <package id="Microsoft.AspNet.TelemetryCorrelation" version="1.0.7" targetFramework="net472" />

  <package id="System.Buffers" version="4.4.0" targetFramework="net472" />
  <package id="System.Diagnostics.DiagnosticSource" version="4.6.0" targetFramework="net472" />
  <package id="System.Memory" version="4.5.3" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
...
</packages>
```

- Layout.cshtml

```html
<head>
...
    <script type = 'text/javascript' >
        var appInsights=window.appInsights||function(config)
        {
            function r(config){ t[config] = function(){ var i = arguments; t.queue.push(function(){ t[config].apply(t, i)})} }
            var t = { config:config},u=document,e=window,o='script',s=u.createElement(o),i,f;for(s.src=config.url||'//az416426.vo.msecnd.net/scripts/a/ai.0.js',u.getElementsByTagName(o)[0].parentNode.appendChild(s),t.cookie=u.cookie,t.queue=[],i=['Event','Exception','Metric','PageView','Trace','Ajax'];i.length;)r('track'+i.pop());return r('setAuthenticatedUserContext'),r('clearAuthenticatedUserContext'),config.disableExceptionTracking||(i='onerror',r('_'+i),f=e[i],e[i]=function(config, r, u, e, o) { var s = f && f(config, r, u, e, o); return s !== !0 && t['_' + i](config, r, u, e, o),s}),t
        }({
            instrumentationKey:'415c0ab5-90c7-4a2b-96dd-fcb94336fc30'
        });
        
        window.appInsights=appInsights;
        appInsights.trackPageView();
    </script>
...
</head>
```

- ConnectedService.json

```json
{
  "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
  "Version": "16.0.0.0",
  "GettingStartedDocument": {
    "Uri": "https://go.microsoft.com/fwlink/?LinkID=613413"
  }
}
```

- FilterConfig.cs

```C#
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new ErrorHandler.AiHandleErrorAttribute());// This line was added
        }
```

# [.NET Core](#tab/netcore)

When you add Application Insights Telemetry to a Visual Studio ASP.NET Core template project, it adds the following code:

- [Your project's name].csproj

    ```C#
      <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <ApplicationInsightsResourceId>/subscriptions/b21990e9-15a7-47eb-9ad0-f6b7155ab349/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4core</ApplicationInsightsResourceId>
      </PropertyGroup>
    
      <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.12.0" />
      </ItemGroup>
    
      <ItemGroup>
        <WCFMetadata Include="Connected Services" />
      </ItemGroup>
    ```
- Appsettings.json:

    ```json
    "ApplicationInsights": {
        "InstrumentationKey": "00000000-0000-0000-0000-000000000000"
    ```

- ConnectedService.json
    
    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "16.0.0.0",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
      }
    }
    ```
- Startup.cs

    ```C#
       public void ConfigureServices(IServiceCollection services)
            {
                services.AddRazorPages();
                services.AddApplicationInsightsTelemetry(); // This is added
            }
    ```

---

## Uninstall using the Package Management Console

# [.NET](#tab/net)

1. To open the Package Management Console, in the top menu select Tools > NuGet Package Manager > Package Manager Console.
     
    ![In the top menu click Tools > NuGet Package Manager > Package Manager Console](./media/remove-application-insights/package-manager.png)
1. Enter the following command: `Uninstall-Package Microsoft.ApplicationInsights.Web -RemoveDependencies`

    After entering the command, the Application Insights package and all of its dependencies will be uninstalled from the project.
    
    ![Enter command in console](./media/remove-application-insights/package-management-console.png)

# [.NET Core](#tab/netcore)

1. To open the Package Management Console, in the top menu select Tools > NuGet Package Manager > Package Manager Console.

1. Enter the following command: ` Uninstall-Package Microsoft.ApplicationInsights.AspNetCore -RemoveDependencies`
    After entering the command, the Application Insights package and all of its dependencies will be uninstalled from the project

---

## Uninstall using the Visual Studio NuGet UI

# [.NET](#tab/net)

1. In the *Solution Explore* on the right, right click on **Solution** and select **Manage NuGet Packages for Solution**
    You'll then see a screen that allows you to edit all the NuGet packages that are part of the project.
    
     ![Right click Solution, in the Solution Explore, then select Manage NuGet Packages for Solution](./media/remove-application-insights/manage-nuget-framework.png)
    
1. Click on the "Microsoft.ApplicationInsights.Web" package. On the right, check the checkbox next to **Project** to select all projects.
    
1. To remove all dependencies when uninstalling, select the **Options** dropdown button below the section where you selected project.

    Under *Uninstall Options*, select the checkbox next to *Remove dependencies*.

1. Select Uninstall
    
    ![Check remove dependencies, then uninstall](./media/remove-application-insights/uninstall-framework.png)
    
    A dialog box will display that shows all of the dependencies to be removed from the application. Select **ok** to remove them.
    
    ![Check remove dependencies, then uninstall](./media/remove-application-insights/preview-uninstall-framework.png)
    
1.  After everything is uninstalled, you may still see  "ApplicationInsights.config" and "AiHandleErrorAttribute.vb" in the *Solution Explore*. You can delete the two files manually.

# [.NET Core](#tab/.netcore)

1. In the *Solution Explore* on the right, right click on **Solution** and select **Manage NuGet Packages for Solution*

    You'll then see a screen that allows you to edit all the NuGet packages that are part of the project.

    ![Right click Solution, in the Solution Explore, then select Manage NuGet Packages for Solution](./media/remove-application-insights/manage-nuget-core.png)

1. Click on "Microsoft.ApplicationInsights.AspNetCore" package. On the right, check the checkbox next to **Project** to select all projects.

    ![Check remove dependencies, then uninstall](./media/remove-application-insights/uninstall-core.png)

---

## Troubleshotting

# [.NET](#tab/net)

- Unable to install because another NuGet packages depends on it error.

 You will need to uninstall the NuGet package or packages that depend on "Microsoft.ApplicationInsights.Web" first. If you have multiple packages that depend on each other, you need to start with the package that nothing depends on first.

    For example, if you have trace collection enabled you may receive an error like the one below when uninstalling Microsoft.ApplicationInsights.Web.

   ![Error of being able to uninstall Microsoft.ApplicationInsights.Web because Microsoft.ApplicationInsights.TraceListener depends on it ](/media/remove-application-insights/uninstall-error1.png)

    If that is case you will first uninstall the "Microsoft.ApplicationInsights.TraceListener" package, and then try uninstalling the  "Microsoft.ApplicationInsights.Web" again.

    If you end up with another error that you can't uninstall "Microsoft.ApplicationInsights.TraceListener" because another package or packages depend on it you can look through those packages and find one that doesn't does not depend on anything to be uninstalled first.

   ![Error of being able to uninstall Microsoft.ApplicationInsights.TraceListener because Microsoft.ApplicationInsights.Dependency Collector, Microsoft.ApplicationInsights.PerfCounterCollector, and Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel all depend on Microsoft.ApplicationInsights.WindowsServer depends on it](/media/remove-application-insights/uninstall-error2.png)

    However in this case Microsoft.ApplicationInsights.Dependency Collector, Microsoft.ApplicationInsights.PerfCounterCollector, and Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel all depend on Microsoft.ApplicationInsights.WindowsServer but Microsoft.ApplicationInsights.WindowsServer depends on Microsoft.ApplicationInsights.Web.

    Go back to uninstall Microsoft.ApplicationInsights.Web but this time in *Uninstall Options* select "Force uninstall even if there are dependencies on it" ("Remove dependencies" should still be selected). This will uninstall the package even if it's still being referenced in the project. This option could lead to broken reference in your project; if that happens you make need to [reinstall](https://docs.microsoft.com/nuget/consume-packages/reinstalling-and-updating-packages) those other packages. Then uninstall "Microsoft.ApplicationInsights.TraceListener".

   ![Select force uninstall](/media/remove-application-insights/force-uninstall.png)

- How to uninstall Microsoft.ApplicationInsights.TraceListener

    If you would like to only uninstall Microsoft.ApplicationInsights.TraceListener, follow the steps in "Unable to uninstall because another NuGet package depends on it error" but instead force uninstalling Microsoft.ApplicationInsights.Web, force uninstall "Microsoft.ApplicationInsights.TraceListener".

- 

# [.NET Core](#tab/netcore)

---

## Next Steps

- [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)