---
title: Power BI REST API limitations
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3cdde6a-0df8-4341-bace-0f24e57a8487
---
# Power BI REST API limitations
---
The Power BI REST API has the following limitations.

**To POST Rows**

- 10,000 max rows per single POST rows request
- 1,000,000 rows added per hour per dataset
- 5 max pending POST rows requests per dataset
- 120 POST rows requests per minute per dataset
- 200,000 max rows stored per table in FIFO dataset
- 5,000,000 max rows stored per table in ‘none retention policy’ dataset
- 32,766 characters per value for string column in POST rows operation

**POST Rows operation per Power BI plan**
- Dataset created by user with free service plan: 10,000 rows added per hour per dataset
- Dataset created by user with paid service plan: 1,000,000 rows added per hour per dataset

- If a user exceeds this limit, we will fail subsequent API calls with the following details:
	* HTTP Status Code: 429 Too Many Requests
	* Response header: Retry-After + {Number of seconds until their quota resets}
	* An OData error with the following message: "You are over your rows per hour limit for this dataset. To push more rows per hour, upgrade your account to Power BI Pro or retry your push later."
	
## See also
[Overview of Power BI REST API](Overview-of-Power-BI-REST-API.md)
