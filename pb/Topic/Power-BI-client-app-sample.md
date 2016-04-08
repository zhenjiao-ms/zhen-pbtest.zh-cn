---
title: Power BI client app sample
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e52924d-695b-4762-860e-9f8bb1e8b344
---
# Power BI client app sample
---
[Download the .NET getting started client app sample](http://go.microsoft.com/fwlink/?LinkId=619280) 

**Important** To run the client app sample, you must register the sample app in **Azure Active Directory** (Azure AD), and get a **Client ID**. The **Client ID** is used to authenticate to **Azure AD**. See [How to run the client app sample](#run).

The Power BI client app sample shows you how to:

- Get an Azure Active Directory access token for authentication
- Create a dataset
- Get all datasets
- Add rows to a dataset and delete rows from a database
- Update a table schema
- Get tables
- Get groups

**Note** The client app sample also shows you how to call Power BI REST operations on [Groups](Get-Groups.md).

**Details about the client app sample are in the [Authenticate a client app](Authenticate-a-client-app.md) topic or [view the code on GitHub](http://go.microsoft.com/fwlink/?LinkId=619280)**

<a name="run"/>
## How to run the client app sample
The client app sample is a console application which shows you how to authenticate a Power BI client app to Azure Active Directory, and call all Power BI REST operations including Group operations. To run the sample you need to:

1. [Download the .NET getting started client app sample from GitHub](http://go.microsoft.com/fwlink/?LinkId=619280)
2. [Register the client app sample in Azure Active Directory](#register)
3. [Set Client ID you got from registering the sample app](#set)

<a name="register"/>
### Register the client app sample in Azure Active Directory
To call a Power BI REST operation in a client app, you need a **Client ID** to authenticate to ** Azure AD ** so that your app has access to the Power BI REST resources. To get a **Client ID**, you register the sample app in the **Windows Azure Management Portal**. Before you register the sample app, here are the settings you will need:

#### Client app sample registration settings
|Setting | Value | Description
|-|-|-|
|Name | PowerBIClientSample | This can be any unique name.|
|Redirect Uri| https://login.live.com/oauth20_desktop.srf | For a client app, a redirect uri gives Azure Active Directory more details on the application that it will authenticate. You can use this redirect uri for your client app.|
| Permissions| View all Datasets, Read and Write all Datasets, and View users Groups| For more about Power BI permissions, see [Power BI permissions](Power-BI-permissions.md).|

#### Register the client app sample

To register the client app sample, see [Register a client app](Register-a-client-app.md), and use the settings above. After you register the client app sample, **Azure AD** generates the **Client ID** for the app. To run the sample, you must set the **clientID** variable in the sample. You can get the **Client ID** from the **Azure Application Configuration** page. See [How to get a client app id](Register-a-client-app.md#clientID).
<a name="set"/>
### Set Client ID you got from registering the sample app

To run the sample, you must set the **clientID** variable in the sample. In Program.cs, set the  **clientID** variable as follows:
	
	private static string clientID = "{client id from Azure AD app registration}";

You can now run the client app sample.

## Related topics
- [Register a client app](Register-a-client-app.md)
- [Authenticate a client app](Authenticate-a-client-app.md)
- [Get started creating a Power BI app](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API reference](Power-BI-REST-API-reference.md)
