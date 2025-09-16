---
title: AMC consent management
description: consent management 
type: guide
interface: api
tags:
    - tcf
    - gpp
    - acs
    - privacy choices
    - consent
    - personal information  
    - amazon consent signal requirements
---

# Consent management in AMC

Advertisers that transmit their United Kingdom (UK) and European Economic Area (EEA) customers' personal data to Amazon Ads must use one of the following formats to communicate their users' privacy choices and consent:

* Transparency  & Consent Framework (TCF)
* Global Privacy Platform (GPP)
* Amazon  Consent Signal (ACS)

## Amazon Consent Signal

Amazon Consent Signal (ACS) is an easy-to-use consent sharing format developed by Amazon for advertisers or 3Ps to share consent choices when uploading or sharing their customer data with Amazon Ads. ACS encompasses two distinct signals:

- `amzn_user_data`:  This parameter sets consent to use personal data for advertising purposes. It accepts values to indicate consent for Amazon to use the customer’s personal data for advertising. The permissible values are "GRANTED" and "DENIED".
- `amzn_ad_storage`: This is an optional signal to set consent for cookies. It accepts values that signify the consent collected by the advertiser (or a third-party) for Amazon to read and/or write Amazon cookies within the customer's browser. The acceptable values are "GRANTED" and "DENIED". If the field is NULL, Amazon will assume that the data collection did not include the usage of cookies and will consider the record as consented.

> [WARNING] Starting November 6, 2024, AMC will accept TCF, GPP, and ACS consent signals. These consent signals will be optional in UK/EEA personal data uploads until they become mandatory. AMC will respect provided consent, rejecting records if denied. During the transitional period starting November 6, 2024, there will be no impact to data uploaded without consent signals. 
Advertisers and third-party providers are advised to update their integrations to include country codes and consent information during this transitional period to ensure uninterrupted services after the mandatory implementation.

Please refer to [Sending Personal Information to Amazon Ads & Amazon Consent Signal requirements](https://advertising.amazon.com/resources/ad-policy/consent-signal-requirements?ref_=a20m_us_spcs_conre) to know more about the policy.

The above-mentioned data decoration standards for country codes and consent signals apply to all advertisers and 3Ps using AMC.

- Provide consent for uploads using [Advertiser data upload APIs](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets)
- Provide consent for data in [AMC - AWS Clean Rooms collaboration instances](guides/amazon-marketing-cloud/acr/5_data_prep)
