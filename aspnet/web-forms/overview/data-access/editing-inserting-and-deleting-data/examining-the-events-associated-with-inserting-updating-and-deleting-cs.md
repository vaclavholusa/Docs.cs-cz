---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: "Zkoumání události související s vkládání, aktualizaci nebo odstranění (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu podíváme pomocí události, které nastaly před, během a po typu vložení aktualizaci nebo odstranění operace dat ASP.NET ovládací prvek webu. W..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 93da23d58d1ba73c5b97f42631d036dd364de24d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Zkoumání události související s vkládání, aktualizaci nebo odstranění (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) nebo [stáhnout PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> V tomto kurzu podíváme pomocí události, které nastaly před, během a po typu vložení aktualizaci nebo odstranění operace dat ASP.NET ovládací prvek webu. Také jsme zobrazí postup přizpůsobení úpravy rozhraní, které chcete aktualizovat pouze podmnožinu pole produktu.


## <a name="introduction"></a>Úvod

Pokud používáte integrované vkládání, úpravy nebo odstranění funkce ovládacích prvků GridView, DetailsView nebo FormView, transpire celou řadu kroků při koncový uživatel dokončí proces přidávání nového záznamu nebo aktualizaci nebo odstranění existujícího záznamu. Jak již bylo zmíněno [předchozí kurzu](an-overview-of-inserting-updating-and-deleting-data-cs.md), když upravíte řádek v GridView tlačítko Upravit je nahrazena aktualizace a zrušit tlačítka a zapněte BoundFields do textových polí. Až koncový uživatel aktualizuje data a klikne na aktualizace, následující kroky se provádějí v zpětné volání:

1. GridView naplní jeho ObjectDataSource `UpdateParameters` s upravený záznam jedinečné identifikační polí (prostřednictvím `DataKeyNames` vlastnost) společně s hodnoty zadané uživatelem.
2. GridView vyvolá jeho ObjectDataSource `Update()` metodu, která zase vyvolá odpovídající metodu podkladového objektu (`ProductsDAL.UpdateProduct`, v našem kurzu předchozí)
3. Zdrojová data, která nyní obsahuje aktualizované změny, je odrážejí na GridView

Během tato posloupnost kroků provést určitý počet událostí, to nám umožňuje vytváření obslužných rutin událostí pro přidání vlastní logiky tam, kde je potřeba. Například před kroku 1, GridView na `RowUpdating` je aktivována událost. Pokud se některé Chyba ověření můžete v tomto okamžiku zrušení žádost o aktualizaci. Když `Update()` metoda je volána, ObjectDataSource `Updating` aktivuje událost, že poskytuje možnost Přidat nebo upravit hodnoty každého `UpdateParameters`. Po zdrojové ObjectDataSource metody objektu dokončil provádění ObjectDataSource `Updated` událost se vyvolá. Obslužné rutiny události pro `Updated` událostí můžete prohlédnout podrobnosti o operaci aktualizace, jako je například situace měla vliv na tom, kolik řádků a zda došlo k výjimce. Nakonec po kroku 2, GridView na `RowUpdated` aktivuje událost; obslužnou rutinu pro tuto událost může vyhledejte další informace o právě aktualizace operaci provést.

Obrázek 1 znázorňuje tuto řadu událostí a kroků při aktualizaci GridView. Vzor událostí na obrázku 1 není jedinečný aktualizace s GridView. Vkládání, aktualizaci nebo odstranění dat z GridView, DetailsView nebo FormView vysráží stejnou sekvenci před a po úroveň události pro ovládací prvek webu data i ObjectDataSource.


[![Řadu před a po události Fire při aktualizaci dat v GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Obrázek 1**: A řady z před a po ještě efektivněji při aktualizaci Data událostí v GridView ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


V tomto kurzu podíváme pomocí těchto událostí rozšířit předdefinovaný vložení aktualizace a odstranění možnosti dat ASP.NET Web ovládací prvky. Také jsme zobrazí postup přizpůsobení úpravy rozhraní, které chcete aktualizovat pouze podmnožinu pole produktu.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Krok 1: Aktualizace produktu`ProductName`a`UnitPrice`pole

Úpravy rozhraní z předchozí kurzu *všechny* polí produktu, které nebyly jen pro čtení musí být zahrnut. Pokud nám chcete pole odebrat z GridView - vyslovení `QuantityPerUnit` – aktualizace dat ovládací prvek webu dat nebude nastavena ObjectDataSource `QuantityPerUnit` `UpdateParameters` hodnotu. ObjectDataSource by pak předejte `null` hodnotu do `UpdateProduct` obchodní logiky vrstvy (BLL) metodu, která by způsobila změnu záznam upravená databáze `QuantityPerUnit` sloupec, který se `NULL` hodnotu. Podobně, pokud povinné pole, například `ProductName`, se odebere z rozhraní úpravy se aktualizace nezdaří s "*'ProductName' sloupec nepovoluje hodnoty Null*" výjimka. Důvod pro toto chování se, protože ObjectDataSource byla nakonfigurována pro volání `ProductsBLL` třídy `UpdateProduct` metodu, která očekává vstupního parametru pro každé pole produktu. Proto ObjectDataSource na `UpdateParameters` kolekce obsahovala parametr pro každou metodu je vstupní parametry.

Pokud chceme poskytují data ovládací prvek webu, která umožňuje koncovým uživatelům aktualizovat pouze podmnožinu pole, pak je potřeba buď programově nastavit chybějící `UpdateParameters` hodnoty v prvku ObjectDataSource `Updating` obslužné rutiny události nebo vytvořte a volání metody BLL, který očekává jenom podmnožinu pole. Podíváme se na tento pozdější přístup.

Konkrétně můžeme vytvořit stránku, která zobrazuje jenom na `ProductName` a `UnitPrice` polí v upravitelné GridView. Tato rutina GridView úpravy rozhraní pouze umožní uživatelům aktualizovat dvě zobrazené pole, `ProductName` a `UnitPrice`. Vzhledem k tomu, že toto úpravy rozhraní zobrazuje pouze podmnožinu pole produktů, musíme buď vytvořit ObjectDataSource, který používá existující BLL `UpdateProduct` metoda a chybějící hodnoty polí produktu nastavil prostřednictvím kódu programu v jeho `Updating` událostí Obslužná rutina nebo jsme muset vytvořit nové BLL metodu, která očekává pouze dílčí sadu pole definovaná v GridView. V tomto kurzu budeme, použijte druhou možnost a přetížit `UpdateProduct` metoda, ten, který přebírá jenom tři vstupní parametry: `productName`, `unitPrice`, a `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Jako původní `UpdateProduct` metoda, toto přetížení začne zjišťujeme, pokud je v databázi se zadaným produktem `ProductID`. Pokud ne, vrátí hodnotu `false`, což indikuje, že žádost o aktualizaci informací o produktu se nezdařila. V opačném případě aktualizuje stávající záznam produktu `ProductName` a `UnitPrice` polí s odpovídajícím způsobem a potvrdí aktualizace voláním TableAdpater `Update()` předávání v případě metody `ProductsRow` instance.

Pomocí tohoto přidání do našich `ProductsBLL` třídy, je vše připraveno k vytvoření zjednodušené rozhraní GridView. Otevřete `DataModificationEvents.aspx` v `EditInsertDelete` složky a přidat GridView na stránku. Vytvoření nové ObjectDataSource a nakonfigurujte ho na použití `ProductsBLL` třídy s jeho `Select()` metoda mapování na `GetProducts` a jeho `Update()` metoda mapování `UpdateProduct` přetížení, které přijímá pouze v `productName`, `unitPrice`, a `productID` vstupní parametry. Obrázek 2 ukazuje průvodce vytvořit zdroj dat při mapování ObjectDataSource `Update()` metodu `ProductsBLL` třída je nové `UpdateProduct` přetížení metody.


[![Map – Metoda Update() ObjectDataSource nové UpdateProduct přetížení](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Obrázek 2**: mapování ObjectDataSource `Update()` metodu pro nový `UpdateProduct` přetížení ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Vzhledem k tomu, že naše ukázka původně právě potřebovat možnost upravovat data, ale není pro vložení nebo odstranit záznamy, pozorně explicitně označuje, že ObjectDataSource `Insert()` a `Delete()` metody by neměla být namapované na žádnou `ProductsBLL` třídy a metody přejdete na kartách INSERT a DELETE a zvolením (None) z rozevíracího seznamu.


[![Zvolte (žádný) z rozevíracího seznamu pro příkaz INSERT a DELETE karty](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Obrázek 3**: Zvolte (žádný) z rozevíracím seznamu pro vložení a odstranit karty ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Po dokončení tohoto průvodce zaškrtněte políčko Povolit úpravy z prvku GridView inteligentních značek.

Visual Studio s dokončením průvodce vytvořte zdroj dat a který vytvoření vazby na GridView vytvořil deklarativní syntaxi pro oba ovládací prvky. Přejdete na zobrazení zdroje, chcete-li prověřit ObjectDataSource deklarativní značky, které jsou uvedeny níže:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Vzhledem k tomu, že neexistují žádná mapování pro ObjectDataSource `Insert()` a `Delete()` metody, neexistují žádné `InsertParameters` nebo `DeleteParameters` oddíly. Kromě toho od `Update()` metoda je namapovaný na `UpdateProduct` přetížení metody, která přijímá jenom tři vstupní parametry, `UpdateParameters` obsahuje jenom tři `Parameter` instance.

Všimněte si, že ObjectDataSource `OldValuesParameterFormatString` je nastavena na `original_{0}`. Tato vlastnost je nastavena automaticky Visual Studio, při použití Průvodce konfigurace zdroje dat. Ale vzhledem k tomu, že naše BLL metody neočekávané původní `ProductID` hodnota, která má být předán, odeberte toto přiřazení vlastnosti zcela z ObjectDataSource deklarativní syntaxe.

> [!NOTE]
> Pokud zrušíte jednoduše `OldValuesParameterFormatString` hodnotu vlastnosti z okna vlastností v zobrazení návrhu, vlastnost bude stále existovat v deklarativní syntaxi, ale bude nastavena pro prázdný řetězec. Buď odeberte vlastnost zcela od deklarativní syntaxi nebo, v okně Vlastnosti, nastavte hodnotu na výchozí, `{0}`.


Zatímco ObjectDataSource má jenom `UpdateParameters` pro název, ceny a ID produktu Visual Studio přidala BoundField nebo vlastnost CheckBoxField v GridView pro každé pole produktu.


[![GridView obsahuje BoundField nebo vlastnost CheckBoxField pro každé pole produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Obrázek 4**: GridView obsahuje BoundField nebo vlastnost CheckBoxField pro každé pole produktu ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Když koncový uživatel upravuje produktu a kliknutím na tlačítko jeho aktualizaci, zobrazí GridView těchto polí, které nebyly jen pro čtení. Potom nastaví hodnotu odpovídající parametr v prvku ObjectDataSource `UpdateParameters` kolekce hodnotě zadané uživatelem. Pokud není k dispozici odpovídající parametr, přidá GridView jedna kolekce. Proto pokud naše GridView obsahuje BoundFields a CheckBoxFields pro všechna pole produktu, ObjectDataSource dojdete k vyvolání `UpdateProduct` přetížení, které přijímá ve všech těchto parametrů, přestože, ObjectDataSource deklarativní určuje jenom tři vstupní parametry (viz obrázek 5). Podobně, pokud je některou kombinaci hodnot jiných jen pro čtení produktu, které se pole v GridView, který neodpovídá vstupní parametry pro `UpdateProduct` přetížení, při pokusu o aktualizaci, bude vyvolána výjimka.


[![Rutina GridView bude přidat parametry do kolekce UpdateParameters ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Obrázek 5**: The GridView bude přidání parametrů ke ObjectDataSource `UpdateParameters` kolekce ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


K zajištění, že ObjectDataSource vyvolá `UpdateProduct` přetížení při právě produktu název, ceny a ID, je potřeba omezit GridView na situaci, kdy upravitelné pole pro jenom na `ProductName` a `UnitPrice`. Toho lze dosáhnout odebráním dalším BoundFields a CheckBoxFields, nastavením, ty další pole `ReadOnly` vlastnost `true`, nebo některé kombinaci obojího. V tomto kurzu budeme stačí odstranit všechna pole GridView s výjimkou `ProductName` a `UnitPrice` BoundFields, po které bude vypadat prvku GridView deklarativní značky:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

I když `UpdateProduct` přetížení očekává tři vstupní parametry, máme dvě BoundFields pouze v našem GridView. Důvodem je, že `productID` vstupní parametr je hodnotu primárního klíče a v předána hodnota `DataKeyNames` vlastnost pro upravená řádek.

Naše GridView, spolu s `UpdateProduct` přetížení, umožňuje uživateli upravit pouze název a cena produktu beze ztráty v ostatních polích produktu.


[![Rozhraní umožňuje úpravy právě produktu název a cena](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Obrázek 6**: umožňuje rozhraní úpravy právě produktu název a cena ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Jak je popsáno v předchozí kurz, je životně důležitá, že GridView stav zobrazení s být povolené (výchozí nastavení). Pokud jste nastavili GridView s `EnableViewState` vlastnost `false`, riskujete, že náhodně odstraněním nebo úpravou zaznamenává počet uživatelů. V tématu [upozornění: souběžnosti problém s ASP.NET 2.0 GridViews nebo DetailsView/FormViews tento podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázána](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) Další informace.


## <a name="improving-theunitpriceformatting"></a>Vylepšení`UnitPrice`formátování

Při na GridView příklad na obrázku 6 funguje `UnitPrice` pole není naformátovaná vůbec, což vede k zobrazení ceny, které nemá žádné měny symboly a má čtyři desetinná místa. Použít pro upravovat řádky měny, jednoduše nastavit `UnitPrice` BoundField na `DataFormatString` vlastnost `{0:c}` a jeho `HtmlEncode` vlastnost `false`.


[![Nastavte pole UnitPrice DataFormatString a vlastnosti HtmlEncode odpovídajícím způsobem](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Obrázek 7**: nastavte `UnitPrice`na `DataFormatString` a `HtmlEncode` vlastnosti odpovídajícím způsobem ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Díky této změně formátu upravovat řádky za cenu jako měnu; upravená řádek, ale stále zobrazuje hodnotu bez symbolu měny a s čtyři desetinná místa.


[![Nelze upravit bez řádky jsou nyní naformátovat jako hodnoty měny](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Obrázek 8**: upravovat řádky jsou nyní formátovaný jako hodnoty měny ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Pokyny k formátování zadané v `DataFormatString` vlastnost lze použít k úpravě rozhraní nastavením BoundField `ApplyFormatInEditMode` vlastnost `true` (výchozí hodnota je `false`). Za chvíli tuto vlastnost nastavit na `true`.


[![Nastavte vlastnost ApplyFormatInEditMode UnitPrice BoundField na hodnotu true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Obrázek 9**: nastavte `UnitPrice` na BoundField `ApplyFormatInEditMode` vlastnost `true` ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Díky této změně hodnotu `UnitPrice` zobrazí v upravená řádek je formát měny.


[![Hodnota UnitPrice upravovaný řádek je nyní naformátovat jako měny](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Obrázek 10**: Upravit řádku `UnitPrice` hodnota je nyní formátu měny ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Však aktualizace produktu pomocí symbolu měny do textového pole, jako je $19.00 vyvolá `FormatException`. Když se pokusí přiřadit hodnoty zadané uživatelem ObjectDataSource GridView `UpdateParameters` kolekce je možné převést `UnitPrice` řetězec "$19.00" do `decimal` vyžaduje parametr (viz obrázek 11). Chcete-li to opravit můžeme vytvořit obslužnou rutinu události pro prvku GridView `RowUpdating` událostí a mějte ho analyzovat uživatelem zadané `UnitPrice` jako měnu formátu `decimal`.

Prvku GridView `RowUpdating` událost přijme jako její druhý parametr objekt typu [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), což zahrnuje `NewValues` slovníku jako jeden z jeho vlastnosti, které obsahuje hodnoty připravené k zadané uživatelem přiřazené ObjectDataSource `UpdateParameters` kolekce. Jsme můžete přepsat existující `UnitPrice` hodnotu `NewValues` kolekce s desetinnou hodnotou analyzovat pomocí následující řádky kódu ve formátu měny `RowUpdating` obslužné rutiny události:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Pokud má zadaný uživatel `UnitPrice` hodnotu (například "$19.00"), se tato hodnota přepíše s desetinnou hodnotou vypočítávají podle [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analýza hodnotu jako měny. To bude správně analyzovat desetinné v případě symboly měny, čárky, desetinných míst a tak dále a používá [výčet NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) v [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) oboru názvů.

Obrázek 11 ukazuje potíže způsobené symboly měny v uživatelem zadané `UnitPrice`, společně s postupy prvku GridView `RowUpdating` obslužné rutiny události, můžete použít k správně analyzovat takové vstup.


[![Hodnota UnitPrice upravovaný řádek je nyní naformátovat jako měny](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Obrázek 11**: Upravit řádku `UnitPrice` hodnota je nyní formátu měny ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Krok 2: zákaz`NULL UnitPrices`

Během konfigurace databáze umožňuje `NULL` hodnoty ve `Products` tabulky `UnitPrice` sloupce, jsme chtít zabránit uživatelům, kteří navštíví konkrétní stránku zadat `NULL` `UnitPrice` hodnotu. To znamená pokud uživatel nezadá `UnitPrice` hodnota při úpravě řádek produktu, nikoli uložte výsledky do databáze chceme zobrazit zprávu informující uživatele, že pomocí této stránky, musí mít všechny produkty, upravená cena zadaná.

`GridViewUpdateEventArgs` Objekt předaný do prvku GridView `RowUpdating` obsahuje obslužné rutiny události `Cancel` vlastnost, pokud nastavena na `true`, ukončí proces aktualizace. Umožňuje rozšířit `RowUpdating` obslužné rutiny události nastavit `e.Cancel` k `true` a zobrazí zprávu vysvětlením důvod, proč, pokud `UnitPrice` hodnotu `NewValues` kolekce je `null`.

Začněte tím, že přidání ovládacího prvku popisek webovou stránku s názvem `MustProvideUnitPriceMessage`. Tento popisek – ovládací prvek se zobrazí, pokud uživatel k určení `UnitPrice` hodnota při aktualizaci produktu. Nastavte jeho `Text` vlastnost "Je nutné zadat cenu pro produkt." Vytvořili jste taky novou třídu CSS v `Styles.css` s názvem `Warning` s následující definice:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Nakonec nastavte jeho `CssClass` vlastnost `Warning`. V tomto okamžiku návrháře by se zobrazit upozornění v red, tučné, kurzíva, velikost velmi velkými písmeny výše GridView, jak ukazuje obrázek 12.


[![Štítek se přidal výše GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Obrázek 12**: A popisek má byla přidána výše GridView ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Ve výchozím nastavení, by být tento popisek skrytý, takže nastavit jeho `Visible` vlastnost `false` v `Page_Load` obslužné rutiny události:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Pokud se uživatel pokusí o aktualizaci produktu bez zadání `UnitPrice`, chceme zrušit aktualizaci a zobrazit upozornění popisku. Posílení prvku GridView `RowUpdating` obslužné rutiny události následujícím způsobem:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Pokud uživatel se pokusí uložit bez zadání cenu produktu, aktualizace byla zrušena, a zobrazí se zpráva s užitečné. Při databáze (a obchodní logiku) umožňuje `NULL` `UnitPrice` s, tato konkrétní stránka technologie ASP.NET není.


[![Uživatele nelze ponechat prázdný UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Obrázek 13**: A nemůžete opustit uživatele `UnitPrice` prázdné ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Zatím jsme viděli, jak používat prvku GridView `RowUpdating` událostí prostřednictvím kódu programu změnit hodnoty parametru přiřazené ObjectDataSource `UpdateParameters` kolekce a jak zrušit aktualizaci zpracovat zcela. Tyto koncepty přenesou do ovládacích prvků DetailsView a FormView a platí také pro vkládání a odstraňování.

Tyto úlohy lze také provést na úrovni ObjectDataSource prostřednictvím obslužné rutiny události pro jeho `Inserting`, `Updating`, a `Deleting` události. Tyto události fire před vyvoláním související metody podkladového objektu a zadejte poslední chance moci upravit kolekci vstupní parametry, nebo přímý operaci zrušte. Obslužné rutiny události pro tyto tři události jsou předávány na objektu typu [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) má dvě vlastnosti, které vás zajímají:

- [Zrušit](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), která, pokud nastavena na `true`, zruší prováděnou operaci
- [Vstupní parametry](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), což je kolekce `InsertParameters`, `UpdateParameters`, nebo `DeleteParameters`, v závislosti na tom, zda obslužná rutina události pro `Inserting`, `Updating`, nebo `Deleting` událostí

Pro ilustraci práce s hodnotami parametrů na úrovni ObjectDataSource, umožňuje zahrnout DetailsView naši stránku, který umožňuje uživatelům přidání nového produktu. Tato DetailsView se použije k poskytnutí rozhraní pro rychle přidání nového produktu do databáze. Při přidání nového produktu umožňuje povolit uživatelům jenom zadat hodnoty pro zachovat konzistentní uživatelské rozhraní `ProductName` a `UnitPrice` pole. Ve výchozím nastavení, je možnost tyto hodnoty, které nejsou dodány v ovládacím prvku DetailsView vkládání rozhraní `NULL` databáze hodnotu. Ale můžeme použít ObjectDataSource `Inserting` událost vložení různé výchozí hodnoty, jako jsme krátce zobrazí.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Krok 3: Poskytuje rozhraní pro přidání nové produkty

Přetáhněte DetailsView z panelu nástrojů na návrháře výše GridView, vymažte jeho `Height` a `Width` vlastnosti a vazby, aby ObjectDataSource už je na stránce. Tím se přidá BoundField nebo vlastnost CheckBoxField pro každé pole produktu. Vzhledem k tomu, že chceme, použijete tento DetailsView k přidání nových produktů, musíme zaškrtnutí políčka Povolit vložení ze inteligentní značky; neexistuje však žádný z těchto možností protože ObjectDataSource `Insert()` metoda není namapovaná na metodu v `ProductsBLL` – třída (odvolání jsme nastavit toto mapování (žádné), při konfiguraci zdroje dat viz obrázek 3).

Pokud chcete nakonfigurovat ObjectDataSource, vyberte odkaz Konfigurace zdroje dat z jeho inteligentních značek, spouští se průvodce. Na první obrazovce umožňuje změnit základní objekt, který je vázána ObjectDataSource; nechte ji nastavit na `ProductsBLL`. Na další obrazovce uvádí mapování metod ObjectDataSource podkladového objektu. I když jsme výslovně uvedené, který `Insert()` a `Delete()` metody nelze mapovat na všechny metody, pokud přejdete do karty INSERT a DELETE uvidíte, že existuje mapování. Důvodem je, že `ProductsBLL`na `AddProduct` a `DeleteProduct` pomocí metody `DataObjectMethodAttribute` atribut k označení, že jsou výchozí metody pro `Insert()` a `Delete()`, v uvedeném pořadí. Proto průvodce ObjectDataSource vybírá tyto pokaždé, když spustíte průvodce, pokud je některá hodnota, které explicitně určena.

Ponechte `Insert()` metoda odkazující na `AddProduct` metoda, ale znovu nastavit kartě odstranění rozevíracího seznamu na (žádný).


[![Nastavení na kartě Vložení rozevíracího seznamu metodu AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Obrázek 14**: kartu vložení rozevíracího seznamu nastavte `AddProduct` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Nastavení na kartě odstranění rozevíracího seznamu (None)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Obrázek 15**: karta odstranit rozevíracím seznamu (None) nastavte ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Po provedení těchto změn, deklarativní syntaxe ObjectDataSource bude rozšířit, aby obsahovaly `InsertParameters` kolekce, jak je uvedeno níže:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Znovu spustit Průvodce přidány zpět `OldValuesParameterFormatString` vlastnost. Za chvíli zrušte tuto vlastnost nastavit na výchozí hodnotu (`{0}`) nebo odeberte úplně ze deklarativní syntaxi.

S ObjectDataSource poskytování vkládání inteligentních značek DetailsView nyní zahrnují políčka Povolit vložení; vrátíte do návrháře a zaškrtnete tuto možnost. V dalším kroku porovnat dolů DetailsView tak, že má jenom dvě BoundFields - `ProductName` a `UnitPrice` - a CommandField. V tomto okamžiku DetailsView deklarativní syntaxe by měl vypadat jako:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

16 obrázek znázorňuje tuto stránku při zobrazení v tomto okamžiku prostřednictvím prohlížeče. Jak vidíte, DetailsView obsahuje název a cena první produktu (Chai). Co chceme, je však vkládání rozhraní, které poskytuje prostředky pro uživatelům rychle přidání nového produktu do databáze.


[![DetailsView je aktuálně v režimu jen pro čtení vykreslit.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Obrázek 16**: DetailsView je aktuálně v režimu jen pro čtení vykreslit ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Chcete-li zobrazit DetailsView v jeho vkládání režimu, je potřeba nastavit `DefaultMode` vlastnost `Inserting`. Tím se vykreslí DetailsView v režimu vkládání po první návštěvě a udržuje ho došlo po vložení nového záznamu. Jak ukazuje obrázek 17, takové DetailsView poskytuje rychlý rozhraní pro přidávání nového záznamu.


[![DetailsView poskytuje rozhraní pro rychle přidání nového produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Obrázek 17**: DetailsView poskytuje rozhraní pro rychle přidání nového produktu ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Když uživatel zadá název produktu a ceny (například "Pokusná horních" a 1,99 jako obrázek 17) a na příkaz Insert, vyplývá zpětné volání a pak zahájí vkládání pracovního postupu, ukončené v novém záznamu produktu se přidává do databáze. DetailsView udržuje jeho vkládání rozhraní a GridView je automaticky odrážejí svůj zdroj dat tak, aby zahrnovaly nového produktu, jak ukazuje obrázek 18.


![Produktu](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Obrázek 18**: produkt "Pokusná horních" se přidal do databáze


Zatímco rutina GridView v 18 obrázek, nezobrazí pole produktu postrádá z rozhraní DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`a tak dále jsou přiřazeny `NULL` databáze hodnoty. Můžete to zjistit tak, že provedete následující kroky:

1. Přejděte do Průzkumníka serveru v sadě Visual Studio
2. Rozšíření `NORTHWND.MDF` uzel databáze
3. Klikněte pravým tlačítkem na `Products` uzlu tabulky databáze
4. Vyberte zobrazení dat v tabulce

To se zobrazí všechny záznamy v `Products` tabulky. Jak 19 obrázek ukazuje, všechny naše nového produktu sloupců než `ProductID`, `ProductName`, a `UnitPrice` mít `NULL` hodnoty.


[![Produktu pole není zadaný v ovládacím prvku DetailsView jsou přiřazené hodnoty NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Obrázek 19**: produktu pole není zadaný v ovládacím prvku DetailsView přiřazené `NULL` hodnoty ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Chceme poskytnout jiné než výchozí hodnotu `NULL` pro jeden nebo více z těchto hodnot sloupce, buď protože `NULL` není doporučené výchozí možnost nebo protože sám sloupce databáze nepovoluje `NULL` s. K tomu můžeme prostřednictvím kódu programu můžete nastavit hodnoty parametrů prvku DetailsView `InputParameters` kolekce. Toto přiřazení lze provést buď události obslužné rutiny DetailsView `ItemInserting` události nebo ObjectDataSource `Inserting` událostí. Vzhledem k tomu, že jste již hledá pomocí před a po úroveň události na data webové řídit úroveň, podíváme se na události ObjectDataSource pomocí této doby.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Krok 4: Přiřazování hodnot k`CategoryID`a`SupplierID`parametry

V tomto kurzu budeme Představte si, že naše aplikace při přidání nového produktu prostřednictvím tohoto rozhraní se by se měla přiřadit `CategoryID` a `SupplierID` hodnotu 1. Jak už bylo zmíněno dříve, bude ObjectDataSource během procesu úpravy dat má pár událostí před a po úrovně tohoto ještě efektivněji. Při jeho `Insert()` metoda je volána, nejprve vyvolá ObjectDataSource jeho `Inserting` událostí, pak zavolá metodu, jeho `Insert()` metoda byla mapována na a nakonec vyvolá `Inserted` událostí. `Inserting` Obslužné rutiny události nám poskytuje jeden poslední možnost k vylepšení vstupní parametry, nebo přímý operaci zrušte.

> [!NOTE]
> V reálné aplikaci byste pravděpodobně chtít buď umožňují uživateli zadat kategorii a dodavatele nebo by vydat tuto hodnotu pro ně na základě některé kritérií nebo obchodní logiky (nikoli slepě výběr ID 1). Bez ohledu na to příklad ukazuje, jak se prostřednictvím kódu programu z předem úroveň události ObjectDataSource nastavit hodnotu vstupního parametru.


Za chvíli k vytvoření obslužné rutiny události pro ObjectDataSource `Inserting` událostí. Všimněte si, že druhý vstupní parametr obslužné rutiny události je objekt typu `ObjectDataSourceMethodEventArgs`, jehož vlastnost, která má přístup ke kolekci parametrů (`InputParameters`) a vlastnost, která má-li zrušit operaci (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

V tomto okamžiku `InputParameters` vlastnost obsahuje ObjectDataSource `InsertParameters` kolekce s hodnotami z DetailsView přiřazenu. Chcete-li změnit hodnotu některého z těchto parametrů, jednoduše použijte: `e.InputParameters["paramName"] = value`. Proto nastavit `CategoryID` a `SupplierID` upravit tak, aby hodnoty 1 `Inserting` obslužné rutiny události pro vypadat třeba takto:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Tento čas při přidání nového produktu (například: MSN Soda), `CategoryID` a `SupplierID` sloupce nového produktu jsou nastaveny na hodnotu 1 (viz obrázek 20).


[![Teď máte nové produkty jejich CategoryID a ID dodavatele hodnoty nastavené na 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Obrázek 20**: nové produkty nyní mají jejich `CategoryID` a `SupplierID` hodnoty nastavena na hodnotu 1 ([Kliknutím zobrazit obrázek v plné velikosti](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Souhrn

Ovládací prvek webu data i ObjectDataSource pokračujte během úprava, vkládání a odstraňování procesu, určitý počet událostí před a po úrovně. V tomto kurzu jsme zkontrolován předem úroveň události a viděli, jak tyto slouží k přizpůsobení vstupní parametry nebo zrušit operace změny dat z ovládací prvek webu data a události na ObjectDataSource zcela i. V dalším kurzu podíváme vytváření a používání obslužných rutin událostí pro události po úrovně.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Jackie Goor a Liz Shulok. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](an-overview-of-inserting-updating-and-deleting-data-cs.md)
[další](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
