---
title: Power BI web app sample
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8402b48-3f71-4c42-996d-7a5310dbb480
---
# Power BI web app sample
---
[Download the web app sample](http://go.microsoft.com/fwlink/?LinkId=619279)

**Important** To run the web app sample, you must register the web app sample in **Azure Active Directory** (Azure AD), and get a **Client ID** and a **Client Secret Key**. The **Client ID** and **Client Secret Key** are used to authenticate to ** Azure AD **. See [How to run the web app sample](#run).
<a name="run"/>
## How to run the web app sample
The web app sample shows you how to authenticate a Power BI web app to **Azure Active Directory** and make a Power BI REST call to get all datasets. 

**Details about the web app sample are in the [Authenticate a web app](Authenticate-a-web-app.md) article or [view the code on GitHub](http://go.microsoft.com/fwlink/?LinkId=619279)**.

To run the sample you need to:

1. [Download the web app sample](http://go.microsoft.com/fwlink/?LinkId=619279)
2. [Register the web app sample in Azure Active Directory](#register)
3. [Set Client ID and Client Secret Key you got from registering the sample app](#set)
4. [Set Redirect Url to the Redirect Url for your web app](#redirect)

<a name="register"/>
### Register the web app sample in Azure Active Directory
To call a Power BI REST operation in a web app, you need a **Client ID** and **Client Secret Key** to authenticate to ** Azure AD ** so that your app has access to the Power BI REST resources. To get a **Client ID** and **Client Secret Key**, you register the sample app in the **Windows Azure Management Portal**. Before you register the sample app, here are the settings you will need:

#### Web app sample registration settings
|Setting | Value | Description
|-|-|-|
|Name | PowerBIWebSample | This can be any unique name.|
|Sign-on Url|http://localhost:13526|This is your web app url.|
|App ID Uri|https://{your tenant id}.onmicrosoft.com/PowerBIWebSample | This is your Azure Tenant URL followed by your app name. For example, https://yourtenant.onmicrosoft.com/YourWebApp |
| Permissions| View all Datasets| Since the web app sample only gets datasets, you just need **View all Datasets** permissions. For more about Power BI permissions, see [Power BI permissions](Power-BI-permissions.md).|

#### Register the web app sample
To register the web app sample, see [Register a web app](Register-a-web-app.md), and use the settings above. After you register the web app sample, **Azure AD** generates the **Client ID** for the app. For the **Client Secret Key**, you will need to set the **Duration**. After you save the app in **Azure Management Portal**, Azure AD generates a **Client Secret Key**. Make sure you copy the key; otherwise, the key will not be available upon future navigation to the app configuration page.

To run the sample, you must set the **ClientID** and **ClientSecret** properties in **web.config**. You can get the **Client ID** from the **Azure Application Configuration** page. See [How to get a web app id](Register-a-web-app.md#clientID). For the **Client Secret Key**, make sure you copy the key; otherwise, the key will not be available upon future navigation to the configuration page.
<a name="set"/>
### Set Client ID and Client Secret Key you got from registering the sample app

To run the web app sample, you must set the **ClientID** and **ClientSecret** properties in **web.config** as follows:
	
	  <applicationSettings>
	    <PBIWebApp.Properties.Settings>
	      <setting name="ClientID" serializeAs="String">
	        <value>{Client ID from Azure AD app registration}</value>
	      </setting>
	      <setting name="ClientSecretKey" serializeAs="String">
	        <value>{Client Secret Key from Azure AD app registration}</value>      
	  </setting>
	    </PBIWebApp.Properties.Settings>
	  </applicationSettings>

<a name="redirect"/>
### Set Redirect Url to the Redirect Url for your web app
The sample is configured to run on localhost. However, if you develop a web app, make sure you change the **RedirectUrl** in web.config to your web app url. For example, http://{web_app_name}.azurewebsites.net/Redirect.
	
	      <setting name="RedirectUrl" serializeAs="String">
	        <value>http://{web_app_url}/Redirect</value>
	      </setting>

<br/>
You can now run the web app sample.

## Related topics
- [Register a web app](Register-a-web-app.md)
- [Authenticate a web app](Authenticate-a-web-app.md)
- [Get started creating a Power BI app](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API reference](Power-BI-REST-API-reference.md)
