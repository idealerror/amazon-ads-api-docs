---
title: How to use the Partner Opportunities API
description: Example requests to the Partner Opportunities API, along with information on error codes and messages.
type: guide
interface: api
tags:
    - Partner opportunities
    - Reporting
    - Account management
keywords:
    - opportunity data file
    - excess inventory ASINs
    - Amazon-Advertising-API-Manager-Account
    - application/vnd.partneropportunity.v1+json
---

# How to use the Partner Opportunities API 

The [Partner Opportunities API](https://advertising.amazon.com/API/docs/en-us/partner-opportunities) empowers Amazon Ads Partner Network members to receive automated, data-driven recommendations and insights for their linked advertisers. Partner Opportunities curates insights for all the campaigns partners manage within their entire book of business. With the Partner Opportunities API you can simplify your API call process, save time on managing advertising activities, and better support your clients’ campaigns. This guide will walk you through getting started, calling the Partner Opportunity API, and troubleshooting common issues. 

See the [Partner Opportunities Getting Started guide](guides/recommendations/partner-opportunities/getting-started#partner-network-account-id) for more information. 

## Opportunity catalog

The Partner Opportunities API and UI maintains an opportunity catalog. Partners can use these opportunities (such as unlaunched ASINs or available promotional click credits) to optimize and improve their advertising campaigns. Review our complete [Opportunity Catalog](guides/recommendations/partner-opportunities/catalog) to identify the Partner Opportunities call(s) best suit your needs.

## Call the Partner Opportunities API

### Step 1: Review the opportunity catalog through list opportunities

Get started with the Partner Opportunities API using our [Postman collection](guides/get-started/using-postman-collection). We provide the following resources to support you: [video walkthrough](https://www.youtube.com/watch?v=SWqOPN33phw), Postman guide, and [Github repository](https://github.com/amzn/ads-advanced-tools-docs).

If you’re new to the Amazon Ads API or prefer not to use Postman, complete the Amazon Ads [API onboarding process](guides/get-started/overview) first. Start by using the [ListOpportunities](https://advertising.amazon.com/API/docs/en-us/partner-opportunities#tag/Partner-Opportunities/operation/partnerOpportunitiesListOpportunities) operation to explore the Partner Opportunities API. 

Most opportunities update daily, while some are updated hourly. Before downloading a file, we recommend you verify that `opportunities.dataMetadata.rowCount` is greater than 0 to ensure matching data exists. If not, you may encounter a 404 Not Found Error.

>[NOTE] Use `locale` for translation/localization.
Example: `locale=en_US`

>[NOTE] Use `advertiserId` or `profileId` to filter for specific advertisers you manage.

For more information, see the [technical specification](https://advertising.amazon.com/API/docs/en-us/partner-opportunities#tag/Partner-Opportunities/operation/partnerOpportunitiesListOpportunities) for the `partnerOpportunities` resource. 

#### Request

```
GET https://advertising-api.amazon.com/partnerOpportunities
```

#### Example cURL command

This example request uses `v1` per the `Accept` header. The example response also represents `v1`. For information about other versions, see the [technical specification](partner-opportunities).

```
curl 'https://advertising-api.amazon.com/partnerOpportunities' \
 -H 'Accept: application/vnd.partneropportunity.v1+json' \
 -H 'Amazon-Advertising-API-ClientId: <LWA_APP_CLIENT_ID>' \
 -H 'Amazon-Advertising-API-Manager-Account: <PARTNER_NETWORK_ACCOUNT_ID>' \
 -H 'Authorization: Bearer <ACCESS_TOKEN>'
 ```

 #### Example response

 ```
 {
  "opportunities": [
    {
      "partnerOpportunityId": "opportunity-1-id",
      "title": "Title for opportunity-1",
      "description": "Description for opportunity-1",
      "callToAction": "Action that opportunity-1 recommends you to take",
      "product": "Amazon advertising product that opportunity-1 corresponds to",
      "objective": "Objective that opportunity-1 intends to improve e.g. brand engagement",
      "createdDate": "2022-05-15T23:42:33.823Z",
      "updatedDate": "2022-05-16T23:43:54.867Z",
      "dataUrl": "/partnerOpportunities/opportunity-1-id/file"
    },
    {
      "partnerOpportunityId": "opportunity-2-id",
      "title": "Title for opportunity-2",
      "description": "Description for opportunity-2",
      "callToAction": "Action that opportunity-2 recommends you to take",
      "product": "Amazon advertising product that opportunity-2 corresponds to",
      "objective": "Objective that opportunity-2 intends to improve e.g. sales",
      "createdDate": "2022-05-14T23:42:33.823Z",
      "updatedDate": "2022-05-15T23:43:54.867Z",
      "dataUrl": "/partnerOpportunities/opportunity-2-id/file"
    },
    {
      "partnerOpportunityId": "opportunity-3-id",
      "title": "Title for opportunity-3",
      "description": "Description for opportunity-3",
      "callToAction": "Action that opportunity-3 recommends you to take",
      "product": "Amazon advertising product that opportunity-3 corresponds to",
      "objective": "Objective that opportunity-3 intends to improve e.g. awareness",
      "createdDate": "2022-05-13T23:42:33.823Z",
      "updatedDate": "2022-05-14T23:43:54.867Z",
      "dataUrl": "/partnerOpportunities/opportunity-3-id/file"
    },
    {
      "partnerOpportunityId": "opportunity-4-id",
      "title": "Title for opportunity-4",
      "description": "Description for opportunity-4",
      "callToAction": "Action that opportunity-4 recommends you to take",
      "product": "Amazon advertising product that opportunity-4 corresponds to",
      "objective": "Objective that opportunity-4 intends to improve e.g. retention",
      "createdDate": "2022-05-15T23:42:33.823Z",
      "updatedDate": "2022-05-16T23:43:54.867Z",
      "dataUrl": "/partnerOpportunities/opportunity-4-id/file"
    }
  ]
}
```

### Step 2: Downloading results

#### Request

```
GET https://advertising-api.amazon.com/partnerOpportunities/<partnerOpportunityId>/file
```

#### Example cURL command

The data fields vary by opportunity type and are defined in the response’s CSV schema.

```
curl 'https://advertising-api.amazon.com/partnerOpportunities/<partnerOpportunityId>/file' \
 -H 'Accept: application/vnd.partneropportunity.v1+json' \
 -H 'Amazon-Advertising-API-ClientId: <LWA_APP_CLIENT_ID>' \
 -H 'Amazon-Advertising-API-Manager-Account: <PARTNER_NETWORK_ACCOUNT_ID>' \
 -H 'Authorization: Bearer <ACCESS_TOKEN>'
 ```

#### Example response

```
HTTP/1.1 307 Temporary Redirect
location: <S3_DOWNLOAD_URL>
```

## Troubleshooting API errors

### 401 Unauthorized

Verify all [required headers](guides/recommendations/partner-opportunities/getting-started) are included in your API call. Ensure the user account associated with your LwA application matches the account used for access/refresh token generation. 

To authenticate: 

1. Sign out of all Amazon accounts or use an incognito/private browser window
1. Sign in with the Amazon account associated with your LwA application. To learn more, review the [Getting Started with the Partner Opportunities API guide](https://advertising.amazon.com/API/docs/en-us/guides/recommendations/partner-opportunities/getting-started).
1. Generate a new [authorization access and tokens](guides/get-started/retrieve-access-token#retrieve-your-client-id-and-client-secret), if needed.

### 403 Forbidden

Verify the LwA user has Admin or Custom-Editor permissions in the Partner Network account. Confirm the `Amazon-Advertising-API-Manager-Account` header contains the correct Partner Network Account ID.

### 404 Not Found

A `rowCount` of 0 indicates no matching data, corresponding to grayed-out download links in the UI. Most opportunities refresh daily, with some as often as hourly. We recommend verifying from the `GET/partnerOpportunities` response that `opportunities.dataMetadata.rowCount` is greater than 0 for the opportunity IDs you’re using. 

To troubleshoot:

1. Verify `partnerOpportunityId` is valid.
1. Check `partnerOpportunityId` in the opportunities list.
1. Use the `dataUrl` property for the correct identifier value.

>[NOTE] Changes to advertisers or ASINs may take up to 24 hours to reflect in the Partner Opportunities API.

### 404 Unsupported Media Type

Verify the `Accept` header contains a valid value. Review the [API specifications](https://advertising.amazon.com/API/docs/en-us/partner-opportunities) `Accept` section for allowed values. 

>[NOTE] Wildcards are not a valid value for the Accept header. For example, Accept: `*/*` is not allowed.

## Partner Opportunities and Marketing Stream

### Integrating Marketing Stream with Partner Opportunities Apply API

[Amazon Marketing Stream](guides/amazon-marketing-stream/overview) provides near real-time metrics and recommendations directly to your AWS account. If you decide to use Amazon Marketing Stream to get account team recommendations, you can apply recommendations with the Partner Opportunities API. 

When you subscribe to the [sponsored-ads-campaign-diagnostics-recommendations](guides/amazon-marketing-stream/data-guide#sponsored-ads-campaign-recommendations-sponsored-ads-campaign-diagnostics-recommendations_) dataset, you’ll receive automatic push notifications containing recommendation IDs that can be used with the Partner Opportunities API to apply the recommendations. 

For example, if you receive a Stream notification suggesting a budget increase for a Sponsored Products campaign, you can apply this recommendation using the following process:

1. Use the `recommendation_id` from the Stream payload.
1. Submit it to the [`/partnerOpportunities/{partnerOpportunityId}/apply`](https://advertising.amazon.com/API/docs/en-us/partner-opportunities#tag/Partner-Opportunities/operation/partnerOpportunitiesApply) resource.
1. Apply up to 100 recommendation IDs in a single request.

To submit a successful Partner Opportunities Apply API request, use these required attributes from your Amazon Marketing Stream datasets:


| PO attribute | Marketing Stream attribute |
|--------------|------------------------|
| advertiserType | Recommendation.advertiser_type |
| encryptedAdvertiserId | advertiserId |
| entityId | entityId |
| marketplace | Recommendation.marketplace_name |
| recommendationIds | recommendationIds |

### Example response

```
{
  "advertiserId": "A2B3WACBWPDWRJ",
  "entityId": "ENTITY1ITEBBD7GQJC8", // NEW
  "marketplaceId": "ATVPDKIKX0DER",
  "datasetId": "sponsored-ads-campaign-diagnostics-recommendations",
  "messageBody": {
    "entity_type": "SELLER_CENTRAL_ENTITY", // NEW
    "advertiser_type": "SELLER", // NEW
    "marketplace_name": "US", // NEW
    "recommendation_id": "1ea17717-c04c-4ef1-817b-1a0ef03a7e07",
    "group_id": "e3c38e01-4309-42d1-83dc-4b06aa2e997a",
    "apply_endpoint": "/recommendations/apply",
    "type": "KEYWORD_BID",
    "published_date": "2024-09-04T17:11:14 UTC",
    "publish_metadata": {
      "published_by": "AMAZON_ADS_ACCOUNT_TEAM",
      "published_to_amazon_ad_console": true
    },
    "expiry_date": "2024-09-18T17:11:14 UTC",
    "explanation": {
      "description": "Campaigns with high clickthrough rate, compared to similar campaigns. Apply these bid recommendations to increase sales.",
      "missed_opportunities": null,
      "estimated_change": null,
      "recommendation_context": {
        "asin_context": null,
        "diagnostic_context": {
          "benchmark_context": {
            "roas": {
              "benchmark_value": 1.56,
              "period": 7
            },
            "impressions": {
              "benchmark_value": 1706,
              "period": 7
            },
            "ad_spend": {
              "benchmark_value": 9.88,
              "period": 7
            },
            "click_through_rate": {
              "benchmark_value": 0.46,
              "period": 7
            },
            "budget_utilization": {
              "benchmark_value": 7.86,
              "period": 7
            }
          },
          "diagnostic_date": "2024-08-31T00:00:00.000Z"
        },
        "grouping_context": {
          "grouping_type": "INCREASE_BID_CONTEXTUAL",
          "brand_id": null,
          "brand_name": null,
          "browse_node_id": null,
          "browseNodeName": null,
          "objective": null,
          "event": null,
          "phase": null
        }
      }
    },
    "campaign_id": "211302794061744",
    "campaign_name": "ECI - [CONS] - AG - Car Pro Shampoo Wash - Manual - SP",
    "ad_product": "SP",
    "current_campaign_settings": {
      "budget": null,
      "ad_groups": [
        {
          "bid": null,
          "bid_optimization": null,
          "asins": null,
          "adgroup_id": "172393940497616",
          "adgroup_name": null,
          "target_type": "KEYWORD",
          "targets": [
            {
              "bid": 0.75,
              "text": "ceramic car shampoo",
              "match_type": "BROAD",
              "target_id": "132038708905255",
              "targeting": "ceramic car shampoo",
              "category": null,
              "expressions": null,
              "audience_target_type": null
            }
          ],
          "creative": null
        }
      ],
      "campaign_targeting": null,
      "bidding_strategy": null,
      "start_date": null,
      "end_date": null
    },
    "recommended_campaign_settings": {
      "budget": null,
      "ad_groups": [
        {
          "bid": null,
          "bid_optimization": null,
          "asins": null,
          "adgroup_id": "172393940497616",
          "adgroup_name": null,
          "target_type": "KEYWORD",
          "targets": [
            {
              "bid": 2.64,
              "text": "ceramic car shampoo",
              "match_type": "BROAD",
              "target_id": "132038708905255",
              "targeting": "ceramic car shampoo",
              "category": null,
              "expressions": null,
              "audience_target_type": null
            }
          ],
          "creative": null
        }
      ],
      "campaign_targeting": null,
      "bidding_strategy": null,
      "start_date": null,
      "end_date": null
    },
    "recommendation_status": "REMOVED",
    "recommendation_group": "sponsored-ads-campaign-diagnostics-recommendations-rec-a-018"
  },
  "eventTimestamp": null
}
```

For a complete list of available recommendations and detailed schema descriptions, see the [Amazon Marketing Stream documentation](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-diagnostics-recommendations#recommendation-types).  
