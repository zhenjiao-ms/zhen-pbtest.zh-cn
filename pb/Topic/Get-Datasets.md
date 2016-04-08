---
title: Get Datasets
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26aa56b1-d57b-4556-b5d3-25f034d93a65
---
# Get Datasets
---
<a name="top"/>
[Request](#request) | [Response](#response) | [Example](#example)  | [Try on Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/list-all-datasets) | [Client and web app samples](Power-BI-Samples.md)

The **Get Datasets** operation returns a JSON list of all **Dataset** objects which includes a name and id or a JSON list from a **Group**.

**Required scope**: Dataset.ReadWrite.All or Dataset.Read.All

To set the permissions scope, see [Register a client app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app) or [Register a web app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-web-app).
<a name="request"/>
## Request
GET https://api.powerbi.com/v1.0/myorg/datasets

### Groups

Groups are a collection of unified **Azure Active Directory** groups that the user is a member of and is available in the Power BI service. To learn how to create a group, see [Create a group](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets

### Uri parameter

|Name|Description|Data Type|
|-|-|-|
|group_id|Guid of the **Group** to use. You can get the group id from the [Get Groups](Get-Groups.md) operation.|String|

### Header
Authorization: Bearer eyJ0eX ... FWSXfwtQ	
<a name="response"/>
## Response

### Status code

|**Code**|**Description**
|---|---
|200|OK. Indicates success. The dataset is returned in the response body.


### Content-Type
application/json

### Body schema
**Datasets** have a GUID **id** and **name**.
    
    {
      "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets","value":
      [
        {
          "id":"{dataset_id GUID}",
          "name":"{dataset_name}"
        }
      ]
    }

### Body Example
The following is an example of a SalesMarketing **Dataset** JSON response.

    {
      "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets","value":
      [
        {
          "id":"7c0b090e--172874c749e0",
          "name":"SalesMarketing"
        }
      ]
    }


<a name="example"/>
## Get Datasets Example
---
[[Top of article]](#top)

The following C# example calls the **Get Datasets** operation. The example shows how to **Deserialize** the JSON response using the **JavaScriptSerializer** class.

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
        
        public dataset[] GetDatasets()
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

            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg/datasets";

            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.

            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;

            //GET web request to list all datasets.
            //To get a datasets in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets
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
