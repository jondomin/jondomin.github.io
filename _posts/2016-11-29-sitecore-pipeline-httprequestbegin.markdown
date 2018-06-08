---
layout: post
title: Sitecore Pipeline - httpRequestBegin
image: "/content/images/2016/11/image00-1.png"
date: '2016-11-29 22:51:47'
tags:
- sitecore
- sitecore-pipelines
- httprequestbegin
- sitecore-patches
---


As a Sitecore developer it is very common to create custom processors in the httpRequestBegin pipeline. This pipeline gives you the ability to execute some custom code for each httpRequest of your site. From my experience this is where a lot can go wrong with Sitecore implementations if not done correctly.

Here are a few recommendations when creating custom processors in the httpRequestBegin pipeline. Pipelines are a nice convenient way for you to execute a process at certain events or throughout the application's lifecycle.  The httpRequestBegin pipeline give syou the ability to execute some custom code for each HTTP request on your site with some Sitecore context.  From my experience, a lot can go wrong if you implement a custom pipeline improperly.

Here are a few recommendations when creating custom processors in the httpRequestBegin pipeline.

##Cache as much as possible
Since these pipelines are executed for each request, it is recommended that you cache as much data as you can in your pipelines.  This will avoid putting unnecessary load on your database. Also, getting data out of the cache is typically must faster than executing database queries.

##Bouncer at the Start
At the beginning of all processors I make sure to have a lot of checks/conditionals to make sure only to execute my code when I intend to.  This is like having a bouncer at the door that lets HTTP requests into your custom logic.  I only want certain requests to go through my processor.  If I can avoid executing my code altogether that will help performance.

##Where to Patch the Processor
Know where to patch your processor in the httpRequestBegin pipeline.  I never place a custom processor first in the <httpRequestBegin> pipeline.

I typically place the custom processor after the ItemResolver processor.

`<processor type="Sitecore.Pipelines.HttpRequest.ItemResolver, Sitecore.Kernal" />`

The ItemResolver will attempt to resolve the item.  If it is found, it will set it in the httpRequestArgs.  A good time to add a processor is after you have resolved the item in the current page context.

##Example
I recently created a 301 Redirect Processor which executes 301 redirects under specific criteria.  I patched this in the `<httpRequestBegin>` pipeline, tested t, and called it a day.

however, at the same point the following week I realized the Sitecore Experienced Editor would load very slowly, around 30 seconds on each load. Yikes!

![Sitecore Experience Editor](/img/2016/11/image00.png)


After some diagnostics, I realized that all .js and .css files in the Experience Editor were taking a few seconds to load.  I found out that the 301 Redirect processor was the first processor registered in the `<httpRequestBegin>` pipeline.

This was executing even if it was a physical file, and it would check if there was a 301 redirect match. In addition, the 301 redirect was doing a very costly ContentSearch API query to the SearchProvider. I then proceeded with the following fixes:

* Patch the processor to execute after the ItemResolver.
* Check the file system if the url resolved to a file on the file system.
* Through this diagnostic I optimized the 301 Redirect checks to use a HashTable and story redirects in cache.

With those simple changes the response times for physical files went from **200 ms** to less than **1 ms**

###Sitecore Example Patch
><script src="https://gist.github.com/jldeveloper27/06c40788f9a110736bd6f4a5c2736010.js"></script>

##Conclusion
If you are experiencing issues on each and every page request, look for any custom processors in your pipelines for they may be the culprit.


For more information on Pipelines in Sitecore check out these John West articles.


[All About Pipelines in Sitecore ASP.NET CMS](http://www.sitecore.net/learn/blogs/technical-blogs/john-west-sitecore-blog/posts/2011/05/all-about-pipelines-in-the-sitecore-aspnet-cms.aspx)


[Important Pipelines in the Sitecore ASP.NET CMS](http://www.sitecore.net/learn/blogs/technical-blogs/john-west-sitecore-blog/posts/2011/05/important-pipelines-in-the-sitecore-aspnet-cms.aspx)



 


