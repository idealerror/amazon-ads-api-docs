---
title: Manager accounts in the Amazon Ads API
description: Overview of manager accounts behavior for authorization, profiles, sponsored ads, and DSP reporting in the Amazon Ads API.
type: guide
interface: api
tags:
    - Authorization
    - Account management
    - Reporting
    - DSP
keywords:
    - linked account
    - third-party access
    - view only
    - profiles
    - global
---

# Manager accounts

Manager accounts enable advertisers and their agencies to link multiple advertising accounts. In the advertising console, this means they can manage users, handle billing, and view account-level alerts, insights, and performance metrics in a single dashboard.

- Learn more about manager accounts in the [advertising console support center](https://advertising.amazon.com/help?ref_=a20m_us_blg#GU3YDB26FR7XT3C8).

You can create manager accounts in the console or via the `POST /managerAccounts` endpoint in the Amazon Ads API. Any Amazon user account can create a manager account. An Amazon user account can have more than one manager account.

## Authorization

To use manager accounts in the Amazon Ads API, a client application can provide an access token representing authorization from an Amazon user account that has access to a manager account. This access token authorizes the client application to make requests to the API on behalf of the manager account’s linked accounts.

- Learn more about [authorization in the Amazon Ads API](guides/account-management/authorization/overview).
- Learn more about [Login with Amazon authorization grants](guides/account-management/authorization/authorization-grants).

### Permissions

Linked sponsored ads accounts owned by a third party can choose in the console whether to enable **Editor** or **Viewer** permissions for the manager account. A manager account with **Editor** permission for a linked account may perform any operation in the API on behalf of that account. 

A manager account with **Viewer** permission can access data and generate reports, but will receive a `401 Unauthorized` response to requests that require **Editor** permission.

### Profiles

The profile ID for a specific linked account can be used in requests to the API to access data and services for that account. The profile ID for each linked sponsored ads account is included in the response from the `GET /managerAccounts` endpoint.

In addition, the `GET /profiles` endpoint returns all sponsored ads advertising accounts that have given **Editor** permission to a manager account owned by the authorized user, as well as any profiles directly owned by the user account.

- Learn more about [profiles](guides/account-management/authorization/profiles).

>[TOPIC:Are manager accounts global?]Manager accounts can accept linked accounts in any marketplace or combination of marketplaces.<br><br>In the API, the manager account will only have access to a given linked account when calling the API host in the linked account’s region. Requests by a manager account to `GET /profiles` and `GET /managerAccounts` will return only those profiles whose marketplace falls within the region covered by the API host to which the call is addressed.<ul style="margin-top: 1em;"><li>Learn more about [regional behavior for profiles](guides/account-management/authorization/profiles#regions).</li></ul>

### Managed DSP accounts

Managed service DSP advertisers need to use a manager account to call the reporting API. This process enables entity administrators to grant read-only permissions to a manager account for one or more advertisers within that entity.

Linked DSP accounts are returned from the `GET /managerAccounts` endpoint with an `accountType` of “DSP\_ADVERTISING\_ACCOUNT”. Use the `dspAdvertiserId` field of a linked account as the account ID for a DSP reporting request.

- Learn more about [API authorization for managed service DSP accounts](guides/reporting/dsp/reporting-by-account-type#managed-service-accounts).

>[TOPIC:Where else can I use manager accounts?]Manager accounts provide access only to Amazon Ads data and services via the Amazon Ads API or console. To access other services for an Amazon user account, such as the Selling Partner API, use the authentication methods documented for those services.

## API operations

### Create a manager account

To create a new manager account for the authorizing user account, make a request to `POST /managerAccounts`, providing values for `managerAccountName` and `managerAccountType` (either “Advertiser” or “Agency”) in the request body:

```json
{
    "managerAccountName": "string",
    "managerAccountType": "Advertiser"
}
```

Each manager account is created without linked accounts.

- View the [technical specifications for `POST /managerAccounts`](reference/1-0/managerAccount#tag/Manager-Accounts/operation/createManagerAccount).

### List manager accounts

The response from `GET /managerAccounts` lists all manager accounts for the authorizing user account. Each manager account includes a list of up to 50 linked accounts. 

Example response for a user with one manager account and one linked sponsored ads account:

```json
{
    "managerAccounts": [
        {
            "linkedAccounts": [
                {
                    "accountId": "ENTITYxxxxxxxxxxx",
                    "accountName": "AdsCustomer, Inc.",
                    "accountType": "SELLER",
                    "dspAdvertiserId": "",
                    "marketplaceId": "ATVPDKIKX0DER",
                    "profileId": "xxxxxxxxxxxxxxxx"
                }
            ],
            "managerAccountId": "amzn1.ads1.ma1.xxxxxxxxxxxxxxxxxxxxxx",
            "managerAccountName": "my_manager_account"
        }
    ]
} 
```

- View the [technical specifications for `GET /managerAccounts`](reference/1-0/managerAccount#tag/Manager-Accounts/operation/getManagerAccountsForUser).

>[TIP:Managing manager accounts]The API returns a maximum of 50 linked accounts for each manager account. To administer more than 50 linked accounts via the API, you can create additional manager accounts for the same user account.

### Associate or disassociate linked accounts

Manager accounts can most easily be associated with linked accounts via the advertising console. For instructions on linking accounts to a manager account, visit [“Add accounts to your manager account“ in the console support center](https://advertising.amazon.com/help?ref_=a20m_us_blg#G23QYEJNEX3FJFH5). 

The API also has two endpoints for managing account associations:

- See the [specifications for `POST /managerAccounts/{managerAccountId}/associate`](reference/1-0/managerAccount#tag/Manager-Accounts/operation/LinkAdvertisingAccountsToManagerAccountPublicAPI).
- See the [specifications for `POST /managerAccounts/{managerAccountId}/disassociate`](reference/1-0/managerAccount#tag/Manager-Accounts/operation/UnlinkAdvertisingAccountsToManagerAccountPublicAPI).

To use the `POST /managerAccounts/{managerAccountId}/associate` endpoint, the user account associated with the access token must have admin access to both the advertiser account and the manager account. 

>[TIP]The accounts that can be associated via this endpoint correspond to the manager account’s **Your accounts** view under **Link accounts** in the console.

For the  `POST /managerAccounts/{managerAccountId}/disassociate` endpoint, only permission for the manager account is required.

