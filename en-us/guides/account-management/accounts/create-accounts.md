---
title: Account creation in the Amazon Ads API
description: Instructions for account creation in the Amazon Ads API, with information about prerequisites.
type: guide
interface: api
tags:
    - Account management
    - Onboarding
    - Authorization
keywords:
    - permission scopes
    - global accounts
    - registration
---

# Account creation

Amazon Ads supports registration via API for businesses who do and do not sell on Amazon. Integrators can offer an ingress to Amazon Ads directly from their portal, making it seamless for their advertisers to sign up for Amazon. With these APIs you can create a sponsored ads account and get its details all from your own interface. 

[View the technical specifications for account management in the Amazon Ads API.](account-management)

## The account construct

These account creation endpoints use an upgraded account construct. Accounts are now globalized in nature and come with a country dimension. Advertisers that are eligible for multiple countries now get one advertising account that spans all countries. 

Each country does have its own ID specific to each advertiser, which you’ll need to make individual requests for most other APIs. See [Profiles](guides/account-management/authorization/profiles) for more information on country-specific identifiers.

>[NOTE]Registration via this API is currently only available for sponsored ads accounts, and does not include Amazon DSP or other account types.

## Expected call flow for account creation

1. Authorize the Amazon user to your system via Login With Amazon
2. Get a token to verify the Amazon’s Ads Agreement has been accepted
3. Register an ads account
4. Get registration status and account details
5. Link to Amazon’s Billing setup page

These steps are explained in detail in this guide.

## Prerequisites

### Amazon login

If you have already completed the [onboarding](guides/onboarding/overview) and [getting started](guides/get-started/overview) sequences for the Amazon Ads API, you will have already created a Login With Amazon client application. You can use access tokens generated through Login With Amazon to administer advertising accounts on behalf of an Amazon user account.

[Learn more about authorization in the Amazon Ads API.](guides/account-management/authorization/overview)

All users must have an Amazon login (username and password) in order to register an ads account. If a user already has a login, ideally with their work email address, they will sign in as part of the next step. If they do not have a login, they must create one. We recommend they use the email they use for their professional work, which differentiates the login from their Amazon shopping login, and we use email addresses for various types of verifications. To create a login, on the Login With Amazon (LwA) sign-in page, they can select **Create Amazon Account** to create a username and password.

>[TIP]If the user is already logged in to Amazon shopping with a personal login, **they should log out first**, or use a new browser to log in with their work email.

### LwA authorization grant

Before registering an Amazon Ads account, we require your users to authorize your client application to access advertising data from their Amazon user account as described in the previous section. Completing this step prior to ads account registration ensures the account is immediately available in your UI upon completing a registration.

To set this up, learn how to [create an authorization grant](guides/get-started/overview) and [retrieve access and refresh tokens](guides/get-started/retrieve-access-token).

You will use this access token in the `Authorization` header of all requests below, along with your Amazon Ads API client ID, as described in the [Authorization overview](guides/account-management/authorization/overview).

## Procedure for Amazon Ads account registration

### Step 1 - Terms and conditions token

In order to register an ads account through the Amazon Ads API, you must verify that the user has seen and accepted our Terms and Conditions, also known as the [Ads Agreement](https://advertising.amazon.com/terms). 

To do so, you will call an API to generate a `termsToken` and `termsUrl`. You must surface a link (`termsUrl`) to the [Ads Agreement](https://advertising.amazon.com/terms) for your users during ads account registration. This link is provided in the API. If you do not surface this, you will not receive a verified token nor will you be able to successfully register an ads account. When the user clicks that link, it will surface an Amazon page that has the Ads Agreement and an **Accept and continue** button. We recommend showing this as a small pop up window, that auto-closes when the user clicks the button. 

When they click **Accept and continue**, the token will be active and you will submit that `termsToken` with the registration request in Step 2. We recommend storing the token in your system for future reference.

[View the technical specifications for the terms tokens API.](account-management#tag/Terms-Token)

##### Examples

POST /termsTokens

Response:

```json
{
    "termsToken": "4de3327e-a595-44cb-b689-f7f746805c02",
    "termsUrl": "https://advertising.amazon.com/terms/agreement/4de3327e-a595-44cb-b689-f7f746805c02"
}
```

GET /termsTokens/{termsToken}

Response:

```json
{
    "termsTokenStatus": "ACCEPTED"
}
```

>[NOTE]Each token may only be used for one request. Once an account is created with a token, its status will be marked as REDEEMED and cannot be reused to create more accounts.

##### Example

You can add a link to the Ads Agreement in your Amazon Ads account registration form, which will open a window like the below.

![Window showing Amazon Ads agreement](/_images/accounts/ads-agreement-window.png)

### Step 2 - Register an account

#### Option 1: Register a business who does not sell on Amazon

An advertiser who does not sell on Amazon can self-register from third-party systems like yours to get started with sponsored ads. To register, we require account and business details, which we will verify automatically within 2-10 seconds. Note that we only support US-based businesses at this time. 

[See the technical specifications for account registration in the Amazon Ads API.](account-management#tag/Account/operation/RegisterAdsAccount)

##### Example

POST /adsAccounts

**Sample request**

You must include the name of the ads account (typically the brand or business name), country where advertising will occur (currently US only for this program), and all business details. Details include legal business name, phone, website, address, city, state, zip and country. 

Note that only a direct employee of the business can advertise. Unprofessional email addresses are not accepted and will result in a rejection. To prevent abuse, we also have controls to prevent an employee of one company registering on behalf of another. Be sure that it’s clear in your UI that only employees of the business can register on their own behalf. Some agencies have special status to enroll on behalf of their clients automatically, but must still submit all the right legal business details of the client. Note that an agency without special status will need to ask their client to register themselves.

```json
{
  "accountName": "Haven Hotel",
  "countryCodes": ["US"],
  "associations": [
    {
      "business": {
        "name": "Haven Hotel Inc.",
        "phone": "555-555-5555",
        "websiteUrl": "www.havenhotel.com",
        "addressLine1": "450 W 33rd St.",
        "addressLine2": "7th Floor",
        "city": "New York",
        "state": "New York",
        "zipCode": "10001",
        "countryCode": "US"
      }
    }
  ]
}
```

**Sample Response**

The response immediately returns an account ID in pending status. Full registration then happens asynchronously. 

```json
Response: 201

{
    "adsAccount": {
        "accountName": "test account name",
        "adsAccountId": "amzn1.ads-account.g.1l2262owvunt6oxnuo7n1re9p",
        "status": "PENDING"
    }
}
```

>[TIP]If you already have business information on file for your advertiser, they don’t need to submit it again manually. You can lift what you have and call our API with a 3-click registration experience. They must still accept our Ads Agreement. 

>[NOTE]Such a business can register multiple ads accounts with Amazon if they prefer to, but there is a limit. This limit varies based on a number of factors, which we cannot disclose. 

**Rejection Response**
Amazon may reject an account request for a variety of reasons, whether the business is not eligible at this time or violates an Amazon policy. If the initial response comes back with a Status of DISABLED, there will be an error reason along with suggested error messages. 

**Manual Review**
If an account request is rejected, the advertiser may request their information to be manually reviewed by Amazon. You can provide the below link in your error messages. They will be directed to an Amazon page where they can submit details directly to Amazon. Amazon will then contact them with a response and account details if they are approved. The account will immediately flow into your system as soon as the advertiser accepts it.

>`https://advertising.amazon.com/contact-sales/sponsored-display-beta`

#### Option 2: Register an Amazon Ads account for a selling partner

To register an Amazon selling partner, such as an Amazon Vendor or Seller, the API is the same but the input parameters different. You must obtain their selling partner ID (Vendor Group or Seller Central Account) in order to register. To get this ID, the advertiser can provide it manually, obtained from within their Vendor or Seller Central accounts.

##### Example

POST /adsAccounts

**Sample request for an Amazon Seller**

You must provide the Seller Central account ID (MCID) of the seller, and the user must have direct permissions to that seller account. Indicate that it’s an `amazonSeller` with their `sellerCentralAccount` ID. 

```json
{
  "termsToken": "1b8bd385-026c-49f0-886b-95cfcd8979e0",
  "associations": [
    {
      "amazonSeller": {
        "sellerCentralAccount": "A6DEQ9LYHIBE8"
      }
    }
  ]
}
```

**Sample request for an Amazon Vendor**

You must provide the Vendor Group of the advertiser, and the user must have direct permissions to that vendor group. Indicate that it's an `amazonVendor` with their `vendorGroup` ID. 

```json
{
  "termsToken": "1b8bd385-026c-49f0-886b-95cfcd8979e0",
  "associations": [
    {
      "amazonVendor": {
        "vendorGroup": "6943231"
      }
    }
  ]
}
```

**Sample Request for a Kindle-Direct Publishing (KDP) Author**

You must provide the email address of the author who owns the KDP account.

```json
{
  "termsToken": "1b8bd385-026c-49f0-886b-95cfcd8979e0",
  "associations": [
    {
      "amazonAuthor": {
        "email": "email-I-used-for-my-author-account@gmail.com"
      }
    }
  ]
}
```

>[NOTE]The user registering under the selling partner ID must have access to the Vendor, Seller, or Author account in question. If they do not, the registration will not go through. If the selling partner is already registered for Sponsored Ads, they cannot register a second account under the same selling partner ID at this time. 

### Step 3 - Get registration status and account details 

Given an account ID, you can check its registration status and get back the associated countries, profiles, and other metadata. To get account and registration status, you will need to poll after a registration request, since registration can take anywhere from 3-30 seconds depending on input parameters. For example, a registration for a selling partner in one country will be much faster than one across 20 countries. We recommend polling every 3-5 seconds to check the account status. It may take up to a few seconds for the profile ID to be populated after account creation.

See [Retrieve accounts](guides/account-management/accounts/retrieve-accounts) to learn how to retrieve account details.

## Next Steps

To set up billing for a newly created account, see [Billing](guides/account-management/billing).

To call an API, for example to create an SD campaign, you must submit the “Profile ID”, not the ads account ID. With the profile ID you can call campaign creation, campaign management, reporting and more APIs. Some APIs do accept the globalized advertising account ID, and these are noted in their documentation. 

[Learn more about profiles.](guides/account-management/authorization/profiles)

>[NOTE]The account ID generated in the registration response is not available in the `/profiles` endpoint, and is not yet compatible with most Amazon APIs during our beta launch of the registration and account APIs. This is a temporary condition during our beta launch of these APIs. 
