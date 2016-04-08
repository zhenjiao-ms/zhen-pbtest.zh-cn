---
title: Power BI REST API reference
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e05cad22-434f-4d4c-90d7-459ea3b255af
---
# Power BI REST API reference
---
Power BI is a cloud-based service to build business intelligence dashboards for your organization. To learn more about the Power BI service, see [Get started with Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).

The Power BI REST API is a REST-based API that provides programmatic access to Power BI dashboard resources such as **Datasets**, **Tables**, and **Rows**.  With the Power BI REST API, you can create custom apps that push data into a Power BI dashboard. 

The Power BI REST API reference describes each Power BI REST operation. To get started creating a Power BI app with the Power BI REST API, see [Create a Power BI app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-introduction-to-creating-a-power-bi-app/).

The Power BI REST API has the following operations:

- [Dataset operations](Dataset-operations.md): Get and create Datasets.
- [Table operations](Table-operations.md): Get Tables and update Table schema.
- [Row operations](Row-operations.md): Add Rows and Delete Rows.
- [Group operations](Group-operations.md): Get Groups.
- [Import operations](Import-operations.md): Create Import, Get Imports, Get Import from GUID, and Get Import by File Path.
- [Dashboard operations](Dashboard-operations.md): Get Dashboards and Get Tiles.

To perform operations on Power BI resources, you send HTTP requests with a supported method: GET, POST, PUT, or DELETE to an endpoint that targets a resource collection or a specific resource. A Power BI operation requires an Azure Active Directory (AAD) access token. To learn more about how to authenticate to the Power BI service, see [Authenticate to Power BI service](Authenticate-to-Power-BI-service.md).

## Related topics
- [Create a Power BI app]( https://powerbi.microsoft.com/en-us/documentation/powerbi-developer-introduction-to-creating-a-power-bi-app/)
- [Get started with Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/)


