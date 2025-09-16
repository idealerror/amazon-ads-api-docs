---
title: Permissions
description: Overview of permissions in the Amazon Ads API
type: guide
interface: api
tags:
    - Authorization
    - Account management
keywords:
    - access
    - headers
    - Amazon-Advertising-API-ClientId
    - Amazon-Advertising-API-Scope
    - unauthorized
---

# Permissions

A user account's permission levels for a given advertising account determine that user's ability to either view or edit that advertising account.

In the advertising console, the permissions set for the logged-in user account affect the behavior of the user interface. 

In the Amazon Ads API, the permissions set for the user account that logged in to [create the authorization grant](guides/get-started/create-authorization-grant) determine whether a given request will be authorized.

## Permissions and profiles

The profile ID of an advertising account is required to complete most requests to the API. By default, the `GET /profiles` endpoint displays only profiles with **View and Edit** permissions. The response can be adjusted using the `apiProgram` and `accessLevel` query parameters in the request.

For example, the following request would return a list of profiles in North America for which the authorizing account has **View** permissions for reports:

```
https://advertising-api.amazon.com/v2/profiles?accessLevel=view&apiProgram=report
```

For more detail, [view the technical specifications for profiles](reference/2/profiles).

>[TIP:Manager accounts]For permissions determined by manager accounts, the same filter behavior applies to profiles. Manager account users can also view profile IDs for all linked accounts using the `GET /managerAccounts` endpoint.

## Response status

Requests to the API for which the authorizing user account has permission to perform the requested action for the account whose profile ID is provided in the `Amazon-Advertising-API-Scope` header will be authorized.

Requests will receive a `401 Unauthorized` response if the authorizing account does not have the required permissions.

## Setting permissions

By default, the creator of a sponsored ads account has **Admin** permissions. In the advertising console, users with **Admin** access can enable other user accounts to access an advertising account by setting permissions. 

Admin users can choose from two methods to enable access for other user accounts: manager accounts and access controls.

### Manager accounts

Advertisers can allow access to a manager account, which can be assigned either **Viewer** or **Editor** permission. [Learn more about manager accounts.](guides/account-management/authorization/manager-accounts)

### Access controls

Advertisers can use [access controls](https://advertising.amazon.com/help/?GDQVHVQMY9F88PCA#GDQVHVQMY9F88PCA) to invite another user account to the advertising account and determine that user account's permission level.

#### For vendor accounts

For vendor accounts, permission can be shared in the advertising console according to three default tiers:

- **Viewer:** grants ability to view content and existing campaigns.
- **Editor:** grants viewer priveleges plus the ability to change content, create new ads, create portfolios, and manage existing campaigns.
- **Admin:** grants editor privileges plus the ability to change payment settings and add or remove users.

Alternatively, admin users can choose to grant granular permissions to a given user across six permission fields. Four of these fields affect behavior in the API. They are listed below, along with the names for the **View** and **View and Edit** permissions of each type as described in ["Permissions definitions" in the support center](https://advertising.amazon.com/help/?GDQVHVQMY9F88PCA#GR9KP77YZUD3UEZD). 

| Name in console | View permission name | View and Edit permission name | Enables |
| --------------- | ---- | ------------- | ------- |
| Campaign manager | `advertiser_campaign_view` | `advertiser_campaign_edit` | Campaign management endpoints |
| Advertising reports | `nemo_report_view` | `nemo_report_edit` | Reporting endpoints |
| Stores builder | N/A | `amazon_stores_edit` | [Stores](reference/2/stores) endpoints |
| Billing history | `nemo_transactions_view` | `nemo_transactions_edit` | [Billing](guides/account-management/billing) endpoints |

>[NOTE]Permissions for payment settings and user management do not affect the behavior of the API.

#### For seller accounts

For seller accounts, admin users can manage granular permissions in Seller Central after inviting another user. **View**, **View & Edit**, and **Admin** permissions for seller accounts behave as described above for vendor accounts for those aspects of a seller's account that affect the behavior of the API, including:

- Campaign manager
- Product Ads Performance Reports
- Product Ads Invoice History
- Stores builder
