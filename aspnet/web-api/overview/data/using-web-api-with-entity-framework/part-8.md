---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Zobrazit podrobnosti o položkách | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868084"
---
<a name="display-item-details"></a><span data-ttu-id="cbaa0-102">Podrobnosti o položkách zobrazení</span><span class="sxs-lookup"><span data-stu-id="cbaa0-102">Display Item Details</span></span>
====================
<span data-ttu-id="cbaa0-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cbaa0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cbaa0-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="cbaa0-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="cbaa0-105">V této části bude přidána možnost zobrazení podrobností o jednotlivých adresáře.</span><span class="sxs-lookup"><span data-stu-id="cbaa0-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="cbaa0-106">V app.js přidejte následující kód do modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="cbaa0-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="cbaa0-107">V Views/Home/Index.cshtml přidá prvek ke data-bind odkaz Podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="cbaa0-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="cbaa0-108">Tato obslužná rutina kliknutí na pro váže &lt;&gt; elementu, který chcete `getBookDetail` funkce na modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cbaa0-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="cbaa0-109">Ve stejném souboru nahraďte následující výstřižku:</span><span class="sxs-lookup"><span data-stu-id="cbaa0-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="cbaa0-110">tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="cbaa0-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="cbaa0-111">Tento kód vytvoří tabulku, která je vázané na data do vlastnosti `detail` pozorovatelné v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="cbaa0-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="cbaa0-112">"&lt;!--Ko –&gt; &quot; syntaxe umožňuje zahrnout vazbu Knockout mimo elementu DOM.</span><span class="sxs-lookup"><span data-stu-id="cbaa0-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="cbaa0-113">V takovém případě `if` vazby způsobí, že v této části kódu, který se zobrazí pouze tehdy, když `details` hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="cbaa0-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="cbaa0-114">Teď Pokud spusťte aplikaci a klikněte na jednu z &quot;podrobností&quot; odkazy, aplikace se zobrazí podrobnosti adresáře.</span><span class="sxs-lookup"><span data-stu-id="cbaa0-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="cbaa0-115">[Předchozí](part-7.md)
> [další](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="cbaa0-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
