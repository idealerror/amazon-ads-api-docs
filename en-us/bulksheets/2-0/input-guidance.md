---
title: Input guidance for Sponsored Products 
description: A guide explaining how to use input guidance to help you navigate bulksheets more intuitively.
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - get started
    - bulksheets  
    - input guidance
---

# Excel input guidance for bulksheets

>[NOTE] The input guidance feature is currently available for Sponsored Products only, and is limited to Excel spreadsheets. <br><br> Also note that if your spreadsheet is very large and/or if you have multiple files open at the same time, you might experience some lag when navigating the input guidance. 

Excel input guidance is a feature in bulksheets that helps you understand what you should enter in the spreadsheet fields, and which fields are required for each campaign entity. This article describes how to access and use this feature. 

>[WARNING] If you paste values into the spreadsheet, this can cause the input guidance feature to stop working properly. If you do want to paste values into the sheet, you should [select “Paste Special” in Excel, and paste the **values only**](https://support.microsoft.com/en-us/office/paste-options-8ea795b0-87cd-46af-9b59-ed4d8b1669ad)**.**  

### How to access the input guidance feature 

| [IMAGE] |   |
| ---- | ---- |
|To access this feature, go to [the bulk operations main page](https://advertising.amazon.com/bulksheet/HomePage):<br><br>1. Under "1. Create & download a custom spreadsheet," select a date range. <br>Note that the date range is for performance metrics, and the range will not affect the input guidance feature. <br><br>2. For the feature to appear, you must select the following two checkboxes from the "Include" list: **Sponsored Products data** and **Guidance for Sponsored Products inputs [BETA]** →  <br>Note that you can include other selections as well, but these are the two that are relevant for this feature. <br> <br>3. To get a spreadsheet pre-populated with your current campaigns, click **Create spreadsheet for download.** <br>If you want a blank template without any campaign data, click the link that reads "Download a bulk operations template" instead. **→**| ![Data selection for bulksheets](/_images/bulksheets/2-0-images/create-download-sheet.png) | 
<br>4.  Scroll down to the "Bulk spreadsheet history" section to download the file <br> <br>5. Open the file and go to the "Sponsored Products" tab in the spreadsheet.

<br>For more details on getting a bulk spreadsheet with the data you want to see, refer to the [bulksheets getting started guide](bulksheets/2-0/get-started-with-bulksheets-part1#how-to-create-and-download-a-custom-spreadsheet)
<br>

### Column header hints

| [IMAGE] |   |
| ---- | ---- |
| The spreadsheet will generally look the same as it does without this feature, but note that the top header row, along with the first three columns, are frozen in place so you can always see those fields even if you scroll. <br><br>You’ll also see hints about each field if you click into any column header. For example, if you click on the “start date” column header, you’ll see that the value must use the format of YYYYMMDD. **→** | ![Depiction of hint in Start Date column header](/_images/bulksheets/2-0-images/ColumnHeaderHints.png) |
<br>If you enter the value in a different format, you will see a warning dialog with an additional hint about what the format should be.<br>

### Dropdowns in some fields

| [IMAGE] |   |
| ---- | ---- |
| In addition to the column header hints, you'll see dropdowns for fields where the required values are finite. Fields with dropdowns include <br>- Product<br>- Entity<br>- Operation<br>- Targeting type<br>- State<br>- Match type<br>- Bidding strategy<br>- Placement <br>You can see the dropdowns if you click into one of the fields listed above. Allowable values will appear in the list, as shown in this example of the "Entity" field **→** | ![Depiction of dropdown menu in Entity field](/_images/bulksheets/2-0-images/dropdowns.png) |

>[TIP] If you scroll horizontally toward the end of the sheet, the dropdown arrow for the “Operation” field might seem to disappear. To make the dropdown arrow reappear, scroll back over to the beginning of the sheet. 

### Highlights to show required and optional fields 

In each row, after you select values for the first three columns (Product, Entity, Operation), cells will automatically highlight to show which fields are required and optional. Required fields will highlight dark blue, and optional fields will be lighter blue. Cells that have no highlighting should remain blank if you are using a blank template. If you’re using a custom downloaded sheet, the cells should be left untouched, as the values in unhighlighted cells cannot be changed.

The image below shows examples of highlights for the “Campaign” and “Ad Group” entity rows when using the “Create” operation. This example shows a blank template, with no campaign data populated. 

>[NOTE] All bulksheets files will also contain fields with gray highlights, which are not pictured in the image below. These highlights indicate fields that are informational only, so you should not type any values into these cells. 

![Depiction of highlighted fields](/_images/bulksheets/2-0-images/HighlightedFields.png)

Based on these highlights, the **required** fields to create a new **campaign** are

* Campaign ID
* Campaign Name
* Start Date
* Targeting Type
* State
* Daily Budget
* Bidding Strategy

Portfolio ID and End Date are both **optional** fields. These highlights will change automatically depending on which entity and operation you select—for example, notice that the image above shows different cells highlighted for the “Ad Group” entity row when compared with the “Campaign” entity row. 

Learn more about [managing Sponsored Products campaigns using bulksheets](bulksheets/2-0/get-started-with-sp-bulksheets)

