---
title: Use portfolios with bulksheets
description: Learn how to use portfolios for campaign management in bulksheets
type: guide
interface: bulk-operations
tags:
    - Sponsored Products
    - Sponsored Display
    - Sponsored Brands
    - Campaign management
keywords:
    - bulksheets
    - portfolios
---

# Use portfolios with bulksheets

**Create, update, and manage portfolios in bulk**

Similar to what you might have seen in the advertising console, portfolios are groups of campaigns that you can organize to meet your advertising needs. For example, you can create portfolios to organize campaigns by brand, product category, or season to provide structure and to manage your advertising activity. 

With the “Portfolios” bulksheets tab, you can create a portfolio and set a budget that will apply to any campaign within the portfolio. To access the tab, you can [follow these steps to download and create a custom spreadsheet or download a bulk template](bulksheets/2-0/get-started-with-bulksheets-part1) from the “Bulk operations” page. After you have your spreadsheet or blank template, select the “Portfolios” tab to see the column fields below.

With the new Portfolios template, we’ve updated the first three columns to align with the new bulksheets version, to provide more parity with the API, and to make the upload process more efficient. Here’s a preview of the template fields you’ll see when you access the new Portfolios tab in bulksheets:

|[grid orange]Product	|Entity	|Operation	|Portfolio ID	|Portfolio Name	|Budget Amount	|Budget Currency	|Budget Policy	|Budget Start Date	|Budget End Date	|State (Informational only)	|In Budget (Informational only)	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|<br>	|	|	|	|	|	|	|	|	|	|	|	|

<br>
## Create a new portfolio 

To create portfolios with bulksheets, you will *always* complete the first three foundational columns: Product, Entity, and Operation. In addition to these first three fields, the only mandatory field to create a new portfolio is **Portfolio** **Name** (Column E). If you want to define a budget when you create the portfolio, you can also add those details.

Use the following guidelines to create a new portfolio:

1. Column A - Product: (Required) You must enter **Portfolios** — plural, with the **s** included
2. Column B - Entity: (Required) Enter **Portfolio**
3. Column C - Operation: (Required) Enter either **Create** or **Update**. “Archive” is not an option for portfolios.
4. Column D - Portfolio ID: (Required when updating a portfolio) Leave blank for new portfolios. If you are updating an existing portfolio, enter the actual ID.
5. Column E - Portfolio Name: (Required) Enter the name you want to assign the portfolio. You can leave blank if you are updating an existing portfolio unless you want to change the portfolio name.
6. Column F - Budget Amount: (Optional) Enter the maximum you want to spend for the date range or monthly cap, depending on which option you select in the “Budget Policy” field. Otherwise, leave blank and it will be auto-set to 0.0. You can update the budget details later using the **Update** operation and **Portfolio ID**. <br>**TIP**: For existing portfolios with a budget assigned, you can remove the budget if you update the portfolio and leave the budget field blank. This will reset the portfolio budget to null and remove it from the portfolio and its child entities.
7. Column G - Budget Currency: (Leave blank) You can leave this field blank, it will be auto-populated with your local currency. 
8. Column H - Budget Policy: (Required only if you are setting a budget) In the advertising console, this is the “Budget cap” setting where you select either a **date range** or **recurring monthly** policy. For bulksheets, you should enter either **dateRange**, **monthlyRecurring**, or **noCap** to specify no budget cap:
    - If you enter **dateRange**, you must define a start date. The end date is optional. 
    - If you enter **recurringMonthly**, you must define a start date, and the budget amount will automatically restart on the 1st of each month. The end date is optional. 
9. Column I - Budget Start Date: (Required only if you are setting a budget) Use the format yyyyMMdd (July 12, 2022 would be **20220712**)
10. Column J - Budget End Date: (Optional) You can either leave blank if you want the budget to run indefinitely, or you can enter an end date using the format yyyyMMdd (February 25, 2023 would be **20230225**). Note that the end date cannot be changed or updated once you set it.
11. Column K - State: (Leave blank) This field is informational only and will be auto-set to be **enabled** when you upload the bulksheets file to create the portfolio.
12. Column L - In Budget: (Leave blank) This field is informational only, and you can ignore it completely when creating or updating a portfolio

#### Create a new portfolio with no set budget

Using only the mandatory fields to create a new portfolio without setting a budget, the sheet would resemble this:

|[grid orange]Product	|Entity	|Operation	|Portfolio ID	|Portfolio Name	|Budget Amount	|Budget Currency	|Budget Policy	|Budget Start Date	|Budget End Date	|State (Informational only)	|In Budget (Informational only)	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Portfolios	|Portfolio	|Create	|	|Summer Sports	|	|	|	|	|	|	|	|

#### Create multiple portfolios in one sheet

You can also create multiple new portfolios in a single upload. The example below adds another portfolio to the example above, and includes budget details:

|[grid orange]Product	|Entity	|Operation	|Portfolio ID	|Portfolio Name	|Budget Amount	|Budget Currency	|Budget Policy	|Budget Start Date	|Budget End Date	|State (Informational only)	|In Budget (Informational only)	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Portfolios	|Portfolio	|Create	|	|Summer Sports	|	|	|	|	|	|	|	|
|Portfolios	|Portfolio	|Create	|	|Spring Gardening	|500	|	|dateRange	|20220225	|20220525	|	|	|

<br>

## Update an existing portfolio

You can update existing portfolios if you want to add, change, or remove the budget details or change the portfolio name. You will need the portfolio ID, which you can find if you [create and download a custom spreadsheet](bulksheets/2-0/get-started-with-bulksheets-part1) and select the “Portfolios” tab. 

#### Add or change budget details

**NOTE**: If you want to **update** a portfolio to add a new budget, the “Budget Amount,” “Budget Policy,” and “Budget Start Date” fields are required.

Let’s assume we created the “Summer Sports” portfolio in the example above, and now we want to add budget details. The bulksheets file might resemble this if you choose to set a budget amount: 

|[grid orange]Product	|Entity	|Operation	|Portfolio ID	|Portfolio Name	|Budget Amount	|Budget Currency	|Budget Policy	|Budget Start Date	|Budget End Date	|State (Informational only)	|In Budget (Informational only)	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Portfolios	|Portfolio	|Update	|10293847	|	|125	|	|monthlyRecurring	|20220327	|	|	|	|

If you wanted to adjust or modify the budget details for a portfolio, you would follow the same logic shown above. For example, if the budget is set to 125, you could enter **100** in the “Budget Amount” field to change the budget to 100.  

#### Remove budget details

You can remove the budget assigned to a portfolio by using the “Update” operation, including the portfolio ID, and leaving the “Budget Amount” field blank. This will reset the budget to 0.0 for the portfolio and all of its campaigns:

|[grid orange]Product	|Entity	|Operation	|Portfolio ID	|Portfolio Name	|Budget Amount	|Budget Currency	|Budget Policy	|Budget Start Date	|Budget End Date	|State (Informational only)	|In Budget (Informational only)	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Portfolios	|Portfolio	|Update	|10293847	|	|	|	|	|	|	|	|	|

<br>

## Create a new campaign within a portfolio

>[WARNING] **Important**: Campaigns can only be added to portfolios that already exist, and you must use the template associated with the campaign type--do not use the portfolios template to create a campaign.   

Here are the recommended steps to add a new campaign to a portfolio using bulksheets:

1. If the portfolio doesn't already exist, create the new portfolio by following the steps above 
2. Then, [create and download a custom spreadsheet](bulksheets/2-0/get-started-with-bulksheets-part1) file from the “Bulk operations” page
3. Copy the portfolio ID from the “Portfolios” tab of the downloaded file
4. Create a new campaign using the product-specific template, and include the portfolio ID in the “Portfolio ID” field of the campaign entity row 

Using the portfolio example shown above, here is what your bulksheet might look like--note that you should include the **portfolio ID** to create the campaign within a portfolio:

|[grid orange]Product	|Entity	|Operation	|Campaign Id	|Ad Group Id	|Portfolio Id	|Campaign Name	|Ad Group Name	|Start Date	|End Date	|Targeting Type	|State	|Daily Budget	|SKU	|ASIN	|Ad Group Default Bid	|Bid	|Keyword Text	|Match Type	|Bidding Strategy	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Sponsored Products	|Campaign	|Create	|Summer Campaign	|	|**10293847**	|Summer Campaign	|	|20220401	|	|Auto	|Enabled	|100	|	|	|	|	|	|	|Fixed bid	|

Note that the template above is the Sponsored Products bulksheets template. You cannot create or update a campaign using the Portfolios template. To learn more about creating campaigns, see the guides below:

* [How to create Sponsored Products campaigns](bulksheets/2-0/create-sp-campaign)
* [How to create Sponsored Brands campaigns](bulksheets/2-0/create-sb-campaign)
* [How to create Sponsored Display campaigns](bulksheets/2-0/create-sd-campaign)