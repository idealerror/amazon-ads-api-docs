---
title: Getting started with Brand View Pro API
description: Learn how to request to the Brand View Pro API
type: guide
interface: api
tags:
    - Reporting
keywords:
    - Brand benchmarks
---

# Getting started with the Brand View Pro API

## Before you begin

To get started, advertisers must complete the necessary setup and authorization steps for the Amazon Ads API, then find the correct credentials to access the Brand View Pro API.

### Set up API access

The below steps are a high-level summary of the steps one must perform to get started with the Amazon Ads API. This includes creating a Login with Amazon (LwA) application, obtaining API access approval, and configuring their environment to make authenticated calls to the API. 

For more detailed, step-by-step instructions, please see the [API onboarding overview](guides/onboarding/overview) and [API getting started overview](guides/get-started/overview). 

1. **[Obtain an Amazon Developer account](guides/onboarding/create-lwa-app)**
    1. Sign up for a new Amazon Developer account if you don't have one already.
    2. Create a new LwA security profile.
    3. Retrieve your security credentials.
2. **Obtain access to the Amazon Ads API**
    1. [Submit an application](guides/onboarding/apply-for-access) to request access to the Amazon Ads API.
    2. [Assign API access](guides/onboarding/assign-api-access) to your LwA account.
3. **[Set up your environment for the Ads API](guides/get-started/overview)**
    1. Create an authorization grant to your LwA application.
    2. Obtain an access token and refresh token.

    >[NOTE]It is **not necessary** to perform the **Retrieve profiles** step to access the Brand View Pro API.

>[TIP:API onboarding support] For help onboarding to the Amazon Ads API, contact our onboarding support team directly at `ads-api-onboarding@amazon.com` or visit our [support page](support/overview) for more information.

### Gain access to the Brand View Pro API

After the above steps are finished, Advertising API setup is complete. ABVP-specific setup is required.

>[NOTE]These steps assume that your Amazon Developer login was used to create the authorization grant during API onboarding. If that is not the case, you can create a new grant by following the same steps and logging in with your Amazon Developer user account.

1. **Gain access to the ABVP advertiser entity.** 

    Ensure that your Amazon Developer account has **Editor** access to the ABVP advertiser entity. This permission is required in order to access ABVP in the console as well. Therefore, if you can access ABVP while logged in to your Amazon Developer user account, this is already completed. If not, the ABVP user account can invite the developer user by visiting **Administration** > **Account access & settings** in the console.
    
2. **Identify the manager account ID needed to call the API.** 

    While logged in to your Amazon Developer account, visit the [Manager Account](https://advertising.amazon.com/mn) page in the console. On this page, you should see the **manager account ID** at the top left corner. If this is not the expected advertiser for the manager account, you can change the manager account using the account switcher in the top right corner.

    ![Ads console screenshot](/_images/abvp-console-screenshot.png)

3. **Find your advertiser code.**

    Navigate to your advertiser's dashboard in the ABVP user interface. The advertiser code can be found in the URL path based on the following pattern:

    `https://advertising.amazon.com/bv#/ABVP/<YOUR_ADVERTISER_CODE>/dashboard`

The manager account ID and advertiser code will be used as parameters in calls to the Brand View Pro API.

## How to call the Amazon Brand View Pro API

There are two endpoints available in the API:

- Fetch the latest ABVP report metadata using the `allReportMetadata` endpoint.
- Retrieve a download URL for a specific ABVP report using the fetched report metadata in the `getReport` endpoint.

Required parameters and example requests and responses are listed below. For full details, see the [technical specifications for the ABVP API](brand-benchmarks).

### Header parameters

|Parameter |How to find the value	|
|---	|---	|
|`<YOUR_ACCESS_TOKEN>`	|Call the Login With Amazon authorization server using your refresh token for a valid access token. [Learn more.](guides/account-management/authorization/access-tokens)	|
|`<YOUR_CLIENT_ID>`	|This is your Login With Amazon client ID.	|
|`<YOUR_MANAGER_ACCOUNT_ID>`	|This is the manager account ID of the ABVP entity. This can be found by checking https://advertising.amazon.com/mn as described above.	|

### Path parameters

|Parameter |How to find the value	|
|---	|---	|
|`<YOUR_ADVERTISER_CODE>`	|The advertiser code can be found in the URL of the advertiser's ABVP dashboard as: `https://advertising.amazon.com/bv#/ABVP/<YOUR_ADVERTISER_CODE>/dashboard`	|
|`<REPORT_TYPE>` 	|Used only in the GET report call. Obtained from first making the report metadata call	|
|`<INDEX_DATE>`	|Used only in the GET report call. Obtained from first making the report metadata call	|

### Create a report metadata request

#### Sample request

```
curl -X GET "https://advertising-api.amazon.com/insights/brandBenchmarks/advertisers/<YOUR_ADVERTISER_CODE>/allReportMetadata" \
-H "Authorization: Bearer <YOUR_ACCESS_TOKEN>" \
-H "Amazon-Advertising-API-ClientId: <YOUR_CLIENT_ID>" \ 
-H "Amazon-Advertising-API-Manager-Account: <YOUR_MANAGER_ACCOUNT_ID>"
```

#### Sample response

```
{
    "nextToken": null,
    "reportsMetadata": [
        {
            "advertiserId": "<YOUR_ADVERTISER_CODE>",
            "indexDate": "2024-02-04",
            "obfuscatedMarketplaceId": "ATVPDKIKX0DER",
            "reportType": "ZIP_ASIN"
        },
        {
            "advertiserId": "<YOUR_ADVERTISER_CODE>",
            "indexDate": "2024-02-09",
            "obfuscatedMarketplaceId": "ATVPDKIKX0DER",
            "reportType": "EXCEL"
        },
        {
            "advertiserId": "<YOUR_ADVERTISER_CODE>",
            "indexDate": "2024-02-09",
            "obfuscatedMarketplaceId": "ATVPDKIKX0DER",
            "reportType": "ZIP_BRAND"
        }
    ]
}
```

### Create a report download request

#### Sample request

```
curl -X GET "https://advertising-api.amazon.com/insights/brandBenchmarks/advertisers/<YOUR_ADVERTISER_CODE>/reports/<REPORT_TYPE>/indexDates/<INDEX_DATE>" \
-H "Authorization: Bearer <YOUR_ACCESS_TOKEN>" \
-H "Amazon-Advertising-API-ClientId: <YOUR_CLIENT_ID>" \
-H "Amazon-Advertising-API-Manager-Account: <YOUR_MANAGER_ACCOUNT_ID>"
```

#### Sample response

```
{
    "downloadLink": "https://brand-view.s3.amazonaws.com/output-reports-v2/..."
}
```

## Report types

There are two types of reports that can be downloaded from the report download endpoint. 

### Brand Report

The Brand Report provides detailed metrics and insights related to your brand’s overall performance. This includes aggregated data across all ASINs associated with your brand providing a comprehensive view of key performance indicators. A sample of the report showing awareness data metrics such as glance views is shown below. 

>[Download this sample report in CSV format.](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/SampleBrandReportDataGV.csv)

|Root  Browse Node	|Level 2 Browse Node	|Level 3 Browse Node	|Level 4 Browse Node	|Level 5 Browse Node	|Brand Name	|Brand Owner	|Metric Name	|Metric Type	|YTD	|TTM	|Y: 2022	|Y: 2023	|Q: 2022 Q4	|Q: 2023 Q1	|Q: 2023 Q2	|Q: 2023 Q3	|Q: 2023 Q4	|Q: 2024 Q1	|Q: 2024 Q2	|Q: 2024 Q3	|M: 2022-10	|M: 2022-11	|M: 2022-12	|M: 2023-1	|M: 2023-2	|M: 2023-3	|M: 2023-4	|M: 2023-5	|M: 2023-6	|M: 2023-7	|M: 2023-8	|M: 2023-9	|M: 2023-10	|M: 2023-11	|M: 2023-12	|M: 2024-1	|M: 2024-2	|M: 2024-3	|M: 2024-4	|M: 2024-5	|M: 2024-6	|M: 2024-7	|M: 2024-8	|M: 2024-9	|WE: 2024-06-15	|WE: 2024-06-22	|WE: 2024-06-29	|WE: 2024-07-06	|WE: 2024-07-13	|WE: 2024-07-20	|WE: 2024-07-27	|WE: 2024-08-03	|WE: 2024-08-10	|WE: 2024-08-17	|WE: 2024-08-24	|WE: 2024-08-31	|WE: 2024-09-07	|WE: 2024-09-14	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Glance Views	|Absolute	|165957	|249593	|279900	|243708	|55743	|57421	|50945	|64200	|71142	|62790	|49068	|54099	|18350	|22065	|15328	|20542	|18121	|18758	|16649	|16928	|17368	|17217	|24557	|22426	|25899	|29591	|15652	|23664	|19543	|19583	|17664	|15030	|16374	|18924	|25243	|9932	|3943	|3547	|3786	|4402	|4297	|5249	|3758	|3434	|6178	|7562	|5738	|4362	|3306	|3186	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Glance Views - YoY	|Year-over-Year (YoY Growth)	|0.9617	|1.0981	|6.7395	|0.8707	|1.3422	|0.6787	|0.6845	|0.9858	|1.2762	|1.0935	|0.9632	|0.8427	|0.0046	|1.0545	|0.7439	|0.7934	|0.5966	|0.6618	|0.5884	|0.6551	|0.856	|0.79	|1.1209	|1.047	|1.4114	|1.3411	|1.0211	|1.152	|1.0785	|1.044	|1.061	|0.8879	|0.9428	|1.0991	|1.0279	|0.4429	|0.9858	|0.8568	|0.9234	|1.2624	|0.9562	|1.4646	|1.0347	|0.6958	|1.1563	|1.3957	|0.9528	|0.7215	|0.5806	|0.6468	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Glance Views Share	|Absolute	|0.002	|0.0023	|0.0032	|0.0022	|0.0026	|0.002	|0.0019	|0.0023	|0.0027	|0.0022	|0.0019	|0.0021	|0.0026	|0.0029	|0.0022	|0.0021	|0.002	|0.002	|0.0019	|0.0019	|0.002	|0.0017	|0.0028	|0.0027	|0.0028	|0.0032	|0.0019	|0.0023	|0.0022	|0.0021	|0.002	|0.0018	|0.0019	|0.0017	|0.0028	|0.0015	|0.002	|0.0017	|0.0018	|0.0021	|0.0019	|0.0015	|0.0018	|0.0017	|0.003	|0.0037	|0.0029	|0.0022	|0.0015	|0.0015	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Glance Views Share - YoY	|Year-over-Year (YoY Growth)	|0.9999	|1	|0.9999	|0.999	|0.9993	|0.9983	|0.9982	|0.9993	|1.0001	|1.0002	|0.9999	|0.9997	|0.0097	|0.9997	|0.9988	|0.9989	|0.9976	|0.9983	|0.9979	|0.9982	|0.9987	|0.9989	|0.9998	|0.9994	|1.0003	|1.0002	|0.9998	|1.0002	|1.0002	|1.0002	|1.0001	|0.9998	|0.9999	|1	|1	|0.9989	|1	|0.9997	|0.9999	|1.0004	|1.0005	|0.9997	|1	|0.9992	|1.0004	|1.001	|0.9998	|0.999	|0.9987	|0.9991	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Conversion Rate	|Absolute	|0.2538	|0.2206	|0.2305	|0.194	|0.2402	|0.2162	|0.231	|0.1693	|0.1719	|0.2276	|0.2705	|0.269	|0.2254	|0.2625	|0.2257	|0.2085	|0.218	|0.2229	|0.23	|0.2469	|0.2165	|0.2132	|0.2007	|0.1011	|0.1724	|0.2218	|0.0767	|0.187	|0.2434	|0.2609	|0.2712	|0.2718	|0.2687	|0.2216	|0.349	|0.156	|0.2506	|0.276	|0.2829	|0.2058	|0.233	|0.1911	|0.2387	|0.2746	|0.2813	|0.3436	|0.3967	|0.411	|0.2356	|0.2316	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Conversion Rate - YoY	|Year-over-Year (YoY Growth)	|1.0507	|0.9887	|0.8762	|0.9635	|0.9368	|1.0376	|1.0045	|0.8751	|0.9317	|1.0114	|1.0395	|1.0997	|2.5359	|1.0646	|1.0267	|1.0003	|1.0688	|1.0399	|1.0153	|1.0411	|0.9472	|0.9546	|0.8822	|0.7957	|0.9469	|0.9593	|0.8511	|0.9785	|1.0254	|1.0379	|1.0412	|1.0249	|1.0521	|1.0085	|1.1483	|1.0548	|1.0453	|1.0622	|1.0875	|0.9767	|1.0225	|0.9838	|1	|1.1309	|1.097	|1.0997	|1.1818	|1.2385	|1.1464	|1.1387	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Conversion Rate Index~vs~Browse	|Absolute	|2.8073	|2.4172	|2.4721	|2.378	|2.8188	|2.9282	|2.7837	|2.0358	|1.9788	|2.6034	|2.7593	|3.1265	|0.0545	|3.0501	|2.8025	|2.997	|2.9114	|2.8855	|2.8237	|2.9279	|2.6028	|2.7951	|2.3539	|1.1342	|1.9501	|2.5468	|0.9041	|2.33	|2.7311	|2.7805	|2.8331	|2.6776	|2.7695	|2.3826	|3.6113	|2.617	|2.5173	|2.8813	|2.9567	|2.352	|2.7305	|1.8996	|2.6409	|2.7845	|2.9674	|3.6493	|4.1131	|4.1213	|2.4545	|2.835	|
|Clothing, Shoes & Jewelry	|Girls	|Clothing	|Overalls	|	|MY BRAND	|Own Brand	|Conversion Rate Index~vs~Browse - YoY	|Year-over-Year (YoY Growth)	|1.104	|0.849	|1.2415	|0.962	|1.3935	|1.5238	|1.2228	|0.6624	|0.702	|0.8891	|0.9912	|1.5358	|0.0026	|1.4904	|1.3895	|1.2982	|1.8514	|1.4832	|1.2454	|1.403	|1.024	|1.0115	|0.7048	|0.3662	|0.769	|0.835	|0.3226	|0.7775	|0.9381	|0.9636	|1.0033	|0.9145	|1.064	|0.8524	|1.5342	|2.3074	|1.0285	|1.1076	|1.1931	|0.7793	|0.9443	|0.6881	|0.9004	|1.6477	|1.3139	|1.2716	|1.6457	|2.1319	|2.4256	|2.7859	|

### ASIN Report

The ASIN report provides more granular insights of how individual products within your brand are performing. A sample ASIN report is provided below.

>[Download this sample report in CSV format.](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/SampleASINReportData.csv)

|browse_path	|brand	|asin	|item_name	|Metric	|period	|value	|
|---	|---	|---	|---	|---	|---	|---	|
|/Beauty & Personal Care/Premium  Beauty/Foot, Hand & Nail Care/Nail Art & Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views Share - YoY	|Q: 2023-Q3	|4.14286	|
|/Beauty & Personal Care/Premium  Beauty/Foot, Hand & Nail Care/Nail Art & Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views	|Y: 2023	|27994	|
|/Beauty & Personal Care/Premium  Beauty/Bath & Body/Cleansers	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Units Share - YoY	|M: 2024-02	|21.4098	|
|/Beauty & Personal Care/Premium  Beauty/Premium Brands/Clairol Professional	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Shipped Product Sales (SPS)	|Y: 2023	|171302.35	|
|/Beauty & Personal Care/Premium  Beauty/Foot, Hand & Nail Care/Nail Art & Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views Share - YoY	|Q: 2023-Q3	|1.11539	|
|/Beauty & Personal Care/Premium  Beauty/Premium Beauty/Clairol Professional	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views	|M: 2022-09	|1068	|
|/Beauty & Personal Care/Premium  Beauty/Foot, Hand & Nail Care/Nail Art & Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Units Share - YoY	|M: 2024-02	|0.63724	|
|/Beauty & Personal Care/Premium  Beauty/Foot, Hand & Nail Care/Nail Art & Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Shipped Product Sales (SPS)	|Y: 2023	|219.68	|
|/Beauty & Personal Care/Premium  Beauty/Premium Brands US 3P/OPI	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Units Share - YoY	|M: 2023-05	|0.70244	|
|/Beauty & Personal Care/Premium  Beauty/Premium Beauty/Clairol Professional	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views	|Y: 2023	|192	|
|/Beauty & Personal Care/Foot, Hand  & Nail Care/Nail Art & Polish/Nail Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views Share - YoY	|M: 2023-10	|1.62097	|
|/Beauty & Personal Care/Premium  Beauty/Hair Care/Hair Color	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Shipped Product Sales (SPS)	|M: 2022-11	|21623.79	|
|/Beauty & Personal Care/Premium  Beauty/Foot, Hand & Nail Care/Nail Art & Polish	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Units Share - YoY	|Q: 2024-Q1	|1.01092	|
|/Beauty & Personal Care/Premium  Beauty/Premium Beauty/Clairol Professional	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views	|Y: 2023	|7286	|
|/Beauty & Personal Care/Premium  Beauty/Bath & Body/Cleansers	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Glance Views Share - YoY	|Q: 2023-Q1	|1	|
|/Beauty & Personal Care/Premium  Beauty/Hair Care/Hair Color	|MY\_BRAND	|MY\_ASIN	|MY\_PRODUCT\_NAME	|Shipped Product Sales (SPS)	|M: 2023-12	|11.33	|
