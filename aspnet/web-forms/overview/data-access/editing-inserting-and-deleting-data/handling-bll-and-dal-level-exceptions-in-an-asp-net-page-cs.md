---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Zpracování výjimek na úrovni knihoven BLL a DAL na stránce ASP.NET (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu uvidíme, jak zobrazit popisný informativní zpráva výjimky se budou objevovat při vložení, aktualizaci nebo odstranění operace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c971b69605055e587aefe92fb4fc755176e6b53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393109"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Zpracování výjimek na úrovni knihoven BLL a DAL na stránce ASP.NET (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) nebo [stahovat PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> V tomto kurzu uvidíme, jak zobrazit popisné a informativní chybová zpráva při vložení, aktualizaci nebo odstranění operace datům v technologii ASP.NET ovládací prvek webového dojde k výjimce.


## <a name="introduction"></a>Úvod

Práce s daty z webové aplikace ASP.NET pomocí architektury vrstvenou aplikaci zahrnuje následující tři hlavní kroky:

1. Určete, jakou metodu vrstvy obchodní logiky se musí zavolat a jaké parametr hodnoty komunikace. Hodnoty parametru může být pevně zakódované, prostřednictvím kódu programu přiřazen nebo vstupy zadané uživatelem.
2. Vyvolání metody.
3. Zpracování výsledků. Při volání metody BLL, která vrací data, může to zahrnovat vazbě dat na data webový ovládací prvek. U knihoven BLL metod, které mění data patří mezi ně některé akce na základě vrácené hodnoty nebo řádně zpracování jakoukoliv výjimku, která vznikla v kroku 2.

Jak jsme viděli v [předchozí kurz o službě](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), ObjectDataSource i data webové ovládací prvky poskytují body rozšiřitelnosti pro kroky 1 a 3. Prvku GridView, například aktivuje její `RowUpdating` událostí před přiřazením hodnoty příslušných polí své ObjectDataSource `UpdateParameters` kolekce; jeho `RowUpdated` událost je aktivována po dokončení operace prvku ObjectDataSource.

Už jsme jste prozkoumat události, které aktivují během kroku 1 a jste viděli, jak můžete použít k přizpůsobení vstupní parametry, nebo operaci zrušte. V tomto kurzu jsme vám zapnout pozornost na události, které se aktivují po dokončení operace. Pomocí těchto obslužných rutin událostí po úrovně, které můžeme, mimo jiné určení, pokud v průběhu operace došlo k výjimce a zpracoval ji řádně, zobrazení popisné a informativní chybová zpráva na obrazovce a ne jako výchozí se použije standardní technologie ASP.NET stránce výjimek.

Pro ilustraci, práce se tyto události po úrovně, Pojďme vytvořit stránku, která zobrazuje seznam produktů v upravitelné prvku GridView. Při aktualizaci produktu, pokud je výjimka vyvolána naše technologie ASP.NET stránce se zobrazí zprávy zadejte krátký nad prvek GridView s vysvětlením, že došlo k problému. Pusťme se do práce!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Krok 1: Vytvoření upravitelné GridView produktů

V předchozím kurzu jsme vytvořili upravovat prvek GridView s právě dvě pole, `ProductName` a `UnitPrice`. To vyžaduje další přetížení pro vytvoření `ProductsBLL` třídy `UpdateProduct` metoda, která přijatelný jenom tři vstupní parametry (produktu název, cena za jednotku a ID) názvem na rozdíl od parametr pro každý produkt pole. Pro účely tohoto kurzu vyzkoušejme tento postup opakujte, vytváření upravitelné prvku GridView, která zobrazuje název produktu, množství na jednotku a cena za jednotku a jednotky v zásobách, ale umožňuje pouze název, cena za jednotku a jednotky v zásobách upravit.

Pro tento scénář budeme potřebovat další přetížení `UpdateProduct` metody, který přijímá čtyři parametry: název produktu, cena za jednotku, jednotky na skladě a ID. Přidejte následující metodu do `ProductsBLL` třídy:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Pomocí této metody kompletní jsme připraveni vytvořit stránku ASP.NET, která umožňuje upravit tyto čtyři pole konkrétního produktu. Otevřít `ErrorHandling.aspx` stránku `EditInsertDelete` složky a přidat na stránku prostřednictvím návrháře GridView. Svázání prvku GridView nový prvek ObjectDataSource, mapování `Select()` metodu `ProductsBLL` třídy `GetProducts()` metoda a `Update()` metodu `UpdateProduct` přetížení právě vytvořili.


[![Použijte přetížení metody UpdateProduct, která přijímá čtyři vstupní parametry](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Obrázek 1**: použijte `UpdateProduct` přetížení, že přijímá čtyři vstupní parametry metody ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Tím se vytvoří ObjectDataSource s `UpdateParameters` kolekce s čtyři parametry a GridView s polem pro každé pole produktu. Deklarativní ObjectDataSource přiřadí `OldValuesParameterFormatString` vlastnost hodnota `original_{0}`, které způsobí výjimku, protože naše třída BLL Neočekáváme, že vstupní parametr s názvem `original_productID` předat. Nezapomeňte odebrat toto nastavení zcela z deklarativní syntaxe (nebo ji nastavte na výchozí hodnotu `{0}`).

V dalším kroku Zredukovat GridView zahrnout pouze `ProductName`, `QuantityPerUnit`, `UnitPrice`, a `UnitsInStock` BoundFields. Také můžete použít libovolné pole formátování na úrovni uznáte za nezbytné (jako je například změna `HeaderText` vlastnosti).

V předchozím kurzu jsme se podívali na tom, jak formátovat `UnitPrice` Vlastnost BoundField jako měnu v režimu jen pro čtení i v režimu úprav. Pojďme si stejné tady. Připomínáme, že to vyžaduje nastavení vlastnost BoundField `DataFormatString` vlastnost `{0:c}`, jeho `HtmlEncode` vlastnost `false`a jeho `ApplyFormatInEditMode` k `true`, jak je znázorněno na obrázku 2.


[![Nakonfigurovat vlastnost UnitPrice BoundField zobrazení jako měna](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Obrázek 2**: konfigurace `UnitPrice` Vlastnost BoundField chcete zobrazit ve formátu měny ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Formátování `UnitPrice` jako měnu v rozhraní úprav vyžaduje vytvoření obslužné rutiny události pro prvku GridView `RowUpdating` událost, která analyzuje řetězec ve formátu měny do `decimal` hodnotu. Vzpomeňte si, že `RowUpdating` obslužné rutiny události z poslední kurz zkontrolovány také zajistit, aby uživatel zadat `UnitPrice` hodnotu. Ale pro účely tohoto kurzu povolíme uživatele chcete vynechat, nechte cena.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Zahrnuje naše GridView `QuantityPerUnit` Vlastnost BoundField, ale tato vlastnost BoundField by měly být pouze pro účely zobrazení a neměla by být upravitelné uživatelem. Toto uspořádání, stačí nastavit BoundFields `ReadOnly` vlastnost `true`.


[![Nastavte vlastnost QuantityPerUnit BoundField jen pro čtení](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Obrázek 3**: Ujistěte se, `QuantityPerUnit` Vlastnost BoundField jen pro čtení ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


A konečně zaškrtněte políčko Povolit úpravy z inteligentních značek v prvku GridView. Po dokončení těchto kroků `ErrorHandling.aspx` stránky návrháře vypadat podobně jako na obrázku 4.


[![Odeberte všechny kromě potřebnou BoundFields a kontrola povolit úpravy zaškrtávací políčko](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Obrázek 4**: Odeberte všechny kromě the potřeby BoundFields a zaškrtněte políčko Povolit úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


V tuto chvíli máme seznam všech produktů `ProductName`, `QuantityPerUnit`, `UnitPrice`, a `UnitsInStock` pole; však pouze `ProductName`, `UnitPrice`, a `UnitsInStock` pole lze upravovat.


[![Uživatelé mohou nyní snadno upravit produkty, názvy, ceny a jednotky v uložených pole](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Obrázek 5**: uživatelé mohou nyní snadno upravit produkty, které se názvy, ceny a jednotky v zásobách pole ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Krok 2: Řádně zpracování výjimek na úrovni DAL

Během naší upravitelné GridView nádherně funguje, když uživatelé zadat platné hodnoty pro název, cena a jednotek v zásobách upravených produktu, zadáte neplatné hodnoty způsobí výjimku. Například vynechání `ProductName` hodnotu způsobí, že [nonullallowedexception –](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) vyvolání od `ProductName` vlastnost `ProdcutsRow` třída má jeho `AllowDBNull` vlastnost nastavena na `false`; Pokud databáze je mimo provoz, `SqlException` budou vyvolány metodou TableAdapter při pokusu o připojení k databázi. Bez cokoli podnikat, tyto výjimky vyvolat z vrstvy přístupu k datům na vrstvy obchodní logiky a pak na stránce technologie ASP.NET a nakonec na modul runtime ASP.NET.

V závislosti na konfiguraci webové aplikace a určuje, jestli navštívený aplikaci `localhost`, neošetřené výjimky může vést k obecné chybě serveru stránky, podrobnou chybovou zprávu nebo uživatelsky přívětivý webové stránky. Zobrazit [webové zpracování chyb aplikace v ASP.NET](http://www.15seconds.com/issue/030102.htm) a [prvek customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) Další informace o jak modul runtime ASP.NET reaguje na vydá nezachycenou výjimku.

Obrázek 6 se zobrazuje obrazovka došlo při pokusu o aktualizaci produktu bez zadání `ProductName` hodnotu. Toto je výchozí podrobnou chybovou zprávu se zobrazí, když přes něj procházejí `localhost`.


[![Vynechání podrobnosti výjimky budou zobrazovaný název produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Obrázek 6**: Vynechání produktu název bude zobrazení Podrobnosti o výjimce ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Tyto podrobnosti výjimky jsou užitečné při testování aplikace, nabízí ten samý koncového uživatele s obrazovkou, i v případě výjimky je menší než ideální. Koncový uživatel nejspíš nebude vědět, co `NoNullAllowedException` je nebo důvod, proč se způsobilo. Lepším řešením je uživateli zprostředkovali přívětivější zpráva s vysvětlením, že došlo k potížím při pokusu o aktualizaci produktu.

Pokud dojde k výjimce při provádění této operace, události po úrovně prvku ObjectDataSource i ovládací prvek webových dat poskytují způsob ji detekuje a zrušit výjimku z šíření až modul runtime ASP.NET. V našem příkladu vytvoříme obslužnou rutinu události pro prvku GridView `RowUpdated` událost, která určuje, zda výjimka byla aktivována a pokud ano, zobrazí podrobnosti o výjimce v ovládacím prvku popisek Web.

Začněte tím, že přidáte popisek na stránku ASP.NET, nastavení jeho `ID` vlastnost `ExceptionDetails` a vymazání jeho `Text` vlastnost. Pokud chcete vykreslení oko uživatele do této zprávy, nastavte jeho `CssClass` vlastnost `Warning`, tedy třídu šablony stylů CSS, přidali jsme do `Styles.css` souboru v předchozím kurzu. Připomínáme, že tato třída šablon stylů CSS způsobí, že popisek zobrazený červené, kurzíva, tučné písmo, velmi velkým písmem.


[![Přidání ovládacího prvku popisek na stránku](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Obrázek 7**: Přidání ovládacího prvku popisek na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Protože chceme, aby tento popisek webový ovládací prvek uvidí pouze ihned poté, co došlo k výjimce, nastavte jeho `Visible` vlastnost na hodnotu false v `Page_Load` obslužné rutiny události:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

S tímto kódem, na první návštěvě stránky a následné zpětného odeslání `ExceptionDetails` bude mít ovládací prvek jeho `Visible` nastavenou na `false`. I v případě DAL - nebo úrovni knihoven BLL výjimku, která můžeme detekovat v prvku GridView `RowUpdated` obslužná rutina události, nastavíme `ExceptionDetails` ovládacího prvku `Visible` vlastnost na hodnotu true. Protože obslužné rutiny událostí pro ovládací prvek webové nastat po `Page_Load` obslužné rutiny události v životního cyklu stránky, popisek se zobrazí. Na další zpětné volání, ale `Page_Load` se vrátí obslužná rutina události `Visible` vlastnost zpět do `false`, znova skryjete v zobrazení.

> [!NOTE]
> Můžeme odebrat také nezbytná pro nastavení `ExceptionDetails` ovládacího prvku `Visible` vlastnost `Page_Load` přiřazením jeho `Visible` vlastnost `false` v deklarativní syntaxe a zakázání svůj stav zobrazení (nastavení jeho `EnableViewState` vlastnost `false`). Použijeme tuto alternativním přístupem v budoucích kurzech.


Pomocí ovládacího prvku popisku přidali, naším dalším krokem je vytvoření obslužné rutiny události pro prvku GridView `RowUpdated` událostí. V Návrháři vyberte prvku GridView, přejít do okna Vlastnosti a klikněte ikonu blesku, výpis událostí prvku GridView. Musí již být položka existuje prvku GridView `RowUpdating` události, jako jsme vytvořili dříve v tomto kurzu obslužnou rutinu události pro tuto událost. Vytvořte obslužnou rutinu události pro `RowUpdated` také události.


![Vytvořte obslužnou rutinu události pro událost RowUpdated metody prvku GridView.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Obrázek 8**: vytvořit obslužnou rutinu události pro prvku GridView `RowUpdated` událostí


> [!NOTE]
> Můžete také vytvořit obslužnou rutinu události prostřednictvím rozevírací seznamy v horní části souboru kódu na pozadí třídy. Vyberte z rozevíracího seznamu na levé straně prvku GridView a `RowUpdated` událostí než ten, na pravé straně.


Vytvoření této obslužné rutiny události bude přidejte následující kód do třídy modelu code-behind stránky ASP.NET:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Tato obslužná rutina události druhé vstupní parametr je objekt typu [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), která má tři vlastnosti týkající se zpracování výjimek:

- `Exception` odkaz na vyvolanou výjimku; Pokud byla vyvolána žádná výjimka, tato vlastnost bude obsahovat hodnotu `null`
- `ExceptionHandled` Logická hodnota, která určuje, zda byla výjimka zpracována v `RowUpdated` obslužné rutiny události; Pokud `false` (výchozí), výjimka je znovu vyvolána percolating až modul runtime ASP.NET
- `KeepInEditMode` Pokud hodnotu `true` upravené řádky GridView zůstane v režimu úprav; Pokud `false` řádku prvku GridView (výchozí), se vrátí zpátky na jeho režimu jen pro čtení

Náš kód, pak by měl zkontrolujte `Exception` není `null`, což znamená, že při provádění operace došlo k výjimce. Pokud je to tento případ, chceme:

- Uživatelsky přívětivé zprávy v zobrazení `ExceptionDetails` popisek
- Označuje, že výjimka byla zpracována.
- Zachovat řádky GridView v režimu úprav

Tento následující kód provede tyto cíle:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Tato obslužná rutina události začíná tak, že zkontrolujete, jestli `e.Exception` je `null`. Pokud ne, `ExceptionDetails` popisku `Visible` je nastavena na `true` a jeho `Text` vlastnost "Došlo k potížím při aktualizaci produktu." Podrobnosti o skutečné výjimce, která byla vydána jsou umístěny v `e.Exception` objektu `InnerException` vlastnost. Prozkoumat Tento vnitřní výjimka, a pokud určitého typu, se připojí další, užitečné zprávy `ExceptionDetails` popisku `Text` vlastnost. A konečně `ExceptionHandled` a `KeepInEditMode` obě vlastnosti jsou nastaveny na `true`.

Obrázek 9 ukazuje snímek obrazovky na této stránce při vynechání název produktu; Obrázek 10 ukazuje výsledky při zadávání neplatné `UnitPrice` hodnotu (-50).


[![Vlastnost ProductName BoundField musí obsahovat hodnotu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Obrázek 9**: `ProductName` Vlastnost BoundField musí obsahovat hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![Záporné hodnoty UnitPrice není povoleno](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Obrázek 10**: negativní `UnitPrice` není povolené jsou hodnoty ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Tím, že nastavíte `e.ExceptionHandled` vlastnost `true`, `RowUpdated` obslužné rutiny událostí udává, že ošetřila výjimku. Výjimky proto nebude šířit do modulu runtime ASP.NET.

> [!NOTE]
> Obrázky 9 a 10 zobrazit řádné způsob, jak zpracovat výjimky vyvolané z důvodu neplatný uživatelský vstup. V ideálním případě však tyto neplatné vstupní se nikdy dosah vrstvy obchodní logiky na prvním místě, protože stránky ASP.NET by měl zajistit, aby vstupů uživatele platná před vyvoláním `ProductsBLL` třídy `UpdateProduct` metody. V následujícím kurzem uvidíme přidání validačních ovládacích prvků do rozhraní úpravy a vložení zajistit, aby data odeslaná do vrstvy obchodní logiky odpovídá obchodní pravidla. Ovládací prvky ověřování nejen zakázat vyvolání `UpdateProduct` metodu, dokud uživatel uvedl dat je platný, ale také poskytují další činnost koncového uživatele pro identifikaci problémů vstupní data.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Krok 3: Řádně zpracování výjimek na úrovni knihoven BLL

Při vkládání, aktualizaci nebo odstraňování dat, vrstva přístupu k datům může výjimku i v případě chybu související s daty. Databáze může být offline, možná jste využili sloupec tabulky databáze hodnota zadaná nebo byla porušena omezení úrovně tabulky. Kromě výhradně data související výjimky vrstvy obchodní logiky můžete výjimky o tom, kdy byla porušena obchodní pravidla. V [vytvoření vrstvy obchodní logiky](../introduction/creating-a-business-logic-layer-cs.md) kurz, například jsme přidali kontrolu obchodní pravidlo na původní `UpdateProduct` přetížení. Konkrétně Pokud uživatel byl označení produkt, protože ukončena, vyžádali jsme, že produkt nebudou jako jediný poskytuje jeho dodavatele. Pokud tato podmínka byla porušena, `ApplicationException` byla vyvolána.

Pro `UpdateProduct` v tomto kurzu vytvořili přetížení, Pojďme přidat obchodní pravidlo, které zakazuje `UnitPrice` pole z nastavena na novou hodnotu, která je více než dvojnásobný původní `UnitPrice` hodnotu. Chcete-li to provést, upravte `UpdateProduct` přetížení tak, aby této kontrole a vyvolá výjimku `ApplicationException` Pokud je toto pravidlo porušeno. Aktualizovaná metoda takto:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Tato změna způsobí libovolné price aktualizaci, která se více než dvojnásobný stávající ceny `ApplicationException` vyvolání. Stejně jako výjimky vyvolané z vrstvy DAL tomto BLL vyvolána `ApplicationException` můžete zjištěna a zpracovávány v prvku GridView `RowUpdated` obslužné rutiny události. Ve skutečnosti `RowUpdated` kód obslužné rutiny události, jak je uvedená, správně rozpozná tuto výjimku a zobrazí `ApplicationException`společnosti `Message` hodnotu vlastnosti. Obrázku 11 můžete vidět snímku obrazovky, když se uživatel pokusí aktualizovat cena Chai $ 50,00, což je více než double jeho aktuální cena 19,95.


[![Obchodní pravidla zakázat zvýšení ceny, které víc než dvakrát ceny produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Obrázek 11**: obchodní pravidla zakázat zvýšení, které víc než dvakrát ceny produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> V ideálním případě by být refaktorovány naše obchodní logiky pravidla z celkového počtu `UpdateProduct` přetížení metody a do běžnou metodu. To je ponecháno cvičení pro čtečku.


## <a name="summary"></a>Souhrn

Během vkládání, aktualizaci a odstranění operace ovládací prvek webových dat a související ObjectDataSource vyvolat události před instrumentací a po ní úrovně tuto zarážku aktuální operace. Jak jsme viděli v tomto kurzu a ta předchozí při práci s upravitelné GridView prvku GridView `RowUpdating` událost je aktivována, za nímž následuje ObjectDataSource `Updating` událostí, v tomto okamžiku je příkaz aktualizace provedené prvku ObjectDataSource základní objekt. Po dokončení operace prvku ObjectDataSource `Updated` událost je aktivována, za nímž následuje prvku GridView `RowUpdated` událostí.

Můžeme vytvořit obslužné rutiny událostí pro události předem úrovně dokážeme vstupní parametry, nebo pro události po úrovně, pokud chcete zkontrolovat a reakce na výsledky operace. Obslužné rutiny událostí po úrovně se nejčastěji používají ke zjištění, zda během operace došlo k výjimce. I v případě výjimku můžete tyto obslužné rutiny událostí po úrovně volitelně zpracování výjimky na své vlastní. V tomto kurzu jsme viděli, jak zpracovat takovou výjimku zobrazením popisná chybová zpráva.

V dalším kurzu uvidíme, jak snížit pravděpodobnost, že výjimky vyplývající z problémů formátování dat (jako je zadání záporné `UnitPrice`). Konkrétně zaměříme na přidání validačních ovládacích prvků do rozhraní pro úpravy a vložení.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Liz Shulok. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [další](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
