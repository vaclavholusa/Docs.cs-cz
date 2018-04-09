---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Vytváření Data Access Layer (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu jsme budete začít od samého začátku a vytvořit Data přístup Layer (DAL), pomocí typové datové sady, pro přístup k informacím v databázi.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e1a457c23ef659bf7ee9c15b66dc5c2d8a31416
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-data-access-layer-c"></a>Vytváření Data Access Layer (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) nebo [stáhnout PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> V tomto kurzu jsme budete začít od samého začátku a vytvořit Data přístup Layer (DAL), pomocí typové datové sady, pro přístup k informacím v databázi.


## <a name="introduction"></a>Úvod

Jako vývojářům webů naše život základem práci s daty. Vytvoříme databáze pro ukládání dat, kód pro načtení a upravit ho a webové stránky pro shromažďování a shrnutí ho. Toto je první kurz v zdlouhavé série, která bude prozkoumat techniky pro implementaci těchto běžných vzorů v technologii ASP.NET 2.0. Začneme s vytvářením [architektura softwaru](http://en.wikipedia.org/wiki/Software_architecture) skládá z Data přístup vrstvy DAL () pomocí zadané v datové sady vrstvu obchodní logiky (BLL), která vynucuje vlastní obchodní pravidla a prezentační vrstvy, který se skládá z ASP.NET stránky, která Sdílejte běžné rozložení stránky. Po této základ back-end je uveden, přesunete do vytváření sestav, znázorňující zobrazíte shrnout, shromažďování a ověřit data z webové aplikace. Tyto kurzy jsou zaměřeny na se stručným a poskytují podrobné pokyny s dostatkem snímky obrazovky vizuálně provedou celým procesem. Každý kurzu je k dispozici v C# a Visual Basic verze a zahrnuje stahování kód dokončení použít. (Tento první kurz je velmi náročná, ale zbývající jsou uvedeny v mnohem víc stravitelné bloků.)

Pro tyto kurzy budeme používat verzi Microsoft SQL Server 2005 Express Edition uložena v databázi Northwind **aplikace\_Data** adresáře. Kromě soubor databáze **aplikace\_Data** složka také obsahuje skripty SQL pro vytvoření databáze, v případě, že chcete použít jiné databázi verze. Tyto skripty můžete také být [stáhnout přímo z Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), pokud si přejete. Pokud používáte jinou verzi systému SQL Server databáze Northwind, budete muset aktualizovat **NORTHWNDConnectionString** nastavení do aplikace **Web.config** souboru. Webová aplikace byla vytvořená s využitím Visual Studio 2005 Professional Edition jako projekt webu založeného na systému souborů. Všechny z kurzů, ale bude fungovat stejně dobře s bezplatnou verzi sady Visual Studio 2005 [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
V tomto kurzu jsme budete začít od samého začátku a vytvořit Data přístup Layer (DAL), za nímž následuje vytváření obchodní logiky vrstvy (BLL) v tomto kurzu druhý a práci na rozložení stránky a navigace ve třetí. Kurzy po třetí jeden bude stavějí základ umístěné v první tři. My mnoho na klientech v tomto prvním kurzu, takže fire Visual Studio a můžeme začít!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Krok 1: Vytvoření webového projektu a připojení k databázi

Můžeme vytvořit naše Data přístup Layer (DAL), nejprve musíme vytvořit web a instalační program databáze. Začněte vytvořením nového souboru na základě systému ASP.NET webu. K tomu, přejděte do nabídky soubor a vyberte nový web zobrazení dialogového okna Nový web. Výběr šablony webu ASP.NET, nastavte rozevírací seznam umístění do systému souborů, vyberte složku pro umístění na webu a nastavení jazyka C#.


[![Vytvoření nového souboru na základě systému webového serveru](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Obrázek 1**: vytvoření webu New File System-Based ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image3.png))


Tím se vytvoří nový web s **Default.aspx** stránku ASP.NET a **aplikace\_Data** složky.

S webem vytvořit dalším krokem je přidání odkazu na databázi v Průzkumníku serveru Visual Studio. Přidáním databáze do Průzkumníka serveru můžete přidat tabulky, uložené procedury, zobrazení a tak dále všechna z Visual Studia. Můžete také zobrazit data tabulky nebo vytvořit vlastní dotazy ručně nebo graficky prostřednictvím Tvůrce dotazů. Kromě toho když jsme sestavení typové datové sady pro vrstvy DAL budeme potřebovat k bodu Visual Studio k databázi, ze kterého by měl zkonstruovat typové datové sady. Při můžeme poskytnout informace o tomto připojení v tomto bodě v čase, Visual Studio automaticky naplní rozevírací seznam databází již zaregistrován v Průzkumníku serveru.

Kroky pro přidání databázi Northwind do Průzkumníka serveru závisí na tom, jestli chcete použít v databázi SQL Server 2005 Express Edition **aplikace\_Data** složky nebo pokud máte Microsoft SQL Server 2000 nebo 2005 databáze serveru instalačního programu, která chcete použít místo.

## <a name="using-a-database-in-theappdatafolder"></a>Použití databáze v theApp\_DataFolder

Pokud máte systém SQL Server 2000 nebo 2005 databázový server pro připojení nebo jednoduše se chcete vyhnout se nutnosti tuto databázi přidat k databázovému serveru, můžete použít SQL Server 2005 Express Edition verze databázi Northwind, který je umístěný v stažené websit e's **aplikace\_Data** složky (**NORTHWND. MDF**).

Databáze je uložena v umístění **aplikace\_Data** složky se automaticky přidá do Průzkumníku serveru. Za předpokladu, že máte SQL Server 2005 Express Edition nainstalovaný na počítači byste měli vidět uzel s názvem NORTHWND. MDF v Průzkumníku serveru, který můžete rozbalit a prozkoumat jeho tabulek, zobrazení, uložené procedury a tak dále (viz obrázek 2).

**Aplikace\_Data** složky také můžete podržet Microsoft Access **.mdb** soubory, které jako jejich protějšky systému SQL Server se automaticky přidají do Průzkumníka serveru. Pokud nechcete, aby k používání některé z možností systému SQL Server, můžete vždy [stažení aplikace Microsoft Access verze souboru databáze Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) a umístěte do **aplikace\_Data** adresáře. Mějte na paměti, ale které nejsou databází Access jako bohaté funkce jako systém SQL Server a nejsou určeny k použití ve scénářích webu. Kromě toho bude několik kurzy 35 využívat určité funkce úroveň databáze, které nepodporuje přístup.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Připojení k databázi v databázi serveru Microsoft SQL Server 2000 nebo 2005

Alternativně může připojit k databázi Northwind nainstalovaná na serveru databáze. Pokud databázový server již nemá databázi Northwind nainstalována, je nejprve třeba přidat ji do databázový server spuštěním skriptu instalace zahrnuté v tomto kurzu stahování nebo pomocí [stahování verze systému SQL Server 2000 Northwind a instalační skript](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) přímo z webu společnosti Microsoft.

Jakmile máte databáze nainstalovaná, přejděte do Průzkumníka serveru v sadě Visual Studio, klikněte pravým tlačítkem na uzel datové připojení a zvolit připojení k přidání. Pokud nevidíte Průzkumníka serveru, přejděte do zobrazení nebo Průzkumníka serveru nebo přístupů Ctrl + Alt + S. Tím se otevře dialogové okno Přidat připojení, ve kterém můžete zadat pro připojení k serveru, informace o ověřování a název databáze. Jakmile úspěšně jste nakonfigurovali informace o připojení databáze a kliknutí na tlačítko OK, databáze, bude přidána jako uzel pod tímto uzlem datová připojení. Můžete rozbalit uzel databáze a prozkoumejte tabulek, zobrazení, uložené procedury a tak dále.


![Přidat připojení k databázi serveru databáze Northwind](creating-a-data-access-layer-cs/_static/image4.png)

**Obrázek 2**: Přidat připojení k databázi serveru databáze Northwind


## <a name="step-2-creating-the-data-access-layer"></a>Krok 2: Vytvoření vrstvy přístupu k datům

Při práci s jednou z možností dat je pro vložení logice specifické pro data přímo do prezentační vrstvy (ve webové aplikaci, tvoří prezentační vrstvy pro stránky ASP.NET). To může mít formu psaní kódu ADO.NET v části kódu stránky ASP.NET nebo pomocí ovládacího prvku SqlDataSource z části značek. V obou případech tento přístup pevně spojuje logice přístup dat s prezentační vrstvy. Doporučený postup je však k oddělení logice přístup dat od prezentační vrstvy. Tato vrstva samostatné se označuje jako Data Access Layer DAL pro zkrácení a obvykle je implementované jako samostatné projektu knihovny tříd. Výhody této vrstveného architektury jsou popsány (naleznete v části "Další odečty" na konci tohoto kurzu informace o tyto výhody) a je přístup, jsme bude trvat této série.

Všechny kódu, které jsou specifické pro daný zdroj dat. například vytvoření připojení k databázi, vydání **vyberte**, **vložit**, **aktualizace**, a  **Odstranit** příkazy a tak dále, musí se nacházet ve DAL. Prezentační vrstvy nesmí obsahovat všechny odkazy na takový kód přístup data, ale měli místo toho provádět volání do vrstvy DAL pro data všech žádostí. Datové vrstvy přístup obvykle obsahují metody pro přístup k základní data databáze. Databázi Northwind, například má **produkty** a **kategorie** tabulky, které zaznamenávají produktů pro prodej a kategorie, do kterých patří. V našem DAL máme metody, třeba:

- **GetCategories(),** které vrátí informace o všech kategorií
- **GetProducts()**, který vrátí informace o všech produktů
- **GetProductsByCategoryID (*categoryID*)**, která vrátí všechny produkty, které patří do zadané kategorii
- **GetProductByProductID (*productID*)**, který vrátí informace o konkrétním produktu

Tyto metody, a to po vyvolání bude připojení k databázi, vydávání odpovídající dotaz a vrátí výsledky. Jak vrátíme těchto výsledků je důležité. Tyto metody může vrátit jednoduše datové sady nebo DataReader nenaplnil databázový dotaz, ale v ideálním případě tyto výsledky má být vrácen pomocí *objektů se silným typem*. Objekt silného typu je jejichž schématu je pevně definovaná při kompilaci, zatímco opak, objekt volného typu, je jedním jejichž schématu není znám, dokud modulu runtime.

Například objektu DataReader a datové sady (ve výchozím nastavení) jsou objekty volného typu vzhledem k tomu, že je jejich schéma definované sloupců vrácených dotazem databáze používaných k naplnění je. Pro přístup k určité sloupce z DataTable volného typu musíme použijte syntaxi jako:  <strong><em>DataTable</em>. Řádky [<em>index</em>] ["<em>columnName</em>"]</strong>. Přijít zadáním DataTable v tomto příkladu je vykazují skutečnost, že musíme přístup pomocí řetězec nebo index pořadových název sloupce. DataTable silného typu, na druhé straně, bude mít každý z jeho sloupců implementovaná jako vlastnosti, výsledkem je kód, který vypadá takto:  <strong><em>DataTable</em>. Řádky [<em>index</em>]. *columnName</strong>*.

Pokud chcete vrátit objektů se silným typem, vývojáři můžete vytvořit své vlastní objekty vlastní obchodní nebo použít typové datové sady. Objekt obchodní je implementoval vývojář jako představuje třídu, jejíž vlastnosti obvykle podle sloupce základní tabulky databáze objekt firmy. Typové datové sady je třída vygenerovány pro vás Visual Studio na základě schématu databáze a jejíž členové jsou silného typu podle tohoto schématu. Datová sada zadali, samotné se skládá z třídy, které rozšiřují třídy sady dat ADO.NET, DataTable a DataRow. Kromě silného typu DataTables typové datové sady teď také zahrnovat TableAdapters, které jsou tříd pomocí metody pro naplnění DataTables datovou sadu a šíření úpravy v rámci DataTables zpět do databáze.

> [!NOTE]
> Další informace o výhody a nevýhody použití typové datové sady a vlastní obchodní objekty, najdete v části [navrhování komponenty datové vrstvy a předávání dat prostřednictvím vrstvy](https://msdn.microsoft.com/library/ms978496.aspx).


Pro tyto kurzy architekturu použijeme silného typu datové sady. Obrázek 3 znázorňuje pracovní postup mezi různých úrovní aplikaci, která používá typové datové sady.


[![DAL je předané centrům všech dat přístupového kódu](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Obrázek 3**: všechna Data přístupového kódu je předané centrům DAL ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Vytvoření typové datové sady a tabulka adaptéru

Zahajte proces vytváření naše DAL, začneme přidáním typové datové sady do našich projektu. K tomu, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a zvolte možnost Přidat novou položku. Vyberte datovou sadu možnost ze seznamu šablon a pojmenujte ji **Northwind.xsd**.


[![Pokud se rozhodnete do projektu přidejte nová datová sada](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Obrázek 4**: vyberte, chcete-li přidat novou datovou sadu do vašeho projektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image10.png))


Po kliknutí na tlačítko Přidat, při zobrazení výzvy k přidání datovou sadu, která **aplikace\_kód** složku, klepněte na tlačítko Ano. Návrhář typové datové sady se pak zobrazí a se spustí Průvodce nastavením TableAdapter umožňuje přidat vaše první TableAdapter do typové datové sady.

Typové datové sady slouží jako silného typu kolekce dat; skládá se instancí silného typu DataTable, z nichž každý se skládá zase instancí DataRow silného typu. Pro každou základní tabulky databáze, které potřebujeme pro práci s této série kurzů vytvoříme DataTable silného typu. Začneme vytvořením DataTable pro **produkty** tabulky.

Uvědomte si, že silného typu DataTables neobsahují žádné informace o tom, jak přistupovat k datům z jejich základní tabulky databáze. Aby bylo možné získat data k naplnění DataTable, použijeme TableAdapter třídy, která funguje jako naše Data Access Layer. Pro naše **produkty** DataTable, TableAdapter bude obsahovat metody **GetProducts()**, **GetProductByCategoryID (*categoryID*)**a tak dále, který jsme vám vyvolat od prezentační vrstvy. Role DataTable je sloužit jako objekty silného typu sloužící k předávání dat mezi vrstvami.

Průvodce nastavením TableAdapter začíná požadavkem, abyste vyberte databázi, ke které pracovat. V rozevíracím seznamu uvedena tyto databáze v Průzkumníku serveru. Pokud databázi Northwind nebyla přidána do Průzkumníka serveru, můžete kliknutím na tlačítko nové připojení v tuto chvíli to udělat.


[![V rozevíracím seznamu vyberte databázi Northwind](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Obrázek 5**: v rozevíracím seznamu vyberte databázi Northwind ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image13.png))


Po výběru databáze a kliknutím na tlačítko Další, zobrazí se dotaz, pokud chcete uložit připojovací řetězec do **Web.config** souboru. Uložením připojovací řetězec je budete nepoužívejte ho pevný programového v TableAdapter třídy, který zjednodušuje věcí, pokud v budoucnu změní informace o připojovacím řetězci. Pokud se přihlásíte k uložení připojovací řetězec v konfiguračním souboru je umístěn v **&lt;connectionStrings&gt;** oddíl, což může být [volitelně šifrované](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) pro vylepšené zabezpečení nebo upravené později pomocí nové stránky vlastností ASP.NET 2.0 v rámci nástroje Správce služby IIS grafického uživatelského rozhraní, což je více ideální pro správce.


[![Připojovací řetězec uložit do souboru Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Obrázek 6**: Uložit připojovací řetězec k **Web.config** ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image16.png))


Dále je potřeba definovat schéma pro první DataTable silného typu a zadat první metodu pro naše TableAdapter používat při naplňování datové sady silného typu. Tyto dva kroky jsou současně provést tak, že vytvoříte dotaz, který vrátí sloupce z tabulky, že chceme reflektované v našem DataTable. Na konci tohoto průvodce nám dáte název metody tohoto dotazu. Po které se provádí, tato metoda může být volána z našich prezentační vrstvy. Metoda bude provést definované dotaz a naplnit DataTable silného typu.

Abyste mohli začít definice dotazu SQL jsme, musíte určit jak chceme TableAdapter vydat dotaz. Jsme můžete použít příkaz SQL ad-hoc, vytvořte nové uložené procedury nebo použijte existující uložené procedury. Tyto kurzy použijeme ad-hoc příkazů SQL. Odkazovat na [Ano/Ne Brian](http://briannoyes.net/)je článek, [sestavení Data Access Layer pomocí návrháře DataSet Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) příklad použití uložených procedur.


[![Dotaz na Data pomocí příkazu SQL Ad-Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Obrázek 7**: dotaz na Data pomocí příkazu SQL Ad-Hoc ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image19.png))


V tuto chvíli jsme můžete zadat v příkazu jazyka SQL ručně. Při vytváření první metodu v TableAdapter obvykle chcete dotaz vrátit tyto sloupce, které musí být vyjádřené v odpovídající element DataTable. Nemůžeme se dá dosáhnout vytvořením dotazu, který vrátí všechny sloupce a všechny řádky z **produkty** tabulky:


[![Zadejte příkaz jazyka SQL do textového pole](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Obrázek 8**: Zadejte SQL dotaz do Textbox ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image22.png))


Můžete taky použít Tvůrce dotazů a graficky vytvořit dotaz, jak je znázorněno na obrázku 9.


[![Graficky, vytvořte dotaz pomocí editoru dotazů](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Obrázek 9**: vytvoření dotazu graficky, pomocí editoru dotazů ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image25.png))


Po vytvoření dotazu, ale před Přesun na další obrazovce klikněte na tlačítko Upřesnit možnosti. V projektech webových stránek "příkazy Generovat Insert, Update a Delete" je jedinou rozšířené možnosti vybrané ve výchozím nastavení; Pokud tohoto průvodce spustíte z knihovny tříd nebo projektu pro Windows bude také vybrána možnost "Použít optimistickou metodu souběžného". Nechte nezaškrtnuté možnost "Použít optimistickou metodu souběžného" teď. V budoucích kurzech podíváme optimistickou metodu souběžného.


[![Vyberte pouze generovat Insert, Update a Delete příkazy možnost](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Obrázek 10**: vyberte pouze generovat Insert, Update a Delete příkazy možnost ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image28.png))


Po ověření rozšířené možnosti, klikněte na tlačítko Další pokračujte na poslední obrazovka. Zde jsme požádáni o výběr které metody pro přidání do TableAdapter. Existují dva vzory pro naplnění dat:

- **Vyplnění DataTable** s tímto přístupem vytvoření metody, která přebírá DataTable jako parametr a ta je na základě výsledků dotazu. Třída ADO.NET DataAdapter, například implementuje tento vzor s jeho **Fill()** metoda.
- **Vrátí DataTable** s tímto přístupem metoda vytvoří a doplní DataTable automaticky a vrátí ji jako návratová hodnota metody.

Můžete mít TableAdapter implementovat jednu nebo obě tyto vzory. Můžete také přejmenovat metody, které jsou tady uvedené. Umožňuje ponechat zaškrtnutí obou políček zaškrtnuto, i když budeme pouze používat pozdější vzor v rámci těchto kurzů. Také umožňuje přejmenovat místo Obecné **GetData** metodu **GetProducts**.

Pokud zaškrtnutí políčka konečné "Generatedbdirectmethods –," vytvoří **Insert()**, **Update()**, a **Delete()** metody pro TableAdapter. Pokud tuto možnost necháte nezaškrtnuté, všechny aktualizace potřeba provést prostřednictvím TableAdapter jediný **Update()** metodu, která přebírá typové datové sady, DataTable, jeden DataRow nebo pole DataRows. (Pokud jste zaškrtnutou možnost "generování Insert, Update a Delete příkazy" možnost z rozšířených vlastností na obrázku 9 toto políčko nastavení nebude mít žádný vliv.) Umožňuje ponechat toto políčko zaškrtnuto.


[![Změňte název metody z GetData na GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Obrázek 11**: změnit název metody z **GetData** k **GetProducts** ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image31.png))


Dokončete průvodce kliknutím na tlačítko Dokončit. Po ukončení průvodce jsme se vrátíte na návrháře datovou sadu, která zobrazuje DataTable že jsme právě vytvořili. Zobrazí se seznam sloupců v **produkty** DataTable (**ProductID**, **ProductName**a tak dále), a metody  **ProductsTableAdapter** (**Fill()** a **GetProducts()**).


[![DataTable produkty a ProductsTableAdapter byly přidány do typové datové sady](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Obrázek 12**: **produkty** DataTable a **ProductsTableAdapter** byly přidány do typové datové sady ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image34.png))


V tomto okamžiku máme typové datové sady s jeden DataTable (**Northwind.Products**) a silně typované třídy DataAdapter (**NorthwindTableAdapters.ProductsTableAdapter**) s  **GetProducts()** metoda. Tyto objekty lze použít pro přístup k seznamu všech produktů z kódu jako:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Tento kód nebylo nutné nám zápis jeden bit data specifická pro přístup kód. Nebylo nutné k vytváření instancí třídy žádné ADO.NET, můžeme neměly odkazovat na libovolné púřipojovací řetězce dotazů SQL nebo uložené procedury. Místo toho TableAdapter poskytuje nízké úrovně dat přístupový kód pro nás.

Každý objekt použitý v tomto příkladu je také silného typu, umožní vám technologie IntelliSense a kontrola typu v kompilaci Visual Studio. A nejlepší všechny DataTables vrácené TableAdapter mohou být vázány na data technologie ASP.NET ovládací prvky webového jako GridView, DetailsView, rozevírací seznam, CheckBoxList a několik dalších. Následující příklad ilustruje vazby DataTable vrácený **GetProducts()** metodu GridView v kódu v rámci právě omezená kapacita tři řádky **stránky\_zatížení** obslužné rutiny události.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Zobrazí se seznam produktů v GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Obrázek 13**: produkty v seznamu se zobrazí v GridView ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image37.png))


Tento příklad vyžadují jsme zapisovat tři řádky kódu v našem stránku ASP.NET **stránky\_zatížení** obslužné rutiny události, v budoucnosti kurzy podíváme, jak používat ObjectDataSource deklarativně načtení dat ze DAL. ObjectDataSource jsme není nutné psaní jakéhokoli kódu a získáte také podpora stránkování a řazení!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Krok 3: Přidání parametry metody pro Data Access Layer

V tomto okamžiku naše **ProductsTableAdapter** třída má ale jednu metodu **GetProducts()**, která vrací všechny produkty v databázi. Při výborný užitečné schopnost pracovat s všechny produkty, které jsou časy, kdy budete chceme načíst informace o konkrétní produkt nebo všechny produkty, které patří do určité kategorie. Přidat tyto další funkce naše Data Access Layer jsme můžete přidat do TableAdapter parametrizované metody.

Umožňuje přidat **GetProductsByCategoryID (*categoryID*)** metoda. Pro přidání nové metody vrstvy Dal, vraťte se do návrháře DataSet, klikněte pravým tlačítkem **ProductsTableAdapter** části a vyberte Přidat dotazu.


![Klikněte pravým tlačítkem na TableAdapter a zvolte Přidat dotazu](creating-a-data-access-layer-cs/_static/image38.png)

**Obrázek 14**: klikněte pravým tlačítkem na TableAdapter a zvolte Přidat dotazu


Nejprve jsme výzva o tom, jestli chceme pro přístup k databázi pomocí příkazu ad-hoc SQL nebo uloženou proceduru nový nebo existující. Umožňuje zvolit příkazu SQL ad-hoc znovu. V dalším kroku jsme se dotaz, jaký typ dotazu SQL, jsme chtěli použít. Vzhledem k tomu, že nám chcete vrátit všechny produkty, které patří do zadané kategorii, chceme zápisu **vyberte** příkaz, který vrací řádky.


[![Můžete vytvořit příkaz SELECT, který vrací řádky](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Obrázek 15**: rozhodnete vytvořit **vyberte** řádky vrátí v příkazu který ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image41.png))


Dalším krokem je dotaz SQL, používaný pro přístup k datům. Vzhledem k tomu, že chceme vrátit pouze produkty, které patří do určité kategorie, použít stejné <strong>vyberte</strong> příkaz z <strong>GetProducts()</strong>, ale přidejte následující <strong>kde</strong> klauzule: <strong>kde CategoryID = @CategoryID</strong> . <strong>@CategoryID</strong> Parametr do Průvodce nastavením TableAdapter označuje, že metoda vytváříme se vyžadují vstupní parametr příslušného typu (integer konkrétně, s možnou hodnotou Null).


[![Zadejte dotaz vrátit pouze produkty v zadané kategorii.](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Obrázek 16**: Zadejte dotaz na pouze vrátit produktů v kategorii zadaný ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image44.png))


V posledním kroku, který jsme můžete zvolit který vzory použít, a také přizpůsobit názvy metod generované přístup k datům. Vzorek výplně, změňte název, který má <strong>FillByCategoryID</strong> a pro návratový DataTable vrátit vzoru ( <strong>získat*X</strong>*  metody), použijeme  <strong>GetProductsByCategoryID</strong>.


[![Zvolte názvy metod TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Obrázek 17**: Zvolte názvy metod TableAdapter ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image47.png))


Po dokončení průvodce, zahrnuje návrháře DataSet nových metod TableAdapter.


![Dotaz na produkty můžete nyní podle kategorie](creating-a-data-access-layer-cs/_static/image48.png)

**Obrázek 18**: produktů můžete nyní dotaz podle kategorie


Za chvíli přidat **GetProductByProductID (*productID*)** metodu pomocí stejným způsobem.

Tyto parametrizované dotazy můžete otestovat přímo z Návrháře datovou sadu. Klikněte pravým tlačítkem na metodu v TableAdapter a zvolte náhledu dat. Potom zadejte hodnoty, které mají použít pro parametry a klikněte na verzi Preview.


[![Tyto produkty patřící do kategorie nápoje jsou zobrazeny.](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Obrázek 19**: jsou uvedené ty produkty patřící do kategorie nápoje ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image51.png))


Pomocí **GetProductsByCategoryID (*categoryID*)** metoda v našem DAL jsme teď vytvořit stránku ASP.NET, které se zobrazí jenom tyto produkty v zadané kategorii. Následující příklad ukazuje všechny produkty, které jsou v kategorii Nápoje, které mají **CategoryID** 1.

Beverages.ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Těchto produktů v kategorii nápoje jsou zobrazeny.](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Obrázek 20**: ty produktů v kategorii Nápoje zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Krok 4: Vkládání, aktualizaci a odstraňování dat

Existují dva vzory běžně používá pro vkládání, aktualizaci a odstraňování dat. Prvním vzoru I budete volat přímé vzor databáze, zahrnuje vytvoření metody, která při vyvolání, problém **vložit**, **aktualizace**, nebo **odstranit** příkaz databáze, která pracuje na záznam jedné databáze. Tyto metody jsou obvykle předána řadu skalárních hodnot (celá čísla, řetězce, logické hodnoty, data a času a tak dále), které odpovídají hodnotám vložit, aktualizovat nebo odstranit. Například v tomto vzoru pro **produkty** tabulky metodu delete by trvat parametr celé číslo, označující **ProductID** odstranit, zatímco metoda vložení by trvat v záznamu řetězec **ProductName**, decimal pro **UnitPrice**, celé číslo pro **UnitsOnStock**a tak dále.


[![Každý Insert, Update a odstranit požadavku posílá databáze okamžitě](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Obrázek 21**: každý vložení, aktualizace a odstranění požadavku posílá databáze okamžitě ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image57.png))


Další vzorek, který označovány jako dávky aktualizace vzor, je aktualizovat celou datovou sadu, DataTable nebo kolekce DataRows ve volání jednu metodu. Pomocí tohoto vzoru vývojář odstraní, vloží, upraví DataRows v DataTable a pak předá tyto DataRows nebo DataTable do metodu aktualizace. Tato metoda pak vytvoří výčet DataRows předaná, určuje, zda budou jste byl změněn, přidat nebo odstraněn (prostřednictvím DataRow [RowState vlastnost](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) hodnotu) a vydá požadavek příslušné databáze pro každý záznam.


[![Všechny změny jsou synchronizovány s databází, když je volána metoda aktualizace](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Obrázek 22**: všechny změny jsou synchronizovány s databází, když je volána metoda aktualizace ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter ve výchozím nastavení používá vzoru batch aktualizace, ale také podporuje přímý vzor DB. Vzhledem k tomu, že jsme vybrali možnost "příkazy Generovat Insert, Update a Delete" z Upřesnit vlastnosti při vytváření naše TableAdapter **ProductsTableAdapter** obsahuje **Update()** metody které implementuje vzor aktualizace dávky. Konkrétně TableAdapter obsahuje **Update()** metoda, která mohou být předány typové datové sady, DataTable silného typu nebo jeden nebo více DataRows. Pokud jste nechali zaškrtnuté políčko "Generatedbdirectmethods –", pokud nejdříve vytvořením TableAdapter přímé vzor DB budou prováděny také prostřednictvím **Insert()**, **Update()**, a **Delete()**  metody.

Oba vzorky úpravy dat použít TableAdapter **událost InsertCommand**, **UpdateCommand**, a **DeleteCommand** vlastností k vydávání jejich **vložit** , **Aktualizace**, a **odstranit** příkazů do databáze. Můžete zkontrolovat a upravit **událost InsertCommand**, **UpdateCommand**, a **DeleteCommand** vlastnosti tak, že kliknete na TableAdapter v Návrháři DataSet pak budete v okně Vlastnosti. (Ujistěte se, jste vybrali TableAdapter a který **ProductsTableAdapter** objekt je v rozevíracím seznamu v okně Vlastnosti.)


[![TableAdapter má událost InsertCommand, vlastnost UpdateCommand a DeleteCommand vlastnosti](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Obrázek 23**: TableAdapter má **událost InsertCommand**, **UpdateCommand**, a **DeleteCommand** vlastnosti ([Kliknutím zobrazit v plné velikosti Obrázek](creating-a-data-access-layer-cs/_static/image63.png))


Chcete-li zkontrolovat nebo upravit některé z těchto vlastností příkaz databáze, klikněte na **CommandText** dílčí vlastnosti, které se otevře Tvůrce dotazů.


[![Konfigurace INSERT, UPDATE a DELETE příkazy v Tvůrce dotazů](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Obrázek 24**: konfigurace **vložit**, **aktualizace**, a **odstranit** příkazy v Tvůrce dotazů ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image66.png))


Následující příklad kódu ukazuje, jak používat vzor aktualizace batch zdvojnásobit cenu všechny produkty, které nejsou zrušeny a které mají 25 jednotek v stock nebo méně:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Následující kód ukazuje, jak pomocí vzoru přímé DB prostřednictvím kódu programu odstranit konkrétní produkt a pak aktualizovat jeden a poté přidejte nový:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Vytváření vlastní Insert, Update a Delete metody

**Insert()**, **Update()**, a **Delete()** metody vytvořené DB přímá metoda může být trochu náročná, zejména u tabulek s mnoha sloupci. Vyhledávání v předcházejícím příkladu, bez pomoci na IntelliSense není zvlášť jasné, co **produkty** mapuje sloupec tabulky pro každý vstupní parametr **Update()** a **Insert()**  metody. Mohou nastat situace, kdy pouze chceme aktualizovat jeden sloupec nebo dva, nebo má přizpůsobený **Insert()** metoda, která bude možná, vrátí hodnotu nově vloženou záznam **IDENTITY** (automatického přírůstku) pole.

Pokud chcete vytvořit vlastní metodu, vrátíte do návrháře datovou sadu. Klikněte pravým tlačítkem na TableAdapter a zvolte Přidat dotaz, vrácením Průvodce nastavením TableAdapter. Na obrazovce druhý jsme označují typ dotaz k vytvoření. Umožňuje vytvoření metody, která přidá nového produktu a vrátí hodnotu nově přidaného záznamu **ProductID**. Proto opt vytvořit **vložit** dotazu.


[![Vytvoření metody přidání nového řádku do tabulky produktů](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Obrázek 25**: vytvoření metody, chcete-li přidat nový řádek, abyste **produkty** tabulky ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image69.png))


Na další obrazovce **událost InsertCommand**na **CommandText** se zobrazí. Posílení tento dotaz přidáním **vyberte oboru\_IDENTITY()** na konci tohoto dotazu, který vrátí poslední hodnotu identity, které jsou vloženy do **IDENTITY** sloupec ve stejném oboru. (Najdete v článku [technické dokumentace](https://msdn.microsoft.com/library/ms190315.aspx) Další informace o **oboru\_IDENTITY()** a důvody, budete pravděpodobně chtít [použít OBOR\_IDENTITY() lieu z @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Ujistěte se, že se vám stát **vložit** příkaz s středníkem před přidáním **vyberte** příkaz.


[![Posílení dotaz vrátil hodnoty SCOPE_IDENTITY()](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Obrázek 26**: posílení dotazu k návratu **oboru\_IDENTITY()** hodnotu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image72.png))


Nakonec název nové metody **InsertProduct**.


[![Nastavit nový název metody k InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Obrázek 27**: nastavte nový název metody na **InsertProduct** ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image75.png))


Když se vrátíte návrháře DataSet uvidíte, který **ProductsTableAdapter** obsahuje nové metody, **InsertProduct**. Pokud tato nová metoda nemá parametr pro každý sloupec v **produkty** tabulky, je možné, se jste zapomněli ukončit **vložit** příkaz s středníkem. Konfigurace **InsertProduct** metoda a zajistěte, abyste měli středníkem omezující **vložit** a **vyberte** příkazy.

Ve výchozím nastavení vložte metody problém není dotazem metody, což znamená, že budou vracet počet ovlivněných řádků. Chceme, ale **InsertProduct** metoda vrátí hodnotu vrácených dotazem, nikoli počet ovlivněných řádků. K tomu, upravte **InsertProduct** metody **ExecuteMode** vlastnost **skalární hodnota**.


[![Změňte vlastnost ExecuteMode skalární hodnota](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Obrázek 28**: Změna **ExecuteMode** vlastnost **skalárních** ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image78.png))


Následující kód ukazuje tento nový **InsertProduct** metoda v akci:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Krok 5: Dokončení Data Access Layer

Všimněte si, že **ProductsTableAdapters** vrací **CategoryID** a **KódDodavatele** hodnoty z **produkty** tabulky, ale neobsahuje **CategoryName** sloupec z **kategorie** tabulky nebo **#companyname** sloupec z **Dodavatelé**tabulky, i když tyto jsou pravděpodobně sloupce jsme má zobrazit při zobrazení informací o produktu. Jsme můžete posílení metody počáteční TableAdapter **GetProducts()**, aby současně obsahovat **CategoryName** a **NázevSpolečnosti** hodnoty sloupce, které se aktualizuje silně typované DataTable zahrnout tyto nové sloupce.

Může to znamenat problém, ale, jako TableAdapter metody pro vložení, aktualizaci, a odstraňuje data jsou na základě této počáteční metody. Naštěstí metody automaticky generovaných pro vložení, aktualizace a odstranění nejsou vliv poddotazy v **vyberte** klauzule. Podle dbejte na to přidat naše dotazy na **kategorie** a **Dodavatelé** jako poddotazy, místo **připojení** s, jsme budete vyhnout se nutnosti přepracování tyto metody pro úpravu data. Klikněte pravým tlačítkem na **GetProducts()** metoda v **ProductsTableAdapter** a zvolte Konfigurovat. Potom upravte **vyberte** klauzule, aby vypadal jako:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Aktualizace příkaz SELECT pro metodu GetProducts()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Obrázek 29**: aktualizace **vyberte** údajů **GetProducts()** – metoda ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image81.png))


Po aktualizaci **GetProducts()** metoda používat tento nový dotaz DataTable bude obsahovat dva nové sloupce: **CategoryName** a **dodavatel**.


![Produkty DataTable obsahuje dva nové sloupce](creating-a-data-access-layer-cs/_static/image82.png)

**Obrázek 30**: **produkty** DataTable obsahuje dva nové sloupce


Za chvíli aktualizovat **vyberte** klauzuli v **GetProductsByCategoryID (*categoryID*)** metoda také.

Při aktualizaci **GetProducts()** **vyberte** pomocí **připojení** syntaxe návrháře DataSet nebude možné automaticky generovat metody pro vložení, aktualizace a odstranění použití vzoru přímé DB data databáze. Místo toho budete muset ručně vytvořit je mnohem jako jsme to udělali s **InsertProduct** metoda dříve v tomto kurzu. Kromě toho ručně musíte poskytnout **událost InsertCommand**, **UpdateCommand**, a **DeleteCommand** hodnoty vlastnosti, pokud chcete použít aktualizace vzor dávky.

## <a name="adding-the-remaining-tableadapters"></a>Přidání zbývající TableAdapters

Záloh jsme si prohlédli pouze práce jeden TableAdapter pro tabulku jedné databáze. Databáze Northwind však obsahuje několik související tabulky, které potřebujeme pro práci s naše webové aplikaci. Typové datové sady mohou obsahovat více souvisejících DataTables. Proto dokončení naše DAL je potřeba přidat DataTables pro jiné tabulky, které budeme používat v těchto kurzech. Pokud chcete přidat nový TableAdapter typové datové sady, otevřete návrháře DataSet, klikněte pravým tlačítkem myši v návrháři a zvolte možnost Přidat nebo TableAdapter. Tato akce vytvoří novou DataTable a TableAdapter a vás provede průvodce, které jsme se zaměřili na dříve v tomto kurzu.

Trvat několik minut pro vytvoření následující TableAdapters a metod pomocí následující dotazy. Všimněte si, že dotazy v **ProductsTableAdapter** zahrnují poddotazy se získat názvy kategorií a dodavatele každý produkt. Kromě toho, pokud jste se podle, jste již přidali **ProductsTableAdapter** třídy **GetProducts()** a **GetProductsByCategoryID (*categoryID* )** metody.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![Po přidání čtyři TableAdapters návrháře DataSet](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Obrázek 31**: datové sadě Návrhář po čtyři TableAdapters byly přidány ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Přidání vlastního kódu vrstvy Dal

TableAdapters a DataTables přidány do typové datové sady jsou vyjádřené jako soubor XML Schema Definition (**Northwind.xsd**). Tyto informace schéma můžete zobrazit kliknutím pravým tlačítkem na **Northwind.xsd** soubor v Průzkumníku řešení a zvolte kód zobrazení.


[![Soubor definice (XSD) schématu XML pro Lhota typové datové sady](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Obrázek 32**: soubor definice schématu XML (XSD) pro datovou sadu zadali Lhota ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image88.png))


Tato informace o schématu je přeložit na kód C# nebo Visual Basic v době návrhu při kompilaci nebo za běhu (v případě potřeby), v tomto okamžiku můžete krokovat ho s ladicím programem. Chcete-li zobrazit tento automaticky generovaný kód přejděte do zobrazení tříd a procházení na třídy TableAdapter nebo typové datové sady. Pokud nevidíte zobrazení tříd na obrazovce, přejděte do nabídky zobrazení a vyberte ho z ní nebo stiskněte kombinaci kláves Ctrl + Shift + C. Ze třídy zobrazení se zobrazí vlastnosti, metod a události typové datové sady a TableAdapter třídy. Chcete-li zobrazit kód pro konkrétní metody, dvakrát klikněte na název metody v zobrazení tříd nebo klikněte pravým tlačítkem myši na něm a zvolte Přejít k definici.


![Kontrola automaticky vygenerovaný kód výběrem přejděte do definice zobrazení tříd](creating-a-data-access-layer-cs/_static/image89.png)

**Obrázek 33**: Zkontrolujte automaticky vygenerovaný kód výběrem přejděte do definice zobrazení tříd


Automaticky generovaný kód mohou být šetřič skvělé čas, kód je často velmi obecná a je potřeba upravit tak, aby jedinečným potřebám aplikace. Riziko rozšíření automaticky generovaný kód, ale je, že nástroj, který generuje kód rozhodnout, že je na čase "obnovit" a přepsání příslušná přizpůsobení. U nové koncepce třídu rozhraní .NET 2.0 je snadné rozdělení třídy napříč více souborů. To umožňuje nám přidat vlastní metody, vlastnosti a události na automaticky generovaný třídy bez nutnosti starat o sadě Visual Studio přepsal naše přizpůsobení.

Abychom ukázali, jak přizpůsobit DAL, přidejme **GetProducts()** metodu **SuppliersRow** třídy. **SuppliersRow** třída reprezentuje jeden záznam v **Dodavatelé** tabulky; každého poskytovatele můžete dodavatele nula k různým produktům, tak **GetProducts()** těch, které vrátí produkty zadaný dodavatele. Provedete to vytvořte nový soubor – třída v **aplikace\_kód** složku s názvem **SuppliersRow.cs** a přidejte následující kód:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Tuto třídu dá pokyn kompilátoru, který po sestavení **Northwind.SuppliersRow** třída zahrnout **GetProducts()** metoda jsme definovali. Pokud sestavení projektu a pak se vraťte do zobrazení tříd uvidíte **GetProducts()** nyní uveden jako metoda **Northwind.SuppliersRow**.


![Metoda GetProducts() je nyní součástí Northwind.SuppliersRow – třída](creating-a-data-access-layer-cs/_static/image90.png)

**Obrázek 34**: **GetProducts()** metoda je nyní součástí **Northwind.SuppliersRow** – třída


**GetProducts()** metoda teď umožňuje výčet sadu produktů pro konkrétní dodavatele, jak ukazuje následující kód:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Tato data lze také zobrazit v některém z ASP. Data na NET ovládací prvky webového. Na následující stránce používá ovládací prvek GridView s dvě pole:

- BoundField, který zobrazí název každého dodavatele a
- TemplateField, který obsahuje BulletedList ovládací prvek, který je vázaný na výsledky vrácené tímto **GetProducts()** metoda pro každou dodavatele.

Podíváme, jak zobrazit tyto hlavní podrobné zprávy v budoucí kurzy. Prozatím se v tomto příkladu slouží pro ilustraci použití vlastní metody přidat do **Northwind.SuppliersRow** třídy.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Název společnosti dodavatele je uvedena ve sloupci vlevo, jejich produkty v pravém](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Obrázek 35**: název společnosti dodavatele je uvedena ve sloupci vlevo, jejich produkty v pravém ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Souhrn

Při vytváření webové aplikace vytváření DAL by měl být jeden z vaší první kroky, tím, než začnete vytvářet prezentační vrstvy. Pomocí sady Visual Studio vytváření DAL, podle typové datové sady je úkol, který lze provést v 10 až 15 minut bez nutnosti psaní řádek kódu. Kurzy postoupíte bude stavět na této vrstvy DAL. V [další kurz](creating-a-business-logic-layer-cs.md) jsme budete definovat všechny obchodní pravidla a zjistit, jak implementovat v samostatné vrstvy obchodní logiky.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vytváření DAL, použitím silného typu TableAdapters a DataTables v VS 2005 a technologii ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Navrhování komponenty datové vrstvy a předávání dat prostřednictvím vrstev](https://msdn.microsoft.com/library/ms978496.aspx)
- [Sestavení Data Access Layer pomocí návrháře DataSet Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Šifrování informací o konfiguraci technologie ASP.NET 2.0 aplikace](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter – přehled](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Práce s typové datové sady](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Pomocí přístup k datům silného typu v sadě Visual Studio 2005 a technologií ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Postup rozšíření metod TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Načítání skalární dat z uložené procedury](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video školení na témata, které jsou obsažené v tomto kurzu

- [Vrstvy přístupu k datům v aplikacích ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Jak ručně vytvořit vazbu datovou sadu Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Jak pracovat s datových sad a filtry z aplikace ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu bylo Ron zelený, Hilton Giesenow, společnosti Dennis Patterson, Liz Shulok, opisek Gomez a Carlos Santos. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](creating-a-business-logic-layer-cs.md)
