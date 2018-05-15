---
title: Distribuované mezipaměti značky Pomocník ASP.NET Core
author: pkellner
description: Ukazuje, jak pracovat s pomocná značky mezipaměti
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 9c1d91fc185a0afecf59af8927ddf6f25eff29ab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="69df7-103">Distribuované mezipaměti značky Pomocník ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69df7-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="69df7-104">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="69df7-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="69df7-105">Pomocník pro značku distribuované mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah ke zdroji distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69df7-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="69df7-106">Pomocník distribuované mezipaměti značky dědí ze stejné základní třídy jako pomocný značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69df7-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="69df7-107">Všechny atributy, které jsou spojené s pomocnou rutinou značky mezipaměti budou také fungovat na pomocná distribuované značky.</span><span class="sxs-lookup"><span data-stu-id="69df7-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="69df7-108">Pomocník distribuované mezipaměti značka odpovídá **explicitní závislosti Princip** známé jako **konstruktor vkládání**.</span><span class="sxs-lookup"><span data-stu-id="69df7-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="69df7-109">Konkrétně `IDistributedCache` rozhraní kontejneru je předána do distribuované mezipaměti značky pomocné rutiny pro konstruktor.</span><span class="sxs-lookup"><span data-stu-id="69df7-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="69df7-110">Pokud žádné konkrétní konkrétní implementaci `IDistributedCache` byla vytvořena v `ConfigureServices`, obvykle najít v souboru startup.cs, pak pomocná distribuované mezipaměti značky použije stejný zprostředkovatel v paměti pro ukládání data uložená v mezipaměti jako základní značky pomocná mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69df7-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="69df7-111">Distribuované mezipaměti atributů pomocná značky</span><span class="sxs-lookup"><span data-stu-id="69df7-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="69df7-112">povoleno na vyprší platnost vyprší po vypršení platnosti klouzavé měnit podle hlavičky se liší podle dotazu se liší podle trasy se liší podle cookie se liší podle uživatele lišit podle priority</span><span class="sxs-lookup"><span data-stu-id="69df7-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="69df7-113">Definice naleznete v tématu Pomocník značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69df7-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="69df7-114">Pomocník značky distribuované mezipaměti dědí z stejnou třídou jako mezipaměti značky pomocná tak tyto atributy jsou společné z mezipaměti značky pomocníka.</span><span class="sxs-lookup"><span data-stu-id="69df7-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="69df7-115">název (povinné)</span><span class="sxs-lookup"><span data-stu-id="69df7-115">name (required)</span></span>

| <span data-ttu-id="69df7-116">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="69df7-116">Attribute Type</span></span>    | <span data-ttu-id="69df7-117">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="69df7-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="69df7-118">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="69df7-118">string</span></span>    | <span data-ttu-id="69df7-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="69df7-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="69df7-120">Požadované `name` atribut slouží jako klíč do této mezipaměti uložené pro každou instanci pomocná značky distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="69df7-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="69df7-121">Na rozdíl od základní pomocná značky mezipaměti, který přiřazuje klíč každý mezipaměti značky pomocná instance na základě názvu stránky Razor a umístění pomocníka značky, na stránce razor pomocná distribuované mezipaměti značky pouze základny svůj klíč na atributu `name`</span><span class="sxs-lookup"><span data-stu-id="69df7-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="69df7-122">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="69df7-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="69df7-123">Implementace distribuované mezipaměti značky pomocná IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="69df7-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="69df7-124">Existují dvě implementace `IDistributedCache` součástí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="69df7-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="69df7-125">Jeden je založený na systému SQL Server a druhý je založena na Redis.</span><span class="sxs-lookup"><span data-stu-id="69df7-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="69df7-126">Podrobnosti o těchto implementace naleznete na adrese <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="69df7-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="69df7-127">Obě implementace zahrnovat nastavení instanci `IDistributedCache` v ASP.NET Core *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="69df7-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="69df7-128">Neexistují žádné atributů značky konkrétně související s používáním žádnou konkrétní implementaci `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="69df7-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69df7-129">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="69df7-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
