---
title: 数据集操作
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d25be11a-ee96-47eb-b808-c070145329fb
---
# 数据集操作
---

**数据集**资源路径是已登录的 Power BI 用户所拥有的一个**表**集合。
**表**具有包含数据的**行**和**列**。
使用 Power BI Rest API，你可以创建新数据集，并将数据添加到这些数据集中的表。
当新数据可用时，将自动更新使用这些数据集的仪表板。

Power BI REST API 具有以下**数据集**操作：
- [获取数据集](Get-Datasets.md)：将返回所有**数据集**对象的 JSON 列表，其中包含名称和 ID。
- [创建数据集](Create-Dataset.md)：从 JSON 架构定义创建新的**数据集**对象。

###数据集 JSON

**数据集**有一个**表**对象的集合。

    {
       "name": "dataset_name",
       "tables":[
        …
        ]
    }

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

##另请参阅

- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API 参考](Power-BI-REST-API-reference.md)
- [Power BI REST API 概述](Overview-of-Power-BI-REST-API.md)



