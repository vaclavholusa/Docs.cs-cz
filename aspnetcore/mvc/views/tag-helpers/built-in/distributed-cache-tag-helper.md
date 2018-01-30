---
title: "Distribuované mezipaměti značky Pomocník ASP.NET Core"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značky mezipaměti"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 710477732b865e2e3821102d34545bbd4e0a5919
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="13aa6-103">Pomocník značky distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="13aa6-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="13aa6-104">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="13aa6-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="13aa6-105">Pomocník pro značku distribuované mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah ke zdroji distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="13aa6-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="13aa6-106">Pomocník distribuované mezipaměti značky dědí ze stejné základní třídy jako pomocný značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="13aa6-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="13aa6-107">Všechny atributy, které jsou spojené s pomocnou rutinou značky mezipaměti budou také fungovat na pomocná distribuované značky.</span><span class="sxs-lookup"><span data-stu-id="13aa6-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="13aa6-108">Pomocník distribuované mezipaměti značka odpovídá **explicitní závislosti Princip** známé jako **konstruktor vkládání**.</span><span class="sxs-lookup"><span data-stu-id="13aa6-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="13aa6-109">Konkrétně `IDistributedCache` rozhraní kontejneru je předána do distribuované mezipaměti značky pomocné rutiny pro konstruktor.</span><span class="sxs-lookup"><span data-stu-id="13aa6-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="13aa6-110">Pokud žádné konkrétní konkrétní implementaci `IDistributedCache` byla vytvořena v `ConfigureServices`, obvykle najít v souboru startup.cs, pak pomocná distribuované mezipaměti značky použije stejný zprostředkovatel v paměti pro ukládání data uložená v mezipaměti jako základní značky pomocná mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="13aa6-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="13aa6-111">Distribuované mezipaměti atributů pomocná značky</span><span class="sxs-lookup"><span data-stu-id="13aa6-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="13aa6-112">povoleno na vyprší platnost vyprší po vypršení platnosti klouzavé měnit podle hlavičky se liší podle dotazu se liší podle trasy se liší podle cookie se liší podle uživatele lišit podle priority</span><span class="sxs-lookup"><span data-stu-id="13aa6-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="13aa6-113">Definice naleznete v tématu Pomocník značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="13aa6-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="13aa6-114">Pomocník značky distribuované mezipaměti dědí z stejnou třídou jako mezipaměti značky pomocná tak tyto atributy jsou společné z mezipaměti značky pomocníka.</span><span class="sxs-lookup"><span data-stu-id="13aa6-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="13aa6-115">název (povinné)</span><span class="sxs-lookup"><span data-stu-id="13aa6-115">name (required)</span></span>

| <span data-ttu-id="13aa6-116">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="13aa6-116">Attribute Type</span></span>    | <span data-ttu-id="13aa6-117">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="13aa6-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="13aa6-118">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="13aa6-118">string</span></span>    | <span data-ttu-id="13aa6-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="13aa6-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="13aa6-120">Požadované `name` atribut slouží jako klíč do této mezipaměti uložené pro každou instanci pomocná značky distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="13aa6-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="13aa6-121">Na rozdíl od základní pomocná značky mezipaměti, který přiřazuje klíč každý mezipaměti značky pomocná instance na základě názvu stránky Razor a umístění pomocníka značky, na stránce razor pomocná distribuované mezipaměti značky pouze základny jeho klíče na atributu`name`</span><span class="sxs-lookup"><span data-stu-id="13aa6-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="13aa6-122">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="13aa6-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="13aa6-123">Distribuované mezipaměti značky pomocná IDistributedCache implementace</span><span class="sxs-lookup"><span data-stu-id="13aa6-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="13aa6-124">Existují dvě implementace `IDistributedCache` součástí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13aa6-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="13aa6-125">Je na základě **systému Sql Server** a druhý je založena na **Redis**.</span><span class="sxs-lookup"><span data-stu-id="13aa6-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="13aa6-126">Podrobnosti o těchto implementace lze najít na prostředek dále pojmenovaný "pracovní s distribuované mezipaměti".</span><span class="sxs-lookup"><span data-stu-id="13aa6-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="13aa6-127">Obě implementace zahrnovat nastavení instanci `IDistributedCache` v ASP.NET Core **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="13aa6-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="13aa6-128">Neexistují žádné atributů značky konkrétně související s používáním žádnou konkrétní implementaci `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="13aa6-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="13aa6-129">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="13aa6-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
