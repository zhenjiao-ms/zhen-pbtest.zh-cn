---
title: Create Dataset
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7567da39-9e24-4da0-b078-146775f736be
---
# Create Dataset
---
[Request](#request) | [Response](#response) | [Example](#example)  | [Try on Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [Client and web app samples](Power-BI-Samples.md)
<a name="top"/>

The **Create Dataset** operation creates a new **Dataset** from a JSON schema definition and returns the **Dataset** ID and the properties of the dataset created. A **Dataset** can be created in a **Group**.

**Required scope**: Dataset.ReadWrite.All 

To set the permissions scope, see [Register a client app](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app/) or [Register a web app](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-web-app/).
<a name="request"/>
## Request

POST https://api.powerbi.com/v1.0/myorg/datasets

**Enable a default retention policy**

POST https://api.powerbi.com/v1.0/myorg/datasets?defaultRetentionPolicy={None | basicFIFO}

### Query string parameter

|Name|Description|Values|
|-|-|-|
|defaultRetentionPolicy|Enables a default retention policy to automatically clean up old data while keeping a constant flow of new data going into your dashboard. To learn more about automatic retention policy, see [Automatic retention policy for real-time data](Automatic-retention-policy-for-real-time-data.md).|None or basicFIFO|


### Groups

Groups are a collection of unified **Azure Active Directory** groups that the user is a member of and is available in the Power BI service. To learn how to create a group, see [Create a group](https://support.powerbi.com/knowledgebase/articles/654250).

POST https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|group_id|Guid of the **Group** to use. You can get the group id from the [Get Groups](Get-Groups.md) operation.|String|


### Header
Content-Type: application/json
Authorization: Bearer eyJ0eX ... FWSXfwtQ

### Body schema
A **Dataset** has a **name** and collection of **Table** objects (tables). A **Table** has a **name** and collection of **Column** objects (columns). A **Column** has a **name** and **data type**.

For a list of Power BI REST data types, see [Supported data types](Supported-data-types.md).

	{"name": "dataset_name", "tables": 
	    [{"name": "", "columns": 
	        [{ "name": "column_name1", "dataType": "data_type"},
	         { "name": "column_name2", "dataType": "data_type"},
	         { ... }
	        ]
	      }
	    ]
	}
### Body example
A SalesMarketing **Dataset** with a Product **Table** schema (tables). The product **Table** has **columns** that describe a product.

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
## Response

### Status code

|**Code**|**Description**
|---|---
|201|Created. The request was fulfilled and a new Dataset was created.


### Content-Type
application/json 

### Body schema
A **Create Dataset** response has the following schema:
  
    {          
        "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets/$entity",       
        "id":"{dataset_id}",
        "name":"{name}",
        "defaultRetentionPolicy":"{None | basicFIFO}"
    }

### Body example
The following is an example of a SalesMarketing **Dataset** JSON response with **defaultRetentionPolicy** set to **basicFIFO**. The **Retention Policy** automatically cleans up old data while keeping a constant flow of new data going into your dashboard.
     
    {
        "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets/$entity",
        "id":"7c0b090e--b51-172874c749e0",
        "name":"SalesMarketing",
        "defaultRetentionPolicy":"basicFIFO"
    }

<a name="example"/>
## Create Dataset Example
---
[[Top of article]](#top)

The following C# example calls the **Create Dataset** operation.

  **Note** The [Client app sample](Power-BI-client-app-sample.md) also shows you how to **Serialize** and **Deserialize** a JSON request and response as well as call **Group** operations.

### To run this code snippet, you need to:

[![s1](../Image/Samples-1.png) - Have an Azure Active Directory tenant.](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)

[![s2](../Image/Samples-2.png) - Sign up for Power BI.](https://powerbi.microsoft.com)

[![s3](../Image/Samples-3.png) - Register your app to get a client id.](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app/)
   
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
