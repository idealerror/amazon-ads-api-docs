---
title: Conversions with relevance table
description: The Conversions with relevance table
type: guide
interface: api
---
# Conversions with relevance

**Analytics table:** 

* `conversions_with_relevance`

**Audience table:** 

* `conversions_with_relevance_for_audiences`

The data sources `conversions` and `conversions_with_relevance` are used to create custom attribution models both for ASIN conversions (purchases) and pixel conversions.

ASIN conversions tracked on Amazon Ad Server for advertisers or campaigns are available in the above tables. Customers who sell products on Amazon can use these signals to run attribution queries to evaluate impact of off-Amazon media on their Amazon conversions.
To improve advertiser experience and simplify the query process, `conversions` and `conversions_with_relevance` tables are updated to contain signals related to ad-attributed and pixel conversions only, saving advertisers’ time and efforts to build dedicated queries to achieve the same outcome by removing non-ad attributed conversions from the tables.


| Name | Data type | Metric/dimension | Description | Aggregation threshold |
|------|-----------|-----------------|-------------|----------------------|
| advertiser | STRING | DIMENSION | Advertiser name | LOW |
| advertiser\_id | LONG | DIMENSION | Advertiser ID | LOW |
| advertiser\_timezone | STRING | DIMENSION | Time zone of the advertiser | LOW |
| campaign | STRING | DIMENSION | Campaign name | LOW |
| campaign\_budget\_amount | LONG | DIMENSION | Campaign budget amount (millicents) | LOW |
| campaign\_end\_date | DATE | DIMENSION | Campaign end date in the advertiser time zone. | LOW |
| campaign\_end\_date\_utc | DATE | DIMENSION | Campaign end date in UTC | LOW |
| campaign\_end\_dt | TIMESTAMP | DIMENSION | Campaign end timestamp in the advertiser time zone. | LOW |
| campaign\_end\_dt\_utc | TIMESTAMP | DIMENSION | Campaign end timestamp in UTC | LOW |
| campaign\_id | LONG | DIMENSION | The campaign ID for Amazon DSP campaigns. For Sponsored Ads campaign IDs, refer to campaign\_id\_string. Amazon DSP campaign IDs can also be found in campaign\_id\_string. | LOW |
| campaign\_id\_string | STRING | DIMENSION | The campaign IDs for Amazon DSP and Sponsored Ads campaigns (Recommended over campaign\_id column) | LOW |
| campaign\_sales\_type | STRING | DIMENSION | Campaign sales type | LOW |
| campaign\_source | STRING | DIMENSION | Campaign source | LOW |
| campaign\_start\_date | DATE | DIMENSION | Campaign start date in the advertiser time zone. | LOW |
| campaign\_start\_date\_utc | DATE | DIMENSION | Campaign start date in UTC. | LOW |
| campaign\_start\_dt | TIMESTAMP | DIMENSION | Campaign start timestamp in the advertiser time zone. | LOW |
| campaign\_start\_dt\_utc | TIMESTAMP | DIMENSION | Campaign start timestamp in UTC. | LOW |
| combined\_sales | DECIMAL(38,4) | METRIC | Sum of total\_product\_sales+off\_Amazon\_product\_sales | NONE |
| conversion\_event\_name | STRING | DIMENSION | The advertiser's name of the conversion definition | LOW |
| conversion\_event\_source\_name | STRING | DIMENSION | The source of the advertiser-provided conversion data. | LOW |
| conversion\_id | STRING | DIMENSION | Unique identifier of the conversion event. In the data set conversions, each row has a unique conversion\_id. In the data set conversions\_with\_relevance, the same conversion event will appear multiple times if a conversion is determined to be relevant to multiple campaigns, engagement scopes, or brand halo types. | VERY\_HIGH |
| conversions | LONG | METRIC | Conversion count | NONE |
| engagement\_scope | STRING | DIMENSION | The engagement scope between the campaign setup and the conversion. Possible values for this column are PROMOTED, BRAND\_HALO, and null. See also the definition for halo\_code. The engagement\_scope will always be 'PROMOTED' for pixel conversions (when event\_category = 'pixel'). | LOW |
| event\_category | STRING | DIMENSION | For ASIN conversions, the event\_category is the high level category of the conversion event, either 'purchase' or 'website'. Examples of ASIN conversions when event\_category = 'website' include: detail page views, add to wishlist, or the first Subscribe and Save order. For search conversions (when event\_subtype = 'searchConversion'), the event\_category is always 'website'. For pixel conversions, this field is always 'pixel'. | LOW |
| event\_date | DATE | DIMENSION | Date of the conversion event in the advertiser time zone. | LOW |
| event\_date\_utc | DATE | DIMENSION | Date of the conversion event in UTC. | LOW |
| event\_day | INTEGER | DIMENSION | Day of the month of the conversion event in the advertiser time zone. | LOW |
| event\_day\_utc | INTEGER | DIMENSION | Day of the month of the conversion event in UTC. | LOW |
| event\_dt | TIMESTAMP | DIMENSION | Timestamp of the conversion event in the advertiser time zone. | MEDIUM |
| event\_dt\_hour | TIMESTAMP | DIMENSION | Timestamp of the conversion event in the advertiser time zone truncated to the hour. | LOW |
| event\_dt\_hour\_utc | TIMESTAMP | DIMENSION | Timestamp of the conversion event in UTC truncated to the hour. | LOW |
| event\_dt\_utc | TIMESTAMP | DIMENSION | Timestamp of the conversion event in UTC. | MEDIUM |
| event\_hour | INTEGER | DIMENSION | Hour of the day of the conversion event in the advertiser time zone. | LOW |
| event\_hour\_utc | INTEGER | DIMENSION | Hour of the day of the conversion event in UTC. | LOW |
| event\_subtype | STRING | DIMENSION | Subtype of the conversion event. This field provides additional detail about the subtype of conversion event that occurred, such as whether it represents viewing a product's detail page on Amazon.com or completing a purchase on an advertiser's website. For on-Amazon conversion events, this field contains human-readable values, while off-Amazon events measured via Events Manager are represented by numeric values. Possible values include: 'detailPageView', 'shoppingCart', 'order', 'searchConversion', 'brandStorePageView', 'wishList', 'babyRegistry', 'weddingRegistry', 'customerReview' for on-Amazon events, and numeric values like '134', '140', '141' for off-Amazon events. For more information on off-Amazon conversion event subtypes, refer to the "Introduction to Events Manager" entry in the IQL. | LOW |
| event\_type | STRING | DIMENSION | Always 'CONVERSION' in the data sources conversions and conversions\_with\_relevance. | LOW |
| event\_type\_class | STRING | DIMENSION | For ASIN conversions, the event\_type\_class is the high level classification of the event type, e.g. consideration, conversion, etc. This field will be blank for pixel conversions (when event\_category = 'pixel') and search conversions (when event\_subtype = 'searchConversion'). | LOW |
| event\_type\_description | STRING | DIMENSION | Human-readable description of the conversion event. For ASIN conversions, examples include: 'add to baby registry', 'Add to Shopping Cart', 'add to wedding registry', 'add to wishlist', 'Customer Reviews Page', 'New SnS Subscription', 'Product detail page viewed', 'Product purchased'. This field will be blank for search conversions (when event\_subtype = 'searchConversion'). For pixel conversions (when event\_category = 'pixel'), the event\_type\_description will include additional information about the pixel based on information provided as part of the campaign setup, such as 'Homepage visit', 'Add to Shopping Cart' or 'Brand store engagement 2'. Before using these fields to evaluate pixel conversions, we recommend that you consult with the team that set up the advertiser's campaign to determine how the event\_type\_description values for pixel conversions should be interpreted. | LOW |
| halo\_code | STRING | DIMENSION | The halo code between the campaign and conversion. Possible values for this column are TRACKED\_ASIN, VARIATIONAL\_ASIN, BRAND\_KEYWORD, CATEGORY\_KEYWORD, BRAND\_MARKETPLACE, BRAND\_GL\_PRODUCT, BRAND\_CATEGORY, BRAND\_SUBCATEGORY, and null. See also the definition for engagement\_scope. The halo\_code will be NULL for pixel conversions (when event\_category = 'pixel'). | LOW |
| marketplace\_name | STRING | DIMENSION | Name of the marketplace where the conversion event occurred. Example values are online marketplaces such as AMAZON.COM, AMAZON.CO.UK. Or in-store marketplaces, such as WHOLE\_FOODS\_MARKET\_US or AMAZON\_FRESH\_STORES\_US. Also, refer to the column 'purchase\_retail\_program' and nd search the AMC Instructional Query Library for *In Store*, *Fresh*, or *Whole Foods* to find related queries | LOW |
| new\_to\_brand | BOOLEAN | DIMENSION | True if the user was new to the brand. | LOW |
| no\_3p\_trackers | BOOLEAN | DIMENSION | Is this item not allowed to use 3P tracking | NONE |
| off\_amazon\_conversion\_value | DECIMAL(38,4) | METRIC | Non monetary "value" of conversion for non-purchase conversions | NONE |
| off\_amazon\_product\_sales | DECIMAL(38,4) | METRIC | "Value" of purchase conversions provided via Conversion Builder | NONE |
| purchase\_currency | STRING | DIMENSION | ISO currency code of the purchase order. | LOW |
| purchase\_unit\_price | DECIMAL(38,4) | METRIC | Unit price of the product sold. | NONE |
| total\_product\_sales | DECIMAL(38,4) | METRIC | Total sales (in local currency) of promoted ASINs and ASINs from the same brands as promoted ASINs purchased by customers on Amazon. | NONE |
| total\_units\_sold | LONG | METRIC | Total quantity of promoted products and products from the same brand as promoted products purchased by customers on Amazon. A campaign can have multiple units sold in a single purchase event. | NONE |
| tracked\_asin | STRING | DIMENSION | The dimension tracked\_asin is the tracked Amazon Standard Identification Number, which is either the promoted or brand halo ASIN. When the tracked\_item results in a purchase conversion, the tracked\_asin will be populated in this column, in addition to being tracked in the tracked\_item column with the same ASIN value. The tracked\_asin is populated only if the event\_category = 'purchase'. For the first SnS order and non-purchase conversions, such as detail page views, the ASIN is populated under tracked\_item. See also 'tracked\_item'. | LOW |
| tracked\_item | STRING | DIMENSION | Identifier for the conversion event item. The value in this field depends on the subtype of the conversion event. For ASIN-related conversions on Amazon such as purchases, detail page views, add to cart events, this field will contain the ASIN of the product. For brand store events, this field will contain the brand store ID. For branded search conversions on Amazon, this field will contain the ad-attributed branded search keyword. For off-Amazon conversions, this field will contain the ID of the conversion definition. Note that when tracked\_asin is populated, the same value will appear in tracked\_item. | LOW |
| user\_id | STRING | DIMENSION | User ID | VERY\_HIGH |
| user\_id\_type | STRING | DIMENSION | Type of user ID | LOW |