---
title: Migrating to the new Amazon DSP Campaign Management API
description: A migration guide for moving to new Amazon DSP campaign resources 
type: guide
interface: api
tags: 
    - Amazon DSP
    - Campaign management
keywords:
    - Migration guide
---

# Migrating to the new Amazon DSP Campaign Management API

## Overview

The new campaign management APIs enable self-serve advertisers and their partners to programmatically manage their Amazon DSP campaign data (campaigns, ad groups, targeting). These new APIs replace the [previous campaign management APIs](https://advertising.amazon.com/API/docs/en-us/dsp-campaigns).

## New API Highlights

* The campaign list endpoint now includes flights in the response so that you can read complete campaign data for up to 100 campaigns at a time. 
* Campaign and ad group endpoints support PATCH operations (partial updates) so you can update individual fields (e.g. budget) without sending all fields. 
* Targeting has its own endpoint and supports adding or removing individual targets so you don’t have to replace the full array of existing targets.
* The new APIs are available worldwide in all marketplaces where ADSP operates.

## Endpoint Equivalencies

| Old endpoint | New endpoint |
| -----------  | ------------ |
| GET/dsp/lineItems/ | POST/dsp/v1/adGroups/list <br /> POST/dsp/v1/targets/list |
| POST/dsp/lineItems/ | POST/dsp/v1/adGroups <br /> POST/dsp/v1/targets <br /> POST/dsp/v1/targets/deleteByObject | 
| PUT/dsp/lineItems/ | PATCH/dsp/v1/adGroups |
| GET/dsp/lineItems/{lineItemId} | POST/dsp/v1/adGroups/list | 
| GET/dsp/targeting/domain | POST/dsp/v1/targets/list | 
| PUT/dsp/targeting/domain | POST/dsp/v1/targets <br / > POST/dsp/v1/targets/deleteByObject | 
| POST/dsp/lineItems/{lineItemId}/deliveryActivationStatus | PATCH/dsp/v1/adGroups | 
| GET/dsp/orders/ | POST/dsp/v1/campaigns/list |
| POST/dsp/orders/ | POST/dsp/v1/campaigns | 
| PUT/dsp/orders/ | PATCH/dsp/v1/campaigns |
| GET/dsp/orders/{orderId} | POST/dsp/v1/campaigns/list |
| n/a | POST/dsp/v1/campaigns/{campaignId}/flights/list | 
| n/a | POST/dsp/v1/campaigns/{campaignId}/flights/delete | 
| n/a | POST/dsp/v1/campaigns/{campaignId}/flights | 
| n/a | PATCH/dsp/v1/campaigns/{campaignId}/flights | 
| GET/dsp/orders/{orderId}/conversionTracking | N/A | 
| PUT/dsp/orders/{orderId}/conversionTracking | N/A | 
| GET/dsp/orders/{orderId}/conversionTracking/pixels | Pixels have been deprecated and replaced by Amazon Ad Tags. |
| PUT/dsp/orders/{orderId}/conversionTracking/pixels | Pixels have been deprecated and replaced by Amazon Ad Tags. | 
| GET/dsp/orders/{orderId}/conversionTracking/products | GET/dsp/v1/campaigns/{campaignId}/conversionTracking/products | 
| PUT/dsp/orders/{orderId}/conversionTracking/products | POST/dsp/v1/campaigns/{campaignId}/conversionTracking/products <br /> POST/dsp/v1/campaigns/{campaignId}/conversionTracking/products/delete | 
| GET/dsp/orders/{orderId}/conversionTracking/products/export | N/A |
| POST/dsp/orders/{orderId}/deliveryActivationStatus | PATCH/dsp/v1/campaigns | 
| GET/dsp/productCategories | POST/dsp/v1/advertisedProductCategories/list | 
| GET/dsp/supplySources | POST/dsp/v1/inventorySourceExchanges/list <br /> POST/dsp/inventorySourceDeals/list | 
| GET/dsp/iabContentCategories | POST/dsp/v1/iabContentCategories/list | 
| GET/dsp/preBidTargeting/doubleVerify/customContextualSegments	 | POST/dsp/v1/thirdPartyTargetSegments/list |
| GET/dsp/preBidTargeting/oracleDataCloud/customPredicts | POST/dsp/v1/thirdPartyTargetSegments/list |
| GET/dsp/preBidTargeting/oracleDataCloud/standardPredicts | POST/dsp/v1/thirdPartyTargetSegments/list |
| GET/dsp/apps | POST/dsp/v1/apps/list | 
| GET/dsp/domainLists | POST/dsp/v1/domainLists/list | 
| N/A | POST/dsp/v1/products/list | 
| N/A | POST/dsp/v1/productCategories/list | 
| GET/dsp/pixels | Pixels have been deprecated and replaced by Amazon Ad Tags. | 	
| GET/dsp/goalConfigurations | N/A | 



## Entity naming

In the new endpoints, `orders` have been renamed to `campaigns`, and `line items` have been renamed to `ad groups`. The identifiers for these objects remain the same.

## Authorization

The new endpoints require an Advertiser header (`Amazon-Ads-AccountId`). This is your Amazon DSP advertiser ID, which can be retrieved from the [advertiser endpoint](https://advertising.amazon.com/API/docs/en-us/dsp-advertiser#tag/Advertiser/paths/~1dsp~1advertisers/get). 

## Update operations

The previous campaign and ad group endpoints used the PUT operation for updates which required a full replacement of all fields. The new endpoints utilize PATCH (partial updates) which means you can send only the fields you wish to update, and any fields not included in the request will remain unchanged. 

## Read operations

The previous endpoints used the GET operation to read resources; and orders and line items had two separate GET endpoints: 1) a batch read endpoint that returned up to 100 orders/line items for a subset of fields, and 2) a read endpoint that returned a single order/line item with all fields. The new endpoints now support the ability to read up to 100 campaigns and ad groups with all fields included in the response. The batch read endpoints are implemented as POST /list operations which allows you to use filters in the request body. 

## Campaign flights

Flights are now returned in the campaign list operation. Because all campaigns must have at least one flight, flights are a required field in the campaign create operation. However, once a campaign is created, you must use the separate [flight endpoints](https://advertising.amazon.com/API/docs/en-us/dsp-ad-group-campaign-flights)to update, add, or remove flights. The new flights endpoint allows you to add or remove individual flights without sending the full list of all existing flights. If you need to delete, update, and create new flights, the sequence should be delete first, then update, then create.

The flight object’s `spentAmount` and `remainingAmount` fields are no longer returned in the campaign list endpoint. We recommend you retrieve spend data using the ADSP report API.

## Campaign optimization

In the new campaign endpoint, only the `kpi` field is required on campaign creation. The value for `goal` is automatically inferred from the `kpi` value and can be found in the campaign list operation response. As a result, you no longer need to reference the legacy /dsp/goalConfigurations discovery endpoint. 

The `productLocation` field has been removed from the new APIs as it is no longer used for optimization. 

## Ad group inventoryType

The field called "lineItemType" in the previous API is now called "inventoryType" in dsp/v1/adGroups. 

These inventoryTypes are scheduled for deprecation on June 30, 2025: STANDARD\_DISPLAY, AMAZON\_MOBILE\_DISPLAY, AAP\_MOBILE\_APP, VIDEO. We recommend you do not create new ad groups with these inventoryTypes. Instead, use these inventoryTypes: DISPLAY, ONLINE\_VIDEO, STREAMING\_TV.

## Targeting

Targeting is no longer part of the line item object and can be managed in it’s own dedicated endpoint: [/dsp/v1/targets](https://advertising.amazon.com/API/docs/en-us/dsp-universal-targeting). This new endpoint allows you to add multiple targets across multiple ad groups within a single request. It also allows you to add or remove individual targets without sending all the existing targets.

## Audience targeting

The structure of audience targeting has changed from the previous API. The new structure is simplified and less likely to lead to errors during setup. 

New audience structure:

* Up to 500 audiences can be associated per ad group.
* All audiences within the same group must either be included or excluded.
* You can specify up to 10 inclusion groups and 1 exclusion group.
* To add audiences to a group, you must specify a groupId. Only numbers 1 through 11 (formatted as strings) are accepted (e.g., "1").  
* We removed the `intraOperator` and `interOperator` fields from the write operations. Instead, the boolean logic will be set automatically: 
    * Audiences within an inclusion group are joined by OR/ANY logic. Audiences within the exclusion group are joined by AND/ALL logic. This means that users can be in any of the audiences inside a group to be included or excluded.
    * Groups are joined with AND logic. 
* We have future plans to update the ADSP UI to match this simplified workflow. Until then, ad groups can be set up with the old structure in the UI. To enable you to read the old structure, the /dsp/v1/targets/list endpoint has the legacy fields `inGroupOperator` and `acrossGroupOperator`. These fields cannot be included in write operations as the operators are set automatically.

What happens if I try to edit an ad group that does not follow the new structure?

* For line items with more than 10 inclusion and/or 1 exclusion groups, you will receive an error when trying to add new audiences or groups; however, you will be allowed to delete existing audiences and groups.
* For groups that have both inclusion & exclusion audiences, you won't be able to add new audiences to these groups. However, you are allowed to delete these audiences/groups, add new audiences to other groups, and add new groups.

### Audience targeting example - legacy API

This example is a truncated request that sets audience targeting using the legacy /dsp/lineItems endpoint:

```
"segmentTargeting": {
    "segmentGroups": [
        {
            "segments": [
                {
                    "segmentId": "430698405022557158",
                    "isNot": false
                },
                {
                    "segmentId": "364952100378102160",
                    "isNot": false
                }
            ],
            "intraOperator": "AND",
            "interOperator": "ANY"
        },
        {
            "segments": [
                {
                    "segmentId": "380754703451245334",
                    "isNot": true
                },
                {
                    "segmentId": "398654571333370664",
                    "isNot": true
                }
            ],
            "intraOperator": "AND",
            "interOperator": "ALL"
        }
    ]
}
```

### Audience targeting example - new API

This example sets the same audience targeting using the new /dsp/v1/targets endpoint:

```
{
    "targets": [
        {
            "adGroupId": "581831922576209192",
            "adProduct": "AMAZON_DSP",
            "negative": false,
            "state": "ENABLED",
            "targetDetails": {
                "audienceTarget": {
                    "audienceId": "430698405022557158",
                    "groupId": "1",
                    "targetType": "AUDIENCE"
                }
            }
        },
        {
            "adGroupId": "581831922576209192",
            "adProduct": "AMAZON_DSP",
            "negative": false,
            "state": "ENABLED",
            "targetDetails": {
                "audienceTarget": {
                    "audienceId": "364952100378102160",
                    "groupId": "1",
                    "targetType": "AUDIENCE"
                }
            }
        },
        {
            "adGroupId": "581831922576209192",
            "adProduct": "AMAZON_DSP",
            "negative": true,
            "state": "ENABLED",
            "targetDetails": {
                "audienceTarget": {
                    "audienceId": "380754703451245334",
                    "groupId": "2",
                    "targetType": "AUDIENCE"
                }
            }
        },
        {
            "adGroupId": "581831922576209192",
            "adProduct": "AMAZON_DSP",
            "negative": true,
            "state": "ENABLED",
            "targetDetails": {
                "audienceTarget": {
                    "audienceId": "398654571333370664",
                    "groupId": "2",
                    "targetType": "AUDIENCE"
                }
            }
        }
    ]
}
```

## Contextual targeting

The new APIs include a new type of contextual targeting. Through Amazon's retail taxonomy of over 40K product categories, advertisers can select specific Amazon products (ASINs) or categories (browse nodes), and express the type of content where they want their ads shown on [Amazon.com](http://amazon.com/) and 3rd party supply. To retrieve the contextual targeting IDs, use these discovery endpoints:

- Products: [/dsp/v1/products/list](https://advertising.amazon.com/API/docs/en-us/dsp-category-discovery#tag/ContextualTargeting-Product-Discovery/operation/dspContextualTargetingProductOptionsV1)
- Product Categories: [/dsp/v1/productCategories/list](https://advertising.amazon.com/API/docs/en-us/dsp-category-discovery#tag/ContextualTargeting-ProductCategory-Discovery/operation/dspContextualTargetingProductCategoryOptionsV1)

Once the IDs have been retrieved, you can use [/dsp/v1/targets](https://advertising.amazon.com/API/docs/en-us/dsp-universal-targeting#tag/Targets/operation/CreateDspTargetsV1) (using the `productTarget` and `productCategoryTarget` objects) to add the targets to an ad group.

## Deal IDs

Deals now have a new ID format. To retrieve a list of deal IDs that can be used in the /dsp/v1/targets endpoint, use the new [deal discovery API](https://advertising.amazon.com/API/docs/en-us/dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals). Deal IDs returned from the legacy [/dsp/supplySources](https://advertising.amazon.com/API/docs/en-us/dsp-campaigns#tag/Discovery/operation/getSupplySources) are not supported in the /dsp/v1/targets endpoint.

## Discovery APIs

The new APIs include new versions of all the [legacy discovery APIs](https://advertising.amazon.com/API/docs/en-us/dsp-campaigns#tag/Discovery) except for these:

|Legacy Discovery API	|Status	|Function	|Note	|
|---	|---	|---	|---	|
|[GET /dsp/pixels](dsp-campaigns#tag/Discovery/operation/getPixels)	|Deprecation path	|Gets list of pixels that can be used for off-Amazon conversion tracking.	|Pixels were deprecated as of Jan '24 and can no longer be added to campaigns.	|
|[GET /dsp/goalConfigurations](dsp-campaigns#tag/Discovery/operation/getGoalConfigurations)	|Deprecation path	|Gets list of possible configurations for campaign goals.	|Campaign goals were simplified and there is no longer a need for this endpoint.	|
|[GET /dsp/geoLocations](dsp-discovery-geo)	|Active	|Gets list of geolocation IDs that can be targeted on an ad group.	|Users will continue to use this endpoint as it didn't require any updates.	|
|[POST /audiences/list](audiences#tag/Discovery/operation/listAudiences)	|Active	|Gets list of audience IDs that can be targeted on an ad group.	|Users will continue to use this endpoint as it didn't require any updates.	|
|[POST /audiences/taxonomy/list](audiences#tag/Discovery/operation/fetchTaxonomy)	|Active	|Gets the taxonomy for audiences.	|Users will continue to use this endpoint as it didn't require any updates.	|

## Inventory Discovery 

The legacy /dsp/supplySources endpoint has been replaced by two new endpoints: 

|New Endpoint	|Function	|
|---	|---	|
|[/dsp/inventorySourceDeals/list](https://advertising.amazon.com/API/docs/en-us/dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals)	|Gets a list of deal IDs that can be targeted on an ad group.	|
|[/dsp/v1/inventorySourceExchanges/list](https://advertising.amazon.com/API/docs/en-us/dsp-discovery-inventory-source-exchanges)	|Gets a list of Amazon owned & operated, Amazon Publisher Direct, and third-party exchange inventory source Ids that can be targeted on an ad group.	|

Note that the `supplySourceType` field has been renamed to `inventorySourceType` and has new enum values:

|supplySourceType (legacy)	|inventorySourceType (new)	|New endpoint	|Note	|
|---	|---	|---	|---	|
|AMAZON_EXCLUSIVE	|AMAZON	|[/dsp/v1/inventorySourceExchanges/list](https://advertising.amazon.com/API/docs/en-us/dsp-discovery-inventory-source-exchanges)	|This refers only to Amazon owned & operated inventory and no longer includes Amazon Publisher Direct which is now a separate category.	|
|AMAZON_EXCLUSIVE	|AMAZON\_PUBLISHER\_DIRECT	|[/dsp/v1/inventorySourceExchanges/list](https://advertising.amazon.com/API/docs/en-us/dsp-discovery-inventory-source-exchanges)	|This now has its own category. APD provides direct access to thousands of premium third-party video, display, and audio publishers around the world.	|
|OPEN_EXCHANGE	|THIRD\_PARTY\_EXCHANGE	|[/dsp/v1/inventorySourceExchanges/list](https://advertising.amazon.com/API/docs/en-us/dsp-discovery-inventory-source-exchanges)	|This refers to third-party exchanges such as OpenX and Pubmatic.	|
|DEAL	|N/A	|[/dsp/inventorySourceDeals/list](https://advertising.amazon.com/API/docs/en-us/dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals)	|Deal Ids are now retrieved in a separate endpoint.	|

## Programmatic Guaranteed deals

The new APIs include support for Programmatic Guaranteed (PG) deals. PG deals are guaranteed, negotiated deals, which include pre-specified date ranges, fixed impression goals, predefined inventory with targeting parameters, and fixed rates. Most targeting and other ad group settings (e.g. frequency caps) are disabled when targeting PG deals because these settings are typically built into the PG deal itself.

To create a PG deal targeted ad group:
1) Use [/dsp/inventorySourceDeals/list](https://advertising.amazon.com/API/docs/en-us/dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals) with `DEAL_TYPE`=`PROGRAMMATIC_GUARANTEED` to find the deal ID.
2) Send the deal ID in [/dsp/v1/adGroups](https://advertising.amazon.com/API/docs/en-us/dsp-ad-group-and-campaign#tag/Ad-Group), using field `targetingSettings.targetedPGDealId`, when creating or editing an ad group.

## Versioning

The new APIs utilize path-based versioning instead of header-based versioning. All new endpoints begin with /dsp/v1/. You no longer need to send an `Accept` header.

## Date format

The new APIs use the RFC3339 profile of ISO-8601 (YYYY-MM-DDThh:mm:ssTZD) as their date format.

## Conversion tracking

Pixels were deprecated at the end of 2023 and can no longer be used on new campaigns. Campaigns tracking off-Amazon conversions should now associate conversion definitions to an order using [this endpoint](https://advertising.amazon.com/API/docs/en-us/dsp-conversion-builder#tag/Conversion-Order-Association/operation/dspAmazonUpdateAssociatedConversionDefinitionsForOrder). 

To retrieve the full list of products used for conversion tracking, you no longer need to export the list as a file. Instead, you can paginate through the full list using [this endpoint](https://advertising.amazon.com/API/docs/en-us/dsp-ad-group-cvrtracking-products#tag/Product-Conversion-Tracking/operation/DspGetCampaignConversionTrackingProductsV1).

## Campaign and ad group activation

Campaigns and ad groups are created in an inactive state and must be activated before they can begin serving. In the old APIs, activation was done via a separate [deliveryActivationStatus endpoint](https://advertising.amazon.com/API/docs/en-us/dsp-campaigns#tag/LineItem/operation/setLineItemStatus). In the new APIs, activation is done via the PATCH operation on the campaign or ad group endpoints.

New API example - set ad group to active:

```
PATCH /dsp/v1/adGroups

{
    "adGroups": [
        {
            "adGroupId": "581831922576209192",
            "state": "ACTIVE"
        }
    ]
}
```

## Error model

The error model has been improved. Key changes include:

* Removed `fieldName`, `errorDetails`, and top level `message`.
* Renamed `errorType` to `errorCode`.
* Renamed `message` to `errorMessage`.
* New fields:
    * `success`: This array contains the list of successful operations and their corresponding index.
    * `index`: This is the position of the operation in your original request.
* Added `requestId` to response for successful calls. This should be included when submitting support tickets so that our support team can find the request in our logs.
* If `success` or `error` array is empty, the array will be omitted from the response.

## Defaults

As with the legacy APIs, certain default values are set automatically. In the new APIs, it’s not possible to override these defaults when creating a campaign or ad group. You can view and edit the default values by using the APIs listed below.

|Workflow	|API used to read the defaults	|API used to edit the defaults	|Field	|API Default value	|Notes	|
|---	|---	|---	|---	|---	|---	|
|Campaign & ad group creation	|POST /dsp/v1/campaigns/list <br /> POST /dsp/v1/adGroups/list	|PATCH /dsp/v1/campaigns <br /> PATCH /dsp/v1/adGroups	|state	|INACTIVE	|It's not possible to set state=`ACTIVE` during the creation workflow.	|
|Ad group creation and update	|POST /dsp/v1/adGroups/list	|PATCH /dsp/v1/adGroups	|budgetAmount	|The value varies based on optimization.	|Users are not allowed to edit budgetAmount when automateBudgetAllocation=true at both the campaign and ad group levels. Instead, it's set automatically by our optimization systems. To edit budgetAmount, change automateBudgetAllocation to false.	|
|Ad group creation	|POST /dsp/v1/targets/list	|POST /dsp/v1/targets <br /> POST /dsp/v1/targets/deleteByObject |dayPartTarget	|All dayparts are included.	|	|
|Ad group creation	|POST /dsp/v1/targets/list	|POST /dsp/v1/targets <br /> POST /dsp/v1/targets/deleteByObject |deviceTarget	|All eligible devices are included.	|	|
|Ad group creation	|POST /dsp/v1/targets/list	|POST /dsp/v1/targets <br /> POST /dsp/v1/targets/deleteByObject |inventorySourceTarget	|All eligible inventory sources (excluding deals and inventory groups) are included.	|	|
|Ad group creation	|POST /dsp/v1/targets/list	|POST /dsp/v1/targets <br /> POST /dsp/v1/targets/deleteByObject |domainTarget	|`advertiserDomainList.inheritFromAdvertiser=true`	|This does not apply when inventoryType=AAP\_MOBILE\_APP because these ad groups don't support domain targeting.	|
|Ad group creation	|POST /dsp/v1/targets/list	|POST /dsp/v1/targets <br/ > POST /dsp/v1/targets/deleteByObject |videoAdFormatTarget	|All ad formats are included.	|This only applies when inventoryType=VIDEO.	|
