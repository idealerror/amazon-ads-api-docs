---
title: Amazon Attribution API troubleshooting
description: Troubleshooting issues with the Amazon Attribution API
type: guide
interface: api
tags:
    - Amazon Attribution
keywords:
    - troubleshooting
    - FAQ
    - frequently asked questions
---

# Troubleshooting issues with the Amazon Attribution API

This troubleshooting guide is divided into three sections: Account authorization, tag implementation, and reporting. Please read through questions on this page top to bottom, confirming you have implemented tags properly and then confirming expectations around reporting.

## Account authorization

In order to use the Amazon Attribution API, your API developer account must have access to an advertiser’s advertising profile. Please refer to [Getting Started](guides/amazon-attribution/get-started) for authorization methods. Note some aspects of setup (e.g. URL domain names) may be different depending upon your advertiser’s geographic region.

#### I called the profiles endpoint but I don’t see my advertiser’s profile in the response

Your advertiser must grant you access to their profile, and then the profile should appear in the profiles endpoint response list within two hours. If you're using the API test domain, you should use the production domain instead. Please refer to [Getting Started](guides/amazon-attribution/get-started) and confirm that you have followed all setup steps.

#### I get a 403 error when I call an Amazon Attribution endpoint using my advertiser’s profile, even though I am authorized

This advertiser profile may not be eligible for Amazon Attribution. See the [overview](guides/amazon-attribution/overview) for eligibility requirements.

## Tag Implementation

#### I clicked on my tag, but the campaign isn’t showing up in reporting at all

This could be related to the following:

* You may have applied the tag to an invalid landing page. Tags will only function when applied to an Amazon product detail page or store page.
* You must call a tag retrieval endpoint with an advertiser’s profile before you can measure campaigns for that advertiser. This is because the endpoint has hidden side-effects which enable measurement. If you skipped that step for that advertiser, retrieve tags and try again.
* Campaign creation may take up to four hours after the first click. Try waiting four hours and look again.
* You may have altered the tag in a manner that interfered with its functionality. Ensure all original parameters are included without duplicates and the URL is properly formatted.

#### I clicked on my tag, and I see the campaign in reporting, but clicks and conversions are all zero

This could be related to the following:

* You may need to wait 24 hours before clicks appear, and up to 48 hours for conversions.
* Our reporting threshold will mask (as zero) all metrics for data rows with <10 clicks. After the tag has been clicked ten times, metrics should appear within 48 hours.

#### I see my campaign in reporting with some clicks but no conversions (DPVs, ATCs, sales)

This may be due to the following reasons:

* Conversions may take up to 48 hours to populate reporting. Try waiting and looking again.
* You applied a tag from one marketplace to a landing page in a different marketplace. For example, a tag for a US account applied to a Canadian landing page.
* You applied a tag from one advertiser profile to a product or store page for products belonging to a different advertiser profile. A tag will only measure conversions for products belonging to the selling account associated with that tag’s profile. 
* Your tag used parameter values which were too long or used illegal characters. Values must be less than 255 characters (after macros are expanded) and must not contain the following characters, even if they are URL-escaped: /, &, =, #, ?, +, ’, ”, %, $.
* Your `aa_campaign_id` value was re-used by a different advertiser or in a different market. Make sure to use a unique campaign id per advertiser per market.
* You applied a tag to a URL which contains a `ref_` and/or `tag` parameter. Because the Attribution tag uses its own `ref_` and `tag` parameters, the one(s) in your URL are interfering with Amazon Attribution’s ability to measure your campaign. Delete the `ref_` or `tag` parameters from your URL, leaving the ones in the Attribution tag.
* Your landing page was an Amazon store that did not have any ASINs assigned to it. If your Amazon store page only uses dynamic widgets that choose which ASINs to show in real-time (rather than the same ones every time) then Amazon Attribution may not associate those ASINs with your campaign. To work around this, either add static ASINs to your store page or use a different landing page.
* You altered the tag in a way that interfered with its functionality, such as removing a parameter or you appended it to a URL without following standard URL formatting rules. A valid URL format will contain (in order): an Amazon domain, a path to a product or stores page, a `?` character, and then a list of parameter name=value pairs each separated by an `&` character.

#### Reporting shows a campaign called {campaignid} or something similar – why isn’t it showing my actual campaign id?

The phrase in curly brackets is a macro intended for a specific publisher who should replace it with a campaign ID value. If you see curly brackets show up in reporting instead of an actual ID value, that means the publisher could not process the macro. This may be due to the following reasons:

* The tag may have been intended for a different publisher (e.g. a Facebook tag used on a Google campaign).
* The tag may have been implemented in the wrong field in the publisher’s ad manager, or something else interfered with the publisher’s processing of the macro, such as improper URL escaping in a wrapped tag.
* If you’re using Amazon Attribution’s non-macro tags, your own system must fill in the id parameters when preparing the link.

#### I used the same tag on multiple landing pages – OR I moved a tag from one landing page to a different landing page – and the metrics don’t look right

Each tag may only ever be used on one landing page. If you’d like to tag multiple landing pages within the same ad group or move a tag to a new landing page, use a unique creative id for each landing page but use the same ad group id and campaign id.

## Reporting

#### Why does my recent reporting look incomplete? Is there a delay?

Reporting for a given date should be considered partially complete until 48 hours after that date. Generally, clicks are reported within 24 hours. If you implemented your tag correctly, you may simply need to wait and you will see your metrics show up in reporting.

#### Why have some of my old conversion metrics changed since I first pulled reporting?

Metrics may be restated up to 28 days later, for example to adjust for product returns. We recommend you occasionally pull the most recent 28 days of reporting and update any data you have stored for that time period.

#### My campaign is driving to a store page – why don’t all products on the page get associated with my campaign as promoted products?

If your Amazon store page only uses dynamic widgets that choose which ASINs to show in real-time (rather than the same ones every time) then Amazon Attribution may not associate those ASINs with your campaign. To work around this, either add static ASINs to your store page or use a different landing page.

#### Why am I not seeing a certain metric or dimension in my report?

Each report type supports a specific selection of dimensions and metrics and the one you’re looking for may not be supported for the report type you’ve selected. For example, the Products report does not include click metrics or the aa_creativeid dimension, but the Performance report does. View the available dimensions and metrics for each report by visiting the [ReportRequestBody parameters and descriptions](amazon-attribution-prod-3p/#/Reports/getAttributionTagsByCampaign).

| Report type | Included dimensions | Aggregation level | Metrics | Available reporting dates |
|--------|---------|---------|--------|--------|
| Performance | `aa_campaignid`, `aa_adgroupid`, `aa_creativeid`, `publisher` | You can choose to aggregate at the CAMPAIGN, ADGROUP, or CREATIVE level (default: CREATIVE) | Click-throughs and all Promoted and Total conversion metrics. See [Report endpoint](amazon-attribution-prod-3p/#tag/Reports/operation/getAttributionTagsByCampaign) for a complete list. | Last 13 months |
| Products | `aa_campaignid`, `aa_adgroupid`, `publisher`, `productName`, `productGroup`, `productCategory`, `productSubcategory`, `brandName`, `productAsin`, `productConversionType` | Always aggregated at ad group level | Only "top 100 Products" Promoted and Brand halo conversion metrics, no click-throughs. (See [Report endpoint](amazon-attribution-prod-3p/#tag/Reports/operation/getAttributionTagsByCampaign) for a complete list.) Limited to top 100 products. | Last 90 days |

#### Why am I getting an error when I pull a Products report for days past 90 days ago?

Due to data availability limits, you can only pull a Products report where the startDate is within the last 90 days

#### Why am I getting an error when I try to pull in all my reporting at once?

If you have a lot of data, we recommend using pagination to pull reporting in batches. You can include the “count” parameter to indicate how many records you’d like to pull per batch, and use the “cursorId” parameter to pull subsequent batches. See the report endpoint reference for details.

#### Why am I not seeing view-through conversions?**

Amazon Attribution API only attributes conversions to clicks, not impressions.

#### Why does my Amazon Attribution campaign reporting show a different number of clicks compared to my ad publisher’s campaign reporting?

This may occur if Amazon Attribution tags were added to only some ads within the campaign. Alternatively, if you’ve implemented the tag as a tracking template, be aware that if multiple tracking templates are implemented at different levels within the same campaign, only the lowest level template will fire – so other tracking templates may be firing instead of your Amazon Attribution tag.

#### Why does my API reporting show different numbers compared to the Amazon Ads console?

This discrepancy is probably the result of our reporting aggregation threshold. To protect shopper privacy, Amazon Attribution will mask data for any reporting row representing a tag or group of tags with < 10 clicks over its lifetime. For example, reporting aggregated at the ad group level will show all data for ad groups with 10+ clicks, even if some of the tags in the ad group have < 10 clicks. Until the click threshold has been surpassed, masked rows will show metric values as zero. Campaigns with very small budgets or shown to a very small audience may take some time to surpass this threshold.

To display reporting at campaign or ad group level, you should request performance reports at these specific aggregation levels, rather than aggregating the data yourself, to ensure your reporting includes all available data and matches the Amazon Ads console.

#### If I want to include third-party publisher dimensions in my reporting, can I use different macros than what comes in the default tag?

Yes, you can modify the tags and add or remove macros to suit your needs. If you’d like to do this, we recommend appending them at the end of the `aa_creativeid` parameter value, using an underscore or dash to separate them from the actual creative id. If you’d like to remove any macros you may do so as well, provided you ensure that the final creative id value will be unique.

Please keep in mind that our reporting threshold will mask reporting on any data row which represents a tag or a group of tags with <10 clicks, so do not add macros which will cause your tag to become too detailed (e.g., several macros in a row) or correspond to small audiences or individuals (e.g., a user id or click id) or reporting won’t be able to surpass the threshold.
