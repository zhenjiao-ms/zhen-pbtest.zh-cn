---
title: Create Import
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55c9125f-969b-4cbf-b97c-e1eafc466aef
---
# Create Import
---
[Request](#request) | [Response](#response) | **[Preview operation]**
<a name="top"/>

The **Create Import** operation creates content from either a PBIX, Excel, package file or file path in OneDrive Pro, and returns the ID of the **Import** created. This allows you to take a package of content, such as a PBIX or Excel file, and import it into a Power BI workspace. The **Imports** API splits the PBIX package into its individual parts including datasets and reports. It will then provide a metadata link between the actual import operation and the objects that were created from it.

**Note** The operation has the same behavior as if you imported an Excel file through the connect screen in Power BI.

**Required scope**: Content.Create 
<a name="request"/>
## Request
POST https://api.powerbi.com/beta/myorg/imports

### Query string parameters
|Name|Type|Description|Values
|---|---|---|---
|datasetDisplayName|String|When using a PBIX or Excel file, setting this parameter will override the default name of the dataset.|A Dataset name.
|nameConflict|String|If datasetDisplayName is provided, determines what to do if a Dataset with the same name exists. Otherwise, determines what to do if the Dataset is already imported.|- Ignore (default) <br/>  - Abort <br/> - Overwrite


### Header
Content-Type: multipart/form-data 
Authorization: Bearer eyJ0eX ... FWSXfwtQ

### Request Body
-	If importing from a file, the body will contain a PBIX or Excel file [encoded as form data](http://www.w3.org/TR/html401/interact/forms.html). 
-	If importing from OneDrive Pro, the body will contain JSON properties that include file path and connection type.
	-	Connection type: import file data as dataset or connect Excel workbook as report. Values: import, connect. Optional parameter. Default is import.
	-	File path can be absolute or relative to the URL of the file.
		-	The filePath will use the [OneDrive file API path](https://msdn.microsoft.com/office/office365/APi/files-rest-operations#FileResource). 

File path example:

```
	filePath = shared%20with%20everyone/Denver%20Data.xlsx 
```

### Body schema

An **Import** has a **filePath** and a **connectionType**.

To import data from your file:
	
	{
	  "filePath": "shared with everyone/FileName.xlsx",
	  "connectionType": "import"
	}

To connect your Excel workbook as a report:
	
	{
	  "filePath": "shared with everyone/FileName.xlsx",
	  "connectionType": "connect"
	}

<a name="response"/>
## Response

### Response Headers
- Location header that points to the location of the import status information. 
- Retry-After header indicating the number of seconds the client should wait to make the next request

### Success status code

|Code|Description
|---|---
|201|Created. The request was fulfilled and a new Import was created.
|202|Accepted if the import is still in progress.


### Error status code

|Code|Description
|---|---
|409|Imported file exists with the same connectionType.


### Content-Type
application/json 

### Body schema
A **Create Import** response has the following schema:
	
	{"id": "{import_guid"}
