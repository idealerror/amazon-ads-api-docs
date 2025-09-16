---
title: Dataset columns
description: How to use dataset columns in AMC
type: guide
interface: api
---
# Manage dataset columns

 The Data set columns API allows you to add, modify, or delete columns from datasets.

### Supported Methods


| Methods       |   Endpoint                                              | Description   |
| ------------- |  ------------------------------------------------------ | ---------------|
| [POST](amc-advertiser-data-upload#tag/Data-Set-Column/operation/AddColumnToDataSet) | [/amc/advertiserData/{instanceId}/dataSets/{dataSetId}/columns](amc-advertiser-data-upload#tag/Data-Set-Column/operation/AddColumnToDataSet)              | Add a new column to a dataset      |
| [PUT](amc-advertiser-data-upload#tag/Data-Set-Column/operation/UpdateColumnInDataSet) | [/amc/advertiserData/{instanceId}/dataSets/{dataSetId}/columns/{columnName}](amc-advertiser-data-upload#tag/Data-Set-Column/operation/UpdateColumnInDataSet) | Update the column name or description |
| [DELETE](amc-advertiser-data-upload#tag/Data-Set-Column/operation/DeleteColumnFromDataSet) | [/amc/advertiserData/{instanceId}/dataSets/{dataSetId}/columns/{columnName}](amc-advertiser-data-upload#tag/Data-Set-Column/operation/DeleteColumnFromDataSet) | Delete a column in a dataset          |

### Adding a new column to a dataset

To define a new column and add it to an existing dataset, use the POST operation of the `/amc/advertiserData/{instanceId}/dataSets/{dataSetId}/columns` resource. Pass the `instanceId` and `dataSetId` as path parameters.

> [NOTE] You cannot add a `mainUserId` or `mainUserIdType` column. If you want to add a `mainUserId` or `mainUserIdType` column, you will need to re-create the dataset.


In the example below, we create a column titled `firstcolumn`.

**Sample request body**

```
{
  "column": {
    "columnType": "DIMENSION",
    "nullable": true,
    "dataType": "STRING",
    "name": "firstcolumn",
    "description": "My first column"
  }
}
```


### Update a column name or description in a dataset

To update a column name and/or description in an existing dataset, use the `PUT /amc/advertiserData/{instanceId}/dataSets/{dataSetId}/columns/{columnName}` API call. Pass the `instanceId`, `dataSetId`, and the `{columnName}` to be updated as path parameters.

In the example below, we create a column titled `firstcolumn`.

**Sample request body**

```
{
  "updatedColumnName": "newcolumnname",
  "updateDescription": "My updated column description"
}
```

### Delete a column from a dataset

To update a column name and/or description in an existing dataset, use the `DELETE /amc/advertiserData/{instanceId}/dataSets/{dataSetId}/columns/{columnName}` API call. Pass the `instanceId`, `dataSetId`, and the `{columnName}` to be deleted as path parameters.

>[WARNING] You can not delete columns if either the `mainEventTime` or `mainUserId` parameters are marked as true.

## Next steps

For examples and answers to frequently asked questions, refer to [Appendix](guides/amazon-marketing-cloud/advertiser-data-upload/adu-examples).