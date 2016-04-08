---
title: 对 Power BI 服务进行身份验证
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b3e918a-9699-414d-9c25-87ecd1fa972b
robots: noindex,nofollow
---
# 对 Power BI 服务进行身份验证
---


本文介绍了 Power BI 中的身份验证，以及如何使用客户端 ID 获取访问令牌。
若要开始创建 Power BI 应用，请参阅[开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)。

Power BI REST API 是基于 REST 的 API，用于提供对仪表板资源（例如数据集、表和行）的编程式访问。
有关 Power BI REST API 简介，请参阅 [Power BI REST API 简介](Introduction-to-Power-BI-REST-API.md)。
若要为你的应用提供安全的登录和授权，使用 ** Azure Active Directory** (Azure AD) 对你的应用进行身份验证。


###本文内容

- [Power BI 身份验证简介](#intro)
- [Azure 应用客户端 ID](#clientID)
- [Azure Web 应用客户端密钥](#clientSecret)

<a name="intro"/>
##Power BI 身份验证简介

Power BI 应用与 ** Azure Active Directory** 集成，从而为你的应用提供安全的登录和授权。
若要将 Power BI 应用与 Azure AD 集成，请使用 Azure 管理门户向 Azure AD 注册有关应用程序的详细信息。
当你在 Azure Active Directory 中注册应用时，该应用程序会将身份验证外包给 Azure AD。
应用程序注册涉及到向 Azure AD 告知你的应用程序，包括它所在的 URL、完成身份验证后用于发送答复的 URL，以及用于标识应用程序的 URI。
在 Azure AD 中注册客户端应用或 Web 应用时，需要对应用授予访问 Power BI REST API 的权限。

Power BI 应用使用**客户端 ID**来向 Azure AD 标识自身。
请参阅 [Azure 应用客户端 ID](#clientID)。
对于 Web 应用，你还需要一个客户端密钥。
请参阅 [Azure Web 应用客户端密钥](#clientSecret)。

若要了解如何注册 Power BI 应用并对其进行身份验证：

- **Power BI 客户端应用**：请参阅[注册客户端应用](Register-a-client-app.md)和[对 Power BI 客户端应用进行身份验证](Authenticate-a-client-app.md)。

- **Power BI Web 应用**：请参阅[注册 Web 应用](Register-a-web-app.md)和[对 Power BI Web 应用进行身份验证](Authenticate-a-web-app.md)。

- 了解如何在不同平台上使用 Azure 身份验证：[Azure 身份验证库](https://msdn.microsoft.com/library/azure/dn151135.aspx)可在不同平台上使用，从而帮助开发人员轻松地对要访问云或内部 Active Directory (AD) 的用户进行身份验证来获取访问令牌，以进行安全的 API 调用。
    本主题包含了在不同平台上可用的身份验证库和每个库的有用资源（包括源代码和示例）的路线图。

<a name="clientID"/>
##Azure 应用客户端 ID

Azure 应用具有一个**客户端 ID**，应用程序在向用户请求权限时使用该 ID 向用户标识自身。
使用**客户端 ID** 获取身份验证令牌。
若要获取 Azure **客户端 ID**，请参阅[如何获取客户端应用 ID](Register-a-client-app.md#clientID)。

有关如何使用 Azure** 客户端 ID** 对客户端应用进行身份验证的完整示例，请参阅[对客户端应用进行身份验证](Authenticate-a-client-app.md)。

例如，下面的 C# 代码使用 Azure 应用客户端 ID 来获取访问令牌。


      static string AccessToken()
      {
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";
            string clientId = {clientIDFromAzureAppRegistration};
            //A redirect uri gives AAD more details about the specific application that it will authenticate.
            //Since a client app does not have an external service to redirect to, this Uri is the standard placeholder for a client app.
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";
            // Create an instance of AuthenticationContext to acquire an Azure access token
            // OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            //  AcquireToken takes a Client Id that Azure AD creates when you register your client app.
            //  To learn how to register a client app and get a Client ID, see https://msdn.microsoft.com/en-US/library/dn877542(Azure.100).aspx   
            string token = authContext.AcquireToken(resourceUri, clientID, new Uri(redirectUri)).AccessToken;
            return token;
      }

<a name="clientSecret"/>
##Azure Web 应用客户端密钥

当你注册 Web 应用时，你会收到一个客户端**密钥**。
Web 应用使用该客户端**密钥**向 **Power BI 服务**安全标识自身。
若要获取 Azure 客户端**密钥**，请参阅[如何获取客户端密钥](Register-a-web-app.md#clientSecret)。

有关如何使用 Azure **客户端 ID** 和客户端**密钥**对 Web 应用进行身份验证的完整示例，请参阅[对 Web 应用进行身份验证](Authenticate-a-web-app.md)。

##相关主题

- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [如何获取 Azure Active Directory 租户](https://azure.microsoft.com/en-us/documentation/articles/active-directory-howto-tenant/)
- [创建 Azure Active Directory 租户](Create-an-Azure-Active-Directory-tenant.md)
- [注册客户端应用](Register-a-client-app.md)
- [注册 Web 应用](Register-a-web-app.md)



