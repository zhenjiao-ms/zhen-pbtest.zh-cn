---
title: Overview of all operations
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 59ed8a1d-07bd-4f57-8876-80d6215b94f3
---
# Overview of all operations
The Power BI REST API (the API) is a cloud-based service that you can use to build custom dashboard applications. The API has the concepts of pushing a dataset to a dashboard. A Dataset is a collection of Tables. A Table has rows and columns that contain data.

To perform operations on Power BI resources, you send HTTP requests with a supported method: GET, POST, PUT, or DELETE to an endpoint that targets a resource collection or a specific resource. A Power BI operation requires an Azure Active Directory (AAD) access token. To  

There are several types of operations that can be executed against the Power BI REST API:

- Dataset operations: Get and create datasets.
- Table operations: Get tables and update table schema.
- Row operations: Add rows and delete rows.
- Dashboard operations: Get dashboards, get tiles, and get a tile.
- Import operations: Get imports, create import, and get import status.
