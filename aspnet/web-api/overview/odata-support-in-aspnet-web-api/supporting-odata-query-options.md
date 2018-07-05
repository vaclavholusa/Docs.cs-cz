---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Podpora možností dotazů OData v rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 3ce2b38a13e8684a88bb0ce6183671fae98795c7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380840"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="12859-102">Podpora možností dotazů OData v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="12859-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="12859-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="12859-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="12859-104">OData definuje parametry, které lze použít k úpravě dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="12859-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="12859-105">Klient odešle tyto parametry v řetězci dotazu identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="12859-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="12859-106">Například pokud chcete výsledky seřadit, klient použije parametr $orderby:</span><span class="sxs-lookup"><span data-stu-id="12859-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="12859-107">Specifikace prostředí OData volá tyto parametry *možnosti dotazu*.</span><span class="sxs-lookup"><span data-stu-id="12859-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="12859-108">Možnosti dotazu OData pro každý kontroler rozhraní Web API můžete povolit ve vašem projektu &#8212; kontroleru nemusí být koncový bod OData.</span><span class="sxs-lookup"><span data-stu-id="12859-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="12859-109">To umožňuje pohodlný způsob, jak přidat funkce, jako je filtrování a řazení do libovolné aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="12859-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="12859-110">Před povolením možnosti dotazu, přečtěte si prosím téma [doprovodné materiály zabezpečení OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="12859-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="12859-111">Povolení možnosti dotazu OData</span><span class="sxs-lookup"><span data-stu-id="12859-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="12859-112">Příklady dotazů</span><span class="sxs-lookup"><span data-stu-id="12859-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="12859-113">Stránkování řízené serverem</span><span class="sxs-lookup"><span data-stu-id="12859-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="12859-114">Omezení možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="12859-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="12859-115">Přímo vyvoláním možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="12859-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="12859-116">Ověření dotazů</span><span class="sxs-lookup"><span data-stu-id="12859-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="12859-117">Povolení možnosti dotazu OData</span><span class="sxs-lookup"><span data-stu-id="12859-117">Enabling OData Query Options</span></span>

<span data-ttu-id="12859-118">Webové rozhraní API podporuje následující možnosti dotazu OData:</span><span class="sxs-lookup"><span data-stu-id="12859-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="12859-119">Možnost</span><span class="sxs-lookup"><span data-stu-id="12859-119">Option</span></span> | <span data-ttu-id="12859-120">Popis</span><span class="sxs-lookup"><span data-stu-id="12859-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="12859-121">$expand</span><span class="sxs-lookup"><span data-stu-id="12859-121">$expand</span></span> | <span data-ttu-id="12859-122">Rozbalí vložené související entity.</span><span class="sxs-lookup"><span data-stu-id="12859-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="12859-123">$filter</span><span class="sxs-lookup"><span data-stu-id="12859-123">$filter</span></span> | <span data-ttu-id="12859-124">Filtrování výsledků na základě logické podmínky.</span><span class="sxs-lookup"><span data-stu-id="12859-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="12859-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="12859-125">$inlinecount</span></span> | <span data-ttu-id="12859-126">Informuje server zahrnout celkový počet odpovídajících entit v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="12859-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="12859-127">(Užitečné pro stránkování na straně serveru.)</span><span class="sxs-lookup"><span data-stu-id="12859-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="12859-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="12859-128">$orderby</span></span> | <span data-ttu-id="12859-129">Seřadí výsledky.</span><span class="sxs-lookup"><span data-stu-id="12859-129">Sorts the results.</span></span> |
| <span data-ttu-id="12859-130">$select</span><span class="sxs-lookup"><span data-stu-id="12859-130">$select</span></span> | <span data-ttu-id="12859-131">Vybere vlastnosti, které chcete zahrnout do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="12859-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="12859-132">$skip</span><span class="sxs-lookup"><span data-stu-id="12859-132">$skip</span></span> | <span data-ttu-id="12859-133">Přeskočí prvních n výsledků.</span><span class="sxs-lookup"><span data-stu-id="12859-133">Skips the first n results.</span></span> |
| <span data-ttu-id="12859-134">$top</span><span class="sxs-lookup"><span data-stu-id="12859-134">$top</span></span> | <span data-ttu-id="12859-135">Vrací jenom prvních n výsledků.</span><span class="sxs-lookup"><span data-stu-id="12859-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="12859-136">Pokud chcete použít možnosti dotazu OData, musíte je povolit explicitně.</span><span class="sxs-lookup"><span data-stu-id="12859-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="12859-137">Můžete povolit globálně pro celou aplikaci nebo je povolit pro konkrétní řadiče nebo konkrétní akce.</span><span class="sxs-lookup"><span data-stu-id="12859-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="12859-138">Chcete-li povolit možnosti dotazu OData globálně, zavolejte **EnableQuerySupport** na **HttpConfiguration** třída při spuštění:</span><span class="sxs-lookup"><span data-stu-id="12859-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="12859-139">**EnableQuerySupport** metoda umožňuje využívat možnosti dotazu globálně pro všechny akce kontroleru, který vrátí **IQueryable** typu.</span><span class="sxs-lookup"><span data-stu-id="12859-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="12859-140">Pokud nechcete, aby možnosti dotazu, které jsou povolené pro celou aplikaci, můžete povolit je pro konkrétní ovladač akcí tak, že přidáte **[Queryable]** atributu na metodu akce.</span><span class="sxs-lookup"><span data-stu-id="12859-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="12859-141">Příklady dotazů</span><span class="sxs-lookup"><span data-stu-id="12859-141">Example Queries</span></span>

<span data-ttu-id="12859-142">Tato část uvádí typy dotazů, které je možné pomocí možností dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="12859-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="12859-143">Pro konkrétní podrobnosti o parametrech dotazů najdete v dokumentaci OData na [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="12859-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="12859-144">Informace o $rozbalte a $select, naleznete v tématu [pomocí $select $expand a $value v ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="12859-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="12859-145">**Stránkování řízené klienta**</span><span class="sxs-lookup"><span data-stu-id="12859-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="12859-146">Pro velká entita sady klient může chtít omezit počet výsledků.</span><span class="sxs-lookup"><span data-stu-id="12859-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="12859-147">Klient může například zobrazovat 10 položek současně s "Další" odkazy, chcete-li získat další stránky výsledků.</span><span class="sxs-lookup"><span data-stu-id="12859-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="12859-148">Klient použije k tomu možnosti $top a $skip.</span><span class="sxs-lookup"><span data-stu-id="12859-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="12859-149">Možnost $top poskytuje maximální počet vrácených položek a možnost $skip poskytuje počet položek pro přeskočení.</span><span class="sxs-lookup"><span data-stu-id="12859-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="12859-150">Předchozí příklad načte položky 21 až 30.</span><span class="sxs-lookup"><span data-stu-id="12859-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="12859-151">**Filtrování**</span><span class="sxs-lookup"><span data-stu-id="12859-151">**Filtering**</span></span>

<span data-ttu-id="12859-152">Možnost $filter umožňuje klientovi filtrování výsledků s použitím logického výrazu.</span><span class="sxs-lookup"><span data-stu-id="12859-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="12859-153">Filtr výrazy jsou velmi výkonné; patří mezi ně aritmetické a logické operátory, řetězce funkce a funkce data.</span><span class="sxs-lookup"><span data-stu-id="12859-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="12859-154">Vrátí všechny produkty s kategorií rovno "Toys".</span><span class="sxs-lookup"><span data-stu-id="12859-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="12859-155">`http://localhost/Products?$filter=Category` EQ 'Toys.</span><span class="sxs-lookup"><span data-stu-id="12859-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="12859-156">Vrátí všechny produkty s cenou menší než 10.</span><span class="sxs-lookup"><span data-stu-id="12859-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="12859-157">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="12859-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="12859-158">Logické operátory: vrátit všechny produkty, kde cena > = 5 a cena < = 15.</span><span class="sxs-lookup"><span data-stu-id="12859-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="12859-159">`http://localhost/Products?$filter=Price` ge 5 a cena le 15</span><span class="sxs-lookup"><span data-stu-id="12859-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="12859-160">Řetězec, funkce: vrátí všechny produkty s "zz" v názvu.</span><span class="sxs-lookup"><span data-stu-id="12859-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="12859-161">Data funkce: vrátí všechny produkty s ReleaseDate po 2005.</span><span class="sxs-lookup"><span data-stu-id="12859-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="12859-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="12859-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="12859-163">**Řazení**</span><span class="sxs-lookup"><span data-stu-id="12859-163">**Sorting**</span></span>

<span data-ttu-id="12859-164">Chcete-li seřadit výsledky, použijte filtr $orderby.</span><span class="sxs-lookup"><span data-stu-id="12859-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="12859-165">Seřadit podle cena.</span><span class="sxs-lookup"><span data-stu-id="12859-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="12859-166">Seřadit podle: cena v sestupném pořadí (nejvyšší k nejnižší).</span><span class="sxs-lookup"><span data-stu-id="12859-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="12859-167">Seřadit podle kategorie a pak řazení podle ceny v sestupném pořadí v rámci kategorie.</span><span class="sxs-lookup"><span data-stu-id="12859-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="12859-168">Stránkování řízené serverem</span><span class="sxs-lookup"><span data-stu-id="12859-168">Server-Driven Paging</span></span>

<span data-ttu-id="12859-169">Pokud databáze obsahuje milióny záznamů, které nechcete posílat je všechny v jedné datové části.</span><span class="sxs-lookup"><span data-stu-id="12859-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="12859-170">Chcete-li tomu zabránit, serveru můžete omezit počet položek, které odešle v jedné odezvě.</span><span class="sxs-lookup"><span data-stu-id="12859-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="12859-171">Chcete-li povolit stránkování na straně serveru, nastavte **PageSize** vlastnost **Queryable** atribut.</span><span class="sxs-lookup"><span data-stu-id="12859-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="12859-172">Hodnota je maximální počet vrácených položek.</span><span class="sxs-lookup"><span data-stu-id="12859-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="12859-173">Pokud váš kontroler vrací formátu OData, text odpovědi bude obsahovat odkaz na další stránku dat:</span><span class="sxs-lookup"><span data-stu-id="12859-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="12859-174">Klienta můžete použít tento odkaz k načtení další stránky.</span><span class="sxs-lookup"><span data-stu-id="12859-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="12859-175">Informace o tom celkový počet položek v sadě výsledků, můžete klienta nastavit možnost dotazu $inlinecount s hodnotou "allpages".</span><span class="sxs-lookup"><span data-stu-id="12859-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="12859-176">Hodnota "allpages" informuje server zahrnout celkový počet v odpovědi:</span><span class="sxs-lookup"><span data-stu-id="12859-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="12859-177">Odkazy na další stránky i vložený počet, který vyžadují formátu OData.</span><span class="sxs-lookup"><span data-stu-id="12859-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="12859-178">Důvodem je, že OData definuje zvláštní pole v těle odpovědi pro uložení odkazu a počet.</span><span class="sxs-lookup"><span data-stu-id="12859-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="12859-179">Protokolu OData formátů, je stále možné podporovat další stránky odkazy a vložený počet, obalením výsledky dotazu v **PageResult&lt;T&gt;**  objektu.</span><span class="sxs-lookup"><span data-stu-id="12859-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="12859-180">To ale vyžaduje trochu další kód.</span><span class="sxs-lookup"><span data-stu-id="12859-180">However, it requires a bit more code.</span></span> <span data-ttu-id="12859-181">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="12859-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="12859-182">Tady je příklad odpověď JSON:</span><span class="sxs-lookup"><span data-stu-id="12859-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="12859-183">Omezení možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="12859-183">Limiting the Query Options</span></span>

<span data-ttu-id="12859-184">Možnosti dotazu dát klientům spoustu kontrolu nad dotaz, který běží na serveru.</span><span class="sxs-lookup"><span data-stu-id="12859-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="12859-185">V některých případech můžete chtít omezit dostupné možnosti z důvodů zabezpečení nebo výkon.</span><span class="sxs-lookup"><span data-stu-id="12859-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="12859-186">**[Queryable]** atribut má některé integrované vlastnosti pro toto.</span><span class="sxs-lookup"><span data-stu-id="12859-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="12859-187">Tady je několik příkladů.</span><span class="sxs-lookup"><span data-stu-id="12859-187">Here are some examples.</span></span>

<span data-ttu-id="12859-188">Povolit pouze $skip a $top, pro podporu stránkování a nic jiného:</span><span class="sxs-lookup"><span data-stu-id="12859-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="12859-189">Povolit řazení jenom podle určité vlastnosti, aby se zabránilo řazení na vlastnosti, které nejsou indexovány v databázi:</span><span class="sxs-lookup"><span data-stu-id="12859-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="12859-190">Povolit funkci logické "eq", ale žádné logické funkce:</span><span class="sxs-lookup"><span data-stu-id="12859-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="12859-191">Nejsou povoleny všechny aritmetické operátory:</span><span class="sxs-lookup"><span data-stu-id="12859-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="12859-192">Můžete omezit možnosti globálně tak, že vytváří **položce QueryableAttribute** instance a předá se **EnableQuerySupport** funkce:</span><span class="sxs-lookup"><span data-stu-id="12859-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="12859-193">Přímo vyvoláním možnosti dotazu</span><span class="sxs-lookup"><span data-stu-id="12859-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="12859-194">Namísto použití **[Queryable]** atribut, možnosti dotazu můžete vyvolat přímo ve vašem řadiči.</span><span class="sxs-lookup"><span data-stu-id="12859-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="12859-195">Chcete-li to provést, přidejte **ODataQueryOptions** parametru k metodě kontroleru.</span><span class="sxs-lookup"><span data-stu-id="12859-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="12859-196">V takovém případě nepotřebujete **[Queryable]** atribut.</span><span class="sxs-lookup"><span data-stu-id="12859-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="12859-197">Naplní webového rozhraní API **ODataQueryOptions** řetězec dotazu z identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="12859-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="12859-198">Použije dotaz, předejte **IQueryable** k **volat metodu** metoda.</span><span class="sxs-lookup"><span data-stu-id="12859-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="12859-199">Metoda vrátí jiný **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="12859-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="12859-200">Pro pokročilé scénáře, pokud nemáte **IQueryable** poskytovatele dotazů, můžete zkontrolovat **ODataQueryOptions** a překládat možnosti dotazu do jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="12859-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="12859-201">(Například najdete v příspěvku blogu RaghuRam Nadiminti [dotazů překladu OData na HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), kde najdete také [ukázka](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="12859-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="12859-202">Ověření dotazů</span><span class="sxs-lookup"><span data-stu-id="12859-202">Query Validation</span></span>

<span data-ttu-id="12859-203">**[Queryable]** atribut ověří dotaz před jeho provedením.</span><span class="sxs-lookup"><span data-stu-id="12859-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="12859-204">Krok ověření se provádí v **QueryableAttribute.ValidateQuery** metody.</span><span class="sxs-lookup"><span data-stu-id="12859-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="12859-205">Můžete také přizpůsobit proces ověření.</span><span class="sxs-lookup"><span data-stu-id="12859-205">You can also customize the validation process.</span></span>

<span data-ttu-id="12859-206">Viz také [doprovodné materiály zabezpečení OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="12859-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="12859-207">Nejprve přepsání jeden validátoru tříd, který je definovaný v **Web.Http.OData.Query.Validators** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="12859-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="12859-208">Například následující třídu validátora zakáže možnost "desc" pro možnost $orderby.</span><span class="sxs-lookup"><span data-stu-id="12859-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="12859-209">Podtřídy **[Queryable]** atribut přepsání **ValidateQuery** metody.</span><span class="sxs-lookup"><span data-stu-id="12859-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="12859-210">Potom nastavte vlastní atribut buď globálně nebo na řadič:</span><span class="sxs-lookup"><span data-stu-id="12859-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="12859-211">Pokud používáte **ODataQueryOptions** přímo, nastavte ověřovací modul na možnostech:</span><span class="sxs-lookup"><span data-stu-id="12859-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
