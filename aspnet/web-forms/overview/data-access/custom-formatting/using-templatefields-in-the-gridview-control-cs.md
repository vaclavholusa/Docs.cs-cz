---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Pomocí TemplateFields v ovládacím prvku GridView (C#) | Microsoft Docs
author: rick-anderson
description: Zajistit flexibilitu nabízí GridView TemplateField, která vykreslí pomocí šablony. Šablony můžete zahrnout směs statické HTML, ovládací prvky webového, a...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 6485cbda50912920808fc0caf41c888493f210dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877873"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Pomocí TemplateFields v ovládacím prvku GridView (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) nebo [stáhnout PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Zajistit flexibilitu nabízí GridView TemplateField, která vykreslí pomocí šablony. Šablona může obsahovat kombinaci statických HTML webové ovládací prvky a syntaxe datové vazby. V tomto kurzu podíváme, jak používat TemplateField k dosažení vyšší stupeň přizpůsobení pomocí ovládacího prvku GridView.


## <a name="introduction"></a>Úvod

GridView se skládá ze sady polí, které indikují jaké vlastnosti z `DataSource` budou zahrnuty ve vykresleném výstupu spolu s jak se data zobrazí. Nejjednodušší typ pole není BoundField, která zobrazuje data hodnotu jako text. Ostatní typy polí zobrazit data pomocí alternativního elementů HTML. Vlastnost CheckBoxField, například vykreslí jako zaškrtávací políčko jejichž zaškrtnutého stavu závisí na hodnotě pole zadaných dat; ImageField vykreslí image, jejíž zdroj bitové kopie je podle pole Zadaná data. Hypertextové odkazy a tlačítka, jejichž stav závisí na základní hodnotu datového pole může být vykreslen pomocí HyperLinkField a ButtonField typy polí.

Při typy polí Vlastnost CheckBoxField, ImageField, HyperLinkField a ButtonField povolit pro alternativní zobrazení dat, ještě jsou velmi omezené s ohledem na formátování. Třída CheckBoxField může zobrazit pouze jeden zaškrtávací políčko, zatímco ImageField může zobrazit pouze jeden obrázek. Co když konkrétního pole musí zobrazit část textu, zaškrtávací políčko, *a* image všechny závislosti na rozsahu hodnot různých datových polí? Nebo co když jsme chtěli zobrazení dat pomocí ovládacího prvku webového než zaškrtávací políčko, Image, hypertextový odkaz nebo tlačítko? Kromě toho BoundField omezuje jeho zobrazení do jednoho datového pole. Co když jsme chtěli zobrazit dvě nebo více hodnot dat pole do jednoho sloupce GridView?

K přizpůsobení tato úroveň flexibilitu GridView poskytuje TemplateField, která vykreslí pomocí *šablony*. Šablona může obsahovat kombinaci statických HTML webové ovládací prvky a syntaxe datové vazby. Kromě toho TemplateField má řadu šablon, které lze použít k přizpůsobení vykreslování pro různé situace. Například `ItemTemplate` se ve výchozím nastavení použije k vykreslení buňky pro každý řádek, ale `EditItemTemplate` šablonu můžete použít k přizpůsobení rozhraní při úpravě data.

V tomto kurzu podíváme, jak používat TemplateField k dosažení vyšší stupeň přizpůsobení pomocí ovládacího prvku GridView. V [předchozí kurzu](custom-formatting-based-upon-data-cs.md) jsme viděli postup přizpůsobení formátování založené na základní dat pomocí `DataBound` a `RowDataBound` obslužné rutiny událostí. Jiný způsob, jak přizpůsobit formátování podle zadaných dat je formátování voláním metod z v rámci šablony. Podíváme tento postup v tomto kurzu také.

V tomto kurzu použijeme TemplateFields k přizpůsobení vzhledu seznam zaměstnanců. Konkrétně jsme budete seznam všech zaměstnanců, ale zobrazí zaměstnance první a poslední názvy v jeden sloupec, datum jejich přijetí v ovládacím prvku kalendář a sloupec Stav, který určuje, jak dlouho budou jste byl použit při společnosti.


[![Tři TemplateFields slouží k přizpůsobení zobrazení](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Obrázek 1**: tři TemplateFields slouží k přizpůsobení zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Krok 1: Vytvoření vazby dat GridView

U sestav, které budete muset použít TemplateFields přizpůsobit vzhled, najít je nejjednodušší začněte tím, že vytvoření ovládacího prvku GridView, která obsahuje jenom BoundFields nejdřív a pak přidejte nový TemplateFields nebo převést stávající BoundFields na TemplateFields podle potřeby. Přidáním GridView na stránku prostřednictvím návrháře a vytvoření vazby ObjectDataSource, který vrátí seznam zaměstnanci proto začněme tohoto kurzu. Tyto kroky vytvoří GridView s BoundFields pro každé pole zaměstnanců.

Otevřete `GridViewTemplateField.aspx` stránky a přetáhněte ji GridView z panelu nástrojů na návrháře. Z prvku GridView inteligentních značek vyberte přidání nové ovládacího prvku ObjectDataSource, která volá `EmployeesBLL` třídy `GetEmployees()` metoda.


[![Přidání nové ovládacího prvku ObjectDataSource, která volá metodu GetEmployees()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Obrázek 2**: Přidání nové ovládacího prvku ObjectDataSource této vyvolá `GetEmployees()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Vazba GridView tímto způsobem automaticky přidá BoundField pro každé z vlastností zaměstnanec: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, a `Country`. Tato sestava umožňuje není zabývat zobrazení `EmployeeID`, `ReportsTo`, nebo `Country` vlastnosti. Chcete-li odebrat tyto BoundFields můžete:

- Pomocí klikněte na pole dialogové okno pole na odkaz Upravit sloupce z prvku GridView inteligentních značek zprovoznit tohoto dialogového okna. V dalším kroku vyberte BoundFields z levého dolního rohu seznamu a klikněte na červené X tlačítko odebrání BoundField.
- Ručně upravte deklarativní syntaxe prvku GridView ze zobrazení zdroje, odstraňte `<asp:BoundField>` element pro BoundField, kterého chcete odebrat.

Po odebrání `EmployeeID`, `ReportsTo`, a `Country` BoundFields, vaše GridView značek by měl vypadat jako:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Chcete-li zobrazit naše průběh v prohlížeči chvíli trvat. V tomto okamžiku byste měli vidět tabulku s záznam pro každý zaměstnanec a čtyři sloupce: jeden pro zaměstnance příjmení, jeden pro jejich křestní jméno, jeden pro jejich název a jeden pro společnost pracovat.


[![Zobrazení LastName, FirstName, název a HireDate pole pro každý zaměstnanec](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Obrázek 3**: `LastName`, `FirstName`, `Title`, a `HireDate` pole jsou zobrazeny pro každý zaměstnanec ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Krok 2: Názvy první a poslední zobrazení do jednoho sloupce

V současné době je každý zaměstnanec první a poslední názvy jsou zobrazeny v samotném sloupci. Může to být dobrý se místo toho zkombinovat do jednoho sloupce. K tomu je potřeba použít TemplateField. Jsme můžete buď přidejte nové TemplateField, přidejte do ní potřebné značky a syntaxe datové vazby a pak odstraňte `FirstName` a `LastName` BoundFields nebo jsme můžete převést `FirstName` BoundField do TemplateField, upravit TemplateField zahrnout `LastName` hodnotu a pak odeberte `LastName` BoundField.

Oba přístupy net stejný výsledek, ale osobní líbí převodu BoundFields na TemplateFields, pokud je to možné, protože převod automaticky přidá `ItemTemplate` a `EditItemTemplate` s syntaxi vazby dat tak, aby napodoboval vzhled a webové ovládací prvky a funkcí BoundField. Výhodou je, že budeme muset udělat méně práce s TemplateField jako proces převodu budou mít provádět některé úkoly pro nás.

Převést stávající BoundField TemplateField, klikněte na odkaz Upravit sloupce z prvku GridView inteligentních značek, přináší dialogové okno pole. Vyberte BoundField převést ze seznamu v levém dolním a potom klikněte na odkaz "Převést toto pole do TemplateField" v pravém dolním rohu.


[![Převést BoundField TemplateField z dialogového okna pole](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Obrázek 4**: převod BoundField do TemplateField dialogové okno pole ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Pokračujte a proveďte převod `FirstName` BoundField do TemplateField. Po této změně není žádný perceptive rozdíl v návrháři. To je proto převodu BoundField TemplateField vytvoří TemplateField, která udržuje vzhledu a chování BoundField. Navzdory bylo žádné visual rozdíly v tomto okamžiku v návrháři, tento proces převodu nahradili deklarativní syntaxe BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` – s následující syntaxí TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Jak můžete vidět, TemplateField se skládá ze dvou šablon `ItemTemplate` , má popisek jejichž `Text` je nastavena na hodnotu `FirstName` pole dat a `EditItemTemplate` textové pole kontrolu jehož `Text` je nastavena také vlastnost na `FirstName` datové pole. Datové vazby syntaxe – `<%# Bind("fieldName") %>` -znamená, že datové pole *`fieldName`* je vázána na zadanou vlastnost webové ovládací prvek.

Chcete-li přidat `LastName` data pole hodnota, která má toto TemplateField budeme muset přidat další ovládací prvek popisek webu v `ItemTemplate` a vytvořte vazbu jeho `Text` vlastnost `LastName`. To můžete provést ručně nebo pomocí návrháře. Chcete-li provést ručně, jednoduše přidejte příslušné deklarativní syntaxe `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Přidejte ho pomocí návrháře, klikněte na odkaz Upravit šablony z prvku GridView inteligentních značek. Zobrazení a úpravy rozhraní prvku GridView šablony. V tomto rozhraní inteligentní značky je uveden seznam šablon v GridView. Vzhledem k tomu můžeme mít pouze jeden TemplateField v tomto okamžiku, v rozevíracím seznamu uvedeny pouze šablony jsou tyto šablony pro `FirstName` TemplateField spolu s `EmptyDataTemplate` a `PagerTemplate`. `EmptyDataTemplate` Šablony,-li zadána, se použije k vykreslení výstupu GridView Pokud v data vázaná na GridView; nebyly nalezeny žádné výsledky `PagerTemplate`, je-li zadána, je použita k vykreslení rozhraní stránkování pro GridView, který podporuje stránkování.


[![Prvku GridView šablony lze upravovat pomocí návrháře](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Obrázek 5**: The GridView šablony může být upravit prostřednictvím Návrhář ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Také zobrazit `LastName` v `FirstName` TemplateField přetáhněte ovládací prvek popisek z nástrojů do `FirstName` na TemplateField `ItemTemplate` v GridView je šablona úpravy rozhraní.


[![Přidání ovládacího prvku popisek na FirstName TemplateField ItemTemplate](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Obrázek 6**: Přidání ovládacího prvku popisek k `FirstName` na TemplateField ItemTemplate ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


V tomto okamžiku má ovládací prvek popisek webové přidán do TemplateField jeho `Text` vlastnost nastavena na hodnotu "Popisek". Je potřeba změnit tak, aby tato vlastnost je vázána na hodnotu `LastName` místo pole data. K provedení této klikněte na inteligentní značky ovládacího prvku popisek a vyberte možnost Upravit datové vazby.


[![Zvolte možnost Upravit datové vazby ze jmenovky inteligentní značky](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Obrázek 7**: Zvolte možnost Upravit datové vazby ze jmenovky inteligentní značky ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Tím se otevře dialogové okno datové vazby. Tady můžete vybrat vlastnost, která má účastnit datové vazby v seznamu na levé straně a vyberte pole, které chcete vytvořit vazbu na data z rozevíracího seznamu na pravé straně. Vyberte `Text` vlastnost z levé straně a `LastName` pole vpravo a klikněte na tlačítko OK.


[![Vytvořit vazbu vlastnosti textu k LastName datové pole](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Obrázek 8**: vytvoření vazby `Text` vlastnost, která má `LastName` datové pole ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> Dialogové okno datové vazby umožňuje určit, jestli se má provést obousměrný datové vazby. Pokud ji necháte za není zaškrtnuto, vazby dat syntaxe `<%# Eval("LastName")%>` se použije místo `<%# Bind("LastName")%>`. Buď přístup je vhodná pro tento kurz. Obousměrné vazby dat se změní na důležité při vkládání a úpravy dat. Pro jednoduše zobrazení dat, ale buď přístup bude fungovat stejně dobře. Obousměrné vazby dat podrobně probereme v budoucnu kurzy.


Chcete-li zobrazit tuto stránku prostřednictvím prohlížeče chvíli trvat. Jak vidíte, GridView stále zahrnuje čtyři sloupce; ale `FirstName` sloupec teď uvádí *i* `FirstName` a `LastName` dat, pole hodnoty.


[![Jak FirstName a LastName hodnoty jsou uvedeny v jednom sloupci](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Obrázek 9**: Jak `FirstName` a `LastName` hodnoty jsou zobrazeny v jeden sloupec ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Chcete-li dokončit tento první krok, odeberte `LastName` BoundField a přejmenujte `FirstName` na TemplateField `HeaderText` vlastnost "Název". Po provedení těchto změn rutina GridView deklarativní by měl vypadat následovně:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Každý zaměstnanec první a poslední názvy se zobrazují v jedním sloupcem](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Obrázek 10**: každý zaměstnanec první a poslední názvy se zobrazují v jedním sloupcem ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Krok 3: Použití ovládacího prvku kalendář pro zobrazení`HiredDate`pole

Zobrazení dat pole hodnotu jako text v GridView je jednoduché, použití BoundField. Pro určité scénáře ale data se nejlépe vyjádřit pomocí konkrétní ovládací prvek webu místo pouze text. Takové přizpůsobení zobrazení dat je možné s TemplateFields. Například spíš než jako text zobrazit datum přijetí zaměstnance, může ukážeme kalendáře (pomocí [ovládacího prvku Kalendář](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) s jejich datum přijetí zvýrazněná.

Chcete-li dosáhnout, spusťte převod `HiredDate` BoundField do TemplateField. Jednoduše přejděte do prvku GridView inteligentních značek a klikněte na odkaz Upravit sloupce, přináší dialogové okno pole. Vyberte `HiredDate` BoundField a klikněte na tlačítko "převést toto pole TemplateField."


[![Převést HiredDate BoundField TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Obrázek 11**: převést `HiredDate` BoundField do TemplateField ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Jak jsme viděli v kroku 2, touto akcí BoundField TemplateField, který obsahuje nahradíte `ItemTemplate` a `EditItemTemplate` s popisek a textové pole jehož `Text` vlastnosti je vázána na `HiredDate` hodnotu pomocí syntaxe datové vazby `<%# Bind("HiredDate")%>`.

Nahraďte text pomocí ovládacího prvku kalendář, úpravě šablony odebrání popisek a přidání ovládacího prvku kalendář. Z návrháře, vyberte Upravit šablony z prvku GridView inteligentních značek a zvolte `HireDate` na TemplateField `ItemTemplate` z rozevíracího seznamu. V dalším kroku odstranit ovládací prvek popisek a přetáhněte ovládacího prvku kalendář z panelu nástrojů do rozhraní úpravy šablony.


[![Přidání ovládacího prvku Kalendář HireDate ItemTemplate TemplateField společnosti](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Obrázek 12**: Přidání ovládacího prvku Kalendář `HireDate` na TemplateField `ItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


V tomto okamžiku každý řádek v GridView bude obsahovat ovládacího prvku kalendář v jeho `HiredDate` TemplateField. Ale je skutečný zaměstnanec `HiredDate` hodnota není nastavená, kdekoli v ovládacím prvku Kalendář způsobuje každý ovládací prvek kalendáře výchozí zobrazí aktuální měsíc a den. Chcete-li to opravit, je potřeba přiřadit každý zaměstnanec `HiredDate` do ovládacího prvku Kalendář [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) a [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) vlastnosti.

Z ovládacího prvku inteligentních značek zvolte Upravit datové vazby. V dalším kroku vazby obě `SelectedDate` a `VisibleDate` vlastnosti, které chcete `HiredDate` datové pole.


[![Vytvořit vazbu SelectedDate a vlastnosti VisibleDate HiredDate datové pole](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Obrázek 13**: vytvoření vazby `SelectedDate` a `VisibleDate` vlastnosti, které chcete `HiredDate` datové pole ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Zvolené datum ovládacího prvku nemusí být nutně viditelné. Například může mít kalendář srpen 1<sup>st</sup>, 1999 jako vybraným datem, ale se zobrazuje aktuální měsíc a rok. Vybrané datum a zobrazené datum je určené vlastností ovládacího prvku `SelectedDate` a `VisibleDate` vlastnosti. Vzhledem k tomu, že chceme obou vyberte zaměstnance `HiredDate` a ujistěte se, zda je zobrazen musíme obě tyto vlastnosti, které chcete vytvořit vazbu `HireDate` datové pole.


Při zobrazení stránky v prohlížeči, kalendáře nyní zobrazuje měsíc datum přijetí zaměstnance a vybere konkrétní datum.


[![Zaměstnance HiredDate se zobrazí v ovládacím prvku Kalendář](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Obrázek 14**: zaměstnance `HiredDate` se zobrazí v ovládacím prvku Kalendář ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Na rozdíl od všech příkladech jsme viděli doposud, v tomto kurzu jsme se *není* nastavit `EnableViewState` vlastnost `false` pro tato rutina GridView. Z důvodu pro toto rozhodnutí je vzhledem k tomu, že kliknete na kalendářní data ovládacího prvku Kalendář způsobí, že zpětné volání, nastavení zvolené datum v kalendáři k datu právě kliknutí na. Pokud je stav zobrazení GridView vypnutá, ale na každé zpětné volání prvku GridView dat je odrážejí na základní zdroj dat, což způsobí, že zvolené datum v kalendáři nastavit *zpět* na zaměstnance `HireDate`, přepisování Datum volená uživatelem.


V tomto kurzu jde moot diskusní vzhledem k tomu, že uživatel není možné aktualizovat zaměstnance `HireDate`. Pravděpodobně je nejvhodnější nakonfigurovat ovládacího prvku kalendář tak, aby jeho data se nedá vybrat. Bez ohledu na to tento kurz ukazuje, že v některých případech musí být stav zobrazení zapnutý s cílem poskytnout určité funkce.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Krok 4: Zobrazuje počet dní, zaměstnanec fungovala společnosti

Pokud jsme viděli dvě aplikace TemplateFields:

- Kombinování dvě nebo více hodnot dat pole do jednoho sloupce a
- Vyjádření hodnotu pole dat pomocí ovládacího prvku webového místo textu

V zobrazení metadata o prvku GridView je třetí použití TemplateFields základní data. Kromě zobrazení dat zaměstnanců, například může také Chceme mít sloupec, který zobrazuje, kolik celkový počet dnů, které jste se v úloze.

Ještě jiné použití TemplateFields vznikl ve scénářích, při podkladová data musí být jinak zobrazena v sestavě webové stránky než ve formátu je uložena v databázi. Představte si, která `Employees` z tabulek `Gender` pole, která uložená znak `M` nebo `F` označíte, pohlaví zaměstnance. Při zobrazení těchto informací na webové stránce chceme zobrazit pohlaví jako "Mužského" nebo "Ženského", oproti právě "M" nebo "F".

Oba scénáře lze zpracovat tak, že vytvoříte *formátování metoda* ve třídě kódu stránky ASP.NET (nebo v samostatné třídy knihovny, implementovaný jako `static` metoda) který lze vyvolat pomocí šablony. Formátování metoda je volána z šablony pomocí stejné datové vazby syntaxe viděli dříve. Metoda formátování využít libovolný počet parametrů, ale musí vrátit řetězec. Tato vrácený řetězec je HTML, který je vložit do šablony.

Pro ilustraci tento koncept, můžeme posílení našem kurzu zobrazíte na sloupec, který uvádí celkový počet dní, po které zaměstnanec byl v úloze. Tato metoda formátování bude trvat `Northwind.EmployeesRow` objektu a vrátí počet dní zaměstnanec být použitá jako řetězec. Tuto metodu lze přidat do třídy kódu stránky ASP.NET, ale *musí* označit jako `protected` nebo `public` Chcete-li být přístupné ze šablony.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Vzhledem k tomu `HiredDate` pole může obsahovat `NULL` databáze hodnoty jsme musíte nejdříve se ujistěte, že hodnota není `NULL` před pokračováním výpočtu. Pokud `HiredDate` hodnota je `NULL`, můžeme jednoduše vrátí řetězec "Neznámý"; Pokud není `NULL`, jsme výpočetní rozdíl mezi aktuální čas a `HiredDate` hodnotu a vrátí počet dní.

Abyste mohli využívat tuto metodu, musíme vyvolat z TemplateField v GridView pomocí syntaxe datové vazby. Začněte přidáním nové TemplateField GridView kliknutím na odkaz Upravit sloupce v prvku GridView inteligentních značek a přidáním nové TemplateField.


[![Přidat nové TemplateField do GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Obrázek 15**: Přidání nové TemplateField do GridView ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Nastavit tento nový TemplateField `HeaderText` vlastnost "Dní na the úloha" a jeho `ItemStyle`na `HorizontalAlign` vlastnost `Center`. K volání `DisplayDaysOnJob` metoda ze šablony, přidejte `ItemTemplate` a použijte následující syntaxi vazby dat:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Vrátí `DataRowView` objekt odpovídající `DataSource` záznam vázána `GridViewRow`. Jeho `Row` vlastnost vrací silného typu `Northwind.EmployeesRow`, který je předán `DisplayDaysOnJob` metoda. Tato syntaxe vazby dat se může zobrazit přímo v `ItemTemplate` (jak je znázorněno níže uvedené deklarativní syntaxe) nebo lze přiřadit k `Text` vlastností ovládacího prvku popisek Web.

> [!NOTE]
> Alternativně místo předávání `EmployeesRow` instance, jsme právě předat v `HireDate` hodnotu pomocí `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Ale `Eval` metoda vrátí `object`, takže nám změnit naše `DisplayDaysOnJob` podpis metody tak, aby přijímal vstupní parametr typu `object`, místo toho. Jsme slepě nejde přetypovat `Eval("HireDate")` volat na `DateTime` protože `HireDate` sloupec v `Employees` tabulka může obsahovat `NULL` hodnoty. Proto je potřeba přijmout `object` jako vstupní parametr pro `DisplayDaysOnJob` metoda, zkontrolujte, pokud ji měla databázi `NULL` hodnotu (jehož lze dosáhnout pomocí `Convert.IsDBNull(objectToCheck)`) a poté pokračujte odpovídajícím způsobem.


Z důvodu těchto odlišnosti I jste se rozhodli předávat celý `EmployeesRow` instance. V dalším kurzu ukážeme další příklad vhodnosti pro použití `Eval("columnName")` syntaxe pro předávání vstupního parametru do metody pro formátování.

Následující příklad zobrazuje deklarativní syntaxi pro naše GridView po přidání TemplateField a `DisplayDaysOnJob` metoda volána z `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

Obrázek 16 zobrazuje dokončené kurzu, když zobrazit pomocí prohlížeče.


[![Zobrazují počet dní, které zaměstnanec byl v úloze](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Obrázek 16**: počtem dnů se zobrazí zaměstnanec byl v úloze ([Kliknutím zobrazit obrázek v plné velikosti](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Souhrn

TemplateField v ovládacím prvku GridView umožňuje vyšší stupeň flexibilitu při zobrazení dat, než je k dispozici v dalších ovládacích prvků pole. TemplateFields jsou ideální pro situace, kde:

- Více datových polí je nutné na jeden sloupec GridView
- Data se nejlépe vyjádřit pomocí ovládacího prvku webového spíše než prostý text
- Výstup závisí na základní data, například zobrazení metadat nebo v přeformátování data

Kromě přizpůsobení zobrazení dat, jsou TemplateFields také použít pro přizpůsobení uživatelského rozhraní, použít pro úpravy a vkládání dat, jako ukážeme v budoucnosti kurzy.

Další dvě kurzy dál zkoumat šablon, podívejte se na použití TemplateFields v DetailsView počínaje. Následující, jsme budete zapněte k FormView, která poskytují větší flexibilitu v rozložení a strukturu dat pomocí šablony místo pole.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Dana Jagers. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](custom-formatting-based-upon-data-cs.md)
> [další](using-templatefields-in-the-detailsview-control-cs.md)
