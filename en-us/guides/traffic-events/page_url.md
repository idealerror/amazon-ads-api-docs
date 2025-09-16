---
title: page url report
description: Querying page url.
type: guide
interface: api
---
# Page URL report

The page URL report allows Amazon DSP advertisers to view impression data organized by page URL across selected campaigns, and line items during a defined time period.

## Querying the page URL report

To initiate the page URL report, use the `POST/avc/workflowexecutions` endpoint with the request body structured as below:

```
{
  "templatedReport":{
    "type":"PAGE_URL_REPORT",
    "attributes": {
        "measurementSelections": {
            "campaignIds": ["campaign1", "campaign2", "campaign3" ],
            "lineItemIds": ["lineItem1", "lineItem2", "lineItem3" ]
         }
     }  
},
  "timeWindow":{
    "type":"CURRENT_MONTH"
  }
}
```

### Supported `timeWindow` types

The following types of time windows can be used for specifying input data for the page URL workflow execution.

-	**EXPLICIT** : The start and end date of the time window must be explicitly provided in the request. Note that the start date-time and end date-time must be in the following format: 00:00:00.

-	**MOST\_RECENT\_WEEK**: The time window will be the most recent 1-week window for which the instance is likely to have data, aligned to day boundaries.

-	**CURRENT\_MONTH**: The time window will be the start of the current month up to the most recent time for which the instance is likely to have data.


#### Sample request body

```
curl \
-X POST \
 --location
 'https://advertising-api.amazon.com/avc/workflowexecutions' \
 --header 'Authorization: XXXXX' \
 --header 'Amazon-Advertising-API-ClientID:amzn1.application-oa2-client.XXXXX' \
 --header 'Amazon-Advertising-Advertiser-Id: XXXXX' \
 --header 'Amazon-Advertising-API-MarketplaceId: ATVP/DKIKX0DER' \
 --data '
    {
  "templatedReport":{
    "type":"PAGE_URL_REPORT",
    "attributes":{
       "measurementSelections": {
            "campaignId":["111111111111111111", "22222222222222", "33333333333" ],
            "lineItemId": ["1234567891234", "0987654321098", "567890987654" ]
      }
    }
  },
  "timeWindow":{
    "type":"MOST_RECENT_WEEK"
  }
}'

```

#### Sample response

```
{
    "workflowExecution": {
        "advertiserId": "XXXXX",
        "createTime": "2025-02-21T18:42:48.339Z",
        "lastUpdatedTime": "2025-02-21T18:42:48.339Z",
        "status": "PENDING",
        "timeWindow": {
            "end": "2025-02-21T00:00:00Z",
            "start": "2025-02-14T00:00:00Z",
            "timeZoneOriginal": "UTC"
        },
        "workflowExecutionId": "30eb7e3a-4ec7-4dae-9817-85d651a9cc34"
    }
}

```

### Report eligibility

The Page URL report is available to all Amazon DSP advertisers free of cost.

### Data availability
-	Data in page URL reports is available starting from February 14, 2025, and cannot be accessed retroactively.
-	Page URL data will be available in this report within 48 hours of serving impressions, though occasional delays may occur.
-	The report's maximum lookback period is 12 months.
-	For a page URL to be included in the report, it must have received impressions from at least two distinct users during the specified timeframe. A NULL value indicates the privacy threshold was not met.

### Sample report outputs

 campaign\_id       | line\_item\_id | supply\_source | page\_url            | total\_impressions\_served | report\_run\_date | filtered\_reason 
----|----|----|----|----|----|----
 111111111111111111 | 1234567891234  | Magnite DV\+ | abc\.de/123/durh     | 543                        | 2025-03-24              |                         
 22222222222222      | 0987654321098  | PubMatic | example\.de/iusjgei/ | 2                          | 2025-03-24                |                           
 33333333333        | 567890987654   | Amazon Media | Twitch\.com          | 65                         | 2025-03-24               |                          
 NULL               | NULL           | NULL       | NULL                 | 1                          | 2025-03-24                |                           

### Report column name definitions

 Column name  | Description   
-----|------------------
 page\_url                 | The full URL of the page on which the ad appeared, as derived from the bid request\. Note that the URL may, in some cases, be truncated by supply partners\. For Amazon\-owned inventory, only the top\-level domain is shown\. 
 campaign\_id              | Campaign id for the campaign\.                                       
  line\_item\_id            | Line item id for the campaign\.                                   
 total\_impression\_served | Total number of impressions served on the page URL\. 
 supply\_source | Consolidated name of the supply source (exchange). Example: Amazon Media. 
 report\_run\_date | Date when the report was run. 
 filtered\_reason | This column will be empty unless the number of events for a particular campaign did not meet the [aggregation threshold](guides/traffic-events/data-aggregation-thresholds). 

 >[NOTE] For help with generating this report, contact your Amazon DSP account executive or reach out to [support](support/overview).