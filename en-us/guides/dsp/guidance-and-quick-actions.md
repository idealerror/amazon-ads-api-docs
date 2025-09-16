---
title: Guidance and Quick Actions
description: Guide for DSP Guidance and Quick Actions APIs
type: guide
interface: api
tags:
    - DSP
keywords:
    - guidance
---

# Guidance and Quick Actions

The [Guidance API](dsp-guidance) enables self-service advertisers to programmatically access and implement Amazon DSP's recommendations. This API allows you to integrate Amazon's machine learning optimization suggestions into your own tools and workflows. Use cases include:

- **Automate campaign optimization**: Receive and apply pre-flight and in-flight recommendations to improve campaign pacing and performance.
- **Build custom optimization workflows**: Integrate recommendations into your existing campaign management tools to create automated optimization rules.
- **Track optimization opportunities**: Monitor recommendations across your portfolio and understand the impact of implemented changes.

## Where are recommendations available?
Recommendations are provided at two stages of the campaign workflow: **1) pre-flight** and **2) in-flight**. 

During the pre-flight workflow, we offer **tactics** and **pre-flight guidance**: 

**Tactics** offer pre-configured ad groups to maximize performance and utilize a specific targeting strategy. Eligible campaigns will receive tactics recommendations when querying for campaign guidance and can create the suggested ad groups through the Quick Actions API. 

**Pre-flight guidance** helps to optimize your created ad groups before launch using machine learning. These recommendations analyze your campaign parameters and suggest optimal values for maximum average CPM and ad group frequency caps. Users can request recommendations for newly created campaigns and ad groups using any of the guidance API endpoints.

**Available pre-flight ad group delivery optimizations**
- Maximum Average CPM
- Frequency Cap

During the in-flight workflow, we offer **in-flight guidance**:

**In-flight guidance** helps to optimize active campaigns by identifying delivery and performance improvement opportunities. These recommendations analyze real-time campaign data to suggest actionable optimizations for your campaign. The API continuously monitors your live campaigns and generates recommendations when it identifies campaigns at risk of underpacing or ad groups with rejected creatives. 

**Available in-flight recommendations**
- **Under delivery Guidance**: Based on recent delivery we predict that this campaign will under deliver by the end of its flight.
    - **Increase Maximum Average CPM**: This ad group has a restrictive maximum average CPM, which is constraining its reach. Increase the maximum average CPM to optimize for pacing and performance.
    - **Increase Frequency Cap**: This ad group has a restrictive frequency cap, which is constraining its reach. Increase frequency cap to optimize for pacing and performance.
    - **Reduce Viewability Target**: This ad group has restrictive viewability targeting which is constraining its reach. Decrease viewability targeting to optimize for pacing. 
    - **Add Missing Creative Sizes**: This ad group is limited by the available creative sizes associated with it. Add additional suggested creative sizes to optimize for pacing and performance. 

## How can I access the guidance API? 
There are two main API’s for interacting with guidance, one to query for any available guidance and one to automatically  execute the recommended actions provided.

### Example:
Make a call to POST `/dsp/v1/guidance/campaigns/list` using the following request body to list guidance for a specific campaign

```json
{
  "filters": {
    "includeRecommendations": true
  },
  "campaignIds": [
    "585258320524611768"
  ]
}
```

The API returns the following body. *Some fields have been removed*.

```json
{
  "success": [
    {
      "guidance": [
        {
          "guidanceName": "UNDERPACING_ORDER_GUIDANCE",
          "guidanceType": "ALERT",
          "title": "Order underpacing",
          "description": "This order has spent $100.00 in the last 3 days, and is on pace to spend less than its budget.",
          "totalRecommendationCount": 1,
          "recommendations": [
            {
              "recommendationId": "Noble|MaxBidLine-v0002/entity-ENTITY3JYS5GV1UGMGB/order-585258320524611768/line-582095551261108351",
              "marketplaceId": "1",
              "advertiserCountry": "US",
              "entityId": "ENTITY3JYS5GV1UGMGB",
              "lastUpdateDate": "2024-11-01T14:00:00Z",
              "description": "This line item has a maximum average CPM that's restricting pacing. Increase your maximum average CPM to $8.00 to help improve pacing and performance.",
              "title": "Increase maximum average CPM for line item on underpacing order",
              "type": "MaxBidLine-v0002",
              "category": "BIDDING",
              "guidanceType": "ALERT",
              "quickactionsData": {
                "currentActions": [
                  {
                    "actionId": "cc193cg7-a195-4c66-8c82-cf0392dca2f4",
                    "actionType": "MAXBIDUPDATE",
                    "description": "",
                    "currentValue": "$5.00",
                    "recommendedValue": "$8.00"
                  }
                ],
                "executionHistory": null
              },
            }
          ]
        }
      ],
      "totalGuidanceCount": 1
    }
  ],
  "error": []
}
```

The response indicates that the queried campaign is underpacing and increasing a specific ad groups’s max average CPM to $8 will help. 

You can then make a call to `POST /dsp/v1/quickactions/cc193cg7-a195-4c66-8c82-cf0392dca2f4/executions` to create an execution for the action associated to the returned recommendation. By default this will also automatically start this execution and increase the max average CPM of the relevant ad group.

## API Endpoints:
**Guidance API**
- `POST /dsp/v1/guidance/adGroups/list` - List Ad Group Guidance
- `POST /dsp/v1/guidance/campaigns/list` - List Campaign Guidance
- `POST /dsp/v1/guidance/advertisers/list` - List Advertiser Guidance

**Quick Actions API**
- `POST /dsp/v1/quickactions/{actionId}/executions` - Create an execution for a Quick Action
- `PUT /dsp/v1/quickactions/{actionId}/executions/{executionId}/start` - Start an execution
- `GET /dsp/v1/quickactions/{actionId}/executions/{executionId}` - Get an execution
- `POST /dsp/v1/quickactions/batchGetExecutions` - Batch get executions
- `POST /dsp/v1/quickactions/batchCreateExecutions` - Batch create executions

## Implementation Guidelines: 
- **Guidance Polling** - Our recommendations are regularly updated to ensure they are accurate. We recommend polling for them daily, to enable you to access recommendations as soon as they are actionable. 
- **Quick Actions Polling** - The state of quick action executions can be queried using the get executions endpoint. We recommend polling every 30 seconds to monitor the action’s success. 

## Common use cases
- Retrieving pre-flight recommendations for new campaigns
- Before a new campaign’s flight begins, you can query this API to receive guidance on whether any settings could changed to improve the delivery of the campaign.
- Once a campaign has started, routinely querying the API can will allow you to review and implement guidance to help keep the campaign’s delivery and performance on track. 
