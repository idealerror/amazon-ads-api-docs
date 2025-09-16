---
title: Bulksheets 2.0 FAQs
description: Bulksheets frequently asked questions for all sponsored ads campaign types.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
keywords:
    - FAQs
    - frequently asked questions
    - bulksheets  
---

# Bulksheets frequently asked questions (FAQs)
This article contains FAQs for general bulksheets questions, along with campaign-specific ones. Click on the questions to see the answers and learn more.

*Jump to* [Sponsored Products](#sponsored-products-faqs) | [Sponsored Brands](#sponsored-brands-faqs) | [Sponsored Display](#sponsored-display-faqs)

## General FAQs

<details>
<summary><strong>How do I access bulksheets?</strong></summary>
You can find bulksheets templates from the main bulk operations page. Access this page from campaign manager: Look for the sponsored ads icon in the left navigation panel > click **Bulk operations**. Learn more about how to access bulksheets in the [Getting started guide](bulksheets/2-0/get-started-with-bulksheets-part1). 
</details>
<br>
<details>
<summary><strong>I’ve never used bulksheets. What’s the easiest way to get started?</strong></summary>
We recommend reviewing the support articles here in the advanced tools center, including the introductory [Getting started guide](bulksheets/2-0/get-started-with-bulksheets-part1). In terms of campaign type, you might find that a Sponsored Products auto-targeting campaign is a good place to start if you want to try using bulksheets. After you create the campaign, you can monitor and optimize by downloading a custom bulksheets file and reviewing the performance metrics. 
</details>
<br>
<details>
<summary><strong>I’ve used bulksheets in the past, but now there’s a new version. Where can I learn more about the changes in the new bulksheets?</strong></summary>
We recommend starting with the [Bulksheets overview article](bulksheets/2-0/overview-about-bulksheets). This resource contains FAQs specifically about the new version, along with other tips and guidelines for migrating. 
</details>
<br>
<details>
<summary><strong>Does bulksheets work with any campaign type?</strong></summary>
Yes. Bulksheets now supports all sponsored ads campaigns, including Sponsored Products, Sponsored Brands, and Sponsored Display. Learn more about using bulksheets to create sponsored ads campaigns in the [product-specific user guides](bulksheets/2-0/get-started-with-bulksheets-part1#product-specific-guides). 
</details>
<br>
<details>
<summary><strong>Can I use bulksheets if I’m managing campaigns in the advertising console?</strong></summary>
Yes. Many advertisers use a combination of Amazon Ads tools, including bulksheets, the advertising console, and the Amazon Ads API. If you're currently using the console to manage several campaigns, you might find that bulksheets is a more efficient way to make changes to multiple campaigns--and bulksheets requires no developer resources, so it might be a better option than the API.
If you aren’t sure which option to use, the advanced tools center can help you decide [which Amazon Ads tool is right for you](API/docs/en-us/index). 
</details>
<br>
<details>
<summary><strong>Where can I find the entity IDs needed to make updates in bulksheets?</strong></summary>
 You can find the IDs for all entities (campaigns, ad groups, etc.) if you download a bulksheets file. Note that bulksheets entity IDs are different than the IDs you'll find in the advertising console, so be sure you are getting the IDs from bulksheets and not from the console. Learn more about [how to update campaigns using bulksheets](bulksheets/2-0/campaign-update-overview). 
</details>
<br>
<details>
<summary><strong>How do I change a campaign’s name?</strong></summary>
To update a campaign name, the mandatory required fields are **Product**, **Entity**, **Operation**, **Campaign ID**, and **Campaign Name**. If you are using a downloaded bulksheets file that contains campaign data, many of those values will already be in the spreadsheet, so you’ll only need to make two edits: Set the “Operation” field to **update**, and enter the new name in the “Campaign Name” field. If you are using a blank template, you’ll need to add all of the mandatory fields. Learn more about changing campaign names in [Overview - update campaigns with bulksheets](bulksheets/2-0/get-started-with-bulksheets-part1#product-specific-guides). 
</details>
<br>
<details>
<summary><strong>How do I change an ad group’s name?</strong></summary>
To update an ad group, the mandatory required fields are **Product**, **Entity**, **Operation**, **Campaign ID**, **Ad Group ID**, and **Ad Group Name**. If you are using a downloaded bulksheets file that contains campaign data, many of those values will already be in the spreadsheet, so you’ll only need to make two edits: Set the “Operation” field to *update*, and enter the new name in the “Ad Group Name” field. 
</details>
<br>
<details>
<summary><strong>What are the "informational only" fields used for?</strong></summary>
When you download a custom bulksheet with campaign data, you will see some fields designated as "informational only." Some examples of these include **campaign name** and **ad group name**, **campaign status** and **ad group status**, **ASIN eligibility status** fields, and **ad group default bid**. <br><br> The purpose of these fields is to give you helpful information about names and statuses that would otherwise be hidden when you have filters applied to the "Entity" column. For example, let's say you want to isolate keywords, so you might apply a filter to show keyword entities only. In this case, the names and statuses of the associated campaigns and ad groups would be hidden. But with the "informational only" fields, you can still see those values regardless of filtering. 
</details>
<br>
<details>
<summary><strong>I uploaded my bulksheets file and got an error. What should I do now?</strong></summary>
If your file contains any errors, you will see a message in the “Bulk spreadsheet history” section indicating how many rows were processed and how many rows contained errors. If your sheet contained errors, you can download a report to see exactly which rows have errors and what the cause of the error might be. <br><br> Then, **you can make edits directly in the error report file, and upload that file to finish creating or updating the campaign**. For example, let’s say you received an error because you included a forbidden character in a keyword row. In the error report, you can fix that error, save the file, and upload it to fix the issue. You don’t need to delete the other sheets because the upload process will simply ignore the summary sheet and error columns. 
</details>
<br>
<details>
<summary><strong>How can I tell if an ASIN is eligible to serve in my ads?</strong></summary>
Before you create the ad in bulksheets, you can check your product inventory to be sure the items you want to advertise are in stock and have the required elements, such as a product image. After you create your campaigns, you can download a bulksheets file with campaign data to check ASIN eligibility. This will show you if any ASINs became ineligible after the campaign started running. If your campaign performance metrics are unusually low, or if you see that lots of customers are clicking your ad but not completing the purchase, this could be due to ASIN eligibility. For example, an ASIN that has gone out of stock might still generate ad clicks, but customers wouldn’t be able to purchase. In a downloaded bulksheets file, look for column headers near the ASIN field that indicate “Eligibility Status” and “Reason for Ineligibility.” In addition to being out of stock, other reasons for ineligibility include missing or incomplete product information, such as a product that does not include a product image. 
</details>
<br>
<details>
<summary><strong>How can I stop a campaign from running?</strong></summary>
Similar to campaign management in the advertising console, you can archive or pause campaigns to stop them from running. Archiving is a permanent action that cannot be undone, while pausing is less permanent.<br><br> To archive a campaign, the mandatory fields are **Product**, **Entity**, **Operation**, and **Campaign ID**. In a downloaded file, most of these fields will already be populated. In the “Operation” field, enter *archive* to archive the campaign. To pause a campaign, the mandatory fields are **Product**, **Entity**, *Operation*, **Campaign ID**, and **State**. In the “Operation” field, enter **update**. In the “State” field, enter **paused**. To undo this action, you can follow these same steps, but enter **enabled** in the “State” field. 
</details>
<br>
<details>
<summary><strong>How do I remove keywords from a campaign?</strong></summary>You cannot update keywords by changing the keyword text. Instead, you should pause or archive the keywords that you want to stop running. This will be easier if you’re using a custom template pre-populated with entity IDs. To pause keywords, set the “Operation” to **update** and the “State” to **paused**. To archive keywords, set the “Operation” to **archived**. Note that the keyword ID is also required, but this will already be in your spreadsheet if you are using a custom bulksheets file. 
Learn more about about [updating keywords for Sponsored Products](bulksheets/2-0/update-sp-campaigns#update-the-keyword-entity) and [updating keywords for Sponsored Brands](bulksheets/2-0/update-sb-campaigns#update-the-keyword-entity). 
</details>
<br>
<details>
<summary><strong>How can I swap out ASINs for a product targeting campaign?</strong></summary>
Similar to keywords, you cannot swap ASINs or change products after the entity is created. Instead, you should archive or pause the product targeting entities that you want to remove. Then, you can create a new product targeting entity within the campaign, using the ASINs you want to apply.
</details>
<br>
<details>
<summary><strong>If I make updates in a downloaded custom bulksheet, do I need to delete the rows that I’m not updating?</strong></summary>
No. If the “Operation” field is blank for any row, that entire row will be ignored in the upload process. So as long as you only include values in the rows you want to update, you can leave the other data in the sheet for better efficiency. 
</details>

## Sponsored Products FAQs

<details>
<summary><strong>How can I set a campaign bidding adjustment?</strong></summary>
To set a bidding adjustment, you should create a new **bidding adjustment** entity under the campaign, using the campaign’s ID. For the bidding adjustment row, the required fields include Product, Entity, Operation, Campaign ID, Bidding strategy, Placement, and Percentage. Learn more about [adding a bidding adjustment to a Sponsored Products campaign](bulksheets/2-0/create-sp-campaign#step-2-define-the-bidding-adjustment-entity-optional). 
</details>
<br>
<details>
<summary><strong>With auto targeting, can I create product targeting entities?</strong></summary>
Yes, you have the option to create four product targeting entities with auto targeting campaigns, and you can apply bids to any of these. If you want to define these entities, you should create four rows in your auto targeting campaign—one for each product targeting expression: close-match, loose-match, complements, and substitutes (see the definitions listed below). In the ads console, this is the section where you can <strong>Set bids by targeting group</strong>. For any targets you do not want to bid on, you should set the “State” field to be <strong>paused</strong> in those rows. <br>

If you don’t create these product targeting entities, Amazon will automatically create one entity for each targeting expression, and the ad group default bid will apply. Learn more about [how to create Sponsored Products auto-targeting campaigns](bulksheets/2-0/create-sp-campaign).

Auto-targeting expression definitions:<br>

**Close-match**: Amazon will show your ad to shoppers who use search terms closely related to your products. If your product is "Doppler 400-count Cotton Sheets," we'll show an ad when shoppers use search terms like "cotton sheets" and "400-count sheets." <br>
**Loose-match**: Amazon will show your ad to shoppers who use search terms loosely related to your products. If your product is Doppler 400-count Cotton Sheets, we'll show an ad when shoppers use search terms like "bed sheets," "bath sheets," and "bath towels."<br>
**Substitutes**: Amazon will show your ad to shoppers who use detail pages of products similar to yours. If your product is "Doppler 400-count Cotton Sheets," we'll show an ad on detail pages that include "300-count Cotton sheets" and "queen 400-count Sheets."<br>
**Complements**: Amazon will show your ad to shoppers who view the detail pages of products that complement your product. If your product is "Doppler 400-count Cotton sheets," we'll show an ad on detail pages that include "queen comforter" and "feather pillows."

 </details>
<br>

 ## Sponsored Brands FAQs 


<details>
<summary><strong>Can I update a campaign’s creative headline?</strong></summary>
No. Creative headlines cannot be updated after you create them. Instead, you should [archive the campaign](bulksheets/2-0/update-sb-campaigns#archive-a-campaign) and create a new campaign using the creative headline you want to apply. If the creative headline is the only campaign element you want to update, you can save time by copying some of the other campaign data (brand asset IDs, for example) into the new campaign row. 
</details>
<br>
<details>
<summary><strong>How can I locate brand asset IDs to create a new Sponsored Brands campaign?</strong></summary>
When you download and create a custom spreadsheet, the file will contain a tab labeled “Brand Assets Data” if you select the option to include this data in the download. Learn more about [how to understand bulksheets data](bulksheets/2-0/understand-bulksheets-data).
</details>

## Sponsored Display FAQs


<details>
<summary><strong>How can I define a targeting tactic for Sponsored Display campaigns?</strong></summary>In bulksheets, you'll enter the ** targeting tactic**  value as an alpha-numeric code. The tactic is either **product targeting**  or **audiences**. In the bulksheets field, you should enter ** T00020**  for product, or **T00030**  for audience. Note that in the advertising console, product targeting is now **contextual** targeting.
</details>
<br>
<details>
<summary><strong>How do I define the targeting expression values for product (contextual) and audience targeting?</strong></summary>In bulksheets, you’ll enter values such as **category=“12345”**  to target based on a product category, or **purchases=(related-product lookback=30)** to target based on purchases remarketing. <br><br> To do this, you'll need the IDs that represent the expression you want. You can get these values by downloading a bulksheet with some past Sponsored Display campaign data. Look in the ** Resolved targeting expression (read only)**  column for values such as category. <br><br>For example, you might see: category="Kids' Electronics." Then, look in the ** Targeting expression**  column in that same row for the corresponding ID. For example, the corresponding targeting expression field might show: category="166164011," so you would know to use that numeric ID. 
<br><br>
Below are some additional examples of what you might enter in this field, depending on what you want to target:
<br>
-  views=(category="2748212011" price=10-50 lookback=30)
<br>
-  purchases=(related-product lookback=30)
<br>
-  purchases=(exact-product lookback=30)
<br>
-  audience="360733347960105375"
<br><br>
Learn more about [how to create Sponsored Display campaigns in bulksheets](bulksheets/2-0/create-sd-campaign). 
</details>
