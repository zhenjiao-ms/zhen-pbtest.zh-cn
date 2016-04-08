---
title: 将 Power BI 磁贴集成到应用中
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f37a6c-b5d0-4b96-bc0b-7a5c7b4e00b2
robots: noindex,nofollow
---
# 将 Power BI 磁贴集成到应用中
---

从 GitHub 下载[集成 Power BI 磁贴示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)。

借助 Power BI，你可以使应用程序开发人员能够通过将 **IFrame** 嵌入应用（例如移动应用或 Web 应用）来在用户的 Power BI 帐户中集成 Power BI **磁贴**和**报告**。 企业用户可以使用 PowerBI.com 服务在其自己的 Power BI 帐户中生成和个性化图表、报告与仪表板。 嵌入**磁贴**或**报告**的功能支持你将这些磁贴或报告集成到应用中。 下面是将 Power BI 磁贴或报告集成到应用可以带来的优势：

-   通过少量的投入，就能在应用中提供用户可自定义的数据磁贴或报告。
-   用户可以集成应用程序外部任何数据源中的数据，以增强上下文认知。
-   用户可以通过单击嵌入的 Power BI 磁贴或报告导航到 Power BI **仪表板**。

使用 **IFrame** HTML 元素将 Power BI **磁贴**或**报告**集成到应用中。 例如，你可以创建一个自定义移动应用，用于在用户的移动设备上显示实时 Power BI 磁贴。 将 IFrame 嵌入应用后，你可以添加一个单击事件处理程序，以使用户可以导航到**仪表板**。

下面提供了将 Power BI 磁贴或报告集成到应用的步骤。 你也可以下载[将磁贴或报告集成到应用示例](https://github.com/PowerBI/Integrate-a-tile-or-report-into-an-app)。

###创建与 Power BI 磁贴集成的应用

| **步骤**| **另请参阅**
|:-|:-
| Power BI 用户的帐户必须至少有一个**仪表板**，该仪表板上至少有一个**磁贴**。| 若要注册 Power BI，请参阅[注册 Power BI 服务](https://msdn.microsoft.com/en-US/library/mt186467.aspx)。<br/><br/>若要了解如何使用 Power BI REST API 获取**仪表板**中的**磁贴**，请参阅[获取 Power BI 磁贴](#gettile)。
| 在应用中，获取 Azure Active Directory (Azure AD) 身份验证令牌。| 若要了解如何获取 Azure AD 身份验证令牌，请参阅[对 Power BI 服务进行身份验证](https://msdn.microsoft.com/en-US/library/mt203565.aspx)。了解 [Azure Active Directory 身份验证库](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-libraries/)。
| 对于磁贴，需要获取**仪表板** ID 和**磁贴** ID 才能将 **IFrame** 磁贴嵌入应用中| 若要了解如何获取**仪表板** ID 和**磁贴** ID，请参阅[获取 Power BI 磁贴](#gettile)。
| 在应用中，将 **IFrame** 的 SRC 属性设置为 **embedUrl**。| 若要设置 **IFrame** SRC 属性，请参阅[将 Power BI 磁贴嵌入 Web 应用](#embed)。
| 添加一个用于导航到 Power BI 仪表板的单击事件处理程序。| 请参阅[从 Power BI 磁贴导航到仪表板](#navigate)

<br/>
**重要说明：**若要为你的应用提供安全的登录和授权，请使用 **Azure Active Directory** (Azure AD) 对你的应用进行身份验证。 在开始将 Power BI 磁贴嵌入应用之前，需要在 Azure Active Directory 中注册你的应用。 请参阅 [注册客户端应用] (https://msdn.microsoft.com/en-us/library/dn877542.aspx) 或[注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)。 若要了解有关如何对 Power BI 服务进行身份验证的信息，请参阅[对 Power BI 服务进行身份验证](https://msdn.microsoft.com/en-US/library/mt203565.aspx)。

<a name="gettile"/>
##获取 Power BI 磁贴

若要获取 Power BI **磁贴**，首先需要获取用户的仪表板 ID。 可以使用[获取仪表板](Get-Dashboards.md)操作获取**仪表板** ID。 获取**仪表板** ID 后，可以使用[获取磁贴](Get-Tiles.md)操作获取**磁贴**。 有关演示如何嵌入 Power BI 磁贴的完整源代码，请下载[将磁贴集成到应用的示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)。

下面是获取 Power BI **磁贴**的步骤：

**步骤 1 – 获取 Azure AD 身份验证令牌**

若要获取 Azure AD 身份验证令牌，请参阅[对 Web 应用进行身份验证](https://msdn.microsoft.com/en-us/library/mt143610.aspx)或[对客户端应用进行身份验证](https://msdn.microsoft.com/en-us/library/dn877545.aspx)。

**步骤 2 – 获取用户的仪表板**

使用以下 REST URI 通过 GET Web 请求获取用户的**仪表板**。 [获取仪表板](Get-Dashboards.md)操作将返回用户仪表板的 JSON 数组，其中包含 **id** 和 **displayName**。 可以在[获取磁贴](Get-Tiles.md)操作中使用**仪表板 ID** 来获取用户的磁贴。 请参阅**步骤 3 – 获取用户的磁贴**。 该操作如下所示：

GET REST URI

    https://api.powerbi.com/beta/myorg/dashboards

JSON 响应


    {
    "@odata.context":"http://api.powerbi.com/beta/myorg/$metadata#dashboards","value":
    [
        {
            "id":"43127a01--e971d4cdc2fb",
            "displayName":"My Dashboard"
         }
    ]
    }

**注意：**有关如何获取仪表板的完整示例，请参阅[将磁贴集成到应用的示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)中的 **getDashboardsButton_Click**。

**步骤 3 – 获取用户的磁贴信息**

使用以下 REST URI 通过 GET Web 请求获取用户的**磁贴**。 请在**磁贴** URL 中，使用你在**步骤 2 – 获取用户的仪表板**中获取的仪表板 **ID**（如下所示）。 [获取磁贴](Get-Tiles.md)操作将返回用户仪表板中磁贴的 JSON 数组，其中包含 ID、标题和 **embedUrl**。 你需要使用 **embedUrl** 来设置 **IFrame** 源 URL。 请参阅 **步骤 4 – 设置 IFrame 源 URL**。 该操作如下所示：

GET REST URI

     GET https://api.powerbi.com/beta/myorg/{dashboard_id}/tiles

JSON 响应

    {
        "@odata.context":"api.powerbi.com/beta/myorg/$metadata#tiles","value":
        [
            {
                "id":"f16dc78a--8970-afe1098a100d",
                "title":"ProductID",
                "embedUrl":"https://api.powerbi.com/embed
                ?dashboardId=43127a01--e971d4cdc2fb
                &tileId=f16dc78a-6897-afe1098a100d"
            },
            {
                "id":"9ab3925d--4d6c896df0c9",
                "title":"Count of CountryRegionCode",
                "embedUrl":"https://api.powerbi.com/embed
                ?dashboardId=43127a01--e971d4cdc2fb
                &tileId=9ab3925d--4d6c896df0c9"
            }
        ]
    }

**注意：**有关如何获取磁贴的完整示例，请参阅[将磁贴集成到应用的示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)中的 **getTilesButton_Click**。

**步骤 4 – 设置 IFrame 源 URL**

获取**磁贴**的 **embedUrl** 后，请将 **IFrame** 的 **SRC** 属性设置为 **embedUrl**。 下一部分[将 Power BI 磁贴嵌入应用](#embed)介绍了如何使用 IFrame 嵌入磁贴。

<a name="embed"/>
##将 Power BI 磁贴嵌入应用

使用 **IFrame** HTML 将 Power BI **磁贴**嵌入应用。 你还需要在加载 **IFrame** 时从其中发布 **loadTile** 消息，并传递**身份验证令牌**以及高度和宽度。 请参阅**步骤 2 – 提交身份验证令牌**。 有关演示如何使用 IFrame 嵌入 Power BI 磁贴的完整代码示例，请参阅[将磁贴集成到应用的示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)。

下面是使用 IFrame 嵌入**磁贴**的步骤：

**步骤 1 – 将 IFrame 源设置为磁贴的 embedUrl**
按如下所示设置 IFrame src 属性：
iframe.src =  embedTileUrl + "&width=" + width + "&height=" + height;

**embedUrl** 具有以下模式和 url 参数：

**示例 embedUrl**

    https://app.powerbi.com/embed?dashboardId=302e6400--26968b3ee1e8&tileId=9bbf6732-f0b1--cb2c40c286fd

**嵌入 URL 参数**

<table>
  <tr>
    <td>
      <b>**名称**</b>
    </td>
    <td>
      <b>**说明**</b>
    </td>
    <td>
      <b>**REST 操作**</b>
    </td>
  </tr>
  <tr>
    <td>dashboardId</td>
    <td>用户仪表板的 ID。</td>
    <td>[获取仪表板](Get--Dashboards.md)</td>
  </tr>
  <tr>
    <td>tileId</td>
    <td>要嵌入的磁贴的 ID。可以从仪表板 ID 获取磁贴的数组。</td>
    <td>[获取磁贴](Get-Tiles.md)</td>
  </tr>
</table>

**步骤 2 – 提交身份验证令牌**

若要在 **IFrame** 中查看 Power BI **磁贴**，需要传递你在**步骤 1 – 获取 Azure AD 身份验证令牌**中获取的**身份验证令牌**。 为此，可按如下所示，向 IFrame.onload 事件分配一个 POST 方法：

    iframe.onload = postActionLoadTile;

POST 方法 (postActionLoadTile) 将构造一个推送消息结构，系统将使用 contentWindow.postMessage() 方法发送该消息，以便在 **Azure Active Directory** 服务上进行身份验证。 身份验证成功后，用户可以查看 Power BI **磁贴**。 下面是一个示例 POST 消息方法：

        // post the auth token to the iFrame. 
        function postActionLoadTile() {
            // get the access token.
            accessToken = document.getElementById('MainContent_accessTokenTextbox').value;
            // return if accessToken is empty
            if ("" === accessToken)
                return;
            var h = height;
            var w = width; 
            // construct the push message structure
            var m = { action: "loadTile", accessToken: accessToken, height: h, width: w};
            message = JSON.stringify(m);
            // push the message.
            iframe = document.getElementById('iFrameEmbedTile');
            iframe.contentWindow.postMessage(message, "*");;
        }

用户在看到 Power BI 磁贴后，可以单击该磁贴，以导航到包含该磁贴的 Power BI 仪表板。

<a name="navigate"/>
##从 Power BI 磁贴导航到仪表板

若要允许用户通过单击导航到 Power BI **仪表板**，你需要侦听并处理用于单击进入父窗口的**磁贴** POST 消息。 下面是执行此操作的步骤。 有关演示如何从 Power BI **磁贴**导航到仪表板的完整代码示例，请参阅[将磁贴集成到应用的示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)。

**步骤 1 – 添加事件侦听器**

在 window.onload 事件处理程序中，添加（或附加）一个事件侦听器，它可以处理用于单击进入父窗口的**磁贴** POST 消息。

    window.onload = function () {
    ...
    // listen for message to receive tile click messages.
    if (window.addEventListener) {
        window.addEventListener("message", receiveMessage, false);
        } else {
        window.attachEvent("onmessage", receiveMessage);
        }
    ...
    }

**步骤 2 – 在函数中获取消息数据并打开仪表板窗口**

在 **receiveMessage(event)** 函数中，通过分析 event.data 中的 JSON 获取消息数据（参阅以下示例）。 若要导航到包含磁贴的仪表板，需要构造包含 **仪表板** ID 的 URL。 以下 receiveMessage 函数显示了如何从 IFrame 源构造仪表板 URL。 **仪表板** URL 如下所示：

    https://msit.powerbi.com/dashboards/61b7e871--6572c921e271

###receiveMessage(event) 示例函数

    function receiveMessage(event)
    {
        if (event.data) {
        try {
            messageData = JSON.parse(event.data);
            if (messageData.event === "tileClicked")
            {
                //Get IFrame source and construct dashboard url
                iFrameSrc = document.getElementById(event.srcElement.iframe.id).src;
                //Split IFrame source to get dashboard id
                var dashboardId = iFrameSrc.split("dashboardId=")[1].split("&")[0];
                //Get PowerBI service url
                urlVal = iFrameSrc.split("/embed")[0] + "/dashboards/{0}";
                urlVal = urlVal.replace("{0}", dashboardId);
                window.open(urlVal);
            }
        }
        catch (e) {
            // In a production app, handle exception
        }
    }
    }

##结语

本文中的步骤一般性地描述了如何使用 **IFrame** 将 Power BI **磁贴**嵌入应用。 [将磁贴集成到应用的示例](https://github.com/PowerBI/Integrate-a-tile-into-an-app)显示了将 Power BI 磁贴嵌入 Web 应用所需的所有代码。 该示例还演示了如何添加**磁贴**单击事件，使用户可以从**磁贴**导航到其 Power BI **仪表板**。

##另请参阅

-   [对 Web 应用进行身份验证](https://msdn.microsoft.com/en-US/library/mt203565.aspx)
-   [注册 Web 应用](https://msdn.microsoft.com/en-us/library/dn985955.aspx)
-   [Azure Active Directory 身份验证库](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-libraries/)。
-   [开始创建 Power BI 应用](https://msdn.microsoft.com/en-US/library/mt186372.aspx)
-   Power BI REST [获取仪表板](Get-Dashboards.md)和[获取磁贴](Get-Tiles.md)操作





