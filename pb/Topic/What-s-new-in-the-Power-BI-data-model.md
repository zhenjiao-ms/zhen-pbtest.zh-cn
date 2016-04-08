---
title: What&#39;s new in the Power BI data model
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 77f755da-26d3-4598-ba03-2d1f1906d8d1
---
# What&#39;s new in the Power BI data model
This page will list changes to the Power BI data model. The Power BI data model has version changes according it the [OData data model versioning standards](Power-BI-data-model-versioning.md).

##July 9, 2015: Added Tables array to [Get Datasets](Get-Datasets.md)

###JSON Body schema
Datasets have a GUID id and name, and a tables array.

	{
	"datasets":
	    [
	        {"id": "dataset_id GUID", "name": "dataset_name", "tables":[]}
	    ]
	}

### JSON Body Example
A collection of three Dataset objects: Product, SalesMarketing and Products. The id is a GUID.

	{\"datasets\":
		[{\"id\":\"74601322-e32b-4dfe-a470-62e5770e610a\",\"name\":\"Product\",
			\"tables\":[]},						
		{\"id\":\"c2f79f2f-7e26-41be-8b30-fca3f72bcf20\",\"name\":\"SalesMarketing\",
			\"tables\":[]}
	]}"
