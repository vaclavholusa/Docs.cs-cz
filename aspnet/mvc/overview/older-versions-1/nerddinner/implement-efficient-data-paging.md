---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: "Implementaci stránkování efektivní dat | Microsoft Docs"
author: microsoft
description: "Krok 8 ukazuje, jak přidat podporu stránkování naše /Dinners adresu URL, takže místo 1000s večeří najednou, budete pouze zobrazujeme 10 nadcházející večeří v..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0b0fba604f97d3bb72d2d403e643b422b9ce48bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="implement-efficient-data-paging"></a>Implementaci stránkování efektivní dat
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 8 tohoto bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 8 ukazuje, jak přidat podporu stránkování na našem /Dinners URL tak, aby místo 1000s večeří najednou, jsme budete zobrazit jenom 10 nadcházející večeří v čase - a povolit koncovým uživatelům, aby stránka zpět a předávat přes celý seznam popisný způsobem optimalizace pro vyhledávací weby.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner krok 8: Podporu stránkování

Pokud naše lokality je úspěšné, bude mít tisíce nadcházející večeří. Potřebujeme, abyste měli jistotu, že škáluje pro zpracování všech těchto večeří naše uživatelské rozhraní a umožňuje uživatelům procházet je. Chcete-li povolit, přidáme podporu stránkování, aby naše */Dinners* URL tak místo zobrazení 1000s večeří na jednou, jsme budete zobrazit jenom 10 nadcházející večeří v čase - a povolit koncovým uživatelům, aby stránka zpět a předávat přes celý seznam v Optimalizace pro vyhledávací weby popisný způsob.

### <a name="index-action-method-recap"></a>Rekapitulace metoda akce index()

Index() metody akce v rámci třídy Naše DinnersController aktuálně vypadá jako níže:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Při požadavku na */Dinners* adresu URL, načte seznam všech nadcházející večeří a pak vykreslí výpis všech je:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Principy IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  je rozhraní, která byla zavedená s dotazy LINQ jako součást rozhraní .NET 3.5. Umožňuje efektivní "odložené spouštění" scénáře, které jsme můžete využít výhod implementovat podporu stránkování.

V našem DinnerRepository jsme se vrací položku IQueryable&lt;večeři&gt; pořadí z našich FindUpcomingDinners() metody:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Položka IQueryable&lt;večeři&gt; objekt vrácený metodou naše FindUpcomingDinners() zapouzdří dotaz pro načtení objektů večeři z našich databáze pomocí LINQ to SQL. Je důležité ho nebude provést dotaz na databázi nástroje dlouho, dokud jsme pokusí o přístup nebo iterace v dat v dotazu nebo jsme volat metodu ToList() u ho. Kód voláním metody naše FindUpcomingDinners() můžete volitelně můžete přidat další operace "zřetězené" / filtry na položku IQueryable&lt;večeři&gt; objekt před provedením dotazu. Technologie LINQ to SQL je pak dostatečně inteligentní provést kombinovaný dotaz proti dané databázi, pokud se požaduje data.

K implementaci stránkování logiku jsme aktualizovat metody akce Index() naše DinnersController tak, aby se vztahuje na vrácený IQueryable další operátory "Přeskočit" a "Vzít"&lt;večeři&gt; pořadí před volání ToList() u ho:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Ve výše uvedeném kódu Přeskočí prvních 10 nadcházející večeří v databázi a vrátí zpět 20 večeří. Technologie LINQ to SQL je dostatečně inteligentní vytvořit optimalizované SQL dotaz, který provede to přeskočení logiky ve službě SQL database – a není v tomto webu. To znamená, že i v případě, že máme miliony nadcházející večeří v databázi, bude jenom 10, který má být načten jako součást této žádosti (což efektivní a škálovatelné).

### <a name="adding-a-page-value-to-the-url"></a>Přidání hodnoty "stránka" na adresu URL

Místo pevně kódováno rozsah konkrétní stránky, chceme naše adresy URL zahrnout parametr "stránka", který určuje, které večeři rozsah uživatel požaduje.

#### <a name="using-a-querystring-value"></a>Pomocí hodnoty řetězce dotazu

Následující kód ukazuje, jak jsme aktualizovat naše metoda akce Index() podporu parametru řetězce dotazu a povolení adresy URL jako */Dinners? stránky = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Metoda akce Index() výše má parametr s názvem "stránka". Parametr je deklarován jako celé číslo s možnou hodnotou Null (který je co int? označuje). To znamená, že */Dinners? stránky = 2* způsobí, že adresa URL hodnotu "2" mají být předány jako hodnotu parametru. */Dinners* adresa URL (bez hodnotu řetězce dotazu) způsobí, že mají být předány hodnotu null.

Chcete-li určit, kolik večeří přeskočíte jsme jsou součin hodnot stránky velikostí stránky (v tomto případě 10 řádků). Používáme [C# "slučování" operátor s hodnotou null (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) je užitečné při plánování práce s typy s možnou hodnotou Null. Výše uvedený kód přiřadí stránky Pokud stránka parametr je null. hodnota 0.

#### <a name="using-embedded-url-values"></a>Pomocí vložených URL hodnot

Alternativu k použití hodnotu řetězce dotazu by pro vložení parametru stránky v rámci skutečná adresa URL sám sebe. Příklad: */Dinners/Page/2* nebo */večeří/2*. ASP.NET MVC zahrnuje modulu Směrování výkonné adresu URL, která usnadňuje podporu scénářů takto.

Vlastní pravidla směrování jsme této mapy všechny příchozí adresy URL nebo adresa URL formátu řadiče třídu nebo akce metody, kterou chceme registrovat. Všechny potřebujeme úkolů je k otevření souboru Global.asax v rámci naší projektu:

![](implement-efficient-data-paging/_static/image2.png)

A pak zaregistrujte nové pravidlo mapování pomocí pomocnou metodu MapRoute() jako první volání trasy. MapRoute() níže:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Vyšší jsme registraci nové pravidel směrování s názvem "UpcomingDinners". Jsme jsou označující má formát adresy URL "večeří/stránky / {stránka}" – kde {page} je hodnota parametru vložených v rámci adresy URL. Třetí parametr metody MapRoute() označuje, že jsme měli mapování adres URL, které odpovídají tomuto formátu na metodu akce Index() k třídě DinnersController.

Můžeme použít přesný stejné Index() kód, který jsme měli před s náš scénář Querystring – s výjimkou teď bude naše "stránka" parametr pochází z adresy URL a není řetězec dotazu:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

A teď když jsme aplikaci spustit a zadejte */Dinners* jsme zobrazí prvních 10 nadcházející večeří:

![](implement-efficient-data-paging/_static/image3.png)

A když jsme zadejte */Dinners/Page/1* ukážeme na další stránku večeří:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Přidání navigaci na stránce uživatelského rozhraní

Poslední krok, dokud naše scénáře stránkování bude implementovat "následující" a "předchozí" uživatelské rozhraní v rámci naší zobrazení šablony umožňuje uživatelům snadno přeskočit večeři data.

Toto správně implementovat, budeme potřebovat vědět, celkový počet večeří v databázi a také jak počtu stránek s daty to znamená, že k. Potom potřebujeme k výpočtu, zda je hodnota aktuálně požadovaný "stránka" na začátku nebo na konec dat a zobrazit nebo skrýt uživatelské rozhraní "" a "předchozí" odpovídajícím způsobem. Tato logika jsme může implementovat v rámci naší Index() metody akce. Případně můžete přidáme pomocná třída pro naše projekt, který zapouzdřuje této logiky opakovaně použitelné způsobem.

Níže je jednoduchý "PaginatedList" pomocná třída, která je odvozena ze seznamu&lt;T&gt; třídy kolekce zabudovaná rozhraní .NET Framework. Implementuje třídu opakovaně použitelné kolekce, který slouží ke stránkování žádné pořadí dat IQueryable. V naší aplikaci NerdDinner jsme si ho fungovat přes IQueryable&lt;večeři&gt; výsledky, ale může stejně snadno použít u IQueryable&lt;produktu&gt; nebo IQueryable&lt;zákazníka&gt;výsledků v jiných scénářích aplikace:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Všimněte si výše způsob vypočítá a pak zpřístupňuje vlastnosti jako "Index", "PageSize", "TotalCount" a "TotalPages". Zpřístupní také pak dvě vlastnosti pomocná "HasPreviousPage" a "HasNextPage", které označuje, zda stránka dat v kolekci na začátku nebo na konci původní pořadí. Ve výše uvedeném kódu způsobí, že dva dotazy SQL ke spuštění – první načíst počet celkový počet objektů večeři (to nevrací objekty – místo provede příkaz "COUNT vyberte", který vrátí celé číslo) a druhou pro načtení právě řádky data, která potřebujeme z našich databáze pro aktuální stránku data.

Můžete aktualizujte naše DinnersController.Index() pomocnou metodu pro vytvoření PaginatedList&lt;večeři&gt; z našich DinnerRepository.FindUpcomingDinners() vést a předejte ji do našich zobrazit šablonu:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Můžete aktualizujte zobrazit šablonu \Views\Dinners\Index.aspx dědění z ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;večeři&gt; &gt; místo ViewPage&lt;rozhraní IEnumerable&lt;Večeři&gt;&gt;a pak přidejte následující kód do spodní části našich šablony zobrazení, pokud chcete zobrazit nebo skrýt další a předchozí uživatelské rozhraní:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Všimněte si výše jak Html.RouteLink() Pomocná metoda se používá ke generování naše hypertextové odkazy. Tato metoda je podobná Html.ActionLink() pomocnou metodu, kterou jsme jste dřív používali. Rozdílem je, že jsme se generování adresy URL pomocí "UpcomingDinners" směrování pravidlo, že jsme instalační program v rámci naší souboru Global.asax. To zajistí, že jsme budete vygenerování adres URL na metodu akce naše Index(), které mají formát: */Dinners/stránky / {page}* – tam, kde hodnota {stránky} je proměnná poskytujeme výše podle index aktuální stránky.

A teď když jsme spuštění aplikace znovu uvidíme 10 večeří současně do prohlížeče:

![](implement-efficient-data-paging/_static/image5.png)

Máme také &lt; &lt; &lt; a &gt; &gt; &gt; uživatelské rozhraní v dolní části stránky, které umožňuje přeskočit dopředný a zpětné přes našich dat pomocí vyhledávací stroj přístupné adresy URL:

![](implement-efficient-data-paging/_static/image6.png)

| **Téma straně: Vysvětlení důsledků IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; je velmi výkonné funkce, která umožňuje škálu zajímavé odložené provedení scénáře (třeba stránkování a sestavení na základě dotazů). Jako s všechny výkonné funkce, chcete opatrnosti v tom, jak používáte a ujistěte se, že nebyl překročen. Je důležité vědět, že vrací položku IQueryable&lt;T&gt; výsledek z úložiště umožňuje volání kódu připojení pro metody zřetězené operátor k němu a proto se podílet na spuštění ultimate dotazu. Pokud nechcete, jak poskytnout volání kódu tuto možnost, pak by měla vrátit zpět IList&lt;T&gt; nebo rozhraní IEnumerable&lt;T&gt; výsledky - obsahujících výsledky dotazu, který již provedena. Pro scénáře stránkování to se vyžaduje push logiku stránkování skutečná data do úložiště metody volané. V tomto scénáři může aktualizujeme naše vyhledávací metody FindUpcomingDinners() podpis, že buď vrátí PaginatedList: PaginatedList&lt; večeři&gt; FindUpcomingDinners (index stránky int, int pageSize) {} nebo vrátit zpět rozhraní IList. &lt;Večeři&gt;a vrátí celkový počet večeří pomocí "totalCount" out param: IList&lt;večeři&gt; FindUpcomingDinners (index stránky int, int pageSize out int totalCount) {} |

### <a name="next-step"></a>Další krok

Nyní podíváme, jak jsme přidat podporu ověřování a autorizace pro naši aplikaci.

>[!div class="step-by-step"]
[Předchozí](re-use-ui-using-master-pages-and-partials.md)
[další](secure-applications-using-authentication-and-authorization.md)
