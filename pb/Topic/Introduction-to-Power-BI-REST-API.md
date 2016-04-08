---
title: Power BI REST API 简介
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e115235-3b5a-4dcd-9526-bf8142c6c1f4
robots: noindex,nofollow
---
# Power BI REST API 简介
---

Power BI 开发人员体验（预览版）是在 Power BI 中推送数据源的 REST API。
当应用将行添加到数据集时，仪表板上的磁贴会通过已更新的数据进行自动更新。

在以前的 Power BI 版本中，所有模型和报表都连接到它们从中请求数据的数据源。
必须按计划刷新数据才能看到新数据。
借助 Power BI REST API，每当数据变为可用时 Power BI 便会推送数据，并且依赖于此数据的报表将实时更新。
这消除了使用请求模型时的延迟，并确保数据尽可能保持最新。

##使用 Power BI REST API 可以做什么？

- [创建数据集](Create-Dataset.md)
- 向/从数据集中[添加行](Add-Rows.md)或[删除行](Delete-Rows.md)，以使用新数据自动更新磁贴
- [获取数据集](Get-Datasets.md)
- [获取表](Get-Tables.md)
- [更新表架构](Update-Table-Schema.md)
- [获取组](Get-Groups.md)

##Power BI REST 和 JSON

Power BI 使用 REST 表示 Power BI Web API，使用 JSON 描述 Power BI 中的对象。

REST 是适用于 Web 编程项目的热门体系结构之一，因其简单性、易学性以及能够利用 HTTP 等现有 Web 协议而具有很大吸引力。


JSON 或称为 JavaScript 对象表示法，是使用用户可读文本传输由属性-值对所组成对象的开放标准格式。
它主要用于在服务器和 Web 应用程序之间传输数据（例如在 REST 实现中）。
在 Power BI 中使用 JSON 描述对象。

- 若要了解有关 REST 的详细信息，请参阅[维基百科](http://en.wikipedia.org/wiki/Representational_state_transfer)。
- 若要了解有关 JSON 的详细信息，请参阅 [JSON 简介](http://json.org/)。

**简单的 JSON 数据格式**

    {
        "name": "SalesMarketing",
        "tables": [
            {
                "name": "Product",
                "columns": [
                {
                    "name": "ProductID",
                    "dataType": "int"
                },
                {
                    "name": "Manufacturer",
                    "dataType": "string"
                },
                {
                    "name": "Category",
                    "dataType": "string"
                },
                {
                    "name": "Segment",
                    "dataType": "string"
                },
                {
                    "name": "Product",
                    "dataType": "string"
                },
                {
                    "name": "IsCompete",
                    "dataType": "bool"
                }
                ]
            }
        ]
    }

##Power BI REST URL

Power BI REST 资源对以下 Power BI 功能公开：

**根资源或对象**

    http(s)://api.powerbi.com/<Tenant>/<root>

**示例**

    https://api.powerbi.com/v1.0/myorg

**资源或对象的集合**

    http(s)://api.PowerBI.com/<Tenant>/<collection>/<key>

**示例**

    https://api.powerbi.com/v1.0/myorg/datasets

**对象**

    https://api.powerbi.com/v1.0/myorg/datasets/{dataset_guid}/tables/Product/rows

**组**

    https://api.powerbi.com/v1.0/myorg/groups/{group_guid}/datasets/{dataset_guid}

**操作（不受谓词支持时）**

    http(s)://api.powerbi.com/<Tenant>/<root>/<Operation>

其中：

- **租户** - 此 Power BI 实例的租户。
    作为快捷方式，可以提供“myorg”值。
    在这种情况下，将使用随请求发送的身份验证令牌中提供的租户。

- **根** - 根对象，例如“用户”、“表”或“磁贴”。

- **集合** - 集合的名称，例如“用户”、“表”或“磁贴”。

- **密钥** - 集合中的对象的唯一标识符。

- **操作** - 要执行的操作，例如“刷新”或“合并”。

###谓词

操作由可跨不同终结点提供一致功能的谓词支持。


**Power BI REST 谓词**

<table>
  <tr>
    <td>
      <strong>谓词</strong>
    </td>
    <td>
      <strong>描述 </strong>
    </td>
  </tr>
  <tr>
    <td>GET</td>
    <td>
      <p>获取指定内容类型的调用方中对象或集合的值（如果适用）： </p>
      <ul>
        <li>application/xml  </li>
        <li>application/json</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>PUT</td>
    <td>替换对象，或创建一个新的命名对象（如果适用）。 </td>
  </tr>
  <tr>
    <td>DELETE</td>
    <td>删除对象。</td>
  </tr>
  <tr>
    <td>POST</td>
    <td>创建一个新的未命名对象或追加到一个现有对象。返回已创建对象的位置（如果创建成功）。  </td>
  </tr>
</table>

##相关主题

- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [使用 Power BI 进行身份验证](http://go.microsoft.com/fwlink/?LinkId=519359)
- [注册应用](http://go.microsoft.com/fwlink/?LinkId=519361)
- [Power BI API 条款](https://powerbi.microsoft.com/en-us/api-terms)




