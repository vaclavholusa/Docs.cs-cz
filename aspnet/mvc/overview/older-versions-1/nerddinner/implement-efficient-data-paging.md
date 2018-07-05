---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementace efektivního stránkování dat | Dokumentace Microsoftu
author: microsoft
description: Krok 8 ukazuje, jak přidat podporu stránkování do naší /Dinners adresy URL, aby místo 1000s večeří najednou, bude pouze zobrazíme 10 nadcházející večeří na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: e6347c817c7518ef96ffbbf83cf98dd4dc6c011e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372446"
---
<a name="implement-efficient-data-paging"></a>Implementace efektivního stránkování dat
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 8 tohoto bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 8 ukazuje, jak přidat podporu stránkování do naší /Dinners adresy URL, aby místo 1000s večeří najednou, vytvoříme pouze zobrazení 10 nadcházející večeří najednou – a umožnit koncovým uživatelům, aby stránce zpět a vpřed mezi celý seznam popisný způsobem optimalizace pro vyhledávací weby.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner krok 8: Podpora stránkování

Pokud naše lokality je úspěšné, bude mít tisíce nadcházející večeří. Potřebujeme, abyste měli jistotu, že škáluje, aby se zpracovávaly všechny tyto večeří naše uživatelské rozhraní a umožňuje uživatelům procházet je. Chcete-li povolit, přidáme podporu stránkování, aby naše */Dinners* URL tak místo zobrazování 1000s večeří v jednou, jsme budete zobrazit jenom 10 nadcházející večeří najednou – a umožnit koncovým uživatelům, aby stránce zpět a vpřed mezi celý seznam v Optimalizace pro vyhledávací weby popisný způsobem.

### <a name="index-action-method-recap"></a>Rekapitulace index() akce – metoda

Index() metody akce v rámci naší třídy DinnersController aktuálně vypadá podobně jako následující:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Když vytvoří požadavek na */Dinners* adresu URL, načte seznam všechny nadcházející večeří a pak vykreslí výpis všech z nich si:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Principy IQuerable&lt;T&gt;

*Položka IQueryable&lt;T&gt;*  je rozhraní, které se zavedly s dotazy LINQ jako součást rozhraní .NET 3.5. Umožňuje efektivní "odložené provedení" scénáře, které nám můžete využít k implementaci podpory stránkování.

V našem DinnerRepository jsme vrací položku IQueryable&lt;Dinner&gt; pořadí z našich FindUpcomingDinners() metody:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Položka IQueryable&lt;Dinner&gt; objekt vrácený metodou naše FindUpcomingDinners() zapouzdřuje dotaz pro načtení objektů večeře v naší databázi pomocí LINQ to SQL. Dotaz na databázi je důležité je, nebude spuštěno až do pokusu o přístup a iterovat nad daty v dotazu nebo dokud jsme volat metodu ToList() na něm. Kódu volající metodě FindUpcomingDinners() můžete volitelně přidat další operace "zřetězené" / filtry na položku IQueryable&lt;Dinner&gt; objektu před provedením dotazu. Technologie LINQ to SQL je dostatečně inteligentní, aby spuštění kombinované dotaz na databázi, pokud se požaduje data.

Implementovat logiku stránkování můžete aktualizujeme naše DinnersController metody akce Index() tak, aby platil další operátory "Přeskočit" a "Vzít" pro vrácený IQueryable&lt;Dinner&gt; pořadí před voláním ToList() na něj:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Výše uvedený kód Přeskočí prvních 10 nadcházející večeří v databázi a potom vrátí zpět večeří 20. Technologie LINQ to SQL je dostatečně inteligentní, k vytvoření optimalizované dotaz SQL, který provádí toto přeskočení logiky ve službě SQL database – a ne v webový server. To znamená, že i v případě, že máme miliony nadcházející večeří v databázi, 10, které chceme načíst jako součást této žádosti (což efektivního a škálovatelného).

### <a name="adding-a-page-value-to-the-url"></a>Přidání hodnoty "page" na adresu URL

Místo pevného kódování konkrétní stránka rozsahu, chceme náš adresy URL, které obsahují parametr "stránky", která určuje rozsah které Dinner uživatele žádá o.

#### <a name="using-a-querystring-value"></a>Pomocí hodnoty řetězce dotazu

Následující kód ukazuje, jak budeme aktualizovat metodě Index() akce pro podporu parametru řetězce dotazu a povolit adresy URL jako */Dinners? stránky = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Výše uvedené metody akce Index() má parametr s názvem "stránky". Parametr je deklarován jako celé číslo s možnou hodnotou Null (to je co int? označuje). To znamená, že */Dinners? stránky = 2* způsobí, že adresa URL hodnotu "2" se mají předat jako hodnota parametru. */Dinners* adresy URL (bez hodnoty řetězce dotazu) způsobí, že se mají předat hodnotu null.

Hodnota stránky jsme se vynásobí velikost stránky (v tomto případě 10 řádků) Chcete-li určit, kolik večeří mají přeskočit. Používáme [C# null "slučovací" – operátor (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) což je užitečné při práci s typy s možnou hodnotou Null. Výše uvedený kód přiřadí stránky hodnota 0 v případě, že stránka parametr má hodnotu null.

#### <a name="using-embedded-url-values"></a>Pomocí adresy URL vložené hodnot

Vložit parametr stránku v rámci příkazu skutečnou adresu URL, samotného je o alternativu k použití hodnotu řetězce dotazu. Příklad: */Dinners/Page/2* nebo */večeří/2*. ASP.NET MVC zahrnuje výkonné adresy URL směrování modul, který usnadňuje podporu scénářů tímto způsobem.

Můžeme můžete zaregistrovat vlastní pravidla směrování, která je namapována příchozí adresy URL nebo adresa URL formátování na všechny řadiče třídy nebo metodě akce, které chceme. Potřebujeme úkolů je otevřít soubor Global.asax v našem projektu:

![](implement-efficient-data-paging/_static/image2.png)

A potom zaregistrovat nové pravidlo mapování pomocí MapRoute() pomocnou metodu jako první volání trasy. MapRoute() níže:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Výše jsme registraci nové pravidlo směrování s názvem "UpcomingDinners". Můžeme se označující má formát adresy URL "večeří nebo stránky / {stránce}" – kde {stránku} je hodnota parametru vloženého do adresy URL. Třetí parametr metody MapRoute() označuje, že jsme měli namapovat adresy URL, které mít tento formát na metodu akce Index() DinnersController třídy.

Můžeme použít přesně stejný kód Index(), které byla před v našem scénáři Querystring – s výjimkou teď bude naše "page" parametr pochází z adresy URL a Ne řetězec dotazu:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

A teď při spuštění aplikace jsme zadejte */Dinners* uvidíme prvních 10 nadcházející večeří:

![](implement-efficient-data-paging/_static/image3.png)

A když zadáme v */Dinners/Page/1* uvidíme na další stránku večeří:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Přidání navigace stránky uživatelského rozhraní

Posledním krokem v postupu dokončení scénáři stránkování bude k implementaci "následující" a "předchozí" uživatelské rozhraní v rámci naší zobrazit šablonu umožňující uživatelům snadno přejít nad daty Dinner.

Chcete-li toto správně implementovat, jsme budete muset znát celkový počet večeří v databázi a také jak mnoho stránek dat. to se přeloží na. Pak potřebujeme k výpočtu, zda je hodnota aktuálně požadované "stránky" na začátku nebo konci data a zobrazit nebo skrýt uživatelské rozhraní "" a "předchozí" odpovídajícím způsobem. Tato logika může implementaci v rámci naší Index() metody akce. Můžete také můžeme přidat pomocnou třídu pro náš projekt, který zapouzdřuje tuto logiku opakovaně použitelné způsobem.

Níže je jednoduchý "PaginatedList" pomocnou třídu, která je odvozena ze seznamu&lt;T&gt; integrovaná do kolekce tříd rozhraní .NET Framework. Implementuje třídu opakovaně použitelné kolekce, která lze stránkovat libovolnou posloupností dat IQueryable. V naší aplikace NerdDinner jsme si, jak nad IQueryable&lt;Dinner&gt; výsledky, ale stejně snadno dá použít proti IQueryable&lt;produktu&gt; nebo IQueryable&lt;zákazníka&gt;výsledků v jiných scénářích aplikace:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Všimněte si, že nad jak počítá a poté zpřístupní vlastnosti, jako jsou "Index", "PageSize", "TotalCount obsahuje" a "TotalPages". Poskytuje také pak dvě vlastnosti pomocné rutiny "HasPreviousPage" a "HasNextPage", které oznamuje, zda stránka data v kolekci je na začátku nebo konci původní sekvence. Výše uvedený kód způsobí, že dva dotazy SQL ke spuštění – první, kdo se načíst počet celkový počet objektů Dinner (to nevrací objekty – místo toho provádí příkaz "Vyberte počet", který vrátí celé číslo) a druhý k načtení jenom řádky data, které potřebujeme pro naši databázi pro aktuální stránku data.

Společnost Microsoft může aktualizovat také naše DinnersController.Index() pomocnou metodu k vytvoření PaginatedList&lt;Dinner&gt; z našich DinnerRepository.FindUpcomingDinners() způsobit a předejte jej do našich zobrazit šablonu:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Společnost Microsoft může aktualizovat také zobrazit šablonu \Views\Dinners\Index.aspx dědit z ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; místo ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;a potom přidejte následující kód do spodní části naší zobrazit šablonu chcete zobrazit nebo skrýt další a předchozí navigační uživatelské rozhraní:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Všimněte si, že nad jak používáme Html.RouteLink() Pomocná metoda ke generování naše hypertextové odkazy. Tato metoda je podobný Html.ActionLink() Pomocná metoda, kterou jsme použili dříve. Rozdíl je, že jsme se generování adresy URL pomocí "UpcomingDinners" směrování pravidlo, že se že nám nastavit v souboru Global.asax. Tím se zajistí, že vytvoříme adresy URL na metodu akce naše Index(), který mají formát: */Dinners/stránky / {stránku}* – Pokud hodnota {stránky} je proměnná poskytujeme nad podle aktuální index stránky.

A teď při provozujeme naši aplikaci znovu uvidíme 10 večeří najednou do prohlížeče:

![](implement-efficient-data-paging/_static/image5.png)

Máme také &lt; &lt; &lt; a &gt; &gt; &gt; navigační uživatelské rozhraní v dolní části stránky, která umožňuje přeskočit dopředu a zpětné vyhledávání přes naše data s využitím adresy URL přístupné modul:

![](implement-efficient-data-paging/_static/image6.png)

| **Téma na straně: Pochopení důsledků IQueryable&lt;T&gt;** |
| --- |
| Položka IQueryable&lt;T&gt; je velmi výkonné funkce, která umožňuje širokou škálu zajímavých scénářů odložené provedení (jako je stránkování a skládání na základě dotazů). Jako se všemi funkcemi výkonné, chcete pozor na tom, jak používáte a ujistěte se, že není zneužít. Je důležité uvědomit si vrací položku IQueryable&lt;T&gt; výsledků z úložiště umožňuje volajícím kódu pro zřetězené operátor metody k němu připojit a tak podílet na spuštění ultimate dotazu. Pokud nechcete zadat volající kód tuto možnost, pak by měl vrátit zpět IList&lt;T&gt; nebo IEnumerable&lt;T&gt; výsledky – které budou obsahovat výsledky dotazu, který byl již spuštěn. Pro scénáře stránkování to by vyžadovalo nasdílení změn do úložiště metoda volána logiky stránkování skutečná data. V tomto scénáři může být aktualizujeme naše vyhledávací metody FindUpcomingDinners() mít podpis, aby buď vrátil PaginatedList: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (int pageIndex, int pageSize) {} nebo vrátit zpět IList &lt;Dinner&gt;a pomocí "totalCount" si param vrátit celkový počet večeří: IList&lt;Dinner&gt; FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Dalším krokem

Nyní Podívejme se na tom, jak můžeme přidat podporu ověřování a autorizace pro naši aplikaci.

> [!div class="step-by-step"]
> [Předchozí](re-use-ui-using-master-pages-and-partials.md)
> [další](secure-applications-using-authentication-and-authorization.md)
