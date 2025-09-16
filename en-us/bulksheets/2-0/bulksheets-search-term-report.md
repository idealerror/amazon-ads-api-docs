---
title: Bulksheets search term report
description: Search term reports can now be downloaded into bulksheets files
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Brands
    - Reporting
keywords:
    - negative keyword
    - targeting
    - performance
---

# Search term reports in bulksheets

>[NOTE] Search term reports are available to download in bulksheets for Sponsored Brands and Sponsored Products only.

A search term report gives visibility into the terms that shoppers enter when searching on Amazon. This report can help you understand exactly what a shopper searched for when they saw your ad, and you can use these insights to adjust your targeting strategy.  For example, you can use this report to identify high-performing searches from customers and to create negative keyword or product targets for search terms that don’t meet your goals.

Including the search term report in bulksheets can help you analyze your keywords and product targets more efficiently because you don’t need to download the report separately from the advertising console. In addition, the bulksheets search term report includes the parent/child entity IDs needed to make quick updates. These parent/child entity relationships are important when managing campaigns in bulksheets, so review the [entity diagrams for Sponsored Products](bulksheets/2-0/sp-entities) and [Sponsored Brands](bulksheets/2-0/sp-entities) if you want to learn more about campaign structure. 

Note that some seller- and vendor-specific lookback metrics (for example, 7-day advertised SKU sales, and 14-day advertised ASIN sales) are not included in the bulksheets search term report. For those metrics, you should get a [search term report from the advertising console](https://advertising.amazon.com/help#G3HEFZYWZF84NPS9). The lookback window in bulksheets search term reports is always 7 days for both vendors and sellers. 

## How to access search term reports in bulksheets

| [IMAGE] |   |
| ---- | ---- |
| The following steps explain how you can include a search term report tab when you create and download a custom bulksheet file:  <br/> 1. Log in to your Amazon Ads account <br/><br/> 2. Navigate to the advertising console (vendors) or Seller Central (sellers) <br/><br/> 3. From the left side menu panel, click <b>Sponsored ads > Bulk operations</b> <br/><br/> 4. Select a date range—and note that this date range does not affect the search term report metrics, which will be a 7-day lookback window. This date range selection is for campaign performance only.<br/> <br/> 5. Select the data you want to see in the custom spreadsheet. Be sure to check the "Sponsored Products Search term data" box. <br/><br/> 6. Download the file and navigate to the "Search term report" tab <br/><br/>Learn more about [how to create and download a custom bulksheets file](bulksheets/2-0/get-started-with-bulksheets-part1#how-to-create-and-download-a-custom-spreadsheet). | ![Search term report in checklist](/_images/bulksheets/2-0-images/GettingStartedPart1_BulkOpsMain.png) |

The section below explains what columns you should expect to see in the bulksheets search term report, along with descriptions of each field. 

## Search term report header descriptions 

The information below is available in bulksheets search term reports for both sellers and vendors. Note that this sheet is read-only, so you should not make changes in this sheet. Instead, you can review the customer search terms and how these relate to your keywords and product targets. Then, use these insights to make updates in the campaign tab for either Sponsored Products or Sponsored Brands, depending on which campaign type you want to manage.  

>[TIP] If you copy values from the search term report and paste them into the “Sponsored Products Campaigns” tab with input guidance enabled, be sure to **paste the values only**. Otherwise, pasting values can overwrite the logic and cause the input guidance3 feature to fail. Learn more about [input guidance for bulksheets](bulksheets/2-0/input-guidance)

|Column	|Header	|Description	|
|---	|---	|---	|
|A	|Product	|The campaign type (Sponsored Products or Sponsored Brands)	|
|B	|Campaign Id	|Use this ID in the relevant entity row to update keywords or product targeting	|
|C	|Ad Group Id	|Use this ID in the relevant entity row to update keywords or product targeting	|
|D	|Keyword Id (Read only)	|Use this ID in the "keyword" entity row to update keywords	|
|E	|Product Targeting Id (Read only)	|Use this ID in the "product targeting" entity row to update product targets	|
|F	|Campaign Name (Informational only)	|Informational field so you know the name of the campaign associated with your targeting entities	|
|G	|Ad Group Name (Informational only)	|Informational field so you know the name of the ad group associated with your targeting entities	|
|H	|Portfolio Name (Informational only)	|Informational field so you know the name of the portfolio associated with your targeting entities. Note that this field applies to Sponsored Products only.	|
|I	|State	|The state or status of the keyword or targeting entity	|
|J	|Campaign State (Informational only)	|Informational field so you know the state of the campaign associated with your targeting entities	|
|K	|Bid	|The bid amount for the related targeting entity. Bids can be updated in the Sponsored Products tab. In the keyword or product targeting row, set the "Operation" field to **update**, enter the new bid value, and upload the bulksheets file. Learn more about [updating campaigns in bulksheets](bulksheets/2-0/campaign-update-overview)	|
|L	|Keyword Text	|The word or phrase that you've named your keywords. Keyword text cannot be updated, but you can create new versions of a keyword if you want to modify the text. You can also [pause or archive keywords](bulksheets/2-0/update-sp-campaigns#update-the-keyword-entity), or add negative keywords for ones that are under-performing	|
|M	|Match Type	|For manual keyword targeting campaigns, this is the match type for the keyword or phrase used in the bid. Will be either broad, phrase, or exact. This value cannot be updated, but you can create new keywords with the match type you want.	|
|N	|Product Targeting Expression	|For auto targeting campaigns, this is the product, category, and other attributes being targeted. For individual products, the expression will be **asin="VALUE"**. For categories and other attributes, the expression will be **category="VALUE"**, **price<VALUE**, and so on. For bids set at the targeting group level, the values will be either **close-match**, **loose-match**, **substitutes**, or **complements**.|
|O	|Customer Search Term	|The term or phrase the customer typed into the search bar when they encountered your ad. Note: You may notice alphanumeric entries such as “b00ipgvvz4” in addition to more traditional search terms such as “phone case.” These alphanumeric entries correspond to ASINs and the related product detail page on which your ad displayed. You’ll only see ASIN-related entries listed as customer search terms for your Sponsored Products automatic-targeted campaigns and product-attribute-targeted ad groups. You can block ASINs through negative targeting, or bid for ASINs in manually targeted campaigns.	|
|P	|Impressions	|The number of times the ad was displayed, excluding general and sophisticated invalid traffic. The traffic quality filters removed the potentially fraudulent, non-human, and other illegitimate impressions. Also note: If you compare impressions in the search term report with what you see in the advertising console, the numbers might be different. This is because the search term report includes only the terms that resulted in at least one ad click.	|
|Q	|Clicks	|The number of times the ad was clicked, excluding general and sophisticated invalid traffic. The traffic quality filter removes potentially fraudulent, non-human, and other illegitimate clicks.	|
|R	|Click-through Rate	|The ratio of how often customers click your ad when it’s shown. This is calculated as impressions divided by clicks.	|
|S	|Spend	|The amount of money spent running the campaign. Cost is assessed per click or per 1,000 views depending on cost type.	|
|T	|Sales	|The total product sales that you’ve received on your ads within 7 days from the date your ad was clicked.	|
|U	|Orders	|The number of orders that you've received within 7 days from the date that your ad was clicked. If an order contains multiples of the same unit in one order, this should show as 1 order and 2 units.	|
|V	|Units	|The number of unit orders that you've received within 7 days from the date that your ad was clicked.	|
|W	|Conversion Rate	|The total number of orders divided by sales within 7 days from the date that your ad was clicked.	|
|X	|ACOS	|Total spend divided by your total sales during the campaign. It's expressed as a percentage. For example, if you spend $5 on ads and generate $25 in sales, your ACOS is 20%. This metric provides you a straightforward measure of your advertising’s profitability. Typically, you want a low ACOS.	|
|Y	|CPC	|The cost-per-click metric determines how much is paid for the ads placed on websites or social media, based on the number of clicks the ad receives. It’s calculated by dividing total spend (cost) by the total number of clicks.	|
|Z	|ROAS	|Total product sales divided by the total advertising spend invested on the campaign. It’s represented as a number that is interpreted as an index (multiplier) rather than a percent (%). For example, if you spent 20 dollars on a sponsored ads campaign that generated 100 dollars in sales, the ROAS will be 5.	|
