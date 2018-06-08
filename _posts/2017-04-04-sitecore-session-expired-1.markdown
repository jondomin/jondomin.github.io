---
layout: post
title: Sitecore Session Expired 1
date: '2017-04-04 16:34:09'
---

##Sitecore Session Timout="1"
If you ever experience your Sitecore Sessions expiring after 1 minute check that you have   `@Html.Sitecore().VisitorIdentification()` in the `<head>` of your Sitecore Main Layout.

##Reproducing this Issue
I recently ran into an instance of Sitecore where the Content Delivery (CD) Environment was configured to use SQL Server for its Session State Configuration. Interestingly this issue did not occur on the Content Management Server as it was using InProc session management.  This means I can only reproduce this issue if the SQL Session State configuration is not set to InProc.

Anyways, we had some code on the site where we were storing some data in session and after 1 minute we were losing the session data.  This was very odd as the session state configuration in the web.config had set the timeout to 20 minutes:

`<sessionState mode="Custom" customProvider="mssql" cookieless="false" timeout="20">`

##Why this happens
After some investigation we found out that there is another setting which sets the Session Timeout for Robots to 1 minute

`<setting name="Analytics.Robots.SessionTimeout" value="1" />`

We then realized that if you do not have `@Html.Sitecore().VisitorIdentification()` in your layout then all visitors will be considered Robots and Sitecore their session will expire after 1 minute (or whatever the Analytics.Robots.SessionTimeout value is).

##Resolution
Easy steps to resolve this issue are as follows:

* Add a Reference in your Web Application to `Sitecore.Mvc.Analytics`
* In your layout register this namespace `@using Sitecore.Mvc.Analytics.Extensions`
* In the `<head>` include `@Html.Sitecore().VisitorIdentification()`
