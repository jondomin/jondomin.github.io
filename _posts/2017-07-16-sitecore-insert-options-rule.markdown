---
layout: post
title: Sitecore Insert Options Rule
image: "/img/2017/10/InsertRules.png"
date: '2017-07-16 17:40:00'
tags:
- sitecore
- sitecore-insert-options
- sitecore-rules
---

There are two ways to configure insert options.  The first and is the typical way we are taught as Sitecore Developers is to go to a template's **__Standard Values** Item and in the Ribbon click 

*Configure -> Assign Insert Options*.

![](/img/2017/10/AssignInsertOption.png)

## Insert Option Rules
The second way, which is probably the lesser known way, is to configure insert options through the system rules.

This is found here:

**/sitecore/system/Settings/Rules/Insert Options/Rules**

In this folder you can create custom rules like so

![](/img/2017/10/InsertRules.png)

## The Benefit

So when is this usefully?  Well, I created this rule so I don't have to package the */sitecore/content* item if I wanted to install this on an different environment or project.  I plan to reuse as many boilerplate templates that I can on a new project.  Now all I have to do is generate a package with my templates and new insert option rules, and I won't have to modify the */sitecore/content* item on a fresh install of Sitecore.