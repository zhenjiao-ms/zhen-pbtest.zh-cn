---
title: Power BI REST API 参考
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e05cad22-434f-4d4c-90d7-459ea3b255af
---
# Power BI REST API 参考
---

Power BI REST API 是基于 REST 的 API，用于提供对 Power BI 中**仪表板**资源（例如**数据集**、**表**和**行**）的编程式访问。 Power BI 是一种基于云的服务，可用于生成自定义仪表板应用程序。 该 API 具有将**数据集**推送到**仪表板**的概念。 **数据集**是**表**的集合。 **表**具有包含数据的行和列。

Power BI REST API 具有以下操作：

- [数据集操作](Dataset-operations.md)：获取和创建数据集。
- [表操作](Table-operations.md)：获取表和更新表架构。
- [行操作](Row-operations.md)：添加行和删除行。
- [组操作](Group-operations.md)：获取组。
- [导入操作](Import-operations.md)：创建导入、获取导入、从 GUID 中获取导入和通过文件路径来获取导入。
- [仪表板操作](Dashboard-operations.md)：获取仪表板和获取磁贴。

若要对 Power BI 资源执行操作，请使用支持的方法（GET、POST、PUT 或 DELETE），将 HTTP 请求发送到面向资源集合或特定资源的端点。 Power BI 操作需要一个 Azure Active Directory (AAD) 访问令牌。 若要了解有关如何对 Power BI 服务进行身份验证的详细信息，请参阅[对 Power BI 服务进行身份验证](Authenticate-to-Power-BI-service.md)。

##相关主题

- [Power BI REST API 简介](Introduction-to-Power-BI-REST-API.md)
- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)






