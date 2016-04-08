---
title: Get Groups
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e8e5b65-49cc-4b7b-8494-d46172415608
---
# Get Groups
---
<a name="top"/>
[Request](#request) | [Response](#response) | [Example](#example)  | [Try on Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [Client and web app samples](Power-BI-Samples.md)

The **Get Groups** operation returns a JSON list of all **Groups** that the signed in user is a member of. 

**Required scope**: Dataset.ReadWrite.All or Dataset.Read.All

To set the permissions scope, see [Register a client app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-client-app) or [Register a web app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-register-a-web-app).
<a name="request"/>
## Request
GET https://api.powerbi.com/v1.0/myorg/groups

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

**Groups** have an array of group names. Each **Group** had an **id** and **name**. 

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

### Body Example 
JSON for a **Group** containing Q1 to Q4 products.

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
## Get Groups Example
---
[[Top of article]](#top)

The following C# example calls the **Get Groups** operation. The example also shows you how to:

- **Deserialize** the JSON response using the **JavaScriptSerializer** class.

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
