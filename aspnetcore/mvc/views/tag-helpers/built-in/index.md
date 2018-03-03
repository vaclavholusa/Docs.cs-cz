---
title: "Předdefinované značky Pomocníci ASP.NET Core"
author: pkellner
description: "Předdefinované značky Pomocníci ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 09/13/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 1d086bc0bc210fe19dda34d13008a49d48801607
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="ed415-103">Předdefinované značky Pomocníci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed415-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="ed415-104">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ed415-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="ed415-105">ASP.NET Core zahrnuje mnoho předdefinovaných značky pomocníci pro zvýšení produktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ed415-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="ed415-106">Tato část obsahuje přehled integrované Pomocníci značky.</span><span class="sxs-lookup"><span data-stu-id="ed415-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="ed415-107">Jsou předdefinované značky pomocné rutiny, které nejsou popsané, protože se používá interně pomocí [Razor](xref:mvc/views/razor) modul zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ed415-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="ed415-108">To zahrnuje značka Pomocník pro ~ znaku, který rozšíří na kořenovou cestu webové stránky.</span><span class="sxs-lookup"><span data-stu-id="ed415-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="ed415-109">Pomocníci značky předdefinované ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed415-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="ed415-110">**[Pomocník značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="ed415-111">**[Pomocník značky mezipaměti](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="ed415-112">**[Pomocník značky distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="ed415-113">**[Pomocník značku prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="ed415-114">**[Pomocník značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="ed415-115">**[Pomocník značka obrázku](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="ed415-116">**[Pomocník vstupní značky](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="ed415-117">**[Pomocník značky popisek](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="ed415-118">**[Pomocník částečné značky](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="ed415-119">**[Vyberte značku pomocné rutiny](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="ed415-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="ed415-121">**[Pomocná rutina pro ověření zprávy značky](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="ed415-122">**[Pomocná rutina pro ověření Summary – značka](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ed415-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed415-123">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ed415-123">Additional resources</span></span>

* [<span data-ttu-id="ed415-124">Vývoj klientské strany</span><span class="sxs-lookup"><span data-stu-id="ed415-124">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="ed415-125">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="ed415-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
