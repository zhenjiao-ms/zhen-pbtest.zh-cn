---
title: Get Imports
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee917f6-37a0-4061-8a82-4fd90ac85bb4
---
# Get Imports
---
[Request](#request) | [Response](#response)
<a name="top"/> | **[Preview operation]**

The **Get Imports** operation returns a JSON list of all **Import** objects with there properties including id, import source, **Reports**, and **Datasets**. 

**Required scope**: Metadata.View_Any
<a name="request"/>
## Request
GET https://api.powerbi.com/beta/myorg/imports

### Header
Content-Type: application/json
Authorization: Bearer eyJ0eX ... FWSXfwtQ	
<a name="response"/> 
## Response

### Status code

|Code|Description
|---|---
|200|OK. Indicates success. The response body contains datasets for imported files, and reports for connected files.

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
    "value":[
        {
          "id" : "{guid}",
          "createdDateTime" : "{DateTime}",
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
      ]    
    }  

### Body Example

    {
    "value":[
        {
          "id" : "405BBEEB--30264301ED95",
          "createdDateTime" : "2015-03-13 15:51:30.937",
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
      ]    
    }  
