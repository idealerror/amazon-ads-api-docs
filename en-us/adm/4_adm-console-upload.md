---
title: adm console upload
description: Ads data manager uploads
type: guide
interface: bulk-operations
---
# Data uploads using Ads data manager console

> [NOTE] This method is ideal for datasets 200MiB or smaller. For details on data preparation requirements, see [Prepare data](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data).

1. To upload a new dataset, navigate to the Datasets page, click **Upload dataset**. Or alternatively, navigate to the **Sources** page, click **Upload dataset** under File upload.
2. In the Upload file page, provide the following dataset details:
  - **Dataset name**: The dataset name can only contain letters (uppercase or lowercase), numbers (0-9), underscores (_), and dashes (-). You cannot use any other special characters or symbols in the dataset name. The dataset name cannot be longer than 100 characters.
  - **Description**: An optional description to identify the dataset you are uploading.
  - **Schema**: Select the schema type. Select **Audience** for audience records or **Events** to bring in events data. 
  All datasets have a schema in Ads data manager. A schema represents the types of attributes a given data table has. When uploading data, Ads data manager will validate the overall data quality, data collection consent, perform compliance checks, and subsequently ensure all records are valid according to the dataset's schema before completing the upload. Individual records that do not meet validation logic will be rejected and noted as “rejected” in the dataset details. The fields present in each schema indicate the dataset's applicability for certain destinations. Using an [event schema](#uploading-event-records) will guarantee that your uploaded data is eligible for use for **conversion attribution** actions. 
  **All datasets**, regardless of schema, are eligible for use in **Amazon Marketing Cloud**.

> [NOTE] Depending on the schema type you select, you can download a corresponding template to help structure your data for upload. 

3. The country you select here will be used as the default country for all records in your uploaded dataset. If a country code is provided in the mapping, it will override the selected country in the uploader.
4. Upload a CSV file (200MiB or smaller) containing the data you want to upload and make available for use in Amazon Ads. For files larger than 200MiB you'll need to use [Ads data manager APIs](guides/ads-data-manager/adm-overview). [Access a hashing utility that helps you format hashed PII for upload.](https://amazadshelp.s3.amazonaws.com/DSP/downloadable-reports/MultiAttributeHashingTool.html)
5. Click **Map attributes** to map at least one of the columns of your uploaded file to the required schema fields. While Ads data manager auto-maps the columns of your uploaded data, you must mandatorily map at least one attribute.
6. Click **Continue** to proceed. Depending on whether you are uploading audience records or event records,  follow the steps listed in the corresponding section below.

#### Uploading audience records 

1. When using the Ads data manager console, after you map the attributes, you are automatically prompted to create an audience for activation. When creating an Audience in Amazon DSP, an external audience ID required.
2. The audience name and external audience ID is prepopulated. If your Amazon DSP campaign managers use a specific naming convention for audiences, you can rename your audience here.
3. Select the **Destination** accounts linked to your manager account to share the uploaded data with. You can also click go to **access and settings** to add more advertiser accounts as destination accounts.

> [NOTE] When uploaded datasets are shared with linked accounts, those accounts are designated as “**destinations**” of the dataset.

4. Click **Submit**.

The uploaded audience dataset gets listed in the **Datasets** page, and the audiences you've shared with your linked advertiser accounts are listed in the **Destinations** page. 

> [NOTE]  You must provide a country code for all records uploaded containing a user identity. You may provide the country code at the dataset level, or at the record level.  If you upload data at both levels, your records will use the country code at the record level. For empty country code fields, Ads data manager will use the country code provided at the dataset level as a default.

#### Uploading event records 

When using the Ads data manager console, after you map the attribute, you are prompted to provide the necessary conversion definition settings to begin utilizing the uploaded event data right away.

>[WARNING] If you share a dataset for conversion attribution (using the event schema) more than once, the conversion definition name settings used previously is prepopulated and sharing settings are non-editable. This is to ensure your data is used only once for attribution and not counted for multiple purposes. 

The first time you use a dataset for conversion attribution, you must define:

- Source type to indicate where the uploaded came from 
- Conversion type to track the specific event type. 
- Value to indicate what each conversion type is worth. This value will be used if the uploaded file does not have a corresponding value indicated for the conversion type. 
- Counting method to indicate whether you're counting every conversion or only the first conversion.

Next, select the **Destination** accounts linked to your manager account to share the uploaded data with. You can also click go to **access and settings** to add more advertiser accounts as destination accounts.

> [NOTE] When uploaded datasets are shared with linked accounts, those accounts are designated as “**destinations**” of the dataset.

#### Importing datasets from existing upload sources

If you previously uploaded data to a specific Advertiser Account via the **Advertiser Audience API** or **Conversions API**, you may import the data in Ads data manager if you are an Editor or Admin of the Manager Account that is the parent of your advertiser account (linked via Admin permissions), you will see eligible datasets to import into the data manager datasets view.

![Import_datasets](/_images/ads-data-manager/adm_import_datasets.png)

First [establish a connection with child/children accounts](adm/2a_ads-data-manager_account_setup).  Then, initiate the migration of existing datasets uploaded data through the Advertiser Audience API or Conversions API into Ads Data Manager.

> [NOTE] To migrate datasets, your Advertiser Account must maintain an admin link with the manager account through the **Account access and settings** options of the Ads console.

Follow the steps listed below to migrate previously uploaded datasets to ads data manager:

1. In the Ads data manager console, navigate to the Datasets page.
2. Under the migrate datasets section, locate the datasets  that were uploaded using either the Conversions API or the Advertise Audience  API. 
3. To verify the dataset names, click the link under the Dataset  column.
4. Click **Import** to migrate the datasets. All imported datasets will appear in the ads data manager, under the Datasets page.

![Imported_datasets](/_images/ads-data-manager/adm_imported_datasets.png)

