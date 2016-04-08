---
title: 对 Web 应用进行身份验证
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc2d0c29-b2e6-41e4-933a-8a82ed55e9e8
robots: noindex,nofollow
---
# 对 Web 应用进行身份验证
---

[下载 Web 应用示例](http://go.microsoft.com/fwlink/?LinkId=619279) | 在 GitHub 上查看代码：[Default.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619431) | [Redirect.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619432)

本文介绍了如何对 Power BI Web 应用进行身份验证。
它包括采用 C# 的示例；但是，对于其他 Web 编程语言而言，身份验证过程都是相同的。
具有 [GitHub 上的 Web 应用示例](http://go.microsoft.com/fwlink/?LinkId=619279)。
若要了解如何运行示例，请参阅 [Web 应用示例](Power-BI-web-app-sample.md)。


Power BI Web 应用使用 Active Directory (AAD) 对用户进行身份验证并保护应用程序。
身份验证是识别应用或用户的过程。
若要在 AAD 中标识你的 Web 应用，使用 AAD 注册你的应用。
在 AAD 中注册 Web 应用时，需要对应用授予访问 Power BI REST API 资源的权限。
若要了解如何注册你的 Power BI Web 应用，请参阅[注册 Web 应用](Register-a-web-app.md)。

##本文内容

- [注册 Web 应用](#register)
- [配置 Power BI 设置以使用 Azure AD 进行身份验证](#configure)
- [创建一个查询字符串来从 Azure AD 获取授权代码](#create)
- [使用授权代码获取 Azure AD 访问令牌](#acquire)
- [使用 Azure AD 访问令牌调用 Power BI 操作](#use)
- [如何添加 Azure Active Directory 身份验证库](#add)

若要了解有关 Azure Active Directory (Azure AD) 授权流的详细信息，请参阅[授权代码授予流](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)。

**注意** 对于 Power BI，需使用 Azure 管理门户将应用创建为多租户应用。

##对 Power BI Web 应用进行身份验证所需的操作

以下是对 Power BI Web 应用进行身份验证以及执行 REST Web 请求的步骤。
以下步骤适用于 ASP.NET Web 应用；但是，这些步骤也适用于其他平台。
若要了解有关 Azure AD 中的 OAuth 2.0 的详细信息，请参阅 [Azure AD 中的 OAuth 2.0](https://msdn.microsoft.com/en-us/library/azure/dn645545.aspx)。
<a name="register"/>
###步骤 1 - 注册你的 Web 应用

在 Azure Active Directory 中注册 Web 应用时，需要对应用授予访问 Power BI REST API 资源的权限。
若要注册 Power BI Web 应用，请参阅[注册 Web 应用](Register-a-web-app.md)。
<a name="configure"/>
###步骤 2 - 配置 Power BI 设置以使用 Azure AD 进行身份验证

以下是使用 Azure AD 对 Power BI Web 应用进行身份验证时所需的设置：

<table>
  <tr>
    <td>
      <b>设置</b>
    </td>
    <td>
      <b>描述</b>
    </td>
    <td>
      <b>值</b>
    </td>
  </tr>
  <tr>
    <td>客户端 ID</td>
    <td>应用程序使用客户端 ID 向用户（应用程序向其请求权限）标识自身。</td>
    <td>若要获取 Power BI 应用客户端 ID，请参阅[如何获取客户端应用 ID] (Register+a+web+app.md#clientID)。</td>
  </tr>
  <tr>
    <td>客户端密钥</td>
    <td>在向 Azure AD 进行身份验证以调用 Web API 时，客户端密钥与客户端 ID 一起发送。</td>
    <td>若要获取 Power BI 应用客户端密钥，请参阅[如何获取客户端密钥] (Register+a+web+app.md#clientSecret)。</td>
  </tr>
  <tr>
    <td>资源 URI</td>
    <td>要获得授权的 Power BI 资源的资源 URI。必须使用以下确切 URI。</td>
    <td>https://analysis.windows.net/powerbi/api</td>
  </tr>
  <tr>
    <td>颁发机构 URI</td>
    <td>颁发机构 URI 是采用客户端 ID 获取访问令牌的 Azure 资源。</td>
    <td>https://login.windows.net/common/oauth2/authorize</td>
  </tr>
  <tr>
    <td>重定向 URL</td>
    <td>Web 应用 URL 的重定向 URL。Azure AD 服务将重定向回包含身份验证代码的 Web 应用 URL。</td>
    <td>示例：http://localhost:13526/Redirect</td>
  </tr>
</table>

<a name="create"/>
###步骤 3 - 创建一个查询字符串来从 Azure AD 获取授权代码

若要对 Power BI Web 应用进行身份验证，请首先创建一个 URL 查询字符串以重定向到 Azure AD 身份验证服务。
提供有效凭据后，Azure AD 将返回授权代码。
以下示例演示了带有查询字符串的完全限定的 Azure AD URL。
你可以使用与此类似的 URL 从 Azure AD 获取授权代码。
使用 **Response.Redirect**() 重定向到将从 Azure AD 返回授权代码的 Azure AD 服务。
使用步骤 4 中的授权代码，通过授权代码获取访问令牌。

    https://login.windows.net/common/oauth2/authorize
      ?response_type=code
      &client_id=1861585d...9a79c296
      &resource= https://analysis.windows.net/powerbi/api
      &redirect_uri= http://localhost:13526/Redirect

**Power BI 身份验证查询字符串设置**

<table>
  <tr>
    <td>
      <b>设置</b>
    </td>
    <td>
      <b>描述</b>
    </td>
  </tr>
  <tr>
    <td>response_type=code</td>
    <td>Azure AD 返回授权代码。</td>
  </tr>
  <tr>
    <td>client_id=1861585d...9a79c296</td>
    <td>应用程序使用客户端 ID 向用户（应用程序向其请求权限）标识自身。将在注册你的 Azure 应用时获取客户端 ID。</td>
  </tr>
  <tr>
    <td>resource= https://analysis.windows.net/powerbi/api</td>
    <td>要获得授权的 Power BI 资源的资源 URI。必须使用以下确切 URI。</td>
  </tr>
  <tr>
    <td>redirect_uri= http://localhost:13526/Redirect</td>
    <td>用户进行身份验证后，Azure AD 将重定向回 Web 应用。</td>
  </tr>
</table>

下面是一个 C# 代码示例，演示了如何使用查询字符串创建一个 Azure AD 授权 URL 以及重定向到 Azure AD 颁发机构服务。


**示例 – C# 登录**

        protected void signInButton_Click(object sender, EventArgs e)
        {
            //Create a query string
            //Create a sign-in NameValueCollection for query string
            var @params = new NameValueCollection
            {
                //Azure AD will return an authorization code. 
                //See the Redirect class to see how "code" is used to AcquireTokenByAuthorizationCode
                {"response_type", "code"},
                //Client ID is used by the application to identify themselves to the users that they are requesting permissions from. 
                //You get the client id when you register your Azure app.
                {"client_id", Properties.Settings.Default.ClientID},
                //Resource uri to the Power BI resource to be authorized
                {"resource", "https://analysis.windows.net/powerbi/api"},
                //After user authenticates, Azure AD will redirect back to the web app
                {"redirect_uri", "http://localhost:13526/Redirect"}
            };
            //Create sign-in query string
            var queryString = HttpUtility.ParseQueryString(string.Empty);
            queryString.Add(@params);
            //Redirect authority
            //Authority Uri is an Azure resource that takes a client id to get an Access token
            string authorityUri = "https://login.windows.net/common/oauth2/authorize/";
            Response.Redirect(String.Format("{0}?{1}", authorityUri, queryString));       
        }

<a name="acquire"/>
###步骤 4 – 使用授权代码获取 Azure AD 访问令牌

若要向 Power BI REST 服务发出数据请求，你需要提供一个访问令牌。
在 .NET Web 应用中，需使用 [Windows Azure 身份验证库 (ADAL)](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx) 获取访问令牌。
如果你没有 [Windows Azure 身份验证库 (ADAL)](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)，请参阅[如何添加 Azure Active Directory 身份验证库](#add)。

在你的应用重定向到 Azure AD 颁发机构 URI 并获取授权代码后，你的应用将通过授权代码获取令牌。
下面介绍了你的应用如何获取授权代码并获取访问令牌：
**在“重定向”类中**：

1.  获取从 Azure AD 返回的 Azure AD 身份验证代码。

    ```
    string code = Request.Params.GetValues(0)[0];
    ```
2.  创建传递颁发机构 URI 的 **AuthenticationContext** 和 TokenCache。

    ```
    TokenCache TC = new TokenCache();

    AuthenticationContext AC = new AuthenticationContext(authorityUri, TC);
    ```

3.  创建传递 Azure 应用**客户端 ID** 和 Azure 应用**客户端密钥**的 **ClientCredential**。


    ```
    ClientCredential cc = new ClientCredential(Properties.Settings.Default.ClientID, Properties.Settings.Default.ClientSecretKey);
    ```

4.  通过传递由 Azure AD 返回的**身份验证代码**和**重定向 URL** 的身份验证代码获取令牌。

    ```
    AuthenticationResult AR = AC.AcquireTokenByAuthorizationCode(code, new Uri(redirectUri), cc);
    ```

5.  重定向回 default.aspx。

    ```
    Response.Redirect("/Default.aspx");
    ```

6.  将会话“authResult”索引字符串设置为 AuthenticationResult，以便使用 default.aspx 页面中的结果。

    ```
    Session["authResult"] = AR;
    ```

**在 Default.aspx 页面中**：

1.  从会话中获取 **AuthenticationResult**。
    在步骤 4 中，使用 **AuthenticationResult** 获取授权 **AccessToken**。

    ```
    AuthenticationResult authResult = (AuthenticationResult)Session["authResult"];
    ```

下面是通过授权代码获取 Azure 访问令牌并重定向回 default.aspx 的完整代码。

**注意** 重定向 URI 必须与请求授权代码时使用的 redirect_uri 匹配。

    public partial class Redirect : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            //Redirect uri must match the redirect_uri used when requesting Authorization code.
            string redirectUri = "http://localhost:13526/Redirect";
            string authorityUri = "https://login.windows.net/common/oauth2/authorize/";
            // Get the auth code
            string code = Request.Params.GetValues(0)[0];
            // Get auth token from auth code       
            TokenCache TC = new TokenCache();
            AuthenticationContext AC = new AuthenticationContext(authorityUri, TC); 
            ClientCredential cc = new ClientCredential
                (Properties.Settings.Default.ClientID,
                Properties.Settings.Default.ClientSecretKey);
            AuthenticationResult AR = AC.AcquireTokenByAuthorizationCode(code, new Uri(redirectUri), cc);
            //Set Session "authResult" index string to the AuthenticationResult
            Session["authResult"] = AR;
            //Redirect back to Default.aspx
            Response.Redirect("/Default.aspx");
        }
    protected void Page_Load(object sender, EventArgs e)
    {
        //Test for AuthenticationResult
        if (Session["authResult"] != null)
        {
            //Get the authentication result from the session
            authResult = (AuthenticationResult)Session["authResult"];
            //Show Power BI Panel
            PBIPanel.Visible = true;
            signinPanel.Visible = false;
            //Set user and toek from authentication result
            userLabel.Text = authResult.UserInfo.DisplayableId;
            accessTokenTextbox.Text = authResult.AccessToken;
        }
        else
        {
            PBIPanel.Visible = false;
        }
    }

<a name="use"/>
###步骤 5 – 使用 Azure AD 访问令牌调用 Power BI 操作

通过在“Authorization”标头中传递通过 Azure Active Directory 获得的访问令牌，Power BI REST API 调用以经过身份验证的用户名义发起。
从 Active Directory (AAD) 获取访问令牌后，需使用该令牌向 Power BI REST API 发出 Web 请求。


通过授权代码 (**AcquireTokenByAuthorizationCode**) 获取令牌对 **AuthenticationResult** 完成设置后，需通过获取 **AuthenticationResult** 的 **AccessToken** 属性获取 Azure 访问令牌。
以下是获取 Power BI **数据集**的代码。
此代码示例将创建 **WebRequest** 并反序列化数据集的响应字符串。
若要访问 Power BI REST API，你可以创建一个请求标头，将“Authorization”设置为“Bearer {AccessToken}”：

    request.Headers.Add("Authorization", String.Format("Bearer {0}", authResult.AccessToken));

**C# 示例 – 获取 Power BI 数据集**

        protected void getDatasetsButton_Click(object sender, EventArgs e)
        {
            string responseContent = string.Empty;
            //The resource Uri to the Power BI REST API resource
            string datasetsUri = "https://api.powerbi.com/v1.0/myorg/datasets";
            //Configure datasets request
            System.Net.WebRequest request = System.Net.WebRequest.Create(datasetsUri) as System.Net.HttpWebRequest;
            request.Method = "GET";
            request.ContentLength = 0;
            request.Headers.Add("Authorization", String.Format("Bearer {0}", authResult.AccessToken));
            //Get datasets response from request.GetResponse()
            using (var response = request.GetResponse() as System.Net.HttpWebResponse)
            {
                //Get reader from response stream
                using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
                {
                    responseContent = reader.ReadToEnd();
                    //Deserialize JSON string
                    //JavaScriptSerializer class is in System.Web.Script.Serialization
                    JavaScriptSerializer json = new JavaScriptSerializer();
                    Datasets datasets = (Datasets)json.Deserialize(responseContent, typeof(Datasets));
                    resultsTextbox.Text = string.Empty;
                    //Get each Dataset from 
                    foreach (dataset ds in datasets.value)
                    {
                        resultsTextbox.Text += String.Format("{0}\t{1}\n", ds.Id, ds.Name);
                    }
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

<a name="add"/>
##如何添加 Azure Active Directory 身份验证库

在 .NET 客户端应用中，使用 Active Directory 身份验证库中的 **AuthenticationContext** 获取 Azure 访问令牌。
你可以从 Visual Studio 安装 Active Directory 身份验证库 NuGet 程序包。
在安装 NuGet 程序包时，Visual Studio 将创建对所需程序集的引用。

1. 右键单击某个解决方案。
2. 选择“管理 NuGet 程序包”。
3. 搜索“Active Directory 身份验证库”。
4. 选择程序包列表中的“Active Directory 身份验证库”，然后单击“安装”。

##相关主题

* [适用于 .NET 的 Azure AD 身份验证库](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)
* [适用于 .NET 的 Active Directory 身份验证库 (ADAL) v1](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/)
* [Azure AD 中的 OAuth 2.0](https://msdn.microsoft.com/en-us/library/azure/dn645545.aspx)
* [授权代码授予流](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)
* [Azure AD 的身份验证方案](https://msdn.microsoft.com/en-US/library/azure/dn499820.aspx)





