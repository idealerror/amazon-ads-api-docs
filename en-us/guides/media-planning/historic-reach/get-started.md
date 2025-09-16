---
title: Getting started with the Historic Reach APIs.
description: A guide on how to make calls to the Historic Reach APIs.
type: guide
interface: api
tags:
  - Historic Reach
keywords:
  - get started
  - historic
  - reach
  - curves
  - curve
---


# Get Started


## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in this tutorial.


## Prerequisites

Before using the Historic Reach API, ensure that you understand the following APIs.

| API                                                                                               | Description                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Profiles API](guides/get-started/retrieve-profiles)                                              | Returns the profile ID list needed to make successful calls. Profile ID in the Amazon Ads API corresponds to accounts in the Amazon Ads console. The retrieved list includes profiles associated with the advertiser whose permission is represented by the access token. This is the value you will use in the `Amazon-Advertising-API-Scope` header. |
| [Advertisers API](dsp-advertiser#tag/Advertiser/paths/~1dsp~1advertisers~1%7BadvertiserId%7D/get) | Returns the list of advertiser IDs that you have access to. This is the value that you will use in the `Amazon-Ads-AccountId` header.                                                                                                                                                                                                                  |
| [Audience Discovery API](audiences#tag/Discovery/operation/listAudiences)                         | Provides you with a list of audiences your advertiser ID is permissioned to and can be used for reach curve generation. You will use these IDs in the `audienceId` dimension.                                                                                                                                                                          |

## Header parameters

| Header                          | Description                                                                                                                                                                                                                                                               | Example                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| Amazon-Advertising-API-ClientId | The identifier of a client associated with a "Login with Amazon" account.	                                                                                                                                                                                                | `amzn1.application-oa2-client.abcdef123456ghijkkl7890` |
| Amazon-Advertising-API-Scope    |The identifier of a profile associated with the advertiser account. Use GET method on Profiles resource to list profiles associated with the access token passed in the HTTP Authorization header and choose profile id "profileId" from the response to pass it as input. | `123123123123123`                                      |
| Amazon-Ads-AccountId            | Account identifier you use to access the DSP. This is your Amazon DSP Advertiser ID.                                                                                                                                                                                      | `898989898989`                                         |
| Content-Type                    | We only support the type “application/json".	                                                                                                                                                                                                                            | `application/json`                                     |
| Accept                          | We only support the type “application/json".                                                                                                                                                                                                                              | `application/json`                                     |

# Endpoints

> [NOTE] Please see [API Endpoints](reference/api-overview#api-endpoints) to see the correct URL that you should be using. Although our APIs are globally available, please ensure that you call the correct endpoint depending on the geography that you want to use in your request. For example, call the EU endpoint if you want to use GB in your request.


## Create Historical Reach Curve

This is the endpoint that you will use for instantiating the generation of a reach curve. It accepts various parameters in the request body to allow you to more precisely include your target audience in the curve.

This is a `POST` request. 

The endpoint is `/mediaPlan/v2/historicalReachCurves`.

[API Schema Reference](historical-reach#tag/HistoricalReachCurves/operation/CreateHistoricalReachCurve)

### The Targeting Dimension Object

The request body uses a dynamic structure to allow us to provide more functionality without impacting the API schema. We call each targeting attribute a `dimension`. Below, we will list and define all the different properties of a dimension.

#### Dimension Type

Represented by the `dimensionType` key. This is a **required** property.

This property is used to describe the type of the dimension, which can be a single value, an array of values, or an array of dimensions. Each dimension type must be used with its corresponding value property.


| Dimension Type | Corresponding Dimension Value Field Name |
| -------------- | ---------------------------------------- |
| `SINGLE`       | `dimensionValue`                         |
| `MULTIPLE`     | `dimensionValues`                        |
| `GROUP`        | `dimensionGroups`                        |


#### Dimension Name

Represented by the `dimensionName` key. This is a **required** property.

This is the name of the targeting attribute. For example, "geography" will be used to specify the geography that you want to use to generate the curve.

The currently available dimension names are:


| Dimension Name       | Description                                                                                                                                            | Allowed Dimension Type(s)     | Example(s)                   | Is Required? |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------- | ---------------------------- | ------------ |
| `supplyPackage`      | The supply source of the data.                                                                                                                         | `SINGLE`                      | `STV`, `PRIME_VIDEO`         | Yes          |
| `demographic`        | The demographic of the people that viewed the ads. The default demographic used is `P2_PLUS`.                                                          | `SINGLE`                      | `A18_34`, `M35_54`, `F25_49` | No           |
| `reachType`          | Whether the reach is denominated in individuals or households.                                                                                         | `SINGLE`                      | `INDIVIDUALS`, `HOUSEHOLDS`  | Yes          |
| `audienceId`         | The ID(s) of the audience(s) to plan against.                                                                                                          | `SINGLE`, `MULTIPLE`, `GROUP` | `123123123`                  | No           |
| `geography`          | The country to plan against. This should be a two-letter representation of the geography.                                                              | `SINGLE`                      | `US`, `GB`, `DE`             | Yes          |
| `frequencyThreshold` | The minimum number of times that an individual or household was exposed to an ad to be counted as a data point in the curve. The default value is `1`. | `SINGLE`, `MULTIPLE`          | `1`, `2`, `3`, `4`, `5`, `6` | No           |


> [NOTE] Scroll down to [Accepted Targeting Values](guides/media-planning/historic-reach/get-started#accepted-targeting-values) to see the different accepted values for the different dimensions, where applicable.

#### Dimension Value

Represented by the `dimensionValue` key. This is a **required** property **IF** the `dimensionType` is `SINGLE`, otherwise this should not be used.

This is used to provide a single value for a targeting attribute/dimension. For example, you can use this field to pass the `supplyPackage` value of `STV` so that the reach curve is generated using STV data.


#### Dimension Values

Represented by the `dimensionValues` key. This is a **required** property **IF** the `dimensionType` is `MULTIPLE`, otherwise this should not be used.

This is similar to the `dimensionValue` field, except that this is an array of dimension values.

You would typically use this field to pass a list of audience IDs that are to be targeted when generating a reach curve.


#### Dimension Group

Represented by the `dimensionGroups` key. This is a **required** property **IF** the `dimensionType` is `GROUP`, otherwise this should not be used.

This field allows you to use nested dimensions. In other words, you can create a complex combination of audiences that you want to target when generating a reach curve.

> [WARNING] It should be noted that the maximum depth of `dimensionGroups` is 2. For example, you can use this property to build an audience target like `"audience1 AND (audience2 OR audience3)"`, but you cannot build an audience target like `"audience1 AND (audience2 AND (audience3 OR audience4))"`.


#### Inter-Operator

Represented by the `interOperator` key. The **default** value is `OR`. This is an **optional** property.

Enum values for this field are `AND` and `OR`.

When using a `dimensionType` of `MULTIPLE`, this field allows you to determine how each value in the `dimensionValues` list is compared with each other when generating a reach curve. For example, if you have a `dimensionValues` list that looks like `["123", 321", 343"]` and you set the `interOperator` value to `OR`, then, when generating the reach curve, the targeted audiences will be “123 OR 321 OR 343".


#### Intra-Operator

Represented by the `intraOperator` key. The **default** value is `AND`. This is an **optional** property.

Enum values for this field are `AND` and `OR`.

When using a `dimensionType` of `GROUP`, then this value is used to determine how the different `dimensionValue` or `dimensionValues` of the different dimensions in the group are compared with each other. For example, if you have one dimension with `dimensionValue` set to `"123"` and another dimension with `dimensionValues` set to `["222", "333", "444"]`  and its `interOperator` set to `OR`, and you set the `intraOperator` value to `AND`, then, when generating the reach curve, the targeted audiences will be “(123) AND (222 OR 333 OR 444)”.


#### Is-Not

> [WARNING] This property is not supported at the moment.

Represented by the `isNot` key.

This field allows you to negate the dimension that it belongs to. For example, if you want to create a targeting expression that roughly translates to “males-18-to-55 or females-18-to-55, but not males-35-to-45”, you would set this field to `true` for the dimension that has the `dimensionValue` of the audience ID of the audience that translates to `males-35-to-45`.



### Request Body

The request body has the following structure:

```
{
  "startDate": "String",
  "endDate": "String",
  "targeting": [
    {
      "dimensionName": "String",
      "dimensionType": "String",
      "dimensionValue": "String",
      "dimensionValues": [
        "String"
      ],
      "dimensionGroups": [
        {

        }
      ],
      "interOperator": "String",
      "intraOperator": "String"
    }
  ]
}
```

| Property | Description | Example(s) | Is Required? |
| ---- | ---| ---| --- |
| `startDate` | The start date for which to generate a reach curve.  Must be a historic start date. The earliest valid start date is `2024-09-25`. | `"2024-11-01"` | Yes          |
| `endDate`   | The end date for which to generate a reach curve. Must be greater than the start date. Must not be fewer than 7 days before the current date. | `"2024-12-01"`  | Yes|
| `targeting` | A list of the targeting dimension objects that define the targeting to use for generating the curve. | [See below](guides/media-planning/historic-reach/get-started#example-1) | Yes |

> [NOTE] Date ranges longer than 3 months may result in failed curve generation.

### Examples of the Request Body

#### Example 1

In this example, we will request to generate a reach curve using a single audience ID and a demographic type.

This request roughly translates to:

> *generate for me a reach curve where the start date is X and end date is Y* <br>
> *use US data for generating the curve* <br>
> *and only return data points where the ads were viewed at least 2 times* <br>
> *use STV as the source of this data* <br>
> *return only data where the impression data is for individuals* <br>
> *target the users to be males 18 to 34* <br>
> *and target the users to be audiences that fall under the audience ID of `123123123`.*

```
{
  "startDate": "2023-05-01",
  "endDate": "2023-05-31",
  "targeting": [
    {
      "dimensionName": "geography",
      "dimensionType": "SINGLE",
      "dimensionValue": "US"
    },
    {
      "dimensionName": "frequencyThreshold",
      "dimensionType": "SINGLE",
      "dimensionValue": "2"
    },
    {
      "dimensionName": "supplyPackage",
      "dimensionType": "SINGLE",
      "dimensionValue": "STV"
    },
    {
      "dimensionName": "reachType",
      "dimensionType": "SINGLE",
      "dimensionValue": "INDIVIDUALS"
    },
    {
      "dimensionName": "demographic",
      "dimensionType": "SINGLE",
      "dimensionValue": "M18_34"
    },
    {
      "dimensionName": "audienceId",
      "dimensionType": "SINGLE",
      "dimensionValue": "123123123"
    }
  ]
}
```


#### Example 2

In this example, we will request to generate a reach curve using multiple audience IDs and a demographic type.

Notice that the `interOperator` value is set to `OR`. This means that each audience ID in the list will be OR’d against each other.

This request roughly translates to:

> *generate for me a reach curve where the start date is X and end date is Y* <br>
> *use US data for generating the curve* <br>
> *and only return data points where the ads were viewed at least 3 times* <br>
> *use STV as the source of this data* <br>
> *return only data where the impression data is for individuals* <br>
> *target the audiences to be males 18 to 34* <br>
> *and target the audiences that fall under the audience ID of `123123123 OR 321321321 OR 789789789`.*

```
{
  "startDate": "2023-02-01",
  "endDate": "2023-02-28",
  "targeting": [
    {
      "dimensionName": "geography",
      "dimensionType": "SINGLE",
      "dimensionValue": "US"
    },
    {
      "dimensionName": "frequencyThreshold",
      "dimensionType": "SINGLE",
      "dimensionValue": "3"
    },
    {
      "dimensionName": "supplyPackage",
      "dimensionType": "SINGLE",
      "dimensionValue": "STV"
    },
    {
      "dimensionName": "reachType",
      "dimensionType": "SINGLE",
      "dimensionValue": "INDIVIDUALS"
    },
    {
      "dimensionName": "demographic",
      "dimensionType": "SINGLE",
      "dimensionValue": "M18_34"
    },
    {
      "dimensionName": "audienceId",
      "dimensionType": "MULTIPLE",
      "interOperator": "OR",
      "dimensionValues": [
        "123123123",
        "321321321",
        "789789789"
      ]
    }
  ]
}
```


#### Example 3

In this example, we will request to generate a curve using multiple audiences with one negated. While negation is not currently supported, this example does a good job to show how to use the `dimensionGroup` field.

This request roughly translates to:

> *generate for me a reach curve where the start date is X and end date is Y* <br>
> *use US data for generating the curve* <br>
> *and only return data points where the ads were viewed at least 1 time* <br>
> *use STV as the source of this data* <br>
> *return only data where the impression data is for individuals* <br>
> *target the audiences to be males 18 to 34* <br>
> *and target the audiences which fall under the audience ID of `NOT 123123123 AND (321321321 OR 789789789 OR 987987987)`.*

Another way of writing the expression in the dimension group is: `((NOT 123123123) AND (321321321 OR 789789789 OR 987987987))`.

```
{
  "startDate": "2023-02-01",
  "endDate": "2023-02-28",
  "targeting": [
    {
      "dimensionName": "geography",
      "dimensionType": "SINGLE",
      "dimensionValue": "US"
    },
    {
      "dimensionName": "frequencyThreshold",
      "dimensionType": "SINGLE",
      "dimensionValue": "1"
    },
    {
      "dimensionName": "supplyPackage",
      "dimensionType": "SINGLE",
      "dimensionValue": "STV"
    },
    {
      "dimensionName": "reachType",
      "dimensionType": "SINGLE",
      "dimensionValue": "INDIVIDUALS"
    },
    {
      "dimensionName": "demographic",
      "dimensionType": "SINGLE",
      "dimensionValue": "M18_34"
    },
    {
      "dimensionName": "audienceId",
      "dimensionType": "GROUP",
      "intraOperator": "AND",
      "dimensionGroups": [
        {
          "dimensionName": "audienceId",
          "dimensionType": "SINGLE",
          "dimensionValue": "123123123",
          "isNot": true
        },
        {
          "dimensionName": "audienceId",
          "dimensionType": "MULTIPLE",
          "interOperator": "OR",
          "dimensionValues": [
            "321321321",
            "789789789",
            "987987987"
          ]
        }
      ]
    }
  ]
}
```


### Response

The response of the create request has only three properties:

* The report ID. This will be used to check the status and to retrieve the reach curve data.
* The status. Upon a successful call to the API, the status should be PROCESSING.
* The status message. This is just additional verbiage that supports the status. When something goes wrong, this can be helpful in finding out what could have gone wrong.


#### Example Response

```
{
  "status": "PROCESSING",
  "reportId": "c571b6ecab2d41eba38c34b053ee1a1e",
  "message": "The curve is being generated."
}
```


## Get Historical Reach Curve Status

This is the endpoint you should use to get the status of your requested report/reach curve.

This is a `GET` request.

The endpoint is `/mediaPlan/v2/historicalReachCurves/{reportId}/status` where the `reportId` is the ID of your requested reach curve that you got by calling the Create endpoint.

[API Schema Reference](historical-reach#tag/HistoricalReachCurves/operation/GetHistoricalReachCurveStatus)

### Responses

#### Pending

```
{
  "status": "PENDING",
  "reportId": "c571b6ecab2d41eba38c34b053ee1a1e",
  "message": "The reach curve generation is pending."
}
```


#### Processing

```
{
  "status": "PROCESSING",
  "reportId": "c571b6ecab2d41eba38c34b053ee1a1e",
  "message": "The reach curve is being generated."
}
```


#### Completed

```
{
  "status": "COMPLETED",
  "reportId": "c571b6ecab2d41eba38c34b053ee1a1e",
  "message": "The reach curve has been generated."
}
```


#### Failed

The generation can fail due to a few different reasons, but the most common one is if the targeting you used resulted in an insufficient sample size to generate an accurate curve. The `message` will let you know when it fails because of an insufficient sample size.

```
{
  "status": "FAILED",
  "reportId": "c571b6ecab2d41eba38c34b053ee1a1e",
  "message": "The reach curve has generation failed."
}
```


## Get Historical Reach Curve

This is the endpoint that returns the successfully generated reach curve data points.

This is a `GET` request.

The endpoint is `/mediaPlan/v2/historicalReachCurves/{reportId}` where the `reportId` is the ID of the requested reach curve that you got by calling the Create endpoint.

[API Schema Reference](historical-reach#tag/HistoricalReachCurves/operation/GetHistoricalReachCurve)

### Response

The response follows the following structure:

```
{
  "totalResults": 2,
  "dataPoints": [
    {
      "supplyPackage": "STV",
      "demographic": "M18_34",
      "startDate": "2023-05-01",
      "endDate": "2023-05-31",
      "impressions": 123123,
      "totalReach": 123123,
      "frequencyThresholdReach": [
        {
          "frequencyThreshold": 1,
          "reach": 11111
        },
        {
          "frequencyThreshold": 2,
          "reach": 1112
        },
        {
          "frequencyThreshold": 3,
          "reach": 113
        }
      ],
      "geography": "US",
      "reachType": "HOUSEHOLDS"
    }
  ],
  "nextToken": "sample-next-token"
}
```


## Accepted Targeting Values

### Geography


| Country       | Geography Value   |
| ------------- | ----------------- |
| United States | `US`              |
| Canada        | `CA`              |
| Mexico        | `MX`              |
| Brazil        | `BR`              |
| Great Britain | `GB`              |
| Germany       | `DE`              |
| Austria       | `AT`              |
| France        | `FR`              |
| Italy         | `IT`              |
| Spain         | `ES`              |
| Australia     | `AU`              |
| Japan         | `JP`              |


### Supply Package


| Supply Package      | Dimension Value                     |
| ------------------- | ----------------------------------- |
| Streaming TV Bundle | `STV`                               |
| Prime Video         | `PRIME_VIDEO`                       |
| Devices             | `FIRE_TV_FIRE_TABLET_ALEXA_DISPLAY` |


## Reach Type

There are only two types of reach types:
- `INDIVIDUALS`
- `HOUSEHOLDS`


### Demographic

The following table shows the different demographic values that are supported and the geographies that support them.


| Demographic   | US  | CA  | MX  | BR  | GB  | DE  | AT  | FR  | IT  | ES  | AU  | JP  |
| ------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `P2_PLUS`     | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   |
| `A18_24`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `A18_34`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `A18_35`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `A18_49`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `A18_55`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `A18_PLUS`    | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   |     |
| `A20_24`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A20_PLUS`    |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A21_34`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `A25_29`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A25_34`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `A25_49`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `A25_54`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `A25_PLUS`    |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `A30_34`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A35_39`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A35_44`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `A35_49`      |     |     |     |     |     |     |     |     |     |     |     |     |
| `A35_54`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `A35_64`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `A35_PLUS`    | ✓   | ✓   | ✓   | ✓   |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `A36_55`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `A36_PLUS`    |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `A40_44`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A45_49`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A45_54`      |     |     |     |     |     |     |     |     | ✓   | ✓   |     |     |
| `A45_PLUS`    |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `A50_54`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A50_PLUS`    | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `A55_59`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `A55_64`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   |     |
| `A55_PLUS`    | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `A56_PLUS`    |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `F18_24`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `F18_34`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `F18_35`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `F18_49`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `F18_55`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `F18_PLUS`    | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   |     |
| `F2_PLUS`     |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `F20_24`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F20_34`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F20_PLUS`    |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F25_29`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F25_34`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `F25_49`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `F25_54`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `F25_PLUS`    |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `F30_34`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F35_39`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F35_44`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `F35_49`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F35_54`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `F35_PLUS`    | ✓   | ✓   | ✓   | ✓   |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `F36_55`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `F36_PLUS`    |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `F40_44`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F45_49`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F45_54`      |     |     |     |     |     |     |     |     | ✓   | ✓   |     |     |
| `F45_PLUS`    |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `F50_54`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F50_PLUS`    | ✓   |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F55_59`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `F55_64`      |     |     |     |     |     |     |     |     | ✓   | ✓   |     |     |
| `F55_PLUS`    | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `F56_PLUS`    |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `M18_24`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `M18_34`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `M18_35`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `M18_49`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `M18_55`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `M18_PLUS`    | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   |     |
| `M2_PLUS`     |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `M20_24`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M20_34`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M20_PLUS`    |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M25_29`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M25_34`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `M25_49`      | ✓   |     |     |     |     |     |     |     |     |     |     |     |
| `M25_54`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `M25_PLUS`    |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `M30_34`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M35_39`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M35_44`      |     |     |     | ✓   |     |     |     |     | ✓   | ✓   |     |     |
| `M35_49`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M35_54`      | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `M35_PLUS`    | ✓   | ✓   | ✓   | ✓   |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `M36_55`      |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `M36_PLUS`    |     |     |     |     | ✓   |     |     |     |     |     |     |     |
| `M40_44`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M45_49`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M45_54`      |     |     |     |     |     |     |     |     | ✓   | ✓   |     |     |
| `M45_PLUS`    |     |     |     | ✓   |     |     |     |     |     |     |     |     |
| `M50_54`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M50_PLUS`    | ✓   |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M55_59`      |     |     |     |     |     |     |     |     |     |     |     | ✓   |
| `M55_64`      |     |     |     |     |     |     |     |     | ✓   | ✓   |     |     |
| `M55_PLUS`    | ✓   | ✓   | ✓   |     |     | ✓   | ✓   | ✓   |     |     | ✓   |     |
| `M56_PLUS`    |     |     |     |     | ✓   |     |     |     |     |     |     |     |
