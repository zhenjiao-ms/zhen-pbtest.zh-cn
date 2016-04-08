---
title: Set Credentials
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58f6def5-f0dc-40da-bfab-2f4337b2e245
---
# Set Credentials
---
<a name="top"/>
[Request](#request) | [Response](#response)

The **Set Credentials** operation sets the credentials for a datasource.

**Required scope**: Dataset.ReadWrite.All

To set the permissions scope, see [Register a client app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-client-app/) or [Register a web app](https://powerbi.microsoft.com/documentation/powerbi-developer-register-a-web-app/).

<a name="request"/>
## Request

PATCH https://api.powerbi.com/beta/myorg/gateways/{gateway_id}/datasources/{datasource_id}
### Uri parameter

|Name|Description|Data Type|
|-|-|-|
| gateway_id | Unique guid of the **Gateway** to use. |String|
| datasource_id | Unique guid of the**Datasource** to use |String|

### Header
Authorization: Bearer eyJ0eX ... FWSXfwtQ

### Body schema

  {
      "credentialType": "Basic",
      "basicCredentials": {
      "username": "userName",
      "password": "password"
    }
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
