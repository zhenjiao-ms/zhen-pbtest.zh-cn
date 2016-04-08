---
title: 支持的数据类型
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f2e7b32-4da8-4722-9b22-2940afd891c5
---
# 支持的数据类型
---

Power BI REST API 具有以下支持的 [EDM](http://msdn.microsoft.com/en-us/library/vstudio/ee382832.aspx) 日期类型和限制：

<table>
  <tr>
    <td>
      <b>数据类型</b>
    </td>
    <td>
      <b>限制</b>
    </td>
  </tr>
  <tr>
    <td>Int64</td>
    <td>不允许使用 Int64.MaxValue 和 Int64.MinValue。</td>
  </tr>
  <tr>
    <td>双精度</td>
    <td>不允许使用 Double.MaxValue 和 Double.MinValue 值。NaN 不受支持。正无穷大和负无穷大在某些函数（例如 Min、Max）中不支持。</td>
  </tr>
  <tr>
    <td>布尔值</td>
    <td> 无</td>
  </tr>
  <tr>
    <td>日期时间</td>
    <td>在数据加载期间，我们将通过用天数除以 1/300 秒（3.33 毫秒）的整数倍，来求取值。</td>
  </tr>
  <tr>
    <td>字符串</td>
    <td>当前最多支持 128K 个字符。</td>
  </tr>
</table>


