---
title: 行操作
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf65ea37-e8c7-4b9c-8536-8e60e8555350
---
# 行操作
---

**行**资源路径是一个**表**的行集合。

Power BI REST API 具有以下**行**操作：

- [添加行](Add-Rows.md)：将**行**添加到**数据集**中的**表**。
- [删除行](Delete-Rows.md)：从**数据集**中的**表**删除**行**。

###行 JSON

**行**有一个**列**名称和值的集合。

    {
        "rows":
            [
                {"column_name1": value, "column_name2": value, "column_name3": value, ...}
            ]
    }

### 另请参阅
- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API 参考](Power-BI-REST-API-reference.md)
- [Power BI REST API 概述](Overview-of-Power-BI-REST-API.md)



