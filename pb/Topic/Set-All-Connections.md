---
title: Set All Connections
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3fb4e899-8e4a-4e76-8b3b-d112a4f494dd
---
# Set All Connections
---
<a name="top"/>
[Request](#request) | [Response](#response)

The **Set All Connections** operation updates all connection strings for each data source in a dataset. The connections strings are updated with the body of the POST call.

**Required scope**: Dataset.ReadWrite.All

To set the permissions scope, see [Register a client app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-client-app/) or [Register a web app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-web-app/).

<a name="request"/>
## Request
POST https://api.powerbi.com/beta/myorg/myorg/datasets/{dataset_id}/Default.SetAllConnections

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|dataset_id|Guid of the **Dataset** to use. You can get the dataset id from the [Get Datasets](Get-Datasets.md) operation.|String|

### Header
Authorization: Bearer eyJ0eX ... FWSXfwtQ

### Body schema

  {
    "connectionString": "data source=MyAzureDB.database.windows.net;initial catalog=Sample2;persist security info=True;encrypt=True;trustservercertificate=False"
  }

<a name="response"/>
## Response

### Status code

|**Code**|**Description**
|---|---
|200|OK. Indicates success. The dataset is returned in the response body.

### Content-Type
application/json

<a name="example"/>
