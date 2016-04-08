---
title: 按文件路径获取导入
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e31b2a38-9653-4a9c-9920-37ce2b116032
---
# 按文件路径获取导入
---

[请求](#request) | [响应](#response) | **[预览操作]**
<a name="top"/>

**按文件路径获取导入**操作将返回 **Import** 对象及其属性（包括 ID、导入源、**报表**和**数据集**）的 JSON 正文。
将**导入文件路径**包含在 GET URI 的 filePath 查询字符串参数中以返回 **Import** 对象。

**所需作用域**：Metadata.View_Any
<a name="request"/>
##请求

GET https://api.powerbi.com/beta/myorg/imports?filePath={filePath}

###查询字符串参数

<table>
  <tr>
    <td>
      <b>名称</b>
    </td>
    <td>
      <b>描述</b>
    </td>
  </tr>
  <tr>
    <td>filePath</td>
    <td>导入文件的路径。</td>
  </tr>
</table>

###请求示例

    GET http://api.powerBI.com/beta/myorg/imports?filePath=http://microsoft-my.sharepoint.com/personal/{user name}/shared%20with%20everyone/mydoc.xlsx

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

###正文示例

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


