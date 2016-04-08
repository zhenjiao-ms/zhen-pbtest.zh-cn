---
title: Get datasources
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1816cb5d-3e15-4f73-b6b0-85210f5c71e6
---
# Get datasources
---
<a name="top"/>
[Request](#request) | [Response](#response)

The **Get Datasources** operation returns a JSON object which includes name and connectionString.

**Required scope**: Dataset.ReadWrite.All or Dataset.Read.All

To set the permissions scope, see [Register a client app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-client-app/) or [Register a web app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-web-app/).

<a name="request"/>
## Request
GET https://api.powerbi.com/beta/myorg/datasets/{datasetID}/dataSources

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|dataset_id|Guid of the **Dataset** to use. You can get the dataset id from the [Get Datasets](Get-Datasets.md) operation.|String|

### Header
Authorization: Bearer eyJ0eX ... FWSXfwtQ
<a name="response"/>
## Response

### Status code

|**Code**|**Description**
|---|---
|200|OK. Indicates success. The dataset is returned in the response body.

### Content-Type
application/json

### Body schema
    {
    "@odata.context": "http://api.powerbi.com/beta/myorg/$metadata#datasources",
    "value": [
      {
          "name": "DataSource1",
          "connectionString": "data source=MyAzureDB.database.windows.net;initial catalog=Sample2;persist security info=True;encrypt=True;trustservercertificate=False"
      }
      ]
    }

<a name="example"/>
