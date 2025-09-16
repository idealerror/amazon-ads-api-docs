---
title: Amazon Ads Sponsored Display creatives API 
description: Learn how to use the Amazon Ads API for Sponsored Display Brand Safety features to upload video, a custom headline, logo, or custom image for use in a campaign.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - campaign creatives
    - video
    - custom headline
    - logo
    - custom image
---

# Creative API developer guide

Sponsored Display allows advertisers to upload a video, custom headline, logo, or custom image for their campaigns that is used when serving your ads. Advertisers can use the headline, logo or custom image to reinforce their brand imagery and highlight the experience with their advertised product.

In this release, we provide a Creative API that allows you to upload a headline, logo, & custom image similar and associate that creative to different ad groups. Before you use the API, you need to have your logo or custom image  within the Creative Assets. If you already have an image uploaded and know the asset Id, you can skip to the Creative API section.

To follow the steps of this creative developer guide, please download and reference the [creatives Postman collection for Sponsored Display](guides/sponsored-display/tutorials/postman).

>[NOTE] Only one creative can be associated to a Sponsored Display `adGroupId`. 

## Get entity information

Make sure you have a valid OAuth token. Using the creative Postman collection, expand the *oauth* folder to run the requests to obtain a new access token, such as using the *OAuth Refresh Token* request to get a new access token.

1) Expand the *oauth* folder and run the *Gets profile information* request. It will use the access token configured within the environment.

The response will return the following:

```bash
[
    {
        "profileId": 1234567890,
        "countryCode": "US",
        "currencyCode": "USD",
        "dailyBudget": 9.99999999E8,
        "timezone": "America/Los_Angeles",
        "accountInfo": {
            "marketplaceStringId": "ATVPDKIKX0DER",
            "id": "A123567900",
            "type": "seller",
            "name": "Ad Expresso"
        }
    }
]
```

The entity id will be based on the accountInfo → id field, which you will use in Step 3 below.

## Creative Assets API

In this section, you will use the Creative Assets API to upload a new logo (ie image) that you can use when creating creatives for your campaigns.

The Creative Assets API requires the following steps:

1. Create an upload location for your new asset → This step provides a URL that you will use to upload your image.
2. Upload an image using the url obtained from step 1. 
3. Register the asset within your account to enable access to the newly uploaded asset.

Expand the *assets* folder to examine the Creative Assets requests. All 3 steps are required to create an asset: 

1. Creates an upload location to upload an asset. (POST request) 
2. Upload image. (PUT request) 
3. Registers an uploaded asset with Creative Assets with optional contextual and tagging information. (POST request)


***Step 1***

In the request body, you specify the name of the file you will upload. It must match the actual filename. The suffix (e.g. png) of the file name is required. The only image type supported are "JPEG", "JPG" and "PNG" (case in-sensitive)

```bash
POST /assets/upload
{
    "fileName": "test_creative.png"
}
```

The response from this request returns the following:

```bash
{
    "url": "https://al-na-9d5791cf-3faf.s3.amazonaws.com/511ca929-566a-4082-89b8-4128eca97cb6.png?x-amz-meta-fileName=test_creative.png&X-Amz-Security-Token=1234567890&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20210415T161206Z&X-Amz-SignedHeaders=host&X-Amz-Expires=3599&X-Amz-Credential=ASIA3NCY4QOMVRSXBSPM%2F20210415%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=1234567890"
}
```

You will use this url in step 2, it is automatically populated using the upload_url variable in the Postman request URL field. The url usually expire after 15 minutes so you will need to get a new one if it has expired. 

***Step 2***

Upload the image you’d like to use as the logo through Postman by choosing “binary” option and select a file as payload. The filename must match the exact name of the file our specified in the previous step. *The Asset image height should not be smaller than 100px, width should not be smaller than 600px, and the size of the image should not exceed 1MB.* The image can be in either png, jpg, jpeg format. 

```bash
PUT {{upload_url}}
{
    "Select File "
}
```

***Step 3***

In the request body, you can specify the following:

* The variable `entity_id` is pulled from Step 0 and filename and `upload_url` are used from step 1. 
* The assetType must be IMAGE, and the assetSubTypeList must be containing single value which is either `LOGO/PRODUCT_IMAGE/AUTHOR_IMAGE/LIFESTYLE_IMAGE/OTHER_IMAGE`. 
* You can add any tags to enable searching of your assets. 
* The accounts field allows you to specify one or more entityIds that will use the asset. If you want to use the image for other entityIds later, just upload the logo image again and register the asset with the new entityIds, and don't include the previous entityIds that you already used. In the request from the Postman collection, it will use the entity from Step 0 where you obtained the profile information.

```bash
POST /assets/register
{
    "name": "{{fileName}}",
    "url": "{{upload_url}}",
    "assetType": "IMAGE",
    "assetSubTypeList": [
        "LOGO"
    ],
    "tags": [
        "SD"
    ]
}
```

The response from this request returns the following:

```bash
{
    "assetId": "amzn1.assetlibrary.asset1.becf0adee0b0dc4e8ab96f3db46bxxxx",
    "versionId": "version_v1"
}
```

Optionally, you can search for assets using one of the following:

* *Retrieves an asset along with the metadata*: returns you asset details based on the assetId. 
* *Search assets*: returns one or more assets based on the search criteria

## Creative API

The Creative API allows you to create creatives using an headline, logo or custom image, where the logo and custom image are referenced using the assetId created in the previous section.

Before you call creative APIs, *please make sure you have created campaign, adGroup, productAd, target* already. When you create productAd, please make sure the ASIN/SKU you are using is valid and available to you. You’ll need to make sure you have a new adGroupId available to perform the following steps to add a creative to an adgroup.

Expand the *sd → creatives* folders to examine the requests.

***Step 1***

You’ll use the assetId and versionId obtained from step 3 of the Creative Assets API. The Postman scripts provide them as variables based on execution of requests from the previous section. *Asset image height should not be smaller than 100px, width should not be smaller than 600px*.

### Example of creating a `brandLogo`  in the Amazon Ads API

```bash
POST /sd/creatives
[
    {
        "adGroupId": 1234567890,
        "properties": {
            "headline": "Test Headline (input custom headline)",
            "brandLogo": {
                "assetId": "{{assetId}}",
                "assetVersion": "{{versionId}}",
                "croppingCoordinates": {
                    "top": 0,
                    "left": 0,
                    "width": 600,
                    "height": 100
                }
            }
        }
    }
]
```

The response from this request returns the following:

```bash
[
    {
        "code": "SUCCESS",
        "creativeId": 1234567890111213,
    }
]
```

### Example of creating a `customImage`  in the Amazon Ads API

```bash
POST /sd/creatives
[
    {
        "adGroupId": 1234567890,
        "properties": {
            "contentType": "CUSTOM_IMAGE",
            "rectCustomImage": {
                "assetId": "{{assetId}}",
                "assetVersion": "{{versionId}}",
                "croppingCoordinates": {
                    "top": 0,
                    "left": 0,
                    "width": 1200,
                    "height": 628
                }
            },
            "squareCustomImage": {
                "assetId": "{{assetId}}",
                "assetVersion": "{{versionId}}",
                "croppingCoordinates": {
                    "top": 0,
                    "left": 0,
                    "width": 628,
                    "height": 628
                }
            }
        }
    }
]
```

The response from this request returns the following:

```bash
[
    {
        "code": "SUCCESS",
        "creativeId": 1234567890111213,
    }
]
```

***Step 2***

You can use the PUT call to edit creative variables including assetId, headline, and croppingCoordinates associated to that creative for the given adGroupId.

Creative in `PENDING_REVIEW` status cannot be updated. You have to wait until the creative is approved or rejected. Once your creative has been approved/rejected, you can update the creative. When you update, if you are not changing anything in the headline and brandLogo section, the update creative API will directly return a 200 OK and the moderation status of the creative will remain same as before. Only when you update the creative with some changes in the headline and logo, the moderation status will be reset to `PENDING_REVIEW` and your changed creative will go into the moderation process. 

```bash
PUT /sd/creatives
[
    {
        "properties": {
            "headline": "Test_Headline",
            "brandLogo": {
                "assetId": "{{assetId}}",
                "assetVersion": "{{versionId}}",
                "croppingCoordinates": {
                    "top": 0,
                    "left": 0,
                    "width": 600,
                    "height": 100
                }
            }
        },
        "creativeId": 1234567890111213
    }
]
```

The response from this request returns the following:

```bash
[
    {
        "code": "SUCCESS",
        "creativeId": 1234567890111213
    }
]
```

***Step 3***

You can use this call to preview the HTML that will be used to generate the image for the new custom creative, including the newly added logo and headline. 

```bash
POST /sd/creatives/preview
{
    "creative": {
        "properties": {
            "headline": "Test Headline (input custom headline)",
            "brandLogo": {
                "assetId": "{{assetId}}",
                "assetVersion": "{{versionId}}",
                "croppingCoordinates": {
                    "top": 0,
                    "left": 0,
                    "width": 600,
                    "height": 100
        
                }
            }
        }
    },
    "previewConfiguration": {
        "size": {
            "width": 300,
            "height": 250
        },
        "products": [
            {
                "asin": "B08KWGHYQH"
            }
        ],
        "isMobile": false,
        "isOnAmazon": true
    }
}
```

The response from this request returns the following:

```bash
{ 
    "previewHtml": "html string that you need to unescape" 
}
```

If you want to preview the creative *without* customization, you can use below payload. "properties" is also optional. "creative": {} will also work.

```bash
POST /sd/creatives/preview
{
    "creative": {
        "properties": {}
    },
    "previewConfiguration": {
        "size": {
            "width": 300,
            "height": 250
        },
        "products": [
            {
                "asin": "B08KWGHYQH"
            }
        ],
        "isMobile": false,
        "isOnAmazon": true
    }
}
```

If you want to preview the creative *without* customization, you can use below payload: 

The response from this request returns the following:

```bash
{ 
    "previewHtml": "html string that you need to unescape without customization" 
}
```

Please note, when you specify size, isMobile, isOnAmazon in the preview configuration, the following values are available for creative previews: (The values in column of isMobile and isOnAmazon should be all *LOWER* case).

| Size (widthxheight) | isMobile | isOnAmazon | 
|--------------------|-----------------|--------|
| 245x250 | FALSE | TRUE | 
| 650x130| FALSE  | TRUE | 
| 414x125 | TRUE | TRUE |
| 320x50 | FALSE | TRUE| 
| 160x600 | FALSE | TRUE| 
| 300x250 | FALSE | TRUE | 
| 728x90 | FALSE | TRUE | 
| 970x250 | FALSE | TRUE | 
| 300x600 | FALSE | TRUE |  
| 980x55 | FALSE | TRUE |
| 300x600 | FALSE | TRUE |
| 270x150 | TBD | TBD |

You will get an escaped HTML in the preview creative API response. You need to unescape the HTML and save it into a file (e.g., PreviewCreative.html). You will need to create another HTML file using *iframe* which should have same size as the creative, and then you can open the iframe HTML in your browser and you will see the creative. 

Sample iframe file code with size 414x125: 

```bash
<html>
<body>
<iframe sandbox="allow-scripts allow-same-origin" src="./PreviewCreative.html" style="border:none; position:absolute;"  width="414px" height="125px" />
</body>
</html>
```

***Step 4***

You can use this call to get a list of creatives for each adGroupId including the customized headline & assetId associated with the adGroupId


```bash
GET sd/creatives?adGroupIdFilter={{adGroupId}}
```

The response from this request returns the following as an example:

```bash
[
    {
        "creativeId": 1234567890111213,
        "properties": {
            "headline": "Raptors 2019 Champions",
            "brandLogo": {
                "assetId": "amzn1.assetlibrary.asset1.becf0adee0b0dc4e8ab96xxxxxxx",
                "assetVersion": "version_v1",
                "croppingCoordinates": {
                    "top": 0,
                    "left": 0,
                    "width": 600,
                    "height": 100
                }
            }
        },
        "moderationStatus": "PENDING_REVIEW"
    }
]
```

***Step 4***

You can use this call to get the moderationStatus from a given creativeId using the creativeIdFilter

```bash
GET sd/moderation/creatives?creativeIdFilter={{creativeId}}&language=en_U
```

The response from this request returns the following as an example.  Please note, "contentType" can also be CUSTOM_IMAGE:

```bash
 [
    {
        "creativeId": 1234567890111213,
        "moderationStatus": "PENDING_REVIEW",
        "policyViolations": []
        "contentType": "HL"
    }
]
```

## Other good things to know

***ProductAd***

When you create a productAd in a campaign, please make sure to use the valid and available ASIN/SKU. Creative cannot render with an invalid ASIN/SKU. 

***TPS limitation***

Currently for creative APIs, we only allow 1 TPS for POST, GET, and PUT request. Higher TPS will be throttled.

***Headline***

* Cannot be empty string
* Length cannot be more than 50 characters
* Cannot contain illegal characters: <  >  #  ^  !  ...
* Capitalized characters cannot be more than 50%, but this validation will not be enabled if the headline length <= 10 characters
* Cannot contain emojis, ex. `\uD83D` `\uDE41`

***Logo or Custom Image***

When registering the asset, you can specify proper values to make sure below validation criteria are met: 

* *asset image height should not be smaller than 628px, width should not be smaller than 1200px*
     * assetType should be IMAGE
     * assetStatus should be ACTIVE
     * assetContentType should be one of the following: 
         * "jpeg",
         * "jpg",
         * "png",
         * "gif",
         * "image/jpeg",
         * "image/jpg",
         * "image/png",
         * "image/gif"
     * assetExtension should be one of the following: 
         * "JPEG",
         * "JPG",
         * "PNG",
         * "GIF"
     * assetFileSize should not be larger than 5MB
     * cropping coordinates should not cropping outside of the image (if your image uploaded in Creative Assets only has width 1200px, don’t crop it to be 1300px)
     * cropping coordinates should not be less than 628x628 px for square custom image.

