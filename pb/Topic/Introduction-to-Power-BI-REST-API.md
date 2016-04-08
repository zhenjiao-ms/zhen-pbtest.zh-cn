---
title: Introduction to Power BI REST API
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e115235-3b5a-4dcd-9526-bf8142c6c1f4
robots: noindex,nofollow
---
# Introduction to Power BI REST API
---
The Power BI developer experience (Preview) is a REST API to push data sources in Power BI. When an app adds rows to a dataset, tiles on the dashboard are updated automatically with the updated data.

In previous Power BI versions, all models and reports connect to a data source that they pull data from. Data had to be refreshed on a schedule in order to see new data. With the Power BI REST API, Power BI will push data whenever it becomes available and reports that are dependent on this data will update in real time. This eliminates the latency with the pull model and ensure that data is as fresh as it can be.

## What can I do with the Power BI REST API? ##
- [Create a dataset](Create-Dataset.md)
- [Add rows](Add-Rows.md) or [Delete rows](Delete-Rows.md) to/from a dataset to update a tile automatically with new data
- [Get datasets](Get-Datasets.md)
- [Get tables](Get-Tables.md)
- [Update Table Schema](Update-Table-Schema.md)
- [Get groups](Get-Groups.md)

## Power BI REST and JSON ##
Power BI uses REST to represent the Power BI web API and JSON to describe objects in Power BI.

REST is one of the more popular architecture choices for web programming projects, appealing in its simplicity, low learning curve, and ability to leverage existing web protocols like HTTP. 

JSON or JavaScript Object Notation, is an open standard format that uses human-readable text to transmit data objects consisting of attribute-value pairs. It is used primarily to transmit data between a server and web application such as in a REST implementation. You describe objects in Power BI using JSON.

- To learn more about REST, see [Wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer).
- To learn more about JSON, see [Introducing JSON](http://json.org/).

**A simple JSON data format**

	{
        "name": "SalesMarketing",
        "tables": [
            {
				"name": "Product",
				"columns": [
				{
					"name": "ProductID",
					"dataType": "int"
       			},
				{
					"name": "Manufacturer",
					"dataType": "string"
     			},
        		{
      				"name": "Category",
   					"dataType": "string"
      			},
             	{
                	"name": "Segment",
                 	"dataType": "string"
             	},
            	{
                 	"name": "Product",
       				"dataType": "string"
              	},
              	{
            		"name": "IsCompete",
               		"dataType": "bool"
               	}
  				]
            }
        ]
	}


## Power BI REST Url's ##
Power BI REST resources are exposed for the following Power BI functionality:

**Root resources or objects**

    http(s)://api.powerbi.com/<Tenant>/<root>

**Example**

	https://api.powerbi.com/v1.0/myorg

**Collection of resources or objects**

    http(s)://api.PowerBI.com/<Tenant>/<collection>/<key>

**Example**

	https://api.powerbi.com/v1.0/myorg/datasets
	
**An object**

	https://api.powerbi.com/v1.0/myorg/datasets/{dataset_guid}/tables/Product/rows
	
**Groups**
	
	https://api.powerbi.com/v1.0/myorg/groups/{group_guid}/datasets/{dataset_guid}

**Operations (when not supported by verb)**

    http(s)://api.powerbi.com/<Tenant>/<root>/<Operation>

Where:

- **Tenant** - The tenant for this Power BI instance. As a shortcut, the value "myorg" can be supplied. In this case you use the tenant that is supplied in the authentication token that is sent with the request.

- **Root** - A root object such as User, Table, or Tile.

- **Collection** - The name of the collection such as Users, Tables, or Tiles.

- **Key** - The unique identifier of an object in a collection.

- **Operation** - Operation to perform such as Refresh or Merge.

### Verbs 
Operations are supported by verbs that provide consistent functionality across different endpoints. 

**Power BI REST verbs**

<table>

	<tr>
		<td><strong>Verb</strong></td>
		<td><strong>Description </strong></td>
	</tr>

	
	<tr>
		<td>GET</td>
		<td><p>Gets the value of an object or collection in the specified content type of the caller when applicable: </p>
		<ul>
			<li>application/xml  </li>
			<li>application/json</li>
		</ul>
		</td>
	</tr>
	<tr>
		<td>PUT</td>
		<td>Replaces an object, or creates a new named object when applicable. </td>
	</tr>
	<tr>
		<td>DELETE</td>
		<td>Deletes an object.</td>
	</tr>
	<tr>
		<td>POST</td>
		<td>Creates a new unnamed object or appends to an existing one. Returns the location of the object that was created if successful.  </td>
	</tr>
</table>

## Related topics
- [Get started creating a Power BI app](Get-started-creating-a-Power-BI-app.md)
- [Authenticate with Power BI](http://go.microsoft.com/fwlink/?LinkId=519359)
- [Registering an App](http://go.microsoft.com/fwlink/?LinkId=519361)
- [Power BI API Terms](https://powerbi.microsoft.com/en-us/api-terms)
