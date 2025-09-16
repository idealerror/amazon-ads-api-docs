---
title: How to update Sponsored Brands multi-ad group campaigns with bulksheets
description: A guide explaining how to update existing Sponsored Brands v4 campaigns using bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Brands v4
    - Campaign management
keywords:
    - bulksheets  
    - update campaigns
---

# How to update Sponsored Brands multi-ad group campaigns

This article explains how to update Sponsored Brands multi-ad group campaigns in bulksheets. Generally when you update a campaign in bulksheets, the steps are similar across all campaign types: Set the “Operation” field to **Update** in the row where you want to make a change, and simply update the value. For example, to update a keyword bid, you would set the keyword row’s “Operation” field to **Update**, and then enter the new bid value in the appropriate spreadsheet cell. For general updates to campaigns—including how to change bids, budgets, and campaign names—you can follow the steps found in this [Update campaigns section of the help center](bulksheets/2-0/campaign-update-overview). 

In this article, you will learn how to update values that were not possible with previous versions of Sponsored Brands, including how to:

* Update or change a landing page URL
* Update or change a creative headline
* Add a new ad group to an existing campaign

Before you begin, [download a custom spreadsheet from the bulk operations home page](bulksheets/2-0/sb-multi-ad-groups-overview#How-to-get-a-Sponsored-Brands-multi-ad-group-file). Be sure to include the tab labeled **Sponsored Brands multi-ad group data**. Download and open the file, then review the following guidelines. 

## Downloaded file example

The table below represents a sample bulksheet with data for two different campaigns (IDs 2921424247 and 458395749). We’ll use this example file to show how to update these campaigns. Read-only fields and performance metrics are omitted to save space. Also notice that the entity IDs and statuses (“State” field) are already in the spreadsheet since we downloaded a custom file. This will make updating the campaign easier, since you don’t need to manually enter the IDs. Also notice that the “Operation” fields are all blank for each row. This will always be the case when you download a bulksheet, so you’ll need to enter a value for any row where you want to make changes. The remaining fields can be left untouched, and our system will ignore those rows as long as the “Operation” field is left blank. Keep reading to see examples of updating these campaigns. 

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|	|2921424247	|	|	|	|	|	|Campaign Name	|	|	|20231101	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|362003785216748	|	|	|	|	|Ad Group Name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|	|2921424247	|	|362003785216748	|465625743252508	|	|	|	|	|Ad Name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/6F75E0CE-7A0B-4068-99F4-EAE6D1CDDD77	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My creative headline	|B00AIZBMPY, B07CMFYR77, B074MDFZB1	|	|[{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"https://www.amazon.com/stores/page/56A2B6B1-3870-4ACD-9368-B6CA6F8AA3D4"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"https://www.amazon.com/stores/page/94991DEE-76EC-490B-B636-181E4E434B2C"},{"asin":"B07RF9GMBY","pageTitle":"Women's Health","url":"https://www.amazon.com/stores/page/C6B05EF3-1EC5-4E5B-99A8-9EB2139F599D"}]	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|362003785216748	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|dog bowl	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|529584950	|	|	|	|	|New Ad Group 123	|	|	|	|enabled	|	|	|	|	|	|3	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|	|2921424247	|	|529584950	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My Other Headline	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|529584950	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|	|458395749	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|458395749	|	|58729395	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Brand video ad	|	|458395749	|	|58729395	|49650395	|	|	|	|	|My brand video ad	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/BF7D4D7A-F099-4F46-9121-70690ABAD03E	|	|Store	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My creative headline	|B0BM1YWSML, B0BM1MRMWX, B0BLZSSPL2	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Product targeting	|	|458395749	|	|58729395	|	|	|582816463	|	|	|	|	|	|enabled	|	|	|	|	|	|5.69	|	|	|	|	|	|	|category="2975362011"	|	|	|	|	|	|	|	|	|	|	|	|	|

### Update a landing page URL

From the same example shown above, notice that the “Landing page URL” for Campaign ID 2921424247 is https://www.amazon.com/stores/page/6F75E0CE. Since the Landing page URL is defined in the **ad format** entity row (in this case, the **Store spotlight ad**), we will set the “Operation” in that row to be **Update**, and then simply enter the new landing page URL as shown below. 

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|	|2921424247	|	|	|	|	|	|Campaign Name	|	|	|20231101	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|362003785216748	|	|	|	|	|Ad Group Name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|Update	|2921424247	|	|362003785216748	|465625743252508	|	|	|	|	|Ad Name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/new-landing-page-url	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My creative headline	|B00AIZBMPY, B07CMFYR77, B074MDFZB1	|	|[{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"https://www.amazon.com/stores/page/56A2B6B1-3870-4ACD-9368-B6CA6F8AA3D4"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"https://www.amazon.com/stores/page/94991DEE-76EC-490B-B636-181E4E434B2C"},{"asin":"B07RF9GMBY","pageTitle":"Women's Health","url":"https://www.amazon.com/stores/page/C6B05EF3-1EC5-4E5B-99A8-9EB2139F599D"}]	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|362003785216748	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|dog bowl	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|529584950	|	|	|	|	|New Ad Group 123	|	|	|	|enabled	|	|	|	|	|	|3	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|	|2921424247	|	|529584950	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My Other Headline	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|529584950	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|	|458395749	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|458395749	|	|58729395	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Brand video ad	|	|458395749	|	|58729395	|49650395	|	|	|	|	|My brand video ad	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/BF7D4D7A-F099-4F46-9121-70690ABAD03E	|	|Store	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My creative headline	|B0BM1YWSML, B0BM1MRMWX, B0BLZSSPL2	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Product targeting	|	|458395749	|	|58729395	|	|	|582816463	|	|	|	|	|	|enabled	|	|	|	|	|	|5.69	|	|	|	|	|	|	|category="2975362011"	|	|	|	|	|	|	|	|	|	|	|	|	|

## Update a creative headline

Using the same example, we’ll update the “Creative headline” for the **Product collection ad**. Similar to the steps from above, the only necessary actions are to include the “Operation” value and input the new headline, because the IDs and statuses are already in the sheet:  

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|	|2921424247	|	|	|	|	|	|Campaign Name	|	|	|20231101	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|362003785216748	|	|	|	|	|Ad Group Name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|	|2921424247	|	|362003785216748	|465625743252508	|	|	|	|	|Ad Name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|[https://www.amazon.com/stores/page/](https://www.amazon.com/stores/page/6F75E0CE-7A0B-4068-99F4-EAE6D1CDDD77)new-landing-page-url	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My creative headline	|B00AIZBMPY, B07CMFYR77, B074MDFZB1	|	|[{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"https://www.amazon.com/stores/page/56A2B6B1-3870-4ACD-9368-B6CA6F8AA3D4"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"https://www.amazon.com/stores/page/94991DEE-76EC-490B-B636-181E4E434B2C"},{"asin":"B07RF9GMBY","pageTitle":"Women's Health","url":"https://www.amazon.com/stores/page/C6B05EF3-1EC5-4E5B-99A8-9EB2139F599D"}]	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|362003785216748	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|dog bowl	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|529584950	|	|	|	|	|New Ad Group 123	|	|	|	|enabled	|	|	|	|	|	|3	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|Update	|2921424247	|	|529584950	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|**New creative headling**	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|529584950	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|	|458395749	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|458395749	|	|58729395	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Brand video ad	|	|458395749	|	|58729395	|49650395	|	|	|	|	|My brand video ad	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/BF7D4D7A-F099-4F46-9121-70690ABAD03E	|	|Store	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My creative headline	|B0BM1YWSML, B0BM1MRMWX, B0BLZSSPL2	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Product targeting	|	|458395749	|	|58729395	|	|	|582816463	|	|	|	|	|	|enabled	|	|	|	|	|	|5.69	|	|	|	|	|	|	|category="2975362011"	|	|	|	|	|	|	|	|	|	|	|	|	|

## Add a new ad group to an existing campaign

To add a new entity to an existing campaign, you can start from a downloaded file like the example we’ve been referencing. But instead of simply updating values in the existing rows, we will create new rows underneath the campaign for the new ad group and its child entities. For this example, we’ll place the new ad group into the campaign with ID 458395749. 

The first step is to add at least three new rows under that campaign, as shown below. These rows are for the new ad group entity, the ad format entity, and the targeting entity. 

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|	|2921424247	|	|	|	|	|	|Campaign Name	|	|	|20231101	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|362003785216748	|	|	|	|	|Ad Group Name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|	|2921424247	|	|362003785216748	|465625743252508	|	|	|	|	|Ad Name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|[https://www.amazon.com/stores/page/](https://www.amazon.com/stores/page/6F75E0CE-7A0B-4068-99F4-EAE6D1CDDD77)new-landing-page-url	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My creative headline	|B00AIZBMPY, B07CMFYR77, B074MDFZB1	|	|[{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"https://www.amazon.com/stores/page/56A2B6B1-3870-4ACD-9368-B6CA6F8AA3D4"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"https://www.amazon.com/stores/page/94991DEE-76EC-490B-B636-181E4E434B2C"},{"asin":"B07RF9GMBY","pageTitle":"Women's Health","url":"https://www.amazon.com/stores/page/C6B05EF3-1EC5-4E5B-99A8-9EB2139F599D"}]	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|362003785216748	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|dog bowl	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|529584950	|	|	|	|	|New Ad Group 123	|	|	|	|enabled	|	|	|	|	|	|3	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|	|2921424247	|	|529584950	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My Other Headline	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|529584950	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|	|458395749	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|458395749	|	|58729395	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Brand video ad	|	|458395749	|	|58729395	|49650395	|	|	|	|	|My brand video ad	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/BF7D4D7A-F099-4F46-9121-70690ABAD03E	|	|Store	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My creative headline	|B0BM1YWSML, B0BM1MRMWX, B0BLZSSPL2	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Product targeting	|	|458395749	|	|58729395	|	|	|582816463	|	|	|	|	|	|enabled	|	|	|	|	|	|5.69	|	|	|	|	|	|	|category="2975362011"	|	|	|	|	|	|	|	|	|	|	|	|	|
|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|

Then, you can add the new entities to the spreadsheet. You can either copy/paste, or drag the values down in order to populate the existing campaign ID into the new ad group and its child entities. Complete each entity row using the same steps described in [How to create Sponsored Brands multi-ad group campaigns](bulksheets/2-0/create-sb-multi-ad-group-campaigns). Below is an example of the new ad group, which uses a **Product collection ad** ad format and a **Keyword** targeting strategy. Note that the new entities use the existing campaign’s unique, system-generated ID since the campaign already exists, but we’ve included temporary IDs for the new ad group and its child entities. This temporary ID will become a unique ID when you upload the file and create the new ad group. 

The new values are shown in **bold**. 

|A	|B	|C	|D	|E	|F	|G	|H	|I	|J	|K	|L	|P	|Q	|R	|S	|AC	|AD	|AE	|AF	|AG	|AH	|AI	|AJ	|AK	|AL	|AM	|AN	|AR	|AS	|AT	|AU	|AV	|AW	|AY	|AZ	|BA	|BB	|BC	|BE	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Product	|Entity	|Operation	|Campaign ID	|Portfolio ID	|Ad Group ID	|Ad ID	|Keyword ID	|Product Targeting ID	|Campaign name	|Ad group name	|Ad name	|Start date	|End date	|State	|Brand Entity ID	|Budget type	|Budget	|Bid optimization	|Product location	|Bid	|Placement	|Percentage	|Keyword text	|Match type	|Native language keyword	|Native language locale	|Product targeting expression	|Landing page URL	|Landing page ASINs	|Landing page type	|Brand name	|Consent to translate	|Brand logo asset ID	|Brand logo crop	|Custom images	|Creative headline	|Creative ASINs	|Video asset IDs	|Sub-pages	|
|Sponsored Brands	|Campaign	|	|2921424247	|	|	|	|	|	|Campaign Name	|	|	|20231101	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|362003785216748	|	|	|	|	|Ad Group Name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Store spotlight ad	|	|2921424247	|	|362003785216748	|465625743252508	|	|	|	|	|Ad Name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|[https://www.amazon.com/stores/page/](https://www.amazon.com/stores/page/6F75E0CE-7A0B-4068-99F4-EAE6D1CDDD77)new-landing-page-url	|	|Store	|Store spotlight brand	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My creative headline	|B00AIZBMPY, B07CMFYR77, B074MDFZB1	|	|{"asin":"B07SBHTQMC","pageTitle":"BeautyFit Store","url":"https://www.amazon.com/stores/page/56A2B6B1-3870-4ACD-9368-B6CA6F8AA3D4"},{"asin":"B0058DKFBS","pageTitle":"Cleansing & Digestion","url":"https://www.amazon.com/stores/page/94991DEE-76EC-490B-B636-181E4E434B2C"},{"asin":"B07RF9GMBY","pageTitle":"Women's Health","url":"https://www.amazon.com/stores/page/C6B05EF3-1EC5-4E5B-99A8-9EB2139F599D"}	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|362003785216748	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|2	|	|	|dog bowl	|Phrase	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|2921424247	|	|529584950	|	|	|	|	|New Ad Group 123	|	|	|	|enabled	|	|	|	|	|	|3	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|	|2921424247	|	|529584950	|	|	|	|	|	|New ad name	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722	|	|Product list	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|My Other Headline	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|	|2921424247	|	|529584950	|	|483071962064133	|	|	|	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Campaign	|	|458395749	|	|	|	|	|	|Campaign example 1	|	|	|	|	|enabled	|	|Daily	|100	|TRUE	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|	|458395749	|	|58729395	|	|	|	|	|Ad group example 1	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Brand video ad	|	|458395749	|	|58729395	|49650395	|	|	|	|	|My brand video ad	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|https://www.amazon.com/stores/page/BF7D4D7A-F099-4F46-9121-70690ABAD03E	|	|Store	|My Brand Name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|{"left":"0","top":"0","width":"450","height":"450"}	|	|My creative headline	|B0BM1YWSML, B0BM1MRMWX, B0BLZSSPL2	|amzn1.assetlibrary.asset1.735ec75f93ae1d0704411a3fc2aeff83:version_v1	|	|
|Sponsored Brands	|Product targeting	|	|458395749	|	|58729395	|	|	|582816463	|	|	|	|	|	|enabled	|	|	|	|	|	|5.69	|	|	|	|	|	|	|category="2975362011"	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Ad group	|Create	|458395749	|	|New ad group temp ID	|	|	|	|	|New ad group name	|	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
|Sponsored Brands	|Product collection ad	|Create	|458395749	|	|New ad group temp ID	|	|	|	|	|	|New product collection ad name 	|	|	|enabled	|	|	|	|	|	|	|	|	|	|	|	|	|	|**https://www.amazon.com/store/**[8667-5FF0B3678722](https://www.amazon.com/stores/page/287C0BF2-CBB8-47A0-8667-5FF0B3678722)	|	|Product list	|Another brand name	|	|amzn1.assetlibrary.asset1.38f47f3fd2fa825c5ca2d4923aead105:version_v1	|	|	|Another creative headlinne	|B087TC4NBL, B00FZN1KWE, B07B3VQ8DY	|	|	|
|Sponsored Brands	|Keyword	|Create	|458395749	|	|New ad group temp ID	|	|	|	|	|	|	|	|	|enabled	|	|	|	|	|	|0.75	|	|	|pink dog collar	|exact	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|	|
