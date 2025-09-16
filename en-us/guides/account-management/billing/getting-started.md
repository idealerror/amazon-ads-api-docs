---
title: Getting started with billing APIs
description: Walkthrough of first-time registration for billing through the Amazon Ads API
type: guide
interface: api
keywords:
    - billing registration
    - billing permissions
    - billing API
---

# Getting started with billing APIs

In this document we will walk through the steps needed to set up billing for an Amazon Ads account through public APIs.

This process establishes *who* is going to pay (the [billing profile](guides/account-management/billing/billing-profiles)) for advertising spend on Amazon and *what* payment method they would like to use (the [payment profile](guides/account-management/billing/payments)). This is required before an advertiser can start creating campaigns and advertise on Amazon.

## Prerequisites

We assume that you have already gone through the general [Getting started](guides/get-started/overview) steps for the Amazon Ads API to determine a **client ID** and **access token**, and that you can use the [account management API](guides/account-management/accounts/retrieve-accounts) to retrieve a **global account ID**.

The following HTTP headers need to be passed for every request in this guide:

```
Amazon-Ads-AccountId: '<GLOBAL_ACCOUNT_ID>'
Authorization: 'Bearer <ACCESS_TOKEN>'
Amazon-Advertising-API-ClientId: '<CLIENT_ID>'
Content-Type: 'application/json'
```

Please replace the `<GLOBAL_ACCOUNT_ID>`, `<ACCESS_TOKEN>` and `<CLIENT_ID>` with your actual information. This enables the API to identify the account and authenticate as well as authorize the user trying to access the account.

## Billing profile setup

A billing profile establishes who is going to pay for advertising and contains info like billing address, tax details, and contact info. The billing profile will be used for tax calculation and generating commercial invoices to the advertiser. This can be linked to one or more countries where you are advertising. 

In this section, we will walk through how to use the billing profiles APIs to set up billing for an advertising account. 

>[NOTE]Currently, public APIs are available to set up billing profiles only for non-seller sponsored ads accounts. Sellers will continue to manage their billing information in Seller Central, and may continue to the [payment setup](#payment-setup).

>[TIP]For a logical overview of billing profiles and how they can be used to facilitate reuse of billing information across multiple marketplaces for a global account, see [Billing profiles](guides/account-management/billing/billing-profiles). 

### (Optional) Step 0: Get agreements

You may need to provide consent to the `BILLING_PROFILE_TAX_AGREEMENT` and
`BILLING_PROFILE_CANADIAN_NON_RESIDENCY_AGREEMENT` agreements respectively. This API will return the HTML-formatted content of the agreement in your preferred locale given an agreement name.

This step is only required if:

1. You want to provide a Value Added Tax (VAT) ID, a Goods and Services Tax (GST) number, or the equivalent in your billing profile.
2. You are a non-resident of Canada who wants to advertise in Canada but you do not have a Canadian GST.

```json
URL: GET /billingProfileAgreementContents/{agreementName}
EXAMPLE: GET /billingProfileAgreementContents/BILLING_PROFILE_TAX_AGREEMENT

SAMPLE RESPONSE:
{
    "content": "..."
}
```

[View the technical specifications for the billing profile agreements API.](billing#tag/Billing-Profiles/operation/GetBillingProfileAgreementContent)

After going through the agreement returned from the above call, you can provide your consent while creating the billing profile (in Step 1).

### Step 1: Create a billing profile

#### Sample request

**URL:** POST /billingProfiles/

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

#### Sample response

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

More details on what each request attribute means can be found in our [API specifications](billing#tag/Billing-Profiles). The response contains the ID of the billing profile resource if it was successfully created. Some important points to note:

1. If `isDefault` is set to `true`, the billing profile will be applied to all countries where no previous billing information is configured. In such cases, your billing setup is complete, and you do not need to perform Step 2. You can still execute Step 2 if you would like to override the billing information for a specific country.
2. Advertisers marketing in EU countries may need to configure their “Payer Name” and “Advertiser Name” shown to the customers. To ensure accuracy of the information shown on the store, we request that you review the "Advertiser Name" and "Payer Name" in our ad systems and update them if required. For more details on how to configure this information, see [Billing profiles: Digital Services Act (EU)](guides/account-management/billing/billing-profiles#digital-services-act-eu).

### (Optional) Step 2: Apply billing profile

If you chose not to set the billing profile as default, you can use the billing profile usages API to apply a billing profile to specific marketplaces.

#### Sample request

```json
POST /billingProfileUsages/

{
    "billingProfileUsages": [
        {
            "advertiser": {
                "countryCode": "JP"
            },
            "billingProfileIds": [
                "amzn1.ads-billing-profile.dj3becs448uso2toisb6nqb6q"
            ]
        }
    ]
}
```

#### Sample response

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
                    "amzn1.ads-billing-profile.dj3becs448uso2toisb6nqb6q"
                ],
                "index": 0
            }
        ]
    }
}
```

[View the technical specifications for the billing profile usages API.](billing#tag/Billing-Profiles/operation/ApplyBillingProfile)

The above request successfully associates the billing profile with ID `amzn1.ads-billing-profile.dj3becs448uso2toisb6nqb6q` to the JP marketplace. The billing information for that profile will be used for billing within JP.

## Payment setup 

In this section, we will walk through how to use the payments APIs to set up a payment agreement and payment profile for an advertising account.

>[TIP]For a logical overview of payment management in the Amazon Ads API, see [Payments](guides/account-management/billing/payments). 

### (Optional) Step 0: Get eligible payment methods

This API returns a list of payment methods that are available for registration. 

[View the technical specifications for the payment methods API.](billing#tag/Get-Payment-Methods/operation/GetCustomerPaymentMethods)

This API should be called with `criteriaType` set to `AUTO_PAY` to find what payment methods are eligible for registration when first setting up an advertising account. 

The response from this API can be used for payment method registration in the next step. 

### Step 1: Create a payment agreement

A payment profile is a way to collect payment (i.e, payment instructions), represented by a list of payment methods. A payment agreement represents an agreement between a given advertiser (the target) and Amazon Ads on how Amazon can collect money that it is owed (the payment profile). 

A payment profile can be created independently using the payment profiles API, or we can create the payment profile and agreement as part of one API call. A user can create a payment profile and associate it to an advertising account using the following request body:

```json
POST /billing/paymentAgreements

{
    "paymentAgreements": [{
        "paymentProfile": {
            "paymentMethods": [{
                "type": "PAY_BY_INVOICE",
                "priority": 1
            }]
        },
        "target": {
            "countryCode": "JP",
            "entityId": "ENTITY215J8KS99D",
            "agreementType": "AUTO_PAY"
        }
    }]
}
```

This will set up the payment profile that will be used for collection on invoices that are issued to your advertising account. 

[View the technical specifications for the payment agreements API.](billing#tag/Create-Payment-Agreement)

To find entities and entity IDs for which your account is eligible to create payment agreements, use the [account management API](guides/account-management/accounts/retrieve-accounts) and look for the `entityId` field in the `alternateIds` found in the API response corresponding to the country code you would like to edit.

The result of this API call will be the creation of a payment profile with the specified payment methods and a payment agreement, which applies the new payment profile to the account provided in the `target` attribute. A sample successful response looks as follows: 

```json
{
    "error": [],
    "success": [{
        "id": "amzn1.ads-payment-agreement.54C68A2A6AD0BDA2C72381DCDA325ED7",
        "index": 0
    }]
}
```

#### Setting a profile as global default

Additionally, advertisers can mark a created payment profile as a global default by including the `defaultFor` attribute in the request. When a payment profile is marked as default for a global account it will be automatically applied to all new marketplaces the global account expands to, as well as to all those marketplaces where the global account does not have a payment agreement setup currently. In other words, a payment agreement will automatically be created in the new marketplace with the new advertising account as the target and the global default payment profile. 

This configuration can be specified during payment agreement creation. The following request body outlines what modification needs to be made to the request above in order to specify the payment profile to be the global default. 

```json
{
    "paymentAgreements": [{
        "paymentProfile": {
            "paymentMethods": [{
                "type": "PAY_BY_INVOICE",
                "priority": 1
            }],
            "defaultFor": [{
                "entityType": "GLOBAL_ACCOUNT",
                "entityId": "amzn1.ads-account.1231231"
            }]
        },
        "target": {
            "countryCode": "US",
            "entityId": "ENTITY215J8KS99D",
            "agreementType": "AUTO_PAY"
        }
    }]
}
```

The entity type attribute is set to `GLOBAL_ACCOUNT` and the entity ID should be the global account ID. The value of the account ID should match the value provided in the `Amazon-Ads-AccountId` header of the request, which for global calls should also be the global account ID.

### (Optional) Step 2: Get payment agreements

After completing payment method registration, you can also retrieve the payment agreements that have been registered to your accounts. This can be achieved via the [payment agreements API](billing#tag/Get-Payment-Agreements).

The API call should be made with query parameter `agreementType` set to `AUTO_PAY`. 

The following is a sample response for the API: 

```json
{
    "paymentAgreements": [{
        "id": "amzn1.ads-payment-agreement.XXX1",
        "paymentProfile": {
            "id": "amzn1.ads-payment-profile.XXX",
            "paymentMethods": [{
                "priority": 0,
                "type": "SELLER_PAYABLE"
            }],
            "defaultFor": [{
                "entityType": "GLOBAL_ACCOUNT",
                "entityId": "XXX"
            }]
        },
        "target": {
            "agreementType": "AUTOPAY",
            "countryCode": "US",
            "entityId": "XXX",
            "entityType": "XXX"
        }
    }, {
        "id": "amzn1.ads-payment-agreement.XXX2",
        "paymentProfile": {
            "id": "amzn1.ads-payment-profile.XXX",
            "paymentMethods": [{
                "priority": 0,
                "type": "SELLER_PAYABLE"
            }],
            "defaultFor": [{
                "entityType": "GLOBAL_ACCOUNT",
                "entityId": "amzn1.ads-account.1231231"
            }]
        },
        "target": {
            "agreementType": "AUTOPAY",
            "countryCode": "UK",
            "entityId": "XXX",
            "entityType": "XXX"
        }
    }]
}
```

As shown above, multiple payment agreements are returned, one for each country that the advertising account is active in. These payment agreements all share a common payment profile with ID `amzn1.ads-payment-profile.XXX`.

## Next steps

To learn how to list or modify billing profiles and billing profile usages as needed, see [Billing profiles](guides/account-management/billing/billing-profiles).

To learn more about setting up payments for a billing profile via the API, see [Payments](guides/account-management/billing/payments).

To learn more about invoices, see [Invoices](guides/account-management/billing/invoices).
