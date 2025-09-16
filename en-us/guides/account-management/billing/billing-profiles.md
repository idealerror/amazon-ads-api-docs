---
title: Billing profiles overview
description: A guide to billing profiles in the Amazon Ads API
type: guide
interface: api
keywords:
    - billing profiles
    - billing permissions
    - billing API
    - Digital Services Act
    - European Union law
---

# Billing profiles

An advertiser needs to configure their billing information (billing address, taxes, email) before they can start creating campaigns to advertise on Amazon. With the launch of the billing profiles public APIs, integrators can configure billing information for sponsored ads vendor accounts without having to interact with the ads console.

## Prerequisites

To operate on these public APIs you need to have a global sponsored ads account. Refer to the [account management guide](guides/account-management/accounts/retrieve-accounts) to find the account ID for your sponsored ads account.

>[NOTE]This API does not support seller or ADSP accounts. For seller accounts, use the Seller Central UI to configure your billing details. For ADSP accounts, use the ads console UI to configure your billing details.

## Billing profile & billing profile usage

A **billing profile** establishes who is going to pay for advertising and contains info like billing address, tax details, contact info. The billing profile will be used for tax calculation and generating commercial invoices for advertiser spend. 

A billing profile can be linked to one or more countries where you are advertising. The link between the billing profile and a country is called a **billing profile usage**.

![billing profile diagram](/_images/billing/billing-profile.png)

In the example above, the account is configured to advertise in the United States, Japan, the United Kingdom, and Australia. One billing profile has been linked to all four countries. Any update to the billing profile is reflected across all countries.

>[NOTE]A country can also be linked to multiple billing profiles. Multiple billing profiles for a country enable advertisers marketing in EU countries to set the “Advertiser Name” shown on the store. For more information, see [Digital Services Act (EU)](#digital-services-act-eu). 

An advertiser can create multiple billing profiles and link them to different countries, which allows different billing information for different countries:

![multiple billing profiles](/_images/billing/multiple-billing-profiles.png)

Advertisers can also mark any billing profile as default, which will link that billing profile to all countries where no billing information is present:

![default billing profile](/_images/billing/default-profile.png)

### Digital Services Act (EU)

In some European Union countries, as per the Digital Services Act, Amazon shows the **Advertiser name** and the **Payer name** for an ad to consumers. To ensure accuracy of the information shown on the store, we request that you review the **Advertiser name** and **Payer name** in our ad systems and update them if required.

![Digital Services Act UI](/_images/billing/digital-services-act-ui.jpg)

By default, when an advertiser links a billing profile to a country, both **Advertiser name** and **Who paid for this ad** will display the `billingName` provided in the billing profile. If you want to show a different value as **Advertiser name**, you can link multiple billing profiles for that marketplace. In such cases, the value of `billingName` in the billing profile with `isBillTo` set as `false` will be used to show the **Advertiser name**, while the value of `billingName` in the billing profile with `isBillTo` set as `true` will be used to show **Who paid for this ad**. Please note that invoicing, taxation, and other billing workflows will continue to use the billingProfile with `isBillTo` set as `true`.

![isBillTo, billingName, advertiser name, and "Who paid for this ad?" behavior](/_images/billing/isBillTo-behavior.png)

## Billing profile public APIs

Amazon provides a set of APIs for programmatically managing an advertiser’s billing profiles and their billing profile usages. 

[View the technical specifications for billing profiles in the Amazon Ads API.](billing#tag/Billing-Profiles)

Required permissions are listed for each API operation below. Note that the user whose permission levels are checked is the Amazon user account that provided authorization for the access token used in the request. For more information, see [Access tokens](guides/account-management/authorization/access-tokens).

### [POST /billingProfiles](billing#tag/Billing-Profiles/operation/CreateBillingProfiles)

Creates new billing profile(s) under a sponsored ads account. A billing profile can exist independent of the countries present in the global account.

**Permissions**

1. The user needs to have **Edit** access to **Payment settings** for at least one country in the global account.
2. If the user wants to create a default billing profile, they need to have **Edit** permission for **Payment settings** for _all_ countries in the global account.

**Sample Request**

```json
{
    "billingProfiles": [
        {
            "billingProfileName": "JP Profile",
            "isDefault": false,
            "isBillTo": true,
            "billingParty": "BRAND_OWNER",
            "address": {
                "billingName": "John Doe",
                "contactName": "John Doe",
                "addressLine1": "Address Line 1",
                "postalCode": "98101",
                "city": "Seattle",
                "countryCode": "US",
                "primaryEmail": "john.doe@email.com",
                "stateOrRegion": "Washington"
            },
            "taxes": [
                {
                    "type": "VAT",
                    "value": "#########"
                }
            ],
            "agreements": [
                {
                    "consent": true,
                    "documentName": "BILLING_PROFILE_TAX_AGREEMENT",
                    "locale": "en_US"
                }
            ]
        }
    ]
}
```

**Notes:**

1. If `isDefault` is set to `true`, the given billing profile will be auto-linked to all countries where no billing profile is applied.
2. Please note that the values in `billingParty` and `isBillTo` once set cannot be changed for that billing profile. However, you can always create a new billing profile with the changed values and apply that to the desired countries.
3. Consent to `BILLING_PROFILE_TAX_AGREEMENT` is required if you provide Value Added Tax (VAT), Goods and Services Tax (GST) or equivalent number along with your billing address.
4. Consent to `BILLING_PROFILE_CANADIAN_NON_RESIDENCY_AGREEMENT` is required if you are a non-resident of Canada who wants to advertise in Canada but do not have a Canadian GST.
5. The content of the above agreements can be fetched and viewed by calling the [agreements API](#get-billingprofileagreementcontentsbillingprofileagreementcontentid).
6. If you are advertising in EU countries, please ensure that the advertiser name and payer names are correctly configured. Refer to [Digital Services Act (EU)](#digital-services-act-eu).

**Sample Response** 

```json
{
    "billingProfiles": {
        "error": [],
        "success": [
            {
                "billingProfileId": "amzn1.ads-billing-profile.dj3becs448uso2toisb6nqb6q",
                "index": 0
            }
        ]
    }
}
```

The response returns all billing profiles that were successfully created. In case of an error, please review the message provided in the error and correct any invalid input present. The `index` represents the index of the billing profile in the input array corresponding to particular success or error.

### [PUT /billingProfiles](billing#tag/Billing-Profiles/operation/UpdateBillingProfiles)

Updates one or more billing profiles under a global account. The API does-not support partial update requests. On successful update, the updated billing informations is automatically reflected across all the countries where the billing profile is associated. 

**Permissions**

1. The user needs to have **Edit** access to **Payment settings** for all the countries where the billing profile is linked.
2. To create a "default" billing profile, the user must have **Edit** access to **Payment settings** for all countries in the global account.

**Sample Request**

```json
{
    "billingProfiles": [
        {
            "billingProfileName": "JP Profile",
            "billingProfileId": "amzn1.ads-billing-profile.dj3becs448uso2toisb6nqb6q",
            "billingParty": "BRAND_OWNER",
            "address": {
                "billingName": "John Doe",
                "contactName": "John Doe",
                "addressLine1": "Address Line 1",
                "postalCode": "98101",
                "city": "Seattle",
                "countryCode": "US",
                "primaryEmail": "john.doe@email.com",
                "stateOrRegion": "Washington"
            },
            "taxes": [
                {
                    "type": "VAT",
                    "value": "#########"
                }
            ],
            "agreements": [
                {
                    "consent": true,
                    "documentName": "BILLING_PROFILE_TAX_AGREEMENT",
                    "locale": "en_US"
                }
            ]
        }
    ]
}
```

**Sample Response** 

```json
{
    "billingProfiles": {
        "error": [],
        "success": [
            {
                "billingProfileId": "amzn1.ads-billing-profile.dj3becs448uso2toisb6nqb6q",
                "index": 0
            }
        ]
    }
}
```

### [POST /billingProfiles/list](billing#tag/Billing-Profiles/operation/GetBillingProfiles)

Fetches billing profiles present under the global account. You can limit the search results by providing filters like `defaultBillingProfileFilter` and `billingProfileIdFilter`, in which case only results matching the filters will be returned. The response is paginated and includes a pagination token (`nextToken`) if more than 50 results match. You can customize the page size to be less than 50 by providing the `maxResults` key in request.

**Permissions**

1. User needs to have **View** access to **Payment settings** for at least one country in their global account.

**Sample Request**

```json
{
    "filters": {
        "billingProfileIdFilter": [
            "amzn1.ads-billing-profile.4k0vdxn50016iqyv83u9cvdj5"
        ]
    }
}
```

**Sample Response**

```json
{
    "billingProfiles": [
        {
            "address": {
                "addressLine1": "Address Line 1",
                "addressLine2": "Address Line 3",
                "billingName": "Nike",
                "city": "Cadaca",
                "contactName": "Saumitra Dey 2",
                "countryCode": "JP",
                "postalCode": "213112",
                "primaryEmail": "smit.new1@gmail.com",
                "secondaryEmails": [],
                "stateOrRegion": "Kyoto"
            },
            "agreements": [
                {
                    "consent": true,
                    "documentName": "BILLING_PROFILE_TAX_AGREEMENT",
                    "locale": "en_US"
                }
            ],
            "billingParty": "BRAND_OWNER",
            "billingProfileId": "amzn1.ads-billing-profile.4k0vdxn50016iqyv83u9cvdj5",
            "billingProfileName": "JP Profile 2",
            "isBillTo": true,
            "isDefault": true,
            "taxes": [
                {
                    "type": "VAT",
                    "value": "T1223344556677"
                }
            ]
        }
    ],
    "maxResults": 1
}
```

### [POST /billingProfileUsages/list](billing#tag/Billing-Profiles/operation/GetBillingProfileUsages)

Lists the billing profiles linked to each country of global ads account. You can further narrow down the search by providing the countries you want the billing profiles for.

**Permissions**

1. The API will only return information for countries where user has **View** permission for **Payment settings**.

**Sample Request**

```json
{
    "expandBillingProfile": true,
    "expandFallbackBillingProfile": true,
    "filters": {
            "advertiserFilter": [{
                "countryCode": "JP"
        }]
    }
}
```

1. Setting `expandBillingProfile` to `true` returns the full billing profile details along with the billing profile ID in the response.
2. Setting `expandFallbackBillingProfile` to `true` provides the current billing information being used for invoicing, taxation, etc. The fallback can be different from the billing profile when the status of the billing profile usage is not `OK`.
3. The `filters` field allows this API to further narrow down the search. In this case, the result will only return the billing profile usage associated with the JP marketplace.

**Sample Response:**

```json
{
    "billingProfileUsages": [
        {
            "advertiser": {
                "countryCode": "JP"
            },
            "billingProfileUsageId": "amzn1.ads-billing-profile-usage.sa.jp.CV63SBI12VVI",
            "billingProfiles": [
                {
                    "address": {
                        "addressLine1": "Address Line 1",
                        "addressLine2": "Address Line 2",
                        "billingName": "Nike",
                        "city": "Cadaca",
                        "contactName": "Saumitra Dey",
                        "countryCode": "JP",
                        "postalCode": "213112",
                        "primaryEmail": "smit.new1@gmail.com",
                        "secondaryEmails": [],
                        "stateOrRegion": "Kyoto"
                    },
                    "agreements": [
                        {
                            "consent": true,
                            "documentName": "BILLING_PROFILE_TAX_AGREEMENT",
                            "locale": "en_US"
                        }
                    ],
                    "billingParty": "BRAND_OWNER",
                    "billingProfileId": "amzn1.ads-billing-profile.6kgxscrphc7adaeer5u8ckpd3",
                    "billingProfileName": "JP Profile 2",
                    "isBillTo": true,
                    "isDefault": false,
                    "taxes": [
                        {
                            "type": "VAT",
                            "value": "T1223344556677"
                        }
                    ]
                }
            ],
            "fallbackBillingProfiles": [
                {
                    "address": {
                        "addressLine1": "Address Line 1",
                        "addressLine2": "Address Line 2",
                        "billingName": "Nike",
                        "city": "Cadaca",
                        "contactName": "Saumitra Dey",
                        "countryCode": "JP",
                        "postalCode": "213112",
                        "primaryEmail": "smit.new1@gmail.com",
                        "secondaryEmails": [],
                        "stateOrRegion": "Kyoto"
                    },
                    "billingParty": "BRAND_OWNER",
                    "billingProfileName": "Saumitra Dey",
                    "isBillTo": true,
                    "isDefault": false,
                    "taxes": [
                        {
                            "type": "VAT",
                            "value": "T1223344556677"
                        }
                    ]
                },
                {
                    "address": {
                        "addressLine1": "Address Line 1",
                        "addressLine2": "Address Line 2",
                        "billingName": "Nike",
                        "city": "Cadaca",
                        "contactName": "Saumitra Dey",
                        "countryCode": "JP",
                        "postalCode": "213112",
                        "primaryEmail": "smit.new1@gmail.com",
                        "secondaryEmails": [],
                        "stateOrRegion": "Kyoto"
                    },
                    "billingParty": "BRAND_OWNER",
                    "billingProfileName": "Saumitra Dey",
                    "isBillTo": false,
                    "isDefault": false
                }
            ],
            "status": {
                "statusCode": "OK",
                "statusMessage": "Billing Profile Usage is active"
            }
        }
    ],
    "maxResults": 1
}
```

### [POST /billingProfileUsages](billing#tag/Billing-Profiles/operation/ApplyBillingProfile)

Links one or more countries with a billing profile. The linked billing profile's billing information will be used for invoicing, taxes, and other billing workflows. A single billing profile can be linked to multiple countries.

**Permissions**

1. The user needs to have **Edit** access to **Payment settings** for the marketplace to which the profile will be linked.

**Sample Request**

```json
{
    "billingProfileUsages": [
        {
            "advertiser": {
                "countryCode": "JP"
            },
            "billingProfileIds": [
                "amzn1.ads-billing-profile.6kgxscrphc7adaeer5u8ckpd3"
            ]
        }
    ]
}
```

**Notes**

1. The above request will link the billing profile with ID `amzn1.ads-billing-profile.6kgxscrphc7adaeer5u8ckpd3` to the `JP` marketplace for the account.
2. There can be one and only one `isBillTo=true` billing profile in the `billingProfileIds` list for a country.
3. There can be no more than one `isBillTo=false` billing profile in `billingProfileIds` list for a country.

**Sample Response**

```json
{
    "billingProfileUsages": {
        "error": [],
        "success": [
            {
                "advertiser": {
                    "countryCode": "JP"
                },
                "billingProfileIds": [
                    "amzn1.ads-billing-profile.6kgxscrphc7adaeer5u8ckpd3"
                ],
                "index": 0
            }
        ]
    }
}
```

### [GET /billingProfileAgreementContents/{billingProfileAgreementContentId}](billing#tag/Billing-Profiles/operation/GetBillingProfileAgreementContent)

In some cases, the user needs to provide consent to certain agreements before creating a billing profile. This API provides a way for users to access the agreement content.

**Permissions**

1. User needs to have **View** access to **Payment settings** for at least one country in the advertising account.

**Sample Request/Response**

```json
URL: GET /billingProfileAgreementContents/{agreementName}?languageOfPreference={languageOfPreference}

EXAMPLE: GET /billingProfileAgreementContents/BILLING_PROFILE_TAX_AGREEMENT

SAMPLE RESPONSE:
{
    "content": "..."
}
```

You can customize the locale you want to fetch the agreement in by providing the `languageOfPreference` query parameter. The default locale is `en_US`. A list of supported locales is provided in the [API specification](billing#tag/Billing-Profiles/operation/GetBillingProfileAgreementContent).
