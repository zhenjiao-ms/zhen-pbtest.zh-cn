---
title: 获取仪表板
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39cc3286-8d68-4966-9fe5-36e9fb772cd2
---
# 获取仪表板
---

<a name="top"/>
[请求](#request) | [响应](#response)

**获取仪表板**操作将返回所有**仪表板**对象的 JSON 列表（其中包含 ID 和displayName）或**组**中的 JSON 列表。

**所需的作用域**：Dashboard.Read.All

若要设置权限作用域，请参阅[注册客户端应用](https://msdn.microsoft.com/en-US/library/dn877542.aspx)或[注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)。
<a name="request"/>
##请求

GET https://api.powerbi.com/beta/myorg/dashboards

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

**仪表板**具有 GUID **ID** 和**名称**。

    {
      "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#datasets","value":
      [
        {
          "id":"{dashboard_id GUID}",
          "displayName":"{dashboard_displayName}"
        }
      ]
    }

###正文示例

下面是 MyDashboard **仪表板** JSON 响应的示例。

    {
        "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#dashboards", "value":
        [
           {
              "id":"43127a01--e971d4cdc2fb",
                "displayName":"My Dashboard"
           }
        ]
    }

<a name="example"/>




