---
title: Creating ADSP Reports via API
description: Understand how to create Amazon DSP reports using the offline report API.
type: guide
interface: api
tags:
    - Reporting
    - DSP
---

# Pulling Amazon DSP Reports via API


There are two methods of pulling Amazon DSP reports using our [offline report API](offline-report-prod-3p). The method you use depends on whether your advertiser’s account permission is **Admin** or **ADSP Reports** within your manager account. 


|Account permission	|Definition	|Advertiser-to-Manager-Account relationship	|
|---	|---	|---	|
|Admin	|The advertiser was created by this manager account and has a billing relationship with this manager account. An advertiser can only have an "Admin" relationship with one manager account.	|many-to-one	|
|ADSP Reports	|The advertiser was linked to the manager account and does not have a billing relationship with this manager account. An advertiser can have an "ADSP Reports" relationship with multiple manager accounts. This method can be used to pull reports for managed service advertisers.	|many-to-many	|

## Admin  

When the account permission is **Admin**, this means this manager account created this advertiser. To use this method, your Manager Account will need to have **Admin** account permission for the advertiser. To check this, click on the **Accounts** tab and look at the **Account permission** column. It should say **Admin** next to the advertiser you want to pull reports for.

![Admin Account Permission](/_images/reporting/admin-account-permission.png)

Process for report API:

1. Make sure your Login with Amazon application is [authorized by a DSP user](getting-started/create-authorization-grant) on a Manager Account that has Admin account permission for the advertiser.
2. Retrieve your profileId using the [GET/v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint.

>[NOTE] V2 profiles is used here for making V3 API calls. 

    -  Sample response:

    ```
    [
    {
        "profileId": 1111111111111111,
        "countryCode": "US",
        "currencyCode": "USD",
        "timezone": "America/Los_Angeles",
        "accountInfo": {
        "marketplaceStringId": "ATVPDKIKX0DER",
        "id": "ENTITY2XYZ1234",
        "type": "agency",
        "name": "Demo: US"
        }
    }
    ]
    ```

    -  In the above example, the profileId is `1111111111111111`.  Note that ADSP profiles will always be of type `agency`. 



3. Pull the list of advertisers using [GET/dsp/advertisers](dsp-advertiser#tag/Advertiser/paths/~1dsp~1advertisers/get). Include the profileId in the `Amazon-Advertising-API-Scope` header.
    -  Sample request:

    ```
    curl -L 'https://advertising-api.amazon.com/dsp/advertisers/' \
    -H 'Amazon-Advertising-API-Scope: 1111111111111111' \
    -H 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
    -H 'Authorization: Bearer Atza|xxxxxxxxxxxxx'
    ```

4. Create a report using [POST/reporting/reports](offline-report-prod-3p#tag/Asynchronous-Reports/operation/createAsyncReport). Include up to 100 advertiserIds in `configuration.filters.values`.
    -  Example call:

    ```
    curl -L 'https://advertising-api.amazon.com/reporting/reports' \
    -H 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
    -H 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
    -H 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
    -H 'Amazon-Advertising-API-Scope: 1111111111111111' \
    -d '{
        "startDate": "2024-10-01",
        "endDate": "2024-10-15",
        "configuration": {
            "adProduct": "DEMAND_SIDE_PLATFORM",
            "format": "GZIP_JSON",
            "groupBy": [
                "campaign"
            ],
            "columns": [
                "intervalStart",
                "intervalEnd",
                "totalCost"
            ],
            "reportTypeId": "dspCampaign",
            "timeUnit": "SUMMARY",
            "filters": [
                {
                    "field": "advertiserId",
                    "values": [
                        "12345678912345"
                    ]
                }
            ]
        }
    }'
    ```




## ADSP Reports 

The account permission is **ADSP Reports** when a user manually links an existing advertiser to a manager account. This process grants read-only reporting permissions at the individual advertiser level. It can be used when you want to give reporting-only access for only a specific advertiser(s) rather than all advertisers. This method can be used to pull reports for Amazon managed service advertisers.

If the advertiser has already been linked, you should see an Account permission of **ADSP Reports** in your manager account Accounts tab:

![ADSP Report Account Permission](/_images/reporting/adsp-report-account-permission.png)

If the advertiser has not yet been linked, follow the process below. This process can only be completed using the Manager Account UI; there is no API-only solution at this time.  

Process to link advertisers to manager account (if this is already done, skip to Report API process below):

1. Access the [Manager Accounts UI](https://advertising.amazon.com/am/managerAccounts/list). 
2. Choose the Manager Account you wish to use, or create a new one.
3. Within the Manager Account, click **Accounts**, then **Add account > Link existing account**. 
4. Click **Request access**.
5. Enter the DSP advertiser ID and marketplace. You can get these from a user who has admin access to the advertisers. If these are Amazon managed service advertisers, you can get this information from your Amazon account representative.
6. Click **Add account**.
7. Copy the approval link associated with the advertisers you just requested, then provide this URL to the user who has admin access to the advertisers.
    -  If Amazon manages your DSP campaigns, provide the URL to your Amazon account manager. 
8. Once approval has been granted, in your Manager Account **Accounts** tab, you should see an Account permission of **ADSP Reports** for this advertiser.


Report API process:

1. To use the API, a user with access to this manager account needs to [authorize your Login with Amazon application](getting-started/create-authorization-grant). 
2. Retrieve the advertiser IDs that your manager account has access to using the [GET /managerAccounts endpoint](reference/1-0/managerAccount#/Manager%20Accounts/getManagerAccountsForUser).

Example response:

```
{
  "managerAccounts": [
    {
      "linkedAccounts": [
        {
          "accountId": "ENTITY3G3R4D4E0FEIV",
          "accountName": "ADVERTISER_NAME_456",
          "accountType": "DSP_ADVERTISING_ACCOUNT",
          "dspAdvertiserId": "1234567890",
          "marketplaceId": "ATVPDKIKX0DER",
          "profileId": ""
        }
      ],
      "managerAccountId": "amzn1.ads1.ma1.7xxxxxxxxxxx",
      "managerAccountName": "Test Agency"
    }
  ]
}
```

3. When creating a report using [POST/reporting/reports](offline-report-prod-3p#tag/Asynchronous-Reports/operation/createAsyncReport), use the `dspAdvertiserId` from the above response in the `Amazon-Ads-AccountId` header AND in the `configuration.filters.values` array. Note that you can only create a report for one advertiser per report using this method.


Example call:

(For an advertiser ID of 1234567890)

```
curl -L 'https://advertising-api.amazon.com/reporting/reports' \
-H 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
-H 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
-H 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
-H 'Amazon-Ads-AccountId: 1234567890' \
-d '{
    "startDate": "2024-10-01",
    "endDate": "2024-10-15",
    "configuration": {
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "format": "GZIP_JSON",
        "groupBy": [
            "campaign"
        ],
        "columns": [
            "intervalStart",
            "intervalEnd",
            "totalCost"
        ],
        "reportTypeId": "dspCampaign",
        "timeUnit": "SUMMARY",
        "filters": [
            {
                "field": "advertiserId",
                "values": [
                    "1234567890"
                ]
            }
        ]
    }
}'
```



## Summary

|Account permission	|Header used in /reporting/reports	|Endpoint to retrieve header value	|Works for Amazon managed service?	|# of advertiserIds per report	|Profile can be retrieved from GET /v2/profiles?	|
|---	|---	|---	|---	|---	|---	|
|Admin	|Amazon-Advertising-API-Scope (value=profileId)	|[GET/v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles)	|No	|Up to 100	|yes	|
|ADSP Reports	|Amazon-Ads-AccountId (value=dspAdvertiserId)	|[GET/managerAccounts](reference/1-0/managerAccount#/Manager%20Accounts/getManagerAccountsForUser)	|Yes	|1	|no	|

## Next steps

For more information on requesting reports, see:

* [Version 3 reporting overview](guides/reporting/v3/overview)
* [Get started with version 3 reports](guides/reporting/v3/get-started)





