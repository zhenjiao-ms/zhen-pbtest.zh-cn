---
title: Client app authentication sample
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e14cde2-b7d0-48db-ae1e-6497c834afe1
robots: noindex,nofollow
---
# Client app authentication sample
[Download the .NET Getting Started Sample](https://github.com/PowerBI/getting-started-for-dotnet/tree/master/PBIGettingStarted)

The Power BI getting started sample shows you how to:
-	Register a Native Client Application
-	Get an Azure Active Directory access token
-	Get all datasets
-	Create a dataset
-	Add rows to a dataset
-	How to add Active Directory Authentication Library

If this is your first time creating a Power BI app, see [Getting started creating Power BI apps](Getting-started-creating-Power-BI-apps.md).

Before you run the sample code, register the sample client app in your Azure Active Directory. Power BI apps are integrated with Azure Active Directory (AD) to provide secure sign in and authorization for your app. To integrate a Power BI app with Azure AD, you register the details about your application with Azure AD by using the Azure Management Portal.

To register an app in Azure Active Directory, see [Register a client app](Register-a-client-app.md).

##Register a Native Client Application
When you register a native client application, such as a console app, you receive a Client ID. The Client ID is used by the application to identify themselves to the users that they are requesting permissions from. For a .NET application, you use the Client ID to get an authentication token. For more information about Power BI Authentication, see [Authenticate with Power BI](http://go.microsoft.com/fwlink/?LinkId=519359).

To register an app in Azure Active Directory, see [Register a client app](Register-a-client-app.md).

###How to get a client app id
1.	In the **Microsoft Azure portal**, choose **ACTIVE DIRECTORY** in the left pane or **Directory** in the **All Items** list. If you do not have an organizational Azure Active Directory, see [Setup Azure Active Directory](https://msdn.microsoft.com/en-US/library/mt203553.aspx). 
2.	In the **Azure Active Directory** page, choose **APPLICATIONS**.
3.	Choose your application.
4.	In the **CONFIGURE** page, copy the **CLIENT ID**.

###To run the sample
1.	Replace **clientID** with your client app ID. To learn how to get a client app ID, see [How to get a client app id](https://msdn.microsoft.com/en-US/library/dn889824.aspx).

    ```
	private static string clientID = "";
   ```

###How to get an Azure Active Directory access token
1.	Add the "Active Directory Authentication Library" NuGet package to your project. To learn how to add Active Directory Authentication Library, see [How to add Active Directory Authentication Library](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.	Add a reference to Microsoft.IdentityModel.Clients.ActiveDirectory.
3.	Create a new **AuthenticationContext** passing an Authority. An authority knows about the service you want to invoke, and knows how to authenticate you.
4.	Get an **Azure Active Directory** token by calling **AcquireToken**.
//Client app ID. To learn how to get a client app ID, see [How to register an app](Register-an-app.md).

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

###How to get all datasets
1.	Get an access token. See [How to get an Azure Active Directory access token](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.	Create an **HttpWebRequest** using a GET method. The sample uses DatsetRequest(datasetsUri, "GET", AccessToken) to make a request to the service. See [How to make a Power BI request](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
3.	Get a response from the Power BI service.

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

###How to create a dataset
1.	Get an access token. See [How to get an Azure Active Directory access token](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.	Create an **HttpWebRequest** using a POST method. The sample uses DatsetRequest(datasetsUri, "POST", AccessToken) to make a request to the service. See [How to make a Power BI request](https://msdn.microsoft.com/en-US/library/dn889824.aspx). 
3.	Get a response from the Power BI service.

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
1.	Get an access token. See How to get an [How to get an Azure Active Directory access token](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.	Create an **HttpWebRequest** using a POST method. The sample uses DatsetRequest(datasetsUri, "POST", AccessToken) to make a request to the service. See [How to make a Power BI request](https://msdn.microsoft.com/en-US/library/dn889824.aspx). 
3.	To identify the dataset to add rows to, the resource uri for a POST method has a dataset id.
4.	Get a response from the Power BI service.

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
1.	Create an **HttpWebRequest** from a dataset resource url. 
2.	Set **Method** to GET or POST. GET returns JSON objects, such as a dataset, from the service. POST is similar to saving data, it POST's JSON objects to the service.
3.	Add an Authorization header to an **HttpWebRequest** object.

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

##How to add Active Directory Authentication Library

You can install the **Active Directory Authentication Library** NuGet package from Visual Studio. When you install a NuGet package, Visual Studio creates a reference to the required assemblies.
1.	Right click a solution.
2.	Choose **Manage NuGet Packages**.
3.	Search for **Active Directory Authentication Library**.
4.	Choose **Active Directory Authentication Library** in the list of packages, and click **Install**.

