---
title: Power BI 客户端应用示例
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e52924d-695b-4762-860e-9f8bb1e8b344
---
# Power BI 客户端应用示例
---

[下载 .NET 入门客户端应用示例](http://go.microsoft.com/fwlink/?LinkId=619280) | [在 GitHub 上查看代码](https://github.com/Microsoft/PowerBI-CSharp/tree/master/samples/consoleapp/getting-started-for-dotnet)

**重要说明** 若要运行客户端应用示例，则必须在 **Azure Active Directory** (Azure AD) 中注册一个示例应用，并获取**客户端 ID**。 **客户端 ID** 用于对 ** Azure AD ** 进行身份验证。 请参阅[如何运行客户端应用示例](#run)。

Power BI 客户端应用示例演示了如何执行以下操作：

- 获取 Azure Active Directory 访问令牌以供身份验证
- 创建数据集
- 获取所有数据集
- 将行添加到数据集并从数据库中删除行
- 更新表架构
- 获取表格
- 获取组

**注意** 客户端应用示例还演示了如何在[组](Get-Groups.md)上调用 Power BI REST 操作。

**有关客户端应用示例的详细信息可从[对客户端应用进行身份验证](Authenticate-a-client-app.md)主题或[在 GitHub 上查看代码](https://github.com/Microsoft/PowerBI-CSharp/tree/master/samples/consoleapp/getting-started-for-dotnet)中获取**

<a name="run"/>
##如何运行客户端应用示例

客户端应用示例是一个控制台应用程序，用于演示如何对 Azure Active Directory 的 Power BI 客户端应用进行身份验证，以及调用包括组操作在内的所有 Power BI REST 操作。 要运行该示例，你需要执行以下操作：

1. [从 GitHub 下载 .NET 入门客户端应用示例](https://github.com/Microsoft/PowerBI-CSharp/tree/master/samples/consoleapp/getting-started-for-dotnet)
2. [在 Azure Active Directory 中注册客户端应用示例](#register)
3. [设置在注册该示例应用时所获取的客户端 ID](#set)

<a name="register"/>
###在 Azure Active Directory 中注册客户端应用示例

若要在客户端应用中调用 Power BI REST 操作，你需要使用**客户端 ID** 对 ** Azure AD ** 进行身份验证，以便你的应用可以访问 Power BI REST 资源。 若要获取**客户端 ID**，请在 **Windows Azure 管理门户**中注册示例应用。 在注册示例应用之前，下面是你需要的设置：

####客户端应用示例注册设置

| 设置| 值| 描述
|-|-|-|
| 名称| PowerBIClientSample| 这可以是任何唯一的名称。|
| 重定向 URI| https://login.live.com/oauth20_desktop.srf| 对于客户端应用，重定向 URI 为 Azure Active Directory 提供了关于将进行身份验证的应用程序的更多详细信息。此重定向 URI 可用于客户端应用。|
| 权限| 查看所有数据集、读取和写入所有数据集，以及查看用户组| 有关 Power BI 权限的详细信息，请参阅 [Power BI 权限](Power-BI-permissions.md)。|
####注册客户端应用示例

若要注册客户端应用示例，请参阅[注册客户端应用](Register-a-client-app.md)，并使用上述设置。 在注册客户端应用示例后，**Azure AD** 将生成该应用的**客户端 ID**。 若要运行该示例，你必须在其中设置 **clientID** 变量。 你可以从 **Azure 应用程序配置**页获取**客户端 ID**。 请参阅[如何获取客户端应用 ID](Register-a-client-app.md#clientID)。
<a name="set"/>
###设置在注册该示例应用时所获取的客户端 ID

若要运行该示例，你必须在其中设置 **clientID** 变量。 在 Program.cs 中，按如下方式设置 **clientID** 变量：

    private static string clientID = "{client id from Azure AD app registration}";

现在，你可以运行客户端应用示例了。

##相关主题

- [注册客户端应用](Register-a-client-app.md)
- [对客户端应用进行身份验证](Authenticate-a-client-app.md)
- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API 参考](Power-BI-REST-API-reference.md)




