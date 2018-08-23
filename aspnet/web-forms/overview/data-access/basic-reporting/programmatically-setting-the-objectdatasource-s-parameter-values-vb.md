---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Programové nastavení hodnot parametru ObjectDataSource (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu podíváme na přidání metody do našich DAL a BLL, která přijímá jeden vstupní parametr a vrací data. V příkladu nastaví tento parametr...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f823d1db7f98dcbbef12d20df4a28e39fae0ac26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752007"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Programové nastavení hodnot parametru ObjectDataSource (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) nebo [stahovat PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> V tomto kurzu podíváme na přidání metody do našich DAL a BLL, která přijímá jeden vstupní parametr a vrací data. V příkladu se tento parametr nastavit prostřednictvím kódu programu.


## <a name="introduction"></a>Úvod

Jak jsme viděli v [předchozí kurz o službě](declarative-parameters-vb.md), řadu možností, které jsou k dispozici pro deklarativně předání hodnot parametru ObjectDataSource metody. Pokud je hodnota parametru pevně zakódované, pochází z webový ovládací prvek na stránce nebo v kterýkoli jiný zdroj, který je čitelný ve zdroji dat `Parameter` objektu, například, že hodnota může být vázaný na vstupní parametr bez nutnosti psaní jediného řádku kódu.

Může nastat situace, ale když hodnota parametru pocházejí z některé zdroje není již bylo začleněno pomocí jedné z předdefinované datové zdroje `Parameter` objekty. Pokud uživatelské účty podporována náš web budeme chtít nastavit parametr podle aktuálně přihlášeného uživatelského ID. návštěvníka Nebo nám může být třeba přizpůsobit hodnotu parametru před odesláním podél metodě ObjectDataSource základní objekt.

Pokaždé, když prvku ObjectDataSource `Select` je vyvolána metoda ObjectDataSource nejprve vyvolá jeho [události Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Potom je volána metoda ObjectDataSource základní objekt. Jakmile, která se dokončí prvku ObjectDataSource [vybrané události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) aktivuje (toto pořadí událostí, které znázorňuje obrázek 1). Parametr hodnoty předané do metody ObjectDataSource základní objekt můžete nastavit nebo upravit v obslužné rutině události pro `Selecting` událostí.


[![Je vyvolána ObjectDataSource vybrané a výběr Fire události před a po jeho základního objektu – metoda](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Obrázek 1**: prvku ObjectDataSource `Selected` a `Selecting` je vyvolána metoda Fire události před a po jeho základního objektu ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


V tomto kurzu se podíváme přidání metody do našich DAL a BLL, která přijímá jeden vstupní parametr `Month`, typu `Integer` a vrátí `EmployeesDataTable` vyplní zaměstnanců, které mají jejich náborovou výročí v zadaném objektu `Month`. Náš příklad nastaví tento parametr prostřednictvím kódu programu na základě aktuálního měsíce zobrazujícím seznam "Výročí tohoto měsíce."

Pusťme se do práce!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Krok 1: Přidání metody do`EmployeesTableAdapter`

V našem prvním příkladu potřebujeme přidat prostředky k načtení těchto zaměstnanci jehož `HireDate` došlo k chybě v zadaném měsíci. Tuto funkčnost v souladu s naší architektury, je nutné nejprve vytvořit metodu v `EmployeesTableAdapter` , která se mapuje na správný příkaz SQL. Chcete-li to provést, začněte otevřením typová Northwind. Klikněte pravým tlačítkem na `EmployeesTableAdapter` označovat popisky a zvolte Přidat dotaz.


[![Přidat nový dotaz EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Obrázek 2**: Přidat nový dotaz, který `EmployeesTableAdapter` ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Zvolte Přidat příkaz SQL, který vrátí řádky. Když se dostanete určení `SELECT` výchozí obrazovky – příkaz `SELECT` příkaz pro `EmployeesTableAdapter` už se načtou. Jednoduše přidejte v `WHERE` klauzule: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) je funkce jazyka T-SQL, která vrací část konkrétní datum `datetime` typ; v tomto případě používáme `DATEPART` vrátit z měsíce `HireDate` sloupce.


[![Vrácení pouze těch řádky kde HireDate sloupec je menší než nebo rovno @HiredBeforeDate parametr](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Obrázek 3**: vrátí pouze ty řádky kde `HireDate` sloupec je menší než nebo rovna hodnotě `@HiredBeforeDate` parametr ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Nakonec změňte `FillBy` a `GetDataBy` metoda názvů pro `FillByHiredDateMonth` a `GetEmployeesByHiredDateMonth`v uvedeném pořadí.


[![Vyberte vhodnější názvy metod než FillBy a GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Obrázek 4**: Zvolte více odpovídající metoda názvy než `FillBy` a `GetDataBy` ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Kliknutím na tlačítko Dokončit dokončete průvodce a vraťte se do datové sady návrhovou plochu. `EmployeesTableAdapter` By teď měl obsahovat novou sadu metod pro přístup k zaměstnance zařazené v zadaném měsíci.


[![Nové metody se zobrazí v návrhové ploše datové sady](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Obrázek 5**: The nové metody se zobrazí v návrhové ploše datové sady ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Krok 2: Přidání`GetEmployeesByHiredDateMonth(month)`metodu pro vrstvy obchodní logiky

Protože naše aplikace architektura používá samostatné vrstvy pro obchodní logiku a data získat přístup k logiku, musíme přidat metody do našich knihoven BLL, který si najali, volání do vrstvy DAL k načtení zaměstnanci před určitým datem. Otevřít `EmployeesBLL.vb` a přidejte následující metodu:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Stejně jako u našich jiné metody v této třídě `GetEmployeesByHiredDateMonth(month)` jednoduše volá do vrstvy DAL a vrátí výsledky.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Krok 3: Zobrazení zaměstnanců, jehož náborovou výročí je tento měsíc

Posledním krokem v tomto příkladu je zobrazení těchto zaměstnanců, jehož náborovou výročí je tento měsíc. Začněte přidáním GridView k `ProgrammaticParams.aspx` stránku `BasicReporting` složky a přidejte nový prvek ObjectDataSource jako svůj zdroj dat. Konfigurace ObjectDataSource používat `EmployeesBLL` třídy s `SelectMethod` nastavena na `GetEmployeesByHiredDateMonth(month)`.


[![Použití třídy EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Obrázek 6**: použijte `EmployeesBLL` třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Vyberte z the GetEmployeesByHiredDateMonth(month) – metoda](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Obrázek 7**: vyberte z `GetEmployeesByHiredDateMonth(month)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Poslední obrazovka zobrazí dotaz, abychom mohli poskytovat `month` zdroje hodnotu parametru. Vzhledem k tomu, že tato hodnota nastavíme prostřednictvím kódu programu, ponechejte zdroji parametru nastavenou na výchozí hodnotu None možnosti a klikněte na tlačítko Dokončit.


[![Ponechte zdrojová sada parametr na hodnotu None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Obrázek 8**: ponechte zdrojový parametr nastaven na hodnotu None ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Tím se vytvoří `Parameter` objektu v prvku ObjectDataSource `SelectParameters` kolekce, ve kterém není zadána žádná hodnota.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Nastavení této hodnoty prostřednictvím kódu programu, potřebujeme vytvořit obslužnou rutinu události pro ObjectDataSource `Selecting` událostí. Chcete-li to provést, přejděte do zobrazení návrhu a dvakrát klikněte na panel prvku ObjectDataSource. Alternativně vyberte ObjectDataSource, přejděte do okna Vlastnosti a klikněte ikonu blesku. V dalším kroku buď poklepejte na položku v textovém poli vedle `Selecting` události nebo zadejte název obslužné rutiny události, kterou chcete použít. Třetí možností, můžete vytvořit obslužnou rutinu události tak, že vyberete ObjectDataSource a jeho `Selecting` události ze dvou seznamů rozevíracího seznamu v horní části třídy modelu code-behind stránky.


![Klikněte na na ikonu blesku v okně Vlastnosti události ovládacího prvku seznam](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Obrázek 9**: klikněte na na ikonu blesku v okně Vlastnosti události ovládacího prvku seznam


Všechny tři přístupy přidat novou obslužnou rutinu události pro ObjectDataSource `Selecting` událostí k použití modelu code-behind třídy stránky. V této obslužné rutiny události jsme čtení a zápis do hodnot parametru pomocí `e.InputParameters(parameterName)`, kde *`parameterName`* je hodnota `Name` atribut `<asp:Parameter>` značky ( `InputParameters` kolekce může být také indexované ordinally, stejně jako v `e.InputParameters(index)`). Chcete-li nastavit `month` parametr pro aktuální měsíc, přidejte následující text do `Selecting` obslužné rutiny události:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Při návštěvě této stránky prostřednictvím prohlížeče můžeme vidět pouze jednoho zaměstnance byl přijat tento měsíc (březen) Novák Laura, který byl od 1994 ve společnosti.


[![Tyto zaměstnanci, jejichž výročí tento měsíc jsou uvedeny.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Obrázek 10**: těch zaměstnanci jejichž výročí tento měsíc jsou uvedeny ([kliknutím ji zobrazíte obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Souhrn

Když prvku ObjectDataSource parametry obvykle lze nastavit hodnoty deklarativní, bez nutnosti psát kód, je snadné programové nastavení hodnot parametrů. Všechny musíme udělat, je vytvořit obslužnou rutinu události pro ObjectDataSource `Selecting` událost, která se vyvolá před základní objekt metoda vyvolána a ručně nastavit hodnoty pro jeden nebo více parametrů prostřednictvím `InputParameters` kolekce.

V tomto kurzu končí část základní tvorbou sestav. [Další kurz](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) zahajuje filtrování a scénáře hlavních-podrobných oddílu, ve kterém vytvoříme podívejte se na techniky umožňující návštěvníka pro filtrování dat a k podrobnostem z hlavní sestavy do sestavy podrobností.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Hilton Giesenow. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](declarative-parameters-vb.md)
