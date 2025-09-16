---
title: Advertiser audiences
description: How to use AMC advertiser audiences
type: guide
interface: api
---
# Advertiser audiences

The Advertiser audiences API allows you to create advertiser first-party audiences in AMC and Amazon demand-side platform (Amazon DSP) by allowing you to create an audience segment, and stream your audience members into AMC and Amazon DSP.

## Before you begin

To use the Amazon Ads Advertiser audiences APIs, you must:

- **Apply for permission to access the API.** The application form includes questions about how your business intends to use the Amazon Ads API, as well as information about the [Amazon Ads API License Agreement](https://advertising.amazon.com/API/docs/license-agreement), the [Data Protection Policy](https://advertising.amazon.com/API/docs/policy/en_US), and [Amazon Marketing Cloud Terms](https://advertising.amazon.com/marketing-cloud/terms/AMC_Terms.html). Visit [the Amazon Ads Partner page](https://advertising.amazon.com/partner-network/register-api?ref_=a20m_us_api_drctad) for information on how to apply for access to the API.
- **Create a Login with Amazon (LwA) application** and submit your application Id (Client ID) for approval. After approval, the Amazon Ads API will handle authentication, authorization, and routing of requests to the appropriate entity or instance. For more information, see the [Create a Login with Amazon application](guides/onboarding/create-lwa-app).
- **Obtain approved access for the Amazon Ads APIs**, with the "Campaign Management" and the "Data Provider" scope associated with your developer credentials (Client ID). Request approval through your [Amazon Ads Partner Network account](https://advertising.amazon.com/partners/network). (Or, submit a [Jira](guides/onboarding/data-provider-api) with your request to approve your access with the Data provider scope).

## Limitations

- To manage Amazon DSP audiences, use the appropriate regional endpoint depending on the region of your Amazon DSP Advertiser Account  (NA/EU/FE). Audiences will be sent to connected AMC instances regardless of endpoint region used.
- To perform an audience upload to AMC, the AMC Terms must be accepted by the user who provided the OAuth2 grant. See the API reference section on **[AMC Terms](#amc-terms-apis)** for more information.
- When using the Advertiser audiences API, only successfully matched audience members will be sent to Amazon Ads.

## Advertiser audiences API

1. To use the **Advertiser audiences API**, you will have to specify the `targetResource` you’re uploading to. You may use either a `connectionId` or an `advertiserId`.
   - Use of the `connectionId` allows audiences to be uploaded to AMC or synchronized with the Amazon DSP.
   - Use of a standalone `advertiserId` will result in audiences being sent to the Amazon DSP only.

> [NOTE] For more details on how to retrieve your advertiser's `connectionId`, see the [ConnectionId API](#connection-apis) request/response section below.
> For details on how to retrieve your advertiser's `advertiserId`, refer to [Retrieve a profile ID](guides/get-started/retrieve-profiles) and [Amazon DSP advertisers](https://advertising.amazon.com/API/docs/en-us/dsp-advertiser/#/Advertiser/get_dsp_advertisers) API reference.

2. To upload to AMC, users must provide consent and agree to AMC's terms and conditions to be able to upload external datasets. For details, see [AMC Terms](#amc-terms-apis).
3. Create one or more audiences using the audience metadata API `/amc/audiences/metadata`.

   - The metadata API is synchronous and allows a single audience to be created for a given invocation.
     - The `audienceId` returned will be used in the Amazon DSP audience activation API `/amc/audiences/records` using the POST method. Hashed records in the request payload must be hashed via SHA-256 encryption. Similar to the metadata API, this API also returns a `jobRequestId` after a successful invocation.
     - The `jobRequestId` returned in the audience activation API can be used to query the status of the job. When a job status returns with a type of SUCCEEDED or FAILED, subsequent polling must be stopped to preserve the transaction per second (TPS) limit consumption and API performance.

> [NOTE] If the Audiences API is invoked with an `audienceId` that was not created using `/amc/audiences/metadata` API, the audience activation job will invalidate this audience.

### Country code

The country code is 2-character string in the ISO 3166 format that indicates from which country the data was collected (for example, US or GB).

Although this is an optional field, it is _highly recommended_ that you specify a country code when uploading data.

| [TILES] Did you know?                                                                                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| For any data you upload that does not specify a country code, AMC will automatically default to a country code of `UNKNOWN`. This will result in lower match rates, and lower usability for data provided to our APIs. |

Therefore, it is strongly encouraged to include country codes in your first-party datasets. For data you upload into a DSP Advertiser Account, records will be defaulted to the marketplace of the DSP Advertiser Account in which your audience has been created.

The country code can be set on the **record-level** or **audience metadata-level**.

If you set the country code at the:

* **Record-level**: This allows you to define a country code for each individual record. Refer to the API reference for [POST /amc/audiences/records](amc-advertiser-audience#tag/Audience-Records/operation/ManageAudienceV2).

> [NOTE] A country code added at the record-level will supersede a country code added at the Audience level when there is a conflict.

* **Audience metadata-level**:  This allows you to apply a country code for all records within the audience. Refer to the API reference for [POST /amc/audiences/metadata](amc-advertiser-audience#tag/Audience-Metadata/operation/CreateAudienceMetadataV2).

> [NOTE] If no Audience-level country code is provided, but an audience is created in an Amazon DSP Advertiser Account, we use the marketplace of the Amazon DSP Advertiser Account as the default.

### Audience membership

Advertiser audiences sent to Amazon DSP under `Advertiser audiences` filter in the Audiences section of your Advertiser account.

Advertiser audiences sent to AMC will appear as a dataset in the schema explorer named:

`{source-name}_audience_members_{unique-string}`

Here, `source_name` is the name of your LwA application.

This dataset contains the current membership by `audience_id`. To query the members of a given audience, use the following query example:

```
SELECT COUNT(DISTINCT user_id) as matched_users
FROM example_audience_members_9c5b8b23
WHERE audience_id = 0000000000000000000
GROUP BY audience_id
```

> [NOTE] For audiences to be active for use in Amazon DSP, you must have at least 2,000 active audience members. Only matched audience members will be queryable in AMC.

#### Dataset schema definitions

| Column name       | Type      | Description                                                                                                                              |
| ----------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| audience\_id      | String    | Amazon DSP audience Id\.                                                                                                                 |
| insert\_date\_utc | Timestamp | The date on which the request to add or remove audiences to AMC was made\.                                                               |
| user\_id          | String    | This field contains a user\_id that can be joined across ad\-exposure event data within AMC\. It is not exportable in workflow outputs\. |
| user\_type        | String    | The resolved user’s identifier type\.                                                                                                   |

### Audience additions and removals

This dataset will show up in the AMC UI schema explorer as:

`{source-name}_audience_logs_{unique-string}`

>[NOTE] Audience logs represents the historical changes to audiences, showing the specific add and remove actions performed.

This dataset contains the add/remove transaction logs by `audience_id`. You may use this table to query the specific members of an audience on a specific date within the last 30 days. The following query example shows how to count the members of all audiences from before June 2023:

```
WITH ranked_updates AS 
(
    SELECT user_id, audience_id, action, user_type, insert_date_utc, ROW_NUMBER()
    OVER( PARTITION BY user_id, audience_id ORDER BY insert_date_utc  DESC) AS ranked_audiences 
    FROM TABLE(EXTEND_TIME_WINDOW(example_audience_logs_9c5b8b23','P397D','P0D')))  
SELECT COUNT(DISTINCT user_id) as user_count, audience_id 
FROM ranked_updates 
WHERE ranked_audiences = 1 
AND action <> 'DELETE'
AND insert_time_in_millis < CAST('2023-06-30' AS DATE)
GROUP BY audience_id
```

#### Dataset schema definitions:

| Column name         | Type      | Description                                                                                                                              |
| ------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| action              | String    | Action indicating to add or remove the userId from audiences\. Values include: CREATE, DELETE\.                                          |
| audience\_id        | Number    | Amazon DSP audience Id\.                                                                                                                 |
| hashed\_address     | String    | Audience member’s hashed address\. This field is used for user\_id resolution and is subsequently dropped in AMC\.                      |
| hashed\_city        | String    | Audience member’s hashed city\. This field is used for user\_id resolution and is subsequently dropped in AMC\.                         |
| hashed\_email       | String    | Audience member’s hashed email\. This field is used for user\_id resolution and is subsequently dropped in AMC\.                        |
| hashed\_first\_name | String    | Audience member’s hashed first name\. This field is used for user\_id resolution and is subsequently dropped in AMC\.                   |
| hashed\_last\_name  | String    | Audience member’s hashed last name\. This field is used for user\_id resolution and is subsequently dropped in AMC\.                    |
| hashed\_phone       | String    | Audience member’s hashed phone number\. This field is used for user\_id resolution and subsequently dropped in AMC\.                    |
| hashed\_state       | String    | Audience member’s hashed state\. This field is used for user\_id resolution and subsequently dropped in AMC\.                           |
| hashed\_zip         | String    | Audience member’s hashed postal code\. This field is used for user\_id resolution and subsequently dropped in AMC\.                     |
| insert\_date\_utc   | Timestamp | Audience add/remove request. date\.                                                                                                      |
| user\_id            | String    | This field contains a user\_id that can be joined across ad\-exposure event data within AMC\. It is not exportable in workflow outputs\. |
| user\_type          | String    | The resolved user’s identifier type\.                                                                                                   |

Operations applicable to AMC datasets can be applied to audiences present in this table. Valid operations include:

- **Audience Overlap** report using AMC workflows. For more information, refer to the [Instructional Query: Audience Overlap report (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/47c61ae2fe6b61ff70137001cefd3825c4d7f664bb5d9ada57ab2c663d03c973).
- AMC [rule-based audience creation](guides/amazon-marketing-cloud/audiences/rule-based-audiences)

## Advertiser audiences resources

The following are the advertiser audiences APIs:

- [Connection](#connection-apis)
- [Metadata](#metadata-apis)
- [AMC Terms](#amc-terms-apis)
- [Audience](#audience-apis)

### Connection APIs

Connections are objects managed by Amazon Ads which store the mapping of AMC instances and Amazon DSP advertiser Ids for use as a target for audience creation, audience targeting, and audience activation use cases. To use the `targetResource.connectionId` object, the you must define a connection object.

There are a number of [APIs to retrieve](#connections---account-lookup) the eligible Ids (AMC & Amazon DSP account Ids) necessary to define a connection. Use the appropriate APIs depending on the product to retrieve the eligible Ids from.

AMC instances exist within an AMC account. When creating a new `connectionId` for use with AMC advertiser audiences APIs, you must provide a valid AMC account and AMC instance combination for AMC-related actions, as well as an associated Amazon DSP Advertiser Id to route audiences for activation.

The two connections APIs are:

- [Connections - account lookup](#connections---account-lookup)
- [Connections - create connection](#connections---create-connection)

#### Connections - account lookup

| Method | Endpoint         | Description                                           |
| ------ | ---------------- | ----------------------------------------------------- |
| GET    | /amc/accounts    | Retrieve AMC account list                             |
| GET    | /amc/instances   | List all AMC instances associated with your account   |
| GET    | /v2/profiles     | Retrieve agency accounts (DSP)                        |
| GET    | /dsp/advertisers | List all DSP advertisers associated with your account |

**Retrieve AMC account list**

Use the GET method of the `/amc/accounts` endpoint to view a list of AMC accounts that you have access to.

**List all AMC instances associated with your account**

To view the list of AMC instances associated with all the AMC accounts you have access to, use `GET /amc/instances`. To limit the number of results returned in the response to a specific number, append `limit=` along with the value to the endpoint. For example, if you want 25 instances listed in your response, use the following endpoint: `GET/amc/instances?limit=25`.
If there are more objects left to retrieve, the response will include an encrypted token (`nextToken`), which can be used to retrieve the next set of results.
If you do not pass limit as a path parameter, 10 instances will be returned in the response.

**List all DSP advertisers associated with your account**

The GET method of the `/dsp/advertisers` endpoint retrieves information of the Amazon DSP advertisers associated with your account.

> [NOTE] You must first call the GET `/v2/profiles` API to retrieve the DSP account ID.

### Connections - create connection

| Method | Endpoint                                               | Description                                 |
| :----- | :----------------------------------------------------- | :------------------------------------------ |
| POST   | /amc/audiences/connections                             | Create a connection                         |
| GET    | /amc/audiences/connections                             | List connections (based on input parameter) |
| DELETE | /amc/audiences/connections?connectionId={connectionId} | Invalidate connection                       |

**Create a connection**

To create a connection, you must fulfill at least one of the following requirements associated with the login (Login with Amazon account) that you used to generate your access token:

- Have a `DSP AdvertiserId` (if selecting a DSP Advertiser Account for connection)
- Or, have an `AMC instanceId`.

The connection must be saved with at least one Id, but can accept more than one Id.
Use the POST method of the `/amc/audiences/connections` to create a new connection between your AMC instances and/or DSP Advertisers.

To set a connection as the default connection, pass `true` as the value for the `isDefault` parameter in the AddConnection’s payload. When set to true, any other previously created connection that was set as default is set to ‘not default’.

The request payload must contain parameters for either AMC or DSP, or both, as indicated in the sample payload below.
A successful call returns a `connectionId`.

**Sample request**

```
{
    "amcAccountId": "ENTITY1000000000000",
    "amcAccountMarketplaceId": "ATVPDKIKX0DER",
    "amcInstanceId": "amcxxxxxxxx",
    "dspAdvertiserId": "0000000000000",
    "dspProfileId": "000000000000000",
    "isDefault": true
}
```

**Sample response**

```
{"connectionId":"00000000-0000-0000-0000-000000000000"}
```

**List connections**

Use the GET method of the `/amc/audiences/connections` endpoint to view a list of connections between your AMC instances and DSP Advertisers.

Note that the responses vary as per the request parameters listed below:

- If you pass the connection identifier (in the string format) as the query parameter GET `/amc/audiences/connections?connectionId=00000000-0000-0000-0000-000000000000`, the response will return details for the specified connection identifier.
- If you provide a Boolean value as true (parameter `GET /amc/audiences/connections?isDefault=true)`, the response will return your default connection details.
- If you do not pass a parameter (and default=false/missing), the response will contain a list of all your connections.

**Sample response (200 OK)**

```
{
  "connections": [
    {
      "amcAccountId": "ENTITY1000000000000",
      "amcAccountMarketplaceId": "ATVPDKIKX0DER",
      "amcInstanceId": "amcxxxxxxxx",
      "clientId": "amzn1.application-oa2-client.00000000000000000000000000000000",
      "connectionId": "00000000-0000-0000-0000-000000000000",
      "customerId": "A00000000000",
      "dspAdvertiserId": "0000000000000",
      "dspProfileId": "000000000000000",
      "isDefault": true
    },
    ...
  ]
}

```

**Invalidate connection**

The DELETE method of the `/amc/audiences/connections` deletes or breaks the connection with the provided `ConnectionId`. You can retrieve the `ConnectionId` from the response of either the POST or GET request.

> [NOTE] If you do not pass a `connectionId` in the request, **all** connections created for your account will be deleted.

```
DELETE /amc/audiences/connections?connectionId={connectionId}
```

**Request parameters**

- The specific `connectionId` (in string format) to delete.

> [WARNING] If this parameter is blank or missing, all your connections will be invalidated.

## Metadata APIs

This API uses metadata to create an audience.

The following are the endpoints for the Metadata API:

| Method | Endpoint                             | Description                     |
| :----- | :----------------------------------- | :------------------------------ |
| POST   | amc/audiences/metadata               | Create data provider audience   |
| PUT    | /amc/audiences/metadata/{audienceId} | Update a data provider audience |
| GET    | /amc/audiences/metadata/{audienceId} | Get a data provider audience    |

#### Create data provider audience

Use the `POST /amc/audiences/metadata` endpoint to create a new data provider audience metadata. This is a synchronous request and allows a single audience to be created for a given invocation.

> [NOTE] The time-to-live (ttl) is used to allow "batch uploads" to refresh your audience on a regular cadence without first removing records previously synchronized. By setting the ttl to 604800 (7 days), all audience members will be removed from the metadata 7 days from the initial synchronization date. If you upload members once per week, you can use this value to reset your audience membership. Use of ttl is a logical removal, when you revert the ttl from zero to a prior number, audience members will re-enter the audience, depending on the date they were ADDED to the Audience. Setting a ttl to 0 will soft remove audience members after we process the change asynchronously. Our default and maximum ttl is 34300800 (13 months).

A 201 response indicates that data provider audience metadata is created and an `audienceId` is generated along with the external audience Id which is the user-defined identifier for the audience.

**Request: Body schema**

| Key            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name           | (string) The audience name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| description    | (string) The audience description.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| targetResource | The identifier of the target resource. Any of the following are accepted inputs for targetResource: (i)**connectionId**  (string) or  (ii) **advertiserId** (number)                                                                                                                                                                                                                                                                                                                                                                                                      |
| countryCode    | (string) A**2-character string** in the **ISO 3166 format** that will be applied for all records within the audience. Although this is an optional field, it is highly recommended that you specify a country code when uploading data.<br /><br />You can also specify the country code at the record-level, which will supersede a country code added at the audience-level. If no audience-level country code is provided, but an audience is created in an Amazon DSP Advertiser Account, we use the marketplace of the Amazon DSP Advertiser Account as the default. |
| metadata       | The**AudienceMetadata**  parameters for audience metadata are:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                | a)**externalAudienceId** (string) - The user-defined audience identifier.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                | b)**ttl** (number) -Time-to-live in seconds. The amount of time the record is associated with the audience. Length 0 – 34300800.                                                                                                                                                                                                                                                                                                                                                                                                                                               |

**Sample request payload**

```
{
      "name": "audience_device",
      "description": "Device audiences",
      "countryCode": "string",
      "targetResource": { //One of the two IDs below is required
        "connectionId": "eb88dece-286d-4ad0-b7c5-87d229cdfd8f",
        "advertiserId": 00000000000
      },
      "countryCode": "US",
      "metadata": {
        "externalAudienceId": "audience-id-for-device",
        "ttl": 604800 // allowed values min: 0 to max: 34300800
    }
}

```

**Sample response (201 OK)**

```
{
    "audienceId": 347891,
    "externalAudienceId": "<audience-id-for-device>" 
}
```

### Update an audience

Use the `PUT /amc/audiences/metadata/{audienceId}` endpoint to update audience metadata. The update process is synchronous and requires an `audienceId` as part of the path parameter. The `audienceId` is returned as a response when creating an audience using `POST /amc/audiences/metadata` endpoint.

Updates an audience based on the `audienceId` and returns all the metadata associated with that `audienceId` to confirm the changes.

#### Retrieve an audience

This is a synchronous process and will require an `audienceId` attached to the end of the GET method to read as `GET /amc/audiences/metadata/{audienceId}`. Obtain the `audienceId` from the response parameters from the POST method described above. The GET endpoint then returns all the metadata associated with that `audienceId`.

## AMC Terms APIs

AMC will not permit the use of any functions related to the upload of external datasets without acceptance of **AMC Terms**. This includes the `/amc/audiences/records` API. To implement data upload to AMC for the generation of clean-room enabled insights, you must accept the AMC terms. To accept AMC terms, either log in to AMC directly or use the APIs below for partner implementations of AMC upload.

Retrieve the terms first, using the GET request. Upon agreement, resubmit term acceptance using the PATCH request below with the `agreementToken` provided as part of the terms and conditions text.

The following are the endpoints for the AMC terms API:

| Method | Endpoint                         | Description                                  |
| :----- | :------------------------------- | :------------------------------------------- |
| GET    | /amc/audiences/connections/terms | Retrieve terms and conditions.               |
| PATCH  | /amc/audiences/connections/terms | Update terms and conditions with acceptance. |

#### Retrieve terms and conditions

If you have not yet accepted the AMC terms and conditions, the `GET /amc/connections/terms` endpoint retrieves the current AMC terms and conditions text. If you have accepted the terms and conditions (using the PATCH method) this endpoint returns a Boolean value to indicate the acceptance.

```
GET /amc/audiences/connections/terms
```

**Sample response (200 OK)**.

```
{"agreementContent":"<h1>Amazon Marketing Cloud Agreement</h1><hr><p>The version of this Agreement in English is the definitive legal version...","agreementToken":"{xxx}","hasAccepted":false}
```

#### Update terms and conditions

Updates your terms and conditions acceptance.

```
PATCH /amc/audiences/connections/terms
```

**Sample request payload**

```
{
   "agreementToken": "{xxx}",
   "hasAccepted": true
}
```

## Audiences APIs

The audiences APIs manage the creation and upload of audiences and helps retrieve the status of audience creation. The following are the endpoints of the audiences API.

| Method | Endpoint                              | Description                                               |
| :----- | :------------------------------------ | :-------------------------------------------------------- |
| POST   | /amc/audiences/records                | Create audience records and uploads to AMC or Amazon DSP. |
| GET    | /amc/audiences/records/{jobRequestId} | Retrieves audience upload status.                         |

#### Create and upload audiences

Use the `POST /amc/audiences/records` endpoint to create audience records and upload to AMC and/or DSP.The audience records upload is an async process. Use the `audienceId` returned in the `POST/amc/audiences/metadata` API. This request accepts one or more audience record(s) in the request payload and returns a `jobRequestId`.

> [WARNING] To maintain the privacy of the records, it is mandatory to pass hashed PII records (as vs. actual values) in each parameter. Click [here for hashing rules](https://advertising.amazon.com/dsp/help/ss/en/audiences#GA6BC9BW52YFXBNE).

The `targetResource` object allows you to specify a connection object that stores the pre-configured mapping of Amazon advertising entities, and uses as a grouping mechanism for updating AMC and Amazon DSP entities. Request examples with the `targetResource` object and one without are listed below.

The `countryCode` is 2-character string in the ISO 3166 format that indicates from which country the data was collected. Although this is an optional field, it is highly recommended that you specify a country code when uploading data.

Datasets from the following countries are supported for hashed PII:  

| countryCode | Country |
|---|---|
| AE  | United Arab Emirates |
| AU | Australia |
| BE | Belgium |
| BR | Brazil |
| CA | Canada |
| DE | Germany |
| EG | Egypt |
| ES | Spain |
| FR | France |
| GB | United Kingdom |
| IN | India |
| IT | Italy |
| JP | Japan |
| MX | Mexico |
| NL | Netherlands |
| PL | Poland |
| SA | Saudi Arabia |
| SE | Sweden |
| SG | Singapore |
| TR | Turkey |
| US | United States of America |


> [NOTE] You can also specify the country code at the [audience metadata-level](#metadata-apis). A country code added at the record-level, however, will supersede a country code added at the Audience level.

```
POST /amc/audiences/records
```

**Sample request payload with targetResource**:

```
{
  "targetResource":{ 
    "connectionId": "5a523424-e8b3-4da5-9a32-24a861aea576",
  },
  "audienceId": 347891,
  "records": [
    {
      "externalUserId": "<external-user-id>",
      "countryCode": "US",
      "action": "CREATE",
      "hashedPII": [
        {
          "email": "c67a544ff3ec2feafb9b22…",
          "phone": "f0e4c2f76c58916ec258f2…",
          "city": "cbd3cfb9b9f51bbb…",
          "state": "569c7f0b41ce9649602a…",
          "postal": "3130329451cc782b7dfda79…",
          "firstname": "02a0218cd02ed0b0a3d93130…",
          "lastname": "282b91e08fd50a38f030d…",
          "address": "4d9dd6e50b298e47844be…"
        },
        …
      ],
      "measurements": {}
    },
    …
  ]
}

```

**Sample request payload example without targetResource**:

```
{
    "audienceId": 413642381039213536,
    "records": 
        [
            {
                "externalUserId": "CDP_Test_Ext_19876",
                "countryCode": "US",
                "action": "CREATE",
                "hashedPII": 
                [
                    {
                        "email": "e1789426d99ac0ba2a78c227b188ebe70c155e2f7b2d76b29f8f4e14d454e86d"
                    }
                ]
            }
        ]
}
```

### Retrieve audience upload status

Use the `GET /amc/audiences/records/{jobRequestId}` endpoint to list the upload status of records for a specific `jobrequestId` that was generated through the POST method, described above.

Response includes the status of the job (audience upload). Valid values of the job status are:

- **PENDING** indicates records are being processed.
- **SUCCEEDED** indicates records for all target accounts have been successfully processed and updated.
- **PARTIALLYSUCCEEDED** indicates records have successfully been processed and updated for at least one of the selected target accounts (`targetResource`), but has not succeeded for another.
- **FAILED** indicates records have not been updated to any accounts due to a processing error.

Additionally, response includes the number of records processed and the rate of processing. In case of invalid records, the response includes the error reason along with the `audienceId` and `externalId`.

## AMC follow-on actions

After audiences are uploaded to Amazon Marketing Cloud, audience data will be accessible for query in AMC under an auto-generated dataset under the **Advertiser uploaded** section of the AMC UI's schema explorer.
When uploading audience data, the following datasets are created in AMC:

- **Audience membership**
- **Audience additions and removals**

When using the advertiser audience APIs, note the following:

- Submissions for audience add/removals should be limited to under 15M records per hour per `connectionId`.
- Audience members uploaded to the API are processed for ingestion into AMC once per hour.
- Audience members uploaded to the API are processed for ingestion to Amazon DSP within 15 minutes, but will reflect in the DSP within 36 hours of submission.

### Audience API limits

Payload size limits are as follows:

- Metadata: 100 audiences per request.
- Audience records: Guidance of 6000 per request (max dependent on number of PII fields submitted).
- Hashed PII values per audience member: 100
