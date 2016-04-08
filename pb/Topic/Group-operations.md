---
title: Group operations
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 87c5bab7-de5d-44cd-908a-e187e26abb9a
---
# Group operations
---
A Power BI app can post content to a user’s personal workspace and make the same REST API calls to any of the groups that the user has access to. To post a dataset to a user’s personal workspace you continue to send the POST request to https://api.powerbi.com/v1.0/myorg/datasets. It you want to post that dataset to a group that the user is a member of, you send the POST request to https://api.powerbi.com/v1.0/myorg/groups/{groupId}/datasets. 

In order to get the group ID to put in the URL, you call the **Get Groups** operation.

	GET https://api.powerbi.com/v1.0/myorg/groups

The Power BI REST API has the following **Group** operations:

- [Get Groups](Get-Groups.md): Returns a JSON list of all **Group** objects that includes.

### See also
- [Get started creating a Power BI app](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API reference](Power-BI-REST-API-reference.md)
- [Overview of Power BI REST API](Overview-of-Power-BI-REST-API.md)
