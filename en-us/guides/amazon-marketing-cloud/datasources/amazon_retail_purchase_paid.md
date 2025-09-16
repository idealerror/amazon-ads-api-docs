---
title: Amazon retail purchase 
description: Data sets for Amazon retail purchase
type: guide
interface: api
tags:
    - amc retail purchase
    - amc 5 years data arp
    - retail datasets
    - 60 months data in AMC
    - paid dataset 5 years
---

# Amazon Retail Purchases in AMC 

**Analytics table:**

- `amazon_retail_purchases`

**Audiences table:**

- `amazon_retail_purchase_for_audiences`

The Amazon Retail Purchases (ARP) dataset (`amazon_retail_purchases`) enables advertisers to analyze long-term (up to 60 months) customer purchase behavior and create more comprehensive insights.

### Availability 

This data source is available for measurement queries through a Paid features subscription in the United States, Canada, Japan, Australia, the United Kingdom, France, Germany, Italy, and Spain marketplaces. 

Data from the United Kingdom and European Union (EU) regions will be excluded from audience creation.

### Subscription and access requirements

- This dataset is available through subscription. You can opt into a 60-day free trial, after which a subscription is necessary to access the dataset.

- Check the Sponsored Ads accounts configured to your AMC instance configuration to ensure the correct Vendor/Seller Central accounts are associated. You can verify the associations in **Ads Console > Administration (gear icon) > Account access & settings > Selling account**.

    - For **Vendor Central** accounts, the scope of data in ARP will match with what is shown in the Vendor Central under **Reports > Retail Analytics > Sales > Distributor View = Manufacturing**.

    - For **Seller Central** accounts, the scope of data in ARP should match with what is shown in the Seller Central **Reports > Business Reports > By ASIN Detail** report.


> [WARNING] The ARP dataset in AMC contains 60 months (5 years) of historical purchase data, as versus the 13-month of advertising records in the other datasets. Exercise caution when joining the retail purchase data with other AMC datasets, as failing to account for the mismatched time windows could lead to inaccurate analysis and incorrect conclusions. 

### Conversion datasets

While the AMC `conversions` and `conversions_all`  (referred to as CONV) datasets may appear similar, they are different in the following ways:


| Aspect | CONV | ARP |
|---------|------|-----|
| Scope of conversion types | Various conversion event types (purchases, considerations, pixel-based conversions). | Amazon store purchase conversion events only. |
| Scope of events | Advertising-centric (can include promoted, brand-halo, and brand-owned ASINs). | Entire purchase activity in Amazon store, regardless of advertising. |
| Data pipelines and restatements | Sourced from advertising data pipelines; subject to restatements and advertising augmentation. | Sourced from retail data pipeline including restatements, and more closely matched data found in other Amazon reporting surfaces like Vendor Central. |

The AMC dataset associated with ARP is `amazon_retail_purchases`.

## amazon\_retail\_purchases

 Field name | Data type | Description | Aggregation Threshold
---|---|---|---
 asin | STRING | ASIN is the abbreviation for Amazon Standard  Identification Number\. This represents the item associated with the retail  event\. A unique block of letters and/or numbers that identify all products  sold on Amazon\. | LOW 
 asin\_brand | STRING | ASIN item merchant brand name\. | LOW 
 asin\_name | STRING | ASIN item name\. | LOW 
 asin\_parent | STRING | The Amazon Standard Identification Number \(ASIN\) that  is the parent of this ASIN for the retail event\. | LOW 
 currency\_code | STRING | ISO currency code of the retail event\. | LOW 
 event\_id | STRING | Unique identifier of the retail event record\. In the  data set, each row has a unique event\_id\. This event\_id can have a  many\-to\-one relationship with purchase\_id\. | VERY\_HIGH 
 is\_business\_flag | BOOLEAN | Indicates whether the retail event is associated with "Amazon Business" program\. | LOW 
 is\_gift\_flag | BOOLEAN | Indicates whether the retail event is associated with "send an order as a gift"\. | LOW 
 marketplace\_id | INTEGER | Marketplace ID of where the retail event occurred\. | INTERNAL 
 marketplace\_name | STRING | Name of the marketplace where the retail event occurred. Example values include AMAZON\.COM, AMAZON\.CO\.UK, etc\. | LOW 
 no\_3p\_trackers | BOOLEAN | Is this item not allowed to use 3P tracking\. | NONE 
 origin\_session\_id | STRING | Describes the session when the retail item was added  to cart\. | VERY\_HIGH 
 purchase\_date\_utc | DATE | Date of the retail event in UTC\. | LOW 
 purchase\_day\_utc | INTEGER | Day of the month of the retail event in UTC\. | LOW 
 purchase\_dt\_hour\_utc | TIMESTAMP | Timestamp of the retail event in UTC truncated to the  hour\. | LOW 
 purchase\_dt\_utc | TIMESTAMP | Timestamp of the retail event in UTC\. | MEDIUM 
 purchase\_hour\_utc | INTEGER | Hour of the day of the retail event in UTC\. | LOW 
 purchase\_id | STRING | Unique identifier of the retail purchase record\. This  purchase\_id can have a one\-to\-many relationship with event\_id\. | VERY\_HIGH 
 purchase\_month\_utc | INTEGER | Month of the conversion event in UTC\. | LOW 
 purchase\_order\_method | STRING | Enumerated descriptor of how the shopper purchased the item\. S = Shopping Cart\. B = Buy Now\. 1 = 1\-Click Buy\. | LOW 
 purchase\_order\_type | STRING | Type of retail order\. Values include “Prime” and “NON\-PRIME”\. | LOW 
 purchase\_program\_name | STRING | Indicates the purchase program associated with the retail event\. | LOW 
 purchase\_session\_id | STRING | Describes the session when the retail item was  purchased\. | VERY\_HIGH 
 purchase\_units\_sold | LONG | Total quantity of retail items associated with the  retail event\. A record can have multiple units sold of a single retail item\. | NONE 
 unit\_price | DECIMAL | Per unit price of the product sold\. | NONE 
 user\_id | STRING | User ID associated with the retail event \(the user ID of the shopper that purchased the item\)\. | VERY\_HIGH 
 user\_id\_type | STRING | Type of user ID\. The only value for this data set is “adUserId”\. | LOW 

