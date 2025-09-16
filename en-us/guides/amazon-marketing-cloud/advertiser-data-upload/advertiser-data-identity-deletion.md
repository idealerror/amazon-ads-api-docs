---
title: Data deletion
description: How to use data deletion in AMC
type: guide
interface: api
---

# Advertiser data identity deletion

This set of resources allows you to a request for user deletion of a set of hashed target identity fields and values in all datasets. You can also retrieve the deletion requests and get the status of your request. 

### Supported Methods


| Methods    |   Endpoint                                                                    | Description   |
| ----------- |  ---------------------------------------------------------------------- | ---------------|
| [POST](amc-advertiser-data-upload#tag/User-Deletion-Request/operation/CreateUserDeletionRequest) | [/amc/advertiserData/{instanceId}/userDeletionRequest](amc-advertiser-data-upload#tag/User-Deletion-Request/operation/CreateUserDeletionRequest)                            | Creates a request for user deletion of a set of hashed target identity fields and values in all data sets.|
| [POST](amc-advertiser-data-upload#tag/User-Deletion-Request/operation/ListUserDeletionRequests) | [/amc/advertiserData/{instanceId}/userDeletionRequest/list](amc-advertiser-data-upload#tag/User-Deletion-Request/operation/ListUserDeletionRequests)                        | Gets a paginated list of all user deletion requests.|
| [GET](amc-advertiser-data-upload#tag/User-Deletion-Request/operation/GetUserDeletionRequest) | [/amc/advertiserData/{instanceId}/userDeletionRequest/{userDeletionRequestId}](amc-advertiser-data-upload#tag/User-Deletion-Request/operation/GetUserDeletionRequest)     | Gets status and metadata for a previously initiated identity deletion.|
