---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Ukládání dat v architektuře (C#) | Microsoft Docs
author: rick-anderson
description: V předchozích kurzu jsme zjistili, jak použít ukládání do mezipaměti na prezentační vrstvy. V tomto kurzu jsme zjistěte, jak využívat naše vrstveného architectu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 9ca91ecdaed536fe69196e0f726138590d7a9b77
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="caching-data-in-the-architecture-c"></a>Ukládání dat v architektuře (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) nebo [stáhnout PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> V předchozích kurzu jsme zjistili, jak použít ukládání do mezipaměti na prezentační vrstvy. V tomto kurzu jsme zjistěte, jak využívat naše Vrstvená architektura ukládat data do mezipaměti na vrstvu obchodní logiky. Provedeme to tím, že rozšíří architektura zahrnout vrstva ukládání do mezipaměti.


## <a name="introduction"></a>Úvod

Jak jsme viděli v předchozím kurzu, ukládání do mezipaměti ObjectDataSource s data je stejně jednoduché jako nastavení několik vlastností. Bohužel ObjectDataSource platí ukládání do mezipaměti na prezentační vrstvu, která pevně spojuje zásad ukládání do mezipaměti s stránku ASP.NET. Jedním z důvodů pro vytváření Vrstvená architektura je umožnit takové spojovací zařízení je přerušeno. Vrstvu obchodní logiky, například oddělí obchodní logiky ze stránky ASP.NET při Data Access Layer oddělí podrobnosti o přístupu dat. Díky tomuto oddělení podrobnosti o přístupu obchodní logiku a data je preferovaná, součást, protože umožňuje systému lépe čitelný, více udržovatelný a flexibilnější, chcete-li změnit. Umožňuje také pro domény znalosti a rozdělení práce vývojář pracující na t nemá prezentační vrstvy musí být obeznámeni s podrobnostmi databáze s cílem provést jeho úlohy. Oddělení zásady ukládání do mezipaměti od prezentační vrstvy nabízí podobné výhody.

V tomto kurzu jsme se posílení Naše architektura zahrnout *ukládání do mezipaměti vrstvy* (nebo pro zkrácení CL), využívá naše zásady ukládání do mezipaměti. Bude obsahovat vrstvě ukládání do mezipaměti `ProductsCL` třídu, která poskytuje přístup k produktu informace metody, třeba `GetProducts()`, `GetProductsByCategoryID(categoryID)`, a tak dále, že při vyvolání, bude první pokus o načtení dat z mezipaměti. Pokud mezipaměti je prázdná, budou tyto metody vyvolání odpovídající `ProductsBLL` metoda v BLL, které by naopak získat data z vrstvy DAL. `ProductsCL` Metody mezipaměti data načtená z BLL před vrácením.

Jak ukazuje obrázek 1, nachází CL mezi prezentační a obchodní logiku vrstvy.


![Ukládání do mezipaměti vrstvy (CL) je další vrstvu v Naše architektura](caching-data-in-the-architecture-cs/_static/image1.png)

**Obrázek 1**: ukládání do mezipaměti vrstvy (CL) je další vrstvu v Naše architektura


## <a name="step-1-creating-the-caching-layer-classes"></a>Krok 1: Vytvoření ukládání do mezipaměti vrstva tříd

V tomto kurzu vytvoříme velmi jednoduché CL s jednu třídu `ProductsCL` má pouze několik metod. Vytváření vrstva dokončení ukládání do mezipaměti pro celou aplikaci by vyžadovaly vytváření `CategoriesCL`, `EmployeesCL`, a `SuppliersCL` třídy a nabízí metody v těchto tříd vrstvy ukládání do mezipaměti pro každou metodu data přístup nebo změna v BLL. Stejně jako u BLL a DAL, by měla být v ideálním případě implementována vrstvě ukládání do mezipaměti jako samostatné projektu knihovny tříd. ale jsme se implementaci jako třída v `App_Code` složky.

Na další samostatné této aplikace CL třídy od třídy DAL a BLL umožňují s vytvořte novou podsložku v `App_Code` složky. Klikněte pravým tlačítkem na `App_Code` složky v Průzkumníku řešení, vyberte novou složku a název nové složky `CL`. Po vytvoření této složky, přidejte do ní novou třídu s názvem `ProductsCL.cs`.


![Přidat novou složku s názvem CL a třídy s názvem ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Obrázek 2**: přidejte novou složku s názvem `CL` a třídy s názvem `ProductsCL.cs`


`ProductsCL` Třída by měla obsahovat stejnou sadu dat přístup a úpravy metody, jak se nachází v jeho odpovídající třídě vrstvu obchodní logiky (`ProductsBLL`). Místo vytváření všechny tyto metody umožňují s jenom sestavení několik zde podívat, pro vzory používá CL. Konkrétně přidáme `GetProducts()` a `GetProductsByCategoryID(categoryID)` metody v kroku 3 a `UpdateProduct` přetížení v kroku 4. Můžete přidat zbývající `ProductsCL` metody a `CategoriesCL`, `EmployeesCL`, a `SuppliersCL` třídy ve volném čase.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Krok 2: Čtení a zápis do mezipaměti dat

ObjectDataSource ukládání do mezipaměti funkce prozkoumali v předchozím kurzu interně používá k ukládání data načtená z BLL data mezipaměti technologie ASP.NET. Mezipaměť dat také je možné programově přistupovat z třídy kódu stránky ASP.NET nebo z třídy v architektuře s webové aplikace. Ke čtení a zápisu do mezipaměti data z třídy kódu s stránka technologie ASP.NET, používají následující vzorec:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache` Třída](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` metoda](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) má několik přetížení. `Cache["key"] = value` a `Cache.Insert(key, value)` jsou synonyma a jak přidat položku do mezipaměti pomocí zadaného klíče bez definované vypršení platnosti. Obvykle chceme zadejte vypršela platnost při přidání položky do mezipaměti, buď jako závislost, na základě času vypršení platnosti nebo obojí. Použijte jednu z dalších `Insert` přetížení metody s poskytnout informace na základě závislostí nebo čas vypršení platnosti.

Ukládání do mezipaměti vrstvu, kterou s metody muset nejdřív zkontrolujte, zda požadovaná data jsou v mezipaměti a pokud ano, vrátí se z ní. Pokud není požadovaná data v mezipaměti, odpovídající metodu BLL musí být volána. Vrácená hodnota by měla do mezipaměti a poté vrácen, jak ukazuje následující diagram pořadí.


![Metody s ukládání do mezipaměti vrstvy vrátit Data z mezipaměti, pokud je k dispozici s](caching-data-in-the-architecture-cs/_static/image3.png)

**Obrázek 3**: vrstvy ukládání do mezipaměti s metody vrátit Data z mezipaměti, pokud je k dispozici s


Pořadí znázorněný na obrázku 3 je provést v třídách CL pomocí následujícího vzorce:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

Zde *typ* je typu dat, které jsou uložené v mezipaměti `Northwind.ProductsDataTable`, například *klíč* klíč, který jednoznačně identifikuje položku mezipaměti. Pokud položka se zadaným *klíč* není v mezipaměti, pak *instance* bude `null` a data budou načtena z odpovídající metodu BLL a přidány do mezipaměti. V čase `return instance` je dosaženo, *instance* obsahuje odkaz na data, buď z mezipaměti nebo stažen z BLL.

Ujistěte se, že použití výše vzoru při přístupu k datům z mezipaměti. Následující vzor, který vypadá na první pohled ekvivalentní, obsahuje jemně rozdíl, který představuje časování. Konflikty časování obtížně ladění, protože k sami odhalit a obtížně se znovu vyvolali.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

Rozdíl v této druhé, fragment kódu nesprávný je, že místo ukládání odkaz na položky v mezipaměti v místní proměnné, datové mezipaměti pracuje přímo v příkazu podmíněného *a* v `return`. Když je dosaženo tento kód, který Představte si `Cache["key"]` jinou hodnotu než`null`, ale předtím, než `return` příkaz je dosaženo, vyloučí systému *klíč* z mezipaměti. V tomto případě výjimečných kód vrátí `null` hodnotu namísto očekávaného typu objektu.

> [!NOTE]
> Mezipaměť dat je bezpečné pro přístup z více vláken, takže neuděláte potřeba t synchronizaci vlákno přístup pro jednoduché čtení nebo zápisu. Ale pokud je třeba provést několik operací na data v mezipaměti, které musí být Atomický, jste zodpovědní za implementaci zámek nebo jiným mechanismem zajistit bezpečný přístup z více vláken. V tématu [synchronizaci přístup k mezipaměti technologie ASP.NET](http://www.ddj.com/184406369) Další informace.


Položku můžete programově vyloučena z mezipaměti dat pomocí [ `Remove` metoda](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) takto:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Krok 3: Vrací informace o produktu z`ProductsCL`– třída

Pro tento kurz umožní s implementovat dvě metody vrací informace o produktu z `ProductsCL` – třída: `GetProducts()` a `GetProductsByCategoryID(categoryID)`. Jako s `ProductsBL` třídy v vrstvy obchodní logiky, `GetProducts()` metoda v CL vrátí informace o všech produktů jako `Northwind.ProductsDataTable` objekt, při `GetProductsByCategoryID(categoryID)` vrátí všechny produkty v zadané kategorii.

Následující kód ukazuje část metody v `ProductsCL` třídy:


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Nejprve, pamatujte `DataObject` a `DataObjectMethodAttribute` atributy použité u třídy a metody. Tyto atributy poskytnout informace o Průvodci s ObjectDataSource označující, co třídy a metody chcete zobrazit kroků v Průvodci s. Vzhledem k tomu, že CL třídy a metody bude přistupovat ObjectDataSource v prezentační vrstvě, po přidání těchto atributů k zajištění lepších možností návrhu. Odkazovat zpět [vytváření vrstvu obchodní logiky](../introduction/creating-a-business-logic-layer-cs.md) kurz podrobnější popis na tyto atributy a jejich důsledky.

V `GetProducts()` a `GetProductsByCategoryID(categoryID)` metody, s daty vrácenými ze `GetCacheItem(key)` metoda je přiřazený k místní proměnné. `GetCacheItem(key)` Metodu, která podíváme krátce, vrátí konkrétní položku z mezipaměti založené na zadaný *klíč*. Pokud žádná taková data nachází v mezipaměti, je načíst z odpovídající `ProductsBLL` třídy metoda a pak přidá do mezipaměti pomocí `AddCacheItem(key, value)` metoda.

`GetCacheItem(key)` a `AddCacheItem(key, value)` metody rozhraní s datovou mezipaměť, čtení a zápis hodnot v uvedeném pořadí. `GetCacheItem(key)` Způsob je jednodušší dvou. Jednoduše vrací hodnotu z mezipaměti třídy pomocí předané *klíč*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` nepoužívá *klíč* hodnotu jako zadaný, ale volání `GetCacheKey(key)` metoda, která vrátí hodnotu *klíč* přidá jako předpona s ProductsCache-. `MasterCacheKeyArray`, Který obsahuje řetězec ProductsCache, také používány `AddCacheItem(key, value)` metoda, jak jsme se zobrazí na okamžik.

Ze třídy kódu stránky s technologie ASP.NET datovou mezipaměť je přístupná pomocí `Page` třídu s [ `Cache` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)a umožňuje syntaxe jako `Cache["key"] = value`, jak je popsáno v kroku 2. Od třídy, v rámci architekturu, datovou mezipaměť je přístupná pomocí `HttpRuntime.Cache` nebo `HttpContext.Current.Cache`. [Petr Janíček](https://weblogs.asp.net/pjohnson/default.aspx)na položce blogu [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) poznámky k výhodu v podobě malého výkonu pomocí `HttpRuntime` místo `HttpContext.Current`; v důsledku toho `ProductsCL` používá `HttpRuntime`.

> [!NOTE]
> Pokud vaší architektury je implementovaná pomocí projektů knihovny tříd pak budete muset přidat odkaz na `System.Web` sestavení, aby bylo možné používat [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) a [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) třídy.


Pokud položka není nalezena v mezipaměti, `ProductsCL` metody třídy s získat data z BLL a přidejte ji do mezipaměti pomocí `AddCacheItem(key, value)` metoda. Chcete-li přidat *hodnotu* do mezipaměti může používáme následující kód, který používá 60 sekundu čas vypršení platnosti:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` Určuje na základě času vypršení platnosti 60 sekund budoucí chvíli [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) označuje tom, že s žádné klouzavé vypršení platnosti. Když je tento `Insert` přetížení metody má vstupní parametry pro obě absolutní a klouzavé vypršení platnosti, můžete jenom zadat jednu ze dvou. Pokud se pokusíte zadejte absolutní čas a časové období, `Insert` vyvolá metoda výjimku `ArgumentException` výjimka.

> [!NOTE]
> Tato implementace `AddCacheItem(key, value)` metoda má aktuálně některé nedostatků. Jsme budete adresy a vyřešit tyto problémy v kroku 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Krok 4: Zneplatnění mezipaměti při dat je změnit prostřednictvím architektura

Společně s metod načítání dat musí poskytovat stejné metody, jako BLL pro vložení, aktualizaci a odstraňování dat vrstvě ukládání do mezipaměti. Změna metody CL s datového neupravujte data uložená v mezipaměti, ale spíš zavolejte metodu BLL s odpovídající data úpravy a pak platnost mezipaměti. Jak jsme viděli v předchozím kurzu, to je stejné chování, která se vztahuje ObjectDataSource při jeho ukládání do mezipaměti funkce jsou povolené a jeho `Insert`, `Update`, nebo `Delete` jsou vyvolány metody.

Následující `UpdateProduct` přetížení znázorňuje, jak implementovat metod úpravy dat CL:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Úpravy příslušná data vrstvu obchodní logiky metoda je volána, ale před vrácením odpovědi musíme platnost mezipaměti. Bohužel zneplatnění mezipaměti není jednoznačné protože `ProductsCL` třídu s `GetProducts()` a `GetProductsByCategoryID(categoryID)` metody každý přidání položek do mezipaměti s různými klíči a `GetProductsByCategoryID(categoryID)` metoda přidá položku různé mezipaměti pro každý jedinečný *categoryID*.

Při zrušení platnosti mezipaměti, je potřeba odebrat *všechny* položek, které byl přidán nástrojem `ProductsCL` třídy. Můžete to provést tím, že přidružíte *závislosti mezipaměti* s každou položkou přidán do mezipaměti `AddCacheItem(key, value)` metoda. Obecně platí závislost mezipaměti může být jiné položky v mezipaměti, soubor v systému souborů nebo data z databáze Microsoft SQL Server. Když závislost změní nebo se nachází odebrány z mezipaměti, položky v mezipaměti je přidružen jsou automaticky odstraněna z mezipaměti. V tomto kurzu budeme rádi vytvoření další položky v mezipaměti, které slouží jako závislost mezipaměti pro všechny položky přidány prostřednictvím `ProductsCL` třídy. Tímto způsobem, všechny tyto položky můžete odeberou z mezipaměti jednoduše odebráním závislosti mezipaměti.

Aktualizace umožňují s `AddCacheItem(key, value)` metoda tak, aby každá položka do mezipaměti přidána prostřednictvím této metody je přidružen jeden mezipaměti závislost:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` pole řetězců obsahující jednu hodnotu, ProductsCache je. Nejprve položku mezipaměti je přidán do mezipaměti a přiřazené k aktuálnímu datu a času. Pokud položku mezipaměti, která již existuje, je aktualizována. V dalším kroku se vytvoří závislost mezipaměti. [ `CacheDependency` Třída](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) konstruktor s má několik přetížení, ale ten používá sem očekává dva `string` pole vstupy. První z nich určuje sadu souborů má být použit jako závislosti. Vzhledem k tomu, že jsme nejsou zobrazeny t chcete použít všechny závislosti na základě souborů, hodnota `null` se používá pro první vstupní parametr. Druhý vstupní parametr určuje sadu mezipaměti klíče k použití jako závislosti. Zde jsme naše jeden závislostí, zadejte `MasterCacheKeyArray`. `CacheDependency` Pak předá do `Insert` metoda.

Pomocí této změny `AddCacheItem(key, value)`, invaliding mezipaměti je jednoduché, odebráním závislosti.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Krok 5: Volání vrstvě ukládání do mezipaměti z prezentační vrstvy

Ukládání do mezipaměti vrstvy s třídy a metody slouží k práci s daty pomocí technik jsme sunout zkontrolován v rámci těchto kurzů. Pro ilustraci práce se data uložená v mezipaměti, uložte změny do souboru `ProductsCL` třídy a pak otevřete `FromTheArchitecture.aspx` stránku `Caching` složky a přidejte GridView. Rutina GridView s inteligentních značek vytvořte nové ObjectDataSource. V prvním kroku průvodce s byste měli vidět `ProductsCL` třídy jako jednu z možností z rozevíracího seznamu.


[![Třída ProductsCL je součástí obchodní objekt rozevíracího seznamu](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Obrázek 4**: `ProductsCL` třída je zahrnuta v rozevíracím seznamu objekt obchodní ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-in-the-architecture-cs/_static/image6.png))


Po výběru `ProductsCL`, klikněte na tlačítko Další. Rozevíracím seznamu v kartě vyberte má dvě položky - `GetProducts()` a `GetProductsByCategoryID(categoryID)` a na kartě aktualizace má jediný `UpdateProduct` přetížení. Vyberte `GetProducts()` metoda kartě vyberte a `UpdateProducts` metoda z karty aktualizace a klikněte na tlačítko Dokončit.


[![Metody třídy ProductsCL s jsou uvedeny v rozevírací seznamy](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Obrázek 5**: `ProductsCL` metody třídy s jsou uvedeny v rozevírací seznamy ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-in-the-architecture-cs/_static/image9.png))


Po dokončení průvodce, Visual Studio nastaví ObjectDataSource s `OldValuesParameterFormatString` vlastnost `original_{0}` a přidejte do GridView na odpovídající pole. Změna `OldValuesParameterFormatString` vlastnost zpět na výchozí hodnotu, `{0}`a nakonfigurujte GridView na podporu stránkování, řazení a úpravy. Vzhledem k tomu `UploadProducts` přetížení používané CL přijímá pouze název upravená produktu s a cenu omezit GridView tak, aby se upravovat pouze těchto polí.

V předchozím kurzu jsme definovali GridView pro zahrnutí polí pro `ProductName`, `CategoryName`, a `UnitPrice` pole. Nebojte se replikovat toto formátování a struktura, v takovém případě vaší GridView a ObjectDataSource s deklarativní značek by měl vypadat takto:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

V tuto chvíli nemáme stránky, která používá vrstvě ukládání do mezipaměti. Nastavte zarážky v mezipaměti v akci najdete `ProductsCL` třídu s `GetProducts()` a `UpdateProduct` metody. Navštivte stránku v prohlížeči a krok prostřednictvím kód při řazení a stránkování, chcete-li zobrazit data vyžádat z mezipaměti. Potom aktualizujte záznam a Upozorňujeme, že se zrušenou platností mezipaměti, a v důsledku toho načítají z BLL, když je k GridView odrážejí data.

> [!NOTE]
> Ukládání do mezipaměti vrstvy součástí stahování doplňujícími v tomto článku není dokončena. Obsahuje pouze jednu třídu, `ProductsCL`, který pouze sportovní několik metod. Kromě toho pouze jednu stránku ASP.NET používá CL (`~/Caching/FromTheArchitecture.aspx`) nikdo jiný k ní stále odkazovat BLL přímo. Pokud máte v úmyslu používat CL ve vaší aplikaci, všechna volání od prezentační vrstvy by měl přejděte do CL, která by vyžadovala CL s třídy a metody zahrnutých tyto třídy a metody v BLL aktuálně používané prezentační vrstvy.


## <a name="summary"></a>Souhrn

Při ukládání do mezipaměti můžete použít v prezentační vrstvě s prostředím ASP.NET 2.0 s SqlDataSource a ovládací prvky ObjectDataSource by v ideálním případě ukládání do mezipaměti odpovědnosti delegovat na samostatné vrstvy architektury. V tomto kurzu jsme vytvořili vrstva ukládání do mezipaměti, který se nachází mezi prezentační vrstvou a vrstvu obchodní logiky. Vrstva ukládání do mezipaměti je potřeba zadat stejnou sadu třídy a metody, které existují v BLL a se nazývají od prezentační vrstvy.

Ukládání do mezipaměti vrstvy příklady jsme prozkoumali v této a předchozích kurzy vystavených *reaktivní načítání*. S reakcí načítání je načíst data do mezipaměti jenom v případě požadavku pro data a data z mezipaměti chybí. Data mohou být také *proaktivně načíst* do mezipaměti techniky, načte data do mezipaměti předtím, než je skutečně potřeba. V dalším kurzu vidíte příklad proaktivní načítání, když se podíváme na tom, jak ukládat do mezipaměti při spuštění aplikace statické hodnoty.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Teresy Murph. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](caching-data-with-the-objectdatasource-cs.md)
> [další](caching-data-at-application-startup-cs.md)
