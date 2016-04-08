---
title: 关于创建 Power BI 应用的简介
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09cf9c5a-517b-4e5a-856d-81c2aed8e032
robots: noindex,nofollow
---
# 关于创建 Power BI 应用的简介
---

借助 Power BI REST API，你可以创建自己的业务解决方案，以将数据实时推送到 Power BI 仪表板中。
当数据变更时，你的仪表板将进行实时更新。
你可以使用你所选的技术（包括 .NET、JQuery 或 Ruby）编写自己的应用。
Power BI REST API 使用诸如 OAuth2 的行业标准，并且具有 REST 操作，可将数据实时推送到 Power BI 仪表板中。

###下面是你创建 Power BI 应用时所需的内容

| 所需内容| 描述|
|-|-|
| [Azure Active Directory 租户](Create-an-Azure-Active-Directory-tenant.md)| Power BI 应用与 Azure Active Directory (Azure AD) 集成，从而为你的应用提供安全的登录和授权。|
| [注册客户端应用](Register-a-client-app.md)或[注册 Web 应用](Register-a-web-app.md)| 若要允许你的应用程序访问 Power BI REST API，你需要使用 Azure Active Directory 来注册应用程序。|
| [对应用进行身份验证](Authenticate-to-Power-BI-service.md)| Power BI 客户端应用使用 Active Directory (AAD) 对用户进行身份验证并保护应用程序。了解如何[对客户端应用进行身份验证](Authenticate-a-client-app.md)或[对 Web 应用进行身份验证](Authenticate-a-web-app.md)。|
| [调用 Power BI REST 操作](Power-BI-REST-API-reference.md)| 借助 Power BI REST API，你可以执行以下操作：创建和获取数据集、获取和更新表架构，以及添加和删除行。<br/><br/> **注意** 你可以在[组](Get-Groups.md)中调用任何 Power BI REST 操作。|
<a name="QuickStarts"/>
###调用每个 Power BI REST 操作快速入门

| 快速入门示例| 描述|
|-|-|
| [创建数据集示例](Create-Dataset.md#example)| 演示如何创建数据集。|
| [获取数据集示例](Get-Datasets.md#example)| 演示如何获取数据集，其中包括如何使用 **JavaScriptSerializer** 类**反序列化** JSON 响应。|
| [获取表示例](Get-Tables.md#example)| 演示如何获取数据集表。该示例还演示了如何使用 LINQ 查询获取数据集 ID，以及使用 **JavaScriptSerializer** 类**反序列化** JSON 响应。|
| [更新表架构示例](Update-Table-Schema.md#example)| 演示如何更新表架构。该示例还演示了如何使用 LINQ 查询获取数据集 ID。|
| [添加行示例](Add-Rows.md#example)| 演示如何将行添加到数据集。该示例还演示了如何使用 LINQ 查询获取数据集 ID。|
| [删除行示例](Delete-Rows.md#example)| 演示如何从数据集中删除行。该示例还演示了如何使用 LINQ 查询获取数据集 ID。|
| [获取组示例](Get-Groups.md#example)| 演示如何获取登录用户所属的组。该示例还演示了如何使用 **JavaScriptSerializer** 类**反序列化** JSON 响应。|
###Power BI 示例

| Power BI 示例| 描述|
|-|-|
| [客户端应用示例](Power-BI-client-app-sample.md)| 演示如何获取客户端应用的访问令牌并执行所有 Power BI REST 操作。|
| [ASP.NET Web 应用示例](Power-BI-web-app-sample.md)| 演示如何获取 Web 应用的访问令牌并执行 Power BI REST 操作。|


