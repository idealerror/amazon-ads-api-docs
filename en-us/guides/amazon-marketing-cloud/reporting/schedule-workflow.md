---
title: Workflow management service
description: How to create and manage workflows in AMC
type: guide
interface: api
tags:
    - AMC Scheduling
    - Schedule management
    - Scheduled reports
    - Scheduled workflows
    - Recurring workflows
    - Automated workflows
    - Workflow scheduling
    - Time-based scheduling
    - create schedule
---


# Manage a workflow schedule

The [schedules](amc-reporting#tag/Schedules) resource allows you to define the cadence at which a saved workflow will run.  Creating a schedule for a workflow helps define the specificity of the run by allowing you to set the specific time interval, day of the week, and time of day to execute the workflow.

When the schedule is triggered, it will run the workflow and place the results in the instance's associated S3 bucket for the given workflow.

## Create a schedule

To create a schedule for a workflow you defined, use the POST operation of the schedules resource with the instance identifier as the path parameter.

```
POST /amc/reporting/{instanceId}/schedules
```

Here is an example of a schedule object in the JSON format.

```
curl \--location
curl --location 'https://advertising-api.amazon.com/amc/reporting/{instanceId}/schedules' \
--header 'Authorization: Bearer XXXXX' \
--header 'Amazon-Advertising-API-ClientID: amzn1.application-oa2-client.XXXXX' \
--header 'Amazon-Advertising-API-AdvertiserId: XXXXX' \
--header 'Amazon-Advertising-API-MarketplaceId: ATVPDKIKX0DER' \
--data '{
    "aggregationHourUtc": 16,
    "aggregationPeriod": "Weekly",
    "aggregationStartDay": "Tuesday",
    "scheduleEnabled": true,
    "scheduleId": "my-first-schedule",
    "workflowId": "my-first-workflow"
}'
```

In the above example, we created a schedule `my-first-schedule` for the workflow, `my-first-workflow`, we created earlier. The defined schedule will trigger a run of the `my-first-workflow` workflow at a five-hour offset from midnight in UTC every week on Tuesday. The interval and amount of data contained within the report are the same, so this would generate a report for the data from the previous Tuesday at the specified offset from UTC until the current Tuesday at the same hour offset.

In order to determine the correct value of the hour offset, you use the `aggregationHourUtc` parameter. Convert your desired start time to Coordinated Universal Time (UTC) and calculate the offset in hours from 12 a.m. (midnight) UTC. For example, 10 a.m. MST is 4 p.m. UTC, which is 16 hours from midnight UTC. So, to run the SQL query in the `my-first-workflow` every Tuesday at 10 am, the value for the `aggregationHourUtc` parameter will be 16.

To enable a schedule, set the `ScheduleEnabled` parameter to `true`. When this value is set to `false` the workflow will not be queued for execution even with an associated schedule.

> [NOTE] If not specified, `aggregationStartDay` will default to the day that the schedule is created in the case of 'Weekly' aggregation time frame. If the `aggregationPeriod` is "Daily" then `aggregationStartDay` is not used.

A successful call returns an HTTP status code 200 OK and queues the workflow for execution as per the defined schedule.

After execution, the report is generated to output a file suffixed with `-ver1.csv` and sent to your S3 bucket approximately 6–7 hours after the scheduled run time. The report generated omits invalid traffic.

> [NOTE] The times indicated for when reports are generated are estimates, and can be affected by upstream data delays or by the compute time needed to generate particularly complex reports.

## View all schedules within an instance

To list all schedules defined within your instance, use the GET operation of the `schedules` resource with the instance identifier as the path parameter.

```
GET /amc/reporting/{instanceId}/schedules
```

## View a schedule

To view a specific schedule within an instance, use the GET operation of the `schedules` resource with the instance identifier and schedule identifier of the requested schedule as path parameters. For example, to view the details of `my-first-schedule`, the schedule we created earlier, use the following call:

```
GET /amc/reporting/{instanceId}/schedules/my-first-schedule
```

## Update a schedule

The PUT operation of the `schedules` resource updates the values in a specific schedule. It uses the same structure as the create schedule request.

To update a schedule:

- First, retrieve the specific schedule. 
- Next, provide the updated values for the individual parameters you want to modify as part of the body of the request. 
- Then, use the PUT operation of the schedules resource to save the changes.

```
PUT /amc/reporting/{instanceId}/schedules/{scheduleId}
```

## Delete a schedule

To delete a schedule within an instance, use the DELETE operation of the `schedules` resources. Pass the instance identifier and schedule identifier as path parameters.

```
DELETE /amc/reporting/{instanceId}/schedules/{scheduleId}
```

Once deleted, the workflow associated with the schedule will not execute at the cadence that was defined in the schedule. To execute the workflow, you will need to either create another schedule or execute the workflow ad hoc, using the workflowExecutions resource.

## Next steps

- [Manage workflow executions](guides/amazon-marketing-cloud/reporting/execute-workflow)
- [Get your workflow results](guides/amazon-marketing-cloud/reporting/get-your-results)