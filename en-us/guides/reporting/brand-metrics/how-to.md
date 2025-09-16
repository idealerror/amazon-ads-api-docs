---
title: How to use the Brand Metrics API
description: Learn how to use the brand metrics API
type: guide
interface: api
tags:
    - Brand Metrics
keywords:
    - getting started
---

# How to use the Brand Metrics API

The following examples demonstrate how to call the Brand Metrics API using cURL. 

## Create a report request

Request:

```bash
curl \

    -X POST \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer YOUR\_ACCESS\_TOKEN" \
    -H "Amazon-Advertising-API-Scope: YOUR\_PROFILE\_ID" \    
    -H "Amazon-Advertising-API-ClientId: YOUR\_CLIENT\_ID" \
    --data @body.json \
    https://advertising-api.amazon.com/insights/brandMetrics/report
```

Response:

```bash
Response
{
  "expiration": "300000",
  "format": "CSV",
  "location": "",
  "reportId": "82cb268a-d124-424c-a38d-1c8c3672a601",
  "status": "IN_PROGRESS",
  "statusDetails": "Generation of the report is in progress!"
}
```

Make note of the report identifier in the `reportId` field of the response.

## Query the report resource for status

Use the report identifier to retrieve the status of the report.

Request:

```bash
curl \

    -X GET \
    -H "Content-Type:application/json" \
    -H "Authorization: Bearer YOUR\_ACCESS\_TOKEN" \
    -H "Amazon-Advertising-API-Scope: YOUR\_PROFILE\_ID" \    
    -H "Amazon-Advertising-API-ClientId: YOUR\_CLIENT\_ID" \
    https://advertising-api.amazon.com/insights/brandMetrics/report/{reportId}
```

Response:

```bash
Response
{
    "reportId": "82cb268a-d124-424c-a38d-1c8c3672a601",
    "format": "CSV",
    "statusDetails": "Generation of the report was successful",
    "location": "https://ReportURL.xyz",
    "expiration": "300000",
    "status": "SUCCESSFUL"
}
```

Once the `status` field is set to `SUCCESSFUL`, the value of the `location` property is the download URL for your report.

## Sample report

The report can have two formats. CSV or JSON.  Below are the sample reports for both CSV and JSON formats.

### CSV sample

|metricsComputationDate	|brandName	|categoryNodeName	|categoryNodeTreeName	|customerConversionRate	|customerConversionRateCategoryMedian	|customerConversionRateCategoryTopPerformers	|engagedShopperRateLowerBound	|engagedShopperRateUpperBound	|engagedShopperRateCategoryMedianLowerBound	|engagedShopperRateCategoryMedianUpperBound	|engagedShopperRateCategoryTopPerformersLowerBound	|engagedShopperRateCategoryTopPerformersUpperBound	|newToBrandCustomerRate	|newToBrandCustomerRateCategoryMedian	|newToBrandCustomerRateCategoryTopPerformers	|brandedSearchesOnly	|brandedSearchesCategoryMedian	|brandedSearchesCategoryTopPerformers	|viewedDetailPageOnly	|viewedDetailPageCategoryMedian	|viewedDetailPageCategoryTopPerformers	|viewedDetailPageOnlyReturnOnEngagement	|viewedDetailPageROECategoryMedian	|viewedDetailPageROECategoryTopPerformers	|brandedSearchesAndDetailPageViews	|brandedSearchesAndDetailPageViewsCategoryMedian	|brandedSearchesAndDetailPageViewsCategoryTopPerformers	|brandedSearchesAndDetailPageViewsReturnOnEngagement	|brandedSearchesAndDetailPageViewsROECategoryMedian	|brandedSearchesAndDetailPageViewsROECategoryTopPerformers	|addToCarts	|addToCartsCategoryMedian	|addToCartsCategoryPerformers	|addToCartsReturnOnEngagement	|addToCartsROECategoryMedian	|addToCartsROECategoryTopPerformers	|brandCustomers	|brandCustomersCategoryMedian	|brandCustomersCategoryTopPerformers	|brandCustomersReturnOnEngagement	|brandCustomersROECategoryMedian	|brandCustomersROECategoryTopPerformers	|highValueCustomers	|highValueCustomersCategoryMedian	|highValueCustomersCategoryTopPerformers	|highValueCustomersReturnOnEngagement	|highValueCustomersROECategoryMedian	|highValueCustomersROECategoryTopPerformers	|awarenessIndex	|considerationIndex	|salesIndex	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Hockey	|us-goods	|0.11628	|0.13043	|0	|0	|5	|0	|5	|0	|0	|0.886	|0.944	|0	|71	|639	|0	|224	|205	|0	|0	|0.95122	|0	|12	|9	|0	|0	|0	|0	|30	|38	|0	|0	|2.056	|0	|31	|33	|0	|0	|0	|0	|4	|4	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Field Hockey/Balls	|us-goods	|0.03571	|0.39452	|0	|0	|5	|10	|20	|0	|0	|1	|0.831	|0	|90	|254	|0	|24	|89	|0	|0	|0.33394	|0	|0	|3.5	|0	|0	|0	|0	|3	|28.5	|0	|0	|3.67477	|0	|0	|34	|0	|0	|0.30273	|0	|1	|4.5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Field Hockey/Player Equipment/Gloves	|us-goods	|0.3913	|0.2248	|0	|5	|10	|10	|20	|0	|0	|0.778	|0.7525	|0	|89	|688.5	|0	|10	|26	|0	|0	|0.62479	|0	|0	|2	|0	|0	|0	|0	|4	|6.5	|0	|0	|4.8994	|0	|8	|9	|0	|0	|0	|0	|1	|2.5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Field Hockey/Protective Gear	|us-goods	|0.21277	|0.2213	|0	|5	|10	|5	|10	|0	|0	|0.9	|0.9	|0	|79	|757	|0	|55	|55	|0	|0	|1.17679	|0	|8	|5	|0	|0	|0	|0	|11	|11	|0	|0	|1.69842	|0	|18	|15	|0	|0	|0	|0	|2	|2	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Field Hockey/Protective Gear/Shin Guards	|us-goods	|0.21277	|0.3913	|0	|5	|10	|5	|10	|0	|0	|0.9	|0.9035	|0	|79	|840.5	|0	|55	|64.5	|0	|0	|1.4735	|0	|8	|5	|0	|0	|0	|0	|11	|11	|0	|0	|7.01121	|0	|18	|16.5	|0	|0	|0	|0	|1	|2	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Field Hockey/Player Equipment/Sticks	|us-goods	|0.14286	|0.1125	|0	|5	|10	|5	|10	|0	|0	|1	|1	|0	|84	|1046	|0	|131	|109.5	|0	|0	|0.46382	|0	|3	|10.5	|0	|0	|0	|0	|13	|17	|0	|0	|1.72727	|0	|4	|11.5	|0	|0	|0	|0	|1	|2	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Ice Hockey	|us-goods	|0.16442	|0.15293	|0.4577	|0	|5	|0	|5	|0	|5	|0.77	|0.909	|0	|61	|61.5	|56783.925	|260	|202.5	|1647.15	|0	|0.79164	|8.46689	|11	|6	|295.7	|0	|0	|19.90488	|39	|39	|428.425	|0	|4.66902	|46.22077	|54	|42	|498.025	|0	|1.60122	|17.87654	|7	|5	|55.65	|0	|0	|57.956	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Ice Hockey/Protective Gear	|us-goods	|0.27692	|0.14286	|0	|0	|5	|0	|5	|0	|0	|0.556	|0.885	|0	|81	|252	|0	|38	|231	|0	|0	|0.81308	|0	|3	|10	|0	|0	|1.20806	|0	|6	|44	|0	|0	|3.9339	|0	|16	|45	|0	|0	|0	|0	|2	|5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Ice Hockey/Protective Gear/Shin Guards	|us-goods	|0.27692	|0.19444	|0	|5	|10	|10	|20	|0	|0	|0.556	|0.857	|0	|81	|1621	|0	|38	|38	|0	|0	|1.36121	|0	|3	|3	|0	|0	|0	|0	|6	|12	|0	|0	|0	|0	|16	|10	|0	|0	|0	|0	|2	|2	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Lacrosse	|us-goods	|0.14286	|0.135	|0	|0	|5	|0	|5	|10	|20	|0.974	|0.924	|1	|66	|137.5	|63498.17	|314	|233	|1844.49	|0	|1.38325	|9.8099	|13	|12.5	|224.68	|0	|0	|43.2834	|47	|56	|491.045	|0	|5.36049	|44.44481	|34	|53	|762.705	|0	|1.84421	|58.80889	|4	|6	|86.15	|0	|0	|62.1725	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Lacrosse/Accessories/Equipment Bags	|us-goods	|0.09511	|0.14381	|0	|20	|30	|10	|20	|0	|0	|0.971	|0.887	|0	|66	|198	|0	|279	|201	|0	|0	|1.01248	|0	|13	|6.5	|0	|0	|1.25	|0	|41	|33	|0	|0	|8.21512	|0	|31	|27	|0	|0	|0.58317	|0	|4	|3.5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Rugby	|us-goods	|0.44118	|0.11594	|0	|0	|5	|0	|5	|0	|0	|0.667	|0.91	|0	|84	|153	|0	|12	|258	|0	|0	|0.11423	|0	|0	|4	|0	|0	|0	|0	|7	|44	|0	|0	|0	|0	|13	|33	|0	|0	|0	|0	|2	|4	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Tennis & Racquet Sports/Squash/Racquets	|us-goods	|0.11628	|0.12897	|0	|5	|10	|30	|40	|0	|0	|0.9	|0.8405	|0	|72	|2962.5	|0	|56	|275	|0	|0	|3.40512	|0	|16	|41.5	|0	|0	|8.76774	|0	|4	|44	|0	|0	|33.34794	|0	|9	|39	|0	|0	|31.57797	|0	|1	|5	|0	|0	|19.83333	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Tennis & Racquet Sports/Squash	|us-goods	|0.11207	|0.13131	|0	|0	|5	|5	|10	|0	|0	|0.846	|0.8735	|0	|68	|333	|0	|80	|308.5	|0	|0	|0.72368	|0	|18	|23.5	|0	|0	|0.99022	|0	|5	|34.5	|0	|0	|7.05969	|0	|11	|49	|0	|0	|2.78274	|0	|2	|5.5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports	|us-goods	|0.13071	|0.14324	|0.36264	|0	|5	|0	|5	|0	|5	|0.854	|0.938	|0	|40	|49.5	|73295.82	|575	|695.5	|13753.385	|1.7146	|0.75931	|12.70545	|21	|7	|1658.25	|6.77	|0	|34.92388	|89	|143.5	|3434.51	|8.59571	|3.20752	|59.22438	|92	|121	|3903.85	|4.71447	|1.2651	|26.94545	|11	|14	|435.74	|11.998	|0	|74.13692	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Tennis & Racquet Sports	|us-goods	|0.11207	|0.13821	|0.37093	|0	|5	|0	|5	|0	|0	|0.846	|0.939	|0	|68	|66	|47720.12	|80	|692	|0	|0	|0.20443	|7.53868	|18	|6	|1589.11	|0	|0	|23.20542	|5	|137	|0	|0	|3.92289	|28.69498	|11	|106	|0	|0	|0.11022	|45.07288	|2	|12	|0	|0	|0	|85.31	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Rugby/Clothing	|us-goods	|0.44118	|0.15019	|0	|0	|5	|0	|5	|0	|0	|0.667	|0.88	|0	|84	|178	|0	|12	|184	|0	|0	|0	|0	|0	|4	|0	|0	|0	|0	|7	|44	|0	|0	|2.586	|0	|13	|27	|0	|0	|0	|0	|2	|4	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Rugby/Clothing/Men	|us-goods	|0.44118	|0.15019	|0	|0	|5	|0	|5	|0	|0	|0.667	|0.885	|0	|84	|176	|0	|12	|178	|0	|0	|0	|0	|0	|3	|0	|0	|0	|0	|7	|50	|0	|0	|0	|0	|13	|27	|0	|0	|0	|0	|2	|4	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Rugby/Clothing/Men/Socks	|us-goods	|0.44118	|N/A	|0	|60	|80	|N/A	|N/A	|0	|0	|0.667	|N/A	|0	|84	|N/A	|0	|12	|N/A	|0	|0	|N/A	|0	|0	|N/A	|0	|0	|N/A	|0	|7	|N/A	|0	|0	|N/A	|0	|13	|N/A	|0	|0	|N/A	|0	|2	|N/A	|0	|0	|N/A	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Ice Hockey/Player Equipment	|us-goods	|0.34783	|0.09756	|0	|0	|5	|5	|10	|0	|0	|0.688	|0.857	|0	|84	|1950	|0	|21	|162	|0	|0	|0.61204	|0	|0	|27	|0	|0	|0	|0	|9	|28	|0	|0	|4.03824	|0	|14	|45	|0	|0	|0	|0	|2	|5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Lacrosse/Accessories	|us-goods	|0.09511	|0.16488	|0	|5	|10	|5	|10	|0	|0	|0.971	|0.8475	|0	|66	|254	|0	|279	|122.5	|0	|0	|1.1146	|0	|13	|10	|0	|0	|0.55556	|0	|41	|30.5	|0	|0	|8.00635	|0	|31	|45	|0	|0	|1.15275	|0	|4	|5	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports/Field Hockey/Player Equipment	|us-goods	|0.08046	|0.1158	|0	|5	|10	|5	|10	|0	|0	|0.857	|1	|0	|83	|874	|0	|140	|100.5	|0	|0	|0.3125	|0	|3	|6	|0	|0	|0	|0	|17	|16.5	|0	|0	|2.05794	|0	|12	|13.5	|0	|0	|0	|0	|2	|2	|0	|0	|0	|0	|N/A	|N/A	|N/A	|
|7/31/2021	|YourAwesomeBrand	|/Categories/Sports & Fitness	|us-goods	|0.12885	|0.1158	|0.31967	|0	|5	|0	|5	|0	|5	|0.846	|0.943	|0	|18	|30	|17936.06	|658	|1152	|19605.99	|1.48434	|0.6011	|9.51179	|39	|9	|1854.87	|5.7545	|0	|27.82146	|94	|201	|0	|10.1137	|3.51739	|43.15696	|105	|156	|3839.695	|5.5614	|1.19101	|31.44083	|12	|18	|429.98	|12	|0.50297	|104.03482	|N/A	|N/A	|N/A	|
|	|YourAwesomeBrand	|/Categories/Sports & Fitness/Sports/Other Sports	|us-goods	|0.14815	|0.13131	|0.37792	|0	|5	|0	|5	|0	|5	|0.854	|0.937	|0	|69	|54	|39976.455	|232	|249.5	|1796.84	|0	|0.44727	|6.84569	|8	|3	|319.075	|0	|0	|14.49356	|36	|45	|404.675	|0	|2.73539	|37.67465	|43	|36.5	|420.78	|0	|0	|19.20021	|5	|5	|47.06	|0	|0	|69.54465	|N/A	|N/A	|N/A	|

### JSON sample

```JSON
{
    "brandBuildingMetrics": [{
        "metadata": {
            "metricsComputationDate": "2021-07-03",
            "brandName": "Your Awesome Brand",
            "categoryNodeName": "/Categories/Sports & Fitness/Sports/Ice Hockey",
            "categoryNodeTreeName": "us-goods",
            "lookbackPeriod": "1w"
        },
        "metrics": {
            "engagedShopperRateLowerBound": "0",
            "customerConversionRate": "0.3235577"
        }
    }, {
        "metadata": {
            "metricsComputationDate": "2021-07-10",
            "brandName": "Your Awesome Brand",
            "categoryNodeName": "/Categories/Sports & Fitness/Sports/Ice Hockey",
            "categoryNodeTreeName": "us-goods",
            "lookbackPeriod": "1w"
        },
        "metrics": {
            "engagedShopperRateLowerBound": "0",
            "customerConversionRate": "0.18482"
        }
    }, {
        "metadata": {
            "metricsComputationDate": "2021-07-17",
            "brandName": "Your Awesome Brand",
            "categoryNodeName": "/Categories/Sports & Fitness/Sports/Ice Hockey",
            "categoryNodeTreeName": "us-goods",
            "lookbackPeriod": "1w"
        },
        "metrics": {
            "engagedShopperRateLowerBound": "0",
            "customerConversionRate": "0.238139"
        }
    }, {
        "metadata": {
            "metricsComputationDate": "2021-07-24",
            "brandName": "Your Awesome Brand",
            "categoryNodeName": "/Categories/Sports & Fitness/Sports/Ice Hockey",
            "categoryNodeTreeName": "us-goods",
            "lookbackPeriod": "1w"
        },
        "metrics": {
            "engagedShopperRateLowerBound": "0",
            "customerConversionRate": "0.1048739"
        }
    }, {
        "metadata": {
            "metricsComputationDate": "2021-07-31",
            "brandName": "Your Awesome Brand",
            "categoryNodeName": "/Categories/Sports & Fitness/Sports/Ice Hockey",
            "categoryNodeTreeName": "us-goods",
            "lookbackPeriod": "1w"
        },
        "metrics": {
            "engagedShopperRateLowerBound": "0",
            "customerConversionRate": "0.1844205"
        }
    }]
}
```
