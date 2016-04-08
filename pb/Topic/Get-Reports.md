---
title: Get Reports
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ceaac90-b065-4509-b350-b4f583c48bdf
---
# Get Reports
---
<a name="top"/>
[Request](#request) | [Response](#response)

The **Get Reports** operation returns a JSON list of all **Report** objects which includes an id, name, webUrl, and embedUrl, or a JSON list of reports from a **Group**.

**Required scope**: Report.Read.All

To set the permissions scope, see [Register a client app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app) or [Register a web app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-web-app).
<a name="request"/>
## Request
GET https://api.powerbi.com/beta/myorg/reports

### Groups

Groups are a collection of unified **Azure Active Directory** groups that the user is a member of and is available in the Power BI service. To learn how to create a group, see [Create a group](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.powerbi.com/beta/myorg/groups/{group_id}/reports

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|group_id|Guid of the Group to use. You can get the group id from the [Get Groups](Get-Groups.md) operation.|String|

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
**Reports** have a GUID **id**, **name**, **webUrl**, and **embedUrl**. You use the embedUrl to embed a report in an app. See [Integrate a Power BI Tile or Report](https://powerbi.microsoft.com/documentation/powerbi-developer-integrate-a-power-bi-tile-or-report/).
    
    {
      "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#reports","value":
      [
        {
          "id":"{report_guid}",
          "name":"{report_name}",
          "webUrl": "https://app.powerbi.com/reports/{report_id}",
          "embedUrl":"https://app.powerbi.com/embed?&reportId={report_id}"
        }
      ]
    }

### Body Example
The following is an example of a JSON response with one report.

    {
      "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#reports","value":[
      {
          "id":"84dbd390--5eb9fe91cdba",
          "name":"AdventureWorks",
          "webUrl":"https:// app.powerbi.com/reports/84dbd390---5eb9fe91cdba",
          "embedUrl":"https://app.powerbi.com/embedReport?reportId=84dbd390--5eb9fe91cdba"
        }
      ]
    }

<a name="example"/>
