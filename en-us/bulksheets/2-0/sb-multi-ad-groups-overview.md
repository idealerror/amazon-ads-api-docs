---
title: Overview - Sponsored Brands multi-ad group campaigns in bulksheets 
description: An overview and introduction to the latest version of Sponsored Brands in bulksheets. Includes key new features such as multi-ad groups and custom bidding adjustments by placement.
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands
    - Sponsored Brands v4
    - Sponsored Brands multi-ad groups
    - Campaign management
    - Onboarding
keywords:
    - get started with bulksheets
    - Sponsored Brands  
---

# Overview - Sponsored Brands multi-ad group campaigns in bulksheets

NOTE: In this guide, we'll refer to the current Sponsored Brands campaigns without multi-ad group support as <b>legacy</b> campaigns whereas campaigns with multi-ad group support are referred to as <b>multi-ad group</b> campaigns.

>[WARNING] Sponsored Brands campaigns created before November 15, 2022 will not show in the multi-ad group tab of bulksheets. To see older campaigns, look in the legacy Sponsored Brands tab. 

This article provides an overview of using Sponsored Brands multi-ad group campaigns using a bulk spreadsheet, including explanations of the new entities and descriptions of each column header in the new spreadsheet tab. For a more detailed explanation on using the new multi-ad group spreadsheet, review the next article: [How to create Sponsored Brands multi-ad group campaigns](bulksheets/2-0/create-sb-multi-ad-group-campaigns). 

With Sponsored Brands multi-ad group campaigns, you can:

* create and manage multiple (multi) ad groups
* add a custom bidding adjustment by placement 
* update your creative assets in bulksheets, including the creative headline, landing pages, and image files
* use all Sponsored Brands ad formats, including Store spotlight, Product collection, Video, and Brand video ads.

You can still use the legacy version of Sponsored Brands in bulksheets, but it will eventually be deprecated, so we recommend migrating to the new version soon.

To use Sponsored Brands multi-ad group campaigns with bulksheets, you will use a new tab that is different from the legacy version, and is described below. 

## How to get a Sponsored Brands multi-ad group file

| [IMAGE] |   |
| ---- | ---- |
|From the bulk operations homepage, in “Step 1. Create & download a custom spreadsheet,” check the box to include Sponsored Brands multi-ad group data → <br><br>You can leave the other boxes checked by default, or you can deselect (uncheck) them for a potentially faster download. Click Create spreadsheet for download. <br><br>This will create a tab in the spreadsheet, titled “SB multi-ad group campaigns.” This new tab will contain your Sponsored Brands ad group information, including entities for the four Sponsored Brands ad types (or ad formats) in each ad group. You’ll also see entities for “Bidding adjustment by placement” if you’ve applied custom adjustments to any campaigns. All entities you might find in the new multi-ad group data tab are described in more detail below. |![Multi-ad groups in checkbox selection](/_images/bulksheets/2-0-images/SBMultiAdGroup_Checkboxes.png "Multi-ad groups in checkbox selection")|

Note: The tab labeled Sponsored Brands data is the legacy version, and it will not include any ad group, ad format, or placements information. It will only contain campaign and targeting entities. If you want to view or create ad groups or bidding adjustments, you must do this in the new Sponsored Brands multi-ad group data tab. 

## Sponsored Brands multi-ad group entities

In bulksheets, you will always define the campaign “entity” in Column B of your spreadsheet. With the introduction of this latest version of Sponsored Brands, you will have additional campaign entities that only apply to the new version. These new entities can be viewed and managed in the Sponsored Brands multi-ad groups data tab in your downloaded bulksheets file. The table below contains more information about Sponsored Brands multi-ad group entities. Learn more about multi-ad group campaign structure in the Sponsored Brands entity diagram

Note: The entities labeled as “NEW” will only be shown in the Sponsored Brands multi-ad groups data tab, and will not appear in the legacy Sponsored Brands data tab. 

|Entity (spreadsheet Column B)    |Description    |
|---    |---    |
|Campaign    |This entity defines the top-level parent campaign, and will include details such as Campaign ID and Name, the Start date, State, Budget information, and Bid optimization status—set this field to "false" if you want to create a custom bidding adjustment. Otherwise, enter "true" and Amazon will automatically set this value.     |
|Bidding adjustment by placement <sup>NEW</sup>    |You can add an optional custom Bidding adjustment by placement if you want to manually configure bid changes depending on placement. To do this, be sure to set the "Bid optimization" field to "false" in the "Campaign" entity row. Then, create this entity in its own separate row in the spreadsheet.     |
|Ad group <sup>NEW</sup>    |With the latest version of Sponsored Brands, you can now create and manage ad groups—including adding multiple ad groups to a single campaign. The ad group is a parent of the ad and targeting entities, so you will always include the Ad group ID value in those child entity rows in bulksheets.     |
|Product collection ad <sup>NEW</sup>    |Select Product collection if you want to promote unique collections of products or existing Stores. With this ad format entity, you can promote multiple products from a landing page of your choice: Either Store on Amazon, a new landing page that Amazon will create for you, or a custom URL that you've created. To create this ad format in bulksheets, you'll need to include creative assets such as landing page URL, brand name and logo, ASINs you want to advertise, and a creative headline.  <br><br> With a product collection, you must also define custom images. Review [Step 3 in the “Create campaigns” article](bulksheets/2-0/create-sb-multi-ad-group-campaigns#step-3-define-the-product-collection-ad-entity) to learn more about how to format the custom images.  |
|Video ad <sup>NEW</sup>    |With video ads, you can promote a single product and drive traffic to the product detail page or your Store. Select the "Video ad" entity if you want to drive traffic to a Product detail page, and enter "Detail page" as the landing page type. If you want to drive traffic to a Store on Amazon, select the "Brand video ad" entity as your ad format, and enter "Store" as the landing page type.      |
|Brand video ad <sup>NEW</sup>    |This is a video ad with a Store on Amazon landing page, so if you select this as your ad format in bulksheets, you should enter "Store" as the landing page type.     |
|Store spotlight ad <sup>NEW</sup>    |Select Store spotlight if you want to drive traffic to certain Store pages. With a Store spotlight, you'll drive traffic to a Store and its sub-pages. The landing page type is "Store," and it must include sub-pages.    |
|Keyword    |You can choose keywords to help your products appear in shopper searches. To create keyword or product targets for Sponsored Brands mult-ad group campaigns, the functionality is exactly the same as it is in the legacy version of Sponsored Brands. If you want a keyword targeting strategy, you need a row for the keyword entity. Include the Campaign ID and Ad group ID, State, Bid amount, Keyword text, and Match type.     |
|Negative keyword    |If keywords are overspending or underperforming, you can negate them to prevent the terms from appearing for certain searches or products. Include the same fields that you use for regular keyword targeting.     |
|Product targeting    |With product targeting, you can choose specific products, categories, brands or other product features to target your ads. You will need the ASIN values for the products you want to advertise.     |
|Negative Product Targeting    |Similar to negative keyword targeting, you can negate products to prevent your ads from displaying when a shopper's search matches your negative product selections. This helps exclude irrelevant searches, reducing your advertising cost. You will need the ASIN values for the products you want to negate.     |

If you’re ready to start creating campaigns using the new Sponsored Brands format, see [How to create Sponsored Brands multi-ad group campaigns](bulksheets/2-0/create-sb-multi-ad-group-campaigns). If you want more details about each field you’ll see in the new Sponsored Brands multi-ad group data tab, continue reading. 


## Explanation of Sponsored Brands multi-ad group fields

The table below represents a downloaded bulksheets template. Each column header from the Sponsored Brands multi-ad group data tab is represented below, with a description of that field. Read-only and metrics headers are not included. Note that the column headers are transposed when compared with an actual bulksheets file. 

|Column header    |Description    |
|---    |---    |
|Product    |This is how you choose your campaign type in bulksheets. Enter Sponsored Brands    |
|Entity    |This is where you'll select the specific campaign entity to create in each row. Enter one of the values described in the entity table above    |
|Operation    |This is the action you want to take in each row. Options are Create, Update, or Archive.     |
|Campaign ID    |To create a new campaign, enter a temporary ID of your choice. Be sure this exact same ID is entered into all child entity fields as well. This ID will become a unique system-generated ID when the campaign is created, and you'll use that unique ID to update the campaign later.     |
|Portfolio ID    |You can add a campaign to an existing portfolio if you enter the Portfolio ID when you create the campaign.     |
|Ad Group ID    |Similar to the Campaign ID field: To create a new ad grouup, enter a temporary ID of your choice. Be sure this exact same ID is entered into all child entity fields as well. This ID will become a unique system-generated ID when the ad group is created, and you'll use that unique ID to update the ad group later.     |
|Ad ID    |For the Ad ID, Keyword ID, and Product Targeting IDs, leave these blank when you're creating the entities. A system-generated ID will be created, and you will see this ID in bulksheets when you download your campaigns.     |
|Keyword ID    |See above    |
|Product Targeting ID    |See above    |
|Campaign name    |Enter a unique name in the Campaign entity row    |
|Ad group name    |Enter a unique name in the Ad group entity row    |
|Ad name    |Enter a unique value in the Ad name entity row     |
|Start date    |This value is defined in the Campaign entity row. Leave blank, and the campaign will start on today's date. Otherwise, enter the future date that you want the campaign to start.     |
|End date    |Leave blank, and the campaign would run indefinitely. You can stop the campaign by pausing or archiving it. Otherwise, enter the date that you want the campaign to stop running.     |
|State    |For all entities other than the "Bidding adjustment by placement," enter a status. Usually this will be enabled, but you can also use this field to pause an entity. To do this, enter "Update" in the "Operation" field, and set this "State" field to be paused     |
|Brand Entity ID    |When you create the "Ad format" entity (e.g. Product collection ad, Video ad, Store spotlight ad, Brand video ad), you will enter creative asset details, including your Brand Entity ID. You can locate this in bulksheets, in the "Brand assets data" tab.     |
|Campaign state    |You can ignore this field as it is read-only    |
|Budget type    |This is defined in the campaign entity row, and will be either Lifetime or Daily    |
|Budget    |Your budget determines how much you will spend on this campaign, so enter the numeric value to set your budget. You can edit your campaign to change your budget anytime by entering the new budget value and entering Update in the campaign's "Operation" field.     |
|Bid optimization    |In the UI, this is called "Automated bidding," and there is a toggle to turn this on or off. In bulksheets, you'll do this by entering either True (to enable automated bidding) or False (to set your own custom bidding adjustment by placement).     |
|Product location    |You can ignore this field as it is read-only    |
|Bid    |The bid is set at the target-level, so you will define this in the row where you create a Keyword or Product targeting entity. You can update this value at any time after you create the campaign. Simply change the bid value, and enter Update as the "Operation" for the keyword bid you want to change.     |
|Placement    |This lets you set a custom bidding adjustment if you indicated False for the campaign's "Bid optimization" value. You'll define this in the Bidding adjustment by placement entity row, and options include Home, Detail page, or Other    |
|Percentage    |If you are setting a custom bidding adjustment, this is where you'll enter the percentage value.     |
|Keyword text    |If you are using a keyword targeting strategy, you will create a row in your spreadsheet for the Keyword entity. In that entity row, you'll enter the keyword you want to add.     |
|Match type    |For the Keyword entity, this defines the match type: broad, phrase, or exact.     |
|Native language keyword    |You can ignore this field as it is read-only    |
|Native language locale    |You can ignore this field as it is read-only    |
|Product targeting expression    |If you want a product targeting strategy, you will create a Product targeting entity row where you'll identify this "Product targeting expression" value. In the UI, this is where you select either "individual products" or "categories" to target. For "individual products," use the asin and enter asin="A000000000000" (using the actual asin ID—note that in this field, asin must be entered all lowercase as shown). For "categories," use the category ID along with other qualifiers such as price limit and star rating. For this field, enter all qualifiers into the same cell of your bulksheets file because they work as a combined unit to define the targeting.    |
|Landing page URL    |All creative details are defined in the same row where you create the ad format—including landing page information. The URL is where you want to direct shoppers when they click your ad, and will be either a Store on Amazon, a Product detail page, a new landing page that Amazon creates with the ASINs you're advertising, or a custom URL that you provide. You also have the option to provide custom images if you don't want to use the images from your advertised ASINs—you'll do this in the "Custom images" field.     |
|Landing page ASINs    |You can ignore this field as it is read-only    |
|Landing page type    |This is where you want to direct shoppers when they interact with your ad. Options  include Store, Custom URL, or New landing page.    |
|Brand name    |This is your brand name—or the name of the brand you want to manage in a given bulksheets file    |
|Consent to translate    |You can ignore this field as it is read-only    |
|Brand logo asset ID    |This is the asset ID for your brand logo. You can find this value in the "Brand assets data" tab of a downloaded bulksheets file.     |
|Brand logo crop    |This field lets you crop the logo if needed, using the format {"left":"0","top":"0","width":"0","height":"0"} to define the pixels.    |
|Custom images    |If you want shoppers to see a different image other than what is represented by your ASINs, enter the image IDs here.     |
|Creative headline    |Enter the creative headline to highlight the unique benefits of the products in your ad. We recommend using a conversational tone that matches your brand. Learn more about headline and messaging guidelines    |
|Creative ASINs    |This is the list of products you want to feature or advertise. Enter all of them in this one field, separated by commas.     |
|Video asset IDs    |If your ad format is Video or Brand video, enter the IDs for the videos you want to use. You can find these asset IDs in the "Brand assets data" tab of a downloaded bulksheets file.     |
|Sub-pages    |If your landing page is a Store on Amazon, you can enter sub-pages to include here.     |

Next: [How to create Sponsored Brands multi-ad group campaigns →](bulksheets/2-0/create-sb-multi-ad-group-campaigns)  

