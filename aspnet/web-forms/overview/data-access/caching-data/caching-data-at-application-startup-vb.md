---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: "Ukládání do mezipaměti dat při spuštění aplikace (VB) | Microsoft Docs"
author: rick-anderson
description: "Všechny webové aplikace se některá data často používat a některá data se zřídka použije. Můžeme vylepšit výkon naše b aplikace ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b84b797bf0c9670ac65a5384b6d95d5df3827eb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-at-application-startup-vb"></a>Ukládání dat do mezipaměti při spuštění aplikace (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> Všechny webové aplikace se některá data často používat a některá data se zřídka použije. Načítání dat používaných techniku známou jako předem jsme lze vylepšit výkon naše aplikace ASP.NET. Tento kurz představuje jeden ze způsobů proaktivní načítání, což je načíst data do mezipaměti při spuštění aplikace.


## <a name="introduction"></a>Úvod

Dva předchozí kurzy prohlédli ukládání dat v prezentace a vrstev ukládání do mezipaměti. V [ukládání dat pomocí ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), jsme se podívali na používání s ObjectDataSource ukládání do mezipaměti funkce, které chcete data do mezipaměti v prezentační vrstvě. [Ukládání dat v architektuře](caching-data-in-the-architecture-vb.md) zkontrolován ukládání do mezipaměti v nové, samostatné vrstvy ukládání do mezipaměti. Obě tyto kurzy použít *reaktivní načítání* při práci s mezipamětí data. Při přepnutí do reaktivního načítání pokaždé, když je požadovaná data, systém nejprve ověří, zda ji s v mezipaměti. Pokud ne, získá data z původního zdroje, například databáze a ukládá je v mezipaměti. Hlavní výhodou reaktivní načítání je jeho snadnou implementaci. Jeden z jeho nevýhody je jeho nerovnoměrné výkon napříč požadavky. Představte si stránky, která používá vrstvě ukládání do mezipaměti z předchozí kurzu k zobrazení informací o produktu. Při této stránky navštívili poprvé nebo navštívené poprvé po data uložená v mezipaměti byl odstraněn z důvodu omezení paměti nebo zadaný vypršení platnosti, že bylo dosaženo, musí být data načíst z databáze. Tyto požadavky uživatelů proto bude trvat déle než uživatelé požadavků, které se dají obsluhovat mezipamětí.

*Proaktivní načítání* poskytuje strategie správy alternativní mezipaměti této vyhlazuje výkon napříč požadavky načtením s potřeby data uložená v mezipaměti před ním. Obvykle proaktivní načítání používá některé proces, který buď pravidelně kontroluje nebo zasláno oznámení, když byla aktualizace v základních datech. Tento proces potom aktualizuje mezipaměť zachovat novou. Proaktivní načítání je obzvláště užitečné, pokud základní data pocházejí z připojení pomalé databáze, webová služba nebo z jiného zdroje dat zejména pomalá. Ale tento přístup k proaktivní načítání je obtížné implementovat, protože vyžaduje vytvoření, správě a nasazení procesu kontrolovat změny a aktualizace mezipaměti.

Jiné příchuť proaktivní načítání a typ, který jsme budete seznamovat v tomto kurzu je načítání dat do mezipaměti při spuštění aplikace. Tento přístup je zvláště užitečná pro ukládání do mezipaměti statických dat, jako je například záznamy v databázi vyhledávací tabulky.

> [!NOTE]
> Více hlubší pohled na rozdíly mezi proaktivní a reaktivní načítání, jakož i seznam výhody, nevýhody a implementace doporučení, najdete v části [správu jeho obsahu mezipaměti](https://msdn.microsoft.com/library/ms978503.aspx) části [ Ukládání do mezipaměti Průvodce architekturou pro aplikace .NET Framework](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Krok 1: Určení, jaká Data do mezipaměti při spuštění aplikace

Ukládání do mezipaměti příklady použití reaktivní načítání jsme se zaměřili na předchozí dva kurzy činnosti i s daty, která může pravidelně změnit a nevyžaduje exorbitantly dlouho ke generování. Ale pokud se data uložená v mezipaměti nikdy změní, vypršení platnosti používané reaktivní načítání je nadbytečné. Podobně pokud data ukládat do mezipaměti trvá generování bylo nepřiměřeně dlouho, pak uživatelům, jejichž požadavky najít prázdný mezipaměti bude mít k prosazování zdlouhavé čekání při podkladová data načte. Vezměte v úvahu ukládání statických dat a data, která trvá velmi dlouho generovat při spuštění aplikace.

Když databáze má mnoho dynamické, často změna hodnoty většinu také mají správného množství statických dat. Například prakticky všechny datové modely mít jeden nebo více sloupců, které obsahují konkrétní hodnotu z dlouhodobého ze sady možností. A `Patients` může mít tabulku databáze `PrimaryLanguage` sloupec, jehož sadu hodnot může být angličtina, španělština, francouzština, ruština, japonštiny a tak dále. Často, jsou tyto typy sloupců implementovaná pomocí *vyhledávací tabulky*. Místo uložení řetězce angličtina nebo Francouzština v `Patients` tabulky, v druhé tabulce je vytvořen, který běžně, obsahuje dva sloupce – jedinečný identifikátor a popis řetězec - s záznam pro každý možná hodnota. `PrimaryLanguage` Sloupec v `Patients` tabulky ukládá odpovídající jedinečný identifikátor ve vyhledávací tabulce. Na obrázku 1 pacienta John Doe s primární jazyk je angličtina, při ruština Ed Janíček s.


![V tabulce jazyky je použít tabulku vyhledávání v tabulce pacientů](caching-data-at-application-startup-vb/_static/image1.png)

**Obrázek 1**: `Languages` tabulka je vyhledávací tabulky používané `Patients` tabulky


Uživatelské rozhraní pro úpravu nebo vytváření pacienta by mělo zahrnovat rozevírací seznam povolených jazyků nenaplnil záznamy v `Languages` tabulky. Bez ukládání do mezipaměti, navštívené pokaždé, když toto rozhraní je v systému musí zadat dotaz `Languages` tabulky. Toto je plýtvání a nepotřebné od hodnoty vyhledávací tabulky změn velmi zřídka, pokud někdy.

Jsme může ukládat do mezipaměti `Languages` dat pomocí se stejné techniky reaktivní načítání zkontrolován v předchozí kurzy. Přepnutí do reaktivního načítání, ale používá vypršení založené na čase, který není nutný pro statické vyhledávání dat v tabulce. Při ukládání do mezipaměti pomocí reaktivní načítání by lepší, než žádné ukládání do mezipaměti na všech, je nejlepším postupem by mohla být proaktivně načíst data tabulky vyhledávání do mezipaměti při spuštění aplikace.

V tomto kurzu se podíváme na postup dat v tabulce mezipaměti vyhledávání a dalších statických informací.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Krok 2: Prozkoumání různé způsoby, jak ukládat Data do mezipaměti

Informace můžete programově do mezipaměti v aplikaci ASP.NET pomocí různé přístupy. Jsme jste už viděli, jak použít datovou mezipaměť v předchozí kurzy. Alternativně objekty prostřednictvím kódu programu do mezipaměti pomocí *statické členy* nebo *stav aplikace*.

Při práci s třídou, obvykle musíte nejprve vytvořit instanci třídy můžete získat přístup k její členy. Například k vyvolání metody jednu z tříd v našem vrstvu obchodní logiky, jsme musíte nejprve vytvořit instance třídy:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Předtím, než můžete vyvolat jsme *SomeMethod* nebo pracovat s *SomeProperty*, jsme musíte nejdřív vytvořit instanci třídy pomocí `New` – klíčové slovo. *SomeMethod* a *SomeProperty* jsou přidruženy ke konkrétní instanci. Doba platnosti těchto členů je vázaný na životnost jejich přidruženého objektu. *Statické členy*, na druhé straně jsou proměnné, vlastnosti a metody, které jsou sdíleny mezi *všechny* instancí třídy a v důsledku toho mají stejně dlouho jako třída životnost. Statické členy jsou rozlišeny pomocí klíčového slova `Shared`.

Kromě statické členy data do mezipaměti pomocí stav aplikace. Každá aplikace technologie ASP.NET udržuje kolekci název hodnota této s sdílet všechny uživatele a stránky aplikace. Tuto kolekci lze přistupovat pomocí [ `HttpContext` třída](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` vlastnost](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)a použít z třídy kódu stránky s ASP.NET takto:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

Mezipaměť dat poskytuje mnohem širší rozhraní API pro ukládání do mezipaměti dat, při kterém mechanismy pro expiries založená na čas a závislostí, priority položky mezipaměti a tak dále. Statické členy a stavu aplikace je nutné tyto funkce ručně přidat vývojářem stránky. Při ukládání dat při spuštění aplikace do mezipaměti po dobu jeho existence aplikace, ale výhody mezipaměti s data jsou moot. V tomto kurzu budeme zabývat kód, který používá všechny tři techniky pro ukládání statických dat do mezipaměti.

## <a name="step-3-caching-thesupplierstable-data"></a>Krok 3: Ukládání do mezipaměti`Suppliers`tabulky dat

Northwind databáze jsme tabulky sunout implementována do data neobsahují žádné tradiční vyhledávací tabulky. Čtyři DataTables implementované v našem DAL všechny tabulky modelu, jejichž hodnoty jsou nestatické. Místo ztrácet čas přidejte nový element DataTable DAL nové třídy a metody pro BLL, pro tento kurz umožní s právě předstírají, že, `Suppliers` dat v tabulce s je statický. Proto jsme může mezipaměti tato data při spuštění aplikace.

Pokud chcete spustit, vytvořte novou třídu s názvem `StaticCache.cs` v `CL` složky.


![Vytvořte třídu StaticCache.vb ve složce CL](caching-data-at-application-startup-vb/_static/image2.png)

**Obrázek 2**: vytvoření `StaticCache.vb` třídy v `CL` složky


Je potřeba přidat metodu, která načte data na spuštění do úložiště příslušné mezipaměti, jakož i metody, které vrací data z této mezipaměti.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Výše uvedený kód používá proměnnou statický člen `suppliers`, k ukládání výsledků `SuppliersBLL` třídu s `GetSuppliers()` metodu, která je volána z `LoadStaticCache()` metoda. `LoadStaticCache()` Metoda je určená k volání při spuštění aplikace s. Jakmile tato data se načetl při spuštění aplikace, můžete volat všechny stránky, která musí pracovat s daty dodavatele `StaticCache` třídu s `GetSuppliers()` metoda. Proto volání do databáze s cílem získat dodavatelů se dělá jenom jednou, při spuštění aplikace.

Místo pomocí statické členské proměnné jako mezipaměť, může mít případně použili jsme stav aplikace nebo datové mezipaměti. Následující kód ukazuje třídu retooled používat stav aplikace:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

V `LoadStaticCache()`, informace o dodavateli je uložit do proměnné aplikace *klíč*. Ho s vrácena jako odpovídající typ (`Northwind.SuppliersDataTable`) z `GetSuppliers()`. Když se stav aplikace je přístupná ve třídách modelu code-behind stránek ASP.NET pomocí `Application("key")`, v architektuře, která musí používáme `HttpContext.Current.Application("key")` zajistí, že aktuální `HttpContext`.

Mezipaměť dat, lze použít jako úložiště mezipaměti, jak ukazuje následující kód:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Chcete-li přidat položku do mezipaměti dat bez vypršení platnosti založené na čase, použijte `System.Web.Caching.Cache.NoAbsoluteExpiration` a `System.Web.Caching.Cache.NoSlidingExpiration` hodnoty jako vstupní parametry. Toto konkrétní přetížení datovou mezipaměť s `Insert` metoda nebyla vybrána, aby bychom mohli zadat *s prioritou* položky mezipaměti. Priorita slouží k položky, které uklizeny z mezipaměti, když nedostatek dostupné paměti. Tady používáme prioritu `NotRemovable`, což zajistí, že tuto položku mezipaměti won t úklid.

> [!NOTE]
> Tento kurz s stažení implementuje `StaticCache` pomocí proměnné přístup statický člen. Kód pro techniky mezipaměti stav a data aplikace je k dispozici v komentářích v souboru třídy.


## <a name="step-4-executing-code-at-application-startup"></a>Krok 4: Spuštění kódu při spuštění aplikace

Spuštění kódu při prvním spuštění webové aplikace, je potřeba vytvořit speciální soubor s názvem `Global.asax`. Tento soubor může obsahovat obslužné rutiny události pro aplikace-, relace- a události na úrovni požadavků a je zde kde nyní můžete přidat kód, který bude proveden při každém spuštění aplikace.

Přidat `Global.asax` soubor do vaší webové aplikace s kořenový adresář pravým tlačítkem myši na název projektu webu v sadě Visual Studio s Průzkumníku řešení a zvolením přidat novou položku. Z dialogového okna Přidat novou položku vyberte typ položky globální třídy aplikace a potom klikněte na tlačítko Přidat.

> [!NOTE]
> Pokud již máte `Global.asax` souboru v projektu, globální třídy aplikace typ položky nebude v seznamu uveden v dialogovém okně Přidat novou položku.


[![Soubor Global.asax přidat do vaší webové aplikace s kořenový adresář](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Obrázek 3**: Přidat `Global.asax` soubor pro vaše webové aplikace s kořenový adresář ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-at-application-startup-vb/_static/image5.png))


Výchozí hodnota `Global.asax` soubor šablony obsahuje pět metod v rámci straně serveru `<script>` značky:

- **`Application_Start`**spustí při prvním spuštění webové aplikace
- **`Application_End`**spustí, když se aplikace vypíná.
- **`Application_Error`**provede vždy, když aplikace dosáhne k neošetřené výjimce
- **`Session_Start`**provede, když je vytvořena nová relace
- **`Session_End`**spustí, když je relace vypršela platnost, nebo opuštění

`Application_Start` Obslužné rutiny události je jenom jednou volána v průběhu životního cyklu aplikace s. Spuštění aplikace při prvním prostředek ASP.NET se požaduje od aplikace a nadále spustit až po restartování aplikace, které může dojít, změně obsahu `/Bin` složky, úprava `Global.asax`, změny obsah v `App_Code` složky nebo změny `Web.config` souboru mezi další příčiny. Odkazovat na [Přehled životního cyklu aplikace ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) podrobnější diskuzi o životním cyklu aplikace.

Tyto kurzy musíme pouze přidejte kód, který `Application_Start` metoda, takže chování volné odeberte ostatní. V `Application_Start`, stačí zavolat `StaticCache` třídu s `LoadStaticCache()` metodu, která načte a dodavatele informace v mezipaměti:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Že všechny existuje s je k němu! Při spuštění aplikace `LoadStaticCache()` metoda získat informace o dodavateli z BLL a uložte ho statické členské proměnné (nebo libovolnou mezipaměti můžete ukládat skončila použití v `StaticCache` třídy). Pokud chcete ověřit, toto chování, nastavte zarážky v `Application_Start` metoda a spusťte aplikaci. Všimněte si, že je při spuštění aplikace průchodu zarážkou. Následné žádosti, ale nezpůsobí `Application_Start` metodu provést.


[![Použít zarážku na ověřte, zda obslužná rutina události Application_Start je spouštěna](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Obrázek 4**: použití zarážek na ověřte, který `Application_Start` obslužné rutiny události je spouštěna ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Pokud není dosáhl `Application_Start` zarážek při prvním spuštění ladění, je proto aplikaci již bylo zahájeno. Vynutit restartování úpravou aplikace vaše `Global.asax` nebo `Web.config` soubory a potom akci opakujte. Můžete jednoduše přidat (nebo odeberte) prázdný řádek na konci jeden z těchto souborů rychle restartování aplikace.


## <a name="step-5-displaying-the-cached-data"></a>Krok 5: Zobrazení Data uložená v mezipaměti

V tomto okamžiku `StaticCache` třída má verzi dodavatele dat do mezipaměti při spuštění aplikace, která je přístupná přes jeho `GetSuppliers()` metoda. Pro práci s těmito daty z prezentační vrstvy, můžeme používat ObjectDataSource nebo prostřednictvím kódu programu vyvolání `StaticCache` třídu s `GetSuppliers()` metoda z třídy kódu stránky s technologie ASP.NET. Umožní s, podívejte se na používání ObjectDataSource a GridView ovládacích prvků pro zobrazení informací v mezipaměti dodavatele.

Začněte otevřením `AtApplicationStartup.aspx` stránku `Caching` složky. Přetáhněte GridView z panelu nástrojů na návrháře nastavení jeho `ID` vlastnost `Suppliers`. V dalším kroku z GridView s inteligentním můžete vytvořit nové ObjectDataSource s názvem `SuppliersCachedDataSource`. Konfigurace ObjectDataSource používat `StaticCache` třídu s `GetSuppliers()` metoda.


[![Konfigurace ObjectDataSource použití třídy StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Obrázek 5**: Konfigurace ObjectDataSource používat `StaticCache` – třída ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-at-application-startup-vb/_static/image11.png))


[![Pomocí této metody GetSuppliers() načíst Data uložená v mezipaměti dodavatele](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Obrázek 6**: použití `GetSuppliers()` metoda pro načtení dat do mezipaměti dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-at-application-startup-vb/_static/image14.png))


Po dokončení průvodce, Visual Studio automaticky přidá BoundFields pro každé datové pole v `SuppliersDataTable`. Vaše GridView a ObjectDataSource s deklarativní značek by měl vypadat takto:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Na obrázku 7 vidíte stránce zobrazí prostřednictvím prohlížeče. Výstup je stejný měl jsme vyžádat data z BLL s `SuppliersBLL` třídy, ale pomocí `StaticCache` třída vrací data dodavatele jako při spuštění aplikace v mezipaměti. Nastavte zarážky v `StaticCache` třídu s `GetSuppliers()` metodu k ověření, toto chování.


[![V GridView se zobrazí dodavatele Data do mezipaměti](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Obrázek 7**: v GridView se zobrazí dodavatele Data do mezipaměti ([Kliknutím zobrazit obrázek v plné velikosti](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Souhrn

Většina každý datový model obsahuje správného množství statických dat, obvykle implementována ve formě vyhledávací tabulky. Vzhledem k tomu, že tyto informace jsou statické, zde s žádný důvod pro průběžně přístup k databázi pokaždé, když tyto informace se mají zobrazit. Kromě toho z důvodu jeho statické povahy, při ukládání do mezipaměti data zde s nemusí vypršela platnost. V tomto kurzu jsme viděli, jak provést taková data a uložení do mezipaměti v datové mezipaměti, stav aplikace a prostřednictvím statické členské proměnné. Tyto informace se uloží do mezipaměti při spuštění aplikace a zůstává v mezipaměti po celou dobu s aplikací.

V tomto kurzu a posledních dvou jsme sunout hledá v ukládání dat do mezipaměti po dobu trvání s životnost aplikace a také pomocí expiries založené na čase. Při ukládání do mezipaměti data databáze, ale vypršení platnosti založené na čase může být menší než ideální. Místo pravidelně abyste vyprázdnili mezipaměť, je optimální pouze vyřazení položka v mezipaměti při změně podkladového dat databáze. Tento ideální je možné prostřednictvím závislosti mezipaměti SQL, které podíváme v našem kurzu Další.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Teresy Murphy a Zack Petr. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](caching-data-in-the-architecture-vb.md)
[další](using-sql-cache-dependencies-vb.md)
