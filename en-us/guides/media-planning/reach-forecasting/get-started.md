---
title: Getting started with the reach forecasting API
description: Walkthrough of a typical request and response to the reach forecasting API.
type: guide
interface: api
tags:
  - Reach forecasting
keywords:
  - get started
  - prerequisites
  - request example
  - response example
---
# Get started

## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in this tutorial.

> [WARNING] If you wish to access one of the reach forecasting APIs currently in closed beta, you will need to have your **Amazon Client ID** added to the allow-list. Please send an email to your Amazon Account Team with your Client ID to gain access.

## Endpoints by Regions

| **URL**                               | **Region**         | **Marketplaces**                                                                                                                                                                                                    |
|---------------------------------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| https://advertising-api.amazon.com    | North America (NA) | United States (US), Canada (CA), Mexico (MX), Brazil (BR)                                                                                                                                                           |
| https://advertising-api-eu.amazon.com | Europe (EU)        | United Kingdom (GB), France (FR), Italy (IT), Spain (ES), Germany (DE), Netherlands (NL), United Arab Emirates (AE), Poland (PL), Turkey (TR), Egypt (EG), Saudi Arabia (SA), Sweden (SE), Belgium (BE), India (IN) |
| https://advertising-api-fe.amazon.com | Far East (FE)      | Japan (JP), Australia (AU), Singapore (SG)                                                                                                                                                                          |

> [NOTE] While this API is available globally, it is deployed in specific regions: **EU (Europe)**, **NA (North America)**, and **FE (Far East)**. Choose the correct endpoint based on the marketplace you want to forecast. For example, forecasts for GB utilize the EU endpoint.


## Prerequisites

Before using the reach forecasting API, ensure you understand the following APIs:

| API                                                                                                                                  | Description                                                                                                                                                                                        | Use case                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Profiles API](guides/get-started/retrieve-profiles)                                                                                 | Profile ID in the Amazon Ads API correspond to accounts in the Amazon Ads console. A separate account is established for each marketplace. Profiles play a crucial role to make successful calls.  | Users must provide the profile ID for the request header parameter `Amazon-Advertising-API-Scope`.                                                                              |
| [Advertisers API](dsp-advertiser#tag/Advertiser/paths/~1dsp~1advertisers~1%7BadvertiserId%7D/get)                                    | Returns the list of advertiser IDs that you have access to.                                                                                                                                        | Users can optionally provide an advertiser ID for the request header parameter `Amazon-Ads-AccountId`.                                                                          |
| [Advertised Product Categories Discovery API](dsp-discovery-advertised-product-categories)                                           | Provides you with a list of advertised product categories to be used for generating reach forecasts.                                                                                               | Users are required to call this API to get a list of valid advertised product categories and pass the `id` values from that list through `advertisedProductCategoryIds` list    |
| [Audiences Discovery API](audiences#tag/Discovery/operation/listAudiences)                                                           | Provide you with audiences that you can target.                                                                                                                                                    | Users are required to call this API to get a list of accessible audiences to pass the `audienceId` values through `AudienceTargetV1` in the targets list.     |
| [GeoLocations API](dsp-campaigns#tag/Discovery/operation/getGeoLocations)                                                            | Return the list of all targetable locations.                                                                                                                                                       | Users are required to call this API to get a list of locations to pass the `id` values through `LocationTargetV1` in the targets list                                  |
| [Product-Targeting ASIN Categories API](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getTargetableCategories) | Returns all targetable categories. This API returns a large JSON string containing a tree of category nodes. Each category node has the fields - category ID, category name, and child categories. | Users are required to call this API to get a list of product category IDs in order to pass the `id` values through `ProductCategoryTargetV1` in the targets list.               |
| [Product Metadata API](product-metadata)                                                                                             | Returns product metadata for the advertiser, which contains ASINs in the response.                                                                                                                 | Users are required to call this API to get a list of ASIN strings to pass the `ASIN` values through `ProductTargetV1` in the targets list.                             |
| [Deals API](dsp-deals-3p#tag/Deal/operation/listDealsDspDeals)                                                                       | Returns the list of DSP deal IDs that you have access to.                                                                                                                                          | Users are required to call this API to get the deal IDs (`internalDealId`) for use in `InventorySourceTargetV1` in the targets list.                                            |


## Header parameters

| Header                          | Description                                                                                                                                                                                                                                                              | Example                                              |
|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------|
| Amazon-Advertising-API-ClientId* | The identifier of a client associated with an Amazon Developer account.                                                                                                                                                                                                  | amzn1.application-oa2-client.abcdef123456ghijkkl7890 |
| Amazon-Advertising-API-Scope*    | The identifier of a profile associated with the advertiser account. Use the GET method on the Profiles resource to list profiles associated with the access token passed in the HTTP Authorization header, and choose the `profileId` from the response to pass as input. | 123123123123123                                      |
| Authorization*                   | Login with Amazon token in the form of `Bearer {{token}}`.                                                                                                                                                                                                                | Bearer AtzaIwEBIIR1q...                              |
| Amazon-Ads-AccountId             | The account identifier used to access the DSP. This is your Amazon DSP Advertiser ID.                                                                                                                                                                                    | 898989898989                                         |
| Accept                           | application/json                                                                                                                                                                                                                                                         | application/json                                     |
| Content-Type                     | application/json                                                                                                                                                                                                                                                         | application/json                                     |

> [TIP] In addition to the individual API parameters, all above listed header parameters are required when calling the reach forecasting API.


## Create Reach Forecasts

Endpoint: [POST /mediaPlan/v1/reachForecasts](reach-forecasting#tag/Reach-Forecasting-API/operation/CreateReachForecastsV1)

**The table below outlines the valid input combinations. Please review this table carefully before making API calls to ensure compliance with accepted values.**


| Supply Package                        | Delivery Type                                       | Country Code                                                       | Device Type                                     | Allowed targeting sets                                                                                                                                                             |
|---------------------------------------|-----------------------------------------------------|--------------------------------------------------------------------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DSP\_STREAMING\_TV                      | GUARANTEED                                          | US, GB, DE, CA, MX                                                | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_AUDIO                             | GUARANTEED                                          | US, CA, MX, GB, DE, IT                                             | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_PRIME\_VIDEO                       | GUARANTEED                                          | US, CA, MX, BR, GB, DE, FR, IT, ES, AU, JP, IN                    | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_TWITCH\_VIDEO                      | GUARANTEED                                          | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| SPONSORED\_PRODUCTS                    | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | Not Supported                                   | 1.[LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                                                                                |
| SPONSORED\_BRANDS                      | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | Not Supported                                   | 1.[LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                                                                                |
| SPONSORED\_BRANDS\_VIDEO                | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | Not Supported                                   | 1.[LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                                                                                |
| SPONSORED\_DISPLAY                     | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | Not Supported                                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                 |
| DSP\_OLV                               | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_STREAMING\_TV                      | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_AUDIO                             | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | CONNECTED\_TV, DESKTOP, MOBILE, CONNECTED\_DEVICE | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_DISPLAY                           | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | Not Supported                                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                 |
| DSP\_TWITCH\_VIDEO                      | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | CONNECTED\_TV, DESKTOP, MOBILE                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, DeviceTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1] |
| DSP\_TWITCH\_DISPLAY                    | NON\_GUARANTEED                                      | US, CA, MX, BR, GB, DE, FR, IT, ES, SE, IN, SA, TR, AE, NL, JP, AU | Not Supported                                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                 |
| DSP\_FIRE\_TV\_FIRE\_TABLET\_ALEXA\_DISPLAY | Refer to [Support by Country, Supply Package, DeliveryType](#support-by-country-supply-package-deliverytype)   | Refer to [Support by Country, Supply Package, DeliveryType](#support-by-country-supply-package-deliverytype)                   | Not Supported                                   | 1.[AudienceTargetV1, ContentGenreTargetV1, ContentRatingTargetV1, LocationTargetV1]<br/> 2.[ProductCategoryTargetV1]<br/> 3.[ProductTargetV1]                 |


### Request Body

All the fields shown below are mandatory for generating a reach forecast. Currently, a maximum of 5 sub-requests is allowed in the `reachForecasts` list at a time.

```
{
  reachForecasts: [
    {
      countryCode: Enum("US", "CA", "MX", "BR", "GB", "DE", "FR", "IT", "ES", "SE", "IN", "SA", "TR", "AE", "NL", "JP", "AU")
      startDate: Date("YYYY-MM-DD"),
      endDate: Date("YYYY-MM-DD"),
      supplyPackage: SupplyV1[],
      deliveryType: Enum("GUARANTEED", "NON_GUARANTEED"),
      reachType: Enum("HOUSEHOLDS"),
      frequencyCap: FrequencyCapV1,
      advertisedProductCategoryIds: string[]
      targets: PlanningTargetV1[]
    }
  ]
}
```

```
type PlanningTargetV1 {
  // Whether to target (false) or exclude (true) the given target.
  negative: boolean,

  targetDetails: {
    audienceTarget: AudienceTargetV1
    contentGenreTarget: ContentGenreTargetV1
    contentRatingTarget: ContentRatingTargetV1
    deviceTarget: DeviceTargetV1
    inventorySourceTarget: InventorySourceTargetV1
    keywordTarget: KeywordTargetV1
    locationTarget: LocationTargetV1
    productCategoryTarget: ProductCategoryTargetV1
    productTarget: ProductTargetV1
    themeTarget: ThemeTargetV1
  }
}
```

### Parameters

The following sections provide in-depth details about each parameter you might need to interact with while calling our API. These explanations are designed to help you understand request structure, functionality, and best practices.


#### CountryCode

**Description**: API callers are required to provide a country code for each forecast.

**Accepted Values**:
`US`, `CA`, `MX`, `BR`, `GB`, `DE`, `FR`, `IT`, `ES`, `SE`, `IN`, `SA`, `TR`, `AE`, `NL`, `JP`, `AU`.

**Example**:

```json
"countryCode": "US"
```


#### Start Date and End Date

**Description**: Defines the date range for the forecast.

**Requirements**:

- Must be in `YYYY-MM-DD` format.
- Both dates must be in the future.
- Start date must be at least tomorrow, with *tomorrow* calculated based on UTC time zone.
- End date must be after the start date.

**Example**:

```
"startDate": "2026-01-01",
"endDate": "2026-01-15"
```

> [NOTE] The end date must fall within 18 months from the current month (inclusive).


#### Supply Package

**Description**: A specific supply package you want to target.

**Accepted Values**: A list of SupplyV1.

```
enum SupplyV1 {
  SPONSORED_PRODUCTS = "SPONSORED_PRODUCTS",
  SPONSORED_BRANDS = "SPONSORED_BRANDS",
  SPONSORED_BRANDS_VIDEO = "SPONSORED_BRANDS_VIDEO",
  SPONSORED_DISPLAY = "SPONSORED_DISPLAY",
  DSP_OLV = "DSP_OLV",
  DSP_STREAMING_TV = "DSP_STREAMING_TV",
  DSP_AUDIO = "DSP_AUDIO",
  DSP_DISPLAY = "DSP_DISPLAY",
  DSP_TWITCH_VIDEO = "DSP_TWITCH_VIDEO",
  DSP_TWITCH_DISPLAY = "DSP_TWITCH_DISPLAY",
  DSP_PRIME_VIDEO = "DSP_PRIME_VIDEO",
  DSP_FIRE_TV = "DSP_FIRE_TV",
  DSP_FIRE_TABLET = "DSP_FIRE_TABLET",
  DSP_ALEXA_DISPLAY = "DSP_ALEXA_DISPLAY",
  DSP_FIRE_TV_FIRE_TABLET_ALEXA_DISPLAY = "DSP_FIRE_TV_FIRE_TABLET_ALEXA_DISPLAY",
}
```

Currently, we support supplyPackage with

- single SupplyV1
- multiple supplies for Guaranteed STV+ (`DSP_STREAMING_TV` with any combination of `DSP_TWITCH_VIDEO`, `DSP_PRIME_VIDEO`)
- multiple supplies for connected devices Guaranteed and Non-Guaranteed (any combination of `DSP_FIRE_TV`, `DSP_FIRE_TABLET`, `DSP_ALEXA_DISPLAY`, `DSP_FIRE_TV_FIRE_TABLET_ALEXA_DISPLAY`).


**Examples**:

Any single supplyPackage with a single supply.

```json
"supplyPackage": ["SPONSORED_PRODUCTS"]
```

STV+

```json
"supplyPackage": ["DSP_STREAMING_TV"]
"supplyPackage": ["DSP_STREAMING_TV", "DSP_TWITCH_VIDEO"]
"supplyPackage": ["DSP_STREAMING_TV", "DSP_PRIME_VIDEO"]
"supplyPackage": ["DSP_STREAMING_TV", "DSP_TWITCH_VIDEO", "DSP_PRIME_VIDEO"]
```

Connected Devices

```json
"supplyPackage": ["DSP_FIRE_TV"]
"supplyPackage": ["DSP_FIRE_TABLET", "DSP_ALEXA_DISPLAY", "DSP_FIRE_TABLET"]
"supplyPackage": ["DSP_FIRE_TV_FIRE_TABLET_ALEXA_DISPLAY"]
```

#### Delivery Type

**Description**: Specifies the delivery mode for the reach forecast.

**Accepted Values**: `GUARANTEED`, `NON_GUARANTEED`

**Example**:

```json
"deliveryType": "GUARANTEED"
```

#### Support by Country, Supply Package, DeliveryType

**Support for supply package other than Connected Devices**:

- For the **Non-Guaranteed** delivery type, all countries and supplies are supported except **DSP\_FIRE\_TV\_FIRE\_TABLET\_ALEXA\_DISPLAY**.
- For **DSP\_FIRE\_TV\_FIRE\_TABLET\_ALEXA\_DISPLAY**, please refer to below table **Countries/Delivery Types Availability for Connected Devices**.
- For the **Guaranteed** delivery type, support for different supplies varies by country. Below are the supported countries for each supply:
  - **DSP\_STREAMING\_TV**: `["US", "CA", "MX", "GB", "DE"]`
  - **DSP\_AUDIO**: `["US", "CA", "MX", "GB", "DE", "IT"]`
  - **DSP\_TWITCH\_VIDEO**: `All countries`
  - **DSP\_PRIME\_VIDEO**: `["US", "CA", "GB", "DE", "FR", "IT", "ES", "MX", "AU", "JP", "BR", "IN"]`

**Support for Connected Devices (DSP\_FIRE\_TV\_FIRE\_TABLET\_ALEXA\_DISPLAY):**

| `CountryCode` |`MANAGED_SERVICE` <br> `GUARANTEED` | `MANAGED_SERVICE` <br> `NON_GUARANTEED` | `SELF_SERVICE` <br> `GUARANTEED` | `SELF_SERVICE` <br> `NON_GUARANTEED` |
| :--- | :----: | :----: | :----:| :----: |
| US   | ✓ | ✓ |   | ✓ |
| CA   | ✓ | ✓ |   | ✓ |
| GB   | ✓ | ✓ |   | ✓ |
| DE   | ✓ | ✓ |   | ✓ |
| FR   | ✓ | ✓ |   | ✓ |
| IT   | ✓ | ✓ |   | ✓ |
| ES   | ✓ | ✓ |   | ✓ |
| JP   | ✓ | ✓ |   | ✓ |
| IN   | ✓ | ✓ |   | ✓ |
| MX   |   |   |   | ✓ |
| BR   |   |   |   | ✓ |
| NL   |   |   |   | ✓ |
| AU   |   |   |   | ✓ |
| TR   |   |   |   |   |
| SA   |   |   |   |   |
| SE   |   |   |   |   |
| AE   |   |   |   |   |


#### Advertised Product Category

**Description**: The list of identifiers of the advertised product categories for the forecast.

**Accepted Values**: Find the advertised product category IDs in [DSP ListAdvertisedProductCategories API](dsp-discovery-advertised-product-categories#tag/Discovery-Advertised-Product-Categories/operation/DspListAdvertisedProductCategoriesV1).

**Requirements**:

  - Do not include multiple parent categories.
  - Do not include child categories from multiple parent categories.

**Example**:

```json
{
  "advertisedProductCategoryIds": ["335417915575420049"]
}
```


#### Frequency Cap

**Description**: The limit of how many times ads appear to the same viewer.

**Accepted Values**:

```
FrequencyCapV1 {
  type: Enum("CUSTOM", "UNCAPPED"),
  maxImpressions: integer(1, 500),
  timeUnitCount: integer(1, 30),
  timeUnit: Enum("DAYS", "WEEKS", "MONTHS")
}
```

**Example**:

```json
{
  "frequencyCap": {
    "type": "CUSTOM",
    "maxImpressions": 10,
    "timeUnitCount": 3,
    "timeUnit": "DAYS"
  }
}
```

**The rule of frequencyCap**:

- Do not pass `maxImpressions`, `timeUnitCount` and `timeUnit` if type is `UNCAPPED `.
- Frequency cap is not supported for supplies: `SPONSORED_PRODUCTS`, `SPONSORED_BRANDS`, `SPONSORED_BRANDS_VIDEO`, `SPONSORED_DISPLAY`.


#### Audience Targeting

**Description**: For targeting specific audiences.

**Accepted Values**:
Find audience IDs at [Audiences Discovery API](audiences#tag/Discovery/operation/listAudiences).

```
type AudienceTargetV1 {
  targetType: "AUDIENCE"
  audienceId: String
  groupId?: String
}
```

**Example**:

```json
{
  "targetDetails": {
    "audienceTarget": {
      "targetType": "AUDIENCE",
      "audienceId": "audienceId",
      "groupId": "groupId"
    }
  }
}
```

**Rules of combining multiple audience targets:**

- Included groups are joined by an intersection. Users must be in all the groups specified to be included.
- Audiences within an inclusion group are joined by OR/ANY logic. Audiences within the exclusion group are joined by AND/ALL logic. This means that users can be in any of the audiences inside a group to be included or excluded.
- All audiences within the same group must either be included or excluded.
- You can specify up to 10 groups to be included, but only 1 group to be excluded.
- Up to 500 audience IDs can be specified per group.
- To add audiences to a new group, choose any string not currently being used on this forecast.
- To add audiences to an existing group, use the existing groupId from this forecast. You may specify up to 10 include groups and 1 exclude group.


#### Content Genre Targeting

**Description**: Include/exclude content based on genre.

**Accepted Values**:

```
type ContentGenreTargetV1 {
  targetType: "CONTENT_GENRE",
  contentGenre: Enum(
    "ACTION",
    "ADVENTURE",
    "ANIMATION",
    "BIOGRAPHY",
    "COMEDY",
    "CRIME",
    "DOCUMENTARY",
    "DRAMA",
    "FAMILY",
    "FANTASY",
    "FILM_NOIR",
    "GAME_SHOW",
    "HISTORY",
    "HORROR",
    "MUSICAL",
    "MYSTERY",
    "NEWS",
    "REALITY_TV",
    "ROMANCE",
    "SCIENCE_FICTION",
    "SHORT",
    "SPORT",
    "SUPER_HERO",
    "TALK_SHOW",
    "THRILLER",
    "WAR",
    "WESTERN",
    "GENRE_NOT_AVAILABLE"
  )
}
```

**Example**:

```json
{
  "targetDetails": {
    "contentGenreTarget": {
      "targetType": "CONTENT_GENRE",
      "contentGenre": "SUPER_HERO"
    }
  }
}
```


#### Content Rating Targeting

**Description**: Exclude content based on available content rating options.

**Accepted Values**:

```
type ContentRatingTargetV1 {
  targetType: "CONTENT_RATING",
  contentRatingType: Enum("DSP_CONTENT_RATING"),
  dspContentRating: Enum(
    "SUITABLE_FOR_ALL_AUDIENCES",
    "SUITABLE_FOR_MOST_AUDIENCES_WITH_PARENTAL_GUIDANCE",
    "SUITABLE_FOR_TEEN_AND_OLDER_AUDIENCES",
    "SUITABLE_FOR_MATURE_AUDIENCES",
    "SUITABLE_FOR_ADULTS",
    "RATING_NOT_AVAILABLE"
  )
}
```

**Example**:

```json
{
  "targetDetails": {
    "contentRating": {
      "targetType": "CONTENT_RATING",
      "contentRatingType": "DSP_CONTENT_RATING",
      "contentRatingDetails": {
        "dspContentRating": "SUITABLE_FOR_ALL_AUDIENCES"
      }
    }
  }
}
```

**Detailed explanation of each content rating**:

- **`SUITABLE_FOR_ALL_AUDIENCES`**: Equivalent to content that is rated G (film), TV-Y (TV), TV-Y7 (TV), TV-G (TV), EC (game), or E (game).
- **`SUITABLE_FOR_MOST_AUDIENCES_WITH_PARENTAL_GUIDANCE`**: Equivalent to content that is rated PG (film), TV-PG (TV), or E-10+ (game).
- **`SUITABLE_FOR_TEEN_AND_OLDER_AUDIENCES`**: Equivalent to content that is rated PG-13 (film), TV-14 (TV), or T (game).
- **`SUITABLE_FOR_MATURE_AUDIENCES`**: Ages 17+. Equivalent to content that is rated R (film), TV-MA (TV), or M (game).
- **`SUITABLE_FOR_ADULTS`**: Ages 18+. Equivalent to content that is rated NC-17 (film).
- **`RATING_NOT_AVAILABLE`**: Content where rating isn't available from the publisher.


#### Device Targeting

**Description**: The type of device being targeted.

**Accepted Values**:

```
type DeviceTargetV1 {
  targetType: "DEVICE",
  deviceType: Enum(
    "DESKTOP",
    "MOBILE",
    "CONNECTED_TV",
    "CONNECTED_DEVICE"
  )
}
```

**Example**:

```json
{
  "targetDetails": {
    "deviceTarget": {
      "targetType": "DEVICE",
      "deviceType": "DESKTOP"
    }
  }
}
```


#### Product Category Targeting

**Description**: For targeting an ASIN category.

**Accepted Values**: Find the ASIN category id at [Product-Targeting ASIN Categories API](sponsored-products/3-0/openapi/prod#tag/Product-Targeting/operation/getTargetableCategories).

**Example**:

```json
{
  "targetDetails": {
    "productCategoryTarget": {
      "targetType": "PRODUCT_CATEGORY",
      "asinCategory": "asinCategoryIdString"
    }
  }
}
```


#### Product Targeting

**Description**: For targeting a specific product ASIN.

**Accepted Values**:
  - Find the ASIN string at [Product Metadata API](product-metadata)

**Example**:

```json
{
  "targetDetails": {
    "productTarget": {
      "targetType": "PRODUCT",
      "asin": "B0D1XD1ZV3"
    }
  }
}
```


#### Location Targeting

**Description**: For targeting a specific location.

**Accepted Values**: Find the location id at [GeoLocations API](dsp-campaigns#tag/Discovery/operation/getGeoLocations).

**Example**:

```json
{
  "targetDetails": {
    "locationTarget": {
      "targetType": "LOCATION",
      "geoLocation": "locationId"
    }
  }
}
```


#### Inventory Source Targeting

**Description**: For targeting an inventory source, currently we supports only DSP Deal.

**Accepted Values**: Find the deal ID at [Deals API](dsp-deals-3p#tag/Deal/operation/listDealsDspDeals).

**Requirements**:

  - Do not include `supplyPackage` when the request contains `InventorySourceTargetV1` with a deal ID.
  - Deal forecasting requests must include exactly one `InventorySourceTargetV1` with a deal ID and no other targets.

**Example**:

```json
{
  "targetDetails": {
    "inventorySourceTarget": {
      "targetType": "INVENTORY_SOURCE",
      "inventorySourceType": "DEAL",
      "inventorySourceId": "dealId"
    }
  }
}
```


#### Keyword Targeting

**Description**: For targeting specific keywords related to products.

**Accepted Values**:

```
type KeywordTargetV1 {
  targetType: "KEYWORD",
  keyword: String,
  matchType: Enum(
    "BROAD",
    "PHRASE",
    "EXACT"
  )
}
```

**Requirements**:

- Must include target bid information (bid price and currency code)
- Must be used with product targeting (ProductTargetV1)
- Only supported for supplies: `SPONSORED_PRODUCTS`, `SPONSORED_BRANDS`, `SPONSORED_BRANDS_VIDEO`

**Detailed explanation of Match Type**:

- **`BROAD`**: Provides the widest exposure to search queries. Search terms may include keyword terms in any order, and may also include singulars, plurals, variations, synonyms, and related terms.
- **`PHRASE`**: Search terms must contain the exact phrase or sequence of words. More restrictive than broad match and includes plural forms of the keyword. In some campaign types, it may also include search terms matching the meaning of the keyword.
- **`EXACT`**: The most restrictive match type where search terms must exactly match the keyword or close variations. The sequence of words must also match exactly.

**Example**:

```json
{
  "negative": false,
  "bid": {
    "bid": 0.75,
    "currencyCode": "USD"
  },
  "targetDetails": {
    "keywordTarget": {
      "targetType": "KEYWORD",
      "keyword": "AirPods Pro 2",
      "matchType": "PHRASE"
    }
  }
},
{
  "negative": false,
  "targetDetails": {
    "productTarget": {
      "targetType": "PRODUCT",
      "asin": "B0D1XD1ZV3"
    }
  }
}
```


#### Theme Targeting

**Description**: For targeting automatically generated keywords related to brands or landing pages.

**Accepted Values**:

```
type ThemeTargetV1 {
  targetType: "THEME",
  matchType: Enum(
    "KEYWORDS_RELATED_TO_YOUR_BRAND",
    "KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES"
  )
}
```

**Requirements**:

- Must include target bid information (bid price and currency code)
- Must be used with product targeting (ProductTargetV1)
- Only supported for supplies: `SPONSORED_BRANDS`, `SPONSORED_BRANDS_VIDEO`

**Match Type Explanation**:

- **`KEYWORDS_RELATED_TO_YOUR_BRAND`**: Targets keywords related to the brand.
- **`KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES`**: Targets keywords related to the content on the landing pages.

**Example**:

```json
{
  "negative": false,
  "bid": {
    "bid": 0.75,
    "currencyCode": "USD"
  },
  "targetDetails": {
    "themeTarget": {
      "targetType": "THEME",
      "matchType": "KEYWORDS_RELATED_TO_YOUR_BRAND"
    }
  }
},
{
  "negative": false,
  "targetDetails": {
    "productTarget": {
      "targetType": "PRODUCT",
      "asin": "B0D1XD1ZV3"
    }
  }
}
```


#### Sample request

In this example, we will create a reach forecast with the following settings.

- Specifying a frequency cap.
- Exclude a list of content rating.
- Target a audiences expressions of the form of include ((a1, a2) and (a3, a4)) AND exclude (a5, a6).

```json
{
  "reachForecasts": [
    {
      "startDate": "2025-02-01",
      "endDate": "2025-06-30",
      "countryCode": "US",
      "supplyPackage": [
        "DSP_STREAMING_TV"
      ],
      "deliveryType": "GUARANTEED",
      "reachType": "HOUSEHOLDS",
      "frequencyCap": {
        "type": "CUSTOM",
        "maxImpressions": 10,
        "timeUnitCount": 3,
        "timeUnit": "DAYS"
      },
      "advertisedProductCategoryIds": ["325359509049081864"],
      "targets": [
        {
          "negative": true,
          "targetDetails": {
            "contentRatingTarget": {
              "targetType": "CONTENT_RATING",
              "contentRatingType": "DSP_CONTENT_RATING",
              "contentRatingDetails": {
                "dspContentRating": "SUITABLE_FOR_MATURE_AUDIENCES"
              }
            }
          }
        },
        {
          "negative": true,
          "targetDetails": {
            "contentRatingTarget": {
              "targetType": "CONTENT_RATING",
              "contentRatingType": "DSP_CONTENT_RATING",
              "contentRatingDetails": {
                "dspContentRating": "SUITABLE_FOR_ADULTS"
              }
            }
          }
        },
        {
          "negative": false,
          "targetDetails": {
            "audienceTarget": {
              "targetType": "AUDIENCE",
              "audienceId": "a1",
              "groupId": "include_1"
            }
          }
        },
        {
          "negative": false,
          "targetDetails": {
            "audienceTarget": {
              "targetType": "AUDIENCE",
              "audienceId": "a2",
              "groupId": "include_1"
            }
          }
        },
        {
          "negative": false,
          "targetDetails": {
            "audienceTarget": {
              "targetType": "AUDIENCE",
              "audienceId": "a3",
              "groupId": "include_2"
            }
          }
        },
        {
          "negative": false,
          "targetDetails": {
            "audienceTarget": {
              "targetType": "AUDIENCE",
              "audienceId": "a4",
              "groupId": "include_2"
            }
          }
        },
        {
          "negative": true,
          "targetDetails": {
            "audienceTarget": {
              "targetType": "AUDIENCE",
              "audienceId": "a5",
              "groupId": "exclude"
            }
          }
        },
        {
          "negative": true,
          "targetDetails": {
            "audienceTarget": {
              "targetType": "AUDIENCE",
              "audienceId": "a6",
              "groupId": "exclude"
            }
          }
        }
      ]
    }
  ]
}
```


### Response body

Details of response body:

* Because this API is a batch API, all success responses will be grouped in the success array with the index matching the index of request input.
* The **SUCCESS** status indicate that the forecast is generated successfully. The **EXPIRED** status indicate that the forecasted data is not up-to-date and we recommend generating a new forecast.
* The `countryCode` is the targeted country of this forecast, which is tied to the account/API client Id that you are using for calling the API.
* The `currencyCode` is the currency for all the monetary values in the request and response. For each `countryCode`, we support only the default `currencyCode` in that country/market.
* The `dataPoints` array contains the reach curve.

```
{
  success: [
    {
      index: Integer,
      reachForecast: {
        reachForecastId: string,
        status: Enum("SUCCESS", "EXPIRED"),
        creationDateTime: DateTime,
        countryCode: string,
        currencyCode: string,
        avgCpm?: Double,
        maxCpm?: Double,
        cpc?: Double,
        dataPoints: ReachCurveDataPointV1[]
      }
    }
  ],
  error: [
    {
      index: Integer,
      error: {
        code: string,
        message: string
      }
    }
  ]
}

```

The key component in the response body is `ReachCurveDataPointV1` as below, which the reach number is the forecasted reach assuming certain number of impressions are delivered.

```
type ReachCurveDataPointV1 {
  impressions: long
  reach: long
}
```

**Sample response**

```json
{
  "success": [
    {
      "index": 0,
      "reachForecast": {
        "reachForecastId": "24281a60-387f-4f9f-abb2-66378a8712e4",
        "status": "SUCCESS",
        "creationDateTime": "2024-06-01T23:36:09.973Z",
        "countryCode": "US",
        "currencyCode": "USD",
        "avgCpm": 20,
        "maxCpm": 40,
        "dataPoints": [
          {
            "impressions": 14000,
            "reach": 11000
          },
          {
            "impressions": 16000,
            "reach": 12000
          }
          // more data points
        ]
      }
    }
  ],
  "error": []
}
```


### SLOs

#### Latency

Generating a single reach forecast can take around 8 ~ 30s. The P99 latency for generating a forecast is **25s**.

#### Throttling

The maximum number of calls supported per account per second is **1**.

#### Reach Forecast Date Range

* Maximum date range supported: 6 months.
* Maximum end date supported: 600 days from date of calling.

#### Forecast Expiration

Forecast will be saved for every request, but we don’t have guarantee on the long-term storage for these records. API integrators should not rely on our storage for long term record.



## Create Deduplicated Reach Forecasts

Endpoint: [POST /mediaPlan/v1/deduplicatedReachForecasts](reach-forecasting#tag/Forecast-deduplication-API/operation/CreateDeduplicatedReachForecastsV1)

This API is used to create a list of De-duplicated Reach Forecasts.

Each deduplication call must contain at least **two** Reach Forecast IDs.

Each `reachForecastId` represents a unique request with `deliveryType`, `supply`, and `targets`.


### Request Body
All the fields shown below are mandatory for creating the de-duplicated reach forecast. Currently, a maximum of 5 sub-requests is allowed in the `deduplicatedReachForecasts` list at a time.
Each sub-request allows a maximum of 100 valid Reach Forecast IDs.

**Sample request**

```json
{
  "deduplicatedReachForecasts": [
    {
      "budgetAllocations": [
        {
          "reachForecastId": "56e80501-7c5c-4b5c-89ff-e104fea9d26d",
          "reach": 123
        },
        {
          "reachForecastId": "a5340ce7-a066-4479-93e5-17435b99388f",
          "reach": 456
        }
      ]
    }
  ]
}
```

The following sections provide in-depth details about each parameter you need to interact with while calling reach forecasting API. These explanations are designed to help you understand request structure, functionality, and best practices.

### Parameters

#### Reach Forecast IDs

**Description**: The identifier of the Reach Forecast that the specified budget is allocated to.

**Accepted Values**: The ReachForecast IDs you provided must be valid IDs obtained from the response of [Create reach forecasts](reach-forecasting#tag/Reach-Forecasting-API/operation/CreateReachForecastsV1).

**Example**:

```json
{
  "budgetAllocations": [
    {
      "reachForecastId": "24281a60-387f-4f9f-abb2-66378a8712e4",
      "reach": 123
    },
    {
      "reachForecastId": "36e8f956-7126-4c12-3792-1ae1d372ef9c",
      "reach": 456
    }
  ]
}
```

**Limitations of Reach Forecast IDs in Deduplicated Reach Forecasts API**:

1. Targets must be same between each Reach Forecast ID.

  - ❌ `ReachForecastId1` (**G + STV + Target X**) + `ReachForecastId7` (**NG + AUDIO + Target Y**) → **Not Allowed** (Different targets).
  - ✅ `ReachForecastId1` (**G + STV + Target X**) + `ReachForecastId2` (**NG + STV + Target X**) → **Allowed** (Same target, same supply with different `deliveryType`).

2. If supplies are same between each `reachForecastId`, `deliveryType` must be different.

  - ❌ `ReachForecastId1` (**G + STV + Target X**) + `ReachForecastId7` (**G + STV + Target X**) → **Not Allowed** (Same supply + Same `deliveryType`).
  - ✅ `ReachForecastId1` (**G + STV + Target X**) + `ReachForecastId2` (**NG + STV + Target X**) → **Allowed** (Same supply + Different `deliveryType`).

3. If supplies are different between each `reachForecastId`, no limitations for `deliveryType`.

  - ✅ `ReachForecastId1` (**STV + Target X + G**) + `ReachForecastId2` (**Prime_Video + Target X + G**) → **Allowed** (Different supply + Same `deliveryType`).
  - ✅ `ReachForecastId3` (**STV + Target X + G**) + `ReachForecastId4` (**Prime_Video + Target X + NG**) → **Allowed** (Different supply + Different `deliveryType`).


#### Reach

**Description**: The reach number of the selected data point of the reach forecast.

**Accepted Values**: Integer number

**Example**:

```json
{
  "reachForecastId": "36e8f956-7126-4c12-3792-1ae1d372ef9c",
  "reach": 123
}
```

> [NOTE] The input reach number must not exceed the maximum forecasted reach for the corresponding reach forecasts.


### Response body

Details of response body:

* Since this API is a batch API, all successful responses will be grouped in the `success` array with the index matching the index of request input.
* The `deduplicatedReachForecastId` is the unique identifier of the Deduplicated Reach Forecast resource.
* The `deduplicatedReach` is forecasted deduplicated reach.

```
{
  success: [
    {
      index: integer,
      deduplicatedReachForecast: {
        deduplicatedReachForecastId: string,
        deduplicatedReach: long
      }
    }
  ],
  error: [
    {
      index: integer,
      error: {
        code: string,
        message: string,
      }
    }
  ]
}
```

**Sample response**

```json
{
  "success": [
    {
      "index": 0,
      "deduplicatedReachForecast": {
        "deduplicatedReachForecastId": "122d5a10-3ffe-6dc5-6127-19a02cb23765",
        "deduplicatedReach": 26758483
      }
    },
    {
      "index": 1,
      "deduplicatedReachForecast": {
        "deduplicatedReachForecastId": "f76e39cd-36a2-3b04-e126-c9e37fe53c23",
        "deduplicatedReach": 28787878
      }
    }
  ],
  "error": []
}
```


### SLOs

#### Latency

Generating the de-duplicated reach forecast can take around 5 ~ 20s. The P99 latency for generating a de-duplicated reach forecast is **16s**.

#### Throttling

The maximum number of calls supported per account is **1 TPS**.
