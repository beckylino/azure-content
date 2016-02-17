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



## Proactive monitoring  


Marcela doesn't just sit around waiting for alerts. Soon after every redeployment, she takes a look at [response times][perf] - both the overall figure and the table of slowest requests, as well as exception counts.  



![Response time graph and grid of server response times.](./media/app-insights-detect-triage-diagnose/09-dependencies.png)

She can assess the performance effect of every deployment, typically comparing each week with the last. If there's a sudden worsening, she raises that with the relevant developers.


## Triage


Triage - assessing the severity and extent of a problem - is the first step after detection. Should we call out the team at midnight? Or can it be left until the next convenient gap in the backlog? There are some key questions in triage.


How much is it happening? The charts on the Overview blade give some perspective to a problem. For example, the Fabrikam application generated four web test alerts one night. Looking at the chart in the morning, the team could see that there were indeed some red dots, though still most of the tests were green. Drilling into the availability chart, it was clear that all of these intermittent problems were from one test location. This was obviously a network issue affecting only one route, and would most likely clear itself.  


By contrast, a dramatic and stable rise in the graph of exception counts or response times is obviously something to panic about.


A useful triage tactic is Try It Yourself. If you run into the same problem, you know it's real.


What fraction of users are affected? To obtain a rough answer, divide the failure rate by the session count.


![Charts of failed requests and sessions](./media/app-insights-detect-triage-diagnose/10-failureRate.png)

In the case of slow response, compare the table of slowest-responding requests with the usage frequency of each page.


How important is the blocked scenario? If this is a functional problem blocking a particular user story, does it matter much? If customers can't pay their bills, this is serious; if they can't change their screen color preferences, maybe it can wait. The detail of the event or exception, or the identity of the slow page, tells you where customers are having trouble.


## Diagnosis


Diagnosis isn't quite the same as debugging. Before you start tracing through the code, you should have a rough idea of why, where and when the issue is occurring.


**When does it happen?** The historical view provided by the event and metric charts makes it easy to correlate effects with possible causes. If there are intermittent peaks in response time or exception rates, look at the request count: if it peaks at the same time, then it looks like a resource problem. Do you need to assign more CPU or memory? Or is it a dependency that can't manage the load?


**Is it us?**  If you have a sudden drop in performance of a particular type of request - for example when the customer wants an account statement - then there's a possibility it might be an external subsystem rather than your web application. In Metrics Explorer, select the Dependency Failure rate and Dependency Duration rates and compare their histories over the past few hours or days with the problem you detected. If there are correlating changes, then an external subsystem might be to blame.  


![Charts of dependency failure and duration of calls to dependencies](./media/app-insights-detect-triage-diagnose/11-dependencies.png)

Some slow dependency issues are geolocation problems. Fabrikam Bank uses Azure virtual machines, and discovered that they had inadvertently located their web server and account server in different countries. A dramatic improvement was brought about by migrating one of them.


**What did we do?** If the issue doesn't appear to be in a dependency, and if it wasn't always there, it's probably caused by a recent change. The historical perspective provided by the metric and event charts makes it easy to correlate any sudden changes with deployments. That narrows down the search for the problem.


**What's going on?** Some problems occur only rarely and can be difficult to track down by testing offline. All we can do is to try to capture the bug when it occurs live. You can inspect the stack dumps in exception reports. In addition, you can write tracing calls, either with your favourite logging framework or with TrackTrace() or TrackEvent().  


Fabrikam had an intermittent problem with inter-account transfers, but only with certain account types. To understand better what was happening, they inserted TrackTrace() calls at key points in the code, attaching the account type as a property to each call. That made it easy to filter out just those traces in Diagnostic Search. They also attached parameter values as properties and measures to the trace calls.


## Dealing with it


Once you've diagnosed the issue, you can make a plan to fix it. Maybe you need to roll back a recent change, or maybe you can just go ahead and fix it. Once the fix is done, Application Insights will tell you whether you succeeded.  


Fabrikam Bank's development team take a more structured approach to performance measurement than they used to before they used Application Insights.

* They set performance targets in terms of specific measures in the Application Insights overview page.

* They design performance measures into the application from the start, such as the metrics that measure user progress through 'funnels.'  




## Usage

Application Insights can also be used to learn what users do with an app. Once it's running smoothly, the team would like to know which features are the most popular, what users like or have difficulty with, and how often they come back. That will help them prioritize their upcoming work. And they can plan to measure the success of each feature as part of the development cycle. [Read more][usage].

## Your applications

So that's how one team use Application Insights not just to fix individual issues, but to improve their development lifecycle. I hope it has given you some ideas about how Application Insights can help you improve the performance of your own applications.

## Video

[AZURE.VIDEO performance-monitoring-application-insights]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[usage]: app-insights-web-track-usage.md
 
