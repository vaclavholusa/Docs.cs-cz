---
title: "Předdefinované značky Pomocníci ASP.NET Core"
author: pkellner
description: "Předdefinované značky Pomocníci ASP.NET Core"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: dd732822a715df19c0ee4b6accad3455ad6537da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="bc555-104">Předdefinované značky Pomocníci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc555-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="bc555-105">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="bc555-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="bc555-106">ASP.NET Core zahrnuje mnoho předdefinovaných značky pomocníci pro zvýšení produktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bc555-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="bc555-107">Tato část obsahuje přehled integrované Pomocníci značky.</span><span class="sxs-lookup"><span data-stu-id="bc555-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="bc555-108">Jsou předdefinované značky pomocné rutiny, které nejsou popsané, protože se používá interně pomocí [Razor](xref:mvc/views/razor) modul zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bc555-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="bc555-109">To zahrnuje značka Pomocník pro ~ znaku, který rozšíří na kořenovou cestu webové stránky.</span><span class="sxs-lookup"><span data-stu-id="bc555-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="bc555-110">Pomocníci značky předdefinované ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc555-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="bc555-111">**[Pomocník značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="bc555-112">**[Pomocník značky mezipaměti](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="bc555-113">**[Pomocník značky distribuované mezipaměti](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="bc555-114">**[Pomocník značku prostředí](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="bc555-115">**[Pomocník značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="bc555-116">**[Pomocník značka obrázku](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="bc555-117">**[Pomocník vstupní značky](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="bc555-118">**[Pomocník značky popisek](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="bc555-119">**[Vyberte značku pomocné rutiny](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="bc555-120">**[Pomocník TextArea značky](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="bc555-121">**[Pomocná rutina pro ověření zprávy značky](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="bc555-122">**[Pomocná rutina pro ověření Summary – značka](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="bc555-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc555-123">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bc555-123">Additional resources</span></span>

* [<span data-ttu-id="bc555-124">Vývoj straně klienta</span><span class="sxs-lookup"><span data-stu-id="bc555-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="bc555-125">Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="bc555-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
