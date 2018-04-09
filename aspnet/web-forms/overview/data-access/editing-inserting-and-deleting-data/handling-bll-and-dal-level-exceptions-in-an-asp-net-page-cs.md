---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Zpracování výjimek BLL a DAL úroveň na stránku ASP.NET (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu zjistíme, jak zobrazit popisné a informativní chybová zpráva by dojít k výjimce během vložení, aktualizaci nebo odstranění operace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: e7589584f3b0773a739b785c433ec45eeac3607e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Zpracování výjimek BLL a DAL úroveň na stránku ASP.NET (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) nebo [stáhnout PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> V tomto kurzu jsme se zobrazí zobrazení popisné a informativní chybová zpráva při vložení, aktualizaci nebo operace odstranění dat ASP.NET ovládací prvek webu dojde k výjimce.


## <a name="introduction"></a>Úvod

Práci s daty z webové aplikace ASP.NET pomocí architektury vrstvené aplikace zahrnuje následující tři obecné kroky:

1. Určete, jakou metodu vrstvu obchodní logiky musí být volána a jaké parametr hodnoty k předejte ji. Hodnoty parametru může být naprogramováno, prostřednictvím kódu programu přiřazen nebo vstupy zadané uživatelem.
2. Vyvolání metody.
3. Zpracování výsledků. Při volání metody BLL metodu, která vrátí data, to může zahrnovat vazbě dat na data ovládací prvek webu. Pro BLL metody, které upravují data to může zahrnovat provedení některé akce na základě návratové hodnoty nebo pohodlné zpracování všechny výjimky, které vznikly v kroku 2.

Jak jsme viděli v [předchozí kurzu](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), ObjectDataSource i data ovládací prvky webového zadejte body rozšiřitelnosti pro kroků 1 a 3. Rutina GridView, například aktivuje její `RowUpdating` událostí před jeho hodnoty polí přiřazení k jeho ObjectDataSource `UpdateParameters` kolekce; jeho `RowUpdated` událost se vyvolá po dokončení operace ObjectDataSource.

Jsme jste už se zaměřili na události, které aktivují během kroku 1 a viděli, jak můžete použít k přizpůsobení vstupní parametry nebo operaci zrušte. V tomto kurzu jsme budete zapněte naše pozornost k událostem, které budou spuštěny po dokončení operace. Pomocí těchto obslužné rutiny událostí po úrovně, které můžeme, mimo jiné určit, pokud při operaci došlo k výjimce a zpracování řádně, zobrazení popisné a informativní chybová zpráva na obrazovce a ne jako výchozí bude použit standardní technologie ASP.NET stránka výjimka.

Pro ilustraci práce se tyto události po úrovni, vytvoříme stránky, který se zobrazuje seznam produktů v upravitelné GridView. Při aktualizaci produktu, pokud se výjimka vyvolána naše ASP.NET stránky se zobrazí zpráva krátké výše GridView, která vysvětluje, že došlo k potížím. Můžeme začít!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Krok 1: Vytvoření upravitelné GridView produktů

V předchozích kurzu jsme vytvořili upravitelné GridView se právě dvěma poli `ProductName` a `UnitPrice`. To vyžaduje další přetížení pro vytvoření `ProductsBLL` třídy `UpdateProduct` metoda, ten, který pouze přijata tři vstupní parametry (produktu název, jednotkové ceny a ID) názvem na rozdíl od parametr pro každé pole produktu. V tomto kurzu praxi, můžeme tento postup opakujte, vytváření upravitelné GridView, který zobrazí název produktu, množství jednotce, jednotkové ceny a jednotky na skladě, ale umožňuje pouze název, jednotkové ceny a jednotky ve skladu k provádění úprav.

Pro tento scénář budeme potřebovat další přetížení `UpdateProduct` metoda, ten, který přijímá čtyř parametrů: název produktu, jednotkové ceny, jednotky v stock a ID. Přidejte následující metodu do `ProductsBLL` třídy:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Pomocí této metody dokončení jsme připraveni vytvořit stránku ASP.NET, která umožňuje pro úpravy tyto čtyři pole určitého produktu. Otevřete `ErrorHandling.aspx` stránku `EditInsertDelete` složky a přidejte GridView na stránku pomocí návrháře. GridView vytvořit vazbu k nové ObjectDataSource, mapování `Select()` metodu `ProductsBLL` třídy `GetProducts()` metoda a `Update()` metodu `UpdateProduct` přetížení právě vytvořili.


[![Použijte přetížení metody UpdateProduct, který přijímá čtyři vstupní parametry](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Obrázek 1**: použití `UpdateProduct` přetížení aby přijímá čtyři vstupní parametry metody ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Tím se vytvoří ObjectDataSource s `UpdateParameters` kolekci pomocí čtyř parametrů a GridView s polem pro každé pole produktu. Přiřadí ObjectDataSource deklarativní `OldValuesParameterFormatString` vlastnost hodnota `original_{0}`, které způsobí výjimku, protože naše třída BLL neočekávané vstupní parametr s názvem `original_productID` být předán. Nezapomeňte odebrat toto nastavení zcela z deklarativní syntaxe (nebo ji nastavte na výchozí hodnotu `{0}`).

V dalším kroku porovnat dolů GridView zahrnout pouze `ProductName`, `QuantityPerUnit`, `UnitPrice`, a `UnitsInStock` BoundFields. Také Nebojte se použijte požadované úrovni poli formátování, vám přijde nezbytné (jako je například změna `HeaderText` vlastnosti).

V předchozích kurzu jsme se podívali na způsob formátování `UnitPrice` BoundField jako měnu v režimu jen pro čtení i v režimu úprav. Umožňuje provést stejný sem. Odvolat, že to vyžaduje nastavení BoundField `DataFormatString` vlastnost `{0:c}`, jeho `HtmlEncode` vlastnost `false`a jeho `ApplyFormatInEditMode` k `true`, jak je znázorněno na obrázku 2.


[![Konfigurace UnitPrice BoundField k zobrazení jako měny](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Obrázek 2**: konfigurace `UnitPrice` BoundField displej jako měny ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Formátování `UnitPrice` jako měny v rozhraní úpravy vyžaduje vytvoření obslužné rutiny události pro prvku GridView `RowUpdating` událost, která analyzuje řetězec ve formátu měny do `decimal` hodnotu. Odvolat, který `RowUpdating` obslužné rutiny události z poslední kurzu zaškrtnuta možnost také zajistit, aby uživatel zadal `UnitPrice` hodnotu. Ale pro účely tohoto kurzu umožňuje povolit uživatelům vynechejte cenu.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Zahrnuje naše GridView `QuantityPerUnit` BoundField, ale tato BoundField by měl být pouze pro účely zobrazení a neměla by být upravitelné uživatelem. Však jednoduše nastavit BoundFields `ReadOnly` vlastnost `true`.


[![Zkontrolujte QuantityPerUnit BoundField jen pro čtení](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Obrázek 3**: Zkontrolujte `QuantityPerUnit` BoundField jen pro čtení ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Nakonec zaškrtněte políčko Povolit úpravy z prvku GridView inteligentních značek. Po dokončení těchto kroků `ErrorHandling.aspx` Návrhář stránky by měl vypadat podobně jako na obrázku 4.


[![Odeberte všechny ale nutný BoundFields a zkontrolujte povolit úpravy zaškrtávací políčko](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Obrázek 4**: Odeberte všechny ale potřeby BoundFields a zaškrtněte políčko Povolit úpravy ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


V tomto okamžiku máme seznam všech tyto produkty `ProductName`, `QuantityPerUnit`, `UnitPrice`, a `UnitsInStock` polí; však pouze `ProductName`, `UnitPrice`, a `UnitsInStock` pole lze upravovat.


[![Uživatelé nyní můžete snadno upravit názvy produkty, ceny a jednotky v uložených pole](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Obrázek 5**: názvy uživatelů lze nyní snadno upravit produkty, ceny a jednotky v Stock pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Krok 2: Pohodlné zpracování výjimky na úrovni DAL

Při našich upravitelné GridView nádherně funguje, když uživatelé zadat platné hodnoty pro název, ceny a jednotky v stock upravená produktu, zadávání neplatné hodnoty mít za následek výjimku. Například vynechání `ProductName` hodnotu příčiny [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) vyvolání od `ProductName` vlastnost v `ProdcutsRow` třída má jeho `AllowDBNull` vlastnost nastavena na hodnotu `false`; Pokud databáze je vypnutý, `SqlException` bude vyvolané TableAdapter při pokusu o připojení k databázi. Bez provedení akce, tyto výjimky vyvolat z Data Access Layer vrstvu obchodní logiky a potom na stránku ASP.NET a nakonec na modulem runtime ASP.NET.

V závislosti na konfiguraci webové aplikace a zda právě navštíveného aplikace z `localhost`, neošetřené výjimky může způsobit na stránce Obecná chyba serveru, sestava podrobnosti o chybě nebo uživatelsky přívětivý webové stránky. V tématu [webové zpracování chyb aplikace technologie ASP.NET](http://www.15seconds.com/issue/030102.htm) a [customErrors Element](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) Další informace o odpovědí modulem runtime ASP.NET na nezachycenou výjimku.

Obrázek 6 ukazuje na obrazovce došlo při pokusu o aktualizaci produktu bez zadání `ProductName` hodnotu. Toto je výchozí podrobné informace o chybě sestava zobrazí, když procházejícího `localhost`.


[![Vynechání název produktu, bude zobrazení Podrobnosti o výjimce](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Obrázek 6**: Vynechání podrobnosti výjimky zobrazení bude název produktu ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Tyto podrobnosti výjimky jsou užitečné při testování aplikace, koncový uživatel prezentuje obrazovky při krátkodobém výjimku je menší než vhodné. Koncový uživatel nejspíš nebude vědět, co `NoNullAllowedException` je nebo proč byl způsobený. Lepší přístupem je uživateli zprostředkovali srozumitelnější zprávu, která vysvětluje, že došlo k potížím, pokus o aktualizaci produktu.

Pokud dojde k výjimce při provádění této operace, po úroveň události v ObjectDataSource a ovládací prvek webu data poskytují prostředky pro zjišťovat a zrušit výjimka v proniknutí až modulem runtime ASP.NET. Pro náš příklad vytvoříme obslužné rutiny události pro prvku GridView `RowUpdated` událost, která určuje, zda výjimku je aktivována a pokud ano, zobrazí podrobnosti o výjimce v ovládacím prvku popisek Web.

Začněte přidáním štítek na stránku ASP.NET, nastavení jeho `ID` vlastnost `ExceptionDetails` a vymazání jeho `Text` vlastnost. Chcete-li kreslení oko uživatele na tuto zprávu, nastavte jeho `CssClass` vlastnost `Warning`, tedy třídu CSS jsme přidali do `Styles.css` souboru v předchozí kurzu. Odvolat, že tato třída šablon stylů CSS způsobí, že text popisku, který se zobrazí písmeny red, kurzíva, tučné písmo, velmi velké.


[![Přidání ovládacího prvku popisek na stránku](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Obrázek 7**: Přidání ovládacího prvku popisek na stránku ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Vzhledem k tomu, že má být tento ovládací prvek popisek webu mají být zobrazeny pouze okamžitě po došlo k výjimce, nastavte jeho `Visible` vlastnost na hodnotu false v `Page_Load` obslužné rutiny události:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

S tímto kódem, na první návštěvě stránky a následné postback `ExceptionDetails` ovládací prvek bude mít jeho `Visible` vlastnost nastavena na hodnotu `false`. Při krátkodobém úrovni DAL nebo BLL výjimky, které jsme můžete zjistit v prvku GridView `RowUpdated` obslužné rutiny události, se nastaví `ExceptionDetails` ovládacího prvku `Visible` vlastnost na hodnotu true. Vzhledem k tomu, že obslužných rutin událostí pro ovládací prvek webové následovat po `Page_Load` obslužné rutiny událostí v životním cyklu stránky, se zobrazí popisek. Na další zpětné volání, ale `Page_Load` obslužné rutiny události vrátíte `Visible` vlastnost zpět do `false`, znovu skrytí ze zobrazení.

> [!NOTE]
> Jsme odebrat také nezbytné pro nastavení `ExceptionDetails` ovládacího prvku `Visible` vlastnost `Page_Load` přiřazením jeho `Visible` vlastnost `false` v deklarativní syntaxi a zakázání svůj stav zobrazení (nastavení jeho `EnableViewState` vlastnost `false`). V budoucích kurzu použijeme tento alternativní přístup.


U ovládacího prvku popisek přidat naše dalším krokem je vytvoření obslužné rutiny události pro prvku GridView `RowUpdated` událostí. Vyberte GridView v návrháři, přejděte do okna vlastností a klikněte na bolt ikonu, výpis prvku GridView události. Měla by existovat již položka existuje pro prvku GridView `RowUpdating` události, jako jsme vytvořili obslužné rutiny události pro tuto událost v tomto kurzu. Vytvoření obslužné rutiny událostí `RowUpdated` také událostí.


![Vytvoření obslužné rutiny události pro událost RowUpdated GridView.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Obrázek 8**: vytvoření obslužné rutiny události pro prvku GridView `RowUpdated` událostí


> [!NOTE]
> Můžete také vytvořit obslužnou rutinu události pomocí rozevíracích seznamů v horní části třídy souboru kódu na pozadí. V rozevíracím seznamu na levé straně vyberte GridView a `RowUpdated` událost z jedné na pravé straně.


Vytvoření této obslužné rutiny události bude do třídy kódu stránky ASP.NET přidejte následující kód:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Druhý vstupní parametr této obslužné rutiny události je objekt typu [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), který má tři vlastnosti týkající se zpracování výjimky:

- `Exception` odkaz na vyvolaná výjimka; Pokud žádné došlo k výjimce, tato vlastnost bude mít hodnotu `null`
- `ExceptionHandled` Logická hodnota, která označuje, zda bylo zpracováno výjimka v `RowUpdated` obslužné rutiny události; Pokud `false` (výchozí), výjimka je znovu vyvolána, percolating až modulem runtime ASP.NET
- `KeepInEditMode` Pokud nastavena na `true` upravená řádek GridView zůstane v režimu úprav; Pokud `false` (výchozí), řádek GridView vrátí zpět na jeho režimu jen pro čtení

Naše kód, pak by měla zkontrolovat Pokud `Exception` není `null`, což znamená, že při provádění operace došlo k výjimce. Pokud je to tento případ, chceme:

- Zobrazit zprávu v uživatelsky přívětivý `ExceptionDetails` popisek
- Označuje, že byla zpracována výjimka
- Zachovat GridView řádek v režimu úprav

Tento následující kód provede tyto cíle:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Tuto obslužnou rutinu události začne kontrolou a zjistěte, zda `e.Exception` je `null`. Pokud není, `ExceptionDetails` popisku `Visible` je nastavena na `true` a jeho `Text` vlastnost "Došlo k potížím při aktualizaci produktu." Podrobnosti o skutečné výjimku, která byla vydána jsou umístěny ve `e.Exception` objektu `InnerException` vlastnost. Vnitřní výjimku je zkontrolován, a pokud je konkrétní typ, připojí se zprávou další, užitečné k `ExceptionDetails` popisku `Text` vlastnost. Nakonec `ExceptionHandled` a `KeepInEditMode` obě vlastnosti jsou nastaveny na `true`.

Obrázek 9 ukazuje snímek obrazovky tuto stránku při vynechání název produktu; Obrázek 10 ukazuje výsledky při zadávání neplatné `UnitPrice` hodnotu (-50).


[![ProductName BoundField musí obsahovat hodnotu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Obrázek 9**: `ProductName` BoundField musí obsahovat hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![Záporné hodnoty UnitPrice je zakázán.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Obrázek 10**: záporné `UnitPrice` není povolené jsou hodnoty ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Nastavením `e.ExceptionHandled` vlastnost `true`, `RowUpdated` obslužné rutiny události oznámilo, že se má zajistit výjimku. Proto nebude výjimka rozšíří až modulem runtime ASP.NET.

> [!NOTE]
> Následující obrázky 9 a 10 zobrazit řádně způsob zpracování výjimek vyvolaných z důvodu neplatné uživatelský vstup. V ideálním případě by ale tyto neplatné vstupní se nikdy reach vrstvu obchodní logiky v prvé řadě jako stránku ASP.NET se ujistěte, že vstupy uživatele jsou platné před vyvoláním `ProductsBLL` třídy `UpdateProduct` metoda. V našem další kurzu ukážeme, jak přidat ověřovací ovládací prvky pro úpravy a vkládání rozhraní zajistit, aby data odeslaná na vrstvu obchodní logiky vyhovuje obchodní pravidla. Ovládací prvky ověřování nejen zabránit vyvolání `UpdateProduct` metodu, dokud uživatel zadal data je platný, ale také poskytují informativnější činnost koncového uživatele pro identifikaci problémů vstupní data.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Krok 3: Pohodlné zpracování výjimek BLL úrovni

Při vkládání, aktualizaci nebo odstranění dat, Data Access Layer může vyvolat výjimku, při krátkodobém chyb souvisejících s daty. Databáze může být offline, nemusí mít měl sloupec tabulky požadovaná databáze má hodnotu nebo omezení úrovně tabulky bylo narušeno. Kromě výhradně souvisejících s daty výjimky vrstvu obchodní logiky, můžete použít výjimky k signalizují, že byla porušena obchodní pravidla. V [vytváření vrstvu obchodní logiky](../introduction/creating-a-business-logic-layer-cs.md) kurzu, například jsme přidali kontrolu obchodní pravidlo do původního `UpdateProduct` přetížení. Konkrétně Pokud uživatel byl označíte produktu, jako zastaveny, vyžádali jsme si produktu nesmí být jen jeden od jeho dodavatele. Pokud tuto podmínku byla porušena, `ApplicationException` byla vyvolána.

Pro `UpdateProduct` přetížení vytvořili v tomto kurzu, přidejme obchodního pravidla, která zakazuje `UnitPrice` pole z Probíhá nastavení objektu na novou hodnotu, která je větší, než dvojnásobek původní `UnitPrice` hodnota. K tomu, upravte `UpdateProduct` přetížení tak, aby tuto kontrolu provede a vrátí `ApplicationException` Pokud porušení pravidla. Aktualizovaná metoda následovně:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Díky této změně způsobí, že všechny aktualizace ceny, která je větší, než dvojnásobek existující cena `ApplicationException` vyvolání. Stejně jako výjimky vyvolané z vrstvy DAL tento BLL vyvolá `ApplicationException` mohou být zjištěna a zpracovávány v prvku GridView `RowUpdated` obslužné rutiny události. Ve skutečnosti `RowUpdated` kód obslužné rutiny události, protože zapisuje, správně rozpozná výjimku a zobrazit `ApplicationException`na `Message` hodnotu vlastnosti. Obrázek 11 ukazuje snímku obrazovky, když se uživatel pokusí aktualizovat cenu Chai $ 50,00, což je více než double jeho aktuální cena 19,95.


[![Obchodní pravidla zakáže zvyšuje ceny, které maximálně dvakrát cenu produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Obrázek 11**: firmy pravidla zvyšuje zakázat ceny, které maximálně dvakrát cenu produktu ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> V ideálním případě by se naše obchodní logiku pravidla mimo rozdělili `UpdateProduct` přetížení metody a do běžnou metodu. To je ponechán jako cvičení pro čtečku.


## <a name="summary"></a>Souhrn

Během vkládání, aktualizaci a odstraňování operace ovládací prvek webu dat a související se situací ObjectDataSource fire události před a po úrovně tohoto zarážku aktuální operace. Jak jsme viděli v tomto kurzu a ten předchozí při práci s upravitelné GridView prvku GridView `RowUpdating` aktivuje událost, za nímž následuje ObjectDataSource `Updating` událostí, okamžiku je příkaz aktualizace provedené ObjectDataSource základní objekt. Po dokončení operace ObjectDataSource `Updated` aktivuje událost, za nímž následuje prvku GridView `RowUpdated` událostí.

Můžeme vytvořit obslužné rutiny událostí předem úroveň události, aby bylo možné přizpůsobit vstupní parametry nebo po úroveň události, aby kontrolovat a reakce na výsledky operace. Obslužné rutiny událostí po úrovně se běžně používají ke zjištění, zda při operaci došlo k výjimce. Při krátkodobém výjimku v těchto obslužných rutinách událostí na úrovni po volitelně může zpracovat výjimku samostatně. V tomto kurzu jsme viděli, jak zpracovat takové výjimky zobrazením popisný chybová zpráva.

V dalším kurzu ukážeme, jak zmenšit prostor pravděpodobnost výjimky vzniklých formátování problémy dat (například zadáte jako záporné `UnitPrice`). Konkrétně podíváme Přidání ověření ovládacích prvků pro úpravy a vkládání rozhraní.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Liz Shulok. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [další](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
