---
title: 获取组
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e8e5b65-49cc-4b7b-8494-d46172415608
---
# 获取组
---

<a name="top"/>
[请求](#request) | [响应](#response) | [示例](#example) | [试用 Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [客户端和 Web 应用示例](Power-BI-Samples.md)

**获取组**操作将返回登录用户所属的所有**组**的 JSON 列表。


**所需的权限范围**：Dataset.ReadWrite.All 或 Dataset.Read.All

若要设置权限作用域，请参阅[注册客户端应用](https://msdn.microsoft.com/en-US/library/dn877542.aspx)或[注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)。
<a name="request"/>
##请求

GET https://api.powerbi.com/v1.0/myorg/groups

###标头

Authorization: Bearer eyJ0eX ...FWSXfwtQ 
<a name="response"/>

##响应

###状态代码

<table>
  <tr>
    <td>
      <b>代码</b>
    </td>
    <td>
      <b>描述</b>
    </td>
  </tr>
  <tr>
    <td>200</td>
    <td>OK.表示成功。数据集将在响应正文中返回。</td>
  </tr>
</table>

###Content-Type

application/json
###正文架构

**组**具有组名称的数组。
每个**组**都有一个 **ID** 和**名称**。


    {
      "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#groups","value":[
        {
          "id":"{guid}","name":"{group_name}"
        },
        {
          ...
        }
      ]
    } 

###正文示例

对于包含第一至第四季度产品的**组**，使用 JSON。

    {
      "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#groups","value":[
        {
          "id":"62b2098e--3c080d8b528d","name":"Q1 Product Group"
        },
        {
          "id":"69c5b1b1--fa16d62a9b61","name":"Q2 Product Group"
        },
        {
          "id":"9507205b--c2d2e72ac843","name":"Q3 Product Group"
        },
        {
          "id":"9507205b--c2d2e72ac843","name":"Q3 Product Group"
        }
      ]
    }

<a name="example"/>
##获取组示例

---

[[Top of article]](#top)

下面的 C# 示例将调用**获取组**操作。
该示例还演示了如何执行以下操作：

- 使用 **JavaScriptSerializer** 类**反序列化** JSON 响应。

**注意** [客户端应用示例](Power-BI-client-app-sample.md)还演示了如何**序列化**和**反序列化** JSON 请求及响应，以及如何调用**组**操作。

###要运行此代码段，你需要执行以下步骤：

[![步骤 1](../Image/Samples-1.png) - 需要有一个 Azure Active Directory 租户。](Create+an+Azure+Active+Directory+tenant.md)

[![步骤 2](../Image/Samples-2.png) - 注册 Power BI。](https://powerbi.microsoft.com)

[![步骤 3](../Image/Samples-3.png) - 注册应用以获取客户端 ID。](Register+a+client+app.md)


        using System;
        using System.Net;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.IO;
        using System.Web.Script.Serialization;
        using System.Linq;
        public group[] GetGroups()
        {
            //This is sample code to illustrate a Power BI operation. 
            //In a production application, refactor code into specific methods and use appropriate exception handling.
            //The client id that Azure AD creates when you register your client app.
            //  To learn how to register a client app, see https://msdn.microsoft.com/en-US/library/dn877542(Azure.100).aspx 
            string clientID = "{client id from Azure AD app registration}";
            //RedirectUri you used when you register your app.
            //For a client app, a redirect uri gives Azure AD more details on the application that it will authenticate.
            // You can use this redirect uri for your client app
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";
            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg/groups";
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;
            //GET web request to list all groups.
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "GET";
            request.ContentLength = 0;
            request.ContentType = "application/json";
            request.Headers.Add("Authorization", String.Format("Bearer {0}", token));
            //Get HttpWebResponse from GET request
            using (HttpWebResponse httpResponse = request.GetResponse() as System.Net.HttpWebResponse)
            {
                //Get StreamReader that holds the response stream
                using (StreamReader reader = new System.IO.StreamReader(httpResponse.GetResponseStream()))
                {
                    string responseContent = reader.ReadToEnd();
                    JavaScriptSerializer json = new JavaScriptSerializer();
                    Groups groups = (Groups)json.Deserialize(responseContent, typeof(Groups));
                    return groups.value;
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
        public class Tables
        {
            public table[] value { get; set; }
        }
        public class table
        {
            public string Name { get; set; }
        }
        public class Groups
        {
            public group[] value { get; set; }
        }
        public class group
        {
            public string Id { get; set; }
            public string Name { get; set; }
        }


