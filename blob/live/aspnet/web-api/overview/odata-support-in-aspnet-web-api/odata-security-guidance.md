---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: "Doprovodné materiály zabezpečení pro rozhraní ASP.NET Web API 2 OData | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 799e2a0c742b545acf3b5cd27531d734aa7def80
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="e78e5-102">Doprovodné materiály zabezpečení pro rozhraní ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="e78e5-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="e78e5-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e78e5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e78e5-104">Toto téma popisuje některé problémy zabezpečení, které byste měli zvážit při vystavení datové sady přes OData.</span><span class="sxs-lookup"><span data-stu-id="e78e5-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="e78e5-105">Zabezpečení EDM</span><span class="sxs-lookup"><span data-stu-id="e78e5-105">EDM Security</span></span>

<span data-ttu-id="e78e5-106">Sémantiku dotazů jsou založené na entity data model (EDM), není základní typy modelu.</span><span class="sxs-lookup"><span data-stu-id="e78e5-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="e78e5-107">Může být vyloučena určitá vlastnost z EDM a nebude viditelná pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="e78e5-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="e78e5-108">Předpokládejme například, že model obsahuje typ zaměstnance s mzda vlastností.</span><span class="sxs-lookup"><span data-stu-id="e78e5-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="e78e5-109">Můžete chtít vyloučit tuto vlastnost z EDM skrytí od klientů.</span><span class="sxs-lookup"><span data-stu-id="e78e5-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="e78e5-110">Existují dva způsoby vyloučit vlastnost z modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="e78e5-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="e78e5-111">Můžete nastavit **[IgnoreDataMember]** atribut na vlastnost ve třídě modelu:</span><span class="sxs-lookup"><span data-stu-id="e78e5-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="e78e5-112">Můžete také odebrat vlastnost z EDM prostřednictvím kódu programu:</span><span class="sxs-lookup"><span data-stu-id="e78e5-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="e78e5-113">Dotaz zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e78e5-113">Query Security</span></span>

<span data-ttu-id="e78e5-114">Klient škodlivý nebo naïve můžou vytvořit dotaz, který trvá velmi dlouhou dobu spuštění.</span><span class="sxs-lookup"><span data-stu-id="e78e5-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="e78e5-115">V nejhorším případě to může narušit poskytování přístupu k službě.</span><span class="sxs-lookup"><span data-stu-id="e78e5-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="e78e5-116">**[Queryable]** atribut je filtr akce, které analyzuje, ověří a použije dotaz.</span><span class="sxs-lookup"><span data-stu-id="e78e5-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="e78e5-117">Filtr převede na výrazu LINQ možnosti dotazu.</span><span class="sxs-lookup"><span data-stu-id="e78e5-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="e78e5-118">Při vrácení kontroler OData **IQueryable** typu, **IQueryable** LINQ zprostředkovatele převede výrazu LINQ do dotazu.</span><span class="sxs-lookup"><span data-stu-id="e78e5-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="e78e5-119">Proto výkonu závisí na LINQ zprostředkovatele, který se používá a také na konkrétní vlastnosti schéma datové sady nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="e78e5-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="e78e5-120">Další informace o použití možností dotazu OData v rozhraní ASP.NET Web API najdete v tématu [podporu možností dotazu OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="e78e5-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="e78e5-121">Pokud víte, že všichni klienti jsou důvěryhodné (například v podnikovém prostředí), nebo pokud vaše datová sada je malý, nemusí být výkon dotazů problém.</span><span class="sxs-lookup"><span data-stu-id="e78e5-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="e78e5-122">Jinak měli byste zvážit následující doporučení.</span><span class="sxs-lookup"><span data-stu-id="e78e5-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="e78e5-123">Testování vaší služby s různé dotazy a profil databáze.</span><span class="sxs-lookup"><span data-stu-id="e78e5-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="e78e5-124">Povolte stránkování řízené serverem, aby se zabránilo vrácení velké sady dat v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="e78e5-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="e78e5-125">Další informace najdete v tématu [Server-Driven stránkování](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="e78e5-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="e78e5-126">Potřebujete $filter a $orderby?</span><span class="sxs-lookup"><span data-stu-id="e78e5-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="e78e5-127">Některé aplikace může povolit klienta stránkování, pomocí $top a $skip, ale zakažte jiné možnosti dotazu.</span><span class="sxs-lookup"><span data-stu-id="e78e5-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="e78e5-128">Vezměte v úvahu omezení $orderby k vlastnosti v clusterovaný index.</span><span class="sxs-lookup"><span data-stu-id="e78e5-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="e78e5-129">Řazení velkých objemů dat bez clusterovaného indexu je pomalý.</span><span class="sxs-lookup"><span data-stu-id="e78e5-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="e78e5-130">Počet uzlů maximální: **MaxNodeCount** vlastnost **[Queryable]** Nastaví maximální počet uzlů, které jsou povolené ve stromu syntaxe $filter.</span><span class="sxs-lookup"><span data-stu-id="e78e5-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="e78e5-131">Výchozí hodnota je 100, ale můžete chtít nastavit nižší hodnotu, protože velký počet uzlů může být pomalé zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="e78e5-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="e78e5-132">To je zvlášť true, pokud používáte LINQ na objekty (tj, dotazů LINQ na kolekci v paměti, ale bez použití zprostředkující LINQ poskytovatele).</span><span class="sxs-lookup"><span data-stu-id="e78e5-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="e78e5-133">Zvažte zakázání funkce any() a all(), protože to může být pomalé.</span><span class="sxs-lookup"><span data-stu-id="e78e5-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="e78e5-134">Pokud žádné vlastnosti řetězce obsahovat velké řetězce & #8212for příklad, popis produktu nebo položku blogu & #8212consider zakázání funkce řetězec.</span><span class="sxs-lookup"><span data-stu-id="e78e5-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="e78e5-135">Vezměte v úvahu zakazuje filtrování na navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e78e5-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="e78e5-136">Filtrování na navigační vlastnosti může mít za následek spojení, což může být pomalé, v závislosti na svého schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e78e5-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="e78e5-137">Následující kód ukazuje validátor dotazu, který brání filtrování na navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e78e5-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="e78e5-138">Další informace o dotazu validátory najdete v tématu [ověření dotazu](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="e78e5-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="e78e5-139">Vezměte v úvahu omezení $filter dotazy napsáním validátor, které je přizpůsobené pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="e78e5-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="e78e5-140">Představte si třeba tyto dva dotazy:</span><span class="sxs-lookup"><span data-stu-id="e78e5-140">For example, consider these two queries:</span></span> 

    - <span data-ttu-id="e78e5-141">Všechny filmy se účastníky, jejichž příjmení začíná "A".</span><span class="sxs-lookup"><span data-stu-id="e78e5-141">All movies with actors whose last name starts with ‘A'.</span></span>
    - <span data-ttu-id="e78e5-142">Všechny filmy vydané v roce 1994 za.</span><span class="sxs-lookup"><span data-stu-id="e78e5-142">All movies released in 1994.</span></span>

    <span data-ttu-id="e78e5-143">Pokud jsou filmy indexovat pomocí aktéři, první dotaz může vyžadovat databázový stroj ke kontrole celý seznam filmy.</span><span class="sxs-lookup"><span data-stu-id="e78e5-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="e78e5-144">Zatímco druhý dotaz zřejmě není přijatelné, předpokladu filmy jsou indexované podle verze roku.</span><span class="sxs-lookup"><span data-stu-id="e78e5-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="e78e5-145">Následující kód ukazuje validátor, který umožňuje filtrování na vlastnosti "ReleaseYear" a "Title", ale žádné jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e78e5-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="e78e5-146">Obecně platí zvažte $filter funkcích, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="e78e5-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="e78e5-147">Pokud vaši klienti nepotřebují úplné expressiveness $filter, můžete omezit mezi povolené funkce.</span><span class="sxs-lookup"><span data-stu-id="e78e5-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
