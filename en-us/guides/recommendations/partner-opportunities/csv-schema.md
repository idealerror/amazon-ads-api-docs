---
title: Partner opportunities CSV data schema
description: An explanation of the schema for CSV files retrieved via Partner Opportunities API.
type: guide
interface: api
tags:
    - Partner opportunities
    - Campaign management
    - Reporting
---

# Partner opportunities CSV schema

The table below details the data fields for CSV files retrieved via the [file resource](partner-opportunities#tag/Partner-Opportunities/operation/partnerOpportunitiesGetOpportunityFile) in the partner opportunities API.

## Data fields

|Name	|Definition	|Example	|
|---	|---	|---	|
|adId	|Unique identifier for the ad.	|A04135221GSW52LIGFIB6	|
|adProductType	|Amazon Ad product including Sponsored Products, Sponsored Brands, Sponsored Display.	|SP, SponsoredProduct	|
|advertiserName	|The registered name of the advertiser	|TEST ADVERTISER	|
|advertiserType	|The type of advertiser account (i.e. Seller, Vendor)	|SELLER, VENDOR, SELLER\_CENTRAL\_ENTITY, BRAND	|
|applyApiEndPoint	|PO API endpoint which could be used to apply the recommendation with one API call.	|https://advertising-api.amazon.com/partnerOpportunities/{partnerOpportunityId}/apply	|
|asin	|Amazon Standard Identification Number	|B00NQ2JLXX	|
|averageTimeInBudget	|Average time campaign is within set budget (percentage) 	|0.8, 99.69	|
|brand	|ASIN's Brand	|TEST Brand	|
|creditDistributionSla	|x number of days after completing the qualifying action	|14	|
|clicks	|Ad Clicks by customer	|29	|
|clickThroughRateRatioToAvg	|4-week clickthrough rate for similar products / 4-week conversion rate for your product	|1.42983	|
|conversions	|Number of sales from a given ad	|3	|
|conversionRate	|Number of conversions / Number of clicks	|0.285714286	|
|conversionRateRatioToAvg	|4-week conversion rate for similar products / 4-week conversion rate for your product	|1.768709	|
|count	|inventory units	|37	|
|currentCampaignSettings	|Object (JSON-encoded string) containing the settings currently used for your campaign	|{"ad\_groups":[{"adgroup\_id":"530905550061343","target\_type":"KEYWORD","targets":[{"bid":0.42,"text":"test","match\_type":"EXACT","target\_id":"459582665159323","targeting":"test"}]}]}	|
|dealEndDate	|End date of product's Deal	|2/12/25	|
|dealStartDate	|Beginning date of product's Deal	|2/10/25	|
|encryptedAdvertiserId	|Advertiser ID	|A236EC8B73O62X	|
|encryptedCampaignId	|Campaign ID	|518770093932188, A09873083AWAMDOEN678X	|
|encryptedAdGroupId	|Ad Group ID	|A0760865T3OJ0N2GNMGM	|
|endDate	|Campaign End Date 	|1737417600000000	|
|entityId	|Unique identifier for vendor advertisers	|ENTITY1N2EM112O48MJ	|
|expiryDate	|Date after which this recommendation would expire	|1738900043000000	|
|explanation	|Object (JSON-encoded string) containing the reasons this particular Opportunity instance was provided	|{"description":"Campaigns with high ROAS and high budget use, compared to similar campaigns. Apply these recommendations to optimize sales and keep your campaigns in budget.","missed\_opportunities":{"impressions":{"upper\_bound":"120720","lower\_bound":"40225"},"clicks":{"upper\_bound":"575","lower\_bound":"182"},"conversions":{"upper\_bound":"2,230.00","lower\_bound":"730.00"},"time\_period":"Last 7 days"},"recommendation\_context":{"diagnostic\_context":{"benchmark\_context":{"roas":{"benchmark\_value":4.58,"period":7},"impressions":{"benchmark\_value":12815.0,"period":7},"ad\_spend":{"benchmark\_value":32.41,"period":7},"click\_through\_rate":{"benchmark\_value":0.78,"period":7},"budget\_utilization":{"benchmark\_value":72.57,"period":7}},"diagnostic\_date":"2025-01-01T00:00:00.000Z"},"grouping\_context":{"grouping\_type":"INCREASE\_BUDGET\_CONTEXTUAL"}}}	|
|impressionTrendSimilarProducts	|The trend of impressions of similar asins: impressions of similar ASINs in the past 4 weeks divided by unit sales between 4 weeks and 8 weeks ago	|0.992005	|
|impressionTrendYourProduct	|The trend of impressions for the 'asin': impressions of this ASIN in the past 4 weeks divided by unit sales between 4 weeks and 8 weeks ago	|0.630435	|
|marketplace	|Represents the marketplace where Amazon is doing business (i.e. US, UK, etc.)	|US	|
|missedOpportunities\_clicks\_lowerBound	|Lower bound of the estimated additional clicks, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual clicks.	|15	|
|missedOpportunities\_clicks\_upperBound	|Upper bound of the estimated additional clicks, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual clicks.	|45	|
|missedOpportunities\_impressions\_lowerBound	|Lower bound of the estimated additional impressions, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions.	|2198	|
|missedOpportunities\_impressions\_upperBound	|Upper bound of the estimated additional impressions, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions.	|6594	|
|missedOpportunities\_sales\_lowerBound	|Lower bound of the estimated additional sales, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual sales.	|8.08, 371	|
|missedOpportunities\_sales\_upperBound	|Upper bound of the estimated additional sales, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual sales.	|24.24, 1113	|
|missedOpportunities\_viewableImpressions\_lowerBound	|Lower bound of the estimated number of times the ad was considered viewable, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions.	|760	|
|missedOpportunities\_viewableImpressions\_upperBound	|Upper bound of the estimated number of times the ad was considered viewable, the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions.	|2280	|
|productName	|Name of Product	|TEST Men's Dress Shirt, Red, 17"-17.5" Neck 34"-35" Sleeve	|
|profileId	|Profiles in the Amazon Ads API represent an advertiser's account in a specific marketplace. Profiles play a crucial role in the Amazon Ads API by determining the management scope for a given call.	|3228655307457599	|
|promotionAmountCurrency	|currency code the offer will be awarded in, like "USD"	|EUR	|
|promotionAmountLc	|amount that eligible advertiser will earn	|250	|
|promotionEndDate	|date the promotion will end and eligible advertisers cannot have a promotion action after this date to earn click credit offer	|1736899200000000	|
|promotionId	|Unique promotion identifier	|V1252008460	|
|promotionStartDate	|date the promotion will start and eligible advertisers must have a promotion action no earlier than this date to earn click credit offer	|1734307200000000	|
|promotionType	|Type of promotion (i.e. ACQ, REACT, ONB, etc.)	|REACT	|
|publishedDate	|Date when the recommendation was published	|1737826802, 1970-01-01T00:28:57.668088Z	|
|purchasesTrendYourProduct	|The trend of purchases of the 'asin': unit sales of this ASIN in the past 4 weeks divided by unit sales between 4 weeks and 8 weeks ago.	|0.428571|
|purchasesTrendSimilarProducts	|The trend of purchases of similar asins: unit sales of similar ASINs in the past 4 weeks divided by unit sales between 4 weeks and 8 weeks ago.	|0.525625	|
|recommendedBudget	|Recommended campaign budget setting 	|350, 483.37 |
|recommendationGroupId	|Unique identifier for the recommendation group	|f861ebe7-8e1a-43e5-a9ca-edab8dc9f082	|
|recommendationId	|Unique identifier for the recommendation	|3cee8afe-bc21-4956-a46b-2bbd9b1556d4	|
|recommendationType	|Type of recommendation	|CAMPAIGN_BUDGET	|
|recommendedCampaignSettings	|Object (JSON-encoded string) containing the recommended settings for your campaign.	|{"a\_groups":[{"adgrou\_id":"530905550061343","targe\_type":"KEYWORD","targets":[{"bid":0.82,"text":"test","matc\_type":"EXACT","targe\_id":"459582665159323","targeting":"test"}]}]}	|
|returnOnAdSpend	|Also called ROAS; Sales / Ad Spend	|13.40207685	|
|similarProducts	|An array of similar ASINs to the 'asin' field	|[B0CWVD1FQS, B08XXFB9AT, B08XXG59V7]	|
|startDate	|Campaign Start Date 	|1736899200000000	|
|target	|The identifier referred to by this recommendation (if applicable). Type (ASIN, etc.) determined by targetingType.	|B0PD34WL1T	|
|targetingType	|The type of identifier provided in target. Could be ASIN, Keyword, etc. Each Opportunity's doc should list what the valid types are.	|ASIN	|

## Examples 

### Explaining percent\_of\_peers, peer\_kpi, peer\_kpi\_difference\_percent for Sponsored Display - Sales opportunity

**Brand:** TestBrand<br />**Browse node:** Gifts & Stationery

30% of TestBrand’s peers in Gifts & Stationery used Sponsored Display. On average they had 71% higher total sales.

1. percent\_of\_peers: Refers to the percentage of peers that has already adopted the suggested opportunity, i.e., use Sponsored Display ads. In the above example, percent\_of\_peers is 30%.
2. peer\_kpi: Refers to the kpi like sales, the consideration that is used for opportunity generation.
3. peer\_kpi\_difference\_percent: Difference in peer\_kpi between the group of peers who have adopted the opportunity vs. the group of peers who have not adopted the opportunity. It showcases potential benefit. In the above example peer\_kpi\_difference\_percent is 71%. TestBrand’s peers who have adopted Sponsored display have 71% higher total sales on average compared to peers who have not adopted Sponsored display ads.
