---
title: HEART Analytics Workbook
description: Overview of HEART analytics workbook and its setup
ms.topic: conceptual
ms.date: 11/11/2021

---

# Analyzing Product Usage with HEART
This article describes how to enable and use the Heart Workbook on Azure Monitor. The HEART workbook is based on the HEART measurement framework, originally introduced by Google. Several Microsoft internal teams use HEART to deliver better software.

 
## Overview
HEART is an acronym that stands for Happiness, Engagement, Adoption, Retention, and Task Success. It helps product teams deliver better software by focusing on the following five dimensions of customer experience:

 

- **Happiness**: Measure of user attitude  
- **Engagement**: Level of active user involvement 
- **Adoption**: Target audience penetration
- **Retention**: Rate at which users return  
- **Task Success**: Productivity empowerment 

These dimensions are measured independently, but they interact with each other as shown below:

:::image type="content" source="/media/usage-overview/heartfunnel3.png" alt-text="Image that displays the funnel relationship between HEART dimensions":::


- Adoption, engagement, and retention form a user activity funnel. Only a portion of users who adopt the tool come back to use it.
- Task success is the driver that progresses users down the funnel and moves them from adoption to retention.
- Happiness is an outcome of the other dimensions and not a stand-alone measurement. Users who have progressed down the funnel and are showing a higher level of activity should ideally be happier.   


## Get Started
### Prerequisites
 - Azure subscription: [Create an Azure subscription for free](https://azure.microsoft.com/free/)
 - Application Insights resource: [Create an Application Insights resource](create-workspace-resource.md#create-workspace-based-resource)
 - Instrument the below attributes to calculate HEART metrics:

  | Source          | Attribute            | Description                                |
  |-----------------|----------------------|--------------------------------------------|
  | customEvents    | user_AuthenticatedId | unique authenticated user identifier       |
  | customEvents    | session_Id           | unique session identifier                  |
  | customEvents    | appName              | unique Application Insights app identifier |
  | customEvents    | itemType             | category of customEvents record            |
  | customEvents    | timestamp            | datetime of event                          |
  | customEvents    | operation_Id         | correlate telemetry events                 |
  | customEvents    | user_Id         	    | unique user identifier                  	  |
  | customEvents*   | parentId             | name of feature                            |
  | customEvents*   | pageName             | name of page                               |
  | customEvents*   | actionType           | category of Click Analytics record         |
  | pageViews       | user_AuthenticatedId | unique authenticated user identifier       |
  | pageViews       | session_Id           | unique session identifier                  |
  | pageViews       | appName              | unique Application Insights app identifier |
  | pageViews       | timestamp            | datetime of event                          |
  | pageViews       | operation_Id         | correlate telemetry events                 |
  | pageViews       | user_Id            	 | unique user identifier                     |

*Use the [Click Analytics Auto collection plugin](javascript-click-analytics-plugin.md) via npm to emit these attributes.
 
### Open the Workbook
The workbook can be found in the gallery under 'public templates'. The workbook will be shown in the section titled **"Product Analytics using the Click Analytics Plugin"** as shown in the following image:

![workbookgallery](./media/usage-overview/gallery.png)

Users will notice that there are seven workbooks as shown in the following image:

![Workbooktabs](./media/usage-overview/heartworkbooktemplates.png)  

The workbook is designed in a way that users only have to interact with the main workbook, 'HEART Analytics - All Sections'. This workbook contains the rest of the six workbooks as tabs. If needed, users can access the individual workbooks related to reach tab through the gallery as well.


### Confirm data is flowing

See the "Development Requirements" tab as shown below to validate that data is flowing as expected to light up the metrics accurately. 

![WorkbookPreview](./media/usage-overview/workbookpreview3.png)  

## Workbook Structure
The workbook shows metric trends for the HEART dimensions split over eight tabs. Each tab contains descriptions of the metrics and how to use them.

A brief description of the tabs can be seen below: 

- **Summary Tab** - Usage funnel metrics giving a high-level view of visits, interactions, and repeat usage. 
- **Adoption** - This tab helps understand what is the penetration among the target audience, acquisition velocity, and total user base. 
- **Engagement** -  Frequency, depth, and breadth of usage. 
- **Retention** - Repeat usage 
- **Task Success** - Enabling understanding of user flows and their time distributions. 
- **Happiness** -  We recommend using a survey tool to measure customer satisfaction score (CSAT) over a 5-point scale. In this tab, we've provided the likelihood of happiness by using usage and performance metrics. 
- **Feature Metrics** - Enables understanding of HEART metrics at feature granularity. 

> ⚠️ **Warning**:
> The HEART Workbook is currently built on logs and effectively are [log-based metrics](pre-aggregated-metrics-log-metrics.md). The accuracy of these metrics will be negatively affected by sampling and filtering.
## How HEART dimensions are defined and measured

### Happiness

Happiness is a user-reported dimension that measures how users feel about the product offered to them. 

A common approach to measure hapiness is to ask users a Customer Satisfaction (CSAT) question like *How satisfied are you with this product?*. Users' responses on a three or a five-point scale (for example, *no, maybe,* and *yes*) are aggregated to create a product-level score ranging from 1-5. Since user-initiated feedback tends to be negatively biased, HEART tracks happiness from surveys displayed to users at pre-defined intervals.

Common happiness metrics include values such as *Average Star Rating* and *Customer Satisfaction Score*. Send these values to Azure Monitor using one of the custom ingestion methods described in [Custom sources](../agents/data-sources.md#custom-sources).




### Engagement
Engagement is a measure of user activity, specifically intentional user actions such as clicks. Active usage can be broken down into three subdimensions:
1. **Activity Frequency** – Measures how often a user interacts with the product. For example, user typically interacts daily, weekly, or monthly.
2. **Activity Breadth** – Measures the number of features users interact with over a given time period. For example, users interacted with a total of five features in June 2021.
3. **Activity Depth** – Measures the number of features users interact with each time they launch the product. For example, users interacted with two features on every launch.

Measuring engagement can vary based on the type of product being used. For example, a product like Microsoft Teams is expected to have a high daily usage, making it an important metric to track. But for a product like a paycheck portal, measurement would make more sense at a monthly or weekly level.

>[!IMPORTANT]
>A user who does an intentional action such as clicking a button or typing an input is counted as an active user. For this reason, Engagement metrics require the [Click Analytics plugin for Application Insights](javascript-click-analytics-plugin.md) implemented in the application.



### Adoption

Adoption enables understanding of penetration among the relevant users, who are we gaining as our user base, and how we are gaining them. Adoption metrics are useful for measuring the below scenarios:

a. Newly released products  
b. Newly updated products  
c. Marketing campaigns  



### Retention
A Retained User is a user who was active in a specified reporting period and its previous reporting period. Retention is typically measured with the following metrics:

| Metric         | Definition                                                                          | Question Answered                                              |
|----------------|-------------------------------------------------------------------------------------|----------------------------------------------------------------|
| Retained users | Count of active users who were also Active the previous period                      | How many users are staying engaged with the product?        |
| Retention      | Proportion of active users from the previous period who are also Active this period | What percent of users are staying engaged with the product? |

>[!IMPORTANT]
>Since active users must have at least one telemetry event with an actionType, Retention metrics require the [Click Analytics plugin for Application Insights](javascript-click-analytics-plugin.md) implemented in the application


### Task Success
Task Success tracks whether users can do a task efficiently and effectively using the product's features. Many products include structures that are designed to funnel users through completing a task. Some examples include:
- Add items to a cart and then complete a purchase
- Search a keyword and then click on a result
- Start a new account and then complete account registration

A successful task meets three requirements:
- Expected Task Flow - The intended task flow of the feature was completed by the user and aligns with the expected task flow.
- High Performance - The intended functionality of the feature was accomplished in a reasonable amount of time.
- High Reliability – The intended functionality of the feature was accomplished without failure.

A task is considered unsuccessful if any of the above requirements isn't met.

>[!IMPORTANT]
>Task Success metrics require the [Click Analytics plugin for Application Insights](javascript-click-analytics-plugin.md) implemented in the application.

Set up a custom task using the below parameters.

| Parameter         | Description                                                                                                                                                                                                                         |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| First Step           | The feature that starts the task. Using the cart/purchase example above, "adding items to a cart" would be the First Step.                                                                                                          |
| Expected Task Duration | The time window to consider a completed task a success. Any tasks completed outside of this constraint is considered a failure. Not all tasks necessarily have a time constraint: for such tasks, select "No Time Expectation". |
| Last Step        | The feature that completes the task. Using the cart/purchase example above, "purchasing items from the cart" would be the Last Step.                                                                                               |










## FAQ 

### How do I view the data at different grains? (Daily, Monthly, Weekly)?
You can click on the 'Date Grain' filter to change the grain (As shown below)

![grainfaq](./media/usage-overview/monthlygrainfaq.png)  

### How do I access insights from my application that aren't available on the HEART workbooks?

You can dig into the data that feeds the HEART workbook if the visuals don't answer all your questions. To do this task, navigate to 'Logs' under 'Monitoring' section and query the customEvents table. Some of the click analytics attributes are contained within the customDimensions field. A sample query is shown in the image below:

![logfaq](./media/usage-overview/logquery.png)  


### Can I edit visuals in the workbook?

Yes, when you click on the public template of the workbook, you can navigate to the top-left corner, click edit, and make your changes.

![workbookeditfaq](./media/usage-overview/workbookeditfaq2.png) 

After making your changes, click 'Done Editing' and then the 'Save' icon.


![workbookeditsave](./media/usage-overview/workbookeditfaq2.5.png)  

To view your saved workbook, navigate to the 'Workbooks' section under 'Monitoring', and then click on the 'Workbooks' tab instead of the 'Public templates' tab. You'll see a copy of your customized workbook there (Shown below). You can make any further changes you want on this particular copy.

![workbookstab](./media/usage-overview/workbookeditfaq3.png)  
 

 

## Next Steps
- Learn more about [Google's HEART framework](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/36299.pdf).
- Set up the [Click Analytics Auto Collection Plugin](javascript-click-analytics-plugin.md) via npm.
- Check out the [GitHub Repository](https://github.com/microsoft/ApplicationInsights-JS/tree/master/extensions/applicationinsights-clickanalytics-js) and [NPM Package](https://www.npmjs.com/package/@microsoft/applicationinsights-clickanalytics-js) for the Click Analytics Auto Collection Plugin.
- Use [Events Analysis in Usage Experience](usage-segmentation.md) to analyze top clicks and slice by available dimensions.
- Find click data under content field within customDimensions attribute in CustomEvents table in [Log Analytics](../logs/log-analytics-tutorial.md#write-a-query). See [Sample App](https://go.microsoft.com/fwlink/?linkid=2152871) for more guidance.

 

 
