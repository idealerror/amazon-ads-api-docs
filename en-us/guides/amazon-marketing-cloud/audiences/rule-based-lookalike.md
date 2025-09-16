---
title: Rule-based lookalike audiences
description: Rule-based lookalike audiences
type: guide
interface: api
tags:
    - lookalike
    - AMC lookalike audience
keywords:
    - lookalike audience
    - seed audience
    - targeting
    - expected reach
---
# Rule-based lookalike audiences

The AMC rule-based audiences lookalike API you can build lookalike audiences to expand your reach and enhance the effectiveness of your advertising efforts, while ensuring the security and privacy of customer and business data.  The AMC rule-based audience lookalike API allows you to create an audience similar to an AMC rule-based audience seed that can be used in Amazon Demand Side Platform (Amazon DSP), Sponsored Brands, Sponsored Products, and Sponsored Display campaigns, to reach millions of shoppers, thereby increasing your targeting capabilities.
The rule-based audience lookalike API is an extension of the current AMC rule-based audiences API. Using the same query language and datasets as rule-based audiences, you can define the seed audience from which AMC will model the lookalike audience.

> [NOTE] When you use the rule-based audience lookalike API, AMC will create only the lookalike audience in Amazon DSP, Sponsored Brands, Sponsored Products, or Sponsored Display. It will not create a duplicate rule-based audience .

The API uses [rule-based audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences) as the seed to build an audience with similar characteristics (that is, an audience that "looks like" the seed audience). Thus, allowing you to leverage your effective rule-based audiences to create lookalike audiences and further expand your reach.

> [WARNING] It is not possible to use an existing rule-based audience as the seed audience. If you have an existing audience you want to use as the seed audience, copy the SQL query and add to the Create Lookalike Audience POST request.

### Create a lookalike audience

The rule-based audiences lookalike API provides the following API endpoint to manage rule-based audiences lookalike audiences:

```
POST /amc/audiences/lookalike

```

Creates a rule-based audience lookalike execution metadata. This operation starts the execution workflow to create lookalike audience and activate it in the target product.

> [WARNING] Seed audiences must contain a minimum of 500 user\_ids and a maximum of 500,000 user\_ids. If the creation of the seed audience yields a population of less than 500 or more than 500,000 user\_ids, the lookalike audience creation will fail. You can determine the approximate size of the seed audience before submitting the request to create the lookalike audience. Refer to [Determine audience size](guides/amazon-marketing-cloud/audiences/rule-based-audiences#sql-queries-and-tables-required-to-create-rule-based-audience).
> You can also use the AMC Query editor in the AMC UI to determine the estimated size of the audience before you attempt to create a lookalike audience.

When creating the rule-based audience lookalike execution metadata, make note of the following parameters to help you tailor your output:

- `lookalikeAudienceExpectedReach`: The `lookalikeAudienceExpectedReach`, a required field for creating lookalike audiences, sets the target size of the lookalike audience. Here you specify if you want to create audiences based on the similarity of the characteristics of the seed audience or if you intend to create a broader set of audiences (and not prioritize the audience creation based on its similarity with the seed audience).

  The following table lists the options for `lookalikeAudienceExpectedReach`, as well as the ranges of the resulting audience size. Note that the expected range of audience size will vary depending on your region.

  | lookalikeAudienceExpectedReach | Europe and the Americas | Asia          |
  | ------------------------------ | ----------------------- | ------------- |
  | MOST_SIMILAR                   | 500K to 1M              | 200K to 300K  |
  | SIMILAR                        | 1.5M to 3M              | 450K to 900K  |
  | BALANCED                       | 2M to 5.5M              | 600K to 1.65M |
  | BROAD                          | 2.5M to 7.5M            | 750K to 2.25M |
  | MOST_BROAD                     | 3M to 10M               | 900K to 3M    |

- `refreshRateDays`: The value of `refreshRateDays` determines how frequently the SQL query is re-run and the existing seed audience is completely overwritten with new data. The `refreshRateDays `parameter allows you to 
    * keep the seed audience active, 
    * re-run the SQL query at a specified interval, and 
    * update the lookalike audience based on the updated seed. For example, if set to 7, the SQL query will be re-run every 7 days, the seed audience will be updated with the new data.

  If the `refreshRateDays` is set to 0, the audience will be created in Amazon DSP, but deactivated after 30 days. Deactivated audiences have no members.

> [NOTE] Only values from 7 to 21 are valid.

If the response returns the status of SUCCEEDED, you can view the audience in the advertiser account it was created for.


> [WARNING] The audience will take between 2 - 24 hours to synchronize with the advertiser account in the target application. This includes the time to create the seed rule-based audience and use the seed set to create the lookalike audience.

If the response returns a status of FAILED, you can view the statusReason to determine the cause of the failure.

For example, the following statusReason is returned when you do not use audience-specific tables in the query:
"statusReason": "Invalid tables in the SQL query: Use audience-specific tables in the query."

Some of the reasons for a lookalike audience creation to fail are:

- The seed audience submitted does not reach 500 user_ids, which is the minimum number required for modeling a lookalike audience.
- The SQL query referenced a table that was not supported for Audiences or there was an error in the SQL code.
- Required parameters such as Advertiser ID, SQL query, Instance ID, Audience name, and so on could be missing.
