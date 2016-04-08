---
title: 获取导入
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee917f6-37a0-4061-8a82-4fd90ac85bb4
---
# 获取导入
---

[请求](#request) | [响应](#response)
<a name="top"/> | **[预览操作]**

**获取导入**操作将返回所有 **Import** 对象及其属性（包括 ID、导入源、**报表**和**数据集**）的 JSON 列表。


**所需作用域**：Metadata.View_Any
<a name="request"/>
##请求

GET https://api.powerbi.com/beta/myorg/imports

###标头

Content-Type: application/json
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

**导入**具有以下属性：

- ID
- 名称
- createdDateTime
- 导入源（文件或 URL 路径）
- **报表**数组

- **数据集**数组
    - **数据集**所具有的属性与[获取数据集](Get-Datasets.md)返回的属性相同。
    
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
    

###正文示例

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


