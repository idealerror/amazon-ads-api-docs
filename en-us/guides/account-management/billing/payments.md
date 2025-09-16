---
title: Payments overview
description: A guide to managing payment methods for billing through the Amazon Ads API
type: guide
interface: api
keywords:
    - payment methods
    - advertising invoices
    - billing permissions
    - billing API
---

# Payment registration, eligibility, and execution

## Logical Data Model

![Logical data model for payments](/_images/billing/payments-logical-data-model.png)

A single payment method is a payment instrument that can be used to attempt collection. 

A payment profile can contain one or many payment methods. 

A payment profile can be shared by one or many payment agreements. 

A payment agreement contains a single payment profile and a single target which indicates the entity (often an advertising account) to apply this payment profile to. This forms an agreement with the target entity indicating how payment can be collected. 

## Prerequisites

The payments public APIs are set up to support both globally scoped and non-globally scoped service calls. 

### Headers

This guide assumes that you have already gone through [general getting started guide](guides/get-started/overview) to obtain your **client ID** and **access token**. This information will be required as headers in the subsequent calls and would be used to authenticate and authorize the request. The following HTTP headers need to be passed for every request:

```
Authorization: 'Bearer <ACCESS_TOKEN>'
Amazon-Advertising-API-ClientId: '<CLIENT_ID>'
Content-Type: 'application/json'
```

Please replace the `<ACCESS_TOKEN>` and `<CLIENT_ID>` with your actual information. This enables the API to identify the account and authenticate as well as authorize the user trying to access the account.

Additionally, global calls require an additional header:

```
Amazon-Ads-AccountId: '<GLOBAL_ACCOUNT_ID>'
```

The `<GLOBAL_ACCOUNT_ID>` corresponds to your global account ID. You can use the [account management API](guides/account-management/accounts/retrieve-accounts) to retrieve a **global account ID**.

## APIs

### [POST /billing/paymentMethods/list](billing#tag/Get-Payment-Methods)

This API returns a list of payment methods that an advertiser is eligible to register, i.e., create a profile using. Clients can directly use the payment methods returned from this API to create payment profiles and register these payment methods to advertising accounts.

A sample response of this API is as follows:

```json
{
    "deductFromPaymentPaymentMethods": [],
    "payByInvoicePaymentMethods": [],
    "sellerPayablePaymentMethods": [{
        "type": "SELLER_PAYABLE"
    }],
    "creditCardPaymentMethods": []
}
```

Each list includes payment methods that the advertiser currently is eligible to use to create a payment agreement. 

### [POST /billing/paymentAgreements](billing#tag/Create-Payment-Agreement)

This API supports creating payment agreements for a given advertiser. You can use this API to update or create a payment agreement for one or many advertising accounts. 

The caller of this API can provide a `paymentAgreementId` in the request in order to update an existing payment agreement, or leave this field blank to create a new payment agreement. A given target can only have one payment agreement associated to it, so once a target has a payment agreement associated to it, the caller must update that agreement and must not attempt to create a new agreement. 

When the caller defines a payment profile as part of their agreement creation request, they can either provide a `paymentProfileId` which makes it so that the payment agreement being created/updated will point to an existing payment profile; or the caller can leave this field blank and define a new payment profile in the request, which will create a new payment profile and associate this agreement to it. When defining a profile, the caller is able to specify a list of payment methods and allocate priorities to these payment methods for how they want them to be used during payment execution. If one payment method fails, we will attempt payment on the next lowest priority payment method. 

A sample request that can be sent to this API is as follows: 

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
            "countryCode": "US",
            "entityId": "ENTITY215J8KS99D",
            "agreementType": "AUTO_PAY"
        }
    }]
}
```

### [POST /billing/paymentAgreements/list](billing#tag/Get-Payment-Agreements)

This API allows clients to retrieve payment agreements that are currently active for a given advertising entity. These payment agreements include the current payment agreement that is being used to automatically try to collect on the advertisers invoices.

A sample response that a customer can expect from this API is as follows:

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
            "agreementType": "AUTO_PAY",
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
            "agreementType": "AUTO_PAY",
            "countryCode": "UK",
            "entityId": "XXX",
            "entityType": "XXX"
        }
    }]
}
```

### [POST /billing/invoices/pay](billing#tag/Pay-Invoices)

This API allows clients to initiate payments on their currently unpaid or open invoices. The API returns details regarding which invoices it failed to collect payment on, a human-readable reason for the failure, and list of invoices for which it successfully collected payment. As a note, this API does not support globally scoped calls today. 

The request body for this API is as follows:

```json
{
  "idempotenceId": "testUnEncryptedId",
  "reasonLocale": "en_US"
}
```

The response includes a list of invoices for which payment was attempted and a reason indicating why a payment may have failed. 

```json
{"error": 
  [
    {"invoiceCFID": "TRG32DAR-1942", 
     "reason": "Your seller or vendor account has insufficient balance. Update your payment method in payment settings."}
  ], 
 "success": 
  []
}
```

### [POST /billing/paymentProfiles](billing#tag/Create-Payment-Profiles/operation/CreatePaymentProfiles)

This API allows the creation or update of payment profiles. 

The caller of this API can provide a `paymentProfileId` in the request in order to update an existing payment profile. Updating an existing payment profile will affect all payment agreements that are using this payment profile. Alternatively, the client can omit this field to create a new payment profile and use the payment agreement API to then point an existing payment agreement or create a new payment agreement that uses the newly created payment profile. 

When defining a profile, the caller can specify a list of payment methods and allocate priorities to these payment methods to specify how these methods should be used during payment execution. If one payment method fails, we will attempt payment on the next lowest priority payment method. 

Clients can additionally specify `defaultFor` when creating a payment profile. Currently, clients can only specify their global advertising account as the `defaultFor`. Setting a payment profile as the `defaultFor` for a global advertising account will mean that all advertising accounts that get created under this global account (as the global account expands to new marketplaces) will use the same payment profile, and payment setup is not needed separately for new marketplaces.

Clients can also specify `eligibleEntities` for the payment profile. `eligibleEntities` controls which all accounts are eligible to use and update a given payment profile. Currently only the global advertising account can be set as an eligible entity, i.e., a payment profile can only be used and updated by different advertising accounts belonging to a global advertising account.

A sample request to this API is as follows:

```json
POST /billing/paymentProfiles

{
    "paymentProfiles": [{
        "paymentProfileId": "amzn1.ads-payment-profile.XXX",
        "paymentMethods": [{
            "type": "PAY_BY_INVOICE",
            "priority": 0
        }],
        "defaultFor": [{
            "entityType": "GLOBAL_ACCOUNT",
            "entityId": "XXX"
        }]
    }]
}
```

## Advanced Use-Case Reference

### Updating Shared Payment Profiles

A payment profile can be shared by multiple payment agreements. The [payment profile creation API](billing#tag/Create-Payment-Profiles) allows us to update these shared profiles so that all agreements that reference this shared profile will now be using the new version of it.

The following is a sample request body that would update the profile that is being shared by the agreements in the example above. 

```json
{
    "paymentProfiles": [{
        "paymentProfileId": "amzn1.ads-payment-profile.XXX",
        "paymentMethods": [{
            "type": "PAY_BY_INVOICE",
            "priority": 0
        }],
        "defaultFor": [{
            "entityType": "GLOBAL_ACCOUNT",
            "entityId": "XXX"
        }]
    }]
}
```

As shown above, the payment profile ID is provided in the request. When the ID is provided, the API will update an existing payment profile. If the ID is not provided, then the API will create a new payment profile and generate a new ID for it. After this request is made, a subsequent [list payment agreements](billing#tag/Get-Payment-Agreements) call would return the same set of agreements, except they would be updated to use the `PAY_BY_INVOICE` payment method due to the update that was made to their shared profile. 

### Updating a payment agreement to use a new payment profile

In addition to updating a payment profile, we can also use the payment profiles API to create a new payment profile. The corresponding request body for this operation would be: 

```json
{
    "paymentProfiles": [{
        "paymentMethods": [{
            "type": "PAY_BY_INVOICE",
            "priority": 0
        }]
    }]
}
```

This would create a new payment profile and return a response that indicates whether the creation was successful, and what the corresponding ID of the created payment profile is: 

```json
{
    "error": [],
    "success": [
        {
            "id": "amzn1.ads-payment-profile.XXX123"
        }
    ]
}
```

Using this payment profile ID, we are then able to use the payment agreement API to create/update a payment agreement that points to this profile.

```json
{
    "paymentAgreements": [{
        "paymentAgreementId": "amzn1.ads-payment-agreement.XXX1",
        "paymentProfile": {
            "paymentProfileId": "amzn1.ads-payment-profile.XXX123"
        },
        "target": {
            "countryCode": "US",
            "entityType": "XXX",
            "entityId": "XXX",
            "agreementType": "AUTO_PAY"
        }
    }]
}
```

This request would update the payment agreement with ID `amzn1.ads-payment-agreement.XXX1` to use the payment profile with ID `amzn1.ads-payment-profile.XXX123` which corresponds to the `PAY_BY_INVOICE` payment method. 

A subsequent get payment agreements call, based on the example above, would yield the following result: 

```json
{
    "paymentAgreements": [{
        "id": "amzn1.ads-payment-agreement.XXX1",
        "paymentProfile": {
            "id": "amzn1.ads-payment-profile.XXX123",
            "paymentMethods": [{
                "priority": 0,
                "type": "PAY_BY_INVOICE"
            }]
        },
        "target": {
            "agreementType": "AUTO_PAY",
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
                "entityId": "XXX"
            }]
        },
        "target": {
            "agreementType": "AUTO_PAY",
            "countryCode": "UK",
            "entityId": "XXX",
            "entityType": "XXX"
        }
    }]
}
```

This response reflects the change for the payment agreement in the US to use the new payment profile which was created in the previous step. 
