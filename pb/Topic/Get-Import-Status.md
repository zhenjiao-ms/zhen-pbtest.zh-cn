---
title: Get Import Status
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ea3637c-f2db-499d-839e-cefcfea503dd
---
# Get Import Status
---
[Request](#request) | [Response](#response) | [Try on Apiary](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset)
<a name="top"/>
            
The **Get Import Status** operation returns a JSON list of all **Import Status** properties that includes  . 

**Required scope**: Dataset.ReadWrite.All or Dataset.Read.All

To set the permissions scope, see [Register a client app](https://msdn.microsoft.com/en-US/library/dn877542.aspx) or [Register a web app](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Request
GET https://api.powerbi.com/beta/myorg/imports/{import_id}

###Groups

Groups are a collection of unified **Azure Active Directory** groups that the user is a member of and is available in the Power BI service. To learn how to create a group, see [Create a group](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.PowerBI.com/v1.0/myorg/groups/{group_id}/datasets

###Uri parameter

<table><tr><td><b>Name</b></td><td><b>Description</b></td><td><b>Data Type</b></td></tr><tr><td>group_id</td><td>Guid of the <b>Group</b> to use. You can get the group id from the [Get Groups](Get-Groups.md) operation.</td><td>String</td></tr></table>

###Header
Content-Type: application/json
Authorization: Bearer eyJ0eX ... FWSXfwtQ	
<a name="response"/> 
##Response
###Status code

###Status code

<table><tr><td><b>Code</b></td><td><b>Description</b></td></tr><tr><td>200</td><td>OK. Indicates success. The Dataset is returned in the response body.</td></tr></table>

###Content-Type
application/json 

###Body schema 
{Description}
