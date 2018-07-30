---
title: Distribuovaná mezipaměť pomocná rutina značek v ASP.NET Core
author: pkellner
description: Ukazuje, jak pracovat s pomocná rutina značek v mezipaměti
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342169"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="4f2bc-103">Distribuovaná mezipaměť pomocná rutina značek v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f2bc-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="4f2bc-104">Podle [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4f2bc-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4f2bc-105">Pomocná rutina značek distribuované mezipaměti umožňuje výrazně zlepšit výkon vaší aplikace ASP.NET Core pomocí ukládání do mezipaměti obsah ke zdroji distribuované mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="4f2bc-106">Distribuované mezipaměti pomocná rutina značek v dědí ze základní třídy stejné jako pomocné rutiny značky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="4f2bc-107">Všechny atributy přidružené k pomocné rutiny značky mezipaměti, budou fungovat i na pomocné rutiny značky distribuovat.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="4f2bc-108">Následuje distribuované mezipaměti pomocná rutina značek v **explicitní závislosti Princip** říká **konstruktor vkládání**.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="4f2bc-109">Konkrétně `IDistributedCache` rozhraní kontejneru je předána do konstruktoru distribuované mezipaměti pomocné rutiny značky společnosti.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="4f2bc-110">Pokud žádné konkrétní konkrétní implementaci `IDistributedCache` byl vytvořen `ConfigureServices`, obvykle nachází v souboru startup.cs a pak distribuované mezipaměti pomocná rutina značek v bude použit stejný zprostředkovatel v paměti pro ukládání dat uložených v mezipaměti jako základní pomocná rutina značek mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="4f2bc-111">Distribuovaná mezipaměť atributy pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="4f2bc-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="4f2bc-112">povolené vyprší dne vyprší po vypršení platnosti klouzavé měnit podle hlavičky se liší podle dotazu se liší podle postupu se liší podle cookie se liší podle uživatele se liší podle priority</span><span class="sxs-lookup"><span data-stu-id="4f2bc-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="4f2bc-113">Viz pomocná rutina značek v mezipaměti pro definice.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="4f2bc-114">Pomocná rutina značek v distribuované mezipaměti dědí ze stejné třídy jako pomocná rutina značek v mezipaměti, takže tyto atributy jsou běžné z pomocná rutina značek v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="4f2bc-115">název (povinné)</span><span class="sxs-lookup"><span data-stu-id="4f2bc-115">name (required)</span></span>

| <span data-ttu-id="4f2bc-116">Typ atributu</span><span class="sxs-lookup"><span data-stu-id="4f2bc-116">Attribute Type</span></span>    | <span data-ttu-id="4f2bc-117">Příklad hodnoty</span><span class="sxs-lookup"><span data-stu-id="4f2bc-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4f2bc-118">odkazy řetězců</span><span class="sxs-lookup"><span data-stu-id="4f2bc-118">string</span></span>    | <span data-ttu-id="4f2bc-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="4f2bc-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="4f2bc-120">Požadované `name` atribut se používá jako klíč do mezipaměti ukládají pro každou instanci distribuované mezipaměti pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="4f2bc-121">Na rozdíl od základní mezipaměti pomocné rutiny značky, který přiřazuje klíč ke každé instanci pomocná rutina značek v mezipaměti na základě názvu stránky Razor a umístění pomocná rutina značky razor stránce distribuované mezipaměti pomocná rutina značek v pouze základů svůj klíč atributu `name`</span><span class="sxs-lookup"><span data-stu-id="4f2bc-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="4f2bc-122">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="4f2bc-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="4f2bc-123">Implementace distribuované mezipaměti IDistributedCache pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="4f2bc-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="4f2bc-124">Existují dvě implementace `IDistributedCache` součástí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="4f2bc-125">Jeden je založena na SQL Server a druhý je založen na Redis.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="4f2bc-126">Podrobnosti o těchto implementacích najdete <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="4f2bc-127">Obou implementacích zahrnují nastavení instance objektu `IDistributedCache` v ASP.NET Core *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="4f2bc-128">Neexistují žádné atributy značky konkrétně spojených s použitím žádnou konkrétní implementaci `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="4f2bc-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f2bc-129">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4f2bc-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
