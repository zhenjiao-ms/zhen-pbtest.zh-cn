---
title: Power BI REST API 限制
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3cdde6a-0df8-4341-bace-0f24e57a8487
---
# Power BI REST API 限制
---

Power BI REST API 具有以下限制。

**发布行**

*   每个单个 POST 行请求最多 10,000 行
*   每个数据集每小时添加了 1,000,000 行
*   每个数据集最多 5 个挂起 POST 行请求
*   每个数据集每分钟 120 个 POST 行请求
*   FIFO 数据集中每个表最多存储了 200,000 行
*   “无保留策略”数据集中每个表最多存储了 5,000,000 行
*   POST 行操作中字符串列的每个值为 32,766 个字符

**每个 Power BI 计划的 POST 行操作**

*   用户使用免费服务计划创建的数据集：每个数据集每小时添加了 10,000 行
*   用户使用付费服务计划创建的数据集：每个数据集每小时添加了 1,000,000 行
*   如果用户超过此限制，后续 API 调用将失败，并显示以下详细信息：
    
    *   HTTP 状态代码：429 请求过多
    *   响应标头：稍后重试 + {重置其配额之前的秒数}
    *   OData 错误并显示以下消息：“超出了此数据集的每小时行数限制。
        若要每小时推送更多行，请将你的帐户升级到 Power BI Pro，或稍后重试推送。”


