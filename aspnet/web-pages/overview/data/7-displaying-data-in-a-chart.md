---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Zobrazení dat v grafu s webovými stránkami ASP.NET (Razor) | Dokumentace Microsoftu
author: microsoft
description: Tato kapitola popisuje způsob zobrazení dat v grafu. V předchozích kapitol jste zjistili, jak zobrazit data ručně a v tabulce. Tato kapitola popisuje...
ms.author: aspnetcontent
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 161dfa1b2c0676c79baebb00e303e8cb9df1d4e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812578"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Zobrazení dat v grafu s webovými stránkami ASP.NET (Razor)
====================
podle [Microsoft](https://github.com/microsoft)

> Tento článek vysvětluje, jak pomocí grafu pro zobrazení dat na webu rozhraní ASP.NET Web Pages (Razor) s použitím `Chart` pomocné rutiny.
> 
> **Co se dozvíte**:
> 
> - Postup zobrazení dat v grafu.
> - Jak styl grafy pomocí předdefinované motivy.
> - Jak ukládat grafy a jak pro ukládání do mezipaměti pro zajištění lepšího výkonu.
> 
> Toto jsou ASP.NET programování funkcí představených v následujícím článku:
> 
> - `Chart` Pomocné rutiny.
> 
> > [!NOTE]
> > Informace v tomto článku se vztahují na 1.0 webových stránek ASP.NET a Web Pages 2.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Pomocné rutiny grafu

Pokud chcete zobrazit data v grafické podobě, můžete použít `Chart` pomocné rutiny. `Chart` Pomocné rutiny může mít za následek bitovou kopii, která zobrazí data v různých typů grafů. Podporuje řadu možností pro formátování a používání popisků. `Chart` Pomocné rutiny může mít za následek více než 30 typů grafů, včetně všech typů grafů, které je možné, že znáte z Microsoft Excelu nebo jiné nástroje &#8212; plošné grafy, pruhový, sloupcový graf, spojnicové grafy a výsečové grafy, společně s více specializované grafů, jako jsou Burzovní grafy.

| **Plošný graf** ![Popis: obrázek typu oblasti grafu](7-displaying-data-in-a-chart/_static/image1.jpg) | **Pruhový graf** ![Popis: obrázek typ pruhového grafu](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Sloupcový graf** ![Popis: obrázek sloupcový graf](7-displaying-data-in-a-chart/_static/image3.jpg) | **Spojnicový graf** ![Popis: obrázek grafu typu řádku](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Výsečový graf** ![Popis: obrázek typu výsečový graf](7-displaying-data-in-a-chart/_static/image5.jpg) | **Graf kurzů akcií** ![Popis: obrázek uložený typ grafu](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Prvky grafu

Grafy zobrazují data a další prvky jako legendy, osy, řady a tak dále. Následující obrázek ukazuje mnoho prvků grafu, které můžete přizpůsobit, pokud použijete `Chart` pomocné rutiny. V tomto článku se dozvíte, jak nastavit některé (ne všechny) z těchto elementů.

![Popis: Obrázek prvky grafu](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Vytváří se graf z dat

Data, která se zobrazí v grafu lze z pole, ve výsledcích vrácených z databáze nebo data, která jsou v souboru XML.

### <a name="using-an-array"></a>Pomocí pole

Jak je vysvětleno v [Úvod do ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), pole umožňuje do jedné proměnné ukládat kolekci podobných položek. Pole můžete použít tak, aby obsahovala data, která chcete zahrnout do grafu.

Tento postup ukazuje, jak můžete vytvořit graf z dat v polích pomocí výchozí typ grafu. Také ukazuje, jak zobrazit graf v rámci stránky.

1. Vytvořte nový soubor s názvem *ChartArrayBasic.cshtml*.
2. Nahraďte existující obsah následujícím kódem: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Kód nejprve vytvoří nový graf a nastaví její šířku a výšku. Zadejte název grafu pomocí `AddTitle` metody. Chcete-li přidat data, použijete `AddSeries` metody. V tomto příkladu použijete `name`, `xValue`, a `yValues` parametry `AddSeries` metody. `name` Parametrů se zobrazí v legendě grafu. `xValue` Parametr obsahuje pole dat, které se zobrazují podél horizontální osy grafu. `yValues` Parametr obsahuje pole dat, který slouží k vykreslení vertikálních bodů grafu.

    `Write` Metoda ve skutečnosti vykreslí grafu. V tomto případě, protože jste neurčili, nevložily typu grafu `Chart` výchozí graf, který sloupcový graf vykresluje pomocné rutiny.
3. Spuštění stránky v prohlížeči. Prohlížeč zobrazí v grafu. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Použití databázového dotazu pro Data grafu

Pokud jsou informace, které chcete graf v databázi, můžete spustit dotaz na databázi a pak použít data z výsledků k vytvoření grafu. Tento postup ukazuje, jak číst a zobrazení dat z databáze vytvoří v následujícím článku [Úvod k práci s databází na webech ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Přidat *aplikace\_Data* složku do kořenového adresáře webu, pokud složce již neexistuje.
2. V *aplikace\_Data* složky, přidejte soubor databáze s názvem *SmallBakery.sdf* , který je popsaný v [Úvod k práci s databází v ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Vytvořte nový soubor s názvem *ChartDataQuery.cshtml*.
4. Nahraďte existující obsah následujícím kódem:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Kód nejprve otevře SmallBakery databáze a přiřadí ji k proměnné s názvem `db`. Představuje tuto proměnnou `Database` objekt, který je možné číst z a zapisovat do databáze. V dalším kroku spuštění kódu jazyka SQL k získání názvu a ceny jednotlivých produktů. Kód vytvoří nový graf a předá dotaz databáze pomocí volání grafu `DataBindTable` metody. Tato metoda přebírá dva parametry: `dataSource` parametr je pro data z dotazu a `xField` parametr umožňuje nastavit, které sloupce dat se používá pro osy x grafu.

    Jako alternativu k použití `DataBindTable` metodu, můžete použít `AddSeries` metodu `Chart` pomocné rutiny. `AddSeries` Metoda vám umožňuje nastavit `xValue` a `yValues` parametry. Například místo použití `DataBindTable` metoda takto:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Můžete použít `AddSeries` metoda takto:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Jak se vykreslují stejné výsledky. `AddSeries` Metoda je flexibilnější, protože typ grafu a dat můžete určit více explicitně, ale `DataBindTable` způsob je jednodušší použít, pokud není nutné mimořádná flexibilita.
5. Spuštění stránky v prohlížeči. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Pomocí dat XML

Třetí možnost pro vytvoření grafu je použít soubor XML jako data grafu. To vyžaduje, že soubor XML také mít schéma soubor (*XSD* souboru), který popisuje struktuře XML. Tento postup ukazuje, jak číst data ze souboru XML.

1. V *aplikace\_Data* složku, vytvořte nový soubor XML s názvem *data.xml*.
2. Nahraďte stávající kód XML následující text, který je některá data XML o zaměstnance ve fiktivní společnosti. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. V *aplikace\_Data* složku, vytvořte nový soubor XML s názvem *data.xsd*. (Všimněte si, že rozšíření je nyní *XSD*.)
4. Nahraďte existující soubor XML s následujícími možnostmi: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. V kořenovém adresáři webu vytvořte nový soubor s názvem *ChartDataXML.cshtml*.
6. Nahraďte existující obsah následujícím kódem: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Kód nejprve vytvoří `DataSet` objektu. Tento objekt se používá ke správě dat, která je pro čtení ze souboru XML a uspořádat je podle informací v souboru schématu. (Všimněte si, že horní části kódu obsahuje příkaz `using SystemData`. Je to nutné, aby bylo možné pracovat `DataSet` objektu. Další informace najdete v tématu [ &quot;použití&quot; příkazy a plně kvalifikované názvy](#SB_UsingStatements) dále v tomto článku.)

    V dalším kroku se vytvoří kód `DataView` objektu podle datové sady. Zobrazení dat obsahuje objekt, který lze svázat grafu &#8212; to znamená, přečtěte si a vykreslení. Sváže data s využitím grafu `AddSeries` metody, jako je viděli dříve při dat pole, s výjimkou, že na graf tentokrát `xValue` a `yValues` parametry jsou nastaveny na `DataView` objektu.

    Tento příklad také ukazuje, jak zadat typ konkrétní graf. Při přidání dat do `AddSeries` metody `chartType` parametr je také nastavena na zobrazení výsečového grafu.
7. Spuštění stránky v prohlížeči. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Pomocí" příkazy a plně kvalifikované názvy
> 
> ASP.NET Web Pages se syntaxí Razor založené na rozhraní .NET Framework se skládá z mnoha tisícům součásti (třídy). Aby se daly spravovat pro práci s všechny tyto třídy, jsou uspořádány do *obory názvů*, což je něco jako knihovny. Například `System.Web` obor názvů obsahuje třídy, které podporují komunikaci mezi serverem a prohlížečem, `System.Xml` obor názvů obsahuje třídy, které se používají k vytvoření a čtení souborů XML a `System.Data` obor názvů obsahuje třídy, které umožňují pracovat s daty.
> 
> Za účelem přístupu k jakékoli dané třídy v rozhraní .NET Framework, potřebuje vědět, ne jenom název třídy, ale také obor názvů, třída je v kódu. Například, chcete-li použít `Chart` pomocné rutiny, kód potřebuje najít `System.Web.Helpers.Chart` třída, která kombinuje obor názvů (`System.Web.Helpers`) s názvem třídy (`Chart`). To se označuje jako třídy *plně kvalifikovaný* název &#8212; jednoznačnou, kompletní umístění v rámci vastness rozhraní .NET Framework. V kódu bude vypadat nějak takto:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Je však náročný (a náchylné k chybám) k použití těchto dlouhý, plně kvalifikovaných názvů pokaždé, když chcete odkazovat na třídu nebo pomocné rutiny. Proto, aby bylo snazší používat názvy tříd, můžete *importovat* obory názvů vás zajímá, což je obvykle je jenom několik z mnoha obory názvů v rozhraní .NET Framework. Pokud importujete obor názvů, můžete použít jenom název třídy (`Chart`) namísto plně kvalifikovaný název (`System.Web.Helpers.Chart`). Pokud váš kód běží a zaznamená název třídy, může vypadat v právě obory názvů, která jste naimportovali najít tuto třídu.
> 
> Když použijete rozhraní ASP.NET Web Pages se syntaxí Razor k vytvoření webové stránky, obvykle použijete stejné sady tříd pokaždé, když, včetně `WebPage` třídy, různé pomocné rutiny a tak dále. Chcete-li uložit práci import příslušných oborů názvů pokaždé, když vytvoříte web, technologie ASP.NET je konfigurována tak automaticky importuje Sada oborů názvů jádra pro každý web. To je důvod, proč ještě museli vypořádat s obory názvů nebo import až nyní; všechny třídy, kterou jste pracovali s jsou v oborech názvů, které již byly naimportovány za vás.
> 
> Ale někdy potřebujete pro práci s třídou, která není v oboru názvů, který je automaticky importován za vás. V takovém případě můžete použít buď plně kvalifikovaný název této třídy, nebo lze ručně importovat obor názvů obsahující třídu. Import oboru názvů, můžete použít `using` – příkaz (`import` v jazyce Visual Basic), jak jste viděli dříve v příkladu článku.
> 
> Například `DataSet` hodina může začít `System.Data` oboru názvů. `System.Data` Obor názvů není automaticky k dispozici pro stránky ASP.NET Razor. Proto pro práci s `DataSet` pomocí jeho plně kvalifikovaného názvu třídy, můžete použít kód následujícím způsobem:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Pokud je nutné použít `DataSet` opakovaně třídy můžete importovat oboru názvů následujícím způsobem a pak použít jenom název třídy v kódu:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Můžete přidat `using` příkazy pro všechny ostatní obory názvů rozhraní .NET Framework, která se má odkazovat. Ale jak bylo uvedeno, nemusíte k tomu často, protože většina tříd, které budete pracovat s jsou v oborech názvů, které jsou importovány automaticky pomocí technologie ASP.NET pro použití v *.cshtml* a *.vbhtml* stránky.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Zobrazení grafů na webové stránce

V příkladech jste viděli zatím, vytvoření grafu a pak je graf vykreslen přímo do prohlížeče jako grafika. V mnoha případech je však chcete zobrazit graf jako součást stránky, ne jenom sám o sobě v prohlížeči. K tomu vyžaduje dvoustupňový proces. Prvním krokem je vytvoření stránky, který generuje graf, jak jsme už jednou vyskytl.

Druhým krokem je zobrazíte výsledná bitová kopie v jiné stránce. A zobrazte obrázek, použijete HTML `<img>` element ve stejném způsobem, jakým byste pro zobrazení libovolné obrázku. Však namísto odkazování *.jpg* nebo *.png* soubor, `<img>` elementu odkazy *.cshtml* soubor, který obsahuje `Chart` pomocné rutiny, který vytvoří graf. Při spuštění zobrazení stránky, `<img>` prvku předá výstup `Chart` pomocné rutiny a vykreslí grafu.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Vytvořte soubor s názvem *ShowChart.cshtml*.
2. Nahraďte existující obsah následujícím kódem: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Tento kód použije `<img>` element zobrazuje graf, který jste vytvořili dříve v *ChartArrayBasic.cshtml* souboru.
3. Spusťte webovou stránku v prohlížeči. *ShowChart.cshtml* zobrazí obrázek grafu na základě kódu obsažené v souboru *ChartArrayBasic.cshtml* souboru.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Práce se styly grafu

`Chart` Pomocné rutiny podporuje spoustu možností, které umožňují přizpůsobit vzhled grafu. Můžete nastavit barvy, písma, ohraničení a tak dále. Snadný způsob, jak přizpůsobit vzhled grafu je použít *motiv*. Motivy jsou soubory informací, které určují, jak u vykreslovaného grafu použít písma, barvy, popisky, palety, ohraničení a efekty. (Všimněte si, že styl grafu nenaznačuje, že typ grafu.)

V následující tabulce jsou uvedeny předdefinované motivy.

| Motiv | Popis |
| --- | --- |
| `Vanilla` | Zobrazí červený sloupce na bílém pozadí. |
| `Blue` | Zobrazí modré sloupce na modrém barevného přechodu pozadí. |
| `Green` | Zobrazí modré sloupců na zelené barevného přechodu pozadí. |
| `Yellow` | Zobrazí oranžové sloupce na žlutou barevného přechodu pozadí. |
| `Vanilla3D` | Zobrazí 3D red sloupce na bílém pozadí. |

Můžete zadat motivu, který chcete použít při vytváření nového grafu.

1. Vytvořte nový soubor s názvem *ChartStyleGreen.cshtml*.
2. Nahraďte existující obsah na stránce s následujícími možnostmi:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Tento kód je stejný jako předchozí příklad, který používá databázi pro data, ale přidá `theme` parametr, když se vytvoří `Chart` objektu. Následuje ukázka změněný kód:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Spuštění stránky v prohlížeči. Zobrazit stejná data jako před ale zajímavější vzhled grafu: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Uložení grafu

Při použití `Chart` pomocné rutiny, jak jste se seznámili s dosud v tomto článku pomocné rutiny znovu vytvoří graf úplně od začátku pokaždé, když je vyvolána. V případě potřeby kód pro graf také znovu dotazuje databázi nebo znovu přečte soubor XML, které se mají získat data. V některých případech to může být složitá operace, například pokud je databáze, který dotazujete na velká, nebo pokud soubor XML obsahuje velké množství dat. I v případě, že v grafu nezahrnuje velké množství dat, proces dynamicky vytváření image zabírá prostředky serveru, a pokud mnoho lidí o stránku nebo stránky, které se zobrazí v grafu, může být dopad na výkon vašeho webu.

Chcete-li vám pomůže snížit potenciální dopad vytvoření grafu, můžete vytvořit graf první čas ho potřebovat a pak ho uložte. Když v grafu, je potřeba znovu, místo jeho opětovného vytváření stačí načíst uloženou verzi a vykreslit.

Graf můžete uložit následujícími způsoby:

- Mezipaměti grafu v paměti počítače (na serveru).
- Uložte graf jako soubor obrázku.
- Uložte graf jako soubor XML. Tato možnost umožňuje upravit graf předtím, než jste jej uložili.

### <a name="caching-a-chart"></a>Ukládání do mezipaměti grafu

Po vytvoření grafu můžete udržovat v mezipaměti je. Ukládání do mezipaměti grafu znamená, že to nemusí být znovu vytvořen, pokud je potřeba znovu nezobrazí. Při ukládání grafu v mezipaměti, je jí klíč, který musí být jedinečné pro tento graf.

Grafy, které jsou uloženy do mezipaměti může být odebrán, pokud na serveru nedostatek paměti. Kromě toho nevymažete mezipaměť Pokud z nějakého důvodu restartování aplikace. Proto je standardní způsob pro práci s grafem v mezipaměti je vždy nejprve zkontrolujte, zda je k dispozici v mezipaměti a pokud ne, pak vytvořit nebo znovu vytvořit.

1. V kořenovém adresáři vašeho webu, vytvořte soubor s názvem *ShowCachedChart.cshtml*.
2. Nahraďte existující obsah následujícím kódem: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>` Značka zahrnuje `src` atribut, který odkazuje na *ChartSaveToCache.cshtml* souboru a předává klíče na stránce jako řetězec dotazu. Klíč, obsahuje hodnotu &quot;myChartKey&quot;. *ChartSaveToCache.cshtml* soubor obsahuje `Chart` pomocné rutiny, která vytvoří graf. Tato stránka vytvoříte za chvíli.

    Na konci stránky je odkaz na stránku s názvem *ClearCache.cshtml*. To je stránka, která také vytvoříte za chvíli. Je nutné *ClearCache.cshtml* pouze k testování ukládání do mezipaměti v tomto příkladu – není odkaz nebo stránka, která by obvykle obsahovat při práci s grafy v mezipaměti.
3. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *ChartSaveToCache.cshtml*.
4. Nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Kód nejprve zkontroluje, zda nic byl předán jako hodnotu klíče v řetězci dotazu. Pokud ano, pokusí načíst graf z mezipaměti voláním kód `GetFromCache` metoda a předají se jí klíč. Pokud se ukázalo se, že není nic do mezipaměti podle klíče (což by mohlo dojít při prvním vyžádání grafu), kód vytvoří graf jako obvykle. Po dokončení se v grafu, kód uloží jej do mezipaměti voláním `SaveToCache`. Tato metoda vyžaduje klíč (takže grafu, mohou být požadována později) a množství času, který má být uložen v grafu v mezipaměti. (Přesného času by mezipaměti grafu bude trvat, závisí na tom, jak často jste si mysleli, že mohou změnit data, která představuje.) `SaveToCache` Také vyžaduje metodu `slidingExpiration` parametr &#8212; Pokud je tato možnost nastavená na hodnotu true, časový limit čítače se vynuluje pokaždé, když je přístupná v grafu. V tomto případě ve skutečnosti znamená, že neprší platnost položky mezipaměti grafu 2 minut po posledním dostal někdo grafu. (Alternativa k klouzavé vypršení platnosti je absolutní vypršení platnosti, což znamená, že položka mezipaměti vyprší po byla zařazena do mezipaměti bez ohledu na to, jak často má nim přesně 2 minuty).

    Nakonec kód použije `WriteFromCache` má metoda načíst a vykreslit graf z mezipaměti. Všimněte si, že tato metoda je mimo `if` blok, který kontroluje mezipaměti, protože se budou získávat graf z mezipaměti, zda grafu na začátku se nebo si museli vygenerovat a uložit do mezipaměti.

    Všimněte si, že v tomto příkladu `AddTitle` metoda obsahuje časové razítko. (Přidá aktuálním datem a časem &#8212; `DateTime.Now` &#8212; na titulek.)
5. Vytvoří novou stránku s názvem *ClearCache.cshtml* a nahraďte jeho obsah následujícím kódem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Tato stránka používá `WebCache` pomocná rutina pro odebrání graf, který se uloží do mezipaměti v *ChartSaveToCache.cshtml*. Jak bylo uvedeno dříve, není nutné obvykle mít stránky následujícím způsobem. Vytváříte ho tady pouze, aby bylo snazší testování ukládání do mezipaměti.
6. Spustit *ShowCachedChart.cshtml* webovou stránku v prohlížeči. Na stránce se zobrazí obrázek grafu na základě kódu součástí *ChartSaveToCache.cshtml* souboru. Poznamenejte si časové razítko říká v nadpisu grafu. 

    ![Popis: Přehled o základní graf s časovým razítkem v nadpisu grafu](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Zavřete prohlížeč.
8. Spustit *ShowCachedChart.cshtml* znovu. Všimněte si, že časové razítko je stejná jako předtím, což znamená, že se znovu nevygeneroval grafu, ale místo toho byl načten z mezipaměti.
9. V *ShowCachedChart.cshtml*, klikněte na tlačítko **vymazat mezipaměť** odkaz. Tím přejdete do *ClearCache.cshtml*, která hlásí, že se vymazala mezipaměť.
10. Klikněte na tlačítko **vrátit ShowCachedChart.cshtml** odkaz nebo znovu spusťte *ShowCachedChart.cshtml* ze služby WebMatrix. Všimněte si, že Tentokrát časové razítko se změnil, protože se vymazala mezipaměť. Proto kód byl znovu vygenerujte graf a vrátit ji do mezipaměti.

### <a name="saving-a-chart-as-an-image-file"></a>Uložit graf jako soubor bitové kopie

Můžete také uložit graf jako soubor obrázku (třeba jako *.jpg* soubor) na serveru. Pak můžete soubor obrázku tak, jak by žádný obrázek. Výhoda spočívá v souboru je uložené spíše než uloží dočasná mezipaměť. Můžete uložit nové obrázku grafu v různých časech (například každou hodinu) a potom sledovat trvalé změny, které nastanou v průběhu času. Poznámka: Ujistěte se, že vaše webová aplikace má oprávnění k uložení souboru do složky na serveru, kam chcete umístit soubor bitové kopie.

1. V kořenovém adresáři vašeho webu, vytvořte složku s názvem  *\_ChartFiles* Pokud ještě neexistuje.
2. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *ChartSave.cshtml*.
3. Nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Kód nejprve zkontroluje, jestli *.jpg* soubor existuje voláním `File.Exists` metody. Pokud soubor neexistuje, vytvoří nový kód `Chart` z pole. Tentokrát, kód volá `Save` metoda a předá `path` parametru určete cestu k souboru a název souboru, kam chcete graf uložit. V těle stránky `<img>` prvek používá tak, aby odkazoval na cestu *.jpg* soubor k zobrazení.
4. Spustit *ChartSave.cshtml* souboru.
5. Vraťte se do služby WebMatrix. Všimněte si, že soubor image s názvem *chart01.jpg* byla uložena do  *\_ChartFiles* složky.

### <a name="saving-a-chart-as-an-xml-file"></a>Uložit graf jako soubor XML

Nakonec můžete uložit graf jako soubor XML na serveru. Výhodou této metody pomocí grafu do mezipaměti nebo uložení grafu do souboru je, že můžete změnit tak, že kód XML před zobrazením grafu, pokud byste chtěli. Aplikace musí mít oprávnění ke čtení/zápisu pro složku na serveru, kam chcete umístit soubor bitové kopie.

1. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *ChartSaveXml.cshtml*.
2. Nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Tento kód je podobný kód, který jste předtím viděli pro ukládání grafu v mezipaměti, s tím rozdílem, že používá soubor XML. Kód nejprve zkontroluje, jestli existuje soubor XML voláním `File.Exists` metody. Pokud soubor neexistuje, vytvoří nový kód `Chart` objektu a předá název souboru, jako `themePath` parametru. Tím se vytvoří graf založený na cokoli, co je v souboru XML. Pokud soubor XML ještě neexistuje, kód vytvoří graf jako normální a pak zavolá `SaveXml` ji uložit. Graf je vykreslen pomocí `Write` způsob, jak jste se seznámili s před.

    Stejně jako u stránky, které jsme si ukázali, ukládání do mezipaměti, tento kód obsahuje časové razítko v nadpisu grafu.
3. Vytvoří novou stránku s názvem *ChartDisplayXMLChart.cshtml* a přidejte do ní následující kód: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Spustit *ChartDisplayXMLChart.cshtml* stránky. Zobrazí se graf. Poznamenejte si časové razítko v nadpisu grafu.
5. Zavřete prohlížeč.
6. Ve službě WebMatrix, klikněte pravým tlačítkem  *\_ChartFiles* složky, klikněte na **aktualizovat**a poté otevřete složku. *XMLChart.xml* soubor v této složce byl vytvořen `Chart` pomocné rutiny. 

    ![Popis: Složka _ChartFiles zobrazující XMLChart.xml soubor vytvořený graf pomocné rutiny.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Spustit *ChartDisplayXMLChart.cshtml* stránku znovu nezobrazovat. Tento graf zobrazuje stejné časové razítko jako první čas spuštění stránky. Důvodem je skutečnost, že v grafu je právě generován ze souboru XML, které jste předtím uložili.
8. Ve Webmatrixu otevřete  *\_ChartFiles* složky a delete *XMLChart.xml* souboru.
9. Spustit *ChartDisplayXMLChart.cshtml* stránce ještě jednou. Tentokrát, časové razítko je aktualizována, protože `Chart` pomocné rutiny museli znovu vytvořit soubor XML. Pokud chcete, zaškrtněte  *\_ChartFiles* složky a Všimněte si, že je soubor XML zpět.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky

- [Úvod k práci s databází v rozhraní ASP.NET Web Pages lokalit](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Pomocí ukládání do mezipaměti v rozhraní ASP.NET Web Pages lokalitách a zlepšit výkon](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Třída grafu](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (reference k rozhraní API technologie ASP.NET webové stránky na webu MSDN)
