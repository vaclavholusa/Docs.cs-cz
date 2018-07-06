---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Použití vlastností TemplateField v ovládacím prvku GridView (VB) | Dokumentace Microsoftu
author: rick-anderson
description: K zajištění flexibility, nabízí prvku GridView TemplateField, která vykresluje pomocí šablony. Šablona může obsahovat kombinaci statický kód HTML, webové ovládací prvky, a...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 411a3a2e4067d0e9b41143d85ddfb9b1031fd684
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829162"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Použití vlastností TemplateField v ovládacím prvku GridView (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) nebo [stahovat PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> K zajištění flexibility, nabízí prvku GridView TemplateField, která vykresluje pomocí šablony. Šablona může obsahovat kombinaci statických jazyka HTML, webové ovládací prvky a syntaxe datové vazby. V tomto kurzu prozkoumáme způsob použití pole TemplateField dosáhnout větší míru přizpůsobení pomocí ovládacího prvku GridView.


## <a name="introduction"></a>Úvod

GridView tvoří sadu polí, které označují vlastností z `DataSource` mají být zahrnuty do vykresleného výstupu spolu s jak se data zobrazí. Nejjednodušší typ pole je vlastnost BoundField, která zobrazuje datovou hodnotu jako text. Jiné typy polí zobrazit data pomocí alternativního elementů HTML. Třída CheckBoxField, například vykreslí jako zaškrtávací políčko jehož zaškrtnutý stav závisí na hodnotě zadané datové pole. třídy ImageField vykreslí obrázek jejíž zdroj obrázku je založena na zadané datové pole. Hypertextové odkazy a tlačítka, jehož stav závisí na základní hodnotou pole dat může být vykreslen pomocí HyperLinkField a ButtonField typy polí.

Typy polí třídě CheckBoxField, ImageField, HyperLinkField a ButtonField povolit alternativní zobrazení dat, ale přesto jsou velmi omezené s ohledem na formátování. Třídě CheckBoxField lze zobrazit pouze jeden checkbox, že ImageField lze zobrazit pouze jedné image. Co když je potřeba určité pole zobrazí text, zaškrtávací políčko, *a* bitové kopie, které jsou všechny založené na různých datových polí? Nebo co když jsme chtěli zobrazit data s využitím webový ovládací prvek, než je zaškrtávací políčko, Image, hypertextový odkaz nebo tlačítko? Kromě toho Vlastnost BoundField omezuje jeho zobrazení do jednoho datového pole. Co když jsme chtěli zobrazit dvě nebo víc hodnot dat pole do jednoho sloupce GridView?

K přizpůsobení této úrovni flexibilitu prvku GridView poskytuje TemplateField, která vykresluje pomocí *šablony*. Šablona může obsahovat kombinaci statických jazyka HTML, webové ovládací prvky a syntaxe datové vazby. Kromě toho TemplateField má celou řadu šablon, které lze použít k přizpůsobení vykreslování pro různé situace. Například `ItemTemplate` se ve výchozím nastavení použije k vykreslení buňky pro každý řádek, ale `EditItemTemplate` šablony umožňuje přizpůsobit rozhraní při úpravách data.

V tomto kurzu prozkoumáme způsob použití pole TemplateField dosáhnout větší míru přizpůsobení pomocí ovládacího prvku GridView. V [předchozím kurzu](custom-formatting-based-upon-data-vb.md) jsme viděli, jak přizpůsobit formátování založené na podkladová data s využitím `DataBound` a `RowDataBound` obslužných rutin událostí. Jinou možností přizpůsobení formátování na základě podkladových dat je voláním metody pro formátování z v rámci šablony. Podíváme se na tento postup v tomto kurzu také.

Pro účely tohoto kurzu používáme vlastností TemplateField pro přizpůsobení vzhledu seznam zaměstnanců. Konkrétně jsme zobrazí seznam všech zaměstnanců, ale zobrazí zaměstnance jména a příjmení v jeden sloupec, jejich Datum zařazení do ovládacího prvku kalendář a sloupec Stav, který určuje, kolik dní se jsme se použijí ve firmě.


[![Tři vlastností TemplateField se používají pro přizpůsobení zobrazení](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Obrázek 1**: tři vlastností TemplateField se používají pro přizpůsobení zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Krok 1: Vytvoření vazby dat na prvku GridView.

U sestav, které je potřeba použití vlastností TemplateField k přizpůsobení vzhledu, můžu vás bude nejjednodušší začít tím, že vytvoření ovládacího prvku GridView, který obsahuje pouze BoundFields nejprve a potom k přidání nových vlastností TemplateField, nebo konvertovat existující BoundFields do Vlastností TemplateField podle potřeby. Přidáním na stránku prostřednictvím návrháře GridView a vytvoříte jejich vazbu na ObjectDataSource, který vrátí seznam zaměstnanců, proto začneme v tomto kurzu. Tyto kroky vytvoří GridView s BoundFields pro každé pole zaměstnance.

Otevřít `GridViewTemplateField.aspx` stránku a přetáhněte z panelu nástrojů na Návrhář GridView. V prvku GridView inteligentních značek zvolte Přidat nový ovládací prvek ObjectDataSource, která volá `EmployeesBLL` třídy `GetEmployees()` metody.


[![Přidat nový ovládací prvek ObjectDataSource, která volá metodu GetEmployees()](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Obrázek 2**: Přidání nového ovládacího prvku ObjectDataSource tohoto volání `GetEmployees()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Vazby prvku GridView. tímto způsobem bude automaticky přidat vlastnost BoundField pro každou z vlastností zaměstnance: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, a `Country`. Pro tuto sestavu teď není zabývat zobrazení `EmployeeID`, `ReportsTo`, nebo `Country` vlastnosti. Chcete-li odebrat tyto BoundFields můžete:

- Pomocí polích dialogového okna klepněte na odkaz Upravit sloupce v prvku GridView inteligentních značek zobrazí toto dialogové okno se. V dalším kroku vyberte BoundFields z levé dolní části seznamu a klikněte na červené X tlačítko Odebrat vlastnost BoundField.
- Deklarativní syntaxe prvku GridView upravit ručně ze zobrazení zdroje, odstranit `<asp:BoundField>` – element pro vlastnost BoundField, kterou chcete odebrat.

Po odebrání `EmployeeID`, `ReportsTo`, a `Country` BoundFields, prvku GridView značek by měl vypadat takto:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Chcete-li zobrazit náš postup v prohlížeči chvíli trvat. V tomto okamžiku byste měli vidět tabulku s záznam pro každý zaměstnanec a čtyři sloupce: jeden pro zaměstnance příjmení, jeden pro své křestní jméno, jeden pro jejich funkce a jeden pro datum jejich přijetí.


[![Pro každý zaměstnanec zobrazené LastName, jméno, název a HireDate pole](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Obrázek 3**: `LastName`, `FirstName`, `Title`, a `HireDate` jsou zobrazena pole pro každý zaměstnanec ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Krok 2: Zobrazení v jednom sloupci jména a příjmení

V současné době jednotliví zaměstnanci vaší první a poslední názvy se zobrazí v samostatném sloupci. Může být dobré si je zkombinovat do jednoho sloupce. K tomu potřeba použít na pole TemplateField. Jsme můžete buď přidejte nové TemplateField, přidejte do ní potřebné značky a syntaxe vázání dat a odstraňte `FirstName` a `LastName` BoundFields nebo nám můžete převést `FirstName` Vlastnost BoundField na pole TemplateField, upravte TemplateField zahrnout `LastName` hodnotu a pak odeberte `LastName` Vlastnost BoundField.

Oba přístupy net stejný výsledek, ale osobně chci převod BoundFields do vlastností TemplateField, pokud je to možné, protože převod automaticky přidá `ItemTemplate` a `EditItemTemplate` webové ovládací prvky a syntaxe datové vazby k napodobení vzhledu a funkce Vlastnost BoundField. Výhodou je, že nám budete muset udělat méně práce s pole TemplateField podle procesu převodu se dá provést určitou část práce pro nás.

Chcete-li převést existující vlastnost BoundField TemplateField, klikněte na odkaz Upravit sloupce z inteligentních značek prvku GridView, spustit až dialogové okno pole. Vyberte vlastnost BoundField převést ze seznamu v levém dolním rohu a pak klikněte na odkaz "Převést toto pole na pole TemplateField" v pravém dolním rohu.


[![Vlastnost BoundField převést z dialogového okna pole TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Obrázek 4**: Vlastnost BoundField do TemplateField převést pole dialogových oken ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Pokračujte a převést `FirstName` Vlastnost BoundField na pole TemplateField. Po této změně není žádný perceptive rozdíl v návrháři. Je to proto, že vlastnost BoundField převod na pole TemplateField vytvoří TemplateField udržuje vzhledu a chování Vlastnost BoundField. I když bylo žádný vizuální rozdíl v tuto chvíli v návrháři, tento proces převodu nahradil deklarativní syntaxe Vlastnost BoundField – `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` – s následující syntaxí TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Jak je vidět pole TemplateField se skládá ze dvou šablon `ItemTemplate` , který má popisek jehož `Text` je nastavena na hodnotu `FirstName` datové pole a `EditItemTemplate` u objektu TextBox ovládací prvek, jehož `Text` nastavena také vlastnost Chcete `FirstName` datové pole. Syntaxe databinding – `<%# Bind("fieldName") %>` – znamená, že datové pole *`fieldName`* je vázán na zadané vlastnosti webového ovládacího prvku.

Chcete-li přidat `LastName` hodnota tohoto pole TemplateField potřebujeme přidat jiný popisek webový ovládací prvek pole data `ItemTemplate` a vytvořit vazbu jeho `Text` vlastnost `LastName`. To můžete provést ručně nebo prostřednictvím návrháře. Provést ručně, stačí přidat vhodné deklarativní syntaxe na `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Přidejte prostřednictvím návrháře, klikněte na odkaz Upravit šablony z inteligentních značek v prvku GridView. Zobrazí se rozhraní úprav šablony prvku GridView. V tomto rozhraní inteligentních značek je seznam šablon v prvku GridView. Protože máme pouze jeden TemplateField v tomto okamžiku, pouze šablony, které jsou uvedeny v rozevíracím seznamu jsou pro tyto šablony `FirstName` TemplateField spolu s `EmptyDataTemplate` a `PagerTemplate`. `EmptyDataTemplate` Šablony,-li zadán, slouží k vykreslení výstupu prvku GridView, pokud neexistují žádné výsledky v data vázaná na GridView; `PagerTemplate`, je-li zadán, slouží k vykreslení rozhraní stránkování prvku GridView, který podporuje stránkování.


[![Prvku GridView šablony se dá upravit v Návrháři](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Obrázek 5**: prvku GridView šablony může být upravovat prostřednictvím návrháře ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Také zobrazíte `LastName` v `FirstName` TemplateField přetáhnout z panelu nástrojů do ovládacího prvku popisku `FirstName` společnosti TemplateField `ItemTemplate` v prvku GridView uživatele úprav šablony rozhraní.


[![Přidání ovládacího prvku popisku ItemTemplate FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Obrázek 6**: Přidání ovládacího prvku popisku k `FirstName` ItemTemplate TemplateField společnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


V tomto okamžiku je ovládací prvek popisek Web přidán pole TemplateField jeho `Text` vlastnost nastavena na "Štítku". Budeme muset změnit tak, aby tato vlastnost je vázána na hodnotu `LastName` datové pole místo. Chcete-li provést tento klikněte na inteligentní značky ovládacího prvku popisku a zvolte možnost Upravit datové vazby.


[![Zvolte možnost Upravit datové vazby z popisku inteligentních značek](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Obrázek 7**: Zvolte možnost upravit vlastnosti DataBindings z inteligentních značek popisku ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Tím se otevře dialogové okno datové vazby. Odtud můžete vybrat vlastnost, která má účastnit datové vazby v seznamu na levé straně a zvolte pole, které chcete vytvořit vazbu data z rozevíracího seznamu na pravé straně. Zvolte `Text` vlastnost z levé strany a `LastName` polí z pravé strany a klikněte na tlačítko OK.


[![Vytvořit vazbu vlastnosti Text do pole LastName dat](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Obrázek 8**: vytvoření vazby `Text` vlastnost `LastName` datové pole ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> Dialogové okno vlastnosti DataBindings umožňuje určit, jestli se má provést dvousměrnou datovou vazbou. Pokud ji necháte nezaškrtnuté, syntaxe databinding `<%# Eval("LastName")%>` se použije namísto `<%# Bind("LastName")%>`. Kterýkoliv přístup je v pořádku pro účely tohoto kurzu. Obousměrná vazba dat je důležitá při vkládání a úpravy dat. Jednoduše zobrazení dat, ale kterýkoliv přístup bude fungovat stejně dobře. Obousměrná vazba dat podrobně probereme v budoucích kurzech.


Za chvíli zobrazení této stránky prostřednictvím prohlížeče. Jak je vidět, prvku GridView stále obsahuje čtyři sloupce; ale `FirstName` nyní obsahuje sloupec *obě* `FirstName` a `LastName` datové pole hodnot.


[![Jak FirstName a LastName hodnoty jsou uvedeny v jednom sloupci](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Obrázek 9**: Jak `FirstName` a `LastName` hodnoty jsou zobrazeny v jednom sloupci ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Chcete-li dokončit tento první krok, odeberte `LastName` Vlastnost BoundField a přejmenovat `FirstName` společnosti TemplateField `HeaderText` vlastnost "Name". Po provedení těchto změn prvku GridView deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Každý zaměstnanec jména a příjmení se zobrazují v jednom sloupci](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Obrázek 10**: v jednom sloupci se zobrazí každý zaměstnanec jména a příjmení ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Krok 3: Použití ovládacího prvku kalendáře do zobrazení`HiredDate`pole

Zobrazení hodnoty datového pole jako text v GridView je snadné – stačí použít vlastnost BoundField. Pro určité scénáře ale data se nejlépe vyjádřena pomocí konkrétní ovládací prvek webové místo pouze text. Tato přizpůsobení zobrazení dat je možné s vlastností TemplateField. Například, spíše než zobrazit datum zaměstnance přijetím jako text, jsme byli schopni ukázat kalendáře (pomocí [ovládacího prvku Kalendář](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) s jejich datum přijetí zvýrazní.

Chcete-li to provést, začněte tím, že převod `HiredDate` Vlastnost BoundField na pole TemplateField. Stačí přejít na inteligentní značky prvku GridView a klikněte na odkaz Upravit sloupce spustit až dialogové okno pole. Vyberte `HiredDate` Vlastnost BoundField a klikněte na tlačítko "převést toto pole TemplateField."


[![Vlastnost HiredDate BoundField převést na pole TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Obrázek 11**: převod `HiredDate` Vlastnost BoundField do TemplateField ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Jak jsme viděli v kroku 2, tato operace nahradí Vlastnost BoundField s TemplateField obsahuje `ItemTemplate` a `EditItemTemplate` s popisku a textového pole jejichž `Text` vlastnosti, které jsou vázány na `HiredDate` hodnotu pomocí syntaxe databinding `<%# Bind("HiredDate")%>`.

Nahradit text s ovládacím prvkem kalendáře, upravte šablonu odeberete příslušný popisek a přidáním ovládacího prvku kalendář. Z návrháře, vyberte Upravit šablony z inteligentních značek v prvku GridView a zvolte `HireDate` společnosti TemplateField `ItemTemplate` z rozevíracího seznamu. V dalším kroku odstranit ovládací prvek popisku a přetáhněte ovládací prvek Calendar z panelu nástrojů do rozhraní pro úpravy šablony.


[![Přidání ovládacího prvku kalendáře HireDate ItemTemplate TemplateField.](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Obrázek 12**: Přidání ovládacího prvku kalendáře `HireDate` společnosti TemplateField `ItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


V tomto okamžiku každého řádku v prvku GridView, bude obsahovat ovládacího prvku kalendáře v jeho `HiredDate` TemplateField. Ale zaměstnance skutečné `HiredDate` hodnota není nastavená, kdekoli v ovládacím prvku kalendář, způsobí každého ovládacího prvku kalendáře na výchozím nastavení zobrazuje aktuální měsíc a den. Chcete-li to napravit, musíme přiřadit každý zaměstnanec `HiredDate` do ovládacího prvku Kalendář [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) a [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) vlastnosti.

Z ovládacího prvku Kalendář inteligentních značek zvolte možnost Upravit datové vazby. V dalším kroku navázat obě `SelectedDate` a `VisibleDate` vlastnosti, které chcete `HiredDate` datové pole.


[![Vytvořit vazbu vlastnosti SelectedDate a VisibleDate HiredDate datového pole](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Obrázek 13**: vytvoření vazby `SelectedDate` a `VisibleDate` vlastnosti, které chcete `HiredDate` datové pole ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Vybrané datum kalendáře ovládacího prvku nemusí být nutně viditelné. Kalendáře může mít například od 1. srpna<sup>st</sup>, 1999 jako vybraným datem, ale měly zobrazovat v aktuálním měsíci a roce. Vybrané datum a datum viditelné jsou určena pomocí ovládacího prvku Kalendář `SelectedDate` a `VisibleDate` vlastnosti. Protože chceme, aby obě vybrat zaměstnance `HiredDate` a ujistěte se, že se zobrazí, potřebujeme pro obě tyto vlastnosti k vytvoření vazby `HireDate` datové pole.


Při zobrazení stránky v prohlížeči, kalendáře nyní zobrazuje měsíc datum přijetí zaměstnance a vybere konkrétní data.


[![Zaměstnance HiredDate se zobrazí v ovládacím prvku Kalendář](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Obrázek 14**: zaměstnance `HiredDate` se zobrazí v ovládacím prvku Kalendář ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Rozporu s všechny příklady jsme viděli doposud, pro účely tohoto kurzu jsme to udělali *není* nastavit `EnableViewState` vlastnost `False` tohoto prvku GridView. Důvod pro toto rozhodnutí je vzhledem k tomu, že kliknete na data ovládacím prvku Kalendář vyvolá zpětné volání, nastavení vybrané datum v kalendáři na kliknutí na datum. Pokud je stav zobrazení prvku GridView zakázán, ale při každém postbacku dat prvku GridView je odrážejí podkladový zdroj dat, což způsobí, že vybrané datum v kalendáři nastavit *zpět* na zaměstnance `HireDate`, přepisování data vybraného uživatele.


Pro účely tohoto kurzu jde diskusi moot vzhledem k tomu, že uživatel není možné aktualizovat zaměstnance `HireDate`. Bylo by pravděpodobně nejlepší konfigurovat ovládací prvek Calendar tak, aby jeho data se nedá vybrat. Bez ohledu na to tento kurz ukazuje, že v některých případech povolen stav zobrazení musí být cílem poskytnout určité funkce.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Krok 4: Zobrazující počet dní, zaměstnanec pracoval pro společnost

Zatím jsme viděli dvě aplikace vlastností TemplateField:

- Kombinace dvou nebo víc hodnot dat pole do jednoho sloupce a
- Vyjádření hodnoty datového pole pomocí ovládacího prvku webového namísto textu

Třetí použití vlastností TemplateField je v zobrazení metadat o prvku GridView podkladová data. Kromě zobrazení dat zaměstnanců, například může být také Chceme mít sloupec, který zobrazuje počet celkový počet dnů, jsme se vydali na úlohu.

Ještě nastane jiný použití vlastností TemplateField v situacích při podkladová data musí být jinak zobrazí v sestavě webové stránky než ve formátu je uložena v databázi. Představte si, že `Employees` měla tabulka `Gender` pole, které ukládají znak `M` nebo `F` udávajících pohlaví zaměstnance. Při zobrazení těchto informací na webové stránce budeme chtít zobrazit pohlaví jako "Mužského" nebo "Ženský", na rozdíl od jenom "M" nebo "F".

Obou těchto scénářích může zpracovat tak, že vytvoříte *formátování – metoda* v třídě modelu code-behind stránky ASP.NET (nebo v samostatné knihovně tříd, implementovat jako `Shared` – metoda), která je vyvolána z šablony. Tyto metody pro formátování je vyvolána z šablony pomocí stejné syntaxe databinding viděli dříve. Metoda formátování můžete provést v libovolném počtu parametrů, ale musí vracet řetězce. Vrácený řetězec je HTML, který se vloží do šablony.

Pro ilustraci tohoto konceptu, můžeme rozšířit v našem kurzu zobrazíte na sloupec, který uvádí celkový počet dní, po které se zaměstnanci v úloze. Bude mít tato metoda formátování `Northwind.EmployeesRow` objekt a vrátí počet dní, zaměstnanec má se použijí jako řetězec. Tuto metodu lze přidat na stránku ASP.NET použití modelu code-behind třídu, ale *musí* označit jako `Protected` nebo `Public` -li být přístupné ze šablony.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Protože `HiredDate` pole může obsahovat `NULL` databáze hodnoty jsme musíte napřed zajistit, že hodnota není `NULL` než se pustíte do výpočtu. Pokud `HiredDate` hodnotu `NULL`, můžeme jednoduše vrátit řetězec "Neznámý"; Pokud není `NULL`, můžeme následujícím způsobem vypočítat rozdíl mezi aktuální čas a `HiredDate` hodnotu a vrátí počet dní.

Chcete-li využívají tuto metodu, musíme ho vyvolat z TemplateField v prvku GridView pomocí syntaxe datové vazby. Začněte přidáním nové TemplateField do prvku GridView. Kliknutím na odkaz Upravit sloupce v prvku GridView inteligentních značek a přidáním nové TemplateField.


[![Přidat nový TemplateField do prvku GridView.](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Obrázek 15**: přidejte nový TemplateField do prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Nastavit tento nový TemplateField `HeaderText` vlastnost "Dní na the úlohy" a jeho `ItemStyle`společnosti `HorizontalAlign` vlastnost `Center`. Volání `DisplayDaysOnJob` metoda ze šablony, přidejte `ItemTemplate` a použijte následující syntaxi datové vazby:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` Vrátí `DataRowView` objekt odpovídající `DataSource` záznam vázán na `GridViewRow`. Jeho `Row` vlastnost vrací silných `Northwind.EmployeesRow`, které se předává `DisplayDaysOnJob` metoda. Tato syntaxe vázání dat se může zobrazit přímo v `ItemTemplate` (jak je znázorněno níže uvedené deklarativní syntaxe) nebo je možné přiřadit `Text` vlastnost ovládacího prvku popisku Web.

> [!NOTE]
> Alternativně namísto předávání `EmployeesRow` instance, jsme mohli stačí pouze předat `HireDate` hodnotu pomocí `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Ale `Eval` vrátí metoda `Object`, takže budeme něco muset změnit náš `DisplayDaysOnJob` podpis metody tak, aby přijímal vstupní parametr typu `Object`, místo toho. Jsme slepě nejde přetypovat `Eval("HireDate")` volání `DateTime` protože `HireDate` sloupec v `Employees` tabulka může obsahovat `NULL` hodnoty. Proto budeme muset přijmout `Object` jako vstupní parametr `DisplayDaysOnJob` metody, zkontrolujte, pokud má databáze `NULL` hodnotu (jehož lze dosáhnout pomocí `Convert.IsDBNull(objectToCheck)`) a pak pokračujte odpovídajícím způsobem.


Z důvodu těchto odlišností můžu nepřejete a zajistěte tak předání celý `EmployeesRow` instance. V dalším kurzu uvidíme více přizpůsobování příkladu pro použití `Eval("columnName")` syntaxe pro předávání vstupního parametru do metody pro formátování.

Následující příklad zobrazuje deklarativní syntaxe pro naše GridView po přidání pole TemplateField a `DisplayDaysOnJob` metodu s názvem z `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Obrázek 16 ukazuje dokončení kurzu při prohlížení prostřednictvím prohlížeče.


[![Zobrazí počet dní, zaměstnanec bylo v úloze](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Obrázek 16**: The číslo ze dnů zaměstnance bylo v úloze se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Souhrn

Umožňuje vyšší míra flexibility v zobrazení dat, než je k dispozici s dalšími prvky pole TemplateField v ovládacím prvku GridView. Vlastností TemplateField jsou ideální pro situace, kde:

- Potřebujete více datových polí zobrazeného do jednoho sloupce GridView
- Data je nejlepší vyjádřena pomocí webový ovládací prvek spíše než prostý text
- Výstup závisí na podkladová data, například zobrazení metadat nebo přeformátování dat

Kromě přizpůsobení zobrazení dat, jsou vlastností TemplateField také používá pro přizpůsobení uživatelských rozhraní pro úpravy a vložení dat, používat, protože uvidíme v budoucích kurzech.

Následující dva kurzy pokračujte ve zkoumání šablon, podívejte se na použití vlastností TemplateField v DetailsView počínaje. Pod jsme budete přepněte FormView, který používá šablony k zajištění větší flexibility v rozložení a strukturu dat namísto polí.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Jagers daň. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](custom-formatting-based-upon-data-vb.md)
> [další](using-templatefields-in-the-detailsview-control-vb.md)
