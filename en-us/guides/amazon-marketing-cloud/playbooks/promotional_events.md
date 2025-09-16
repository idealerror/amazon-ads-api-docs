---
title: Amazon Marketing Cloud - Promotional events
description: promotional events playbook
type: guide
interface: api
---
# AMC lookalike audiences for promotional events

Promotional or tentpole events like Prime Day and Black Friday offer significant opportunities for customer acquisition and revenue generation, potentially accounting for 20-30% of annual revenue for some brands. Advertisers can target ad-exposed users through rule-based audiences, but only have in-market or lifestyle audiences for non-ad exposed users. This subset of audiences may miss out on people looking to make their first purchase, or a gift for someone else. AMC Lookalike Audiences address these gaps by identifying potential customers who haven't been exposed to a brand, using signals from existing customers.

## Introduction

This playbook guides you through analyzing previous tentpole events, creating AMC lookalike audiences for upcoming events, querying data from AMC, analyzing the results of the previous promotional event, identifying high value audience segments, creating an AMC lookalike audience, and evaluating the created audience.
The playbook is designed for an illustrative use case. Further customization for your use cases will lead to a better outcome.

This playbook will answer the following questions:

- Which ASINs and campaigns were used in previous promotional events?
- When to target audiences for promotional events (lead-in and lead-out phases)?
- How to build an AMC Lookalike Audience for a promotional event?
- How to target a lookalike audience: keywords, frequency, and ad format?

### What is a promotional event?

A promotional or tentpole event is a significant and highly anticipated promotional activity or campaign that serves as a central focus around which other marketing efforts are organized. Promotional events are usually characterized in the data by large spikes in spend or impressions signifying a large influx of traffic.

These events are typically planned well in advance and can be tied to various occasions, such as product launches, holidays, special sales events (Prime Day, Black Friday, Cyber Monday etc.) , or significant cultural moments. The primary goal of a promotional event is to drive consumer engagement, boost brand visibility, and ultimately increase sales or conversions.

Promotional events are effective marketing strategies as they provide a focal point for a brand's efforts, generate peak consumer interest, and can result in increased brand exposure, customer engagement, and revenue. The [Tentpole Phase Overlap Instructional Query](https://advertising.amazon.com/marketing-cloud/instructional-queries/2863325acae4bc8c36b5f04743be92f5f1f01e43040a7be1f416aac3e067d338) (link only available for AMC UI users) provides a similar analysis and is referenced heavily in this playbook.

### What are AMC lookalike audiences?

AMC lookalike audiences allow advertisers to find users in the Amazon demand-side platform (Amazon DSP) with similar characteristics as the audiences they custom built in AMC. These audiences can then be activated in Amazon DSP campaigns, allowing advertisers to expand campaign reach to new audiences who share similar characteristics to an existing customer subset.

For information on how to use AMC lookalike audiences please view the following resources:

- [AMC rule-based audience lookalike audiences](guides/amazon-marketing-cloud/audiences/rule-based-lookalike)
- [Introduction to AMC Lookalike Audiences IQ](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries/b51dbe6f0ab13f09bf1d997baf84633c95fe1b72dbfd57641737ae0396a04535) (link only available for AMC UI users).

### Why use AMC lookalike audiences for promotional events?

A good portion of potential purchasers can be targeted using traditional retargeting audiences built in AMC, but these don't help find high-value, new-to-brand customers. An even more difficult segment to target is impulse shoppers, or individuals who see ads and buy within a short time window (less than 1 week). Using your existing customers, AMC lookalike audiences allow you to target impulse shoppers who are either new to your brand or have been exposed to an ad before.

You can use AMC lookalike audiences to build an audience based on previous fast-moving purchasers to get your ad in front of potential new-to-brand or repeat customers before they make their purchase. In this case, our seed audience would be "customers who converted quickly after an impression." Advertisers use the model to build an audience with similar signals to those "customers who converted quickly after an impression," and then seamlessly target them in the Amazon demand-side platform (Amazon DSP).

## Prerequisites

This playbook is for AMC users who have:

- Completed onboarding for the AMC APIs.
- Possess an Amazon DSP account.
- Have historical data for a promotional event (preferably the same one being planned for).
  The event should have occurred within the last 13 months
- Knowledge of AMC Lookalike Audiences

### Required knowledge

The concepts presented in this playbook require knowledge of SQL and working with DSP campaigns.

While there are elements that can be used by all, this playbook will be most valuable to partners with the following characteristics:

- DSP advertisers
- Endemic advertisers
- AMC analysts

### Assumptions

- The advertiser has access to historical data from any promotional event in their AMC instance, preferably a similar promotional event to the one they are planning for. If they do not have this data, we recommend utilizing the [AMC Lookalike Audiences IQ](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries/b51dbe6f0ab13f09bf1d997baf84633c95fe1b72dbfd57641737ae0396a04535)(link only available for AMC UI users).
- Price promotions for the ASINs in question will not change (If a product was 50% off last year vs 10% this year the buyer subset may change as the group last year may have been more price sensitive). If the model is trained on a group of individuals who purchased during a big sale, it will create lookalike audiences who are likely to make a purchase during a big sale.
- Actions of customers who converted during the previous promotional event can be used to identify users who are similar to them. The model relies on the signals (clicks, page views, etc.) from the seed audience to identify users who are similar to that seed audience and thus likely to convert.

## Playbook steps

The playbook is structured in the following steps:

[Step 1: Event and ASIN identification](#step-1-asin-identification)

[Step 2: Seed audience creation](#step-2-seed-audience-creation)

[Step 3: Event phase analysis](#step-3-event-phase-analysis)

[Step 4: Seed audience activation analysis](#step-4-seed-audience-activation-analysis)

### Step 1: ASIN identification

For this playbook, we will adjust an audience for a promotional event. The seed audience for a promotional event will  be narrowed down to a subset of customers from that event. In this case, building a seed around a specific ASIN or product category (group of ASINs) will instruct the model to create audiences similar to that subset of customers rather than the whole customer base.

We recommend coming with a [pre-selected product category or ASIN(s)](#option-1-predetermined-asins) that will be promoted during the promotional event (option 1).
If you do not have a pre-selected ASIN & campaigns there are additional queries below that can be used to [find ASINs](#option-2-asin-analysis) (option 2).

#### Option 1: Predetermined ASINs

If you are bringing in predetermined ASINs for your upcoming promotional event there are a few limitations as lookalikes rely on historical data.

> [NOTE] If the ASINs you select were not around during the previous promotional event, we recommend you take a broader approach (ASIN-less), or substitute in a similar product that was sold during the previous promotional event.

A more advanced option for this would be to leverage the [Audience programmatic framework playbook](guides/amazon-marketing-cloud/playbooks/programmatic_audience_framework) to identify the highest performing audiences or strategies during a previous event. This playbook provides context on measurement strategies for audiences after a campaign has finished.

The [IQ: Identify high value customer segments](https://advertising.amazon.com/marketing-cloud/instructional-queries/7ba78510f7dd5c16ab8f2763c4aab180d9974efa44ee80cb747afc599dfa9ca3) (link only available for AMC UI users) also provides an avenue to identify high performing audience segments during a previous promotional event.

You will need the following information to proceed to [Step 2](#step-2-seed-audience-creation):

- dsp campaign id
- sponsored ads campaign name
- Optional: campaign effective cost per mille (ecpm)

#### Option 2: ASIN Analysis

If you are unsure about the  ASINs that were the best-performing ones during a previous promotional event, this section will walk you through the steps to figure out which ASINs you might want to use for your current/next promotional event.

##### Exploratory query: Identify campaigns running during previous promotional event

To figure out which ASINs to use in our tentpole event, or to even leverage pre-determined ASINs, we need to identify the campaigns that were running during the previous event. These campaigns will be used throughout the playbook to build and analyze a lookalike seed audience.

If campaigns have not already been identified from a previous promotional event, utilize the query below to find campaigns running through a specific date period around a previous promotional event you would like to mirror. In this example we are looking for campaigns that were running during prime day.

###### Query

For the query set the time window with a healthy buffer, in our case for 7/11 and 7/12, we set a window of 6/01 → 08/31.

```
select campaign_id,
campaign,
campaign_start_date,
campaign_end_date,
count(distinct user_id) as unique_reach
from amazon_attributed_events_by_traffic_time
-- where ASIN in ('ASIN1', 'ASIN2')
-- Use this if you have ASINs chosen already
-- AND campaign similar to 'Prime Day'
-- Use this to help filter down campaigns
group by 1,2,3,4
```

| campaign\_id    | campaign                    | campaign\_start\_date       | campaign\_end\_date         | unique\_reach |
| --------------- | --------------------------- | --------------------------- | --------------------------- | ------------- |
| SA\- Campaign 1 | 2023\-07\-06T17:00:00\.000Z |                             | 57937                       |               |
| SA\- Campaign 2 | 2023\-07\-06T17:00:00\.000Z |                             | 37817                       |               |
| SA\- Campaign 3 | 2023\-07\-06T17:00:00\.000Z |                             | 26717                       |               |
| SA\- Campaign 4 | 2022\-06\-19T17:00:00\.000Z |                             | 23678                       |               |
| SA\- Campaign 5 | 2023\-07\-06T17:00:00\.000Z |                             | 19991                       |               |
| SA\- Campaign 6 | 2023\-07\-06T17:00:00\.000Z |                             | 17874                       |               |
| SA\- Campaign 7 | 2022\-06\-19T17:00:00\.000Z |                             | 17317                       |               |
| SA\- Campaign 8 | 2023\-07\-06T17:00:00\.000Z |                             | 16028                       |               |
| 1234567         | DSP\- Campaign 1            | 2023\-07\-10T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 11708         |
| 2234567         | DSP\- Campaign 2            | 2023\-07\-06T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 11382         |
| 3234567         | DSP\- Campaign 3            | 2023\-07\-06T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 10852         |
| 4234567         | DSP\- Campaign 4            | 2023\-07\-10T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 10103         |
| 5234567         | DSP\- Campaign 5            | 2023\-07\-06T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 8428          |
| 6234567         | DSP\- Campaign 6            | 2023\-07\-10T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 8208          |
| 7234567         | DSP\- Campaign 7            | 2023\-07\-06T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 8064          |
| 8234567         | DSP\- Campaign 8            | 2023\-07\-06T20:00:00\.000Z | 2023\-07\-12T20:00:00\.000Z | 8063          |

###### Interpretation

For the following analyses, we suggest incorporating data from all active campaigns. However, you can select specific campaigns at your discretion as you prefer.

##### Exploratory query: Identify ASINs purchased

**Optional**: If you have already chosen your ASINs you can skip this section

Once the campaigns have been identified, we can use them in a query to identify ASINs for the previous promotional event. This will allow us to build our seed audience around a specific set of campaigns & ASINs.

###### Parameters - campaigns

```
CAMPAIGN_SCOPE (campaign_name) AS (
    VALUES
        ('SA - Campaign 1'),
        ('SA - Campaign 2'),
        ('SA - Campaign 3'),
        ('SA - Campaign 4'),
        ('SA - Campaign 5'),
        ('SA - Campaign 6'),
        ('SA - Campaign 7'),
        ('SA - Campaign 8'),
        ('DSP - Campaign 1'),
        ('DSP - Campaign 2'),
        ('DSP - Campaign 3'),
        ('DSP - Campaign 4'),
        ('DSP - Campaign 5'),
        ('DSP - Campaign 6'),
        ('DSP - Campaign 7'),
        ('DSP - Campaign 8')
)
```

###### Query

```
WITH CAMPAIGN_SCOPE (campaign_name) AS (
    VALUES
        ('SA - Campaign 1'),
        ('SA - Campaign 2'),
        ('SA - Campaign 3'),
        ('SA - Campaign 4'),
        ('SA - Campaign 5'),
        ('SA - Campaign 6'),
        ('SA - Campaign 7'),
        ('SA - Campaign 8'),
        ('DSP - Campaign 1'),
        ('DSP - Campaign 2'),
        ('DSP - Campaign 3'),
        ('DSP - Campaign 4'),
        ('DSP - Campaign 5'),
        ('DSP - Campaign 6'),
        ('DSP - Campaign 7'),
        ('DSP - Campaign 8')
)
SELECT 
    tracked_asin, 
    SUM(conversions) AS conversions_by_asin
FROM 
    amazon_attributed_events_by_traffic_time a
INNER JOIN 
    (SELECT campaign_name FROM CAMPAIGN_SCOPE) S
    ON a.campaign = S.campaign_name
GROUP BY 
    1
```

###### Results table

| tracked\_asin | conversions\_by\_asin |
| ------------- | --------------------- |
| B0014C2NBC    | 470                   |
| B0014C5S7S    | 411                   |
| B0014BYHJE    | 354                   |
| B0014C0LSY    | 320                   |
| B001526RFO    | 318                   |
| B01H70LSHE    | 303                   |
| B0961NR8KN    | 300                   |
| B0831R88S6    | 297                   |
| B0014BYHGC    | 296                   |
| B01H70LTW8    | 278                   |
| B000ZPDDW2    | 272                   |

###### Interpretation

Based on the ASINs here we would recommend two primary approaches:

**Specific product category to target**: If you have an internal mapping available, start by grouping ASINs together that are in a similar product category. This will allow for the AMC lookalike audience model to find the ideal customer for that specific category of ASINs. We recommend this approach for advertisers that have multiple product categories or target audiences.
**Specific list of ASINS to target**: Choose the ASINS you want to target for the promotional event. This may result in a broader target group, but will still yield positive results based on your products. We would recommend this approach for advertisers that sell a specific category of products; example - baby products, makeup, etc.

### Step 2: Seed audience creation

For information on how to create a seed audience and lookalike audience best practices see [IQ: Introduction to AMC Lookalike Audiences](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries/b51dbe6f0ab13f09bf1d997baf84633c95fe1b72dbfd57641737ae0396a04535) (link only available for AMC UI users) and [Rule-based lookalike audiences](guides/amazon-marketing-cloud/audiences/rule-based-lookalike).

Now that the ASINs and campaigns have been identified, we can leverage that information to create a seed audience. A seed audience is a group of user ids that have been selected via an AMC query. The seed audience follows the same logic as a standard rule-based audience.

This section will help identify an appropriate seed for your promotional event lookalike audience based on what subset of users performed best during a previous promotional event.

#### Flight time analysis

Determining how long it takes from first impression to conversion is crucial for choosing our seed audience. For this example, our goal is to target impulse buyers, or those who buy shortly after being exposed to an ad. To target those individuals we need to build a seed audience of customers who converted shortly after being exposed to an ad.

The query below splits the time to conversion relative to the conversion that occurred during the window specified:

- First reached 1 day ago
- First reached 2-7 days ago
- First reached 1-2 weeks ago (7-14 days)
- First reached 2-3 weeks ago (14-21 days)
- First reached 3-4 weeks ago (21-28 days)
- First reached 5+ weeks ago (greater than 28 days)

##### Parameters

The parameters below will be used in a number of queries to follow and define the campaigns that were running during the previous promotional event and/or define the ASINs utilized to create your lookalike audience. For the query below fill in each line by campaign in the following format:

`(DSP Campaign ID, Sponsored Ads Campaign Name, ECPM, include_brand_halo)`

- `DISPLAY_campaign_id`: Default for this is -1, fill in the value for your campaign
- `sa_campaign_name`: Default for this is ‘’, fill in campaign name if you wish to include
- `ecpm` (effective cost per mille): Default for this is -1, if you wish to include a value you may
- `include_brand_halo`: 1 to include as default (-1 to exclude)

```
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id, 
    sa_campaign_name, 
    ecpm, 
    include_brand_halo
) AS (
    VALUES
        (-1, 'SponsoredAdsCampaign1', -1, 1),
        (-1, 'SponsoredAdsCampaign2', -1, 1),
        (-1, 'SponsoredAdsCampaign3', -1, 1),
        (-1, 'SponsoredAdsCampaign4', -1, 1),
        (-1, 'SponsoredAdsCampaign5', -1, 1),
        (-1, 'SponsoredAdsCampaign6', -1, 1),
        (-1, 'SponsoredAdsCampaign7', -1, 1),
        (-1, 'SponsoredAdsCampaign8', -1, 1),
        (1111111111111, '', -1, 1),
        (2222222222222, '', -1, 1),
        (3333333333333, '', -1, 1),
        (4444444444444, '', -1, 1),
        (5555555555555, '', -1, 1),
        (6666666666666, '', -1, 1),
        (7777777777777, '', -1, 1),
        (8888888888888, '', -1, 1) 
)
AND tracked_item IN (
    'ASIN1', 
    'ASIN2', 
    'ASIN3', 
    'ASIN4', 
    'ASIN5', 
    'ASIN6', 
    'ASIN7', 
    'ASIN8', 
    'ASIN9', 
    'ASIN10', 
    'ASIN11'
)
```

##### Query

Update the following query on lines where `AND tracked_item in (‘ASIN1’, ‘ASIN2’, ... ‘ASIN_N’)`

<details class="details-bar">
<summary>Click to see query: flight time  </summary>

```
-- AMC Tentpole Deep Dive Module: Flight Time
-- UPDATE: Define campaign list in a comma-separated format with either 
-- dsp_campaign_id populated (with an empty string for sa_campaign_name) 
-- or sa_campaign_name populated (with a -1 for dsp_campaign_id). 
-- For automatic use of AMC cost data, populate the ecpm column with -1; 
-- alternatively, provide an ecpm figure for the campaign in question. 
-- Populate 1 in the include_brand_halo column if you want to consider both 
-- promoted and brand halo ASIN conversions; alternatively, 
-- populate 0 if you want to consider promoted ASIN conversions only.
WITH CAMPAIGN_SCOPE (
    dsp_campaign_id,
    sa_campaign_name,
    ecpm,
    include_brand_halo
) AS (
    VALUES
        (-1, 'SponsoredAdsCampaign1', -1, 1),
        (-1, 'SponsoredAdsCampaign2', -1, 1),
        (-1, 'SponsoredAdsCampaign3', -1, 1),
        (-1, 'SponsoredAdsCampaign4', -1, 1),
        (-1, 'SponsoredAdsCampaign5', -1, 1),
        (-1, 'SponsoredAdsCampaign6', -1, 1),
        (-1, 'SponsoredAdsCampaign7', -1, 1),
        (-1, 'SponsoredAdsCampaign8', -1, 1),
        (1111111111111, '', -1, 1),
        (2222222222222, '', -1, 1),
        (3333333333333, '', -1, 1),
        (4444444444444, '', -1, 1),
        (5555555555555, '', -1, 1),
        (6666666666666, '', -1, 1),
        (7777777777777, '', -1, 1),
        (8888888888888, '', -1, 1)
),

CONVERSIONS_PREP AS (
    -- Gather Conversions data for DSP campaigns without brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT dsp_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE dsp_campaign_id != -1
                AND CAST(include_brand_halo AS TINYINT) = 0
        ) S ON C.campaign_id = S.dsp_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1',
            'ASIN2',
            'ASIN3',
            'ASIN4',
            'ASIN5',
            'ASIN6',
            'ASIN7',
            'ASIN8',
            'ASIN9',
            'ASIN10',
            'ASIN11'
        )
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Conversions data for DSP campaigns with brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(total_product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT dsp_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE dsp_campaign_id != -1
                AND CAST(include_brand_halo AS TINYINT) = 1
        ) S ON C.campaign_id = S.dsp_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1',
            'ASIN2',
            'ASIN3',
            'ASIN4',
            'ASIN5',
            'ASIN6',
            'ASIN7',
            'ASIN8',
            'ASIN9',
            'ASIN10',
            'ASIN11'
        )
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns without brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
                AND CAST(include_brand_halo AS TINYINT) = 0
        ) S ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1',
            'ASIN2',
            'ASIN3',
            'ASIN4',
            'ASIN5',
            'ASIN6',
            'ASIN7',
            'ASIN8',
            'ASIN9',
            'ASIN10',
            'ASIN11'
        )
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns with brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(total_product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
                AND CAST(include_brand_halo AS TINYINT) = 1
        ) S ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1',
            'ASIN2',
            'ASIN3',
            'ASIN4',
            'ASIN5',
            'ASIN6',
            'ASIN7',
            'ASIN8',
            'ASIN9',
            'ASIN10',
            'ASIN11'
        )
    GROUP BY 1, 2, 3
),

CONVERSIONS AS (
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(sales) AS sales
    FROM
        CONVERSIONS_PREP
    GROUP BY 1, 2, 3
),

IMPRESSIONS_PREP AS (
    -- Gather Impressions data for DSP campaigns
    SELECT
        user_id,
        impression_dt
    FROM
        DSP_IMPRESSIONS I
        INNER JOIN (
            SELECT dsp_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE dsp_campaign_id != -1
        ) S ON I.campaign_id = S.dsp_campaign_id
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    -- Gather Impressions data for Sponsored Ads campaigns
    SELECT
        user_id,
        event_dt AS impression_dt
    FROM
        SPONSORED_ADS_TRAFFIC I
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
        ) S ON I.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    SELECT
        user_id,
        event_dt AS impression_dt
    FROM
        TABLE(EXTEND_TIME_WINDOW('SPONSORED_ADS_TRAFFIC', 'P28D', 'P0D'))
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    -- Gather Impressions data for DSP campaigns
    SELECT
        user_id,
        impression_dt
    FROM
        TABLE(EXTEND_TIME_WINDOW('DSP_IMPRESSIONS', 'P28D', 'P0D')) I
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2
),

IMPRESSIONS AS (
    SELECT
        user_id,
        MIN(impression_dt) AS first_impression_dt
    FROM
        IMPRESSIONS_PREP
    GROUP BY 1
),

COMBINED AS (
    SELECT
        P.user_id,
        P.conversion_event_date,
        P.sales,
        CASE
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (24*60*60)
                THEN 'FIRST REACHED 1 DAY AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (1*7*24*60*60)
                THEN 'FIRST REACHED 2-7 DAYS AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (2*7*24*60*60)
                THEN 'FIRST REACHED 1-2 WEEKS AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (3*7*24*60*60)
                THEN 'FIRST REACHED 2-3 WEEKS AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (4*7*24*60*60)
                THEN 'FIRST REACHED 3-4 WEEKS AGO'
            ELSE 'FIRST REACHED 5+ WEEKS AGO'
        END AS time_to_conversion
    FROM
        CONVERSIONS P
        LEFT JOIN IMPRESSIONS I ON P.user_id = I.user_id
    WHERE
        P.conversion_event_dt >= I.first_impression_dt
)

SELECT
    conversion_event_date,
    time_to_conversion,
    COUNT(DISTINCT user_id) as Distinct_Buyers,
    SUM(sales) AS sales
FROM
    COMBINED
GROUP BY 1, 2
```

</details>

##### Output table

| conversion\_event\_date | time\_to\_conversion         | sales     | Percentage |
| ----------------------- | ---------------------------- | --------- | ---------- |
| 7/7/2023                | FIRST REACHED 2\-7 DAYS AGO  | 187\.14   | 0\.05%     |
| 7/7/2023                | FIRST REACHED 1 DAY AGO      | 469\.54   | 0\.12%     |
| 7/7/2023                | FIRST REACHED 2\-3 WEEKS AGO | 243\.71   | 0\.06%     |
| 7/7/2023                | FIRST REACHED 1\-2 WEEKS AGO | 95\.36    | 0\.02%     |
| 7/7/2023                | FIRST REACHED 5\+ WEEKS AGO  | 149\.94   | 0\.04%     |
| 7/7/2023                | FIRST REACHED 3\-4 WEEKS AGO | 416\.98   | 0\.11%     |
| 7/8/2023                | FIRST REACHED 1\-2 WEEKS AGO | 244\.62   | 0\.06%     |
| 7/8/2023                | FIRST REACHED 2\-3 WEEKS AGO | 417\.78   | 0\.11%     |
| 7/8/2023                | FIRST REACHED 2\-7 DAYS AGO  | 557\.74   | 0\.15%     |
| 7/8/2023                | FIRST REACHED 3\-4 WEEKS AGO | 408\.26   | 0\.11%     |
| 7/8/2023                | FIRST REACHED 5\+ WEEKS AGO  | 116\.17   | 0\.03%     |
| 7/8/2023                | FIRST REACHED 1 DAY AGO      | 464\.16   | 0\.12%     |
| 7/9/2023                | FIRST REACHED 5\+ WEEKS AGO  | 202\.95   | 0\.05%     |
| 7/9/2023                | FIRST REACHED 1\-2 WEEKS AGO | 269\.07   | 0\.07%     |
| 7/9/2023                | FIRST REACHED 2\-7 DAYS AGO  | 435\.33   | 0\.11%     |
| 7/9/2023                | FIRST REACHED 2\-3 WEEKS AGO | 484\.9    | 0\.13%     |
| 7/9/2023                | FIRST REACHED 3\-4 WEEKS AGO | 636\.62   | 0\.17%     |
| 7/9/2023                | FIRST REACHED 1 DAY AGO      | 373\.93   | 0\.10%     |
| 7/10/2023               | FIRST REACHED 2\-3 WEEKS AGO | 399\.57   | 0\.10%     |
| 7/10/2023               | FIRST REACHED 3\-4 WEEKS AGO | 842\.96   | 0\.22%     |
| 7/10/2023               | FIRST REACHED 1 DAY AGO      | 598\.25   | 0\.16%     |
| 7/10/2023               | FIRST REACHED 1\-2 WEEKS AGO | 385\.83   | 0\.10%     |
| 7/10/2023               | FIRST REACHED 2\-7 DAYS AGO  | 670\.19   | 0\.17%     |
| 7/10/2023               | FIRST REACHED 5\+ WEEKS AGO  | 739\.36   | 0\.19%     |
| 7/11/2023               | FIRST REACHED 1 DAY AGO      | 65887\.29 | 17\.17%    |
| 7/12/2023               | FIRST REACHED 1 DAY AGO      | 60162\.25 | 15\.68%    |
| 7/11/2023               | FIRST REACHED 1\-2 WEEKS AGO | 19074\.74 | 4\.97%     |
| 7/12/2023               | FIRST REACHED 1\-2 WEEKS AGO | 16479\.47 | 4\.29%     |
| 7/11/2023               | FIRST REACHED 2\-3 WEEKS AGO | 21976\.76 | 5\.73%     |
| 7/12/2023               | FIRST REACHED 2\-3 WEEKS AGO | 19041     | 4\.96%     |
| 7/12/2023               | FIRST REACHED 2\-7 DAYS AGO  | 37923\.03 | 9\.88%     |
| 7/11/2023               | FIRST REACHED 2\-7 DAYS AGO  | 26148     | 6\.81%     |
| 7/11/2023               | FIRST REACHED 3\-4 WEEKS AGO | 24186\.83 | 6\.30%     |
| 7/12/2023               | FIRST REACHED 3\-4 WEEKS AGO | 22979\.54 | 5\.99%     |
| 7/12/2023               | FIRST REACHED 5\+ WEEKS AGO  | 30252\.83 | 7\.88%     |
| 7/11/2023               | FIRST REACHED 5\+ WEEKS AGO  | 25623\.51 | 6\.68%     |
| 7/13/2023               | FIRST REACHED 2\-3 WEEKS AGO | 1307\.39  | 0\.34%     |
| 7/13/2023               | FIRST REACHED 1 DAY AGO      | 923\.23   | 0\.24%     |
| 7/13/2023               | FIRST REACHED 3\-4 WEEKS AGO | 2011\.64  | 0\.52%     |

![PE_Fig1](/_images/amazon-marketing-cloud/playbooks/pe/pe_1.png)

From the visual we can see the majority of purchases occurred during the promotional event on 7/11 and 7/12. A large percentage of the conversions were from consumers who were first reach less than 1 week before the promotional event.

##### Conversion Table - Promotional Event

The table below is grouped on time to conversion across the dates of the promotional event. For this it was 7/11/2023 and 7/12/2023.

| time\_to\_conversion         | Conversions | Percentage |
| ---------------------------- | ----------- | ---------- |
| FIRST REACHED 1 DAY AGO      | 126,050     | 34\.09%    |
| FIRST REACHED 2\-7 DAYS AGO  | 64,071      | 17\.33%    |
| FIRST REACHED 1\-2 WEEKS AGO | 35,554      | 9\.62%     |
| FIRST REACHED 2\-3 WEEKS AGO | 41,018      | 11\.09%    |
| FIRST REACHED 3\-4 WEEKS AGO | 47,166      | 12\.76%    |
| FIRST REACHED 5\+ WEEKS AGO  | 55,876      | 15\.11%    |

##### Interpretation

This table can help you determine how to build your seed audience. Our seed limit is between 500 and 500,000 users as defined by the [lookalike documentation](guides/amazon-marketing-cloud/audiences/rule-based-lookalike). Since we are looking at historical data and our time window will not change, we don’t have to leave a buffer and can select any audience size between that limit.

Our use case is to target a subset of conversions from the previous promotional event. In this case, we would like to build our seed audience of users whose time to conversion was less than a day. If we didn’t have 500 users in that bucket we might consider including other date ranges in this audience.

The time from first impression to purchase with a time of less than 1 week was over 50%. Almost 80% of purchases fell into the “1 day to 7 days” bracket, or the “3 weeks to 5 weeks bracket”.

##### Actions

Rule based audiences would be highly effective in targeting the individuals who were reached in the weeks leading up to the promotional event. Given the minimum refresh time of 1 day, rule based audiences would not be able to reach the large number of fast converting customers: particularly those who saw an ad a day before or on the promotional event itself.

Given the fast time to purchase for the 1 day to 7 day group, a more proactive approach can be used to identify individuals with similar habits to those who purchased in the previous tentpole with a lower time from first impression to purchase.

AMC lookalike audiences are perfect for this situation, the model can use an existing audience of fast buying customers from the previous event, and generate new audiences that show similar signals to that previous group of customers. This allows an advertiser to target NTB impulse shoppers before they even see an ad from the advertiser.

#### Seed creation process

To address the problem of not being able to target users who purchase immediately, we will look to train an AMC lookalike model on the users from last year who converted quickly. The model will utilize those users to identify behavioral signals that led up to their decision to purchase. It will then identify a new subset of users for you to target for the upcoming promotional event. This allows you to get your message in front of prospective buyers before they begin to interact with your brand.

To create your seed utilize the following query & parameters. The only section that will change is the bottom portion of the query

> [NOTE] The seed can also be submitted via the **AMC UI** under the **Audiences Tab**. Further instructions can be found in the [Lookalike Audiences IQ](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries/b51dbe6f0ab13f09bf1d997baf84633c95fe1b72dbfd57641737ae0396a04535) (link only available for AMC UI users).

##### Parameters - campaigns

```
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id, 
    sa_campaign_name, 
    ecpm, 
    include_brand_halo) AS (
    VALUES
        (-1,'SponsoredAdsCampaign1',-1,1),
        (-1,'SponsoredAdsCampaign2',-1,1),
        (-1,'SponsoredAdsCampaign3',-1,1),
        (-1,'SponsoredAdsCampaign4',-1,1),
        (-1,'SponsoredAdsCampaign5',-1,1),
        (-1,'SponsoredAdsCampaign6',-1,1),
        (-1,'SponsoredAdsCampaign7',-1,1),
        (-1,'SponsoredAdsCampaign8',-1,1),
        (1111111111111,'',-1,1),
        (2222222222222,'',-1,1),
        (3333333333333,'',-1,1),
        (4444444444444,'',-1,1),
        (5555555555555,'',-1,1),
        (6666666666666,'',-1,1),
        (7777777777777,'',-1,1),
        (8888888888888,'',-1,1)                
),
```

##### Query

<details class="details-bar">
<summary>Click to see query: seed creation  </summary>

```
-- AMC Tentpole Deep Dive Module: Flight Time
-- UPDATE: Define campaign list in a comma-separated format with either 
-- dsp_campaign_id populated (with an empty string for sa_campaign_name) 
-- or sa_campaign_name populated (with a -1 for dsp_campaign_id). 
-- For automatic use of AMC cost data, populate the ecpm column with -1; 
-- alternatively, provide an ecpm figure for the campaign in question. 
-- Populate 1 in the include_brand_halo column if you want to consider both 
-- promoted and brand halo ASIN conversions; alternatively, populate 0 
-- if you want to consider promoted ASIN conversions only.
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id,
    sa_campaign_name,
    ecpm,
    include_brand_halo
) AS (
    VALUES
        (-1, 'SponsoredAdsCampaign1', -1, 1),
        (-1, 'SponsoredAdsCampaign2', -1, 1),
        (-1, 'SponsoredAdsCampaign3', -1, 1),
        (-1, 'SponsoredAdsCampaign4', -1, 1),
        (-1, 'SponsoredAdsCampaign5', -1, 1),
        (-1, 'SponsoredAdsCampaign6', -1, 1),
        (-1, 'SponsoredAdsCampaign7', -1, 1),
        (-1, 'SponsoredAdsCampaign8', -1, 1),
        (1111111111111, '', -1, 1),
        (2222222222222, '', -1, 1),
        (3333333333333, '', -1, 1),
        (4444444444444, '', -1, 1),
        (5555555555555, '', -1, 1),
        (6666666666666, '', -1, 1),
        (7777777777777, '', -1, 1),
        (8888888888888, '', -1, 1)
),

CONVERSIONS_PREP AS (
    -- Gather Conversions data for DSP campaigns without brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME_FOR_AUDIENCES C
        INNER JOIN (
            SELECT DISPLAY_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE DISPLAY_campaign_id != -1
                AND CAST(include_brand_halo AS TINYINT) = 0
        ) S ON C.campaign_id = S.DISPLAY_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        /*AND tracked_item in (
            'ASIN1',
            'ASIN2',
            'ASIN3',
            'ASIN4',
            'ASIN5',
            'ASIN6',
            'ASIN7',
            'ASIN8',
            'ASIN9',
            'ASIN10',
            'ASIN11')
        */
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Conversions data for DSP campaigns with brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(total_product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME_FOR_AUDIENCES C
        INNER JOIN (
            SELECT DISPLAY_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE DISPLAY_campaign_id != -1
                AND CAST(include_brand_halo AS TINYINT) = 1
        ) S ON C.campaign_id = S.DISPLAY_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        /*AND tracked_item in (
            'ASIN1', 'ASIN2', 'ASIN3', 'ASIN4', 'ASIN5',
            'ASIN6', 'ASIN7', 'ASIN8', 'ASIN9', 'ASIN10', 'ASIN11')
        */
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns without brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME_FOR_AUDIENCES C
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
                AND CAST(include_brand_halo AS TINYINT) = 0
        ) S ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns with brand halo
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(total_product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME_FOR_AUDIENCES C
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
                AND CAST(include_brand_halo AS TINYINT) = 1
        ) S ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
    GROUP BY 1, 2, 3
),

CONVERSIONS AS (
    SELECT
        user_id,
        conversion_event_dt,
        conversion_event_date,
        SUM(sales) AS sales
    FROM
        CONVERSIONS_PREP
    GROUP BY 1, 2, 3
),

IMPRESSIONS_PREP AS (
    -- Gather Impressions data for DSP campaigns
    SELECT
        user_id,
        impression_dt
    FROM
        DSP_IMPRESSIONS_FOR_AUDIENCES I
        INNER JOIN (
            SELECT DISPLAY_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE DISPLAY_campaign_id != -1
        ) S ON I.campaign_id = S.DISPLAY_campaign_id
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    -- Gather Impressions data for Sponsored Ads campaigns
    SELECT
        user_id,
        event_dt AS impression_dt
    FROM
        SPONSORED_ADS_TRAFFIC_FOR_AUDIENCES I
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
        ) S ON I.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    SELECT
        user_id,
        event_dt AS impression_dt
    FROM
        TABLE(EXTEND_TIME_WINDOW('SPONSORED_ADS_TRAFFIC_FOR_AUDIENCES', 'P28D', 'P0D'))
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    -- Gather Impressions data for DSP campaigns
    SELECT
        user_id,
        impression_dt
    FROM
        TABLE(EXTEND_TIME_WINDOW('DSP_IMPRESSIONS_FOR_AUDIENCES', 'P28D', 'P0D')) I
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2
),

IMPRESSIONS AS (
    SELECT
        user_id,
        MIN(impression_dt) AS first_impression_dt
    FROM
        IMPRESSIONS_PREP
    GROUP BY 1
),

COMBINED AS (
    SELECT
        P.user_id,
        P.conversion_event_date,
        P.sales,
        CASE
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (24*60*60)
                THEN 'FIRST REACHED 1 DAY AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (1*7*24*60*60)
                THEN 'FIRST REACHED 2-7 DAYS AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (2*7*24*60*60)
                THEN 'FIRST REACHED 1-2 WEEKS AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (3*7*24*60*60)
                THEN 'FIRST REACHED 2-3 WEEKS AGO'
            WHEN SECONDS_BETWEEN(first_impression_dt, conversion_event_dt) <= (4*7*24*60*60)
                THEN 'FIRST REACHED 3-4 WEEKS AGO'
            ELSE 'FIRST REACHED 5+ WEEKS AGO'
        END AS time_to_conversion
    FROM
        CONVERSIONS P
        LEFT JOIN IMPRESSIONS I ON P.user_id = I.user_id
    WHERE
        P.conversion_event_dt >= I.first_impression_dt
)

SELECT DISTINCT
    user_id
FROM 
    Combined
WHERE 
    time_to_conversion = 'FIRST REACHED 1 DAY AGO'
    OR time_to_conversion = 'FIRST REACHED 2-7 DAYS AGO
```

</details>

##### Submit Seed to API

The following API payload should be sent following the documentation here: Rule-Based Lookalike Audiences

The following parameters should be set as follows:

`refreshRateDays`: 7 - We recommend this to ensure optimal performance
`timeWindowRelative`: FALSE - Keep time window consistent so it is always looking at the individuals from the previous event

For more information on other parameters like “expected reach” please view the IQ here: [Introduction to AMC Lookalike Audiences](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries/b51dbe6f0ab13f09bf1d997baf84633c95fe1b72dbfd57641737ae0396a04535) (link only available for AMC UI users).

The query from above should be flattened and placed into this JSON payload to be submitted to the API

```

{ "audienceName": "Lookalike Audience Test", "audienceDescription": "Audience for xyz lookalike audience", "advertiserId": "123456789", "query": "insert_flattened_query_here", "timeWindowStart": "2023-07-11T00:00:00Z", "timeWindowEnd": "2023-07-12T00:00:00Z", "refreshRateDays": 7, "timeWindowRelative": "FALSE", "lookalikeAudienceExpectedReach": "BALANCED" }

```

### Step 3: Event phase analysis

Once we have created our lookalike audience, we can perform an analysis to determine how we should target the audience through sponsored ads and the Amazon DSP. The goal of this section is to provide three date ranges to utilize for future queries.

The following section should be done utilizing the ASINS above for a more precise approach, or can be done ASIN-less for more generalized insights.

#### Lead-in phase analysis

This is the initial stage of a promotional event timeline, where the focus is on creating awareness and capturing the attention of potential customers. This phase is about introducing your brand, product, or service to a wider audience and piquing their interest. The length of this phase can be vary widely by product and budget, with the steps below lined up.
For an AMC lookalike audience in a promotional event, our lead-in period is the number of days before conversion that users saw the ad. Refer to step 2.1 Flight-Time to solve this problem.

Per chart 2.2, and the decision to build a seed on the 1-7 day conversion audience, we would look to have a minimum of 7 days of lead time for this audience. Additional time in the DSP will allow for optimization of your audience, so if the budget exists, having a longer lead-in time than 7 days would be beneficial.

#### Promotional event phase analysis

This is the core part of your promotional event where you will make the vast majority of your sales. Usually this phase is accompanied by deals or incentives to get customers to purchase. The timeline for this phase generally revolves around a pre-set time window where the promotional event will occur (Prime Day, Black Friday, etc.). For our example in this playbook, the date was Prime Day 2023 - July 11th & 12th.

#### Lead-out phase analysis

Here awareness has been established and potential customers have shown interest in your offerings. At this point you are targeting customers who did not purchase when provided with an incentive, so this phase is about nurturing the relationships you've built and guiding potential customers towards making a future purchase.

Recommendation: Target your audiences here using rule based audiences on individuals who have been exposed to an ad, had a detailed page view, added to cart but didn’t purchase, or other lower funnel rule based audience strategies. This can be paired with the AMC lookalike audience to focus on the high value customers identified by the model.

##### Lead-out query: Impressions / conversions by day

The goal of the lead out period is to capture as many additional sales as possible, nurture relationships that were built, and ultimately guide customers towards making a future purchase. For the lead out portion of your campaign we do not recommend utilizing AMC lookalike audiences as the high percentage of new to brand customers would not be a good fit for this.

Instead we recommend utilizing AMC rule based audiences to target these individuals who decided not to purchase during the promotional event. Some examples of these audiences can be found in the [AMC Instructional Query Library](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries) (link only available for AMC UI users).

###### Query

```
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id, 
    sa_campaign_name, 
    ecpm, 
    include_brand_halo) AS (
    VALUES
        (-1,'SponsoredAdsCampaign1',-1,1),
        (-1,'SponsoredAdsCampaign2',-1,1),
        (-1,'SponsoredAdsCampaign3',-1,1),
        (-1,'SponsoredAdsCampaign4',-1,1),
        (-1,'SponsoredAdsCampaign5',-1,1),
        (-1,'SponsoredAdsCampaign6',-1,1),
        (-1,'SponsoredAdsCampaign7',-1,1),
        (-1,'SponsoredAdsCampaign8',-1,1),
        (1111111111111,'',-1,1),
        (2222222222222,'',-1,1),
        (3333333333333,'',-1,1),
        (4444444444444,'',-1,1),
        (5555555555555,'',-1,1),
        (6666666666666,'',-1,1),
        (7777777777777,'',-1,1),
        (8888888888888,'',-1,1)                  
)

select  
    impression_date, 
    sum(impressions) as impressions_by_date, 
    sum(conversions) as conversions_by_date,
    COUNT(DISTINCT user_id) as Distinct_Buyers
from amazon_attributed_events_by_traffic_time a
INNER JOIN (SELECT sa_campaign_name FROM CAMPAIGN_SCOPE) S 
    ON a.campaign = S.sa_campaign_name
group by 1
```

###### Results table

| conversion\_event\_date | impressions\_by\_date | conversions\_by\_date |
| ----------------------- | --------------------- | --------------------- |
| 7/7/2023                | 15967                 | 22576                 |
| 7/8/2023                | 10691                 | 17255                 |
| 7/9/2023                | 15933                 | 24170                 |
| 7/10/2023               | 18476                 | 32134                 |
| 7/11/2023               | 68107                 | 399402                |
| 7/12/2023               | 157379                | 418294                |
| 7/13/2023               | 44581                 | 70348                 |
| 7/14/2023               | 29698                 | 47734                 |
| 7/15/2023               | 27513                 | 44719                 |
| 7/16/2023               | 29759                 | 47615                 |
| 7/17/2023               | 24662                 | 39640                 |
| 7/18/2023               | 24303                 | 38433                 |
| 7/19/2023               | 23844                 | 37246                 |
| 7/20/2023               | 22398                 | 34809                 |
| 7/21/2023               | 18887                 | 30051                 |
| 7/22/2023               | 17560                 | 27807                 |
| 7/23/2023               | 17711                 | 28161                 |
| 7/24/2023               | 16191                 | 27458                 |
| 7/25/2023               | 12645                 | 21527                 |
| 7/26/2023               | 4979                  | 11580                 |
| 7/27/2023               |                       | 4708                  |
| 7/28/2023               |                       | 4968                  |
| 7/29/2023               |                       | 5563                  |
| 7/30/2023               |                       | 5410                  |
| 7/31/2023               |                       | 5867                  |
| 8/1/2023                |                       | 5575                  |
| 8/2/2023                |                       | 5922                  |
| 8/3/2023                |                       | 4972                  |
| 8/4/2023                |                       | 4909                  |
| 8/5/2023                |                       | 5469                  |
| 8/6/2023                |                       | 4213                  |
| 8/7/2023                |                       | 5263                  |
| 8/8/2023                |                       | 5548                  |
| 8/9/2023                |                       | 5495                  |
| 8/10/2023               |                       | 5373                  |
| 8/11/2023               |                       | 5109                  |
| 8/12/2023               |                       | 5051                  |

###### Visual

The following visual is based off the results table.

![PE_Fig2](/_images/amazon-marketing-cloud/playbooks/pe/pe_2.png)

###### Interpretation

Based on the table above, we can see that conversions tail off on 7/27, returning back to their normal baseline. For this we would recommend a leadout phase of 14 days. This leadout phase would be focused primarily around lower funnel strategies with a unique combination of rule based audiences.

- **Broad targeting strategy**: Utilize traditional retargeting tactics over these 14 days such as detailed page views or add-to-cart did not purchase.
- **Specific targeting strategy**: Utilize the retargeting tactics from above, and limit by users in your AMC lookalike audience. This is a more high value targeting strategy

#### Phase analysis outcome

Based on the analysis above the final dates were decided for each of the phases:

- Lead-in: 7/4 to 7/10: minimum of 7 days before depending on funding
- Promotional event:  7/11 to 7/12:  Duration of event
- Lead-out: 7/13 to 7/26:  Day before conversions trailed off for previous tentpole event

### Step 4: Seed audience activation analysis

The following section will guide you through a series of analyses that help answer questions for all 3 phases (Lead in, promotional event, lead out,) on:

- [Frequency analysis](#frequency-analysis)
- [Ad format analysis (DSP vs. sponsored ads)](#ad-format-analysis-dsp-vs-sponsored-ads)
- [Keyword analysis](#keyword-analysis)

#### Frequency analysis

How often you reach your potential customers during a promotional event can help you get a competitive edge on the rest of the market, particularly in busy periods surrounding holidays and major promotional events. This section will help you determine a frequency strategy for each stage of a promotional event to maximize ad coverage while still seeing a positive return on ad-spend.

We assume that the ASINs you chose were utilized in a previous promotional event or ASINs chosen are similar to the upcoming event’s ASINs.

##### Instructions

The following process will be based on the lead-in, event, and lead out dates. The query and corresponding interpretation should be done for all 3 phases to determine the optimal targeting amount for your chosen audience.

For ease of use, restrict your query each time using the date range as the limiting factor. Below are the three dates that were used for this example and the two following analyses.

- Date range 1: June 26, 2023 - July 10, 2023
- Date range 2: July 11, 2023 - July 12, 2023
- Date range 3: July 13, 2023 - July 27, 2023

##### Parameters - campaigns

```
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id, 
    sa_campaign_name, 
    ecpm, 
    include_brand_halo) AS (
   VALUES
        (-1,'SponsoredAdsCampaign1',-1,1),
        (-1,'SponsoredAdsCampaign2',-1,1),
        (-1,'SponsoredAdsCampaign3',-1,1),
        (-1,'SponsoredAdsCampaign4',-1,1),
        (-1,'SponsoredAdsCampaign5',-1,1),
        (-1,'SponsoredAdsCampaign6',-1,1),
        (-1,'SponsoredAdsCampaign7',-1,1),
        (-1,'SponsoredAdsCampaign8',-1,1),
        (1111111111111,'',-1,1),
        (2222222222222,'',-1,1),
        (3333333333333,'',-1,1),
        (4444444444444,'',-1,1),
        (5555555555555,'',-1,1),
        (6666666666666,'',-1,1),
        (7777777777777,'',-1,1),
        (8888888888888,'',-1,1)                 
),
AND tracked_item in (
    'ASIN1', 
    'ASIN2', 
    'ASIN3', 
    'ASIN4', 
    'ASIN5', 
    'ASIN6', 
    'ASIN7', 
    'ASIN8', 
    'ASIN9', 
    'ASIN10', 
    'ASIN11')
```

##### Query

Update the following query on lines where `AND tracked_item in (‘ASIN1’, ‘ASIN2’, ... ‘ASIN_N’)`

<details class="details-bar">
<summary>Click to see query:  frequency stage  </summary>

```
-- Frequency: AMC Tentpole Deep Dive Module
-- UPDATE: Define campaign list in a comma-separated format with either 
-- dsp_campaign_id populated (with an empty string for sa_campaign_name) -- or, sa_campaign_name populated (with a -1 for dsp_campaign_id). 
-- For automatic use of AMC cost data, populate the ecpm column with -1; -- alternatively,provide an ecpm figure for the campaign in question. 
-- Populate 1 in the include_brand_halo column if you want to consider -- both promoted and brand halo ASIN conversions; alternatively, populate -- 0 if you want to consider promoted ASIN conversions only.
WITH CAMPAIGN_SCOPE (
    dsp_campaign_id,
    sa_campaign_name,
    ecpm,
    include_brand_halo
) AS (
    VALUES
        (-1, 'SponsoredAdsCampaign1', -1, 1),
        (-1, 'SponsoredAdsCampaign2', -1, 1),
        (-1, 'SponsoredAdsCampaign3', -1, 1),
        (-1, 'SponsoredAdsCampaign4', -1, 1),
        (-1, 'SponsoredAdsCampaign5', -1, 1),
        (-1, 'SponsoredAdsCampaign6', -1, 1),
        (-1, 'SponsoredAdsCampaign7', -1, 1),
        (-1, 'SponsoredAdsCampaign8', -1, 1),
        (1111111111111, '', -1, 1),
        (2222222222222, '', -1, 1),
        (3333333333333, '', -1, 1),
        (4444444444444, '', -1, 1),
        (5555555555555, '', -1, 1),
        (6666666666666, '', -1, 1),
        (7777777777777, '', -1, 1),
        (8888888888888, '', -1, 1)
),

CONVERSIONS_PREP AS (
    -- Gather Conversions data for DSP campaigns without brand halo
    SELECT
        user_id,
        conversion_event_dt,
        SUM(product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT dsp_campaign_id 
            FROM CAMPAIGN_SCOPE
            WHERE dsp_campaign_id != -1
                AND CAST(include_brand_halo AS TINYINT) = 0
        ) S ON C.campaign_id = S.dsp_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1', 'ASIN2', 'ASIN3', 'ASIN4', 'ASIN5',
            'ASIN6', 'ASIN7', 'ASIN8', 'ASIN9', 'ASIN10', 'ASIN11'
        )
    GROUP BY 1, 2

    UNION ALL

    -- Gather Conversions data for DSP campaigns with brand halo
    SELECT
        user_id,
        conversion_event_dt,
        SUM(total_product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT dsp_campaign_id
            FROM CAMPAIGN_SCOPE
            WHERE dsp_campaign_id != -1
                AND CAST(include_brand_halo AS TINYINT) = 1
        ) S ON C.campaign_id = S.dsp_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1', 'ASIN2', 'ASIN3', 'ASIN4', 'ASIN5',
            'ASIN6', 'ASIN7', 'ASIN8', 'ASIN9', 'ASIN10', 'ASIN11'
        )
    GROUP BY 1, 2

    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns without brand halo
    SELECT
        user_id,
        conversion_event_dt,
        SUM(product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
                AND CAST(include_brand_halo AS TINYINT) = 0
        ) S ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1', 'ASIN2', 'ASIN3', 'ASIN4', 'ASIN5',
            'ASIN6', 'ASIN7', 'ASIN8', 'ASIN9', 'ASIN10', 'ASIN11'
        )
    GROUP BY 1, 2

    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns with brand halo
    SELECT
        user_id,
        conversion_event_dt,
        SUM(total_product_sales) AS sales
    FROM
        AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
        INNER JOIN (
            SELECT sa_campaign_name
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
                AND CAST(include_brand_halo AS TINYINT) = 1
        ) S ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item IN (
            'ASIN1', 'ASIN2', 'ASIN3', 'ASIN4', 'ASIN5',
            'ASIN6', 'ASIN7', 'ASIN8', 'ASIN9', 'ASIN10', 'ASIN11'
        )
    GROUP BY 1, 2
),

CONVERSIONS AS (
    SELECT
        user_id,
        MAX(conversion_event_dt) AS last_conversion_dt,
        SUM(sales) AS sales
    FROM
        CONVERSIONS_PREP
    GROUP BY 1
),

IMPRESSIONS_PREP AS (
    -- Gather Impressions data for DSP campaigns
    SELECT
        user_id,
        impression_dt,
        SUM(impressions) AS impressions,
        SUM(IF(
            ecpm >= 0,
            ecpm / 1000 * impressions,
            total_cost / 100000
        )) AS cost
    FROM
        DSP_IMPRESSIONS I
        INNER JOIN (
            SELECT 
                dsp_campaign_id,
                CAST(ecpm AS INT) AS ecpm
            FROM CAMPAIGN_SCOPE
            WHERE dsp_campaign_id != -1
        ) S ON I.campaign_id = S.dsp_campaign_id
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2

    UNION ALL

    -- Gather Impressions data for Sponsored Ads campaigns
    SELECT
        user_id,
        event_dt AS impression_dt,
        SUM(impressions) AS impressions,
        SUM(IF(
            ecpm >= 0,
            ecpm / 1000 * impressions,
            spend / 100000000
        )) AS cost
    FROM
        SPONSORED_ADS_TRAFFIC I
        INNER JOIN (
            SELECT 
                sa_campaign_name,
                CAST(ecpm AS INT) AS ecpm
            FROM CAMPAIGN_SCOPE
            WHERE sa_campaign_name != ''
        ) S ON I.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2
),

IMPRESSIONS AS (
    SELECT
        user_id,
        impression_dt,
        SUM(impressions) AS impressions,
        SUM(cost) AS ad_spend
    FROM
        IMPRESSIONS_PREP
    GROUP BY 1, 2
),

COMBINED_PREP AS (
    SELECT
        I.user_id AS imp_user_id,
        P.user_id AS pur_user_id,
        I.impression_dt,
        P.last_conversion_dt,
        I.impressions,
        P.sales,
        I.ad_spend
    FROM
        CONVERSIONS P
        FULL OUTER JOIN IMPRESSIONS I ON I.user_id = P.user_id
),

COMBINED AS (
    SELECT
        imp_user_id,
        pur_user_id,
        SUM(impressions) AS impressions,
        MAX(sales) AS sales,
        SUM(ad_spend) AS ad_spend
    FROM
        COMBINED_PREP
    GROUP BY 1, 2
)

SELECT
    IF(
        impressions < 31,
        CONCAT('frequency_', impressions),
        'frequency_31+'
    ) AS frequency_bucket,
    COUNT(DISTINCT imp_user_id) AS unique_reach,
    COUNT(DISTINCT pur_user_id) AS unique_purchasers,
    SUM(impressions) AS impressions,
    SUM(sales) AS sales,
    SUM(ad_spend) AS ad_spend,
    SUM(sales) / SUM(ad_spend) AS roas
FROM
    COMBINED
GROUP BY 1
```

</details>

##### Results table

There will be 3 results tables returned from the 3 queries that were run. The resulting tables should look like the one below. They can then be used to create a visual to see the results.

| frequency\_bucket | unique\_reach | unique\_purchasers | impressions | sales     | ad\_spend | roas      |
| ----------------- | ------------- | ------------------ | ----------- | --------- | --------- | --------- |
| frequency\_1      | 547432        | 92                 | 547432      | 3360\.35  | 321\.2    | 10\.46186 |
| frequency\_2      | 404328        | 199                | 808656      | 7641\.4   | 997\.58   | 7\.65994  |
| frequency\_3      | 179868        | 253                | 539604      | 9391\.64  | 1116\.92  | 8\.40852  |
| frequency\_4      | 149442        | 356                | 597768      | 13313\.69 | 1347\.55  | 9\.87992  |
| frequency\_5      | 98769         | 308                | 493845      | 11902\.61 | 1294\.27  | 9\.19639  |
| frequency\_6      | 85983         | 362                | 515898      | 12927\.74 | 1389\.61  | 9\.30314  |
| frequency\_7      | 65987         | 334                | 461909      | 13083\.58 | 1279\.35  | 10\.22674 |
| frequency\_8      | 57625         | 364                | 461000      | 13936\.87 | 1263\.73  | 11\.02836 |
| frequency\_9      | 47824         | 324                | 430416      | 11990\.94 | 1166\.47  | 10\.27968 |
| frequency\_10     | 42548         | 331                | 425480      | 12840\.51 | 1189\.84  | 10\.7918  |
| frequency\_11     | 36063         | 314                | 396693      | 12032\.29 | 1115\.15  | 10\.78984 |
| frequency\_12     | 32448         | 288                | 389376      | 11685\.7  | 1106\.55  | 10\.56048 |

##### Visual

This visual is representative and there should be 3 visuals total for each stage: Lead-in, Promotional Event, Lead-out.

The following 3 visuals are illustrative in nature. Number of touchpoints is highly dependent on campaign budget, and cumulative reach goals.

#### Lead In - Cumulative Reach & ROAS by DSP Frequency

![PE_Fig3](/_images/amazon-marketing-cloud/playbooks/pe/pe_3.png)

Interpretation: 5 ads, 80% coverage, would provide the most coverage with a positive ROAS.

#### Promotional Event - Cumulative Reach & ROAS by DSP Frequency

![PE_Fig4](/_images/amazon-marketing-cloud/playbooks/pe/pe_4.png)

Due to the granular nature of this data, there might be outliers skewing 7 touchpoints. Here we have a ROAS of 2.42 and 91% cumulative reach with 4 touchpoints.

#### Lead Out - Cumulative Reach & ROAS by DSP Frequency

![PE_Fig5](/_images/amazon-marketing-cloud/playbooks/pe/pe_5.png)

Interpretation, at 88% cumulative reach and 1.69 ROAS, 4 touchpoints per day would be a good choice as there is a steep drop in ROAS between 4 and 5 touchpoints.

### Ad Format Analysis (DSP vs. Sponsored Ads)

When working within the Amazon ecosystem, ad placement has the potential to play a huge role in converting potential customers to your clients. The section below outlines how different marketing strategies ultimately impact a customer’s likelihood of making a purchase.

###### Instructions

The following process will be based on the lead-in, event, and lead out dates. The query and corresponding interpretation should be done for all 3 phases to determine the optimal targeting amount for your chosen audience.

Follow the instructions laid out in section 4.1 to adjust dates accordingly for the three phases.

###### Parameters - campaigns**

```
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id, 
    sa_campaign_name, 
    ecpm, 
    include_brand_halo) AS (
    VALUES
        (-1,'SponsoredAdsCampaign1',-1,1),
        (-1,'SponsoredAdsCampaign2',-1,1),
        (-1,'SponsoredAdsCampaign3',-1,1),
        (-1,'SponsoredAdsCampaign4',-1,1),
        (-1,'SponsoredAdsCampaign5',-1,1),
        (-1,'SponsoredAdsCampaign6',-1,1),
        (-1,'SponsoredAdsCampaign7',-1,1),
        (-1,'SponsoredAdsCampaign8',-1,1),
        (1111111111111,'',-1,1),
        (2222222222222,'',-1,1),
        (3333333333333,'',-1,1),
        (4444444444444,'',-1,1),
        (5555555555555,'',-1,1),
        (6666666666666,'',-1,1),
        (7777777777777,'',-1,1),
        (8888888888888,'',-1,1)                 
),
AND tracked_item in (
    'ASIN1', 
    'ASIN2', 
    'ASIN3', 
    'ASIN4', 
    'ASIN5', 
    'ASIN6', 
    'ASIN7', 
    'ASIN8', 
    'ASIN9', 
    'ASIN10', 
    'ASIN11')

```

###### Query - ad-product two way endemic

<details class="details-bar">
<summary>Click to see query:  Ad-product two way endemic  </summary>

```
-- Ad Format (Two-Way) AMC Tentpole Deep Dive Module: Ad Format (Two-Way)
-- UPDATE: Define campaign list in a comma-separated format with either
-- DISPLAY_campaign_id populated (with an empty string for sa_campaign_name)
-- or sa_campaign_name populated (with a -1 for DISPLAY_campaign_id). For
-- automatic use of AMC cost data, populate the ecpm column with -1;
-- alternatively, provide an ecpm figure for the campaign in question.
-- Populate 1 in the include_brand_halo column if you want to consider
-- both promoted and brand halo ASIN conversions; alternatively, populate
-- 0 if you want to consider promoted ASIN conversions only.
WITH CAMPAIGN_SCOPE (
    DISPLAY_campaign_id,
    sa_campaign_name,
    ecpm,
    include_brand_halo
) AS (
VALUES
    (-1,'SponsoredAdsCampaign1',-1,1),
    (-1,'SponsoredAdsCampaign2',-1,1),
    (-1,'SponsoredAdsCampaign3',-1,1),
    (-1,'SponsoredAdsCampaign4',-1,1),
    (-1,'SponsoredAdsCampaign5',-1,1),
    (-1,'SponsoredAdsCampaign6',-1,1),
    (-1,'SponsoredAdsCampaign7',-1,1),
    (-1,'SponsoredAdsCampaign8',-1,1),
    (1111111111111,'',-1,1),
    (2222222222222,'',-1,1),
    (3333333333333,'',-1,1),
    (4444444444444,'',-1,1),
    (5555555555555,'',-1,1),
    (6666666666666,'',-1,1),
    (7777777777777,'',-1,1),
    (8888888888888,'',-1,1)
),

-- Gather Conversions data for DISPLAY and Sponsored Ads separately
CONVERSIONS_PREP AS (
    -- Gather Conversions data for DISPLAY campaigns without brand halo
    SELECT
        user_id
        , conversion_event_dt
        , SUM(product_sales) AS sales
    FROM
    AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
    INNER JOIN (
        SELECT DISPLAY_campaign_id
        FROM CAMPAIGN_SCOPE
        WHERE DISPLAY_campaign_id != -1
        AND CAST(include_brand_halo AS TINYINT) = 0) S
    ON C.campaign_id = S.DISPLAY_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item in (
        'ASIN1',
        'ASIN2',
        'ASIN3',
        'ASIN4',
        'ASIN5',
        'ASIN6',
        'ASIN7',
        'ASIN8',
        'ASIN9',
        'ASIN10',
        'ASIN11')
    GROUP BY 1, 2
  
    UNION ALL
    -- Gather Conversions data for DISPLAY campaigns with brand halo
    SELECT
        user_id
        , conversion_event_dt
        , SUM(total_product_sales) AS sales
    FROM
    AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
    INNER JOIN (
        SELECT DISPLAY_campaign_id
        FROM CAMPAIGN_SCOPE
        WHERE DISPLAY_campaign_id != -1
        AND CAST(include_brand_halo AS TINYINT) = 1) S
        ON C.campaign_id = S.DISPLAY_campaign_id
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item in (
        'ASIN1',
        'ASIN2',
        'ASIN3',
        'ASIN4',
        'ASIN5',
        'ASIN6',
        'ASIN7',
        'ASIN8',
        'ASIN9',
        'ASIN10',
        'ASIN11')
    GROUP BY 1, 2
  
    UNION ALL

    -- Gather Conversions data for Sponsored Ads campaigns without brand halo
    SELECT
        user_id
        , conversion_event_dt
        , SUM(product_sales) AS sales
    FROM
    AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
    INNER JOIN (
        SELECT sa_campaign_name
        FROM CAMPAIGN_SCOPE
        WHERE sa_campaign_name != ''
        AND CAST(include_brand_halo AS TINYINT) = 0) S
        ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item in (
        'ASIN1',
        'ASIN2',
        'ASIN3',
        'ASIN4',
        'ASIN5',
        'ASIN6',
        'ASIN7',
        'ASIN8',
        'ASIN9',
        'ASIN10',
        'ASIN11')
    GROUP BY 1, 2
  
    UNION ALL
    -- Gather Conversions data for Sponsored Ads campaigns with brand halo
    SELECT
        user_id
        , conversion_event_dt
        , SUM(total_product_sales) AS sales
    FROM
    AMAZON_ATTRIBUTED_EVENTS_BY_TRAFFIC_TIME C
    INNER JOIN (
        SELECT sa_campaign_name
        FROM CAMPAIGN_SCOPE
        WHERE sa_campaign_name != ''
        AND CAST(include_brand_halo AS TINYINT) = 1) S
        ON C.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
        AND total_product_sales > 0
        AND tracked_item in (
        'ASIN1',
        'ASIN2',
        'ASIN3',
        'ASIN4',
        'ASIN5',
        'ASIN6',
        'ASIN7',
        'ASIN8',
        'ASIN9',
        'ASIN10',
        'ASIN11')
    GROUP BY 1, 2
    ),
    CONVERSIONS AS (
    SELECT
        user_id
        , MAX(conversion_event_dt) AS last_conversion_dt
        , SUM(sales) AS sales
    FROM
    CONVERSIONS_PREP
    GROUP BY 1
),
-- Gather Impressions data for DISPLAY and Sponsored Ads separately
IMPRESSIONS_PREP AS (
    -- Gather Impressions data for DISPLAY campaigns
    -- Note: DISPLAY campaign cost is reported in millicents on AMC.
    -- Divide the cost value by 100,000 to convert it into local currency.
    SELECT
        user_id
        , impression_dt
        , 'DISPLAY' AS imp_ad_type
        , SUM(impressions) AS impressions
        , SUM(IF(
        ecpm >= 0,
        ecpm / 1000 * impressions,
        total_cost / 100000)) AS cost
    FROM
    DISPLAY_IMPRESSIONS I
    INNER JOIN (
        SELECT DISPLAY_campaign_id,
        CAST(ecpm AS INT) AS ecpm
        FROM CAMPAIGN_SCOPE
        WHERE DISPLAY_campaign_id != -1) S
        ON I.campaign_id = S.DISPLAY_campaign_id
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2, 3

    UNION ALL

    -- Gather Impressions data for Sponsored Ads campaigns
    -- Note: Sponsored Ads campaign cost is reported in microcents on AMC.
    -- Divide the cost value by 100,000,000 to convert it into local currency.
    SELECT
        user_id
        , event_dt AS impression_dt
        , 'SPONSORED ADS' AS imp_ad_type
        , SUM(impressions) AS impressions
        , SUM(IF(
        ecpm >= 0,
        ecpm/ 1000 * impressions,
        spend / 100000000)) AS cost
    FROM
    SPONSORED_ADS_TRAFFIC I
    INNER JOIN (
        SELECT sa_campaign_name,
        CAST(ecpm AS INT) AS ecpm
        FROM CAMPAIGN_SCOPE
        WHERE sa_campaign_name != '') S
        ON I.campaign = S.sa_campaign_name
    WHERE
        user_id IS NOT NULL
    GROUP BY 1, 2, 3
),

IMPRESSIONS AS (
    SELECT
        user_id
        , imp_ad_type
        , MIN(impression_dt) AS first_impression_dt
        , SUM(impressions) AS impressions
        , SUM(cost) AS ad_spend
    FROM
    IMPRESSIONS_PREP
    GROUP BY 1, 2
),

COMBINED_PREP AS (
    SELECT
        COALESCE(P.user_id, I.user_id) AS user_id
        , I.imp_ad_type
        , I.impressions
        , P.sales
        , I.ad_spend
        , CASE
            WHEN last_conversion_dt IS NULL THEN 'NO PURCHASE'
            WHEN last_conversion_dt >= first_impression_dt THEN 'PRE-PURCHASE'
            WHEN last_conversion_dt < first_impression_dt THEN 'POST-PURCHASE'
            ELSE 'OTHER'
        END AS impression_timing
    FROM
    CONVERSIONS P
    FULL OUTER JOIN IMPRESSIONS I
    ON I.user_id = P.user_id
),

COMBINED AS (
    SELECT
        user_id
        , SUM(
            IF(impression_timing = 'NO PURCHASE' AND imp_ad_type = 'DISPLAY',
            impressions, 0)) AS no_purch_DISPLAY_impressions
        , SUM(
            IF(impression_timing = 'PRE-PURCHASE' AND imp_ad_type = 'DISPLAY', 
            impressions,0)) AS pre_purch_DISPLAY_impressions
        , SUM(
            IF(impression_timing = 'POST-PURCHASE' AND imp_ad_type = 'DISPLAY',
            impressions, 0)) AS post_purch_DISPLAY_impressions
        , SUM(
            IF(impression_timing = 'OTHER' AND imp_ad_type = 'DISPLAY',
            impressions, 0)) AS other_DISPLAY_impressions
        , SUM(
            IF(impression_timing = 'NO PURCHASE' AND imp_ad_type = 'SPONSORED ADS',
        impressions, 0)) AS no_purch_sa_impressions
        , SUM(
            IF(impression_timing = 'PRE-PURCHASE' AND imp_ad_type = 'SPONSORED ADS',
        impressions, 0)) AS pre_purch_sa_impressions
        , SUM(
            IF(impression_timing = 'POST-PURCHASE' AND imp_ad_type = 'SPONSORED ADS',
        impressions, 0)) AS post_purch_sa_impressions
        , SUM(
            IF(impression_timing = 'OTHER' AND imp_ad_type = 'SPONSORED ADS',
            impressions, 0)) AS other_sa_impressions
        , SUM(impressions) AS impressions
        , MAX(sales) AS sales
        , SUM(ad_spend) AS ad_spend
    FROM
    COMBINED_PREP
    GROUP BY 1
    HAVING
        SUM(impressions) > 0
        OR SUM(ad_spend) > 0
),

USERS_PURCHASED AS (
    SELECT
        user_id
        , no_purch_DISPLAY_impressions
        , pre_purch_DISPLAY_impressions
        , post_purch_DISPLAY_impressions
        , other_DISPLAY_impressions
        , no_purch_sa_impressions
        , pre_purch_sa_impressions
        , post_purch_sa_impressions
        , other_sa_impressions
    FROM
    COMBINED
    WHERE
        sales > 0
),

USERS_PURCHASED_EXP AS (
    SELECT
        IF(
        (pre_purch_DISPLAY_impressions > 0
        OR no_purch_DISPLAY_impressions > 0)
        AND (pre_purch_sa_impressions > 0
        OR no_purch_sa_impressions > 0)
        , 'BOTH'
        , IF(
        (pre_purch_DISPLAY_impressions > 0
        OR no_purch_DISPLAY_impressions > 0)
        AND (pre_purch_sa_impressions = 0
        AND no_purch_sa_impressions = 0)
        , 'DISPLAY ONLY'
        , IF(
        (pre_purch_DISPLAY_impressions = 0
        AND no_purch_DISPLAY_impressions = 0)
        AND (pre_purch_sa_impressions > 0
        OR no_purch_sa_impressions > 0)
        , 'SPONSORED ADS ONLY'
        , IF(
        (pre_purch_DISPLAY_impressions = 0
        AND no_purch_DISPLAY_impressions = 0
        AND pre_purch_sa_impressions = 0
        AND no_purch_sa_impressions = 0)
        , 'NONE'
        , 'NOT APPLICABLE'
        )
        )
        )
        ) AS exposure_group
        , COUNT(DISTINCT user_id) AS unique_purchasers
    FROM
    USERS_PURCHASED
    GROUP BY 1
),

ALL_USERS_EXP AS (
    SELECT
        IF(
        (pre_purch_DISPLAY_impressions > 0
        OR no_purch_DISPLAY_impressions > 0)
        AND (pre_purch_sa_impressions > 0
        OR no_purch_sa_impressions > 0)
        , 'BOTH'
        , IF(
        (pre_purch_DISPLAY_impressions > 0
        OR no_purch_DISPLAY_impressions > 0)
        AND (pre_purch_sa_impressions = 0
        AND no_purch_sa_impressions = 0)
        , 'DISPLAY ONLY'
        , IF(
        (pre_purch_DISPLAY_impressions = 0
        AND no_purch_DISPLAY_impressions = 0)
        AND (pre_purch_sa_impressions > 0
        OR no_purch_sa_impressions > 0)
        , 'SPONSORED ADS ONLY'
        , IF(
        (pre_purch_DISPLAY_impressions = 0
        AND no_purch_DISPLAY_impressions = 0
        AND pre_purch_sa_impressions = 0
        AND no_purch_sa_impressions = 0)
        , 'NONE'
        , 'NOT APPLICABLE'
        )
        )
        )
        ) AS exposure_group
        , COUNT(DISTINCT user_id) AS unique_reach
        , SUM(impressions) AS impressions
        , SUM(sales) AS sales
        , SUM(ad_spend) AS ad_spend
    FROM
    COMBINED
    GROUP BY 1
)
SELECT
    A.exposure_group
    , A.unique_reach
    , P.unique_purchasers
    , A.impressions
    , A.sales
    , A.ad_spend
    , A.sales / A.ad_spend AS roas
FROM
    ALL_USERS_EXP A
LEFT JOIN USERS_PURCHASED_EXP P
    ON A.exposure_group = P.exposure_group
```

</details>

###### Results table

This table is an output of the query above and was run for the days of the promotional event. For a full picture run this query for lead-in, lead-out and promotional event dates, as shown above in the “instructions” section.

| exposure\_group    | unique\_reach | unique\_purchasers | impressions | sales      | ad\_spend   | roas     |
| ------------------ | ------------- | ------------------ | ----------- | ---------- | ----------- | -------- |
| NONE               | 1720          | 9                  | 56          | 114\.58    | 3339\.46    | 0\.03431 |
| SPONSORED ADS ONLY | 6215456       | 8518               | 26341261    | 140862\.18 | 128056\.527 | 1\.1     |
| DSP                | 6588383       | 8688               | 35033877    | 106348     | 115596      | 0\.92    |
| Both               | 11068484      | 15378              | 57105220    | 173498\.23 | 62634\.7401 | 2\.77    |

The three tables can then be combined to create a chart seen below. This is optional and just provides a visualization of the numbers.

![PE_Fig6](/_images/amazon-marketing-cloud/playbooks/pe/pe_6.png)

###### Interpretation

Based on the ROAS we see during out event, while DSP impressions alone do not have a high ROAS, we see that when combined with sponsored ads there is a much higher roas. Thus for the promotional event phase we would focus on both DSP & Sponsored Ads to see an optimal return on our investment.

##### Keyword analysis

Similar to ad types and placements, keywords play an important role on your promotional event strategy. Below we will determine which keywords were most impactful by promotional event phase (lead in, event period, lead out). This is designed to be directional in nature and to guide which keywords to bid on during each phase.

The keywords chosen here are specific to the seed and will help identify which keywords were most effective for your seed audience. Since AMC lookalike audiences are similar to the seed, the derived keywords should also be effective with your new lookalike audience.

###### Instructions

The following process will be based on the lead-in, event, and lead out dates. The query and corresponding interpretation should be done for all 3 phases to determine the optimal targeting amount for your chosen audience.

Follow the instructions laid out in section 4.1 to adjust dates accordingly for the three phases.

###### Parameters - campaigns (DSP/SA)

```
WITH CAMPAIGN_SCOPE (
    dsp_campaign_id, 
    sa_campaign_name, 
    ecpm, 
    include_brand_halo) AS (
    VALUES
        (-1,'SponsoredAdsCampaign1',-1,1),
        (-1,'SponsoredAdsCampaign2',-1,1),
        (-1,'SponsoredAdsCampaign3',-1,1),
        (-1,'SponsoredAdsCampaign4',-1,1),
        (-1,'SponsoredAdsCampaign5',-1,1),
        (-1,'SponsoredAdsCampaign6',-1,1),
        (-1,'SponsoredAdsCampaign7',-1,1),
        (-1,'SponsoredAdsCampaign8',-1,1)
),
```

###### Query: ad-product two way endemic

<details class="details-bar">
<summary>Click to see query:  Ad-product two way endemic  </summary>

```
-- Sponsored Ads Keywords: AMC Tentpole Deep Dive Module
-- UPDATE: Define campaign list in a comma-separated format with either
-- dsp_campaign_id populated (with an empty string for sa_campaign_name) or
-- sa_campaign_name populated (with a -1 for dsp_campaign_id). For automatic use
-- of AMC cost data, populate the ecpm column with -1; alternatively, provide
-- an ecpm figure for the campaign in question. Populate 1 in the include_brand_halo
-- column if you want to consider both promoted and brand halo ASIN conversions;
-- alternatively, populate 0 if you want to consider promoted ASIN conversions only.
WITH CAMPAIGN_SCOPE (
    dsp_campaign_id,
    sa_campaign_name,
    ecpm,
    include_brand_halo
) AS (
    VALUES
        (-1, 'SponsoredAdsCampaign1', -1, 1),
        (-1, 'SponsoredAdsCampaign2', -1, 1),
        (-1, 'SponsoredAdsCampaign3', -1, 1),
        (-1, 'SponsoredAdsCampaign4', -1, 1),
        (-1, 'SponsoredAdsCampaign5', -1, 1),
        (-1, 'SponsoredAdsCampaign6', -1, 1),
        (-1, 'SponsoredAdsCampaign7', -1, 1),
        (-1, 'SponsoredAdsCampaign8', -1, 1)
)

SELECT
    I.customer_search_term,
    COUNT(DISTINCT I.user_id) AS unique_reach,
    SUM(impressions) AS impressions,
    SUM(clicks) AS clicks
FROM
    SPONSORED_ADS_TRAFFIC I
    INNER JOIN (
        SELECT sa_campaign_name
        FROM CAMPAIGN_SCOPE
        WHERE sa_campaign_name != ''
    ) S ON I.campaign = S.sa_campaign_name
GROUP BY 1
```

</details>

###### Results table

| customer\_search\_term | unique\_reach | impressions | clicks | click\_through\_rate |
| ---------------------- | ------------- | ----------- | ------ | -------------------- |
| keyword1               | 294332        | 542287      | 282    | 0\.000520            |
| keyword2               | 289488        | 963182      | 18461  | 0\.019167            |
| keyword3               | 275896        | 501267      | 1122   | 0\.002238            |
| keyword4               | 163614        | 260707      | 222    | 0\.000852            |
| keyword5               | 140682        | 203074      | 138    | 0\.000680            |
| keyword6               | 114658        | 206002      | 620    | 0\.003010            |
| keyword7               | 109404        | 358602      | 8523   | 0\.023767            |
| keyword8               | 105720        | 215435      | 494    | 0\.002293            |
| keyword9               | 105046        | 320369      | 3314   | 0\.010344            |
| keyword10              | 99937         | 145612      | 73     | 0\.000501            |
| keyword11              | 72579         | 108656      | 69     | 0\.000635            |
| keyword12              | 66384         | 105827      | 363    | 0\.003430            |

###### Visual

![PE_ad_product](/_images/amazon-marketing-cloud/playbooks/pe/pe_7.png)

###### Interpretation

We see that there are a number of categories with a low unique reach but high CTR. Specifically keywords 2, 7, and 9. These keywords would probably be good targets for the upcoming promotional event during the “lead-up” phase.

This analysis should be done for all 3 phases so you know which keywords to bid on throughout the promotional event. The visual below highlights how keywords might change from phase to phase and how the keywords for the lead-in might differ from the keywords for the promotional event.

![PE_ad_product](/_images/amazon-marketing-cloud/playbooks/pe/pe_8.png)
