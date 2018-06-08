---
layout: post
title: Sitecore 7 Language Fallback Revisited and Glass
date: '2014-07-09 12:03:43'
tags:
- glass-mapper
- sitecore-7
- sitecore
- language-fallback
---

I recently implemented Language Fallback on a Sitecore 7.1 site and decided to share some of the things I ran into.   Like with all things programming, this may not be the best solution, however this may give you some guidance as to how to handle some of the same problems when implementing this in Sitecore 7.1.


## Code Reuse

Why invent the wheel when we have monster truck tires at our disposal?  The monster truck tire I’m referring to is the highly rated [Language Fallback](https://marketplace.sitecore.net/en/Modules/Language_Fallback.aspx "Language FallBack") Sitecore Marketplace Module that does exactly what I need, minus the small fact that it is not supported for Sitecore 7 yet. So let’s see what we can do about that!

<iframe src="//giphy.com/embed/v4sExiGkE7OF2?html5=true" width="480" height="224" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/cheezburger-motivational-follow-your-dreams-monster-trucks-v4sExiGkE7OF2">via GIPHY</a></p>

The Language Fallback concept is simple. When an item you are requesting in a specific language isn’t available, fallback to another specified language.  The documentation tab on the marketplace page provides you with a link to [Alex Syba blog post](http://sitecoreblog.alexshyba.com/2010/11/approaching-language-fallback-with.html "Approaching Language Fallback in Sitecore") that explains this concept very well.  For the purposes of the my project we simply need to fallback to English Language for our Spanish site for all items.

## Upgrade the Source

To do so I downloaded the Source for Dmitry Kostenko’s Language Fallback Item Provider module.

![language fallback](/img/2016/04/2015-03-25-10_19_36-Language-Fallback-Sitecore-Marketplace-1024x678.png)


Once added the Visual Studio project to my solution I had to change the Target Framework to 4.5 to be compatible with Sitecore 7 and Sitecore Kernel.  To do so edit the Properties of the project.

![Net Framework 4.5](/img/2016/04/2014-07-09-10_44_18-BCU-Microsoft-Visual-Studio-Administrator.png)

From there I installed the package that is provided in the source zip, setup the English fallback on my Spanish language item under /sitecore/system/languages, and tested it out.

No luck,  our Spanish item’s weren’t falling back still.


## Glass.Mapper

So we are using [Glass.Mapper](http://glass.lu/ "Glass Mapper") throughout our site as our ORM solution.  We love it and it works fantastic.  After some time digging around we narrowed down the problem to the

> Glass.Mapper.Sc.Pipelines.Response.GetModel **processor that returns the Glass casted item.

The reason being is that the **GetObject**  method is returning null when it calls

scContext.CreateType(type, item, false, false, null);

Snippet below:

<script src="https://gist.github.com/jldeveloper27/fae4d88ee40ae08957de412c682e68c1.js"></script>


## Solution

I simply modified the **LanguageFallbackitemProvider.cs ** **GetItem **method

<script src="https://gist.github.com/jldeveloper27/7d5579ddda6ed4a0e8d46e65ad87977a.js"></script>

Since we didn’t need the extra properties that StubItem class gave us we simply just returned the fallback item instead.

 protected override Item GetItem(ID itemId, Language language, Version version, Database database) { ..... return GetItem(itemId, fallbackLanguage, Version.Latest, database); }

Viola! The Items were properly falling back and Glass was playing nice with the Language Fallback now.

<iframe src="//giphy.com/embed/2n8480RCQ2jBe?html5=true" width="480" height="368" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/dancing-happy-jimmy-fallon-2n8480RCQ2jBe">via GIPHY</a></p>
