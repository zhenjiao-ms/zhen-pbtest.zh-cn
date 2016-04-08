---
title: 对客户端应用进行身份验证
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 53e5ee1c-2a6e-414c-bb2b-798e7f148a9f
robots: noindex,nofollow
---
# 对客户端应用进行身份验证
---

[下载 .NET 客户端应用示例](http://go.microsoft.com/fwlink/?LinkId=619280) | [在 GitHub 上查看代码](http://go.microsoft.com/fwlink/?LinkId=619429)

本文介绍了如何对 Power BI 客户端应用进行身份验证。
它包括采用 C# 的示例；但是，对于其他编程语言而言，身份验证过程都是相同的。

有关演示如何对 Power BI 客户端应用进行身份验证的完整 C# 示例，请参阅[客户端应用示例](Power-BI-client-app-sample.md)。

##本文内容

*   [对 Power BI 客户端应用进行身份验证所需的操作](#What)
*   [如何使用令牌向 Power BI REST API 发起数据请求](#Datarequest)
*   [Azure 身份验证上下文流](#Flow)
*   [如何添加 Azure Active Directory 身份验证库](#Library)

Power BI 客户端应用使用 **Azure Active Directory** (AAD) 对用户进行身份验证并保护应用程序。
身份验证是识别应用或用户的过程。
若要在 AAD 中标识你的客户端应用，你需要使用 AAD 注册你的应用。
在 AAD 中注册客户端应用时，需要对应用授予访问 Power BI API 的权限。
若要了解如何注册你的 Power BI 客户端应用，请参阅[注册客户端应用](Register-a-client-app.md)。

通过在请求的“Authorization”标头中传递令牌，Power BI REST API 调用以经过身份验证的用户名义发起。
令牌通过 Azure Active Directory 获取。

**注意** 对于 Power BI Private Preview，需使用 Azure 管理门户将应用创建为多租户应用。

<a name="What"></a>

##对 Power BI 客户端应用进行身份验证所需的操作

若要对 Power BI 客户端应用进行身份验证并执行 REST Web 请求，你需要：

1.  **注册客户端应用   ** - 若要注册 Power BI 客户端应用，请参阅[注册客户端应用](Register-a-client-app.md)。
    在 **Azure Active Directory** 中注册客户端应用时，需要对应用授予访问 Power BI API 的权限。
2.  **为你的应用分配客户端 ID** - 若要为你的应用获取客户端 ID，请参阅[如何获取客户端应用 ID](Register-a-client-app.md#clientID)。
    应用程序使用该客户端 ID 向用户（应用程序向其请求权限）标识自身。
    
    *   在客户端应用代码中，将 **clientID** 变量分配到 Azure 应用程序的客户端 ID。
3.  **分配重定向 URI** - 对于客户端应用，重定向 URI 将为 AAD 提供它将进行身份验证的特定应用程序的更多详细信息。
    统一资源标识符 (URI) 是一个用于标识资源名称的值。
    
    *   在客户端应用代码中，将 **redirectUri** 分配到“https://login.live.com/oauth20_desktop.srf”。
        由于客户端应用没有要重定向到的外部服务，所以此 URI 是客户端应用的标准占位符。
4.  **为 Power BI API 分配资源 URI** - 资源 URI 可标识 Power BI API 资源。
    
    *   在客户端应用代码中，将 **resourceUri** 分配到“https://analysis.windows.net/powerbi/api”。
5.  **分配 OAuth2 颁发机构 URI ** - 颁发机构 URI 可标识 OAuth2 颁发机构资源。
    
    *   在客户端应用代码中，将颁发机构 URI 分配到“https://login.windows.net/common/oauth2/authorize”。
6.  **为 Power BI API 数据集分配数据集 URI** - 数据集 URI 可标识 Power BI API 数据集资源。
    
    *   在客户端应用代码中，将 **datasetsUri** 分配到“https://api.powerbi.com/v1.0/myorg/datasets”。

若要向 Power BI REST 服务发出数据请求，你需要提供一个访问令牌。
在 .NET 客户端应用中，使用 [Windows Azure 身份验证库 (ADAL)](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx) 获取访问令牌。
下面介绍了该过程。
以下是 **AccessToken()** 方法示例。

如果你没有 Windows Azure 身份验证库 (ADAL)，请参阅[如何添加 Azure Active Directory 身份验证库](#Library)。

**重要说明** 若要对客户端应用进行身份验证，你必须添加对 **Microsoft.IdentityModel.Clients.ActiveDirectory** 的引用，它包含在 Windows Azure 身份验证库 (ADAL) 中。

##获取访问令牌的步骤

1.  **创建 AuthenticationContext 的实例** - AuthenticationContext 是表示 Azure AD 资源的令牌颁发机构的主类。
    构造函数采用：
    
    *   OAuth2 authorityUri 


    ```
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
    AuthenticationContext  authContext = new AuthenticationContext(authorityUri);

    ```

1.  **调用 AuthenticationContext.AcquireToken() 来获取令牌** - 该方法采用：
    
    *   Power BI API resourceUri
    *   Power BI 应用 clientID
    *   Power BI 应用 redirectUri 


    ```
    string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;

    ```

有关 AuthenticationContext 为获取令牌所执行的操作的详细信息，请参阅 [Azure 身份验证上下文流](#Flow)。

###C# 示例 - 获取访问令牌

在 .NET 客户端应用中，使用 **AuthenticationContext** 获取访问令牌。


```
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

```

<a name="Datarequest"></a>

##使用令牌向 Power BI REST API 发出数据请求

从 Active Directory (AAD) 获取访问令牌后，需使用该令牌向 Power BI REST API 发出 Web 请求。
若要创建 Power BI REST Web 请求，请将访问令牌添加到请求标头，如下所示：


```
request.Headers.Add("Authorization", String.Format("Bearer {0}", AccessToken()));

```


###C# 示例 - 使用令牌的 Power BI REST API 数据请求

有关演示如何对 Power BI 客户端应用进行身份验证并调用所有 Power BI REST 操作的完整 C# 示例，请参阅[客户端应用示例](Power-BI-client-app-sample.md)或[在 GitHub 上查看代码](http://go.microsoft.com/fwlink/?LinkId=619429)。


```
        static dataset[] GetDatasets()
        {
            //This is sample code to illustrate a Power BI operation. 
            //In a production application, refactor code into specific methods and use appropriate exception handling.

            //Power BI Datasets Url
            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg/datasets";

            //Get Azure AD access token (see above)
            string token = AccessToken();

            //GET web request to list all datasets.
            //To get a datasets in a group, use the Groups uri: https://api.PowerBI.com/v1.0/myorg/groups/{group_id}/datasets
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "GET";
            request.ContentLength = 0;
            request.ContentType = "application/json";

            //Add access token to Request header
            request.Headers.Add("Authorization", String.Format("Bearer {0}", token));

            //Get HttpWebResponse from GET request
            using (HttpWebResponse httpResponse = request.GetResponse() as System.Net.HttpWebResponse)
            {
                //Get StreamReader that holds the response stream
                using (StreamReader reader = new System.IO.StreamReader(httpResponse.GetResponseStream()))
                {
                    string responseContent = reader.ReadToEnd();

                    JavaScriptSerializer jsonSerializer = new JavaScriptSerializer();
                    Datasets datasets = (Datasets)jsonSerializer.Deserialize(responseContent, typeof(Datasets));

                    return datasets.value;
                }
            }
        }

public class Datasets
{
    public dataset[] value { get; set; }
}

public class dataset
{
    public string Id { get; set; }
    public string Name { get; set; }
}


```

<a name="Flow"></a>

##Azure 身份验证上下文流

在 .NET 客户端应用中，使用 **AuthenticationContext** 获取 Azure 访问令牌。
**AuthenticationContext** 是表示 Azure AD 资源的令牌颁发机构的主类。
**AuthenticationContext** 执行以下操作：

1.  AuthenticationContext 通过将用户代理重定向到 Azure Active Directory 授权终结点来启动流。
    用户进行身份验证并同意（如果需要同意）。
2.  Azure Active Directory 授权终结点再将带有授权代码的用户代理重定向到 AuthenticationContext。
    用户代理将授权代码返回到客户端应用程序的重定向 URI。
3.  AuthenticationContext 从 Azure Active Directory 令牌颁发终结点请求一个访问令牌。
    它将提供授权代码来证明用户已经同意。
4.  Azure Active Directory 令牌颁发终结点返回访问令牌。
5.  客户端应用程序使用该访问令牌对 Web API 进行身份验证。
6.  对客户端应用程序进行身份验证后，Power BI REST API 将返回所请求的数据。

若要了解有关 Azure Active Directory (Azure AD) 授权流的详细信息，请参阅[授权代码授予流](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)。

<a name="Library"></a>

##如何添加 Azure Active Directory 身份验证库

在 .NET 客户端应用中，使用 **Active Directory 身份验证库**中的 **AuthenticationContext** 获取 Azure 访问令牌。
你可以从 Visual Studio 安装 **Active Directory 身份验证库** NuGet 程序包。
在安装 NuGet 程序包时，Visual Studio 将创建对所需程序集的引用。

1.  右键单击某个解决方案。
2.  选择“管理 NuGet 程序包”****。
3.  搜索“Active Directory 身份验证库”****。
4.  选择程序包列表中的“Active Directory 身份验证库”****，然后单击“安装”****。

##相关主题

*   [适用于 .NET 的 Azure AD 身份验证库](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)
*   [适用于 .NET 的 Active Directory 身份验证库 (ADAL) v1](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/)
*   [Azure AD 中的 OAuth 2.0](https://msdn.microsoft.com/en-us/library/azure/dn645545.aspx)
*   [授权代码授予流](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)
*   [Azure AD 的身份验证方案](https://msdn.microsoft.com/en-US/library/azure/dn499820.aspx)


