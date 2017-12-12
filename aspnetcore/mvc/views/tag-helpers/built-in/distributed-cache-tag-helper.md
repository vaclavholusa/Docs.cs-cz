---
title: "Distribuované mezipaměti značky pomocná | Microsoft Docs"
author: pkellner
description: "Ukazuje, jak pracovat s pomocná značky mezipaměti"
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 462c3775677924fc7b9b715cd6de75fe53ada89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="39d40-104">Pomocník značky distribuované mezipaměti</span><span class="sxs-lookup"><span data-stu-id="39d40-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="39d40-105">Podle [Petr Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="39d40-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="39d40-106">Pomocník pro značku distribuované mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti jeho obsah ke zdroji distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="39d40-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="39d40-107">Pomocník distribuované mezipaměti značky dědí ze stejné základní třídy jako pomocný značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="39d40-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="39d40-108">Všechny atributy, které jsou spojené s pomocnou rutinou značky mezipaměti budou také fungovat na pomocná distribuované značky.</span><span class="sxs-lookup"><span data-stu-id="39d40-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="39d40-109">Pomocník distribuované mezipaměti značka odpovídá **explicitní závislosti Princip** známé jako **konstruktor vkládání**.</span><span class="sxs-lookup"><span data-stu-id="39d40-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="39d40-110">Konkrétně `IDistributedCache` rozhraní kontejneru je předána do distribuované mezipaměti značky pomocné rutiny pro konstruktor.</span><span class="sxs-lookup"><span data-stu-id="39d40-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="39d40-111">Pokud žádné konkrétní konkrétní implementaci `IDistributedCache` byla vytvořena v `ConfigureServices`, obvykle najít v souboru startup.cs, pak pomocná distribuované mezipaměti značky použije stejný zprostředkovatel v paměti pro ukládání data uložená v mezipaměti jako základní značky pomocná mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="39d40-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="39d40-112">Distribuované mezipaměti atributů pomocná značky</span><span class="sxs-lookup"><span data-stu-id="39d40-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="39d40-113">povoleno na vyprší platnost vyprší po vypršení platnosti klouzavé měnit podle hlavičky se liší podle dotazu se liší podle trasy se liší podle cookie se liší podle uživatele lišit podle priority</span><span class="sxs-lookup"><span data-stu-id="39d40-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="39d40-114">Definice naleznete v tématu Pomocník značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="39d40-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="39d40-115">Pomocník značky distribuované mezipaměti dědí z stejnou třídou jako mezipaměti značky pomocná tak tyto atributy jsou společné z mezipaměti značky pomocníka.</span><span class="sxs-lookup"><span data-stu-id="39d40-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="39d40-116">název (povinné)</span><span class="sxs-lookup"><span data-stu-id="39d40-116">name (required)</span></span>

| <span data-ttu-id="39d40-117">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="39d40-117">Attribute Type</span></span>    | <span data-ttu-id="39d40-118">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="39d40-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="39d40-119">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="39d40-119">string</span></span>    | <span data-ttu-id="39d40-120">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="39d40-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="39d40-121">Požadované `name` atribut slouží jako klíč do této mezipaměti uložené pro každou instanci pomocná značky distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="39d40-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="39d40-122">Na rozdíl od základní pomocná značky mezipaměti, který přiřazuje klíč každý mezipaměti značky pomocná instance na základě názvu stránky Razor a umístění pomocníka značky, na stránce razor pomocná distribuované mezipaměti značky pouze základny jeho klíče na atributu`name`</span><span class="sxs-lookup"><span data-stu-id="39d40-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="39d40-123">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="39d40-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="39d40-124">Distribuované mezipaměti značky pomocná IDistributedCache implementace</span><span class="sxs-lookup"><span data-stu-id="39d40-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="39d40-125">Existují dvě implementace `IDistributedCache` součástí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39d40-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="39d40-126">Je na základě **systému Sql Server** a druhý je založena na **Redis**.</span><span class="sxs-lookup"><span data-stu-id="39d40-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="39d40-127">Podrobnosti o těchto implementace lze najít na prostředek dále pojmenovaný "pracovní s distribuované mezipaměti".</span><span class="sxs-lookup"><span data-stu-id="39d40-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="39d40-128">Obě implementace zahrnovat nastavení instanci `IDistributedCache` v ASP.NET Core **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="39d40-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="39d40-129">Neexistují žádné atributů značky konkrétně související s používáním žádnou konkrétní implementaci `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="39d40-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="39d40-130">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="39d40-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
