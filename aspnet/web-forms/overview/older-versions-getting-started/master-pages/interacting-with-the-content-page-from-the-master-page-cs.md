---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interakci s stránky obsahu ze stránky předlohy (C#) | Microsoft Docs
author: rick-anderson
description: Prozkoumá tom, jak volat metody, nastavte vlastnosti atd stránky obsahu z kódu na hlavní stránce.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c845c7b0077e6d3fb5ce770029b4f9f48609b17
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interakci s stránky obsahu ze stránky předlohy (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Prozkoumá tom, jak volat metody, nastavte vlastnosti atd stránky obsahu z kódu na hlavní stránce.


## <a name="introduction"></a>Úvod

Předchozí kurzu zkoumat jak vám má stránky obsahu prostřednictvím kódu programu interakci s jeho stránky předlohy. Odvolání, že byl aktualizován stránky předlohy GridView ovládací prvek, který pět uveden naposledy přidat produkty. Potom jsme vytvořili obsahu stránce, ze kterého uživatel může přidávat nového produktu. Při přidání nového produktu, stránky obsahu potřeba dáte pokyn, aby stránka předlohy a aktualizujte jeho GridView tak, aby ho by mělo zahrnovat právě přidané produktu. Tato funkce byla provést přidáním veřejnou metodu k hlavní stránce že aktualizovat data vázána na GridView, a pak volání této metody ze stránky obsahu.

Nejběžnější formu obsahu a interakce stránky předlohy pochází ze stránky obsahu. Nicméně je možné stránky předlohy pro rouse aktuální stránky obsahu do akce a takové funkce může být potřeba, pokud stránka předlohy obsahuje prvky uživatelského rozhraní, které umožňují uživatelům měnit data, která se také zobrazí na stránce obsahu. Vezměte v úvahu stránky obsahu, zobrazí informace o produkty v GridView řízení a hlavní stránky, který obsahuje tlačítko řídit, která při kliknutí na, zdvojnásobí ceny všech produktů. Podobně jako v příkladu v tomto kurzu předchozí GridView musí aktualizovat po dvojité cenu, kterou tak, aby zobrazila nové ceny po kliknutí na tlačítko, ale v tomto scénáři je vyžadující rouse stránky obsahu do akce stránky předlohy.

Tento tento kurz popisuje, jak vám má vyvolání funkce je definována v stránky obsahu stránky předlohy.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Pokus programové interakce prostřednictvím událost a obslužné rutiny událostí

Vyvolání funkce stránky obsahu ze stránky předlohy je další náročné než opačným způsobem. Protože obsahu stránce má jednu stránku předlohy, když pokus programové interakci ze stránky obsahu víme veřejné metody a vlastnosti jsou naše k dispozici. Na hlavní stránce, ale může mít mnoho různých obsahu stránky, každou s vlastní sadu vlastností a metod. Jak potom můžete jsme psát kód na hlavní stránce provádět některé akce na stránce s jejím obsahu, když nevíme, jaké stránky obsahu, který bude vyvolán dokud runtime?

Vezměte v úvahu ovládací prvek ASP.NET Web, jako je například ovládacího prvku tlačítko. Ovládací prvek tlačítko se objeví na libovolný počet stránek ASP.NET a potřebuje mechanismus, pomocí kterého ho může upozornit, stránky, aby bylo stisknuto. Toho dosahuje pomocí *události*. Konkrétně ovládacího prvku tlačítko vyvolá jeho `Click` událostí po klepnutí; stránky ASP.NET, která obsahuje tlačítko můžete volitelně reagovat na tomto oznámení prostřednictvím *obslužné rutiny události*.

Tento stejný vzor umožňuje mít je aktivační událost funkce stránky předlohy v jeho obsahu stránky:

1. Přidání události k hlavní stránce.
2. Vyvolejte událost vždy, když je komunikovat s jeho stránky obsahu stránky předlohy. Například pokud stránky předlohy potřebuje obsahu stránce s jejím upozornění, že uživatel má se používají tyto ceny, jeho události by vydána ihned po ceny byl se používají.
3. Vytvoření obslužné rutiny události v těchto obsahu stránek, které vyžadují určitou akci.

Tento zbývající část tohoto kurzu implementuje uvedené v úvodu; stránky obsahu, která jsou uvedeny produkty, v databázi a hlavní stránky, který obsahuje tlačítko konkrétně, řídit poklikejte ceny.

## <a name="step-1-displaying-products-in-a-content-page"></a>Krok 1: Zobrazení produkty obsahu stránce

Naše první of obchodní pořadí je vytvoření stránky obsahu, která jsou uvedeny produkty z databáze Northwind. (Databázi Northwind do projektu jsme přidali v předchozím kurzu [ *interakci se stránkou předlohy ze stránky obsahu*](interacting-with-the-master-page-from-the-content-page-cs.md).) Začněte přidáním nové stránky ASP.NET do `~/Admin` složku s názvem `Products.aspx`, které opravdu pro vytvoření vazby na `Site.master` stránky předlohy. Obrázek 1 zobrazuje Průzkumník řešení po přidání této stránky na web.


[![Přidat novou stránku ASP.NET ke složce Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Obrázek 01**: Přidat novou stránku ASP.NET do `Admin` složky ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Odvolat, že [ *zadáte název, značky Meta a ostatní hlavičky HTML na hlavní stránce* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní základní stránky třídy s názvem `BasePage` který generuje nadpis stránky, pokud není explicitně nastaven. Přejděte na `Products.aspx` kódu stránky třídy a ji odvodit z `BasePage` (místo z `System.Web.UI.Page`).

Nakonec aktualizujte `Web.sitemap` souboru záznam pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro obsah na hlavní stránce interakce lekce:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

Přidání tohoto `<siteMapNode>` element se odrazí v poznatky seznamu (viz obrázek 5).

Vraťte se do `Products.aspx`. V ovládacím prvku obsahu pro `MainContent`, přidání ovládacího prvku GridView a pojmenujte ji `ProductsGrid`. Vytvoření vazby GridView do ovládacího prvku nové SqlDataSource s názvem `ProductsDataSource`.


[![Vytvoření vazby GridView do ovládacího prvku nové SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Obrázek 02**: vazby GridView do ovládacího prvku nové SqlDataSource ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Nakonfigurujte průvodce tak, aby používala databázi Northwind. Pokud jste pracovali prostřednictvím předchozí kurz, pak byste již měli mít připojovací řetězec s názvem `NorthwindConnectionString` v `Web.config`. V rozevíracím seznamu vyberte tento připojovací řetězec, jak je znázorněno na obrázku 3.


[![Konfigurace SqlDataSource k použití databáze Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Obrázek 03**: Konfigurace SqlDataSource chcete použít databázi Northwind ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


Potom zadejte ovládacího prvku zdroje dat `SELECT` příkaz zvolením tabulky produktů z rozevíracího seznamu a vrací `ProductName` a `UnitPrice` sloupce (viz obrázek 4). Klikněte na tlačítko Další a pak dokončete průvodce Konfigurace zdroje dat dokončit.


[![Vrátí pole UnitPrice a ProductName z tabulky produktů](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Obrázek 04**: vrátit `ProductName` a `UnitPrice` pole z `Products` tabulky ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


To je všechno je k němu! Visual Studio po dokončení průvodce přidá dva BoundFields GridView pro dvě pole vrácené ovládacího prvku SqlDataSource zrcadlení. Rutina GridView a SqlDataSource prvky značek následuje. Obrázek 5 ukazuje výsledky při zobrazení prostřednictvím prohlížeče.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Každý produkt a jeho cena je uvedena v prvku GridView.](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Obrázek 05**: každý produkt a jeho cena je uvedena v prvku GridView ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> Nebojte se, že vyčištění vzhled GridView. Některé návrhy patří formátování zobrazená hodnota UnitPrice jako měny a používání písma a barvy pozadí k vylepšení vzhledu mřížky. Další informace o zobrazení a formátování dat v technologii ASP.NET, najdete v části Moje [práce s kurz datové řady](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Krok 2: Přidání tlačítka dvojitý ceny na hlavní stránku

Naše dalším krokem je přidání ovládacího prvku tlačítko webové k hlavní stránky, která po kliknutí na bude double cenu všechny produkty v databázi. Otevřete `Site.master` hlavní stránky a přetáhněte ji na tlačítko z panelu nástrojů na návrháře pod jeho umístění `RecentProductsDataSource` prvek SqlDataSource jsme přidali v předchozím kurzu. Nastavte na tlačítko `ID` vlastnost `DoublePrice` a jeho `Text` vlastnost "Dvojité ceny produktů".

Dál přidejte ovládacího prvku SqlDataSource na hlavní stránku pojmenování ho `DoublePricesDataSource`. Tato SqlDataSource se použít ke spuštění `UPDATE` příkaz zdvojnásobit všechny ceny. Konkrétně je potřeba nastavit jeho `ConnectionString` a `UpdateCommand` vlastnosti odpovídající připojovací řetězec a `UPDATE` příkaz. Pak je potřeba volat tento ovládací prvek SqlDataSource `Update` metoda při `DoublePrice` po kliknutí na tlačítko. Chcete-li nastavit `ConnectionString` a `UpdateCommand` vlastnosti, vyberte ovládací prvek SqlDataSource a potom přejděte do okna vlastností. `ConnectionString` Těchto připojovací řetězce, který je už uložený v seznamech vlastností `Web.config` v rozevíracím seznamu; zvolte `NorthwindConnectionString` možnost, jak je znázorněno na obrázku 6.


[![Konfigurace SqlDataSource používat NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Obrázek 06**: Konfigurace SqlDataSource pro použití `NorthwindConnectionString` ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Chcete-li nastavit `UpdateCommand` vlastnost, vyhledejte možnost UpdateQuery v okně Vlastnosti. Tato vlastnost, pokud vybraná, zobrazí tlačítko se třemi tečkami; Kliknutím na toto tlačítko zobrazit dialogové okno Editor kolekce parametrů vidět na obrázku 7. Zadejte následující příkaz `UPDATE` příkaz do textového pole dialogových oken:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Tento příkaz při spuštění, bude dvakrát `UnitPrice` hodnotu pro každý záznam v `Products` tabulky.


[![Nastavte vlastnost UpdateCommand SqlDataSource společnosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Obrázek 07**: nastavte SqlDataSource `UpdateCommand` vlastnost ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Po nastavení těchto vlastností, deklarativní vaší a SqlDataSource ovládacích prvků by měl vypadat takto:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Zbývá volat jeho `Update` metoda při `DoublePrice` po kliknutí na tlačítko. Vytvoření `Click` obslužné rutiny události pro `DoublePrice` tlačítko a přidejte následující kód:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Pokud chcete vyzkoušet tuto funkci, navštivte `~/Admin/Products.aspx` stránky jsme vytvořili v kroku 1 a klikněte na tlačítko "Dvojité ceny produktů". Kliknutím na tlačítko způsobí, že zpětné volání a provede `DoublePrice` tlačítka `Click` obslužné rutiny události, zvýší ceny všech produktů. Pak znovu vykreslení stránky a kód je vrátí a znovu se zobrazí v prohlížeči. Rutina GridView obsahu stránce, ale uvádí stejné ceny jako před "Dvojité produktu ceny" bylo stisknuto tlačítko. Je to proto, že data původně načten do GridView měla stavu uložené v zobrazení stavu, takže není znovu na postback nevydá. Pokud navštívíte na jinou stránku a pak se vraťte do `~/Admin/Products.aspx` stránky se zobrazí aktualizovaná ceny.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Krok 3: Vyvolávání událostí události při the ceny jsou se používají.

Protože rutina GridView v `~/Admin/Products.aspx` stránky nezohledňuje okamžitě zvýší ceny, uživatel může understandably myslíte, že není klikněte na tlačítko "Dvojité ceny produktů" nebo že nefungovala. Se mohou zkuste klepnutím na tlačítko několik dalších krát, zvýší se ceny znovu a znovu. Chcete-li tomu je potřeba mít mřížky v obsahu stránce zobrazit nové ceny ihned po jejich zdvojnásobí.

Jak je popsáno výše v tomto kurzu, musíme aktivuje událost na hlavní stránce vždy, když uživatel klikne `DoublePrice` tlačítko. Událost je způsob, jak jednu třídu (vydavatel události) oznámit jiné sadu jiné třídy (odběratele událostí), které něčeho zajímavého došlo k chybě. V tomto příkladu je stránka předlohy vydavatel události; ty obsahu stránky, které jsou pro vás o při zpracování `DoublePrice` po kliknutí na tlačítko se odběratele.

Třída jako odběratel u událost vytvořením *obslužné rutiny události*, což je metoda, která se spustí v reakci na událost se vyvolá. Vydavatel definuje události, kterou vyvolá definováním *delegát události*. Delegát události Určuje, jaké vstupní parametry obslužné rutiny události musí přijmout. V rozhraní .NET Framework – delegáti událostí není vrátí všechny hodnoty a přijmout dva vstupní parametry:

- `Object`, Který identifikuje zdroji událostí a
- Třídy odvozené od `System.EventArgs`

Druhý parametr předaný obslužné rutiny události může obsahovat další informace o události. Při základní `EventArgs` třída nepředává podél žádné informace, rozhraní .NET Framework zahrnuje několik tříd, které rozšiřují `EventArgs` a zahrnovat další vlastnosti. Například `CommandEventArgs` instance předána obslužné rutiny událostí, které reagují na `Command` události a zahrnuje dvě informační vlastnosti: `CommandArgument` a `CommandName`.

> [!NOTE]
> Další informace o vytváření vyvolání a zpracování událostí, najdete v části [události a delegáti](https://msdn.microsoft.com/library/17sde2xt.aspx) a [– delegáti událostí v jednoduchých anglické](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Chcete-li definovat událost použijte následující syntaxi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Protože je potřeba upozornit stránky obsahu, když uživatel klikne `DoublePrice` tlačítko a není potřeba předají jakýchkoli dalších informací, můžeme použít delegát události `EventHandler`, která definuje obslužnou rutinu události, který přijímá jako jeho parametr objekt typu `System.EventArgs`. Pokud chcete vytvořit událost na hlavní stránce, přidejte následující řádek kódu do třídy kódu stránky předlohy:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Výše uvedený kód přidá veřejnou událost na hlavní stránku s názvem `PricesDoubled`. Nyní potřebujeme Vyvolejte tuto událost po ceny byl se používají. Umožňuje aktivovat události použijte následující syntaxi:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Kde *odesílatele* a *eventArgs* jsou hodnoty chcete předat obslužné rutiny události odběratele.

Aktualizace `DoublePrice` `Click` obslužné rutiny události s následujícím kódem:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Jako předtím `Click` obslužné rutiny události spustí voláním `DoublePricesDataSource` prvek SqlDataSource `Update` metoda zdvojnásobit ceny všech produktů. Následující, že existují dvě dodatky k obslužné rutině událostí. Nejdřív `RecentProducts` GridView data se aktualizují. Tato rutina GridView byl přidán k hlavní stránce v předchozím kurzu a zobrazí pět nejvíce nedávno přidané produkty. Je potřeba aktualizovat mřížce tak, aby se zobrazí ceny se právě používají pro tyto produkty pět. Následující, která `PricesDoubled` událost se vyvolá. Odkaz na hlavní stránce samotné (`this`) se odešle do obslužné rutiny události jako zdroje událostí a prázdnou `EventArgs` objekt se odešle jako argumenty událostí.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Krok 4: Zpracování událostí v stránky obsahu

V tomto okamžiku stránky předlohy vyvolá jeho `PricesDoubled` událost vždy, když `DoublePrice` po kliknutí na tlačítko – ovládací prvek. Ale toto je pouze poloviční boj – musíme zpracovat událost v odběrateli. To zahrnuje dva kroky: vytvoření obslužné rutiny události a přidání kód kabeláž události tak, aby při vyvolání události se spustí obslužné rutiny události.

Začněte vytvořením obslužné rutiny události s názvem `Master_PricesDoubled`. Z důvodu jak jsme definovali `PricesDoubled` událostí na hlavní stránce dvou vstupních parametrů obslužné rutiny události musí být typu `Object` a `EventArgs`, v uvedeném pořadí. Ve volání obslužné rutiny událostí `ProductsGrid` GridView `DataBind` metoda se svázat data k mřížce.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Kód pro obslužnou rutinu události dokončení, ale zatím jsme na propojit stránky předlohy `PricesDoubled` událost, která má této obslužné rutiny události. Odběratel vedení událost vodiče na obslužnou rutinu události pomocí následující syntaxe:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*Vydavatel* je odkaz na objekt, který nabízí událost *eventName*, a *methodName* je název obslužné rutiny události definované v odběrateli, který má odpovídající podpis *eventDelegate*. Jinými slovy, pokud událost delegovat je `EventHandler`, pak *methodName* musí být název metody v odběrateli, která nevrátí hodnotu a přijímá dva vstupní parametry typů `Object` a `EventArgs`, v uvedeném pořadí.

Tento kód kabeláž událostí je třeba spustit na první návštěvě stránky a následné zpětná vystavení a má probíhat na bod v průběhu životního cyklu stránky, předcházejícího při událost může být vyvolána. Ve fázi PreInit, který probíhá velmi brzy v životním cyklu stránky je vhodná doba přidat kód kabeláž události.

Otevřete `~/Admin/Products.aspx` a vytvořte `Page_PreInit` obslužné rutiny události:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Chcete-li dokončit tento kód kabeláž potřebujeme programový odkaz na hlavní stránce ze stránky obsahu. Jak jsme uvedli v předchozím kurzu, existují dva způsoby, jak to udělat:

- Pomocí přetypování volného typu `Page.Master` vlastnost, která má typ příslušné stránky předlohy nebo
- Přidáním `@MasterType` direktivy v `.aspx` stránku a pak pomocí silného typu `Master` vlastnost.

Umožňuje použít pozdější přístup. Přidejte následující `@MasterType` direktivy do horní části stránky deklarativní značky:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Přidejte následující kód kabeláž událostí v `Page_PreInit` obslužné rutiny události:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

S tímto kódem na místě, se aktualizují GridView obsahu stránce vždy, když `DoublePrice` po kliknutí na tlačítko.

Toto chování je znázorněn 8 obrázky a 9. Obrázek 8 ukazuje na stránku při první navštívené. Všimněte si, že cena hodnoty v obou `RecentProducts` GridView (v levém sloupci stránky předlohy) a `ProductsGrid` GridView (v obsahu stránce). Obrázek 9 ukazuje stejné obrazovce ihned po `DoublePrice` bylo stisknuto tlačítko. Jak vidíte, nové ceny se okamžitě projeví v obou GridViews.


[![Počáteční cena hodnoty](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Obrázek 08**: počáteční hodnoty cen ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![Ceny Just-Doubled se zobrazují v GridViews](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Obrázek 09**: The Just-Doubled ceny jsou zobrazeny v GridViews ([Kliknutím zobrazit obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Souhrn

V ideálním případě stránky předlohy a stránky jeho obsahu jsou zcela oddělené od sebe navzájem a nevyžadují žádné úrovně interakce. Ale pokud máte na hlavní stránku nebo stránky obsahu, která zobrazí data, která je možné upravovat na hlavní stránku nebo stránky obsahu, pak může musíte mít stránky předlohy upozornění obsahu stránce (nebo naopak) změny dat, aby bylo možné aktualizovat zobrazení. V předchozím kurzu jsme viděli, jak mají stránky obsahu prostřednictvím kódu programu interakci s jeho stránky předlohy; v tomto kurzu jsme se podívali na postup mít spuštění stránky předlohy interakce.

Při programové interakce mezi obsah a hlavní stránce můžete pocházejí z obsah nebo stránky předlohy, vzoru interakce použít závisí na místem původu. Rozdíly jsou vzhledem k tomu, že obsahu stránce má jednu stránku předlohy, ale na hlavní stránce může mít mnoho různých obsahu stránky. Místo na hlavní stránce přímo komunikovat s stránky obsahu, je vhodnější se se vyvolat událost signál, že některé akce proběhla stránky předlohy. Obslužné rutiny událostí můžete vytvořit tyto stránky obsahu, které jsou pro vás o akci.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přístup k informacím a aktualizace dat v technologii ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Události a delegáti](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Předávání informací mezi obsah a stránky předlohy](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Práce s daty v kurzech ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott lze dosáhnout za [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Suchi Banerjee. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](interacting-with-the-master-page-from-the-content-page-cs.md)
> [další](master-pages-and-asp-net-ajax-cs.md)
