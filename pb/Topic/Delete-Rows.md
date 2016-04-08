---
title: 删除行
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0775aad-c351-4339-bdd4-b610011d6a55
---
# 删除行
---

<a name="top"/>
[请求](#request) | [响应](#response) | [示例](#example) | [试用 Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [客户端和 Web 应用示例](Power-BI-Samples.md)

**删除行**操作将从**数据集**中的**表**或**组**中的**数据集**删除**行**。

**所需的权限范围**：Dataset.ReadWrite.All

若要设置权限作用域，请参阅[注册客户端应用](https://msdn.microsoft.com/en-US/library/dn877542.aspx)或[注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)。
<a name="request"/>
##请求

DELETE https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables/{table_name}/rows

###组

组是用户所属的且在 Power BI 服务中可用的统一 **Azure Active Directory** 组的集合。
若要了解如何创建一个组，请参阅[创建组](https://support.powerbi.com/knowledgebase/articles/654250)。

DELETE https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables/{table_name}/rows

###URI 参数

| 名称| 描述| 数据类型|
|-|-|-|
| group_id| 要使用的**组**的 Guid。你可以使用[获取组](Get-Groups.md)操作获取组 ID。| 字符串|
| dataset_id| 要使用的**数据集**的 Guid。你可以使用[获取数据集](Get-Datasets.md)操作获取数据集 ID。| 字符串|
| table_name| **数据集**中**表**的名称。| 字符串|
###DELETE 示例

https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables/Product/Rows
<a name="response"/>
##响应

###StatusCode

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


###正文

无

<a name="example"/>
##删除行示例

---

[[Top of article]](#top)

下面的 C# 示例将调用**删除行**操作。
该示例假定你拥有一个带有**产品**表的数据集。
若要创建数据集，请参阅[创建数据集](Create-Dataset.md)操作。
该示例还演示了如何执行以下操作：

- 使用 LINQ 查询获取数据集 ID。
    若要获取数据集 ID，请参阅[获取数据集](Get-Datasets.md)。

**注意** [客户端应用示例](Power-BI-client-app-sample.md)还演示了如何**序列化**和**反序列化** JSON 请求及响应，以及如何调用**组**操作。

###要运行此代码段，你需要执行以下步骤：

[![步骤 1](../Image/Samples-1.png) - 需要有一个 Azure Active Directory 租户。](Create+an+Azure+Active+Directory+tenant.md)

[![步骤 2](../Image/Samples-2.png) - 注册 Power BI。](https://powerbi.microsoft.com)

[![步骤 3](../Image/Samples-3.png) - 注册应用以获取客户端 ID。](Register+a+client+app.md)

![步骤 4](../Image/Samples-4.png) - 需要一个**数据集** ID。
若要获取数据集 ID，请参阅[获取数据集](Get-Datasets.md)操作。


        using System;
        using System.Net;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.IO;
        using System.Web.Script.Serialization;
        using System.Linq;
        public string DeleteRows()
        {
            //This is sample code to illustrate a Power BI operation. 
            //In a production application, refactor code into specific methods and use appropriate exception handling.
            //The client id that Azure AD creates when you register your client app.
            //  To learn how to register a client app, see https://msdn.microsoft.com/en-US/library/dn877542(Azure.100).aspx  
            string clientID = "{client id from Azure AD app registration}";
            //Assuming you have a dataset named SalesMarketing
            // To get a dataset id, see Get Datasets operation.           
            dataset[] datasets = GetDatasets();
            string datasetId = (from d in datasets where d.Name == "SalesMarketing" select d).FirstOrDefault().Id;
            // Assumes the Dataset named SalesMarketing has a Table named Product
            string tableName = "Product";
            //RedirectUri you used when you register your app.
            //For a client app, a redirect uri gives Azure AD more details on the application that it will authenticate.
            // You can use this redirect uri for your client app
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";
            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            string powerBIApiUrl = String.Format("https://api.powerbi.com/v1.0/myorg/datasets/{0}/tables/{1}/Rows", datasetId, tableName);
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;
            //DELETE web request to delete rows.
            //To delete rows in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables/{table_name}/rows
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "DELETE";
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
                    return responseContent;
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


