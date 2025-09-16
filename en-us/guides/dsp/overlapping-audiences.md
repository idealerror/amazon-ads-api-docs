---
title: Overlapping Audiences
description: Overview of overlapping audiences using the DSP API
type: guide
interface: api
tags:
    - DSP
keywords:
    - audiences
---

# Overlapping Audiences 

The [Overlapping Audiences API](overlapping-audiences) helps surface other audiences that are similar to your selected audience and that you may want to target in your campaigns. You can use any audience returned from the [Discovery API](audiences#tag/Discovery/operation/listAudiences) as an input to generate overlapping audiences.

For example, to retrieve overlapping audiences for an audience with ID equal to `1234567890`:

```bash
GET /insights/audiences/1234567890/overlappingAudiences?adType=DSP&advertiserId=112233445566
```

The API will return the top 30 audiences that overlap with the selected audience and meet the specified criteria. Below you’ll see a sample with the selected audience and just its top 3 overlapping audiences shown.

```bash
HTTP/1.1 200 
{
  "requestedAudienceMetadata": {
     "audienceId": "0000000000",
     "name": "Example 1",
     "size": 0,
     "category": "Example Category 1",
     "impressionForecastRange": {
       "lowerBound": 000000000
    }
   },
   "overlappingAudiences": [
     {
       "audienceMetadata": {
         "audienceId": "0000000000",
         "name": "Example 2",
         "size": 0,
         "category": "Example Category 2",
         "impressionForecastRange": {
          "lowerBound": 00000000,
          "upperBound": 00000000        }
       },
       "affinity": 00.00
    },
     {
       "audienceMetadata": {
         "audienceId": "0000000000",
         "name": "Example 3",
         "size": 0,
         "category": "Example Category 3",
         "impressionForecastRange": { 
          "lowerBound": 000000000,
          "upperBound": 000000000
        }
       },
       "affinity": 00.00
    },
     {
       "audienceMetadata": {
         "audienceId": "0000000000",
         "name": "Example 4",
         "size": 0,
         "category": "Example Category 4",
         "impressionForecastRange": {
          "lowerBound": 000000000
        }
       },
       "affinity": 00.00
    }
```

You can use one or more filters to narrow the results. The API offers filters for audienceCategory, minimumOverlapAffinity, and maximumOverlapAffinity.

For example, the following request returns the top 30 overlapping audiences that are in the in-market `audienceCategory` and have a `minimumOverlapAffinity` of 2.

```bash
GET /insights/audiences/1234567890/overlappingAudiences?adType=DSP&advertiserId=112233445566&audienceCategory=In-market&minimumOverlapAffinity=2
```
