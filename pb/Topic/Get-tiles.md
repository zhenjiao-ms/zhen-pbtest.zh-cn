---
title: Get Tiles
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59df1d7d-dd08-49aa-bf02-8c6938f70f3a
---
# Get Tiles
---
<a name="top"/>
[Request](#request) | [Response](#response)

The **Get Tiles** operation returns a JSON list of all **Tile** objects which includes an id, title, and embedUrl, or a JSON list from a **Group**.

**Required scope**: Tile.Read.All

To set the permissions scope, see [Register a client app](https://msdn.microsoft.com/library/dn877542.aspx) or [Register a web app](https://msdn.microsoft.com/library/dn985955.aspx).
<a name="request"/>
## Request
GET https://api.powerbi.com/beta/myorg/dashboards/{dashboard_id}/tiles

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|dashboard_id|Guid of the Dashboard to use. You can get the **Dashboard** id from the [Get Dashboards](Get-Dashboards.md) operation.|String|


### Groups

Groups are a collection of unified **Azure Active Directory** groups that the user is a member of and is available in the Power BI service. To learn how to create a group, see [Create a group](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.powerbi.com/beta/myorg/groups/{group_id}/dashboards

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|group_id|Guid of the Group to use. You can get the group id from the [Get Groups](Get-Groups.md) operation.|String|

### Header
Authorization: Bearer eyJ0eX ... FWSXfwtQ	
<a name="response"/>
## Response

### Status code

<table><tr><td>Code</td><td>Description</td></tr><tr><td>200</td><td>OK. Indicates success. The dataset is returned in the response body.</td></tr></table>

### Content-Type
application/json

### Body Schema
**Tiles** have a GUID **id**, **title**, and **embedUrl**. You use the embedUrl to embed a tile in an app. See [Integrate a Power BI Tile](https://powerbi.microsoft.com/documentation/powerbi-developer-integrate-a-power-bi-tile-or-report/).
    
    {
      "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#tiles","value":
      [
        {
          "id":"{tile_guid}",
          "title":"tile_title",
          "embedUrl":"https://api.powerbi.com/embed?dashboardId={dashboard_id}&tileId={title_id}"
        }
      ]
    }

### Body Example
The following is an example of a JSON response with two tiles.

	{
    "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#tiles", "value":
        [
        {
           "id":"f16dc78a--afe1098a100d",
           "title":"ProductID",
           "embedUrl":"https://dxt.powerbi.com/embed?dashboardId=43127a01--e971d4cdc2fb&tileId=f16dc78a-6897-afe1098a100d"
        },
        {
      	 "id":"9ab3925d--4d6c896df0c9",
           "title":"Count of CountryRegionCode",
           "embedUrl":"https://dxt.powerbi.com/embed?dashboardId=43127a01--e971d4cdc2fb&tileId=9ab3925d--4d6c896df0c9"
    		}
  		]
    }

<a name="example"/>
