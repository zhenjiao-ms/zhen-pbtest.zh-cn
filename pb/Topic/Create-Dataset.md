---
title: 创建数据集
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7567da39-9e24-4da0-b078-146775f736be
---
# 创建数据集
---

[请求](#request) | [响应](#response) | [示例](#example) | [试用 Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [客户端和 Web 应用示例](Power-BI-Samples.md)
<a name="top"/>

**创建数据集** 操作将从 JSON 架构定义创建一个新**数据集**，并返回所创建的数据集的 **Dataset** ID 和属性。
**数据集**可在**组**中进行创建。

**所需作用域**：Dataset.ReadWrite.All


若要设置权限作用域，请参阅[注册客户端应用](https://msdn.microsoft.com/en-US/library/dn877542.aspx)或[注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)。
<a name="request"/>
##请求

POST https://api.powerbi.com/v1.0/myorg/datasets

**启用默认的保留策略**

POST https://api.powerbi.com/v1.0/myorg/datasets?defaultRetentionPolicy={None | basicFIFO}

###查询字符串参数

| 名称| 描述| 值|
|-|-|-|
| defaultRetentionPolicy| 启用默认保留策略，以便在新数据不断进入仪表板时自动清除旧数据。若要了解有关自动保留策略的详细信息，请参阅[实时数据的自动保留策略](Automatic-retention-policy-for-real-time-data.md)。| 无或 basicFIFO|

###组

组是用户所属的且在 Power BI 服务中可用的统一 **Azure Active Directory** 组的集合。
若要了解如何创建一个组，请参阅[创建组](https://support.powerbi.com/knowledgebase/articles/654250)。

POST https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets

###URI 参数

| 名称| 描述| 数据类型|
|-|-|-|
| group_id| 要使用的**组**的 Guid。你可以使用[获取组](Get-Groups.md)操作获取组 ID。| 字符串|

###标头

Content-Type: application/json
Authorization: Bearer eyJ0eX ...FWSXfwtQ

###正文架构

**Dataset** 具有一个**名称**和一个 **Table** 对象的集合（表）。
**Table** 具有一个**名称**和一个 **Column** 对象的集合（列）。
**Column** 具有一个**名称**和**数据类型**。

有关 Power BI REST 数据类型的列表，请参阅[支持的数据类型](Supported-data-types.md)。

    {"name": "dataset_name", "tables": 
        [{"name": "", "columns": 
            [{ "name": "column_name1", "dataType": "data_type"},
             { "name": "column_name2", "dataType": "data_type"},
             { ... }
            ]
          }
        ]
    }

###正文示例

带有产品**表** 架构（表）的市场营销**数据集**。
产品**表**具有描述产品的**列**。

    {"name": "SalesMarketing","tables": 
        [{"name": "Product", "columns": 
            [{ "name": "ProductID", "dataType": "Int64"},
             { "name": "Name", "dataType": "string"},
             { "name": "Category", "dataType": "string"},
             { "name": "IsCompete", "dataType": "bool"},
             { "name": "ManufacturedOn", "dataType": "DateTime"}
            ]
          }
        ]
    }

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
    <td>201</td>
    <td>Created.表示请求已完成且新数据集已创建。</td>
  </tr>
</table>

###Content-Type

application/json


###正文架构

一个**创建数据集**响应具有以下架构：

    {          
        "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets/$entity",       
        "id":"{dataset_id}",
        "name":"{name}",
        "defaultRetentionPolicy":"{None | basicFIFO}"
    }

###正文示例

下面是其 **defaultRetentionPolicy** 已设置为 **basicFIFO** 的市场营销**数据集** JSON 响应的一个示例。
**保留策略**会在新数据不断进入仪表板时自动清除旧数据。

    {
        "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets/$entity",
        "id":"7c0b090e--b51-172874c749e0",
        "name":"SalesMarketing",
        "defaultRetentionPolicy":"basicFIFO"
    }

<a name="example"/>
##创建数据集示例

---

[[Top of article]](#top)

下面的 C# 示例将调用**创建数据集**操作。

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
        static string CreateDataset()
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
            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg";
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientID, new Uri(redirectUri)).AccessToken;
            //POST web request to create a dataset.
            //To create a Dataset in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets
            HttpWebRequest request = System.Net.WebRequest.Create(string.Format("{0}/datasets", powerBIApiUrl)) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "POST";
            request.ContentLength = 0;
            request.ContentType = "application/json";
            request.Headers.Add("Authorization", String.Format("Bearer {0}", token));
            //Create dataset JSON for POST request
            string json = "{\"name\": \"SalesMarketing\", \"tables\": " +
                "[{\"name\": \"Product\", \"columns\": " +
                "[{ \"name\": \"ProductID\", \"dataType\": \"Int64\"}, " +
                "{ \"name\": \"Name\", \"dataType\": \"string\"}, " +
                "{ \"name\": \"Category\", \"dataType\": \"string\"}," +
                "{ \"name\": \"IsCompete\", \"dataType\": \"bool\"}," +
                "{ \"name\": \"ManufacturedOn\", \"dataType\": \"DateTime\"}" +
                "]}]}";
            //POST web request
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(json);
            request.ContentLength = byteArray.Length;
            //Write JSON byte[] into a Stream
            using (Stream writer = request.GetRequestStream())
            {
                writer.Write(byteArray, 0, byteArray.Length);
                var response = (HttpWebResponse)request.GetResponse();
                return response.StatusCode.ToString();
            }
        }


