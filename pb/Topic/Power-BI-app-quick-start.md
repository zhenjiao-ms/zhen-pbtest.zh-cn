---
title: Power BI app quick start
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: f6655baa-b01c-4e97-a2f4-63daaec7d50e
robots: noindex,nofollow
---
# Power BI app quick start
**[Please note this is preview content]**

Last updated: 5/4/2015

**Important** Before you create a Power BI app you need

- An Azure Active Directory with at least one organization user
	- If you do not have an Azure Active Directory (AAD), see [Setup Azure Active Directory](#setup).
	- To create an organization user in an Azure AD tenant, see [Add a user to your Azure Active Directory](#newuser).
	 
- A Power BI Service.  [Sign up for Power BI](https://portal.office.com/Start?Sku=a403ebcc-fae0-4ca2-8c8c-7a907fd6c235/). 
	- **Note** You need an ADD organization user to sign up for Power BI.
- Once you have the Power BI Service, you need to register your app. To learn how to register your Power BI app, see [Register an app](Register-an-app.md).
<a name="setup"></a>
## Setup Azure Active Directory ##
Power BI apps are integrated with Azure Active Directory (AD) to provide secure sign in and authorization for your app. To integrate a Power BI app with Azure AD, you register the details about your application with Azure AD by using the Azure Management Portal.
### To setup Azure Active Directory ###
 1. Navigate to https://manage.windowsazure.com and log in with the account that has an Azure subscription.
 2. Click **ACTIVE DIRECTORY** management icon in the left pane.
 3. Click **NEW** button at the bottom of the page.
 4. Choose **APP SERVICES** > **DIRECTORY** > **CUSTOM CREATE**.

    ![New AD](../Image/NewAD.png)

 5. Enter a name and domain name. For country or region choose United States or the country were Power BI is available. 

    ![Add directory](../Image/NewDir.png)

 6. Choose OK icon. An Azure Active Directory is created.
<a name="newuser"></a>

### Add a user to your Azure Active Directory ###
 1. In your Azure Active Directory, click **USERS**.

    ![Add user](../Image/AddADUser.png)

 2. At the bottom of the page, click **ADD USER**. A user account is used to sign up for Power BI.
 
     ![enter image description here](../Image/AddUser2.png)
 
	1. For **TYPE OF USER**, choose **New user in you organization**.
	2. Enter **USER NAME**.
	3. Enter {Azure_AD_name}.onmicrosoft.com. An Azure AD name is the same as a Tenant ID.
	4. Click **Next**.
	
	      ![User Profile](../Image/UserProfile.png)
		   
	5. Enter a **user profile**
	6. click **Next**. For ROLE, you can use User. 
	7. Click **Create** to create a temporary. The new user is assigned a temporary password that must be changed on first sign in.
	8. In the Get temporary password page, click **Complete** icon. A new Azure AD user is created.

**Now you can sign up for Power BI.**
 
<a name="signup"></a>
## Sign up for Power BI ##

 1. [Sign up for Power BI](https://portal.office.com/Start?Sku=a403ebcc-fae0-4ca2-8c8c-7a907fd6c235/).
 2. In the Power BI registration page, enter the user that you created in the [Add a user to your Azure Active Directory](#newuser) steps. The format is {user name}@{Azure_AD_Name}.onmicrosoft.com.

The registration process validates the user and provisions the Power BI service in your Azure AD. After Power BI is provisioned, a Power Bi tenant is available for a user.

**Note**:  A first time Power BI registration can take up to 30 minutes to provision.

##Register your Power BI app
You can now register a Power BI app. To learn how to register an app, see [Register an app](Register-an-app.md).
