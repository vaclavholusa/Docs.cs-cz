---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: "Pomocí postback ReorderList (VB) | Microsoft Docs"
author: wenz
description: "ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, objednávky a..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="ee365-104">Pomocí postback ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="ee365-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="ee365-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ee365-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ee365-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee365-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="ee365-107">ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení.</span><span class="sxs-lookup"><span data-stu-id="ee365-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="ee365-108">Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.</span><span class="sxs-lookup"><span data-stu-id="ee365-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="ee365-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="ee365-109">Overview</span></span>

<span data-ttu-id="ee365-110">`ReorderList` Ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení.</span><span class="sxs-lookup"><span data-stu-id="ee365-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="ee365-111">Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.</span><span class="sxs-lookup"><span data-stu-id="ee365-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="ee365-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="ee365-112">Steps</span></span>

<span data-ttu-id="ee365-113">Existuje několik možných datových zdrojů pro `ReorderList` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="ee365-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="ee365-114">Jedna je používat `XmlDataSource` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="ee365-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="ee365-115">Chcete-li vytvořit vazbu tento XML tak, aby `ReorderList` musí být nastavena řízení a povolit postback, následující atributy:</span><span class="sxs-lookup"><span data-stu-id="ee365-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="ee365-116">`DataSourceID`: ID zdroje dat</span><span class="sxs-lookup"><span data-stu-id="ee365-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="ee365-117">`SortOrderField`: Vlastnost řadit podle</span><span class="sxs-lookup"><span data-stu-id="ee365-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="ee365-118">`AllowReorder`: Zda chcete povolit uživatelům změnit pořadí prvky seznamu</span><span class="sxs-lookup"><span data-stu-id="ee365-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="ee365-119">`PostBackOnReorder`: Zda dojde k automatickému seznamu je přeskupení zpětné volání</span><span class="sxs-lookup"><span data-stu-id="ee365-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="ee365-120">Tady je vhodné značky ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="ee365-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="ee365-121">V rámci `ReorderList` , konkrétní data ze zdroje dat může být vázán pomocí `Eval()` metoda:</span><span class="sxs-lookup"><span data-stu-id="ee365-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="ee365-122">Na libovolné pozici na stránce štítek bude obsahovat informace při poslední změna došlo k chybě:</span><span class="sxs-lookup"><span data-stu-id="ee365-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="ee365-123">Tento popisek, naplní se text v kódu na straně serveru, zpracování zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="ee365-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="ee365-124">Nakonec, aby bylo možné aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit na stránce:</span><span class="sxs-lookup"><span data-stu-id="ee365-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="ee365-125">[![Každý změna aktivuje zpětné volání](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ee365-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="ee365-126">Každý změna aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ee365-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ee365-127">[Předchozí](drag-and-drop-via-reorderlist-cs.md)
[další](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ee365-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
