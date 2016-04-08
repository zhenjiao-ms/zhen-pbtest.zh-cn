---
title: Get Import by File Path
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e31b2a38-9653-4a9c-9920-37ce2b116032
---
# Get Import by File Path
---
[Request](#request) | [Response](#response) | **[Preview operation]**
<a name="top"/>

The **Get Import by File Path** operation returns a JSON body for an **Import** object with its properties including id, import source **Reports**, and **Datasets**. Include the **Import file path** in the filePath query string parameter in the GET uri to return an **Import** object.

**Required scope**: Metadata.View_Any
<a name="request"/>
## Request
GET https://api.powerbi.com/beta/myorg/imports?filePath={filePath}

### Query string parameter
|Name|Description
|---|---
|filePath|The path to the import file.


### Request examples    
    GET http://api.powerBI.com/beta/myorg/imports?filePath=http://microsoft-my.sharepoint.com/personal/{user name}/shared%20with%20everyone/mydoc.xlsx

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
              "name":"{import_name}"
          }  
      ]
    }
 
### Body Example

    {
      "id" : "405BBEEB--30264301ED95",
      "createdDateTime" : "2015-03-13 15:51:30.937",
      "updatedDateTime" : "2015-03-13 15:51:30.937",
      "importState" : "Published", 
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
              "id":"A68F51BD--5EFF229013C7",
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
