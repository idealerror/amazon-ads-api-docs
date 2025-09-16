---
title: Amazon your garage
description: Amazon your garage
type: guide
interface: api
---
# Amazon Your Garage tables

**Analytics table:**

- `amazon_your_garage`

**Audiences table:**

- `amazon_your_garage_for_audiences`

Amazon Your Garage signals  are available within an Amazon Marketing Cloud (AMC) account belonging to the North America marketplaces as an AMC Paid Feature enrollment. The Amazon Your Garage dataset includes signals across United States, Mexico, and Canada Amazon marketplaces. This dataset is a DIMENSION dataset type and not FACT dataset type. Based on this implementation detail, there is no associated lookback for the dataset.  The Amazon Your Garage dataset provides user-to-vehicle association details that describe vehicle type, the make and model of the vehicle, as well as detailed vehicle configuration attributes expressed in the context of an AMC `user_id`.

> [NOTE] The data "snapshot" will be the most-recent available user-to-vehicle association, refreshed and published into AMC daily.

With Amazon Your Garage signals, advertisers can use the feature alongside DSP traffic events, sponsored ads event-level signals, Paid feature subscriptions, and advertiser uploaded signals in order to conduct saved vehicle segment analysis for planning, measurement, and activation purposes.

Table names:



| Field category     | Name                 | Data type | Metric / dimension | Description                                                                             | Aggregation threshold |
| ------------------ | -------------------- | --------- | ------------------ | --------------------------------------------------------------------------------------- | --------------------- |
| Amazon Your Garage | marketplace\_name    | STRING    | Dimension          | The marketplace associated with the Amazon Garage record.                               | LOW                   |
| Amazon Your Garage | user\_id             | STRING    | Dimension          | The User ID of the customer owning the vehicle.                                         | VERY_HIGH             |
| Amazon Your Garage | user\_id\_type       | STRING    | Dimension          | The `user_id` type.                                                                   | LOW                   |
| Amazon Your Garage | creation\_date       | DATE      | Dimension          | Creation date (in UTC) of the record.                                                            | LOW                   |
| Amazon Your Garage | last\_accessed\_date | DATE      | Dimension          | The last accessed date (in UTC) for a customer invoked interaction with an Amazon Garage record. | LOW                   |
| Amazon Your Garage | update\_date         | DATE      | Dimension          | The date (in UTC) for the most recent garage record edit.                                        | LOW                   |
| Amazon Your Garage | garage\_year         | STRING    | Dimension          | Vehicle year attribute.                                                                 | LOW                   |
| Amazon Your Garage | vehicle\_type        | STRING    | Dimension          | Vehicle type attribute.                                                                 | LOW                   |
| Amazon Your Garage | garage\_make         | STRING    | Dimension          | Vehicle make attribute.                                                                 | LOW                   |
| Amazon Your Garage | garage\_model        | STRING    | Dimension          | Vehicle model attribute.                                                                | LOW                   |
| Amazon Your Garage | garage\_submodel     | STRING    | Dimension          | Vehicle sub-model (trim) attribute.                                                     | LOW                   |
| Amazon Your Garage | garage\_bodystyle    | STRING    | Dimension          | Vehicle body style attribute.                                                           | LOW                   |
| Amazon Your Garage | garage\_engine       | STRING    | Dimension          | Vehicle engine attribute.                                                               | LOW                   |
| Amazon Your Garage | garage\_transmission | STRING    | Dimension          | Vehicle transmission attribute.                                                         | LOW                   |
| Amazon Your Garage | garage\_drivetype    | STRING    | Dimension          | Vehicle drive type attribute.                                                           | LOW                   |
| Amazon Your Garage | garage\_brakes       | STRING    | Dimension          | Vehicle brakes attribute.                                                               | LOW                   |
| Amazon Your Garage | no\_3p\_trackers     | BOOLEAN   | Dimension          | Is this item not allowed to use 3P tracking?                                            | NONE                  |
