---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Ukládání do mezipaměti dat v architektuře (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V předchozím kurzu jsme zjistili, jak používat ukládání do mezipaměti v prezentační vrstvě. V tomto kurzu jsme zjistěte, jak využít výhod našich vrstvami architectu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 7776fb01d31d9f84e57de2d5d899726b52c26d19
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401997"
---
<a name="caching-data-in-the-architecture-vb"></a>Ukládání dat do mezipaměti v architektuře (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) nebo [stahovat PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> V předchozím kurzu jsme zjistili, jak používat ukládání do mezipaměti v prezentační vrstvě. V tomto kurzu jsme zjistěte, jak využít výhod naší vrstvené architektury dat do mezipaměti na vrstvy obchodní logiky. Provedeme to rozšířením architektury zahrnout vrstvu ukládání do mezipaměti.


## <a name="introduction"></a>Úvod

Jak jsme viděli v předchozím kurzu, ukládání do mezipaměti dat prvku ObjectDataSource s je stejně jednoduché jako si několik vlastností. Bohužel ObjectDataSource platí, ukládání do mezipaměti na prezentační vrstvu, která úzce spáruje zásad ukládání do mezipaměti na stránce ASP.NET. Jedním z důvodů pro vytváření vrstvené architektury je umožnit takové spojky je přerušeno. Vrstvy obchodní logiky, například odděluje obchodní logiku ze stránek ASP.NET, zatímco vrstva přístupu k datům odděluje obě části Podrobnosti o přístupu dat. Díky tomuto oddělení podrobnosti o přístupu obchodní logiku a data totiž upřednostňované, v části systému umožňuje lépe čitelný, snadněji spravovatelné a flexibilnější, chcete-li změnit. Také umožňuje znalosti domény a rozdělení práce, vývojář, pracující na t kódu prezentační vrstva musí být obeznámeni s podrobnostmi o databázi s aby bylo možné její práci. Zásady ukládání do mezipaměti od prezentační vrstvy oddělení nabízí podobné výhody.

V tomto kurzu jsme se rozšířit Naše architektura zahrnout *vrstev ukládání do mezipaměti* (nebo CL zkráceně), který využívá naše zásady ukládání do mezipaměti. Bude obsahovat vrstev ukládání do mezipaměti `ProductsCL` třída, která poskytuje přístup k informacím o produktu pomocí metody, jako je `GetProducts()`, `GetProductsByCategoryID(categoryID)`a tak dále, že při vyvolání, bude první pokus o načtení dat z mezipaměti. Pokud do mezipaměti je prázdný, tyto metody vyvolá odpovídající `ProductsBLL` metoda ve BLL, který by pak získat data z vrstvy DAL. `ProductsCL` Metody mezipaměti dat načtených z BLL před jeho vrácením.

Jak ukazuje obrázek 1, je umístěn CL mezi prezentační a obchodní logiky vrstvy.


![Další vrstva v architektuře náš je vrstev ukládání do mezipaměti (CL)](caching-data-in-the-architecture-vb/_static/image1.png)

**Obrázek 1**: ukládání do mezipaměti vrstvy (+/ CL) je další vrstva v architektuře náš


## <a name="step-1-creating-the-caching-layer-classes"></a>Krok 1: Vytvoření třídy vrstev ukládání do mezipaměti

V tomto kurzu vytvoříme velmi jednoduchý CL s jednu třídu `ProductsCL` , který má pouze několik metod. Vytváření vrstvu dokončeno ukládání do mezipaměti pro celou aplikaci by vyžadovaly vytvoření `CategoriesCL`, `EmployeesCL`, a `SuppliersCL` třídy a nabízí metody v těchto tříd vrstev ukládání do mezipaměti pro každou metodu dat přístup nebo změna v BLL. Stejně jako u knihoven BLL a DAL, by měla být v ideálním případě implementována vrstev ukládání do mezipaměti jako samostatný projekt knihovny tříd; ale jsme provede jako třída v `App_Code` složky.

Na další samostatné čistě CL třídy z třídy DAL a BLL umožňují s vytvořit novou podsložku v `App_Code` složky. Klikněte pravým tlačítkem na `App_Code` složku v Průzkumníku řešení zvolte novou složku a pojmenujte novou složku `CL`. Po vytvoření této složky, přidejte do ní novou třídu s názvem `ProductsCL.vb`.


![Přidat novou složku s názvem CL a třídu s názvem ProductsCL.vb](caching-data-in-the-architecture-vb/_static/image2.png)

**Obrázek 2**: Přidat novou složku s názvem `CL` a třídu s názvem `ProductsCL.vb`


`ProductsCL` Třída by měla obsahovat stejnou sadu dat přístup a úpravy metod, jak se nachází ve své třídě odpovídající vrstvy obchodní logiky (`ProductsBLL`). Místo vytváření všechny tyto metody umožňují s pouze sestavení používá několik tady získat představu pro tyto vzory se dají CL. Zejména, přidáme `GetProducts()` a `GetProductsByCategoryID(categoryID)` metody v kroku 3 a `UpdateProduct` přetížení v kroku 4. Můžete přidat zbývající `ProductsCL` metody a `CategoriesCL`, `EmployeesCL`, a `SuppliersCL` třídy ve volném čase.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Krok 2: Čtení a zápis do mezipaměti dat

ObjectDataSource ukládání do mezipaměti funkce prozkoumali v předchozím kurzu interně používá k ukládání dat načtených z BLL datové mezipaměti technologie ASP.NET. Mezipaměť dat je také možné programově přistupovat z třídy modelu code-behind stránky technologie ASP.NET nebo ze tříd v architektuře s webovou aplikací. Ke čtení a zápis do mezipaměti dat z modelu code-behind třídy s stránky technologie ASP.NET, používají následující vzor:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

[ `Cache` Třídy](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` metoda](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) má několik přetížení. `Cache("key") = value` a `Cache.Insert(key, value)` je synonymní a jak přidat položku do mezipaměti pomocí zadaného klíče bez definované vypršení platnosti. Obvykle chcete zadat vypršení platnosti při přidání položky do mezipaměti, buď jako závislosti nebo vypršení platnosti časovou synchronizací. Pomocí jednoho z jiných `Insert` přetížení metody s k poskytnutí informací podle závislostí nebo času vypršení platnosti.

Ukládání do mezipaměti vrstvu, kterou metod s muset nejdřív zkontrolujte, zda je požadovaná data v mezipaměti a pokud ano, vrátí ho odtud. Pokud není požadovaná data v mezipaměti, odpovídající metodu BLL musí vyvolat. Vrácená hodnota by měla uložit do mezipaměti a vráceny, jak ukazuje následující sekvenční diagram.


![Metody s vrstev ukládání do mezipaměti vrátit Data z mezipaměti, pokud je k dispozici s](caching-data-in-the-architecture-vb/_static/image3.png)

**Obrázek 3**: metod The vrstev ukládání do mezipaměti s vrátit Data z mezipaměti, pokud je k dispozici s


Posloupnost znázorněno na obrázku 3 je provést v CL třídy pomocí následujícího vzorce:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Tady *typ* je typ dat uložené v mezipaměti `Northwind.ProductsDataTable`, například *klíč* je klíč, který jednoznačně identifikuje položku mezipaměti. Pokud položka se zadaným *klíč* není v mezipaměti, pak *instance* bude `Nothing` a data budou načteny z příslušné metody knihoven BLL a přidají se do mezipaměti. Době `Return instance` dosažení *instance* obsahuje odkaz na data, buď z mezipaměti nebo získaných z BLL.

Ujistěte se, že výše uvedené model použijte při přístupu k datům z mezipaměti. Následující vzor, který vypadá na první pohled ekvivalentní, obsahuje malý rozdíl, který představuje časování. Ke konfliktům časování je obtížné ladit, protože samotné odhalit nedojde a je obtížné reprodukovat.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

Rozdíl v této druhé nesprávný kód fragmentu kódu je, že místo uložení odkazu na položku z mezipaměti v místní proměnné, datové mezipaměti přistupuje přímo v podmíněném příkazu *a* v `Return`. Představte si, že po dosažení tohoto kódu `Cache("key")` není `Nothing`, ale předtím, než `Return` je dosažen příkaz, vyloučí systému *klíč* z mezipaměti. V tomto případě výjimečných kód vrátí `Nothing` namísto očekávaného typu objektu.

> [!NOTE]
> Mezipaměť dat je bezpečná pro vlákno, takže není nutné k synchronizaci přístupu vláken pro jednoduché operace čtení nebo zápisu. Ale pokud potřebujete provést více operací s daty v mezipaměti, která musí být Atomický, zodpovídáte za implementaci zámek nebo jiný mechanismus zajistit bezpečný přístup z více vláken. Zobrazit [synchronizaci přístupu k mezipaměti ASP.NET](http://www.ddj.com/184406369) Další informace.


Položky můžete programově vyřazen z mezipaměti data s využitím [ `Remove` metoda](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) takto:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Krok 3: Vrací informace o produktu z`ProductsCL`třídy

Pro tento kurz umožní s implementovat dvě metody pro vracení informací o produktu ze `ProductsCL` třídy: `GetProducts()` a `GetProductsByCategoryID(categoryID)`. Jak je `ProductsBL` třídy v vrstvy obchodní logiky `GetProducts()` metodu CL vrátí informace o všech produktů, jako `Northwind.ProductsDataTable` objektu, při `GetProductsByCategoryID(categoryID)` vrátí všechny produkty v zadané kategorii.

Následující kód ukazuje část metody v `ProductsCL` třídy:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Nejprve, Všimněte si, `DataObject` a `DataObjectMethodAttribute` atributy použité na třídy a metody. Tyto atributy poskytují informace k Průvodci s prvek ObjectDataSource, označující, co třídy a metody by se zobrazit v Průvodci s kroky. Protože CL třídy a metody bude přistupovat prvku ObjectDataSource v prezentační vrstvě, po přidání těchto atributů a zlepšit tak prostředí v době návrhu. Vraťte se do [vytvoření vrstvy obchodní logiky](../introduction/creating-a-business-logic-layer-vb.md) kurz pro důkladnější popis těchto atributů a jejich vliv.

V `GetProducts()` a `GetProductsByCategoryID(categoryID)` metody, že data vrácená z `GetCacheItem(key)` metoda je přiřazena k místní proměnné. `GetCacheItem(key)` Metodu, která prozkoumáme krátce, vrátí určité položky z mezipaměti stanoveného *klíč*. Pokud žádná taková data nenajde v mezipaměti, je načten z odpovídající `ProductsBLL` metoda třídy a pak přidá do mezipaměti pomocí `AddCacheItem(key, value)` metody.

`GetCacheItem(key)` a `AddCacheItem(key, value)` metody rozhraní se mezipaměť dat, čtení a zápis hodnot, v uvedeném pořadí. `GetCacheItem(key)` Metoda je jednodušší z nich. Jednoduše vrací hodnotu z mezipaměti třídy pomocí předaným *klíč*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` nepoužívá *klíč* hodnota zadána, ale místo toho volání `GetCacheKey(key)` metoda, která vrátí *klíč* předponou ProductsCache-. `MasterCacheKeyArray`, Který obsahuje řetězec ProductsCache, také používá `AddCacheItem(key, value)` způsob, jak uvidíme okamžik.

Z třídy modelu code-behind stránky s ASP.NET, mezipaměť dat lze přistupovat pomocí `Page` třída s [ `Cache` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)a umožňuje syntaxí `Cache("key") = value`, jak je popsáno v kroku 2. Ze třídy v rámci architektury mezipaměti dat lze přistupovat pomocí buď `HttpRuntime.Cache` nebo `HttpContext.Current.Cache`. [Petr Johnsonem](https://weblogs.asp.net/pjohnson/default.aspx)na blogu [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) poznámky výhod snížený výkon při použití `HttpRuntime` místo `HttpContext.Current`; v důsledku toho `ProductsCL` používá `HttpRuntime`.

> [!NOTE]
> Pokud vaše architektura je implementováno pomocí projekty knihovny tříd, budete muset přidat odkaz na `System.Web` sestavení, aby bylo možné používat [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) a [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) třídy.


Pokud položka není nalezena v mezipaměti, `ProductsCL` metody třídy s získat data z knihoven BLL a přidejte ji do mezipaměti pomocí `AddCacheItem(key, value)` metoda. Chcete-li přidat *hodnotu* do mezipaměti můžeme použít následující kód, který používá 60 druhé čas vypršení:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` Určuje podle času vypršení platnosti budoucí dobu 60 sekund [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) označuje tom, že s žádné klouzavé vypršení platnosti. Když je tento `Insert` přetížení metody obsahuje vstupní parametry pro obě absolutní a klouzavé vypršení platnosti, můžete pouze zadat jeden z nich. Pokud se pokusíte k určení absolutní čas a časové období `Insert` vyvolá metoda výjimku `ArgumentException` výjimky.

> [!NOTE]
> Tato implementace `AddCacheItem(key, value)` metoda aktuálně má některé nedostatky. Vytvoříme řešení a vyřešit tyto problémy v kroku 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Krok 4: Zrušení platnosti při the Data v mezipaměti je upravovat prostřednictvím the architektury

Spolu s metod načítání dat je potřeba poskytovat stejné metody, jako BLL pro vkládání, aktualizaci a odstraňování dat vrstev ukládání do mezipaměti. Metody CL data s úpravy neprovádějte žádné změny data uložená v mezipaměti, ale místo toho volat metodu BLL s odpovídající data změny a pak zneplatnění mezipaměti. Jak jsme viděli v předchozím kurzu, to je stejné chování, která se použije prvku ObjectDataSource při jeho ukládání do mezipaměti funkce jsou povolené a jeho `Insert`, `Update`, nebo `Delete` jsou metody vyvolány.

Následující `UpdateProduct` přetížení, ukazuje, jak implementovat metody změny dat CL:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Úprava dat odpovídající metoda vrstvy obchodní logiky je vyvolán, ale před vrácením odpovědi musíme zneplatnění mezipaměti. Bohužel zrušení platnosti mezipaměti není jednoznačné protože `ProductsCL` třída s `GetProducts()` a `GetProductsByCategoryID(categoryID)` metody každé přidání položek do mezipaměti s různými klíči a `GetProductsByCategoryID(categoryID)` metoda přidá položku různé mezipaměti pro každý jedinečný *categoryID*.

Při zrušení platnosti mezipaměti, budeme muset odebrat *všechny* položek, které byly přidány pomocí `ProductsCL` třídy. Toho můžete docílit tím, že přidružíte *závislosti mezipaměti* s každou položku Přidat do mezipaměti `AddCacheItem(key, value)` metoda. Obecně platí závislost mezipaměti může být jiná položka v mezipaměti, souboru v systému souborů nebo data z databáze Microsoft SQL Server. Když závislost změní nebo je odebrat z mezipaměti, položky v mezipaměti je spojený se automaticky vyřadí z mezipaměti. Pro účely tohoto kurzu chceme vytvořit další položky v mezipaměti, která slouží jako závislost mezipaměti pro všechny položky přidané prostřednictvím `ProductsCL` třídy. Tímto způsobem všechny tyto položky lze odebrat z mezipaměti jednoduše odebráním závislosti mezipaměti.

Umožněte s aktualizace `AddCacheItem(key, value)` tak, aby každá položka přidána do mezipaměti prostřednictvím této metody není přidružená metoda závislost jedné mezipaměti:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` je pole řetězců obsahující jednu hodnotu, ProductsCache. Položka mezipaměti je nejprve přidány do mezipaměti a přiřazené aktuálnímu datu a času. Pokud položka mezipaměti už existuje, se aktualizuje. V dalším kroku se vytvoří závislost mezipaměti. [ `CacheDependency` Třídy](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) konstruktor s má několik přetížení, ale použitému v tady očekává, že dvě `String` pole vstupy. První z nich určuje sadu souborů pro použití jako závislosti. Protože jsme zadávat t chcete použít libovolné souborové závislosti, hodnota `Nothing` se používá pro první vstupní parametr. Druhé vstupní parametr určuje sadu mezipaměti klíče, které slouží jako závislosti. Tady můžeme určit jednu závislost, `MasterCacheKeyArray`. `CacheDependency` Je pak předán `Insert` metody.

Pomocí této změny `AddCacheItem(key, value)`, invaliding mezipaměť je stejně jednoduché jako odebráním závislosti.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Krok 5: Volání ukládání do mezipaměti vrstvy od prezentační vrstvy

Ukládání do mezipaměti vrstvu s třídy a metody je možné pracovat s daty pomocí technik jsme ve prozkoumat v rámci těchto kurzů. Pro ilustraci, práci s daty v mezipaměti, uložte změny do souboru `ProductsCL` třídy a pak otevřete `FromTheArchitecture.aspx` stránku `Caching` složky a přidejte prvku GridView. Vytvořte nový prvek ObjectDataSource z GridView s inteligentních značek. V prvním kroku průvodce s byste měli vidět `ProductsCL` třídy jako jednu z možností z rozevíracího seznamu.


[![Třída ProductsCL je zahrnuta v rozevíracím seznamu obchodní objekt](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Obrázek 4**: `ProductsCL` třída je zahrnuta v rozevíracím seznamu obchodní objekt ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-in-the-architecture-vb/_static/image6.png))


Po výběru `ProductsCL`, klikněte na tlačítko Další. Rozevírací seznam v kartě vyberte má dvě položky - `GetProducts()` a `GetProductsByCategoryID(categoryID)` a kartu aktualizace má jediný `UpdateProduct` přetížení. Zvolte `GetProducts()` metodu z vyberte kartu a `UpdateProducts` metoda kartu aktualizace a klikněte na Dokončit.


[![Metody třídy ProductsCL s jsou uvedeny v rozevírací seznamy](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Obrázek 5**: `ProductsCL` metody třídy s jsou uvedeny v rozevírací seznamy ([kliknutím ji zobrazíte obrázek v plné velikosti](caching-data-in-the-architecture-vb/_static/image9.png))


Po dokončení průvodce, Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}` a přidejte do příslušných polí do prvku GridView. Změnit `OldValuesParameterFormatString` vlastnost zpět na výchozí hodnotu, `{0}`a konfigurace ovládacího prvku GridView pro podporu stránkování, řazení a úpravy. Vzhledem k tomu, `UploadProducts` přetížení používané CL přijímá pouze upravených produkt s názvem a ceny, omezit prvku GridView tak, aby se jenom tato pole upravovat.

V předchozím kurzu jsme definovali GridView pro zahrnutí polí pro `ProductName`, `CategoryName`, a `UnitPrice` pole. Nebojte se replikace tohoto formátování a struktura, v takovém případě vašeho ovládacího prvku GridView a prvku ObjectDataSource s deklarativní značek by měl vypadat nějak takto:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

V tuto chvíli máme stránku, která používá vrstev ukládání do mezipaměti. Pokud chcete zobrazit mezipaměti v akci, nastavte zarážky v `ProductsCL` třída s `GetProducts()` a `UpdateProduct` metody. Na stránce v prohlížeči a krokovat kód při řazení a stránkování, chcete-li zobrazit data získaná z mezipaměti. Potom aktualizovat záznam a Všimněte si, že platnost mezipaměti a v důsledku toho jsou načteny z BLL při dat je znovu připojeno k prvku GridView.

> [!NOTE]
> Ukládání do mezipaměti vrstvy k dispozici v souboru pro stažení doprovodném Tento článek není úplný. Obsahuje pouze jednu třídu, `ProductsCL`, která jenom sportovní několik metod. Kromě toho používá jenom jednu stránku ASP.NET CL (`~/Caching/FromTheArchitecture.aspx`) všechny ostatní stále odkazují BLL přímo. Pokud máte v úmyslu používat CL ve vaší aplikaci, všechna volání od prezentační vrstvy by měl přejděte do CL, která by vyžadovala třídy s CL a metody, na něž se tyto třídy a metody v BLL aktuálně používá prezentační vrstvy.


## <a name="summary"></a>Souhrn

Při ukládání do mezipaměti je možné použít na prezentační vrstvu s ASP.NET 2.0 s SqlDataSource a ovládací prvky prvku ObjectDataSource, by v ideálním případě ukládání do mezipaměti odpovědnosti delegovat na samostatné vrstva v architektuře. V tomto kurzu jsme vytvořili vrstvu ukládání do mezipaměti, který se nachází mezi prezentační vrstva a vrstva obchodní logiky. Vrstev ukládání do mezipaměti je potřeba zadat stejnou sadu tříd a metod, které existují v knihoven BLL a jsou volány od prezentační vrstvy.

Došlo k ukládání do mezipaměti vrstvy příklady, Prozkoumali jsme v této a v předchozích kurzech *reaktivní načítání*. S načítáním reaktivní, načtení dat do mezipaměti pouze v případě provedení požadavku na data a tato data z mezipaměti chybí. Data mohou být také *proaktivně načíst* do mezipaměti, technika, která načte data do mezipaměti předtím, než je skutečně potřeba. V dalším kurzu uvidíme příklad proaktivní načítání, když se podíváme na tom, jak ukládat do mezipaměti při spuštění aplikace statickými hodnotami.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](caching-data-with-the-objectdatasource-vb.md)
> [další](caching-data-at-application-startup-vb.md)
