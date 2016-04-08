---
title: 获取磁贴
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59df1d7d-dd08-49aa-bf02-8c6938f70f3a
---
# 获取磁贴
---

<a name="top"/>
[请求](#request) | [响应](#response)

**获取数据集**操作将返回所有**磁贴**对象的 JSON 列表（其中包含 ID、标题和 embedUrl）或**组**中的 JSON 列表。

**所需的作用域**：Tile.Read.All

若要设置权限作用域，请参阅[注册客户端应用](https://msdn.microsoft.com/en-US/library/dn877542.aspx)或[注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)。
<a name="request"/>
##请求

GET https://api.powerbi.com/beta/myorg/{dashboard_id}/tiles

###URI 参数

| 名称| 描述| 数据类型|
|-|-|-|
| dashboard_id| 要使用的<b>仪表板</b>的 Guid。你可以使用[获取仪表板](Get-Dashboards.md)操作获取**仪表板** ID。| 字符串|

###组

组是用户所属的且在 Power BI 服务中可用的统一 **Azure Active Directory** 组的集合。
若要了解如何创建一个组，请参阅[创建组](https://support.powerbi.com/knowledgebase/articles/654250)。

GET https://api.powerbi.com/beta/myorg/groups/{group_id}/dashboards

###URI 参数

| 名称| 描述| 数据类型|
|-|-|-|
| group_id| 要使用的<b>组</b>的 Guid。你可以使用[获取组](Get-Groups.md)操作获取组 ID。| 字符串|
###标头

Authorization: Bearer eyJ0eX ...FWSXfwtQ 
<a name="response"/>
##响应

###状态代码

<table>
  <tr>
    <td>
      <b>代码</b>
    </td>
    <td>
      <b>描述</b>
    </td>
  </tr>
  <tr>
    <td>200</td>
    <td>OK.表示成功。数据集将在响应正文中返回。</td>
  </tr>
</table>

###Content-Type

application/json

###正文架构

**磁贴**具有 GUID **id**、**标题**和 **embedUrl**。
可以使用 embedUrl 将磁贴嵌入应用。
请参阅[集成 Power BI 磁贴](Integrate-a-Power-BI-Tile.md)。

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

###正文示例

下面是包含两个磁贴的 JSON 响应示例。

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




