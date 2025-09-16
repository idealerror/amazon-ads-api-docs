---
title: Ads data manager
description: Upload data to ads data manager API
type: guide
interface: api
---
# Dataset management

Datasets represent a grouping of uploaded advertiser data in the Ads data manager. A dataset closely resembles a data table, comprising attributes (columns) records (rows) as well as metadata, such as the dataset name and the schema template utilized to create the dataset.

You can choose to bring in your data programmatically using the Ads data manager APIs or use the [Ads data manager console](adm/1_ads-data-manager-console-overview) to upload data. The APIs allow you to send your data via JSON files. This method is perfect for large-scale, frequent data uploads and can be integrated into your existing data pipelines.

Datasets comprise of all data brought to Ads data manager, including [audiences](#upload-audience-data) and [events](#upload-event-data).

> [NOTE] Ads data manager will soon supporting datasets with custom schemas. 

## Upload audience data

To programmatically upload audience data to Ads data manager, you will need to perform the following steps:

- [Create an audience dataset](#create-an-audience-dataset)
- [Ingest audience data](#ingest-audience-data-to-the-dataset)

### Create an audience dataset

> [NOTE] In this tutorial, we are creating an audience dataset, hence using the Audiences APIs.

Utilizing datasets, you can create advertiser audiences directly as well as analyze granular details such as record counts.
The audience dataset API will create datasets with the audience schema. You must use the `dataSets` API to update or delete a dataset.

1. Call the `POST/adm/audiences` endpoint to create a new audience dataset with the following header parameters:

| Header parameter                | Description                                                             |
| ------------------------------- | ----------------------------------------------------------------------- |
| Amazon-Advertising-API-ClientId | The identifier of a client associated with an Amazon Developer account. |
| Amazon-Ads-Manager-Account-ID   | Id of the manager account.                                              |
| Amazon-Ads-AccountId            | Id of the advertiser account.                                           |
| Authorization                   | Login with Amazon token in the form of Bearer {token}.                  |

2. In the request body, provide the following details:

  - **countryCode**: the 2-character string in the ISO 3166 format that indicates from which country the data was collected.

> [NOTE] You must provide a `countryCode` for all records uploaded containing a user identity. You may provide the `countryCode` at the `dataSet` level, or at the record level. If you upload data at both levels, your records will use the `countryCode` at the record level. For empty `countryCode` fields, Ads data manager will use the `countryCode` provided at the `dataSet` level as the default value.

  - **Name**: a description for the audience dataset you are creating.

  - **Idretention**:   (reserved for future support) the retention of hashed data for 90 days. Provide a Boolean value (TRUE or FALSE) to indicate your retention preference.
  - **description**: a readable description for the audience data set you are creating.
  - **ttl**:  (reserved for future support) the time-to-live value in seconds, which is the amount of time the record is associated with the `dataSetId`, the maximum `ttl` is 12.5 months.

**Sample request body**

```
{
  "countryCode": "US",
  "name": "<string>",
  "description": "<string>"
}

```

**Sample response**

```
{
  "code": "string",
  "dataSetId": "string",
  "details": "string",
  "message": "string",
  "errors": [
  {
    "errorType": "string",
    "errorMessage": "string",
    "errorCode": 0
  }
 ]

}

```

3. A successful call will return a `dataSetId`. Capture the `dataSetId` to use it in further API calls, when uploading records to a dataset.
4. Use the `GET/adm/audiences` or `GET/adm/audiences/{{dataSetId}}` endpoint to verify if the dataset was created successfully.

Now your dataset with a **audience schema** has been defined and you can proceed with uploading data to it.

### Ingest audience data to the dataset

Audiences are treated the same as general datasets, but use a pre-defined schema template meant for ingesting audience records. Any dataset that contains PII or ExternalID data is required to include a consent signal (either GPP, TCF String, or Amazon Consent Signal). Populate the audience dataset by calling `POST /adm/audiences/{{dataSetId}}/members` and providing the user records in the request body. Include a consent string in your request for each member you submit.

1. Use the `dataSetId` returned from the `POST /adm/audiences` endpoint using which you had [created the dataset](#create-an-audience-dataset).

2. Verify the records ingested by calling the upload status endpoints `GET/adm/audiences/{dataSetId}`.

See [how to format hashed PII for identity matching](https://advertising.amazon.com/help/GCCXMZYCK4RXWS6C)
For better match rates, include as many hashed identifiers as possible in your first-party data. If you have multiple personally identifiable information (PII) sets for a user, include them in the same member object. If using a third-party identity provider, include their external identifier as a standalone match key or combine it with your PII data.

3. For each member of the audience you are uploading, provide the following details in the request body

- `userConsent`: For each record, the `userConsent` is collected as two components: `geo` and  `consent`.

  - `geo` is composed of the following parameters:

    - The country code is 2-character string in the ISO 3166 format that indicates from which country the data was collected (for example, US or GB).  Note, the `countryCode` supplied for consent management is not the same as the `consentCode` used for hashed PII identity matching. For details on the resolution priority for country codes, see  [Country code resolution priority.](guides/ads-data-manager/adm-consent-management#country-code-resolution-priority)

    - `ipAddresss`: you can optionally include an `ipAddress` to determine the country for members in this audience.

  - `consent`: Provide your consent sharing format. For more details, see [Providing consent signals](guides/ads-data-manager/adm-consent-management#providing-consent-signals).

  - `action`: Specify whether you want to add members to the audience ("CREATE") or removing members ("DELETE") from the dataset.

  - `userIdentity:`For each member uploaded, provide the `externalidentities `to indicate the source of the user record and `hashedPII` fields. See [how to format hashed PII for identity matching](https://advertising.amazon.com/help/GCCXMZYCK4RXWS6C)

  For better match rates, include as many hashed identifiers as possible in your first-party data. If you have multiple personally identifiable information (PII) sets for a user, include them in the same member object. If using a third-party identity provider, include their external identifier as a standalone match key or combine it with your PII data.

>[WARNING] The size of the file for upload cannot exceed 10 MB. We recommend each request contain 5000 records.

Verify the records ingested by calling the upload status endpoints `GET/adm/audiences/{dataSetId}`.

## Upload event data

Ads data manager also allow you to bring in your event data from multiple online and offline sources directly to Amazon's demand-side Platform ( Amazon DSP).  The [Conversions API](dsp-conversion-builder) allows you to pass such conversion events from online or offline sources, which are directly accessible for diagnostics, sharing, and management. Once uploaded, you can use these events for conversion attribution reporting, and use in Performance+ or Brand+ campaigns.

Any measurable action that can be tracked across digital and physical spaces is referred to as an event. Events include online behaviors like website page views, sign-ups, purchases, and cart additions; mobile app activities such as installations, or in-app purchases; or offline actions like retail transactions.  At attribute provides additional context about the event. Examples of attributes are currency type,  value, units sold, or any product specific like color or size. 

To upload event data to Ads data manager, you must first [define a conversion rule](#define-conversion-events), after which you [ingest event datasets](#ingest-conversion-events) to your dataroom. 

### Define conversion events

1. Send a POST request to the `/accounts/{accountId}/dsp/conversionDefinitions` endpoint.

2. Include the following headers:

 - `Amazon-Advertising-API-ClientId`: The identifier of a client associated with a Login with Amazon account.
 - `Amazon-Advertising-API-Scope`: The identifier of a profile associated with the advertiser account.
 - `Amazon-Ads-AccountId`: The identifier of the DSP advertiser account.

3. Create conversion definition rules to specify how these imported events should be counted and reported on. To do so, in the request body, provide an array of up to 10 conversion definition objects, each containing the following fields:

 - `name`: Provide a name for the conversion definition.
 - `source`: Specify the source as either "AMAZON\_AD\_TAG" or "SERVER\_TO\_SERVER".
 - `sourceType`: Specify the source type, such as "ANDROID", "FIRE\_TV", "IOS", "OFFLINE", or "WEBSITE".
 - `conversionType`: Select the conversion type, such as "ADD\_TO\_SHOPPING\_CART", "APPLICATION", "CHECKOUT", etc.
 - `countingMethod`: Specify whether to count "EVERY" conversion event or only the "FIRST" one per user per 24-hour period.
 - `value`: Provide a value for the conversion event.

4. A successful request will return a `conversionDefinitionId`. When importing event data, this ID will be used to map the data to the conversion definition.

### Ingest conversion events

After you successfully define your conversion events,  import your event data by making a POST request to the `/accounts/{accountId}/dsp/conversionDefinitions/eventData` endpoint. 

Include the following headers:

 - `Amazon-Advertising-API-ClientId`: The identifier of a client associated with a Login with Amazon account.
 - `Amazon-Advertising-API-Scope`: The identifier of a profile associated with the advertiser account.
 - `Amazon-Ads-AccountId`: The identifier of the DSP advertiser account.

In the request body, include the `source` used to import the event and the `eventData` array containing one or more conversion event objects. Each conversion entry object in `eventData` must include the following mandatory parameters:

- `name`: The name of the  imported event.   
- `conversionDefinitionID`: The id of the  associated conversion definition returned from the POST operation of the `/accounts/{accountId}/dsp/conversionDefinitions`.
- `timestamp`: The reported  timestamp of when the event occurred. The timestamp can be up to 7 days before  you send an event. Greater than 7 days latent data won't be processed.   
- `matchKeys`: An array of customer/device identifiers, hashed using SHA-25 that accepts the `type` and `value` for attribution. 

>[NOTE] Hash any user-level identifiers (e.g., email, phone number) must be normalized and hashed using SHA-256 before sending them in the `matchKeys` array.  


You can also provide values for the following optional parameters in the request body:  
   
- `clientDedupeId`: The advertiser-specified unique identifier used to deduplicate conversion events. Only the first event with a given ID is processed; subsequent events with the same ID are ignored. 

>[WARNING] We recommend submitting this value for all integrations to avoid double counting.

- `value`: The value of the  event you are defining.   
- `currencyCode`: The ISO-4217 currency code for event value (specified above). Required only for `OFF_AMAZON_PURCHASES` conversions. If omitted, it defaults to the conversion definition's currency code .
- `unitsSold`: The quantity of items purchased. Applies only to `OFF_AMAZON_PURCHASES` conversions. Defaults to 1 if not specified.
- `dataProcessingOptions`: Flag to indicate how the event must be processed. Events marked as `LIMITED_DATA_USE` will be excluded from processing.
   
A successful request will return a 207 multi-status response with information about the successful and failed conversions. 

## Share datasets

A sharing rule specifies the applications (destinations) with which a dataset will be shared. Currently supported applications are listed in the table below, with their corresponding application IDs. More applications will be added in the future.

| Destination Name          | Description                                                                               | Application ID  |
| ------------------------- | ----------------------------------------------------------------------------------------- | --------------- |
| Amazon DSP Audiences      | Used for Amazon DSP audience remarketing,  suppression, or look-a-like audience creation. | "DSP\_AUDIENCES" |
| Amazon DSP Events Manager | Used for conversion attribution within  Amazon DSP standard reports.                      | "EVENTS\_MANAGER"|
| Amazon Marketing Cloud    | Used to perform event-grained clean room  analysis.                                       | "AMAZON\_MARKETING\_CLOUD"      |

### Create sharing rules to share datasets with linked advertiser accounts (destination)

Sharing rules allow you to share current datasets with linked accounts. You can also create new sharing rules to make datasets available for use in advertiser accounts for a specific application.

To share uploaded datasets with advertiser accounts associated with a destination accounts.

1. Create a sharing rule by calling `POST/adm/sharingRules` with the following request body.

**Sample request body**

```
{

 "applicationId": "DSP_AUDIENCES",
 "dataSetId": "<string>",
 "destinationAccountId": "<string>",
 "destinationEntityId": "<string>",
 "marketplaceId": "<string>",
 "metadata": {
    "audienceMetadata": {
      "audienceName": "<string>",
      "externalAudienceId": "<string>",
      "description": "<string>"
   	 }
 	}

}

```

2. Verify the sharing rule was created by sending a `POST/adm/sharingRules/list` request.
3. The shared audience dataset should now be visible in the destination account.
   The response lists all the sharing rules created for your manager account, with the following details:

   - currently supported applications
   - date and time before which  the sharing rule was activated
   - date and time after which  the sharing rule was activated
   - the dataset id
   - status of the sharing rule
   - destination account id

> [NOTE] Sharing rules will synchronize all current records within a dataset, as well as keep the destination application in sync with future data uploaded. For Audiences, the initial sync will take some time depending on the current size of the dataset. We recommend waiting at least 24 hours for initial syncs, whereas incremental records will be synchronized within one hour of upload. For Event data, data should be reflected within two hours in Events Manager's record statistics.  For AMC, data will be available within two hours of upload, and immediately after sharing if the upload was processed beforehand. Note that applications may have added time for refreshing their UIs or calculating derived outputs, such as reports or audience sizes.

## Delete a dataset from Ads data manager

You can remove your uploaded data from Ads data manager in two ways:

1. Delete the entire dataset using [DELETE /adm/datasets/{dataSetId}](https://advertising.amazon.com/API/docs/en-us/ads-data-manager#tag/Datasets/operation/DeleteDataset). This will stop sharing the data with all destinations. The data will either stop being updated or will be completely removed from those destinations.
2. Stop sharing with specific destinations by calling `PATCH /adm/sharingRules/{sharingRuleId}/revoke`. You can control the dataset's access and choose the accounts or destinations to which you want to share your data.

See the table below for additional details:

Destination | Action | Result 
---|---|---
 **Amazon DSP Audiences** | Revoke destination sharing rule | The specific Amazon DSP advertiser account which has been shared this audience will no longer receive updates\.  
 **** | Delete dataset | All Amazon DSP advertisers who have been shared this audience will no longer receive updates\.  
 **Amazon Marketing Cloud** | Revoke destination sharing rule | All AMC instances who have been shared this audience will no longer receive updates and the dataset will no longer be visible in all AMC instances\. 
 **** | Delete dataset | The specific AMC Instance which has been shared this dataset will no longer receive updates, and the dataset will no longer be visible in the AMC instance for querying\. Data deleted will be hard deleted from ADM and AMC and is non\-recoverable 
 **Amazon DSP Conversion Attribution \(Events Manager\)** | Revoke destination sharing rule | The Amazon DSP Account will no longer see the Conversion Definition\(s\) associated to this dataset for attribution purposes\. NOTE: Certain shares are non\-revocable if created and shared through the Conversions API\. Only sharing initiated directly through the data manager UI or APIs are revocable\. 
 **** | Delete dataset | All Amazon DSP Accounts will no longer see the Conversion Definition\(s\) associated to this dataset for attribution purposes\. NOTE: Certain datasets cannot be deleted if created and shared through the Conversions API\. Only sharing initiated directly through the data manager UI or APIs are revocable\. 


## De-identifying a user (GDPR compliance)

To de-identify a user, use [POST
/adm/identities/delete](https://advertising.amazon.com/API/docs/en-us/ads-data-manager#tag/Identity-Deletion)

## Removing a user from an Audience 
To remove a user from an audience, use [POST
/adm/audiences/{dataSetId}/members](https://advertising.amazon.com/API/docs/en-us/ads-data-manager#tag/Audiences/operation/IngestAudiences) with an "ACTION" of DELETE.

> [NOTE] The API call must contain a "GRANTED" Amazon Consent Signal or it will be ignored.
