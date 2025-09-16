---
title: Ads data manager -consent management
description: Consent management in ADM
type: guide
interface: api
---
# Consent management 

Advertisers that transmit their United Kingdom (UK) and European Economic Area (EEA) customers' personal data to Amazon Ads must use one of the following formats to communicate their users' privacy choices and consent:

 - Transparency & Consent Framework (TCF)
 - Global Privacy Platform (GPP)
 - Amazon Consent Signal (ACS)

## Amazon Consent Signal

Amazon Consent Signal (ACS) is an easy-to-use consent sharing format developed by Amazon for advertisers or 3Ps to share consent choices when uploading or sharing their customer data with Amazon Ads. ACS encompasses two distinct signals:

- `amzn_user_data`: This parameter sets consent to use personal data for advertising purposes. It accepts values to indicate consent for Amazon to use the customer’s personal data for advertising. The permissible values are "GRANTED" and "DENIED".

- `amzn_ad_storage`: This is an optional signal to set consent for cookies. It accepts values that signify the consent collected by the advertiser (or a third-party) for Amazon to read and/or write Amazon cookies within the customer's browser. The acceptable values are "GRANTED" and "DENIED". If the field is NULL, Amazon will assume that the data collection did not include the usage of cookies and will consider the record as consented.

### Providing consent signals 

When populating an audience dataset by calling `POST/adm/audiences/{{dataSetId}}/members` to include records, for each of the `members` of the audience you are uploading, provide the following details in the request body:


- **externalUserId**: For each record, this is the user identifier as defined by the 3P provider.

- **userConsent**: For each record, the `userConsent` is collected as two components: **geo** and **consent**.

  - **`geo`** is composed of the following parameters: 

    - `countryCode`: The country code is 2-character string in the ISO 3166 format that indicates from which country the data was collected (for example, US or GB).  Note, the `countryCode` supplied for consent management is not the same as the `consentCode` used for hashed PII identity matching. For details on the resolution priority for country codes, see  [Country code resolution priority.](#country-code-resolution-priority)
    - `ipAddresss`: you can optionally include an `ipAddress` to determine the country for members in this audience.

  - **`consent`**: For personal data uploaded for customers from UK and European Economic Area EEA,  use one of the following formats:

    - TCF
    - GPP 
    - ACS

  In case of ACS, provide values for `amzn_user_data` and/or `amzn_ad_storage`. 

#### Country code resolution priority

Values for a `countryCode` can be supplied at the record-level or at the `dataSet` level. Ads data manager determines the `countryCode` to use through a priority sequence. At the highest priority is the record-level `countryCode`. If this is provided, it will be used regardless of other sources. In the absence of a record-level `countryCode`, Ads data manager will attempt to resolve the country from the provided `ipAddress`. If the `ipAddress` is unavailable, then the `countryCode` specified at the `dataSet` level is considered. If none of these sources yield a valid `countryCode`,  the system defaults to "**UNKNOWN**," which may impact match rates or or complete omission of the records as it may fail consent requirements. 

