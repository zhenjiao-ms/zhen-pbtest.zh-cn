---
title: 客户端应用身份验证示例
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e14cde2-b7d0-48db-ae1e-6497c834afe1
robots: noindex,nofollow
---
# 客户端应用身份验证示例
              [下载 .NET 入门示例](https://github.com/PowerBI/getting-started-for-dotnet/tree/master/PBIGettingStarted)

Power BI 入门示例向你演示了如何执行以下操作：

*   注册本机客户端应用程序
*   获取 Azure Active Directory 访问令牌
*   获取所有数据集
*   创建数据集
*   将行添加到数据集
*   如何添加 Active Directory 身份验证库

如果你是首次创建 Power BI 应用，请参阅[开始创建 Power BI 应用](Getting-started-creating-Power-BI-apps.md)。

在运行示例代码之前，请在你的 Azure Active Directory 中注册示例客户端应用。
Power BI 应用与 Azure Active Directory (AD) 集成在一起，以为你的应用提供安全的登录和授权。
若要将 Power BI 应用与 Azure AD 集成，请使用 Azure 管理门户向 Azure AD 注册有关应用程序的详细信息。

若要在 Azure Active Directory 中注册应用，请参阅[注册客户端应用](Register-a-client-app.md)。

##注册本机客户端应用程序

在注册本机客户端应用程序（例如控制台应用）时，你会收到一个客户端 ID。
应用程序使用该客户端 ID 向用户（应用程序向其请求权限）标识自身。
对于 .NET 应用程序，你可以使用客户端 ID 获取身份验证令牌。
有关 Power BI 身份验证的详细信息，请参阅[对 Power BI 进行身份验证](http://go.microsoft.com/fwlink/?LinkId=519359)。

若要在 Azure Active Directory 中注册应用，请参阅[注册客户端应用](Register-a-client-app.md)。

###如何获取客户端应用 ID

1.  在 **Microsoft Azure 门户**中，选择左侧窗格中的“ACTIVE DIRECTORY”****或“所有项”****列表中的“目录”****。
    如果你没有组织结构的 Azure Active Directory，请参阅[安装 Azure Active Directory](https://msdn.microsoft.com/en-US/library/mt203553.aspx)。
2.  在“Azure Active Directory”****页中，选择“应用程序”****。
3.  选择你的应用程序。
4.  在“配置”****页中，复制“客户端 ID”****。

###运行示例

1.  使用客户端应用 ID 替换 **clientID**。
    若要了解如何获取客户端应用 ID，请参阅[如何获取客户端应用 ID](https://msdn.microsoft.com/en-US/library/dn889824.aspx)。
    
    
    ```
    private static string clientID = "";

    ```
    

###如何获取 Azure Active Directory 访问令牌

1.  将“Active Directory 身份验证库”NuGet 程序包添加到你的项目。
    若要了解如何添加 Active Directory 身份验证库，请参阅[如何添加 Active Directory 身份验证库](https://msdn.microsoft.com/en-US/library/dn889824.aspx)。
2.  将引用添加到 Microsoft.IdentityModel.Clients.ActiveDirectory。
3.  创建一个新的用于传递颁发机构的 **AuthenticationContext**。
    颁发机构知道你要调用的服务，以及如何对你进行身份验证。
4.  通过调用 **AcquireToken** 获取 **Azure Active Directory** 令牌。
    //客户端应用 ID。
    若要了解如何获取客户端应用 ID，请参阅[如何注册应用](Register-an-app.md)。


```
     private static string clientID = ""; 

     //Power BI resource uri
     private static string resourceUri = "https://analysis.windows.net/powerbi/api";           
     //OAuth2 authorityUri
     private static string authorityUri = "https://login.windows.net/common/oauth2/authorize";

     //Create a new AuthenticationContext passing an Authority.
     AuthenticationContext authContext = new AuthenticationContext(authority);

     //Get an Azure Active Directory token by calling AcquireToken
     string token = authContext.AcquireToken(resourceUri, clientID, new Uri(redirectUri)).AccessToken.ToString();

```


###如何获取所有数据集

1.  获取访问令牌。
    请参阅[如何获取 Azure Active Directory 访问令牌](https://msdn.microsoft.com/en-US/library/dn889824.aspx)。
2.  使用 GET 方法创建 **HttpWebRequest**。
    此示例使用 DatsetRequest(datasetsUri, "GET", AccessToken) 向服务发出请求。
    请参阅[如何发出 Power BI 请求](https://msdn.microsoft.com/en-US/library/dn889824.aspx)。
3.  获取 Power BI 服务的响应。


```
    private static string datasetsUri = "https://api.powerbi.com/v1.0/myorg/datasets";

    static PBIDatasets GetAllDatasets()
    {
        PBIDatasets PBIDatasets = null;

        //In a production application, use more specific exception handling.
        try
        {
            //Create a GET web request to list all datasets
            HttpWebRequest request = DatasetRequest(datasetsUri, "GET", AccessToken());

            //Get HttpWebResponse from GET request
            string responseContent = GetResponse(request);

            //Deserialize JSON string from response
            PBIDatasets = JsonConvert.DeserializeObject<PBIDatasets>(responseContent);

        }
        catch (Exception ex)
        {
            //In a production application, handle exception
        }

        return PBIDatasets;
    }

```


###如何创建数据集

1.  获取访问令牌。
    请参阅[如何获取 Azure Active Directory 访问令牌](https://msdn.microsoft.com/en-US/library/dn889824.aspx)。
2.  使用 POST 方法创建 **HttpWebRequest**。
    此示例使用 DatsetRequest(datasetsUri, "POST", AccessToken) 向服务发出请求。
    请参阅[如何发出 Power BI 请求](https://msdn.microsoft.com/en-US/library/dn889824.aspx)。
3.  获取 Power BI 服务的响应。


```
    static void CreateDataset()
    {
        //In a production application, use more specific exception handling.           
        try
        {
            //Create a POST web request to list all datasets
            HttpWebRequest request = DatasetRequest(datasetsUri, "POST", AccessToken());

            var datasets = GetAllDatasets().Datasets.GetDataset(datasetName);

            if (datasets.Count() == 0)
            {
                //POST request using the json schema from Product
                Console.WriteLine(PostRequest(request, new Product().ToJsonSchema(datasetName)));
            }
            else
            {
                Console.WriteLine("Dataset exists");
            }
        }
        catch (Exception ex)
        {

            Console.WriteLine(ex.Message);
        }
    }
 ```

###How to add rows to a dataset
1.  Get an access token. See How to get an [How to get an Azure Active Directory access token](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.  Create an **HttpWebRequest** using a POST method. The sample uses DatsetRequest(datasetsUri, "POST", AccessToken) to make a request to the service. See [How to make a Power BI request](https://msdn.microsoft.com/en-US/library/dn889824.aspx). 
3.  To identify the dataset to add rows to, the resource uri for a POST method has a dataset id.
4.  Get a response from the Power BI service.


```

    static void AddRowsFromClass()
    {
        //Get dataset id from a table name
        string datasetId = GetAllDatasets().Datasets.GetDataset(datasetName).First().Id;

        //In a production application, use more specific exception handling. 
        try
        {
            HttpWebRequest request = DatasetRequest(String.Format("{0}/{1}/tables/{2}/rows", datasetsUri, datasetId, tableName), "POST", AccessToken());

            //Create a list of Product
            List<Product> products = new List<Product>
            {
                new Product{ProductID = 1, Name="Adjustable Race", Category="Components", IsCompete = true, ManufacturedOn = new DateTime(2014, 7, 30)},
                new Product{ProductID = 2, Name="LL Crankarm", Category="Components", IsCompete = true, ManufacturedOn = new DateTime(2014, 7, 30)},
                new Product{ProductID = 3, Name="HL Mountain Frame - Silver", Category="Bikes", IsCompete = true, ManufacturedOn = new DateTime(2014, 7, 30)},
            };

            //POST request using the json from a list of Product
            //NOTE: Posting rows to a model that is not created through the Power BI API is not currently supported. 
            //      Please create a dataset by posting it through the API following the instructions on http://dev.powerbi.com.
            Console.WriteLine(PostRequest(request, products.ToJson(JavaScriptConverter<Product>.GetSerializer())));

        }
        catch (Exception ex)
        {

            Console.WriteLine(ex.Message);
        }
    }


 ```

###How to make a Power BI request
To make a GET or POST request on the Power BI service:
1.  Create an **HttpWebRequest** from a dataset resource url. 
2.  Set **Method** to GET or POST. GET returns JSON objects, such as a dataset, from the service. POST is similar to saving data, it POST's JSON objects to the service.
3.  Add an Authorization header to an **HttpWebRequest** object.

```
private static string datasetsUri = "https://api.powerbi.com/v1.0/myorg/datasets";

  private static HttpWebRequest DatsetRequest(string datasetsUri, string method, string authorizationToken)
  {
      HttpWebRequest request = System.Net.WebRequest.Create(datasetsUri) as System.Net.HttpWebRequest;
      request.KeepAlive = true;
      request.Method = method;
      request.ContentLength = 0;
      request.ContentType = "application/json";

      //Add an Authorization header to an HttpWebRequest object
      request.Headers.Add("Authorization", String.Format( "Bearer {0}", authorizationToken));

      return request;
  }
  ```

###How to get a JSON response from the service
To GET a JSON response from an **HttpWebRequest** request, call request.**GetResponse()** and read the response into a **StreamReader**.

```
    private static string GetResponse(HttpWebRequest request)
    {
        string response = string.Empty;

        using (HttpWebResponse httpResponse = request.GetResponse() as System.Net.HttpWebResponse)
        {
            //Get StreamReader that holds the response stream
            using (StreamReader reader = new System.IO.StreamReader(httpResponse.GetResponseStream()))
            {
                response = reader.ReadToEnd();                 
            }
        }

        return response;
    }
```

###How to POST JSON to the service
To POST JSON to the service from an **HttpWebRequest** request and json string, write a json byte[] array into a **Stream**.

```
    private static void PostRequest(HttpWebRequest request, string json)
    {
        byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(json);
        request.ContentLength = byteArray.Length;

        //Write JSON byte[] into a Stream
        using (Stream writer = request.GetRequestStream())
        {
            writer.Write(byteArray, 0, byteArray.Length);
        }
    }

 ```


##如何添加 Active Directory 身份验证库

你可以从 Visual Studio 安装 **Active Directory 身份验证库** NuGet 程序包。
在安装 NuGet 程序包时，Visual Studio 将创建对所需程序集的引用。

1.  右键单击某个解决方案。
2.  选择“管理 NuGet 程序包”****。
3.  搜索 **Active Directory 身份验证库**。
4.  选择程序包列表中的“Active Directory 身份验证库”****，然后单击“安装”****。


