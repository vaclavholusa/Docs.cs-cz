---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Zkoumání událostí spojených s vložením, aktualizací a odstraněním (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu, kterou prozkoumáme použití událostí, ke kterým dochází před, během a po vložení aktualizaci nebo odstranění operace datům v technologii ASP.NET ovládací prvek webu. W....
ms.author: riande
ms.date: 07/17/2006
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: d8a16500388acd331042b7a9d62cf710edf3c61a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752436"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Zkoumání událostí spojených s vložením, aktualizací a odstraněním (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) nebo [stahovat PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> V tomto kurzu, kterou prozkoumáme použití událostí, ke kterým dochází před, během a po vložení aktualizaci nebo odstranění operace datům v technologii ASP.NET ovládací prvek webu. Také jsme zobrazí přizpůsobení úpravy rozhraní aktualizovat pouze podmnožinu polí produktů.


## <a name="introduction"></a>Úvod

Při použití předdefinovaných vkládání, úprava nebo odstranění funkce ovládacího prvku GridView, DetailsView nebo FormView ovládacích prvků, probíhají celou řadu kroků, když koncový uživatel dokončí proces přidání nového záznamu nebo aktualizaci nebo odstranění existující záznam. Jak jsme probírali v [předchozí kurz o službě](an-overview-of-inserting-updating-and-deleting-data-cs.md), při úpravě řádku v prvku GridView tlačítko pro úpravy je nahrazena tlačítka aktualizace a zrušit a zapněte BoundFields do textových polí. Když koncový uživatel aktualizuje data a klikne na tlačítko aktualizace, následující kroky se provádějí na zpětné volání:

1. GridView naplní její ObjectDataSource `UpdateParameters` s upravený záznam jedinečné identifikační polí (prostřednictvím `DataKeyNames` vlastnost) společně s hodnotami zadané uživatelem
2. GridView vyvolá jeho ObjectDataSource `Update()` metodu, která postupně vyvolá vhodnou metodu v základní objekt (`ProductsDAL.UpdateProduct`, v našem předchozí kurz o službě)
3. Podkladová data, která nyní obsahuje aktualizované změny, je znovu připojeno k prvku GridView.

Během této posloupnosti kroků, aktivuje počet událostí, umožňuje nám vytváření obslužných rutin událostí přidávat vlastní logiku místech. Například před kroku 1, prvku GridView `RowUpdating` dojde k aktivaci události. V tomto okamžiku můžete, zrušení žádosti o aktualizaci při některých chyb ověření. Když `Update()` je vyvolána metoda ObjectDataSource `Updating` událost je aktivována, poskytuje možnost Přidat nebo upravit hodnot ve všech `UpdateParameters`. Po ObjectDataSource podkladového metody objektu dokončil provádění ObjectDataSource `Updated` událost se vyvolá. Obslužná rutina události `Updated` událostí můžete prohlédnout podrobnosti o operaci aktualizace, jako je například vliv na tom, kolik řádků a zda došlo k výjimce. Nakonec po kroku 2, prvku GridView `RowUpdated` dojde k aktivaci události; obslužnou rutinu události pro tuto událost můžete prozkoumat další informace o jenom operaci aktualizace provést.

Obrázek 1 znázorňuje tuto řadu událostí a kroků při aktualizaci GridView. Vzor události na obrázku 1 není jedinečný pro aktualizaci GridView. Vkládání, aktualizaci nebo odstranění dat z prvku GridView, DetailsView nebo FormView vysráží stejné pořadí provedení před instrumentací a po ní úroveň události pro ovládací prvek webových dat a ObjectDataSource.


[![Série před a po událostech Fire při aktualizaci dat v GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Obrázek 1**: A řada z provedení před instrumentací a po událostech Fire při aktualizaci dat v GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


V tomto kurzu, kterou prozkoumáme pomocí těchto událostí pro rozšíření integrovaných vkládání aktualizaci a odstraňování možnosti data technologie ASP.NET webové ovládací prvky. Také jsme zobrazí přizpůsobení úpravy rozhraní aktualizovat pouze podmnožinu polí produktů.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Krok 1: Aktualizace produktu`ProductName`a`UnitPrice`pole

V rozhraní pro úpravy z předchozí kurz o službě *všechny* pole produktů, které nebylo jen pro čtení musí být zahrnut. Pokud nám chcete pole odebrat z prvku GridView - Řekněme, že `QuantityPerUnit` – při aktualizaci dat ovládacího prvku webových dat nenastavujte ObjectDataSource `QuantityPerUnit` `UpdateParameters` hodnotu. Prvku ObjectDataSource by pak úspěšně prošel zpracováním v `null` hodnoty do `UpdateProduct` obchodní logiky vrstvy (BLL) metodu, která by změnila upravených databázového záznamu `QuantityPerUnit` sloupec, který se `NULL` hodnotu. Podobně, pokud povinné pole, jako `ProductName`, se odebere z rozhraní úprav se aktualizace nezdaří s "*'ProductName' sloupec nepovoluje hodnoty Null*" výjimka. Důvod pro toto chování se protože ObjectDataSource byl nakonfigurován na volání `ProductsBLL` třídy `UpdateProduct` metodu, která očekává jako vstupní parametr pro každé pole produktu. Proto ObjectDataSource `UpdateParameters` kolekce obsahovala parametr pro každou metodu je vstupní parametry.

Pokud nám chcete poskytnout data webový ovládací prvek, který umožňuje koncovému uživateli aktualizovat pouze podmnožinu polí, pak musíme buď prostřednictvím kódu programu nastavit chybějící `UpdateParameters` hodnoty v prvku ObjectDataSource `Updating` obslužná rutina události nebo vytvořte a volání metody BLL, který očekává, že pouze podmnožinu polí. Podíváme se na tento druhý přístup.

Konkrétně můžeme vytvořit stránku, která zobrazuje jenom `ProductName` a `UnitPrice` pole upravitelné prvku GridView. Úpravy rozhraní prvku GridView pouze umožní uživateli aktualizovat dvě zobrazené pole `ProductName` a `UnitPrice`. Protože tato editační rozhraní poskytuje pouze podmnožinu pole produktu, potřebujeme buď vytvořit ObjectDataSource, používající stávající BLL `UpdateProduct` metoda a má chybějící hodnoty polí produkt programově v jeho `Updating` událostí Obslužná rutina nebo jsme muset vytvořit novou metodu BLL, která očekává pouze podmnožinu polí definovaných v prvku GridView. Pro účely tohoto kurzu, můžeme použít druhou možnost a přetížit `UpdateProduct` metody, ten, který má v pouhých tří vstupní parametry: `productName`, `unitPrice`, a `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Jako původní `UpdateProduct` metoda, toto přetížení začíná tak, že zkontrolujete, jestli je v databázi se zadaným produktem `ProductID`. Pokud ne, vrátí `false`, což indikuje, že žádost o aktualizaci informace o produktu se nezdařilo. V opačném případě aktualizuje existující záznam produktu `ProductName` a `UnitPrice` pole odpovídajícím způsobem a potvrdí aktualizace voláním TableAdpater `Update()` metodu `ProductsRow` instance.

Tato uveďte do našich `ProductsBLL` třídy, jsme připraveni vytvořit zjednodušené rozhraní ovládacího prvku GridView. Otevřít `DataModificationEvents.aspx` v `EditInsertDelete` složky a přidat na stránku GridView. Vytvoření nového prvku ObjectDataSource a nakonfigurujte ho na použití `ProductsBLL` třídy s jeho `Select()` mapování metody `GetProducts` a jeho `Update()` metoda mapování `UpdateProduct` přetížení, které přijímá pouze `productName`, `unitPrice`, a `productID` vstupní parametry. Obrázek 2 ukazuje průvodce vytvořit zdroj dat při mapování ObjectDataSource `Update()` metodu `ProductsBLL` vaší třídy nový `UpdateProduct` přetížení metody.


[![Map – Metoda Update() ObjectDataSource nové UpdateProduct přetížení](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Obrázek 2**: mapování ObjectDataSource `Update()` metodu pro nový `UpdateProduct` přetížení ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Vzhledem k tomu, že náš příklad bude zpočátku právě potřebujete mít možnost upravovat data, ale ne pro vložení nebo odstranění záznamů, věnujte chvíli explicitně určit ObjectDataSource `Insert()` a `Delete()` metody by neměly být namapované na žádnou `ProductsBLL` metody třídy tak, že přejdete na kartách INSERT a DELETE a zvolíte (žádný) z rozevíracího seznamu.


[![Zvolte z rozevíracího seznamu pro vložení a odstranění karty (žádný)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Obrázek 3**: Zvolte (žádný) z rozevíracího seznamu pro vložení a odstranění karty ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Po dokončení tohoto průvodce, zaškrtněte políčko Povolit úpravy z inteligentních značek v prvku GridView.

Dokončení průvodce vytvořit zdroj dat a vazby, která do prvku GridView Visual Studio vytvořila deklarativní syntaxe pro oba ovládací prvky. Přejděte do zobrazení zdroje ke kontrole ObjectDataSource deklarativní, který je uveden níže:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Vzhledem k tomu, že neexistují žádná mapování pro ObjectDataSource `Insert()` a `Delete()` metody, neexistují žádné `InsertParameters` nebo `DeleteParameters` oddíly. Kromě toho od `Update()` metoda je namapována na `UpdateProduct` přetížení metody, která přijímá pouze tři vstupní parametry, `UpdateParameters` oddíl obsahuje pouze ve třech `Parameter` instancí.

Všimněte si, že prvku ObjectDataSource `OldValuesParameterFormatString` je nastavena na `original_{0}`. Tato vlastnost je nastavena automaticky pomocí sady Visual Studio, při použití Průvodce konfigurace zdroje dat. Ale protože naše BLL metody Neočekáváme, že původní `ProductID` hodnota, která má být předaný, úplně odebrat toto přiřazení vlastnosti z deklarativní syntaxe ObjectDataSource.

> [!NOTE]
> Pokud jednoduše smažte `OldValuesParameterFormatString` hodnota vlastnosti z okna vlastnosti v okně návrhu, vlastnost zůstanou uchovány deklarativní syntaxe, ale bude nastavena na prázdný řetězec. Buď odeberte vlastnost úplně z deklarativní syntaxe, nebo v okně Vlastnosti nastavte hodnotu na výchozí hodnotu `{0}`.


Když prvku ObjectDataSource má jenom `UpdateParameters` pro název, cena a ID produktu Visual Studio přidala Vlastnost BoundField nebo třídě CheckBoxField v prvku GridView. pro každé pole produktu.


[![GridView obsahuje vlastnost BoundField nebo třídě CheckBoxField pro každé pole produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Obrázek 4**: prvku GridView obsahuje vlastnost BoundField nebo třídě CheckBoxField pro každé pole produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Když koncový uživatel upravuje produktu a jeho aktualizace tlačítko klikne, zobrazí prvku GridView těchto polí, která nejsou jen pro čtení. Potom nastaví hodnotu vlastnosti odpovídající parametr v prvku ObjectDataSource `UpdateParameters` kolekce k hodnotě zadané uživatelem. Pokud se nejedná o odpovídající parametr, prvku GridView. přidá jej do kolekce. Proto pokud naše GridView obsahuje BoundFields a CheckBoxFields pro všechna pole produkt, ObjectDataSource skončí volání `UpdateProduct` přetížení přebírající ve všech těchto parametrů, přestože, který prvku ObjectDataSource deklarativní určuje jenom tři vstupních parametrů (viz obrázek 5). Podobně, pokud je kombinace bez – jen pro čtení produktu se pole v prvku GridView, který neodpovídá instalovanému vstupní parametry pro `UpdateProduct` přetížení, bude vyvolána výjimka při pokusu o aktualizaci.


[![GridView se přidat parametry do kolekce UpdateParameters ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Obrázek 5**: prvku GridView se přidání parametrů ke ObjectDataSource `UpdateParameters` kolekce ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


K zajištění, že prvku ObjectDataSource vyvolá `UpdateProduct` přetížení, které přijímá pouze produkt název, cena a ID, potřebujeme k omezení prvku GridView s tím, že upravitelná pole pro jenom `ProductName` a `UnitPrice`. Toho můžete docílit tak, že odeberete jiné BoundFields a CheckBoxFields, tak, že nastavíte ty ostatní pole `ReadOnly` vlastnost `true`, nebo kombinaci obou. Pro účely tohoto kurzu můžeme jednoduše odebrat všechna pole prvku GridView s výjimkou `ProductName` a `UnitPrice` BoundFields, po jejímž uplynutí bude vypadat prvku GridView deklarativní:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

I v případě, `UpdateProduct` přetížení očekává tři parametry, máme dva BoundFields pouze v našich ovládacího prvku GridView. Důvodem je, že `productID` vstupní parametr je hodnota primárního klíče a předává hodnotu `DataKeyNames` vlastnost upravených řádku.

Naše prvku GridView, spolu s `UpdateProduct` přetížení, umožňuje uživateli upravit jenom název a cena produktu bez ztráty v ostatních polích produktu.


[![Rozhraní umožňuje úpravy jenom název a produktu cena](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Obrázek 6**: rozhraní umožňuje úpravy jenom název a produktu cena ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Jak je popsáno v předchozím kurzu, je životně důležitá, že GridView s zobrazení stav povoleno (výchozí chování). Pokud nastavíte GridView s `EnableViewState` vlastnost `false`, spustíte riskujete souběžných uživatelů zaznamenává neúmyslnému odstranění nebo úpravy. V tématu [upozornění: problém souběžnosti s ASP.NET 2.0 prvků GridViews/DetailsView/FormViews tohoto podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázaná](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) Další informace.


## <a name="improving-theunitpriceformatting"></a>Zlepšení`UnitPrice`formátování

Zatímco GridView příklad znázorňuje obrázek 6 funguje `UnitPrice` pole není vůbec ve formátu, což vede k zobrazení ceny, které se nejsou uvedeny žádné měny symboly a má čtyři desetinná místa. Použít měny formátování bez možností úprav řádků, stačí nastavit `UnitPrice` Vlastnost BoundField `DataFormatString` vlastnost `{0:c}` a jeho `HtmlEncode` vlastnost `false`.


[![Odpovídajícím způsobem nastavit vlastnosti HtmlEncode a DataFormatString pole UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Obrázek 7**: nastavte `UnitPrice`společnosti `DataFormatString` a `HtmlEncode` vlastnosti odpovídajícím způsobem ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Díky této změně bez možností úprav řádků formátování ceny jako měnu; upravená řádek, ale stále zobrazuje hodnotu bez symbolu měny a čtyři desetinná místa.


[![Bez možností úprav řádků jsou teď naformátované jako hodnoty měny](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Obrázek 8**: nelze upravit řádky jsou nyní naformátovaná jako hodnoty měny ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Formátování podle pokynů `DataFormatString` vlastnost můžete použít na úpravy rozhraní tak, že nastavíte vlastnost BoundField `ApplyFormatInEditMode` vlastnost `true` (výchozí hodnota je `false`). Tuto vlastnost nastavte na za chvíli `true`.


[![Nastavte vlastnost UnitPrice BoundField ApplyFormatInEditMode vlastnost na hodnotu true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Obrázek 9**: nastavte `UnitPrice` Vlastnost BoundField `ApplyFormatInEditMode` vlastnost `true` ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


S touto změnou hodnoty `UnitPrice` zobrazí v upravený řádek je formát měny.


[![Upravovaný řádek UnitPrice hodnota je teď ve formátu měny](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Obrázek 10**: úpravy řádku `UnitPrice` hodnota je nyní formátu měny ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Symbol měny v textovém poli však aktualizace produktu, jako je $19.00 vyvolá `FormatException`. Pokusy prvku GridView přiřadit hodnoty uživatelem zadané prvku ObjectDataSource `UpdateParameters` kolekce nelze převést `UnitPrice` řetězce "$19.00" do `decimal` vyžaduje parametr (viz obrázek 11). Chcete-li to napravit můžeme vytvořit obslužnou rutinu události pro prvku GridView `RowUpdating` události a ho parsovat uživatelem zadané `UnitPrice` jako formátu měny `decimal`.

Prvku GridView `RowUpdating` události přijímá jako svůj druhý parametr objektu typu [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), což zahrnuje `NewValues` slovníku jako jeden z jeho vlastnosti, které obsahuje uživatelem zadané hodnoty připravené k odeslání přiřazená ObjectDataSource `UpdateParameters` kolekce. Jsme můžete přepsat existující `UnitPrice` hodnotu `NewValues` kolekce s desetinnou hodnotou analyzovat s následujícími řádky kódu ve formátu měny `RowUpdating` obslužné rutiny události:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Pokud uživatel dodal `UnitPrice` hodnotu (například "$19.00"), tato hodnota je přepsána Desítková hodnota vypočítaná aplikací [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analýze hodnoty jako měnu. Tato možnost správně analyzovat desetinné čárky v případě symboly měny, čárek, desetinných míst a tak dále a používá [výčet NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) v [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) oboru názvů.

Obrázku 11 můžete vidět potíže způsobené symboly měny v uživatelem zadané `UnitPrice`, spolu s jak prvku GridView `RowUpdating` obslužná rutina události je možné využít správně analyzovat tyto vstup.


[![Upravovaný řádek UnitPrice hodnota je teď ve formátu měny](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Obrázek 11**: úpravy řádku `UnitPrice` hodnota je nyní formátu měny ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Krok 2: zákaz`NULL UnitPrices`

Zatímco databáze nakonfigurována, aby umožňovala `NULL` hodnoty v `Products` tabulky `UnitPrice` sloupce, může chceme, aby se zabránilo uživatelům, kteří navštíví konkrétní stránku ze `NULL` `UnitPrice` hodnotu. To znamená pokud uživatel nezadá `UnitPrice` hodnota při úpravě řádku produktu, nikoli uložte výsledky do databáze, chceme zobrazit zprávu informující uživatele, že prostřednictvím této stránky, musí mít všechny produkty, upravené cena zadaná.

`GridViewUpdateEventArgs` Objekt předaný do prvku GridView `RowUpdating` obsahuje obslužnou rutinu události `Cancel` vlastnost, která, pokud nastavena na `true`, ukončí proces aktualizace. Můžeme rozšířit `RowUpdating` obslužnou rutinu události pro nastavení `e.Cancel` k `true` a zobrazí se zpráva s vysvětlením důvod, proč, pokud `UnitPrice` hodnotu `NewValues` kolekce je `null`.

Začněte přidáním ovládacího prvku popisku webovou stránku s názvem `MustProvideUnitPriceMessage`. Tento ovládací prvek popisku se zobrazí, pokud se uživateli nepodaří zadat `UnitPrice` hodnotu při aktualizaci produktu. Nastavte jeho `Text` vlastnost "Produktu musí poskytnout cenu." Vytvořili jste taky novou třídu šablony stylů CSS v `Styles.css` s názvem `Warning` s následující definice:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Nakonec nastavte jeho `CssClass` vlastnost `Warning`. V tomto okamžiku návrháře by se zobrazit upozornění v červené, tučné, kurzíva, velmi velké velikosti písma nad prvku GridView, jak ukazuje obrázek 12.


[![Popisek se přidala nad prvku GridView.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Obrázek 12**: A popisek má byla přidána nad prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Ve výchozím nastavení, by měl být skrytý tento popisek, takže nastavte jeho `Visible` vlastnost `false` v `Page_Load` obslužné rutiny události:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Pokud se uživatel pokusí o aktualizaci produktu bez zadání `UnitPrice`, chceme zrušit aktualizaci a zobrazit popisek upozornění. Posílení prvku GridView `RowUpdating` obslužná rutina události následujícím způsobem:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Pokud se uživatel pokusí uložit produktu bez zadání cenu, aktualizace se zrušila, a zobrazí se zpráva užitečné. Zatímco databáze (a obchodní logiky) umožňuje `NULL` `UnitPrice` s, konkrétní stránku ASP.NET tak není.


[![Uživatel nemůže opustit UnitPrice prázdné](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Obrázek 13**: A uživatel nemůže opustit `UnitPrice` prázdné ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Zatím jsme viděli, jak pomocí prvku GridView `RowUpdating` událostí prostřednictvím kódu programu měnit hodnoty parametrů přiřazená ObjectDataSource `UpdateParameters` kolekce a jak zrušit aktualizaci zpracovat úplně se vynechá. Tyto koncepty přenesou na ovládacím prvku DetailsView a FormView a platí také pro vložení a odstranění.

Tyto úlohy je možné provést na úrovni prvku ObjectDataSource prostřednictvím obslužné rutiny událostí pro jeho `Inserting`, `Updating`, a `Deleting` události. Tyto události vyvolat před vyvoláním související metody základní objekt a zadejte odpovídající poslední příležitost změnit kolekci vstupních parametrů nebo zrušit operaci rovnou předplatit. Obslužné rutiny událostí pro tyto tři události jsou předány objekt typu [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , která má dvě vlastnosti, které vás zajímají:

- [Zrušit](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), který, pokud nastaveno `true`, zruší právě prováděnou operaci
- [Vstupní parametry](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), což je kolekce `InsertParameters`, `UpdateParameters`, nebo `DeleteParameters`, v závislosti na tom, zda obslužná rutina události pro `Inserting`, `Updating`, nebo `Deleting` událostí

Pro ilustraci, práci s hodnotami parametrů na úrovni prvku ObjectDataSource, můžeme zahrnout DetailsView naši stránku, která umožňuje uživatelům přidání nového produktu. Tento prvek DetailsView se použije k poskytnutí rozhraní pro rychlé přidání nového produktu do databáze. Zajištění konzistentního uživatelského rozhraní při přidání nového produktu můžeme povolit uživatelům zadat pouze hodnoty pro `ProductName` a `UnitPrice` pole. Ve výchozím nastavení, je možnost tyto hodnoty, které nejsou dodány v ovládacím prvku DetailsView vkládání rozhraní `NULL` databáze hodnotu. Ale můžeme použít ObjectDataSource `Inserting` události vkládat různé výchozí hodnoty, jak uvidíme za chvíli.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Krok 3: Poskytuje rozhraní pro přidání nové produkty

Přetáhněte DetailsView z panelu nástrojů na Návrhář nad prvku GridView, vymažte jeho `Height` a `Width` vlastnosti a svázat tak ObjectDataSource už je na stránce. Tato možnost přidá vlastnost BoundField nebo třídě CheckBoxField pro každé pole produktu. Protože chceme použít tohoto prvku DetailsView. Chcete-li přidat nové produkty, musíme zaškrtněte možnost Povolit vložení z inteligentních značek; neexistuje ale žádná z těchto možností protože ObjectDataSource `Insert()` metoda není mapována na metodu v `ProductsBLL` třídy (Vzpomínáte, že nastavíme toto mapování na (žádný), při konfiguraci zdroje dat zobrazit obrázek 3).

Ke konfiguraci ObjectDataSource, vyberte odkaz Konfigurovat zdroj dat z jeho inteligentních značek, průvodce. Na první obrazovce umožňuje změnit základní objekt, který ObjectDataSource je vázán na; Nechejte nastavenou `ProductsBLL`. Na další obrazovce uvádí mapování z metody ObjectDataSource na základní objekt. I když společnost Microsoft výslovně uvedené, který `Insert()` a `Delete()` metody by neměly být namapovány na všechny metody, pokud přejdete na kartách INSERT a DELETE uvidíte, že existuje mapování. Důvodem je, že `ProductsBLL`společnosti `AddProduct` a `DeleteProduct` metody používají `DataObjectMethodAttribute` atribut označuje, že jsou výchozí metody pro `Insert()` a `Delete()`v uvedeném pořadí. Proto průvodce ObjectDataSource vybere tyto pokaždé, když spustíte průvodce, pokud není explicitně zadat jinou hodnotu.

Ponechte `Insert()` metody odkazující na `AddProduct` metody, ale znovu nastavit SMAZAT kartu rozevíracího seznamu na (žádný).


[![Nastavení rozevíracího seznamu na kartě Vložení AddProduct – metoda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Obrázek 14**: nastavte na kartě Vložení rozevíracího seznamu `AddProduct` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Nastavte SMAZAT kartu rozevíracího seznamu na (žádný)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Obrázek 15**: nastavte odstranit kartu rozevíracího seznamu na (žádný) ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Po provedení těchto změn, deklarativní syntaxe ObjectDataSource rozšíří na `InsertParameters` kolekce, jak je znázorněno níže:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Znovu spustit průvodce přidat zpět `OldValuesParameterFormatString` vlastnost. Za chvíli zrušte tuto vlastnost nastavíte na výchozí hodnotu (`{0}`) nebo odeberte zcela ze deklarativní syntaxe.

Ovládacím prvkem ObjectDataSource vkládání zajišťuje inteligentních značek v ovládacím prvku DetailsView teď bude zahrnovat zaškrtávací políčko Povolit vložení; Vraťte se do návrháře a zaškrtněte tuto možnost. V dalším kroku Zredukovat ovládacím prvku DetailsView, takže má jenom dvě BoundFields - `ProductName` a `UnitPrice` – a CommandField. Deklarativní syntaxe ovládacím prvku DetailsView v tomto okamžiku by mělo vypadat jako:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

Obrázek 16 ukazuje tuto stránku, když v tomto okamžiku zobrazit pomocí prohlížeče. Jak je vidět, ovládacím prvku DetailsView se zobrazuje název a cena první produktu (Chai). Chceme, ale vkládání rozhraní, které poskytuje prostředky pro uživatele pro rychlé přidání nového produktu do databáze.


[![Ovládacím prvku DetailsView se aktuálně zobrazují v režimu jen pro čtení](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Obrázek 16**: The DetailsView se aktuálně zobrazují v režimu jen pro čtení ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Chcete-li zobrazit ovládacím prvku DetailsView. v režimu vkládání musíme nastavit `DefaultMode` vlastnost `Inserting`. To vykreslí ovládacím prvku DetailsView. v režimu vkládání, když uživatel poprvé a udržuje ho došlo po vložení nového záznamu. Jak ukazuje obrázek 17 takové DetailsView poskytuje rychlé rozhraní pro přidání nového záznamu.


[![Ovládacím prvku DetailsView poskytuje rozhraní pro rychlé přidání nového produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Obrázek 17**: ovládacím prvku DetailsView poskytuje rozhraní pro rychlé přidání nového produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Když uživatel zadá název produktu a cena (například "Acme Water" a 1,99 jako obrázek 17) a klikne na tlačítko Vložit, vyplývá zpětné volání a začíná vkládání pracovního postupu, ukončené v novém záznamu produktu se přidávají do databáze. Ovládacím prvku DetailsView udržuje jeho vkládání rozhraní a prvku GridView je automaticky znovu připojeno ke zdroji dat tak, aby obsahovaly nového produktu, jak ukazuje obrázek 18.


![Produkt](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Obrázek 18**: "Acme Water" produkt byl přidán do databáze


Zatímco GridView v obrázek 18, nezobrazí produktu pole chybí z rozhraní DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, a tak dále jsou přiřazeny `NULL` databáze hodnoty. Vidíte to provedením následujících kroků:

1. Přejděte do Průzkumníka serveru v sadě Visual Studio
2. Rozšíření `NORTHWND.MDF` uzel databáze
3. Klikněte pravým tlačítkem na `Products` uzel tabulky databáze
4. Vyberte zobrazení tabulky dat

Tím se zobrazí všechny záznamy v `Products` tabulky. Obrázek 19 ukazuje, všechny sloupce náš nový produkt jiného než `ProductID`, `ProductName`, a `UnitPrice` mají `NULL` hodnoty.


[![Produktu pole není k dispozici v ovládacím prvku DetailsView jsou přiřazeny hodnoty NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Obrázek 19**: The produktu pole není k dispozici v ovládacím prvku DetailsView jsou přiřazeny `NULL` hodnoty ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Může být vhodné poskytnout jiné než výchozí hodnotu `NULL` pro jeden nebo více z těchto hodnot sloupce, buď protože `NULL` není nejvhodnější výchozí nebo proto, že neumožňuje přímo v samotném sloupci databáze `NULL` s. Provedete to tak jsme můžete prostřednictvím kódu programu nastavit hodnoty parametrů ovládacím prvku DetailsView `InputParameters` kolekce. Toto přiřazení lze provést buď v obslužné rutiny ovládacím prvku DetailsView `ItemInserting` události nebo ObjectDataSource `Inserting` událostí. Protože se už podívali jsme události před instrumentací a po ní úroveň s využitím datových webové řízení úrovně, podíváme se na používání událostí prvku ObjectDataSource této doby.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Krok 4: Přiřazování hodnot`CategoryID`a`SupplierID`parametry

Pro účely tohoto kurzu Představte si, že pro naši aplikaci při přidání nového produktu prostřednictvím tohoto rozhraní by měla být přiřazena `CategoryID` a `SupplierID` hodnotu 1. Jak už bylo zmíněno dříve, bude ObjectDataSource během procesu změny dat nemá dvojici události před instrumentací a po ní úrovně, které se spustí. Při jeho `Insert()` vyvolání metody, nejprve vyvolá ObjectDataSource jeho `Inserting` události, potom volá metodu, která jeho `Insert()` metoda namapovaný na a nakonec vyvolává `Inserted` událostí. `Inserting` Obslužná rutina události nám poskytuje jeden poslední možnost k vylepšení vstupní parametry nebo zrušit operaci rovnou předplatit.

> [!NOTE]
> V reálné aplikaci budete pravděpodobně chtít buď nechat uživatele zadejte kategorie a dodavatele nebo by pro ně podle kritérií vybrat tuto hodnotu nebo obchodní logiky (spíše než slepě výběr ID 1). Bez ohledu na to v příkladu ukazuje, jak prostřednictvím kódu programu nastavit hodnotu vstupního parametru ObjectDataSource předem úroveň události.


Za chvíli vytvořit obslužnou rutinu události pro ObjectDataSource `Inserting` událostí. Všimněte si, že obslužná rutina události druhé vstupní parametr je objekt typu `ObjectDataSourceMethodEventArgs`, který má vlastnost, která má přístup ke kolekci parametrů (`InputParameters`) a vlastnost na zrušení operace (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

V tomto okamžiku `InputParameters` vlastnost obsahuje ObjectDataSource `InsertParameters` kolekce s hodnoty přiřazené v ovládacím prvku DetailsView. Chcete-li změnit hodnotu některého z těchto parametrů, jednoduše použijte: `e.InputParameters["paramName"] = value`. Proto se nastavit `CategoryID` a `SupplierID` hodnoty 1, upravte `Inserting` obslužná rutina události vypadat nějak takto:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Tento čas při přidání nového produktu (například Acme Soda), `CategoryID` a `SupplierID` sloupce nového produktu jsou nastaveny na hodnotu 1 (viz obrázek 20).


[![Nové produkty teď mají jejich CategoryID a ID dodavatele hodnoty nastavené na hodnotu 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Obrázek 20**: nové produkty teď mají jejich `CategoryID` a `SupplierID` hodnoty nastavené na hodnotu 1 ([kliknutím ji zobrazíte obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Souhrn

Během úpravy, vložení a odstranění procesu webového ovládacího prvku dat a ObjectDataSource pokračovat procházet celou řadou události před instrumentací a po ní úrovně. V tomto kurzu prozkoumat události předem úrovně jsme viděli, jak použít k přizpůsobení vstupní parametry nebo zrušit operaci úpravy dat úplně i z ovládacího prvku webových dat a ObjectDataSource události. V dalším kurzu podíváme na vytváření a používání obslužných rutin událostí pro události po úrovně.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Jackie Goor a Liz Shulok. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [další](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
