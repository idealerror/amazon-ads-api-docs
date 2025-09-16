---
title: Bid controls
description: Amazon Ads API version 1 bid controls reference.
type: guide
interface: api
---

# Bid controls

 Bid controls are a smart bidding feature that automatically adjusts your bid at auction based on the controls that you set, called *bidding strategies*. When activated, bidding strategies work on your behalf by adjusting your bid higher or lower according to the probability of a conversion or sale. 

 Bid controls include the ability to set priorities for top-of-search pages and product pages. Advertisers will continue to have the option to specify an exact or fixed bid that will not be subject to bid adjustments. Bid control strategies and bid adjustments for placement can be used separately or together. Amazon Ads factors in both settings to calculate bids in real time.

 **Note:** Bid controls apply only to Sponsored Products and are not available for Sponsored Brands. You cannot use bid controls together with the Premium bid adjustment (or Bid+) feature. Only one of these features may be used at a time. 

## Using bid controls in the sandbox environment

Bid controls feature is not enabled by default in the sandbox. There is an additional requirement to add a header with API calls using bid controls in the sandbox. The bid controls header does not follow the standard syntax of other headers. The acceptable values for the header that will activate bid controls are: `true` or `yes`. If your header contains other values besides 'true' or 'yes', your request will still be accepted, but bid controls feature will remain deactivated.

```shell
GET /v1/sp/campaigns                                                   

 Host: advertising-api.amazon.com                                  
 Authorization: Bearer Atza|IQEBLjAsAhRmHjNgHpi0U-Dme37rR6CuUpSR…  
 Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.a8358a60…
 Amazon-Advertising-API-Scope: 1834670349583049580398              
 BIDDING_CONTROLS_ON: true
 Content-Type: application/json
 ``` 

## Bidding strategies

There are three bidding strategies available to you at the campaign level.

| Attribute name | Strategy name | Description |
| --- | --- | --- |
| `legacyForSales` | Adjust bidding (down only) | Lowers your bids in real time when your ad may be less likely to convert to a sale. Any campaign created before October 2018 uses this setting by default. |
| `autoForSales` | Adjust bidding (up and down) | Increases your bids (by a maximum of 100%) in real time when your ad may be more likely to convert to a sale, and lowers your bids when less likely to convert to a sale. |
| `manual` | Fixed bid | Uses your exact bid and any manual adjustments you set, and is not subject to adjusted bidding. |

## Bid adjustments by placement

You can enable controls to adjust your bid based on the placement location. Specify a location where you want to use bid controls. The percentage value set is the percentage of the original bid for which you want to have your bid adjustment increased. For example, a 50% adjustment on a $1.00 bid would increase the bid to $1.50 for the opportunity to win a specified placement.

| Predicate | Placement | Percentage increase range |
| --- | --- |
| `placementTop` | Top of search page | 0 - 900% |
| `placementProductPage` | On a product page | 0 - 900% |

## Methods

| Method | URL | Use Case |
| --- | --- | --- |
| GET | `/v1/sp/campaigns/{campaignId}` | Retrieve a campaign by ID |
| GET | `/v1/sp/campaigns` | Retrieve a list of campaigns |
| PUT | `/v1/sp/campaigns <campaign_obj_json>` | Update bid controls for a list of campaigns with the values included in the `campaign_obj_json` |
| POST | `/v1/sp/campaigns` | Create new bid controls for a list of campaigns with the values included in the `campaign_obj_json` |

## Resource representation

### Campaign

```JSON
{
   "campaignType": "sponsoredProducts",
   "dailyBudget": 1.00,
   "endDate": "20190401",
   "campaignId": 248014350279610,
   "name": "Bid_controls_test_campaign",
   "targetingType": "manual",
   "state": "enabled",
   "premiumBidAdjustment": false,
   "startDate": "20190101",
   "bidding": {
       "strategy": "autoForSales",
       "adjustments": [
            {
               "predicate": "placementTop",
               "percentage": 50
            }
        ]
    }
  }
```

## Bid controls and premium bid adjustments (Bid+)
Bid controls replaces premium bid adjustments (also known as Bid+). Premium bid adjustments feature will be deprecated in the future. While Bid+ is still an active feature, it is important to note that you cannot use Bid+ together with bidding controls. Existing campaigns using Bid+ will be converted to the bidding strategy: 'Legacy for sales (down only)' with a 50% adjustment for the top-of-search placement. 

## Reports and exports

Bid control data is presented in the campaign report and in [exports](exports). You can include the `placement` parameter with the campaign performance report to assess performance for top-of-search and product pages. 
