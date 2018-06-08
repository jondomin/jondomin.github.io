---
layout: post
title: Sitecore User Group Chicago and Item Buckets
date: '2013-11-14 01:04:24'
tags:
- chicago-sitecore
- item-buckets
- sitecore-cms
- sitecore-user-group
- sitecore-7
- sitecore
---



## The Event

On October 22, 2013 Americaneagle.com hosted the first ever Sitecore user group in Chicago Illinois. We had a great turnout of people with various backgrounds.  Most were content authors and marketers wanting to learn more about the features of Sitecore.  Time and time again, I hear people wanting to know more about Sitecore 7 and the mysterious DMS.

I presented some information to the group regarding the new features of Sitecore 7 with a technical perspective.


## Sitecore 7 Item Buckets

Item buckets are one of the best new features in Sitecore 7. This feature transforms your normal items into a bucket which act as a container for potentially millions of items. The example I gave was related to managing News or Events in the CMS.

Pre-Sitecore 7 you would have to create a folder structure in the content editor to manage thousands of News Articles. An example might be /sitecore/content/site name/news/2013/11/13/event-name. The extra date folders are necessary because Sitecore recommends no more than 200 items be beneath an item.  Because of this, you can’t have all your News Articles as direct children in your News folder. One of the major down sides is that this makes it difficult to find and manage Events in the content editor.

Enter Item Buckets. Item buckets solve this problem and change the way we think of designing our tree structure. In the News Articles example above, you can do away with the unnecessary 2013/11/13 folders an just create News Articles under the News Bucket. Converting the News folder to an item bucket changes the experience in the content editor. You now are presented with a new interface to search for a News Articles in your bucket and are presented with a nice listing with pagination and Facet Filtering.

[![AdminListing](http://jonathandeveloper.com/wp-content/uploads/2013/11/AdminListing-1024x663.png)](http://jonathandeveloper.com/wp-content/uploads/2013/11/AdminListing.png)


## Pros and Cons

Really, I’ve only seen amazing things with Sitecore 7 and Item buckets.  Some of the new features include:

### Queries

The queries you create in the Item Bucket Interface can be used in data source of a controls to render content.  This will allow you to make a more generic Listing Controls that can populate lists of multiple types of items such as News, Events, News of a specific category, etc.

### Tagging

Now that the parent child relationship is no longer there you can use tagging to maintain relationships.  The semantic tagging system in Item buckets allows you to tag associations on every item.  To do this, make a field on your News template that is a *Multi-list with Search* field.  These fields are now automatically index and usable in your code.

### Cons

The only con to using Sitecore Item buckets is that there can be some overhead when it comes to handling Custom URLS of items in a bucket.  To do something like this you might have to create some pipelines and create a custom LinkProvider and ItemResolver so that your bucketed items URLs are built properly and resolve properly.

 

All in all, this is an amazing feature that changes the way I’m thinking about designing my Sitecore structure, and in the end this makes it more friendly for Content Authors to edit content.

For more information on the Sitecore User Group in Chicago, sign up here: [http://www.meetup.com/Sitecore-User-Group-Chicago/](http://www.meetup.com/Sitecore-User-Group-Chicago/)

 


