---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: "Zobrazení dat v grafu s rozhraní ASP.NET Web Pages (Razor) | Microsoft Docs"
author: microsoft
description: "Tato kapitola vysvětluje, jak zobrazit data v grafu. V předchozích kapitol jste zjistili, jak zobrazit data ručně a v mřížce. Tato kapitola vysvětluje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 15daa5fa94094fb50971514a152312fd81a749a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Zobrazení dat v grafu s ASP.NET Web Pages (Razor)
====================
podle [Microsoft](https://github.com/microsoft)

> Tento článek vysvětluje, jak používat grafu pro zobrazení dat na webu technologie ASP.NET Web Pages (Razor) pomocí `Chart` pomocné rutiny.
> 
> **Co se dozvíte**:
> 
> - Postup zobrazení dat v grafu.
> - Jak grafy styl pomocí předdefinované motivy.
> - Ukládání grafů a jak je do mezipaměti pro dosažení vyššího výkonu.
> 
> Jsou to ASP.NET programování v článku nové funkce:
> 
> - `Chart` Pomocné rutiny.
> 
> > [!NOTE]
> > Informace v tomto článku se vztahují na 1.0 webových stránek ASP.NET a webové stránky 2.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Pomocné rutiny grafu

Pokud chcete zobrazit data ve formuláři grafického rozhraní, můžete použít `Chart` pomocné rutiny. `Chart` Pomocník může vykreslit obrázek, který zobrazí data v různých typů grafů. Podporuje mnoho možností pro formátování a označování. `Chart` Pomocník může vykreslit více než 30 typy grafů, včetně všech typů grafů, které mohou být obeznámeni s z aplikace Microsoft Excel nebo jiné nástroje &#8212; plošných grafech panelu grafy, sloupcové grafy řádek grafy a koláčové grafy, spolu s více specializované grafů, jako kontingenčních grafů.

| **Plošný graf** ![Popis: přehled o typ oblasti grafu.](7-displaying-data-in-a-chart/_static/image1.jpg) | **Pruhový graf** ![Popis: přehled o typ pruhového grafu](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Sloupcový graf** ![Popis: přehled o sloupcový graf](7-displaying-data-in-a-chart/_static/image3.jpg) | **Spojnicový graf** ![Popis: Obrázek řádku typ grafu.](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Výsečový graf** ![Popis: obrázek typu výsečového grafu](7-displaying-data-in-a-chart/_static/image5.jpg) | **Graf kurzů akcií** ![Popis: přehled o Stock typ grafu.](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Prvky grafu

Grafy zobrazují data a další prvky jako legendy, osy, řady a tak dále. Následující obrázek znázorňuje mnoho elementů diagramu, které můžete přizpůsobit při použití `Chart` pomocné rutiny. Tento článek ukazuje, jak nastavit některé (ne všechny) z těchto elementů.

![Popis: Obrázek zobrazující prvky grafu](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Vytvoření grafu z dat

Data, která můžete zobrazit v grafu může být z pole, z výsledky vrácené z databáze nebo data, která je v souboru XML.

### <a name="using-an-array"></a>Pomocí pole

Jak je popsáno v [Úvod k technologii ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), pole umožňuje uložit kolekci podobných položek do jedné proměnné. Pole můžete použít tak, aby obsahovala data, která chcete zahrnout do grafu.

Tento postup ukazuje, jak můžete vytvořit graf z dat v polích, pomocí výchozí typ grafu. Také ukazuje, jak zobrazit graf v rámci dané stránky.

1. Vytvořte nový soubor s názvem *ChartArrayBasic.cshtml*.
2. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Kód nejprve vytvoří nový graf a nastaví svou šířku a výšku. Zadejte název grafu pomocí `AddTitle` metoda. Chcete-li přidat data, je použít `AddSeries` metoda. V tomto příkladu použijete `name`, `xValue`, a `yValues` parametry `AddSeries` metoda. `name` Parametr se zobrazí v legendě grafu. `xValue` Parametr obsahuje pole dat, která se zobrazují podél horizontální osy grafu. `yValues` Parametr obsahuje pole dat, který slouží k vykreslení vertikálních bodů grafu.

    `Write` Metoda vykreslí ve skutečnosti grafu. V takovém případě, protože jste neurčili typ grafu, `Chart` pomocná vykreslí jeho výchozí graf, který je sloupcový graf.
3. Spusťte stránku v prohlížeči. Prohlížeč zobrazí graf. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Pomocí dotazu na databázi pro Data grafu

Pokud jsou informace, které chcete grafu v databázi, můžete spustit dotaz na databázi a potom data z výsledků použít k vytvoření grafu. Tento postup ukazuje, jak načíst a zobrazit data z databáze vytvořené v článku [Úvod k práci s databází v lokalitách webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Přidat *aplikace\_Data* složky do kořenového adresáře webu, pokud složce již neexistuje.
2. V *aplikace\_Data* složky, přidejte soubor databáze s názvem *SmallBakery.sdf* popsané v [Úvod k práci s databází v lokalitách webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Vytvořte nový soubor s názvem *ChartDataQuery.cshtml*.
4. Nahradí existující obsah s následujícími službami:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Kód nejprve otevře SmallBakery databáze a přiřadí ji k proměnné s názvem `db`. Tato proměnná představuje `Database` objekt, který lze číst a zapisovat do databáze. V dalším kroku kód spustí dotaz SQL název a ceny každého produktu. Kód vytvoří nový graf a předá databázový dotaz voláním grafu `DataBindTable` metoda. Tato metoda přebírá dva parametry: `dataSource` parametr je pro data z dotazu a `xField` parametr umožňuje nastavit, které sloupce dat se používá pro osy x grafu.

    Jako alternativu k použití `DataBindTable` metodu, můžete použít `AddSeries` metodu `Chart` pomocné rutiny. `AddSeries` Metoda umožňuje nastavit `xValue` a `yValues` parametry. Například místo použití `DataBindTable` metoda takto:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Můžete použít `AddSeries` metoda takto:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Jak vykreslit stejné výsledky. `AddSeries` Metoda je flexibilnější, protože můžete zadat typ grafu a dat více explicitně, ale `DataBindTable` způsob je jednodušší použít, pokud nepotřebujete navíc flexibilitu.
5. Spusťte stránku v prohlížeči. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Pomocí dat XML

Třetí možnost pro grafů, je použití souboru XML jako data grafu. To vyžaduje, aby soubor XML bylo taky soubor schématu (*XSD* soubor), který popisuje strukturu XML. Tento postup ukazuje, jak načíst data ze souboru XML.

1. V *aplikace\_Data* složky, vytvořte nový soubor XML s názvem *data.xml*.
2. Následující text, který je některá data XML o zaměstnanci ve fiktivní společnosti nahraďte existující soubor XML. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. V *aplikace\_Data* složky, vytvořte nový soubor XML s názvem *data.xsd*. (Všimněte si, že tato doba je rozšíření *XSD*.)
4. Nahraďte existující soubor XML s následujícími službami: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. V kořenovém adresáři webu, vytvořte nový soubor s názvem *ChartDataXML.cshtml*.
6. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Kód nejprve vytvoří `DataSet` objektu. Tento objekt se používá ke správě dat, která je pro čtení ze souboru XML a uspořádat podle informací v souboru schématu. (Všimněte si, že na začátek kód obsahuje příkaz `using SystemData`. To je vyžadováno k mohli pracovat `DataSet` objektu. Další informace najdete v tématu [ &quot;pomocí&quot; příkazy a plně kvalifikované názvy](#SB_UsingStatements) dále v tomto článku.)

    V dalším kroku kód vytvoří `DataView` objekt založen na datovou sadu. Zobrazení dat obsahuje objekt, který grafu můžete vázat na &#8212; To znamená, přečtěte si a vykreslení. Vytvoří vazbu grafu dat pomocí `AddSeries` metoda, jak jste viděli dříve při grafů pole dat, ale tentokrát `xValue` a `yValues` parametry jsou nastaveny na `DataView` objektu.

    Tento příklad také ukazuje, jak zadat typ konkrétní grafu. Když dat je přidaný do `AddSeries` metody `chartType` parametr je také nastavena na zobrazení ve výsečovém graf.
7. Spusťte stránku v prohlížeči. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP] 
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Pomocí" příkazů a plně kvalifikované názvy
> 
> Rozhraní .NET Framework, který je na základě technologie ASP.NET Web Pages se syntaxí Razor se skládá z mnoha tisíce součásti (třídy). Chcete-li spravovat pro práci s všechny tyto třídy, se uspořádány do *obory názvů*, které jsou o něco jako knihovny. Například `System.Web` obor názvů obsahuje třídy, které podporují komunikaci prohlížeče a serveru, `System.Xml` obor názvů obsahuje třídy, které se používají k vytvoření a číst soubory XML a `System.Data` obor názvů obsahuje třídy, které umožňují pracovat s daty.
> 
> Chcete-li získat přístup k libovolné dané třídy v rozhraní .NET Framework, je potřeba vědět, ne jenom název třídy, ale taky obor názvů, který třída je v kódu. Např. Chcete-li použít `Chart` pomocné rutiny, kód musí najít `System.Web.Helpers.Chart` třída, která spojuje obor názvů (`System.Web.Helpers`) s názvem třídy (`Chart`). To se označuje jako třídy *úplný* název &#8212; jeho dokončení, jednoznačným umístění v rámci vastness rozhraní .NET Framework. V kódu by vypadat asi takto:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Je však komplikované (a tím zvyšuje i pravděpodobnost chyby) tak, aby měl používání těchto dlouhý, plně kvalifikovaných názvů pokaždé, když chcete k odkazování na třídu nebo pomocné rutiny. Proto, aby bylo snazší použijte názvy tříd, můžete *importovat* obory názvů v co vás zajímá, který je obvykle je jenom několik z mnoha obory názvů v rozhraní .NET Framework. Pokud jste importovali obor názvů, můžete použít jenom název třídy (`Chart`) namísto plně kvalifikovaný název (`System.Web.Helpers.Chart`). Pokud váš kód spustí a zaznamená název třídy, můžou se podívat v jenom oborech názvů, které jste importovali najít třídy.
> 
> Pokud používáte rozhraní ASP.NET Web Pages se syntaxí Razor k vytvoření webové stránky, obvykle používáte stejnou sadu tříd pokaždé, když, včetně `WebPage` třídy, různé pomocné rutiny a tak dále. Uložit práci při importu relevantní obory názvů pokaždé, když vytvoříte web, ASP.NET je nakonfigurován tak, že se automaticky importuje Sada oborů názvů jádra pro každý web. To je důvod, proč jste nebyly museli řešit obory názvů nebo import až nyní; všechny třídy, kterými jste pracovali jsou obory názvů, které jsou již byla naimportována za vás.
> 
> Však někdy je nutné pro práci s třídu, která není v oboru názvů, který je automaticky importován za vás. V takovém případě můžete použít buď plně kvalifikovaný název této třídy a, nebo obor názvů, který obsahuje třídu lze ručně importovat. Chcete-li importovat obor názvů, použijte `using` – příkaz (`import` v jazyce Visual Basic), jak jste viděli dříve v příklad článek.
> 
> Například `DataSet` třída je v `System.Data` oboru názvů. `System.Data` Obor názvů není automaticky k dispozici pro stránky ASP.NET Razor. Proto můžete pracovat `DataSet` pomocí plně kvalifikovaným názvem, můžete použít kód takto:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Pokud budete muset použít `DataSet` třídy opakovaně můžete importovat obor názvů, jako je to a pak použít jenom název třídy kódu:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Můžete přidat `using` příkazy pro všechny ostatní obory názvů rozhraní .NET Framework, kterou chcete odkazovat. Ale, jak je uvedeno, nebudete muset tomu často, protože většina tříd, které budete pracovat se obory názvů, které jsou importovány automaticky pomocí technologie ASP.NET pro použití v *.cshtml* a *.vbhtml* stránky.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Zobrazení grafů uvnitř webové stránky

V příkladech jste se seznámili, pokud vytvoříte graf a pak je graf vykreslen přímo do prohlížeče jako grafika. V mnoha případech je ale chcete zobrazit graf v rámci stránky, ne jenom sám o sobě v prohlížeči. Kvůli tomu vyžaduje ve dvou krocích. Prvním krokem je vytvoření stránky, který generuje grafu, jako jste už viděli.

Druhým krokem je zobrazit výsledné obrázek na další stránce. Pro zobrazení bitovou kopii, můžete použít HTML `<img>` element stejným způsobem, jakým byste zobrazíte všechny bitové kopie. Však místo odkazující na *.jpg* nebo *.png* souboru `<img>` element odkazy *.cshtml* soubor, který obsahuje `Chart` pomocné rutiny, Vytvoří grafu. Při spuštění stránky zobrazení, `<img>` element načte výstup `Chart` pomocné rutiny a vykreslí grafu.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Vytvořte soubor s názvem *ShowChart.cshtml*.
2. Nahradí existující obsah s následujícími službami: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Kód používá `<img>` element zobrazuje graf, který jste vytvořili předtím v *ChartArrayBasic.cshtml* souboru.
3. Spusťte webovou stránku v prohlížeči. *ShowChart.cshtml* zobrazí obrázek grafu na základě kódu, které jsou součástí *ChartArrayBasic.cshtml* souboru.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Určení stylu grafu

`Chart` Pomocná podporuje velký počet možností, které umožňují přizpůsobit vzhled grafu. Můžete nastavit barvy, písma, ohraničení a tak dále. Snadný způsob, jak přizpůsobit vzhled grafu se má používat *motiv*. Motivy jsou soubory informací, které určují způsob vykreslení grafu použít písma, barvy, popisky, palety, hranice a efekty. (Všimněte si, že styl grafu neindikuje typ grafu.)

Následující tabulka uvádí předdefinované motivy.

| Motiv | Popis |
| --- | --- |
| `Vanilla` | Zobrazí červený sloupce v bílé pozadí. |
| `Blue` | Zobrazí modré sloupců na modré barevného přechodu pozadí. |
| `Green` | Zobrazí modré sloupců na zelenou barevného přechodu pozadí. |
| `Yellow` | Zobrazí oranžové sloupce v žlutý barevného přechodu pozadí. |
| `Vanilla3D` | Zobrazí 3D red sloupce v bílé pozadí. |

Můžete zadat motivu, který chcete použít, když vytvoříte nový graf.

1. Vytvořte nový soubor s názvem *ChartStyleGreen.cshtml*.
2. Nahradí existující obsah na stránce s následujícími službami:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Tento kód je stejný jako předchozí příklad, který používá databázi pro data, ale přidá `theme` parametr při vytváření `Chart` objektu. Na obrázku změněné kódu:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Spusťte stránku v prohlížeči. Zobrazit stejná data jako před, ale vypadá zajímavější grafu: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Ukládání grafu

Při použití `Chart` pomocné rutiny, jako je jste viděli, pokud v tomto článku pomocné rutiny znovu vytvoří grafu od začátku pokaždé, když je volána. V případě potřeby kód pro graf také znovu dotazuje databázi nebo znovu načte soubor XML se získat data. V některých případech to může být složité operace, například pokud je databáze, která jste dotazování velký, nebo pokud soubor XML obsahuje velké množství dat. I v případě, že grafu není zahrnují velké množství dat, proces dynamicky vytvoření bitové kopie zabírají prostředky serveru, a pokud mnoho uživatelů o stránku nebo stránky, které zobrazí graf, může být dopad na výkon vašeho webu.

Můžete snížit potenciální dopad vytvoření grafu, můžete vytvořit graf první čas ho potřebovat a pak ho uložte. Když bude znovu požadován grafu, místo jeho opětovného, můžete jenom načíst uloženou verzi a vykreslit.

Graf můžete uložit těmito způsoby:

- Mezipaměti grafu v paměti počítače (na serveru).
- Uložte jako soubor obrázku grafu.
- Uložte graf jako soubor XML. Tato možnost umožňuje upravit graf předtím, než jste jej uložili.

### <a name="caching-a-chart"></a>Ukládání do mezipaměti graf

Po vytvoření grafu, můžete ho mezipaměti. Ukládání do mezipaměti graf znamená, že nemá být vytvořeny znovu, pokud je potřeba znovu nezobrazí. Při ukládání grafu v mezipaměti, je mu klíč, který musí být jedinečný pro tento graf.

Grafy uložené do mezipaměti může být odstraněna, pokud na serveru nedostatek paměti. Kromě toho mezipaměti není zaškrtnuto, pokud se aplikace restartuje z jakéhokoli důvodu. Standardní způsob práce s grafu v mezipaměti je proto vždy nejprve ke kontrole toho, jestli je k dispozici v mezipaměti a pokud ne, pak se vytvořit nebo znovu vytvořit.

1. V kořenovém adresáři vašeho webu, vytvořte soubor s názvem *ShowCachedChart.cshtml*.
2. Nahradí existující obsah s následujícími službami: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>` Zahrnuje značky `src` atribut, který odkazuje na *ChartSaveToCache.cshtml* a předá klíč do tabulky jako řetězec dotazu. Klíč, obsahuje hodnotu &quot;myChartKey&quot;. *ChartSaveToCache.cshtml* soubor obsahuje `Chart` pomocné rutiny, která vytvoří grafu. Vytvoříte tuto stránku za chvíli.

    Na konci stránky je odkaz na stránku s názvem *ClearCache.cshtml*. Je stránka, která také vytvoříte za chvíli. Je nutné *ClearCache.cshtml* pouze k testování ukládání do mezipaměti v tomto příkladu – není odkaz nebo stránka, která by za normálních okolností zahrnout při práci s grafy v mezipaměti.
3. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *ChartSaveToCache.cshtml*.
4. Nahradí existující obsah s následujícími službami:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Kód nejprve ověří, zda nic byl předán jako hodnota klíče v řetězci dotazu. Pokud ano, kód pokusí načíst graf z mezipaměti voláním `GetFromCache` metoda a předání klíč. Pokud ukazuje se, že není nic v mezipaměti v klíči (což by mohlo dojít při prvním grafu se požaduje), vytvoří kód grafu jako obvykle. Po dokončení grafu kód uloží ho do mezipaměti voláním `SaveToCache`. Tato metoda vyžaduje klíč (takže grafu, mohou být požadována později) a množství času, který má být uložen grafu v mezipaměti. (Přesný čas by mezipaměti graf bude záviset na jak často si myslíte, že data, která reprezentuje může změnit.) `SaveToCache` Metoda také vyžaduje, `slidingExpiration` parametr &#8212; Pokud je nastavená na hodnotu true, časový limit čítač se vynuluje pokaždé, když je přístupný grafu. V takovém případě platí znamená to, že grafu položky mezipaměti vyprší po posledního někdo přístupu k grafu 2 minut. (Alternativu ke klouzavé vypršení platnosti je absolutní vypršení platnosti, což znamená, že by položky mezipaměti vyprší přesně 2 minut po byla zařazena do mezipaměti, bez ohledu na to, jak často byl byla přistupoval.)

    Nakonec kód používá `WriteFromCache` metoda pro načtení a vykreslit graf z mezipaměti. Všimněte si, že tato metoda je mimo `if` blok, který hledá v mezipaměti, protože ať už grafu se někdo na začátku nebo musel být vygenerované a uloženy v mezipaměti, se budou získávat graf z mezipaměti.

    Všimněte si, že v příkladu `AddTitle` metoda obsahuje časové razítko. (Přidá aktuální datum a čas &#8212; `DateTime.Now` &#8212; k titulu.)
5. Vytvořit novou stránku s názvem *ClearCache.cshtml* a nahraďte jeho obsah následujícím kódem:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Tato stránka používá `WebCache` pomocníka odebrat graf, který je uložené v mezipaměti v *ChartSaveToCache.cshtml*. Jak již bylo uvedeno dříve, nemáte za normálních okolností jsou na stránce takto. Vytváříte ji sem pouze, aby bylo snazší testování ukládání do mezipaměti.
6. Spustit *ShowCachedChart.cshtml* webovou stránku v prohlížeči. Na stránce se zobrazí obrázek grafu na základě kódu, které jsou součástí *ChartSaveToCache.cshtml* souboru. Poznamenejte si časové razítko uvádí v název grafu. 

    ![Popis: Obrázek základní grafu s časovým razítkem v název grafu](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Zavřete prohlížeč.
8. Spustit *ShowCachedChart.cshtml* znovu. Všimněte si, že časové razítko je stejný jako předtím, což naznačuje, že se znovu nevygeneroval grafu, ale místo toho byl načten z mezipaměti.
9. V *ShowCachedChart.cshtml*, klikněte **vymazat mezipaměť** odkaz. Tím přejdete *ClearCache.cshtml*, který hlásí, že byl vymazán mezipaměti.
10. Klikněte na tlačítko **vrátit do ShowCachedChart.cshtml** odkaz nebo opětovným spuštěním *ShowCachedChart.cshtml* ze služby WebMatrix. Všimněte si, že Tentokrát časové razítko došlo ke změně, protože mezipaměti byl vymazán. Proto kód byl znovu vygenerovat grafu a vrátit zpět do mezipaměti.

### <a name="saving-a-chart-as-an-image-file"></a>Uložení grafu do souboru bitové kopie

Můžete také uložit graf jako soubor obrázku (například jako *.jpg* soubor) na serveru. Pak můžete soubor bitové kopie způsob, jakým byste žádný obrázek. Výhoda spočívá v souboru je uložené spíše než uložit do dočasného mezipaměti. Můžete uložit nové obrázku grafu v různou dobu (například každou hodinu) a potom zaznamenejte si trvalé změny, ke kterým došlo v průběhu času. Všimněte si, že musíte zkontrolovat, zda webová aplikace má oprávnění k uložení souboru do složky na serveru, kam chcete umístit soubor bitové kopie.

1. V kořenovém adresáři vašeho webu, vytvořte složku s názvem  *\_ChartFiles* Pokud ještě neexistuje.
2. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *ChartSave.cshtml*.
3. Nahradí existující obsah s následujícími službami:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Kód nejdřív zkontroluje, zda *.jpg* soubor existuje voláním `File.Exists` metoda. Pokud soubor neexistuje, kód vytvoří novou `Chart` z pole. Tento čas, kód zavolá metodu `Save` metoda a předává `path` parametr k určení místa k uložení grafu název souboru a cesta k souboru. V těle stránky `<img>` element používá cesta tak, aby odkazoval *.jpg* soubor k zobrazení.
4. Spustit *ChartSave.cshtml* souboru.
5. Vraťte se do služby WebMatrix. Všimněte si, že soubor bitové kopie s názvem *chart01.jpg* byla uložena do  *\_ChartFiles* složky.

### <a name="saving-a-chart-as-an-xml-file"></a>Uložení grafu do souboru XML

Nakonec můžete uložit graf jako soubor XML na serveru. Výhodou použití této metody oproti grafu do mezipaměti nebo uložení grafu do souboru je před zobrazením grafu, pokud jste chtěli, je možné upravit soubor XML. Aplikace musí mít oprávnění ke čtení/zápisu pro složku na serveru, kam chcete umístit soubor bitové kopie.

1. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *ChartSaveXml.cshtml*.
2. Nahradí existující obsah s následujícími službami:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Tento kód je podobný kód, který jste předtím viděli pro ukládání grafu v mezipaměti, s tím rozdílem, že používá soubor XML. Kód nejprve zkontroluje, zda existuje soubor XML při volání `File.Exists` metoda. Pokud soubor neexistuje, kód vytvoří novou `Chart` objektu a předá název souboru, jako `themePath` parametr. Tím se vytvoří graf založený na ať je v souboru XML. Pokud soubor XML ještě neexistuje, kód vytvoří graf jako normální a pak zavolá `SaveXml` ji uložit. Je graf vykreslen pomocí `Write` metoda, jak jste jste viděli.

    Stejně jako u stránky, který vám ukázal, ukládání do mezipaměti, tento kód obsahuje časovým razítkem v název grafu.
3. Vytvořit novou stránku s názvem *ChartDisplayXMLChart.cshtml* a přidejte do ní následující kód: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Spustit *ChartDisplayXMLChart.cshtml* stránky. Grafu se zobrazí. Poznamenejte si časové razítko v nadpisu grafu.
5. Zavřete prohlížeč.
6. Ve službě WebMatrix, klikněte pravým tlačítkem myši  *\_ChartFiles* složku, klikněte na tlačítko **aktualizovat**a potom otevřete složku. *XMLChart.xml* soubor v této složce byl vytvořen `Chart` pomocné rutiny. 

    ![Popis: Složka _ChartFiles zobrazující soubor XMLChart.xml vytvořené pomocné rutiny grafu.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Spustit *ChartDisplayXMLChart.cshtml* stránku znovu. Graf zobrazuje časové razítko stejný jako při prvním spuštění stránky. Je to způsobeno grafu je generován z XML, který jste předtím uložili.
8. Ve službě WebMatrix, otevřete  *\_ChartFiles* složku a odstraňte *XMLChart.xml* souboru.
9. Spustit *ChartDisplayXMLChart.cshtml* stránka ještě jednou. Tentokrát časové razítko aktualizováno, protože `Chart` pomocná museli znovu vytvořte soubor XML. Pokud chcete, podívejte se  *\_ChartFiles* složku a Všimněte si, že soubor XML je zpátky.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky

- [Úvod k práci s databází v rozhraní ASP.NET Web Pages lokalit](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Pomocí ukládání do mezipaměti v rozhraní ASP.NET Web Pages lokalitách a zlepšit výkon](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Třída grafu](https://msdn.microsoft.com/en-us/library/system.web.helpers.chart(v=vs.99)) (rozhraní API ASP.NET Web Pages referenční dokumentace na webu MSDN)
