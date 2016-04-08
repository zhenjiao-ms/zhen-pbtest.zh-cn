---
title: 表操作
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ebd7c9ea-999c-4727-a7f6-fe145ad0ebd7
robots: nofollow
---
# 表操作
---

**表**资源路径是一个特定**数据集**的**表**对象的集合。
**表**具有包含数据的**行**和**列**。
**列**具有一个名称和**数据类型**。

Power BI REST API 具有以下**表**操作：

- [获取表](Get-Tables.md)：将返回指定**数据集**的**表**的 JSON 列表。
- [更新表架构](Update-Table-Schema.md)：更新现有的**表**架构。

###表 JSON

**表**具有一个名称和**列**对象的集合。
**列**具有一个**名称**和**数据类型**。

    {
         "name": "table_name",
         "columns":[
            {
               "name": "column_name",
               "dataType": "data_type"
            },
        …
        ]
    }

###另请参阅

- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API 参考](Power-BI-REST-API-reference.md)
- [Power BI REST API 概述](Overview-of-Power-BI-REST-API.md)



