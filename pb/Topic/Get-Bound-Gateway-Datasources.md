---
title: Get Bound Gateway Datasources
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a79175bc-fa84-4b90-8dc2-aea42e92d9ad
---
# Get Bound Gateway Datasources
---
<a name="top"/>
[Request](#request) | [Response](#response)

The **Get Bound Gateway Datasources** operation returns a JSON list of all gateway data sources for a dataset.

**Required scope**: Dataset.ReadWrite.All or Dataset.Read.All

To set the permissions scope, see [Register a client app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-client-app/) or [Register a web app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-web-app/).

<a name="request"/>
## Request
GET https://api.PowerBI.com/beta/myorg/datasets/{dataset_id}/Default.GetBoundGatewayDataSources

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
  "@odata.context": "http://wabi-staging-us-east-redirect.analysis.windows.net/beta/myorg/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "849",
      "gatewayId": "17",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"MyAzureDB.database.windows.net\",\"database\":\"sample2\"}"
    }
  ]
  }

<a name="example"/>
