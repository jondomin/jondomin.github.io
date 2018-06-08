---
layout: post
title: Sitecore MVC and Resources
date: '2013-12-10 21:45:06'
tags:
- sitecore-mvc
- sitecore
---


Going forward throughout most of this blog I’m going to be providing examples in Sitecore MVC. Exciting right?!  Typical questions I’ve been getting are:  “Hey how do you setup your MVC Site?”, “How do you [insert specific Webform logic] in MVC?” etc.  Well, I’ll give a brief introduction and some helpful resources to get you started and to shed some light on Sitecore MVC.

Let’s start with some basic information.  With the release of Sitecore 7 there has been added support for Microsoft .NET MVC 3 which allows MVC to be a first class rendering engine in Sitecore.  Sitecore MVC is relatively new to the community and I haven’t seen too many implementations out there.  This is a really exciting time to be Sitecore developer because of this added support.  Sitecore handles MVC framework really well and makes it a very flexible system to develop in.  One of the nice things about setting up your site in MVC, is that you don’t have to exclusively use MVC/razor syntax on all of your pages if you don’t want to.  You can have both MVC and Webforms in the same project if you have a need to do so. It is pretty impressive stuff.

The MVC design pattern has been around for a long time and .NET MVC has become the framework of choice by many developers.

Here is how to get started:

### Microsoft .NET MVC Jump Start
Back in September 2013 Microsoft Virtual Academy had a live Jump Start in .NET MVC.  This goes over the foundations of Microsoft .NET MVC and I highly recommend checking it out here: 

[http://www.microsoftvirtualacademy.com/Content/ViewContent.aspx?et=4099&m=4090&ct=19595#?fbid=k-yIw7Lu6S6](http://www.microsoftvirtualacademy.com/Content/ViewContent.aspx?et=4099&m=4090&ct=19595#?fbid=k-yIw7Lu6S6)

Note that these videos are labeled with MVC 4.

The important ones to look are the following: 
> 1. Introduction to MVC 4
2. Developing ASP.NET MVC 4 Models
3. Developing MVC 4 Controllers
4. Developing ASP.NET MVC 4 Views
5. Integrating JavaScript and MVC 4 (Specifically partial actions and ajax)

##Sitecore SDN Documentation
If you are a Sitecore Certified Developer go to the Sitecore Developer Network and download the Sitecore MVC documentation in the Sitecore 7 Documentation section.  Start by reading that in order to get an understanding of how MVC was implemented in Sitecore.

##Sitecore Launch site
Update: Next step is to check out the awesome demo site at [http://www.launchsitecore.net](http://www.launchsitecore.net/). This is a great demo site and is written in MVC.

If you are looking for other Architecture starting points for Sitecore I would also suggest [Sitecore Habitat](https://github.com/Sitecore/Habitat)

Hopefully these resources give you a starting point to get started with Sitecore MVC.  In my upcoming blog post I’ll be more hands on and delving into code.

 


