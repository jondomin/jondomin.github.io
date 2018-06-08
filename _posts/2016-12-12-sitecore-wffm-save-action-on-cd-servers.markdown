---
layout: post
title: Sitecore WFFM Save Action on CD servers
image: "/content/images/2016/12/image00.png"
date: '2016-12-12 21:53:21'
tags:
- sitecore
- sitecore-save-action
- sitecore-web-forms-for-marketers
- wffm
- sitecore-wffm
- sitecore-cd-wffm
---

##Web Forms for Marketers Custom Save Action
Web Forms for Marketers Module (wffm) lets content authors/marketers create forms in the Sitecore Admintration and insert those forms on pages of their choosing.  


##Custom Save Actions
In a recent project I had to submit the web forms for marketers form submissions to an external system.  To accomplish this I created a Custom Save Action in web forms for marketers.  It is pretty straight forward.  Create a class that derives from **Sitecore.WFFM.Abstractions.Actions.ISaveAction** interface or **Sitecore.WFFM.Actions.Base.WffmSaveAction** abstract class.  From there you need to register this in Sitecore Admin.


## Content Delivery Server’s not executing save action
Once we deployed the code to production we realized that the Save Action was not executing.  After much investigation we found a sparsely documented feature of WFFM.


> You have to define which Save Actions execute on the CD servers!


The way WFFM works is that it submits form submssions to your storage provider choice (MongoDB or SQL) and once the user’s session ends it will then make it’s way to the Reporting Database in the xDB architecture.

From Sitecore's documentation site:
> This field is only used in the staging environment. If this check box is cleared, this save action is transferred from the Slave to the Master server and is performed there. If this check box is selected, a save action is performed on the Slave server.

Save Actions that you want to execute on the CD servers need to be set to do so.


##Solution
Go to your Custom Save action and check the “Client Action”


![Client Action Save Action](/content/images/2016/12/image00.png)

While this does put some extra load on your CD server it will execute the Save Action immediately when the form submission occurs on the CD server.

### Resources

[Sitecore Documentation: Create a new save action](https://doc.sitecore.net/web_forms_for_marketers/working_with_actions_and_validations/save_actions/create_a_new_save_action)

[Sitecore Save Action item Fields](https://doc.sitecore.net/web_forms_for_marketers/working_with_actions_and_validations/save_actions/the_save_action_item_fields)
