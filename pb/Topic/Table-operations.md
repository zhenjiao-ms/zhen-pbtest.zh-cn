---
title: Table operations
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ebd7c9ea-999c-4727-a7f6-fe145ad0ebd7
robots: nofollow
---
# Table operations
---
A **Table** resource path is a collection of **Table** objects of a particular **Dataset**. A **Table** has **Rows** and **Columns** that contain data. A **Column** has a name and **dataType**.

The Power BI REST API has the following **Table** operations:

- [Get Tables](Get-Tables.md): Returns a JSON list of **Tables** for the specified **Dataset**.
- [Update Table Schema](Update-Table-Schema.md): Updates the schema of an existing **Table**.

### Table JSON
**Tables** have a name and a collection of **Column** objects. A **Column** has a **name** and **dataType**.

	{
         "name": "table_name",
         "columns":[
            {
               "name": "column_name",
               "dataType": "data_type"
            },
        …
        ]
	}

### See also
- [Get started creating a Power BI app](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API reference](Power-BI-REST-API-reference.md)
- [Overview of Power BI REST API](Overview-of-Power-BI-REST-API.md)
