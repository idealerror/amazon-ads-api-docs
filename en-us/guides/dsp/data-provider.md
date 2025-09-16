---
title: Data Provider API
description: A guide for using the Amazon DSP Data Provider API
type: guide
interface: api
tags: 
    - Amazon DSP
    - Data Provider
keywords:
    - guide
---

# Get started with the Data Provider API

Use the Data Provider API to create audiences using either your own customer records or customer records from external data providers. Populate your audiences with IDs corresponding to records in the following formats:

* `COOKIE` - IDs corresponding to cookies sent to Amazon from data providers.
* `MAID` - mobile advertising identifiers.
* `EXTERNAL_USER_ID` - external IDs created either by data providers or by you while uploading hashed customer data.

After creating a data provider audience, you can use it like any other Amazon DSP audience. 

>[NOTE] If you're using the data provider API in order to upload your own customer data into an audience, you must follow these steps: <br /> <br />
1. [Create a record by uploading your hashed data](#upload-hashed-first-party-customer-data) <br />
2. [Create a data provider audience](#create-a-new-data-provider-audience) <br />
3. [Associate your data provider audience with your newly created record](#associate-or-disassociate-your-records-with-your-data-provider-audience)

## Upload hashed first-party customer data

To create an audience from your own records, first upload hashed personally-identifiable information (PII) records using [POST /dp/records/hashed](data-provider/hashed-records#tag/User-deletion/paths/~1v2~1dp~1users/patch).

The [POST /dp/records/hashed](data-provider/hashed-records#tag/User-deletion/paths/~1v2~1dp~1users/patch) endpoint attempts to match your hashed records with internal Amazon customer records. For privacy reasons, you will not be able to see which matches were successful.

### Hash your data

>[NOTE]External links in this section may require Amazon Ads console access.

Before uploading, you may need to hash your data. To do so, first organize your data according to our [formatting guidelines](https://advertising.amazon.com/help/GCCXMZYCK4RXWS6C). [Download our template](https://amazadshelp.s3.amazonaws.com/DSP/downloadable-reports/MultiAttributeTemplate.csv) to create correctly organized tables that have some or all of the headers below:

* Email: email
* Phone: phone
* First name: first_name
* Last name: last_name
* Street address: address
* City: city
* State / Province: state
* Postal code: zip

Your table must have between 10,000 and 5,000,000 rows. Finally, use [our hashing tool](https://amazadshelp.s3.amazonaws.com/DSP/downloadable-reports/MultiAttributeHashingTool.html) to hash your data.

Alternatively, if you intend to use your own SHA-256 hashing tool, make sure that you follow our [formatting guidelines](https://advertising.amazon.com/help/GCCXMZYCK4RXWS6C) to avoid errors in the uploading process.

>[NOTE] The maximum input size is limited to 5 MB.

### Upload your hashed data to a record

Now that your data is hashed, you can upload it to create a record.

#### Sample request: upload hashed customer data

```bash
curl --location 'https://advertising-api.amazon.com/dp/records/hashed/' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-xxxxxxxxxx' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Atza|xxxxxx' \
    --header 'Host: advertising-api.amazon.com' \
    --data '{
        "records": [
            {
            "hashedRecords": [
                {
                    "firstName": "XXXXXX",
                    "lastName": "XXXX",
                    "address": "XXXX",
                    "phone": "XXXXX",
                    "city": "XXXXX",
                    "postalCode": "XXXX",
                    "state": "XXXXX",
                    "email": "XXXXXX"
                }
            ],
            "externalId": "ID_1"
            }
        ]
    }'
```

Each record requires you to provide an `externalId` of your choice. Later, you’ll use this ID to associate the record to a data provider audience. 

## Create a new data provider audience

#### Sample request: create a data provider audience

Use [POST v2/dp/audiencemetadata/](data-provider/openapi#tag/Metadata/paths/~1v2~1dp~1audiencemetadata~1/post):

```bash
curl --location 'https://advertising-api.amazon.com/v2/dp/audiencemetadata/' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.XXXXXXX' \
    --header 'Authorization: Bearer Atza|XXXXXX' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Amazon-Advertising-API-Scope: 123123123' \
    --data '{
        "name": "string",
        "description": "string",
        "advertiserId": 1111111111`,
        "metadata": {
            "type": "DATA_PROVIDER",
            "externalAudienceId": "string",
            "ttl": 34300800,
            "dataSourceCountry": ["US"]
        }
    }'
```

This audience will not have any customers in it until you explicitly associate a record.

Make sure to specify the correct data source country for each of your audiences. Else, your uploaded data may receive lower internal match rates.

## Associate or disassociate your records with your data provider audience

Provide a list of customer records to be associated with the audience as an array of identifiers. Only the matched customer records will be associated with the audience.

>[NOTE] The [PATCH /v2/dp/audience](data-provider/openapi#tag/Add-or-remove-records/paths/~1v2~1dp~1audience/patch) endpoint has a 100TPS throttle with a max payload of 1MB. Design your batch uploads around these limits.

#### Sample request: associate records with data provider audiences

```bash
curl --location --request PATCH 'https://advertising-api.amazon.com/v2/dp/audience' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Atza|xxxxxxxxxxx' \
    --header 'Host: advertising-api.amazon.com' \
    --data '{
        "patches": [
            {
                "op": "add",
                "path": "/COOKIE-XYZXYZXYZ/audiences",
                "value": [
                    12312312123
                ]
            }
        ]
    }'
```

The `value` field corresponds to the `audienceId` returned to you in the `v2/dp/audiencemetadata/` response. 

>[NOTE] Use the [PATCH /v2/dp/audience](data-provider/openapi#tag/Add-or-remove-records/paths/~1v2~1dp~1audience/patch) endpoint to associate MAID and COOKIE record IDs to your data provider audience. For hashed records, use the `externalId` you provided while uploading your hashed PII. For external data providers, use the ID they’ve provided you with. 

To disassociate records from your audience, use the same endpoint but with `op` changed from `add` to `remove`.

#### Sample request: disassociate records with data provider audiences

```bash
curl --location --request PATCH 'https://advertising-api.amazon.com/v2/dp/audience' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Atza|xxxxxxxxxxx' \
    --header 'Host: advertising-api.amazon.com' \
    --data '{
        "patches": [
            {
                "op": "remove",
                "path": "/COOKIE-XYZXYZXYZ/audiences",
                "value": [
                    12312312123
                ]
            }
        ]
    }'
```

## (optional) Remove your user data

To completely remove user data that you’ve provided, use [PATCH /v2/dp/users](data-provider/openapi#tag/User-deletion/paths/~1v2~1dp~1users/patch):

#### Sample request: remove user data

```bash
curl --location --request PATCH 'https://advertising-api.amazon.com/v2/dp/users' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Content-Type: application/json' \
--header 'Authorization: Atza|xxxxxxxx' \
--data '{
    "users": [
        {
            "userId": [
                {
                    "type": "COOKIE",
                    "id": "XXXXXXXXX"
                }
            ],
            "advertiserId": "xxxxxxxxx"
            "consentTime": 1000
        }
    ]
    }'
```

If you add an `advertiserId` to your request, the user data will only be removed for that particular advertiser. Else, all corresponding user data sourced from your client will be removed.