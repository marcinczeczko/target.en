---
description: Several changes occur in your data collection process when enabling Analytics as the reporting source for Target (A4T).
keywords: Recommendations
seo-description: Several changes occur in your data collection process when enabling Analytics as the reporting source for Target (A4T).
seo-title: Before you implement
solution: Target
title: Before you implement
topic: Premium
uuid: fe603a4b-bd61-49f4-b1b7-a0329aa905f5
---

# Before you implement{#before-you-implement}

Several changes occur in your data collection process when enabling Analytics as the reporting source for Target (A4T).

Before you decide to use this integration, review the following sections and consider the impact to your reporting processes:

## Implementation Requirements {#section_A0D2EF18033D4C3997B08A6EBB34C17A}

>[!IMPORTANT]
>
>Before you can begin using A4T, you need to request that your account be provisioned for the integration. Use [this form](https://www.adobe.com/go/audiences) to request to be provisioned.

This A4T integration requires that you implement the following library versions, depending on whether you want to use redirect offers with A4T or not:

| Requirements Needed for A4T | Requirements Needed for Redirect Offers Using A4T |
|--- |--- |
|This integration requires that you implement the following library versions (or newer) if you do not plan on using redirect offers with A4T:<ul><li>Adobe Analytics: appMeasurement.js version 1.7.0</li><li>Experience Cloud Visitor ID Service: visitorAPI.js version 1.8.0</li><li>Adobe Target (depending on your implementation): at.js version 0.9.1 or mbox.js version 61</li></ul>|To use redirect offers with A4T, you must implement the following library versions (or newer):<ul><li>Experience Cloud Visitor ID Service: visitorAPI.js version 2.3.0 or later</li><li>Adobe Analytics: appMeasurement.js version 2.1</li><li>Adobe Target: at.js version 1.6.2 or later</li></ul><br>**Note:** The  mbox.js library does not support redirect offers with A4T. Your implementation must use at.js.|

Download and deployment instructions are listed in [Adobe for Target Implementation](https://marketing.adobe.com/resources/help/en_US/target/a4t/c_a4timplementation.html).

## Things to Know Before You Implement {#section_50D49CC52E11414089C89FB67F9B88F5}

* This integration is enabled on new activities when you select to use Analytics as the reporting source. After you make the implementation changes described in this document, your existing activities are not impacted. 
* The process of setting up Analytics as the reporting source for Target includes several implementation steps, followed by a provisioning step. It is a good idea to read through the process as described below before implementing. After you complete these steps, you will be ready to use Analytics as your reporting source as soon as it is enabled for you. The provisioning process can take up to five business days. 
* The Visitor ID service creates a shared Visitor ID across the Experience Cloud. While it does not replace the Target mboxPC id or Audience Manager UUID, it does replace the way Analytics identifies new visitors. If set up properly, returning Analytics visitors should also be identified via their old Analytics ID to prevent visitor cliffing. Similarly, because the Target mboxPCid remains intact, no Target visitor profile data is lost when you upgrade to the Visitor ID service. 
* The Visitor ID service must execute before your Analytics and Target page code. Make sure that `VisitorAPI.js` appears above the tags for all other Experience Cloud products.

## Latency {#section_9489BE6FD21641A4844E591711E3F813}

After this integration is enabled, you will experience an additional 5-10 minutes of latency in Adobe Analytics. This latency increase allows data from Analytics and Target to be stored on the same hit, allowing you to break down tests by page and site section.

This increase is reflected in all Adobe Analytics services and tools, including the live stream and real-time reporting, and applies in the following scenarios:

* For live stream, real-time reports & API requests, and current data for traffic variables, only hits with a supplemental data ID are delayed. 
* For current data on conversion metrics, finalized data, and data feeds, all hits are delayed an additional 5-7 minutes.

Be aware that the latency increase starts after you implement the Experience Cloud visitor ID service, even if you have not fully implemented this integration.

## Supplemental ID {#section_2C1F745A2B7D41FE9E30915539226E3A}

All Target calls used by an A4T activity to deliver content or record the goal metric must have a corresponding Analytics hit that shares the same supplemental ID for A4T to work properly.

Hits that contain data from Analytics and Target contain a supplemental data ID. You can see this ID in the [Adobe Debugger](https://marketing.adobe.com/resources/help/en_US/sc/implement/?f=debugger) as the `sdid` parameter. For example: `sdid=2F3C18E511F618CC-45F83E994AEE93A0`. This ID is generated anytime the following criteria are in place:

* The visitor ID service is in implemented 
* A version of [!DNL mbox.js] that supports this integration is implemented.

When troubleshooting, be sure to confirm that the supplemental ID is present on Analytics hits. 