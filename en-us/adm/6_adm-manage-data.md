---
title: Ads data manager 
description: Manage data and datasets.
type: guide
interface: bulk-operations
---
# Activate data in destination platforms

Once the data upload is complete using any of the methods Ads data manager supports, the uploaded files show up in the Datasets tab within Ads data manager console. You can do the following with your uploaded data: 
 
 - [View datasets](#view-datasets)
 - [Create advertiser audience](#create-advertiser-audience)
 - [Attribute off-Amazon conversions](#attribute-off-amazon-conversions)
 - [Share data with Amazon Marketing Cloud](#share-data-with-amazon-marketing-cloud) 


## View datasets

You can view the dataset details you uploaded by clicking the dataset name. The page also lists the schema, source from which the dataset was uploaded, the number of active destinations of the dataset, records and the date the dataset was created date.

## Create advertiser audience

You can create an audience using **Actions** from the Datasets or dataset detail tab and then selecting **Create advertiser audience** from the Action menu associated with the dataset.

You should be able to see all accounts linked to your manager account in the "Create Audience" page.

> [NOTE] In the current release, we've added support for the audience schema. In subsequent releases, we will extend support for conversion events and custom schemas.

From the "Datasets" tab, you can also click on the dataset that represents the uploaded audience dataset. The Dataset details page that appears provides more granular details, including destinations, counts and match rates, Dataset ID, Dataset Details (Name, Description, etc.) and all counts in the top right: Accepted Records, Rejected Records, Match rates. Once your audience is created, you will find your advertiser audience in your Amazon DSP advertiser account under "Advertiser Audiences".

## Attribute off-Amazon conversions

Ads data manager also offers the ability to use event data from sources outside of Amazon for conversion attribution in Amazon DSP campaigns. Any data shared for conversion attribution will also be used in Performance+ and Brand+ campaigns, if this option is selected in Amazon DSP.

To attribute event data to off-Amazon conversions, after your event data is uploaded, select **Attribute off-Amazon conversion** either by using **Actions** from the Datasets page or navigate to the dataset's detail tab and then select **Attribute off-Amazon conversion.**

The first time you use a dataset for **conversion attribution**, you must define:

- Conversion definition name  
- Source type to indicate where the uploaded came from 
- Conversion type to track the specific event type. 
- Value to indicate what each conversion type is worth. This value will be used if the uploaded file does not have a corresponding value indicated for the conversion type. 
- Counting method to indicate whether you're counting every conversion or only the first conversion.

If you have already used a dataset for conversion attribution, secondary shares will inherit the settings selected for your initial share.  

>[NOTE] You cannot edit the conversion settings in Ads data manager after creating it for a dataset.  

Now, select the **Destination** accounts linked to your manager account to share the uploaded data with. You can also click go to **access and settings** to add more advertiser accounts as destination accounts.

> [NOTE] When uploaded datasets are shared with linked accounts, those accounts are designated as “**destinations**” of the dataset.

## Share data with Amazon Marketing Cloud 

You can share uploaded data with your Amazon Marketing Cloud (AMC) instance. 

>[NOTE]  To share datasets with AMC, you must add linked instances with data sharing permissions. Talk to your account executive or your partner manager. 


To share uploaded data with AMC, after your event data is uploaded, select **Share with Amazon Marketing Cloud** either by using **Actions** from the Datasets page or navigate to the dataset's detail tab and then select **Share with Amazon Marketing Cloud*.**

Now, select the **Destination** instance to which the data will be shared with.

Navigate to AMC. The uploaded dataset will appear under the Advertiser Uploaded section of the schema editor.   

>[NOTE] When uploaded datasets are shared with linked accounts, those accounts are designated as “**destinations**” of the dataset.

## Remove data from Ads data manager

You can remove your uploaded data from Ads data manager in two ways:

- **Delete the whole dataset**
  This will stop sharing the data with all destinations. The data will either stop being updated or will be completely removed from those destinations. This varies per destination.
- **Stop sharing with specific destinations**
  You can control the dataset's access and choose the accounts or destinations to which you want to share your data. Go to the "Destinations" tab or the dataset details page. Next to each destination, you'll see an option to "deactivate" it. Click "deactivate destination" to stop sharing your data with that particular destination, while still sharing it with others.

>[NOTE] For destinations you have shared to via the Advertiser Audience API or the Conversions API, you will not be able to disable sharing from the Ads data manager console. You may only disable sharing of datasets you originally shared from the Ads data manager console. 


 For further details, see the table below.

 Destination | Action | Result 
---|---|---
 **Amazon DSP Audiences** | Revoke destination sharing rule | The specific Amazon DSP advertiser account which has been shared this audience will no longer receive updates\.  
 **** | Delete dataset | All Amazon DSP advertisers who have been shared this audience will no longer receive updates\.  
 **Amazon Marketing Cloud** | Revoke destination sharing rule | All AMC instances who have been shared this audience will no longer receive updates and the dataset will no longer be visible in all AMC instances\. 
 **** | Delete dataset | The specific AMC Instance which has been shared this dataset will no longer receive updates, and the dataset will no longer be visible in the AMC instance for querying\. Data deleted will be hard deleted from ADM and AMC and is non\-recoverable 
 **Amazon DSP Conversion Attribution \(Events Manager\)** | Revoke destination sharing rule | The Amazon DSP Account will no longer see the Conversion Definition\(s\) associated to this dataset for attribution purposes\. NOTE: Certain shares are non\-revocable if created and shared through the Conversions API\. Only sharing initiated directly through the data manager UI or APIs are revocable\. 
 **** | Delete dataset | All Amazon DSP Accounts will no longer see the Conversion Definition\(s\) associated to this dataset for attribution purposes\. NOTE: Certain datasets cannot be deleted if created and shared through the Conversions API\. Only sharing initiated directly through the data manager UI or APIs are revocable\. 