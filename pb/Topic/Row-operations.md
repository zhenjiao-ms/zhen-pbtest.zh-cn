---
title: Row operations
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf65ea37-e8c7-4b9c-8536-8e60e8555350
---
# Row operations
---
A **Row** resource path is a collection of rows for a **Table**.

The Power BI REST API has the following **Row** operations:

- [Add Rows](Add-Rows.md): Adds **Rows** to a **Table** in a **Dataset**.
- [Delete Rows](Delete-Rows.md): Deletes **Rows** from a **Table** in a **Dataset**.

### Row JSON
**Rows** have a collection of **Column** names and values.

	{
		"rows":
	    	[
	        	{"column_name1": value, "column_name2": value, "column_name3": value, ...}
	    	]
	}
 
 ### See also
- [Get started creating a Power BI app](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API reference](Power-BI-REST-API-reference.md)
- [Overview of Power BI REST API](Overview-of-Power-BI-REST-API.md)
