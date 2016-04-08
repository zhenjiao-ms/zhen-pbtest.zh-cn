---
title: Dataset operations
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d25be11a-ee96-47eb-b808-c070145329fb
---
# Dataset operations
---
A **Dataset** resource path is a collection of **Tables** owned by a signed in Power BI user. A **Table** has **Rows** and **Columns** that contain data. Using the Power BI Rest API, you can create new datasets and add data to the tables in those datasets. A dashboard that uses those datasets will automatically update when new data is available.

The Power BI REST API has the following **Dataset** operations:
- [Get Datasets](Get-Datasets.md): Returns a JSON list of all **Dataset** objects that includes a name and id.
- [Create Dataset](Create-Dataset.md): Creates a new **Dataset** object from a JSON schema definition.

### Dataset JSON
A **Dataset** has a collection of **Table** objects.

	{
	   "name": "dataset_name",
	   "tables":[
	   	…
	    ]
	}

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


## See also
- [Get started creating a Power BI app](https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-steps-to-create-a-power-bi-app)
- [Power BI REST API reference](Power-BI-REST-API-reference.md)
- [Overview of Power BI REST API](Overview-of-Power-BI-REST-API.md)
