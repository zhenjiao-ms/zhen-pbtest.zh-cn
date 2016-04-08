---
title: Get Import by GUID
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d71ff2b0-1b9b-4f57-b39f-52da11dba554
---
# Get Import by GUID
---
[Request](#request) | [Response](#response) | **[Preview operation]**
<a name="top"/>

The **Get Import by GUID** operation returns a JSON body for an **Import** object with its properties including id, import source, **Reports**, and **Datasets**. Include the **Import guid** in the GET uri to return an **Import** object.

**Required scope**: Metadata.View_Any
<a name="request"/>
## Request
GET https://api.powerbi.com/beta/myorg/imports/{import_guid}

### Header
Content-Type: application/json
Authorization: Bearer eyJ0eX ... FWSXfwtQ	
<a name="response"/> 
## Response

### Status code

|Code|Description
|---|---
|200|OK. Indicates success. The response body contains a dataset if the file is imported, and a report if the file is connected.


### Content-Type
application/json

### Body schema 

**Imports** have the following properties:

- Id
- Name
- createdDateTime
- Import source (either file or URL path)
- Array of **Reports** 
  - A **Report** has the same properties as returned by [GET Reports](Get-Reports.md).
- Array of **Datasets**
  - A **Dataset** has the same properties as returned by [GET Datasets](Get-Datasets.md).

 
    {
      "id" : "{import_guid}",
      "createdDateTime" : "{DateTime}",
      "updatedDateTime" : "{DateTime}",
      "importState" : "{string}",
      "error":
        {
            "code": "{string}",
            "message": "{message}",
            "target": "{target}",
            "details": [
                {
                "code": "{code}",
                "target": "{target}",
                "message":  "{message}",
                }
            ]
        },
        "reports" : [
        {
          "id":"{guid}",
          "name":"{string}"
        }  
        ],        
        "datasets" : [
          {
              "id":"{guid}",
              "name":"{string}"
          }  
        ]
      }

### Body Example

    {
      "id" : "405BBEEB--30264301ED95",
      "createdDateTime" : "2015-03-13 15:51:30.937",
      "updatedDateTime" : "2015-03-13 15:51:30.937",
      "importState" : "Published", -Need to get Enum values
      "error":
        {
            "code": "NotImplemented",
            "message": "Requested feature is not implemented",
            "target": "query",
            "details": [
                {
                "code": "QueryParmNotSupported",
                "target": "$search",
                "message":  "Requested query option not supported",
                }
            ]
        },
        "reports" : [
        {
          "id":"1662698F-045D-4F2D-AFD1-556BEC8F4413",
          "name":"forecast.xlsx"
        }  
        ],        
      "datasets" : [
          {
              "id":"A68F51BD--5EFF229013C7",
              "name":"forecast.xlsx"
          }  
      ]
    }
