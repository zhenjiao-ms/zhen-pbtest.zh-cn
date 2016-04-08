---
title: Update Table Schema
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3615c1bc-9bfe-46fb-b30e-c76c015473e0
---
# Update Table Schema
---
<a name="top"/>
[Request](#request) | [Response](#response) | [Example](#example)  | [Try on Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [Client and web app samples](Power-BI-Samples.md)

The **Update Table Schema** operation updates a **Table** schema. A **Table** schema can be updated in a **Group**.

**Required scope**: Dataset.ReadWrite.All

To set the permissions scope, see [Register a client app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app) or [Register a web app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-web-app).

<a name="request"/>
## Request
PUT https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables/{table_name}

### Groups

Groups are a collection of unified **Azure Active Directory** groups that the user is a member of and is available in the Power BI service. To learn how to create a group, see [Create a group](https://support.powerbi.com/knowledgebase/articles/654250).

PUT https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables/{table_name}

### Uri parameters

|Name|Description|Data Type|
|-|-|-|
|group_id|Guid of the **Group** to use. You can get the group id from the [Get Groups](Get-Groups.md) operation.|String|
|dataset_id|Guid of the **Dataset** to use. You can get the dataset id from the [Get Datasets](Get-Datasets.md) operation.|String|
|table_name|Name of **Table** in the **Dataset**.|String|


### PUT example
https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables/Product
### Header
Content-Type: application/json
Authorization: Bearer eyJ0eX ... FWSXfwtQ
### Body schema
A **Table** schema has a collection of **Columns**. A **Column** has a **name** and **data type**.

	{
	  "name": "table_name",
	  "columns": [
	    {
	      "name": "column_name1",
	      "dataType": "data_type"
	    },
	    {
	      "name": " column_name2",
	      "dataType": "data_type"
	    },
	    {
	    ...
	    }
	  ]
	}  

### Body example
A product **Table** with **Columns** that describe a product.

	{
	  "name": "Product",
	  "columns": [
	    {
	      "name": "ProductID",
	      "dataType": "Int64"
	    },
	    {
	      "name": "Name",
	      "dataType": "string"
	    },
	    {
	      "name": "Category",
	      "dataType": "string"
	    },
	    {
	      "name": "IsCompete",
	      "dataType": "bool"
	    },
	    {
	      "name": "ManufacturedOn",
	      "dataType": "DateTime"
	    },
	    {
	      "name": "NewColumn",
	      "dataType": "string"
	    }
	  ]
	}  

<a name="response"/>
## Response
### Status code

|**Code**|**Description**
|---|---
|201|Created. The request was fulfilled and a new Dataset was created.


The request has been fulfilled and resulted in a new resource being created.</td></tr></table>

### Content-Type
application/json 

### Body schema
Returns entity **name** that was updated.
    
    {
      "@odata.context":"http://api.powerbi.com/v1.0/myorg/$metadata#tables/$entity",
      "name":"{table_name}"
    }


### Body Example
The following is an example of a Product entity that was updated.

    {
      "@odata.context":"http://api.powerbi.com/v1.0/myorg/$metadata#tables/$entity",
      "name":"Product"
    }

<a name="example"/>
## Update Table Schema Example
---
[[Top of article]](#top)

The following C# example calls the **Update Table Schema** operation. The example assumes you have a Dataset named SalesMarketing with a Product table. To create a dataset, see [Create Dataset](Create-Dataset.md) operation. The example also shows how to:

- Get a Dataset id using a LINQ query. To get a dataset id, see [Get Datasets](Get-Datasets.md).

**Note** The [Client app sample](Power-BI-client-app-sample.md) also shows you how to **Serialize** and **Deserialize** a JSON request and response as well as call **Group** operations.

### To run this code snippet, you need to:

[![s1](../Image/Samples-1.png) - Have an Azure Active Directory tenant.](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)

[![s2](../Image/Samples-2.png) - Sign up for Power BI.](https://powerbi.microsoft.com)

[![s3](../Image/Samples-3.png) - Register your app to get a client id.](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app/)

![s4](../Image/Samples-4.png) - You will need a **Dataset** id. To get a dataset id, see [Get Datasets](Get-Datasets.md) operation. 

        using System;
        using System.Net;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.IO;
        using System.Web.Script.Serialization;
        using System.Linq;
    
        public void UpdateTableSchema()
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

            string powerBIApiUrl = String.Format("https://api.powerbi.com/v1.0/myorg/datasets/{0}/tables/{1}", datasetId, tableName);

            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.

            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;

            //PUT web request to update table schema.
            //To update table schema in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables/{table_name}
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "PUT";
            request.ContentLength = 0;
            request.ContentType = "application/json";
            request.Headers.Add("Authorization", String.Format("Bearer {0}", token));

            //Add a column to Product JSON content
            string json = "{\"name\": \"Product\", \"columns\": " +
                "[{ \"name\": \"ProductID\", \"dataType\": \"Int64\"}, " +
                "{ \"name\": \"Name\", \"dataType\": \"string\"}, " +
                "{ \"name\": \"Category\", \"dataType\": \"string\"}," +
                "{ \"name\": \"IsCompete\", \"dataType\": \"bool\"}," +
                "{ \"name\": \"ManufacturedOn\", \"dataType\": \"DateTime\"}," +
                "{ \"name\": \"NewColumn\", \"dataType\": \"string\"}" +
                "]}";

            //POST web request
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(json);
            request.ContentLength = byteArray.Length;

            //Write JSON byte[] into a Stream
            using (Stream writer = request.GetRequestStream())
            {
                writer.Write(byteArray, 0, byteArray.Length);
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
