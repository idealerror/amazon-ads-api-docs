---
title: Guide for keyword translations
description: Understand how to automatically translate keywords for advertisers in Egypt, Saudi Arabia, and United Arab Emirates
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Keyword translations
    - Bulksheets
    - Bulk operations
keywords:
    - bulksheets
    - translations
    - keyword translations
---

# Guide for keyword translations in bulksheets

>[NOTE] This feature is currently available in Egypt, Saudi Arabia, and United Arab Emirates only 

When you create Sponsored Products campaigns using bulksheets, you can now add keywords in your preferred or native language. The keywords will be automatically translated into English (or the default language of your locale) when you create the campaign.

## Overview

For some locales, English is the default language in the Amazon Ads system, even though the native language for that locale is not English—this includes Egypt, Saudi Arabia, and United Arab Emirates. For these locales, advertisers must use English when creating keywords, which can be challenging for advertisers who aren’t proficient in English. For this reason, we’re introducing automatic keyword translations in bulksheets. 

## How to fill out the spreadsheet

To access this feature, you’ll use two fields in your bulksheets file: **Native language keyword** and **Native language locale**. 

1. In the “Native language keyword” field, you should enter your keywords in your native language. 
2. In the “Native language locale” field, enter the [language code](#native-language-locale-codes) of your native language 

>[WARNING] You will leave the “Keyword text” field blank when using this feature—if you enter anything in the “Keyword text” field, it will override the automatic translation feature. 

As an example, let’s say you are in Egypt, Saudi Arabia, or United Arab Emirates, and your native language is Arabic. Using the fields mentioned above, your bulksheets file might resemble this:

|Keyword Text	|Native language keyword	|Native language locale	|
|---	|---	|---	|
|	<span> </span>|أطواق الكلاب	|ar_AE	|
|	|أسرة الكلاب	|ar_AE	|
|	|مستلزمات الحيوانات الأليفة	|ar_AE	|
|	|أزياء الحيوانات الأليفة	|ar_AE	|
|	|يعامل الكلب	|ar_AE	|


>[TIP] The language code for Arabic is **ar_AE**, which is why this code is used in the Arabic example above. [Review the table below for a full list of language codes](#native-language-locale-codes)

When you upload the file, the Arabic keywords would be translated into English because that is the default language locale for the Egypt, Saudi Arabia, and United Arab Emirates locales. If you download a bulksheets file later, you can see the English versions in the “Keyword text” field, which will have been automatically populated with the English translations: 

|Keyword Text	|Native language keyword	|
|---	|---	|
|dog collars	|أطواق الكلاب	|
|dog beds	|أسرة الكلاب	|
|pet supplies	|مستلزمات الحيوانات الأليفة	|
|pet costumes	|أزياء الحيوانات الأليفة	|
|dog treats	|يعامل الكلب	|

## Native language locale codes

You can also use languages other than Arabic if you prefer a different language. The table below shows the languages that are supported and the language codes you should enter in the “Native language keyword” field, depending on the language you prefer to use—the code should match the language of the keywords you enter in the “Native language keyword” field: 

|Language	|Native language locale code	|
|---	|---	|
|Arabic	|ar_AE	|
|Dutch	|nl_NL	|
|French	|fr_FR	|
|German	|de_DE	|
|Italian	|it_IT	|
|Japanese	|ja_JP	|
|Polish	|pl_PL	|
|Portuguese 	|pt_BR	|
|Spanish (Spain)	|es_ES	|
|Spanish (Mexico)	|es_MX	|
|Swedish	|sv_SE	|
|Simplified Chinese	|zh_CN	|

For example, if your preferred language is French, you could enter **fr_FR** in the “Native language locale” field and use French for the “Native language keyword” field. The French keyword text would then be translated into the default language for your locale. 

