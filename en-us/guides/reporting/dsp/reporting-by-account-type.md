---
title: DSP reporting by account type
description: Understand authorization for self-service vs. managed DSP accounts when requesting DSP reporting using the Amazon Ads API.
type: guide
interface: api
tags:
    - DSP
    - Reporting
---

# DSP reporting by account type

Amazon DSP currently provides support for both managed and self-service accounts. Self-service accounts manage their own campaigns while managed accounts have their campaigns managed by either Amazon or a third-party agency. Both managed and self-service advertisers can use the Amazon Ads API to retrieve reporting data, however the process differs depending on the type of account.

## Self-service accounts 

Self-service DSP entity owners manage campaigns and may have multiple advertisers connected to their DSP entity. Entity owners may manage campaigns for one or more advertisers, or may be a tool provider. 

To call the DSP reporting API:

1. Make sure your Login with Amazon application is [authorized by a DSP account](guides/get-started/create-authorization-grant) that has entity-level access.
2. Retrieve your entity ID using the [GET /v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint.
    
    a. Sample response:

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
    
    b. In the above example, your entity ID is the `accountInfo.id` of `ENTITY2XYZ1234`.  Note that DSP profiles will always be of type `agency`. 

>[NOTE] You can also find your entity ID in the [DSP console](https://advertising.amazon.com/dsp/entities) by looking at the URL. The URL follows the pattern: `https://advertising.amazon.com/dsp/{ENTITY_ID}/advertisers`. 

3. Use your entity ID as the `accountId` in the URI of the `POST /accounts/{accountId}/dsp/reports`. 
    
    a. Example call:

    (For entity ID `ENTITY2XYZ1234`)

    ```
    curl --location --request POST 'https://advertising-api.amazon.com/accounts/ENTITY2XYZ1234/dsp/reports' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
    --header 'Accept: application/vnd.dspcreatereports.v3+json' \
    --data-raw '{
        "startDate": "2022-12-05",
        "endDate": "2022-12-19"
    }'
    ```

    b. If you want reporting data for individual advertisers, use the `advertiserIds` array in the request body to specify the relevant IDs. You can find advertiser IDs associated to an entity in the [DSP console](https://advertising.amazon.com/dsp/entities).

    Example call:

    (For entity ID `ENTITY2XYZ1234` and advertiser ID `123456789`)

    ```
    curl --location --request POST 'https://advertising-api.amazon.com/accounts/ENTITY2XYZ1234/dsp/reports' \
        --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
        --header 'Content-Type: application/json' \
        --header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
        --header 'Accept: application/vnd.dspcreatereports.v3+json' \
        --data-raw '{
          "advertiserIds": [
            "123456789"
          ],
          "startDate": "2022-12-05",
          "endDate": "2022-12-19"
        }'
    ```

    >[NOTE] You can only request up to 100 advertiser IDs at a time. If your entity has more than 100 advertisers, you may need to request multiple reports.

## Managed service accounts

Managed service advertisers need to use a manager account to call the reporting API. This process enables entity administrators to grant read-only permissions to a manager account for one or more advertisers within that entity.  

>[NOTE]Some steps of the following process can only be completed using the manager accounts UI; there is no API-only solution at this time. [Learn more about manager accounts in the API.](guides/account-management/authorization/manager-accounts) 

To call the DSP reporting API:

1. Access the [Manager Accounts UI](https://advertising.amazon.com/am/managerAccounts/list) and click **Create Manager account**. 
2. Enter a **Name** and choose whether this is an **Agency** or **Advertiser** account.
3. From the **Link accounts** section, go to the **Request access** tab.
4. Add the DSP advertiser IDs and marketplaces that you want reporting data for. You’ll need to get the advertiser ID from someone who has access to the DSP entity.
5. When you’re finished adding advertiser IDs, select **Create Manager account**.
6. On the bottom left of the screen, select the gear icon, then **Account access & settings**.
7. Click **Accounts**.
8. Copy the approval link associated with the advertisers you just requested, then provide this URL to an administrator of the DSP entity for approval.

    a. If Amazon manages your DSP campaigns, provide the URL to your Amazon account manager. 
    
9. Once approval has been granted, a user with access to the manager account needs to [authorize your Login with Amazon application](guides/get-started/create-authorization-grant). 
10. Now when calling the API, you can use the DSP advertiser ID (from step 4) as the `accountId` in the URI of `POST /accounts/{accountId}/dsp/reports`. 

Example call:

(For an advertiser ID of `1234567890`)

```
curl --location --request POST 'https://advertising-api.amazon.com/accounts/1234567890/dsp/reports' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.dspcreatereports.v3+json' \
--data-raw '{
    "startDate": "2022-12-05",
    "endDate": "2022-12-19"
}'
```


### Manager account advertiser IDs

You can also get any advertiser IDs that your manager account has access to using the [GET /managerAccounts endpoint](reference/1-0/managerAccount#tag/Manager-Accounts/operation/getManagerAccountsForUser).

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
          "dspAdvertiserId": "12345678",
          "marketplaceId": "ATVPDKIKX0DER",
          "profileId": ""
        },
        {
          "accountId": "ENTITY147ELY1DV9VO0",
          "accountName": "ADVERTISER_NAME_123",
          "accountType": "DSP_ADVERTISING_ACCOUNT",
          "dspAdvertiserId": "98765432",
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

You can use the `dspAdvertiserId` (values `12345678` and  `98765432` from the example above) as the `accountId` in the URI of `POST /accounts/{accountId}/dsp/reports`.

## Next steps

For more information on requesting reports, see:

* [Getting started with DSP reports](guides/reporting/dsp/get-started)
* [DSP report types](guides/reporting/dsp/report-types)