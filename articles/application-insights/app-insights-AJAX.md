<properties
	pageTitle="Detect, Triage, Diagnose"
	description="Analyse crashes and detect  and diagnose performance issues in your applications"
	authors="alancameronwills"
	services="application-insights"
    documentationCenter=""
	manager="douge"/>

<tags
	ms.service="application-insights"
	ms.workload="tbd"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article" 
	ms.date="11/06/2015"
	ms.author="awills"/>

# AJAX Dependency Collection in Application Insights 
*Application Insights is in preview.*

## How to Get It 


Application Insights automatically tells you about the performance of AJAX calls made by your web page apps. Many modern web apps load their basic structure, then use AJAX calls to load content. Failed or slow AJAX calls leave the users looking at empty web parts or stuck progress bars.

With this feature, you’ll be able to see whether and how often your AJAX-dependent features cause problems. Best of all, you don’t have to do any additional configuration to make it happen. Telemetry about AJAX calls is a function of our JavaScript web client SDK, so make sure you’ve set up your web pages for Application Insights.

## Diagnozing AJAX Issues


How can you find out that you’ve got issues caused by AJAX calls? And how can you use the new features to help you fix them?

Open the browsers blade by clicking Settings, Browsers; or click through the browsers chart, Page View Duration, on the service overview blade.

The three charts: Dependency Calls, Dependency Failures and Dependency Duration, give a nice overview of how your AJAX calls are behaving. Drill into any of these charts to gather more insights.

Note: If you do not see these new charts select the Restore Defaults option.

![AJAX Dependency Charts](./media/AJAX/01-charts.png)

In this case, the dependency is the AJAX server. We track AJAX calls from the browser in the same way that we track other dependencies in the server. If you open the Filters for these charts, you’ll see that we filter on device.type = “Browser.”

## Using the Charts


Dependency Duration: Use the Dependency Duration chart to see how long responses are taking. You can decide what acceptable call durations would be for your application and compare that to the durations you see on the Dependency Duration chart.

An alternative approach would be to insert code to time how long it takes to complete a response to the user, then report it in trackMetric calls. But that would involve investing in some code, whereas you get the AJAX call durations without writing anything. Since AJAX calls are likely to be the longest and least reliable part of an operation, it’s usually enough to measure those alone. So although the exact way to monitor your app’s response time involves writing some app-specific code, this general method is much easier and almost as effective.

Dependency Calls: The Dependency Calls chart provides quick access to the number of calls made. Compare the Dependency Calls chart to Dependency Failures to gain an idea of your failure rate.

Dependency Failures: Depending on where the failure is, this typically indicates customer dissatisfaction. Anything with a response code of 400 or higher is counted as a failure. Filter the charts by URI to compare the total count of calls for that URI with the number of failures, to see how often it happens.

Under the Dependency charts there are a couple grids:

![AJAX Dependency Grids](./media/AJAX/02-grids.png)

Average of Page view duration by Operation name: This grid provides the average amount of time your customer is waiting on a page to load. Drill into a specific operation name to gain more insights and discover what's slowing you down. 

Total of Dependency calls by Dependency: This grid shows AJAX requests grouped by AJAX server name. Click through any chart for more detail. Some web apps just send AJAX calls to their home server, but others call on other servers. If your app is one of the latter, you’ll find it useful to look at this grid.

#### AJAX Dependency Blade

From the single page request details blade you can select a single AJAX call. Selecting a call will open a new dependency blade for this AJAX call. Here you can see specific properties for this AJAX call including full URL. Also on this blade in the Related Items section, you'll find a link to the Page View Blade that this AJAX call was made on. If you landed on an AJAX dependency blade via search, you may want to look into at the page view blade. Because AJAX dependencies are treated the same as other dependency types they are displayed similarly.

![AJAX Details Blade](./media/AJAX/03-details-blade.png)


## Data Quota


Depending on your application, collecting AJAX dependencies could result in a steep increase in your data collection rates. This increase could possibly cause your application to reach its quota sooner.

To see the breakdown of your data volume by data type go to settings and select Quota and Pricing. 

![AJAX Data Quota](./media/AJAX/04-data-quota.png)

You can click into charts to see more details for certain data types. AJAX collection began on December 23, 2015. If you notice a large spike in dependencies around that date, it was highly likely the AJAX collection caused it. 

## Limit Usage

If you want to limit usage you have a few options:

1. Use Sampling: See our documentation page on Sampling.
2. Use the maxAjaxCallsPerView parameter.
    
    `// Default 500 - controls how many ajax calls will be monitored per page view.`
    `// Set to -1 to monitor all ajax calls on the page.`
    `maxAjaxCallsPerView: number;`

3. Turn off AJAX auto collection: Learn how to Opt Out in the following section. 

## Opt Out

AJAX calls are automatically collected. To disable this simply add: disableAjaxTracking: true in your config file. For more details see our documentation.
Marcela doesn't just sit around waiting for alerts. Soon after every redeployment, she takes a look at [response times][perf] - both the overall figure and the table of slowest requests, as well as exception counts.  

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[usage]: app-insights-web-track-usage.md
 
