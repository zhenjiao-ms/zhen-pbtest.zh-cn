---
title: 创建导入
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55c9125f-969b-4cbf-b97c-e1eafc466aef
---
# 创建导入
---

[请求](#request) | [响应](#response) | **[预览操作]**
<a name="top"/>

**创建导入**操作可以从 PBIX、Excel、程序包文件或从 OneDrive Pro 中的文件路径创建内容，然后返回所创建**导入**的 ID。
这允许你将 PBIX 或 Excel 文件等打包，然后将其导入 Power BI 工作区。
**导入** API 将 PBIX 程序包拆分为它的各个部分，包括数据集和报表。
然后，它将提供实际导入操作与其中创建的对象之间的元数据链接。

**注意：**该操作的行为与你在 Power BI 中通过连接屏幕导入 Excel 文件的行为相同。

**所需的作用域**：Content.Create
<a name="request"/>
##请求

POST https://api.powerbi.com/beta/myorg/imports

###查询字符串参数

<table><tr><td><b>名称</b></td><td><b>类型</b></td><td><b>说明</b></td><td><b>值</b></td></tr><tr><td>datasetDisplayName</td><td>字符串</td><td>当使用 PBIX 或 Excel 文件时，设置此参数将替代数据集的默认名称。</td><td>数据集名称。</td></tr><tr><td>nameConflict</td><td>字符串</td><td>确定存在同名数据集时要执行的操作。</td><td>- 忽略（默认）<br/>
- 中止<br/>
- 覆盖</td></tr></table>

###标头

Content-Type：multipart/form-data
Authorization: Bearer eyJ0eX ...FWSXfwtQ

###请求正文

-   如果从文件导入，正文将包含[以表单数据编码的](http://www.w3.org/TR/html401/interact/forms.html) PBIX 或 Excel 文件。

-   如果从 OneDrive Pro 导入，正文将包含 JSON 属性（其中包括文件 URL 的相对路径）。
- filePath 将使用 [OneDrive 文件 API 路径](https://msdn.microsoft.com/office/office365/APi/files-rest-operations#FileResource)。


例如：

```
    filePath = shared%20with%20everyone/Denver%20Data.xlsx 
```

###正文架构

**数据集**的 JSON 列表，以及包含名称和 ID 的属性。
还将返回**导入**操作的 ID。

**导入**操作具有 **filePath**。

    {
      "filePath": "shared with everyone/FileName.xlsx"
    }

<a name="response"/>
##响应

###响应标头

- 指向导入状态信息位置的位置标头。

- Retry-After 标头指示客户端要进行下一个请求而应等待的秒数

###成功状态代码

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
    <td>201</td>
    <td>Created.表示请求已完成且新的导入已创建。</td>
  </tr>
  <tr>
    <td>202</td>
    <td>如果导入仍在进行则已接受。</td>
  </tr>
</table>

###错误状态代码

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
    <td>409</td>
    <td>导入的文件已存在。</td>
  </tr>
</table>

###Content-Type

application/json


###正文架构

**创建导入**响应具有以下架构：

    {"id": "{import_guid"}


