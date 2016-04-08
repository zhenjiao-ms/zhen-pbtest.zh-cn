---
title: 组操作
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 87c5bab7-de5d-44cd-908a-e187e26abb9a
---
# 组操作
---

Power BI 应用可以将内容发布到用户的个人工作区，并对用户有权访问的任何组进行相同的 REST API 调用。
若要将数据集发布到用户的个人工作区，请继续向 https://api.powerbi.com/v1.0/myorg/datasets 发送 POST 请求。
如果你想要将该数据集发布到用户所属的组中，请向 https://api.powerbi.com/v1.0/myorg/groups/{groupId}/datasets 发送 POST 请求。


为了获取组 ID 以放入 URL 中，需调用**获取组** 操作。

    GET https://api.powerbi.com/v1.0/myorg/groups

Power BI REST API 具有以下**组**操作：

- [获取组](Get-Groups.md)：将返回包含所有 **Group** 对象的 JSON 列表。

###另请参阅

- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API 参考](Power-BI-REST-API-reference.md)
- [Power BI REST API 概述](Overview-of-Power-BI-REST-API.md)



