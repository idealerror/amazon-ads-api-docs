---
title: ADSP Campaign Management API FAQ
description: Frequently asked questions about the ADSP campaign management APIs.
type: guide
interface: api
---

# ADSP Campaign Management API FAQ

## General

### What functionality is supported?

The DSP campaign management APIs support the below features.

* **Customer type**: Amazon self service.
* **Ad group inventory types**: Display, Streaming TV, Online Video
* **Deals**: Preferred, Private Auction, Programmatic Guaranteed, and Amazon Guaranteed (read only)

### What functionality is not supported via the new CM APIs?

The DSP campaign management APIs do not currently support the below features.

* **Customer type**: Amazon managed service.
* **Ad group inventory types**: Audio, Podcast, Digital Out-Of-Home
* **Deals**: Creating or updating campaigns targeting Amazon Guaranteed deals. Read-only support is enabled.

### Why do the APIs use “campaign” and “ad group” whereas the UI uses “order” and “line item”?

Campaigns and ad groups are alternative names for orders and line items. These APIs were designed to more closely align with our Sponsored Ads APIs which use the “campaign” and “ad group” terminology. Even though the object names differ from the UI, the objects and data are the same across the UI and API.

## Campaigns

### I can’t find the goal or productLocation field on campaigns. 

In the new campaign endpoint, only the `kpi` field is required on campaign creation. The value for `goal` is now automatically inferred from the `kpi` value and can be found in the campaign list operation response. As a result, you no longer need to reference the legacy /dsp/goalConfigurations discovery endpoint.

The `productLocation` field has been removed from the DSP campaign management APIs as it is no longer used for optimization.

### When I set automatedBudgetAllocation to false at the campaign level, it is still set to true at the ad group level. Is this expected?

Yes, this is expected behavior and a change from the legacy APIs. In the DSP campaign management APIs, automateBudgetAllocation is now independently set at the campaign and ad group levels. In order for automated budget allocation to be enabled, it must be set to true at both the campaign and ad group levels separately. Note that this is slightly different from the ADSP Console UI, where if automateBudgetAllocation=false at the order level, the line item will automatically change to a manual budget.


### Were the flight spentAmount and remainingAmount fields removed from the new campaign API?

Yes, the flight `spentAmount` and `remainingAmount` fields from the legacy Order API have been removed in the new API. Instead, you can pull flight spend amount from Amazon Marketing Stream’s [ADSP performance data set](guides/amazon-marketing-stream/datasets/adsp-performance).

### I’m not able to edit some campaigns/ad groups. When I try to update them, I receive an AMAZON\_DEAL\_NOT\_SUPPORTED error.

Campaigns and ad groups targeting Amazon Guaranteed deals are supported on a read-only basis. This is because Amazon Guaranteed deals are on a deprecation path, and will be replaced by Programmatic Guaranteed deals instead. 

### Some of my campaigns/ad groups are missing the deliveryStatus and/or state fields. Is this expected?

If your campaign/ad group has not ended, it should never be missing the deliveryStatus or state field. However, there may be some older ended campaigns or ad groups that are missing the deliveryStatus and/or state fields. We recommend updating your logic such that whenever a campaign or ad group has an end date in the past, and these fields are missing, the deliveryStatus becomes ENDED and state becomes INACTIVE.

### Some of my campaigns have goal=OTHER, but the UI shows goal=Awareness. Why?

Some older campaigns were created without any goal value; however, the UI has no option to show a null goal value, so it defaults to Awareness. In this case, the API is correct while the UI is incorrect. This should only affect a smaller number of older, ended campaigns.

### Where can I find the `autoOptimizations` field that was in the legacy /orders API?

The replacement fields in the DSP campaign management APIs are `automateBudgetAllocation` and `automateBidAllocation`. Note that `automateBidAllocation` is read only, and only appears in the response for POST/dsp/v1/campaigns/list. This is because, starting in Sept 2023, there was a change in ADSP optimization settings. Campaigns created after this month could no longer use the `automateBidAllocation` field, so you will only see this field on old legacy campaigns as read only.

## Ad Groups

### I noticed some inventoryTypes are on a deprecation path. Which ones should I use?

We recommend you do not create new ad groups with these inventoryTypes, as they will be removed in the next major version: STANDARD\_DISPLAY, AMAZON\_MOBILE\_DISPLAY, AAP\_MOBILE\_APP, VIDEO. Instead, please use these new inventoryTypes: DISPLAY, ONLINE\_VIDEO, STREAMING\_TV.

### The targetingSettings field on ad groups is missing many target types. Where can I find these?

Only a limited set of targeting fields are in the targetingSettings field on /dsp/v1/adGroups. Most targeting settings are found in our targeting endpoint, [/dsp/v1/targets](dsp-universal-targeting).

### Can I delete ad groups via the API? Can I retrieve deleted ad groups via the API?

Currently, users may only delete ad groups via the Amazon DSP native UI. This is a hard delete, meaning that once deleted, the ad group will no longer be visible in the UI and can no longer be retrieved via API. However, deleted ad group performance data will still exist in reports pulled via both UI and API.

## Targets

### After creating an ad group, there is a delay before I can retrieve the default targets. Is this expected?

This is expected behavior, as the campaign management read APIs are eventually consistent. For retrieving default targets after ad group creation, the p99 time to wait is 20s and maximum is 30s. 

### Why does the structure for audience targeting in the API not match what’s in the UI?

The new API audience targeting structure represents a simplified format that more closely aligns with industry standards and prevents common setup errors. Detailed audience targeting instructions can be found in our [migration](reference/migration-guides/adsp-campaign-management#audience-targeting) and [getting started](guides/dsp/developer-guide#using-audience-targeting) guides.

### What happens if I try to edit an ad group that has the legacy audience targeting structure?

* For line items with more than 10 inclusion and/or 1 exclusion groups, you will receive an error when trying to add new audiences or groups; however, you will be allowed to delete existing audiences and groups.
* For groups that have both inclusion & exclusion audiences, you won't be able to add new audiences to these groups. However, you are allowed to delete these audiences/groups, add new audiences to other groups, and add new groups.

### When adding targets and one fails, the entire request fails. Is this expected?

Yes, currently our /dsp/v1/targets endpoint is atomic, meaning it does not support partial success. If any single target fails, the entire operation will fail. 

### Does the targets API support partial success?

The targets API supports partial success across different targetTypes, but it doesn’t support partial success within a single targetType on a single ad group. This means that if you send a geo target and a domain target and one fails, the other can still succeed. However, if you send two domain targets on one ad group and one target fails, then the whole request will fail.

### Why can’t I change the “state” field on targets from ENABLED to DISABLED?

The “state” field on our /dsp/v1/targets endpoint currently only has one possible value: ENABLED. We do not support any other states. This means that if a target is removed from an ad group, it will no longer appear in the targets list response. It’s not currently possible to have a disabled target on an ad group.

### Which targetTypes support the “negative” attribute?

The `negative` boolean field in /dsp/v1/targets can be either true or false for these target types: audienceTarget, locationTarget, domainTarget, appTarget, productCategoryTarget, productTarget. For all other target types, `negative` must be false, as these target types do not support exclusion targeting.

### The UI supports dspContentRatingTarget and contentGenreTarget for online video ad groups, but the API doesn’t. Why?

While the UI allows you to set dspContentRatingTarget and contentGenreTarget for online video ad groups, these targets don’t actually work (they only work for streaming TV ad groups). Because of this, we chose to block adding these targets via API when inventoryType=ONLINE\_VIDEO. Note that twitchContentRatingTarget does work for online video ad groups.
