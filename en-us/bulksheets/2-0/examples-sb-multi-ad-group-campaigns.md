---
title: Additional examples of Sponsored Brands multi-ad group campaigns
description: A guide containing examples of various ad formats
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands V4
    - Sponsored Brands multi-ad groups
    - Campaign management
keywords:
    - bulksheets  
    - create campaigns
---

# Examples of Sponsored Brands multi-ad group campaigns in bulksheets 

This article provides additional examples of what a bulk spreadsheet might look like for various Sponsored Brands multi-ad group campaign types, including the different ad formats and targeting types. For a more general overview, see this [Overview of Sponsored Brands multi-ad group campaign in bulksheets](bulksheets/2-0/sb-multi-ad-groups-overview). For in-depth guidance on creating campaigns, see the guide on [How to create Sponsored Brands multi-ad group campaigns](bulksheets/2-0/create-sb-multi-ad-group-campaigns).

## Examples of  multi-ad group campaigns in bulksheets 

The table below represents a bulksheets file. In the table, you can see the required fields that you must complete in the spreadsheet in order to create each entity—including the [ad formats described here](bulksheets/2-0/sb-multi-ad-groups-overview#Sponsored-Brands-multi-ad-group-entities). 

NOTE: To save space, we have removed the “Informational only” fields from the example spreadsheets below. You can see where these fields would appear by observing the column header letters. For example, notice that Column S (Brand Entity ID) skips to Column AC—this is because we’ve removed several of the informational fields in that area.  

### Create new Campaign with two Ad groups, a Video ad, and a Store spotlight ad

Notes about this campaign:

* Notice that the Campaign and Ad group IDs are included in all of their child entity rows
* This example will create a new campaign, so we are using temporary, text-based IDs. When this campaign is uploaded and created, a system-generated ID will be created, which you would use to update the campaign later 
* The campaign “Bid optimization” is set to FALSE, so we do not need to create a custom **Bidding adjustment by placement** entity for this campaign. However, this entity could be added later if the performance metrics suggest a custom placement would be more optimal.
* When you create a **Store spotlight ad**, you can include three sub-pages from your Store, and an image to best represent each page. In bulksheets, use the following formula in the “Sub-pages” field of the **Store spotlight ad** row, replacing the asin values, page title, and URL with your information: [{"asin":"B07SBHTQMC","pageTitle":"MyDisplayName","url":"www.amazon.com/stores/page/MyStore1"},{"asin":"B0058DKFBS","pageTitle":"My DisplayName2","url":"www.amazon.com/stores/page/MyStore2"},{"asin":"B07RF9GMBY","pageTitle":"MyDisplayName3","url":"www.amazon.com/stores/page/MyStore3"}]
* Tip: In the UI, the **Store spotlight ad** page title is the “Display name.” The asin is the image you want to display. And the URL is the actual URL of your store page. 



|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|Create	|Campaign example 1	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|Campaign example 1	|	|Ad group example 1	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Video ad	|Create	|Campaign example 1	|	|Ad group example 1	|	|	|	|	|	|Video ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/dp/B08CK9FVB9	|	|Detail Page	|	|	|	|	|	|	|B08CK9FVB9	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Keyword	|Create	|Campaign example 1	|	|Ad group example 1	|	|	|	|	|	|	|	|	|enabled	|	|	|	|	|	|1.47	|	|	|dogs collars for large dogs	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|Campaign example 1	|	|Ad group example 2	|	|	|	|	|Ad group example 2	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|	|Campaign example 1	|	|Ad group example 2	|	|	|	|	|	|Store spotlight ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|www.amazon.com/stores/page/MyStore1	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|Store spotlight headline	|	|	|[{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"www.amazon.com/stores/page/MyStore2"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"www.amazon.com/stores/page/MyStore3}] |

### Create new Campaign with Brand video ad and Product targeting

Notes about this campaign:

* A **Brand video ad** is essentially a **Video ad** with a **Store on Amazon** as its landing page
* Notice that in the “Brand video ad” row, there is an optional field titled “Brand logo crop” (Col AY). This field allows you to apply a crop or resize to your logo, using the desired pixel values. You should leave this field blank unless you know the exact dimensions that you need—or, if you are cloning an existing campaign and want to reuse a cropped image that you have the dimensions for.
* The creative assets—including Video asset IDs and Brand asset logo IDs—can be found in a downloaded bulksheets file. When you [get a bulksheets file from the bulk operations homepage](, be sure to check the box to include “Brand assets data,” and this tab will contain all IDs from your creative assets library. You can also copy or clone your existing Sponsored Brands campaigns to create new ones using the same assets. 
* For the **Product targeting** entity row, notice that the “Product targeting expression” field indicates product *category* targeting. If you wanted *individual* product targeting, the “category=” value would be “asin=” instead.

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|Create	|Campaign example 1	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|Campaign example 1	|	|Ad group example 1	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Brand video ad	|Create	|Campaign example 1	|	|Ad group example 1	|	|	|	|	|	|My brand video ad	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/BF7D4D7A-F099-4F46-9121-70690ABAD03E	|	|Store	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My creative headline	|B0BM1YWSML, B0BM1MRMWX, B0BLZSSPL2	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Product targeting	|Create	|Campaign example 1	|	|Ad group example 1	|	|	|	|	|	|	|	|	|enabled	|	|	|	|	|	|5.69	|	|	|	|	|	|	|category="2975362011"	|	|	|	|	|	|	|	|	|	|	|	|	|

### Create an additional Ad group with a Product collection ad in an existing Campaign

Notes about this campaign:

* Notice that since this is an existing campaign, there are system-generated unique IDs for the entities that already existed. For the new Ad group and Product collection ad (in **bold**), we’ve included the parent Campaign ID to indicate that these new entities belong to this Campaign 
* For the new Ad group, we’ve used a text-based temporary ID, which will become a unique ID once the Ad group is created 
* The new Ad group and Product collection ad are the only fields that contain an “Operation” value because those are the only fields where a change is taking place. The other fields where the “Operation” is blank will be ignored when this file is uploaded
* For the “Landing page type,” you can leave this field blank, and the default will be a new landing page that we’ll create for you with the products (asins) you provide in the spreadsheet. In this example, we’ve entered “Store,” which also includes the asins (Column BB) and sub-pages (Column BE). The next example shows how to create a Product collection ad with a “Product list” instead of a “Store.” 

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|	|292142424756096	|	|	|	|	|	|Campaign Name	|	|	|20231101	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|292142424756096	|	|362003785216748	|	|	|	|	|Ad Group Name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|	|292142424756096	|	|362003785216748	|465625743252508	|	|	|	|	|Ad Name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/6F75E0CE-7A0B-4068-99F4-EAE6D1CDDD77	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My creative headline	|B00AIZBMPY, B07CMFYR77, B074MDFZB1	|	|[{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"https://www.amazon.com/stores/page/56A2B6B1-3870-4ACD-9368-B6CA6F8AA3D4"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"https://www.amazon.com/stores/page/94991DEE-76EC-490B-B636-181E4E434B2C"},{"asin":"B07RF9GMBY","pageTitle":"Women's Health","url":"https://www.amazon.com/stores/page/C6B05EF3-1EC5-4E5B-99A8-9EB2139F599D"}]	|
|Sponsored Brands	|Keyword	|	|292142424756096	|	|362003785216748	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|dog bowl	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|292142424756096	|	|New Ad Group 123	|	|	|	|	|New Ad Group 123	|	|	|	|enabled	|	|	|	|	|	|3	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|Create	|292142424756096	|	|New Ad Group 123	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My Other Headline	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|

### Create a new Campaign  with a Product collection ad that uses a Product list landing page

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|Create	|New Campaign	|	|	|	|	|	|New Campaign	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|New Campaign	|	|New Ad Group	|	|	|	|	|New Ad Group	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|Create	|New Campaign	|	|New Ad Group	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|New Headline	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|Create	|New Campaign	|	|New Ad Group	|	|	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|Cat toys	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
