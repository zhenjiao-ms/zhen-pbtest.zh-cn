---
title: Supported data types
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f2e7b32-4da8-4722-9b22-2940afd891c5
---
# Supported data types
---
The Power BI REST API has the following supported [EDM](http://msdn.microsoft.com/library/vstudio/ee382832.aspx) date types and restrictions:
<table><tr><td>Data type</td><td>Restrictions</td></tr><tr><td>Int64</td><td>Int64.MaxValue and Int64.MinValue not allowed.</td></tr><tr><td>Double</td><td>Double.MaxValue and Double.MinValue values not allowed. NaN not supported.+Infinity and -Infinity not supported in some functions (e.g. Min, Max).</td></tr><tr><td>Boolean</td><td> None</td></tr><tr><td>Datetime</td><td>During data loading we quantize values with day fractions to whole multiples of 1/300 seconds (3.33ms).</td></tr><tr><td>String</td><td>Currently allows up to 128K characters.</td></tr></table>

## See also
[Overview of Power BI REST API](Overview-of-Power-BI-REST-API.md)