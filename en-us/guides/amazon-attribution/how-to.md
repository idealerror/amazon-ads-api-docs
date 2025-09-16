---
title: Amazon Attribution API how-to
description: How to get use the Amazon Attribution API
type: guide
interface: api
tags:
    - Amazon Attribution
keywords:
    - how to
    - attribution resources
---

# How to use the Amazon Attribution API

Once you have been granted permission to an advertiser's profile you can use the Amazon Attribution API to read the following resources associated with your advertiser:

* Profiles
* Advertisers
* Publishers 
* Attribution tags 
* Reports

## Profile resource

A profile represents an advertiser account in a particular market. Each time you make a call to an Amazon Attribution API endpoint you must pass a profile ID as the `Amazon-Advertising-Api-Scope` header. Retrieve the list of profiles you can access by calling [/v2/profiles](reference/2/profiles). 

## Advertisers resource

> [WARNING] The Amazon Attribution Advertisers resource will be deprecated in a future version of the API. It is currently supported on all profiles to ensure backward compatibility. Integrators supporting only ad console profiles should not use this resource.

Get the [Advertisers resource](amazon-attribution-prod-3p/#tag/Advertisers) to retrieve a list of Amazon Attribution "advertisers” associated with a profile. Ad console profiles will only have a single advertiser. DSP-based profiles for vendor advertisers may have multiple Amazon Attribution advertisers, but sellers will always have only one. You may want to pass an `advertiserId` as an optional parameter to one of the following endpoints.

## Publishers resource

Get the [Publishers resource](amazon-attribution-prod-3p/#tag/Publishers) to retrieve a list of all supported publishers. Each item in the list includes publisher name, publisher ID, and whether or not the publisher’s tag includes macros, also known as dynamic parameters. The Publishers resource is identical for all advertisers. You will pass publisher IDs when calling Attribution tag-retrieval endpoints.

If you do not see a publisher listed, contact the API support team at [ads-api-support@amazon.com](ads-api-support@amazon.com).

## Attribution tags resource

Once you have your advertiser’s `profileId` and the list of publishers, choose which [Attribution tags resource](amazon-attribution-prod-3p/#tag/Attribution-Tags) you will call to retrieve tags. 

Amazon Attribution offers tags that include macros for publishers such as Google Ads, Facebook, Microsoft Ads, and Pinterest. For these publishers, you will call `/attribution/tags/macroTag`. These tags will only function on the intended publisher and will use campaign ID values provided by that publisher.

For publishers that do not support macro tags, retrieve tags by calling `/attribution/tags/nonMacroTemplateTag`. Non-macro tags require that you provide unique campaign ID values in the tag parameters.

> [WARNING] Calling a tag-retrieval endpoint is a mandatory step required to enable measurement for each profile you plan to work with. Do not skip this step or else tags will not function.<br> <br>Amazon Attribution’s API tags are different than Amazon Attribution tags created in the Amazon Ads console. Do not modify tags created in the Amazon Ads console for use with the API and do not try to use API tags to add new tags to a campaign created in the Amazon Ads console.

### Macro tags

Using a macro tag for a given publisher and advertiser, you can use that same tag across all of the advertiser's campaigns on that publisher. Macro tags allow the publisher to dynamically insert campaign information into the click-through URL when preparing the ad for display. Each tag contains publisher-specific macros that vary depending on the publisher.

For example, here’s a hypothetical macro tag for Google Ads returned from a call to the macroTag endpoint:

```bash
"?maas=maas_adg_api_123456789_macro_1_99&ref_=aa_maas&tag=maas&aa_campaignid={campaignid}&aa_adgroupid={adgroupid}&aa_creativeid=ad-{creative}_{targetid}_dev-{device}_ext-{feeditemid}"
```

Here is an example landing page URL that the campaign will drive to:

```bash
https://www.amazon.com/AmazonBasics-Performance-Alkaline-Batteries-Count/dp/B00MNV8E0C
```

After the tag is applied on the campaign in the publisher’s ad manager, when the publisher prepares each ad for display it will dynamically replace the `{campaignid}`, `{adgroupid}`, `{creative}`, `{targetid}`, `{device}`, and `{feeditemid}` macros with the actual campaign values.

In this example, the resulting click-through URL would be:

```bash
https://www.amazon.com/AmazonBasics-Performance-Alkaline-Batteries-Count/dp/B00MNV8E0C?maas=maas_adg_api_123456789_macro_1_99&ref_=aa_maas&tag=maas&aa_campaignid=1234&aa_adgroupid=6789&aa_creativeid=ad-1357_kwd-123_dev-c_ext-2468
```

### Non-macro tags

You can also use non-macro tags for publishers that do not support dynamic parameters to enable Amazon Attribution measurement on ads that drive to an Amazon Store or Product Detail page. To prepare your non-macro tags for implementation, you first retrieve the advertiser’s non-macro template tag by calling the nonMacroTemplateTag endpoint and then your system can fill in the template values for the `aa_campaignid`, `aa_adgroupid`, and `aa_creativeid` URL parameters each time it needs to create a new tag.

Here’s an example non-macro tag template. It will not function unless you replace the template values.

```bash
?maas=maas_adg_api_123456789_static_9_99&ref_=aa_maas&tag=maas&aa_campaignid={campaignId}&aa_adgroupid={adgroupId}&aa_creativeid={creativeId}
```

> [NOTE] When using non-macro tags, the same tag cannot be used across more than one landing page URL. If you need to change the landing page for a live tag, prepare a new tag using a unique creative ID and implement this new tag on the updated landing page.

>[WARNING]Values for `aa_campaignid`, `aa_adgroupid`, and `aa_creativeid` parameters must be no longer than 255 characters (after all macros are expanded) and must not contain the following characters, even if they are URL-escaped: /, &, =, #, ?, +, ’, ”, %, $.<br> <br>`aa_campaignid` values must be unique per account. Do not re-use the same campaign id among multiple advertisers or across multiple markets. If using macro tags, take care to organize your 3P campaigns so each Amazon account and market gets a separate campaign.

## Attribution tag implementation

Attribution tags can be implemented in any manner that results in a valid URL that is called when the ad is clicked, as long as the landing page is an Amazon Store or Product Detail Page. Here are some common ways to implement attribution tags.

You may append an attribution tag to the end of a landing page URL and put the resulting URL into a publisher’s click-through URL field. Some publishers also allow for URL parameters to be kept separate from the landing page and will combine them dynamically.

Amazon Attribution tags can be appended to a landing page URL used organically in a blog post or email, just as you would typically create a link to Amazon.

If using a macro tag, it must be placed in a field that supports macro processing. This is publisher-specific, so check your publisher’s help documentation if you are having issues.

For some publishers, you can implement attribution tags as click-trackers that fire upon each ad click. These tracking fields are different for each publisher. Remember that attribution tags will only work when the landing page is an Amazon Store or Product Detail Page.

For Social campaigns, such as Facebook, you can implement your advertiser's attribution tags within the "URL parameters" tracking field.

For Search campaigns, such as Google Ads and Microsoft Ads, implement your advertiser's attribution tags within the tracking template field, preceded by the `{unescapedlpurl}` macro for Google or the `{lpurl}` macro for Microsoft. Here is a Google example:

```bash
{unescapedlpurl}?maas=maas_adg_api_123456789_macro_1_99&ref_=aa_maas&tag=maas&aa_campaignid={campaignid}&aa_adgroupid={adgroupid}&aa_creativeid=ad-{creative}_{targetid}_dev-{device}_ext-{feeditemid}
```

If no other tracking templates will interfere, you can implement the attribution tag at the campaign level. However, keep in mind that if multiple tracking templates are implemented at different levels of a campaign, only the tracking template at the lowest level fires.

> [WARNING] Remove any pre-existing `ref_` or `tag` parameters from the click-through URL so they do not interfere with Amazon Attribution’s `ref_` and `tag` parameters. If two parameters with the same name are included in the URL, measurement will not be accurate.

## Measurement

When an Amazon Store or Product Detail Page URL including Amazon Attribution API tag parameters is clicked, this triggers Amazon Attribution to infer the structure of the campaign and measure click-attributed conversion events on Amazon. For a given profile, only links with Amazon Attribution tags from that profile are considered for conversion attribution and the last-clicked tag in the 14-day window gets full credit. Conversions will not be attributed to Amazon ads or any other links not tagged by the given profile.

Amazon Attribution will automatically create an Amazon Attribution campaign, ad group, and creative using the values from the `aa_campaignid`, `aa_adgroupid`, and `aa_creativeid` parameters. After campaigns are created, promoted products will be automatically associated to the campaign to enable measurement of conversion metrics on these products. Clicks to Product Detail pages auto-associate that product and its variations to the campaign. Clicks to Amazon Stores pages auto-associate products listed on the Stores page to the campaign.

Once campaigns, ad groups, and creatives are auto-created and products are auto-associated to the campaign, Amazon Attribution measures and reports clicks, conversions, and sales. Conversions on the associated products will show in reporting as “promoted” conversion metrics. “Total” and “brand halo” conversion metrics will reflect conversions on non-promoted products in the same brand.

> [NOTE] If your Store page uses dynamic widgets that select products to display in real-time, these products may not be associated to your campaign by Amazon Attribution. Examples include dynamic widgets “Best selling,” “Recommended for you,” “Featured Deals,” or product grids/lists powered by search keywords. To work around this limitation, add the ASINs you want to measure directly to your Store page.


## Reports resource

Once attribution tags have been implemented and shoppers have clicked your links, call the [report endpoint](amazon-attribution-prod-3p/#tag/Reports/operation/getAttributionTagsByCampaign) to retrieve reporting. Clicks may take up to 24 hours to appear, and conversions may take up to 48 hours. 

Two types of reports are available: Performance and Products. You can access each report type by entering the optional `reportType` request body property with a value of PERFORMANCE (default) or PRODUCTS. Each report type differs in terms of its aggregation level, dimensions, and metrics. For the performance report, you may choose the aggregation level by passing the `groupBy` request body property with a value of CAMPAIGN, ADGROUP, or CREATIVE (default). The products report is always aggregated at ad group level and is limited to the top 100 products per campaign, as ranked by sales, purchases, and DPVs.

If you don’t indicate which metrics you want the report to include, the response will include all available metrics (except `brb_bonus_amount`). For more efficient reporting you can include a list of desired metrics in your request body. 

>[TIP:Brand Referal Bonus metric]For advertisers enrolled in the [Brand Referral Bonus](https://sellercentral.amazon.com/gp/help/external/L9HPJ34VBFP76HX) (BRB) program, you can request the `brb_bonus_amount` metric in a performance report, provided you explicitly ask for the metric and pass a `groupBy` value of CAMPAIGN or ADGROUP. Requesting this metric for advertisers not enrolled in BRB does not result in an error – the response will simply not include the metric.

View the available dimensions and metrics for each report type by visiting the [ReportRequestBody documentation](amazon-attribution-prod-3p/#tag/Reports/operation/getAttributionTagsByCampaign).

| Report type | Included dimensions | Aggregation level | Metrics | Available reporting dates |
|--------|---------|---------|--------|--------|
| Performance | `aa_campaignid`, `aa_adgroupid`, `aa_creativeid`, `publisher` | You can choose to aggregate at the CAMPAIGN, ADGROUP, or CREATIVE level (default: CREATIVE) | Click-throughs and all Promoted and Total conversion metrics. See [Report endpoint](amazon-attribution-prod-3p/#/Reports/getAttributionTagsByCampaign) for a complete list. | Last 13 months |
| Products | `aa_campaignid`, `aa_adgroupid`, `publisher`, `productName`, `productGroup`, `productCategory`, `productSubcategory`, `brandName`, `productAsin`, `productConversionType` | Always aggregated at ad group level | Promoted and Brand halo conversion metrics, Brand Referal Bonus. No click-throughs. See [Report endpoint](amazon-attribution-prod-3p/#tag/Reports/operation/getAttributionTagsByCampaign) for a complete list. | Last 90 days |

Amazon updates Amazon Attribution reporting at least once per day and performs historical restatements every 1, 7, and 28 days to ensure reporting is updated with the most recent conversions and takes into account returns or refunds. We recommend that you retrieve the last 30 days of reporting each day.

>[NOTE]To protect shopper privacy, Amazon Attribution will mask data for any reporting row representing a tag or group of tags with < 10 clicks over its lifetime. For example, reporting aggregated at the ad group level will show all data for ad groups with 10+ clicks, even if some of the tags in the ad group have < 10 clicks. Until the click threshold has been surpassed, masked rows will show metric values as zero. Campaigns with very small budgets or shown to a very small audience may take some time to surpass this threshold.<br> <br>To display reporting at campaign or ad group level, you should request performance reports at these specific aggregation levels, rather than aggregating the data yourself, to ensure your reporting includes all available data and matches the Amazon Ads console.


## Next steps

If you encounter problems using the Amazon Attribution API, the [troubleshooting guide](guides/amazon-attribution/troubleshooting) can help you diagnose and correct issues.
