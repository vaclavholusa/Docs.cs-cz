---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Vytvoření vrstvy přístupu k datům (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu začneme od úplného začátku a vytvořit na datový přístup vrstvou DAL, pomocí zadané datové sady, přístup k informacím v databázi.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e124edc137db8444c687092f33db7e03aaeb179
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398317"
---
<a name="creating-a-data-access-layer-c"></a>Vytvoření vrstvy přístupu k datům (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) nebo [stahovat PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> V tomto kurzu začneme od úplného začátku a vytvořit na datový přístup vrstvou DAL, pomocí zadané datové sady, přístup k informacím v databázi.


## <a name="introduction"></a>Úvod

Jako webové vývojáře naše životy – ať točí kolem práce s daty. Vytvoříme databáze k ukládání dat, kód pro načítání a úpravy a webových aplikací, aby shromažďujících a shrnujících ho. Toto je první kurz zdlouhavé série, se bude věnovat technik implementace těchto běžných vzorů v technologii ASP.NET 2.0. Začneme s vytvářením [softwarovou architekturu](http://en.wikipedia.org/wiki/Software_architecture) skládající se z na datový přístup vrstvou DAL pomocí datové vrstvy obchodní logiky (BLL) zadané sady, která vynucuje vlastní podniková pravidla a prezentační vrstva skládá z technologie ASP.NET, které stránky Sdílejte společný rozložení stránky. Jakmile tento back-endu základy byla přijata, přejdeme do vytváření sestav, ukazuje, jak zobrazit, shrnout, shromažďovat a ověřit data z webové aplikace. Tyto kurzy jsou zaměřené na být stručné a poskytují podrobné pokyny s dostatečným snímky obrazovky, který vás provede procesem vizuálně. Každý kurz je dostupná v C# a Visual Basic verze a nabízí ke stažení kompletní kód používá. (Tento první kurz je poměrně dlouhé, ale ostatní jsou uvedeny v mnohem více stravitelné bloků.)

Pro tyto kurzy budeme používat Microsoft SQL Server 2005 Express Edition verzi databáze Northwind, umístí do **aplikace\_Data** adresáře. Kromě souborů databáze **aplikace\_Data** složka také obsahuje skripty SQL pro vytvoření databáze, v případě, že chcete použít jiné databázi verze. Tyto skripty mohou také být [stáhnout přímo od Microsoftu](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), pokud si přejete. Pokud používáte jinou verzi systému SQL Server databáze Northwind, budete muset aktualizovat **NORTHWNDConnectionString** nastavení v aplikačním **Web.config** souboru. Webová aplikace byla vytvořena pomocí Visual Studio 2005 Professional Edition jako projekt webu založeného na systému souborů. Všechny kurzy, ale bude fungovat stejně dobře s bezplatnou verzi Visual Studio 2005 [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
V tomto kurzu začneme od úplného začátku a vytvořit na datový přístup vrstvou DAL, za nímž následuje vytváření obchodní logiky vrstvy (BLL) v druhé části kurzu a práci na rozložení stránky a navigace ve třetí. V kurzech po třetí příkaz budou vycházejí z základ podle první tři. Máme hodně na klientech v tomto prvním kurzu, takže pusťte sady Visual Studio a můžeme začít!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Krok 1: Vytvoření webového projektu a připojení k databázi

Než vytvoříme našich dat přístup vrstvy DAL (), nejprve musíme vytvořit webovou stránku a nastavit naší databázi. Začněte tím, že vytvoříte nový soubor na základě systému ASP.NET Web. K tomu, přejděte do nabídky soubor a zvolte nový web zobrazení dialogového okna Nový web. Výběr šablony webové stránky ASP.NET, nastavte rozevírací seznam umístění do systému souborů, vyberte složku, umístěte na webu a nastavit jazyk C#.


[![Vytvoření nového souboru na základě systému webového serveru](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Obrázek 1**: vytvoření webu New File System-Based ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image3.png))


Tím se vytvoří nový web s **Default.aspx** stránky technologie ASP.NET a **aplikace\_Data** složky.

Se vytvořili na webu dalším krokem je přidání odkazu do databáze v Průzkumníku serveru Visual Studio. Přidejte databázi do Průzkumníka serveru můžete přidat tabulky, uložené procedury, zobrazení a tak dále všechny z Visual Studia. Můžete také zobrazit data tabulky nebo vytvořit ručně nebo graficky své vlastní dotazy pomocí Tvůrce dotazů. Navíc když vytváříme typované datové sady pro vrstvy DAL potřebujeme bodu Visual Studio k databázi, ze kterého třeba vytvořit datové sady typu. Zatímco můžeme poskytnout tyto informace o připojení v tomto okamžiku v čase, Visual Studio automaticky naplní rozevírací seznam databází už zaregistrovaný v Průzkumníku serveru.

Kroky pro přidání databáze Northwind do Průzkumníka serveru závisí na tom, jestli chcete použít databázi SQL Server 2005 Express Edition v **aplikace\_Data** složky nebo pokud máte Microsoft SQL Server 2000 nebo 2005 nastavení serveru databáze, kterou chcete použít místo toho.

## <a name="using-a-database-in-theappdatafolder"></a>Použití databáze v theApp\_%{datafolder/

Pokud máte SQL Server 2000 nebo 2005 databázový server pro připojení k nebo chcete jednoduše vyhnout se tak nutnosti přidat databázi k databázovému serveru, můžete použít SQL Server 2005 Express Edition verzi databáze Northwind, který se nachází v stažený websit e's **aplikace\_Data** složky (**NORTHWND. MDF**).

Databáze umístěn v **aplikace\_Data** složku se automaticky přidá do Průzkumníka serveru. Za předpokladu, že máte SQL Server 2005 Express Edition na vašem počítači nainstalovaný byste měli vidět uzel s názvem NORTHWND. MDF v Průzkumníku serveru, který lze rozbalit a prozkoumat její tabulek, zobrazení, uložené procedury a tak dále (viz obrázek 2).

**Aplikace\_Data** složky může také obsahovat aplikace Microsoft Access **.mdb** soubory, které jako jejich protějšky v systému SQL Server, se automaticky přidají do Průzkumníka serveru. Pokud nechcete použít některou z možností SQL serveru, můžete vždy [stažení aplikace Microsoft Access verzi souboru databáze Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) myší do **aplikace\_Data** adresáře. Mějte na paměti, ale který Accessové databáze nejsou jako plně funkční jako SQL Server a nejsou určeny pro použití ve scénářích webu. Kromě toho několik více než 35 kurzy budou využívat určité funkce úrovni databáze, které nejsou podporovány přístup.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Připojení k databázi v databázi serveru Microsoft SQL Server 2000 nebo 2005

Alternativně může připojit k databázi Northwind na databázovém serveru nainstalovaná. Pokud databázový server ještě není nainstalována databáze Northwind, nejprve musíte přidat ho k databázovému serveru spuštěním skriptu instalace zahrnuté v tomto kurzu stažení nebo podle [stahování verze systému SQL Server 2000 Northwind a instalační skript](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) přímo z webu společnosti Microsoft.

Jakmile budete mít databázi nainstalovat, přejděte do Průzkumníka serveru v sadě Visual Studio, klikněte pravým tlačítkem na uzel datové připojení a zvolte Přidat připojení. Pokud nevidíte přejděte do zobrazení Průzkumníka serveru / Průzkumník serveru nebo přístupů Ctrl + Alt + S. Tím se otevře dialogové okno Přidat připojení, kde můžete určit server pro připojení k, informace o ověřování a název databáze. Jakmile úspěšně jste nakonfigurovali informace o připojení databáze a kliknutí na tlačítko OK, přidá se jako uzel pod uzlem datová připojení databáze. Rozbalte databázi a prozkoumejte tabulek, zobrazení, uložené procedury a tak dále.


![Přidat připojení k databázi Northwind databázový Server](creating-a-data-access-layer-cs/_static/image4.png)

**Obrázek 2**: Přidání připojení k databázi Northwind databázový Server


## <a name="step-2-creating-the-data-access-layer"></a>Krok 2: Vytvoření vrstvy přístupu k datům

Při práci s data jednou z možností je vložení logiky týkající se dat přímo do prezentační vrstvy (ve webové aplikaci, tvoří prezentační vrstvy pro stránky ASP.NET). To může být ve tvaru psaní kódu technologie ADO.NET v části kódu stránky technologie ASP.NET nebo pomocí ovládacího prvku SqlDataSource z část kódu. V obou případech se tento přístup pevně spojuje logikou přístupu dat s prezentační vrstvy. Doporučený postup je však k oddělení logiky přístupu k datům od prezentační vrstvy. Tato vrstva samostatné se označuje jako vrstva přístupu k datům, DAL zkráceně a obvykle je implementována jako samostatný projekt knihovny tříd. Výhody této vrstvy architektury jsou dobře zdokumentovaná (viz oddíl "Další čtení" na konci tohoto kurzu o tyto výhody) a my podnikneme v této sérii přístup.

Veškerý kód, který je specifický pro podkladovému zdroji dat, jako je například vytváření připojení k databázi, vydávání **vyberte**, **vložit**, **aktualizace**, a  **Odstranit** příkazy a tak dále musíte umístit do vrstvy DAL. Prezentační vrstva by neměl obsahovat žádné odkazy na takový kód přístupu k datům, ale místo toho by měl provádět volání do vrstvy DAL pro všechny požadavky. Vrstvy přístupu k datům obvykle obsahují metody pro přístup k datům podkladové databáze. Databáze Northwind, například obsahuje **produkty** a **kategorie** tabulek, které zaznamenávají produktů pro prodej a kategorie, do kterých patří. V našem DAL máme metody, například:

- **GetCategories(),** která vrátí informace o všech kategorií
- **GetProducts()**, která vrátí informace o všech produktů
- **GetProductsByCategoryID (*categoryID*)**, která vrátí všechny produkty, které patří do zadané kategorie
- **GetProductByProductID (*productID*)**, která vrátí informace o daném produktu

Tyto metody při vyvolání, bude připojení k databázi, proveďte odpovídající dotaz a vrátí výsledky. Jakým způsobem jsme vrátit výsledky, je důležité. Tyto metody může jednoduše vrací datovou sadu nebo DataReader vyplněn databázového dotazu, ale v ideálním případě tyto výsledky by měly být vráceny za použití *objektů se silným typem*. Objekt se silnou typovou je jedním jehož schéma je pevně definovaná v době kompilace, zatímco opak volného typu objektu, je jedna jehož schéma není znám, dokud modulu runtime.

Objektu DataReader a datovou sadu (ve výchozím nastavení) jsou objekty volného typu, protože jejich schématu je definovaný podle sloupců vrácených dotazem databáze použitých k naplnění je. Pro přístup ke konkrétním sloupci z DataTable volného typu musíme použijte syntax jako:  <strong><em>DataTable</em>. Řádky [<em>index</em>] ["<em>Názevsloupce</em>"]</strong>. Vykazují dojde ke ztrátě zadáním objektu DataTable v tomto příkladu je skutečnost, že budeme potřebovat pro přístup k názvu sloupce použít řetězec nebo index pořadových. Objekt DataTable silného typu na druhé straně, budou mít všechny její sloupce implementovány jako vlastnosti, výsledkem je kód, který bude vypadat takto:  <strong><em>DataTable</em>. Řádky [<em>index</em>]. *Názevsloupce</strong>*.

K vrácení objektů se silným typem, vývojáře můžou vytvořit své vlastní objekty vlastních obchodních nebo používají typované datové sady. Obchodní objekt je implementoval vývojář, protože představuje třídu, jejíž vlastnosti obvykle zahrnují sloupce základní tabulky databáze, obchodní objekt. Typované datové sady je třída, vygeneruje se pro vás sada Visual Studio na základě schématu databáze a jejíž členové jsou silného typu podle tohoto schématu. Zadané samotná datová sada se skládá z třídy, které rozšiřují třídy DataSet technologie ADO.NET, DataTable a objekt DataRow. Kromě DataTables silného typu zadané datové sady teď také obsahují objekty TableAdapter, které jsou třídy pomocí metody pro naplnění datové sady DataTables změny a šíření změn v rámci DataTables zpět do databáze.

> [!NOTE]
> Další informace na výhody a nevýhody používání typované datové sady a vlastní obchodní objekty [navrhování komponenty datové vrstvy a předávání dat prostřednictvím vrstev](https://msdn.microsoft.com/library/ms978496.aspx).


Pro architekturu těchto kurzů použijeme silně typovaných datových sad. Obrázek 3 znázorňuje pracovní postupy mezi různé vrstvy aplikace, která používá zadané datové sady.


[![Všechny kód přístupu k datům je předané centrům vrstvy Dal](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Obrázek 3**: všechny kód přístupu k datům je předané centrům vrstvy Dal ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Vytvoření typové datové sady a tabulka adaptéru

Zahajte proces vytváření naše DAL, začneme přidáním typované datové sady do projektu. Chcete-li to provést, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a zvolte Přidat novou položku. Vybrat datovou sadu ze seznamu šablon a pojmenujte ho **Northwind.xsd**.


[![Přidat novou datovou sadu do projektu](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Obrázek 4**: chcete přidat novou datovou sadu do vašeho projektu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image10.png))


Po kliknutí na tlačítko Přidat, po zobrazení výzvy k přidat datovou sadu, která **aplikace\_kód** složky, zvolte Ano. Návrhář pro zadané datové sady se potom zobrazí a se spustí Průvodce nastavením TableAdapter, vám umožňuje přidat k datové sadě zadán první TableAdapter.

Datové sady typu slouží jako kolekce silného typu dat; skládá se z silného typu instance objektu DataTable zase z nichž každý se skládá z instance objektu DataRow silného typu. Vytvoříme objekt DataTable silného typu pro každou z podkladové databázové tabulky, které potřebujeme pro práci v této sérii kurzů. Začněme vytvořením datové tabulky pro **produkty** tabulky.

Uvědomte si, že silného typu DataTables neobsahují žádné informace o tom, jak přistupovat k datům z jejich podkladové databázové tabulky. Pokud chcete načíst data pro naplnění DataTable, používáme třídy TableAdapter, který funguje jako náš vrstvy přístupu k datům. Pro naše **produkty** DataTable, TableAdapter bude obsahovat metody **GetProducts()**, **GetProductByCategoryID (*categoryID*)**, a tak dále, který jsme vám vyvolat od prezentační vrstvy. Role DataTable je sloužit jako silně typované objekty sloužící k předávání dat mezi vrstvami.

Průvodce nastavením TableAdapter začíná výzvou k výběru databázi, kterou chcete pracovat. Rozevíracím seznamu zobrazí tyto databáze v Průzkumníku serveru. Pokud databázi Northwind nebyl přidán do Průzkumníka serveru, můžete kliknutím na tlačítko nové připojení v tuto chvíli k tomu.


[![Z rozevíracího seznamu zvolte databázi Northwind](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Obrázek 5**: z rozevíracího seznamu zvolte databázi Northwind ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image13.png))


Po výběru databáze a kliknutí na tlačítko Další, budete požádáni Pokud chcete uložit připojovací řetězec **Web.config** souboru. Uložením připojovací řetězec budete vyhnout, minimu měl pevné zakódovat třídy TableAdapter, což zjednodušuje věci, pokud se v budoucnu změní informace o připojovacím řetězci. Pokud se připojíte k uložení připojovacího řetězce v konfiguračním souboru je umístěný v **&lt;connectionStrings&gt;** oddíl, což může být [volitelně šifrované](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) lepší zabezpečení nebo upravené později pomocí nové stránky vlastností ASP.NET 2.0 v rámci služby IIS grafickým uživatelským rozhraním nástroj pro správu, což je více ideální pro správce.


[![Uložit připojovací řetězec do souboru Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Obrázek 6**: Uložit připojovací řetězec do **Web.config** ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image16.png))


Dále musíme definovat schéma pro první objekt DataTable silného typu a představují první metodu pro naše TableAdapter při naplňování datové sady silného typu. Tyto dva kroky jsou současně lze provést tak, že vytvoříte dotaz, který vrátí sloupce z tabulky, že má být v naší datové tabulky. Na konci průvodce poskytujeme název metody tohoto dotazu. Jakmile, který se provádí, tuto metodu lze volat z našich prezentační vrstvy. Metoda spustí definovaný dotaz a naplnit DataTable silného typu.

Abyste mohli začít definovat dotaz SQL musí nejprve Udáváme jak chceme TableAdapter vydat dotaz. Jsme můžete použít příkaz SQL ad-hoc, vytvořit novou úložnou proceduru nebo použít stávající úložnou proceduru. Těchto kurzech používáme SQL příkazy ad-hoc. Odkazovat na [Ano/Ne Brian](http://briannoyes.net/)uživatele na článek, [vytvoření vrstvy přístupu k datům s návrháři datové sady Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) příklad použití uložených procedur.


[![Dotazování dat pomocí příkazu SQL Ad-Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Obrázek 7**: dotazování dat pomocí příkazu SQL Ad-Hoc ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image19.png))


V tuto chvíli jsme můžete zadat v dotazu SQL ručně. Při vytváření první metoda v TableAdapter obvykle chcete dotaz vrátit sloupce, které musí být vyjádřena v odpovídajícím objektu DataTable. Můžeme to provést tak, že vytvoříte dotaz, který vrátí všechny sloupce a všechny řádky z **produkty** tabulky:


[![Do textového pole zadejte příkaz jazyka SQL](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Obrázek 8**: Zadejte SQL dotazu do textovém poli ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image22.png))


Můžete také pomocí Tvůrce dotazů a graficky vytvořit dotaz, jak je znázorněno na obrázku 9.


[![Vytvořit dotaz graficky, pomocí editoru dotazů](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Obrázek 9**: vytvořit dotaz graficky, pomocí editoru dotazů ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image25.png))


Po vytvoření dotazu, ale před přechodem na další obrazovce klikněte na tlačítko Upřesnit možnosti. V projektech webových stránek "příkazy Generovat Insert, Update a Delete" je jediná rozšířené možnosti vybrané ve výchozím nastavení; tohoto průvodce spustíte z knihovny tříd nebo projekt Windows bude také vybrána možnost "Pomocí optimistického řízení souběžnosti". Zatím nechejte nastavené možnosti "Pomocí optimistického řízení souběžnosti" není zaškrtnuto. Prozkoumáme optimistického řízení souběžnosti v budoucích kurzech.


[![Vyberte pouze generovat Insert, Update a Delete příkazy možnost](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Obrázek 10**: vyberte pouze generovat Insert, Update a Delete příkazy možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image28.png))


Po ověření rozšířené možnosti, klikněte na tlačítko Další přejděte na poslední obrazovce. Tady jsme se výzva k výběru které metody chcete přidat do TableAdapter. Existují dva způsoby pro naplňování dat:

- **Naplnit DataTable** s tímto přístupem vytvoří metodu, která přijímá jako parametr DataTable a naplní ho na základě výsledků dotazu. Třída adaptéru dat ADO.NET, například implementuje tento model s jeho **Fill()** metody.
- **Vrátit tabulku DataTable** s tímto přístupem metoda vytvoří a naplňuje objekt DataTable za vás a vrátí jej jako návratová hodnota metody.

Můžete mít TableAdapter, implementujte jeden nebo oba tyto vzory. Můžete také přejmenovat metody tady. Ponecháme obou políček zaškrtnuto, přestože jenom popíšeme druhý případ v rámci těchto kurzů. Navíc přejmenujme spíše obecný **GetData** metodu **GetProducts**.

Pokud je zaškrtnuto, finální zaškrtávací políčko "GenerateDBDirectMethods," vytvoří **Insert()**, **Update()**, a **Delete()** metody pro TableAdapter. Pokud tuhle možnost necháte nezaškrtnuté, všechny aktualizace, bude nutné provést prostřednictvím jediného objektu TableAdapter **Update()** metoda, která přebírá zadané datové sady, datové tabulky, jednoho datového řádku nebo pole DataRows. (Pokud jste toto políčko zaškrtnuté políčko "generovat Insert, Update a Delete příkazy" možnost rozšířené vlastnosti na obrázku 9 nastavení nebude mít žádný efekt.) Ponecháme toto políčko zaškrtnuto.


[![Změnit název metody z GetData na GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Obrázek 11**: změnit název metody z **GetData** k **GetProducts** ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image31.png))


Dokončete průvodce kliknutím na tlačítko Dokončit. Po zavření průvodce jsme se vrátí do návrháře datových sad, které zobrazuje objekt DataTable, že jsme právě vytvořili. Zobrazí se seznam sloupců v **produkty** DataTable (**ProductID**, **ProductName**, a tak dále), stejně jako metody  **ProductsTableAdapter** (**Fill()** a **GetProducts()**).


[![Objekt DataTable produkty a ProductsTableAdapter byly přidány k datové sadě zadán](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Obrázek 12**: **produkty** DataTable a **ProductsTableAdapter** byly přidány k datové sadě zadán ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image34.png))


V tuto chvíli máme typované datové sady jedné datové tabulky (**Northwind.Products**) a třídu adaptéru dat silného typu (**NorthwindTableAdapters.ProductsTableAdapter**) s  **GetProducts()** metody. Tyto objekty lze použít pro přístup k seznamu všech produktů, které z kódu, jako jsou:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Tento kód nevyžadoval nám napsat jeden bit kód specifické pro přístup k datům. Nebylo nutné k vytvoření instance libovolné třídy rozhraní ADO.NET, jsme neměli k odkazování na libovolné púřipojovací řetězce dotazů SQL nebo uložených procedur komponentami TableAdapter. Místo toho TableAdapter přístupový kód nízké úrovně dat nám poskytuje.

Každý objekt použitý v tomto příkladu je také silného typu, což Visual Studio, abyste technologie IntelliSense a kontrola typu v době kompilace. A to nejlepší z všechny datové tabulky vrácené TableAdapter může být vázaný na data technologie ASP.NET webové ovládací prvky, například prvku GridView, DetailsView, DropDownList, CheckBoxList a několik dalších. Následující příklad ukazuje vazby vrácený objekt DataTable **GetProducts()** metodu GridView v kódu v rámci jen omezená kapacita tři řádky **stránky\_zatížení** obslužné rutiny události.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Zobrazí se seznam produktů v GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Obrázek 13**: seznam produktů se zobrazí v GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image37.png))


V tomto příkladu vyžadují napíšeme tři řádky kódu na naší stránce ASP.NET **stránky\_zatížení** obslužná rutina události, v budoucích kurzech prozkoumáme postupy použití ovládacího prvku ObjectDataSource deklarativně načítat data z VRSTVY DAL. Ovládacím prvkem ObjectDataSource jsme není potřeba psát jakýkoli kód a získáte také podporu stránkování a řazení.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Krok 3: Přidání parametry metody pro vrstvy přístupu k datům

V tomto okamžiku naše **ProductsTableAdapter** třída má ale jedna metoda **GetProducts()**, který vrátí všechny produkty v databázi. Schopnost pracovat u všech produktů je jednoznačně užitečné, jsou časy, kdy jsme budete chtít načíst informace o konkrétní produkt nebo všechny produkty, které patří do určité kategorie. Přidejte tyto funkce do našich vrstvy přístupu k datům jsme parametry metody přidat do TableAdapter.

Přidejme **GetProductsByCategoryID (*categoryID*)** metody. Chcete-li přidat novou metodu vrstvy Dal, vraťte se na návrháři datových sad klikněte pravým tlačítkem **ProductsTableAdapter** a zvolte Přidat dotaz.


![Klikněte pravým tlačítkem na TableAdapter a zvolte Přidat dotaz](creating-a-data-access-layer-cs/_static/image38.png)

**Obrázek 14**: klikněte pravým tlačítkem na TableAdapter a zvolte Přidat dotaz


Můžeme se nejdřív zobrazí výzva o, jestli chcete přístup k databázi pomocí ad-hoc příkazu SQL nebo uloženou proceduru nové nebo existující. Umožňuje zvolit použití příkazu SQL ad-hoc znovu. Dále jsme se výzva, jaký typ dotazu SQL, jsme chtěli použít. Protože chceme vrátit všechny produkty, které patří do zadané kategorie, chceme napsat **vyberte** příkaz, který vrátí řádky.


[![Můžete vytvořit příkaz SELECT, který vrátí řádky](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Obrázek 15**: Zvolte možnost vytvořit **vyberte** příkaz který vrací řádky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image41.png))


Dalším krokem je definování dotaz SQL použitý pro přístup k datům. Protože chceme vrátit pouze produkty, které patří do určité kategorie, použiji stejný <strong>vyberte</strong> příkaz z <strong>GetProducts()</strong>, ale přidejte následující <strong>kde</strong> klauzule: <strong>kde CategoryID = @CategoryID</strong> . <strong>@CategoryID</strong> Parametr označuje do Průvodce vytvořením objektu TableAdapter, že metoda vytváříme budou vyžadovat vstupní parametr typu odpovídající (konkrétně, s možnou hodnotou Null celé číslo).


[![Zadejte dotaz a vrátit pouze produktech v určené kategorii](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Obrázek 16**: Zadejte dotaz pouze vrátit produktů v zadané kategorii ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image44.png))


V posledním kroku, abychom si mohli vybrat který vzorce a používat, stejně jako vlastní názvy metod, vygeneruje pro přístup k datům. Vzorek výplně Změníme název, který má <strong>FillByCategoryID</strong> a pro vrácený objekt DataTable vrátí vzor ( <strong>získat*X</strong>*  metody), použijeme  <strong>GetProductsByCategoryID</strong>.


[![Zvolte názvy metody třídy TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Obrázek 17**: Zvolte názvy metody třídy TableAdapter ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image47.png))


Po dokončení průvodce, návrháři datových sad zahrnuje nové metody třídy TableAdapter.


![Dotazování na produkty můžete nyní podle kategorie](creating-a-data-access-layer-cs/_static/image48.png)

**Obrázek 18**: dotazování produkty můžete nyní podle kategorie


Za chvíli přidat **GetProductByProductID (*productID*)** metodu pomocí stejným způsobem.

Tyto parametrizovaných dotazů můžete otestovat přímo v návrháři datových sad. Klikněte pravým tlačítkem na metodu v TableAdapter a zvolit Data ve verzi Preview. Pak zadejte hodnoty pro parametry a klikněte na tlačítko ve verzi Preview.


[![Jsou uvedeny tyto produkty, který patří do kategorie Nápoje](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Obrázek 19**: jsou uvedeny tyto produkty, který patří do kategorie nápoje ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image51.png))


S **GetProductsByCategoryID (*categoryID*)** metoda v našich DAL, teď můžeme vytvořit stránku ASP.NET, která se zobrazí jenom produkty v určené kategorii. Následující příklad ukazuje všechny produkty, které jsou do kategorie Nápoje, které mají **CategoryID** 1.

Beverages.ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Se zobrazují tyto produkty do kategorie Nápoje](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Obrázek 20**: se zobrazují tyto produkty do kategorie nápoje ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Krok 4: Vložení, aktualizace a odstranění dat

Existují dva způsoby, které se běžně používá pro vkládání, aktualizaci a odstraňování dat. První vzor, který budu jim říkat přímé model databáze, zahrnuje vytvoření metody, že při vyvolání, problém **vložit**, **aktualizace**, nebo **odstranit** příkaz databáze, která funguje na záznam v jedné databázi. Tyto metody jsou obvykle předány v řadě skalárních hodnot (celá čísla, řetězce, logické hodnoty, data a času a tak dále), které odpovídají hodnotám vložit, aktualizovat nebo odstranit. Například s tímto modelem stačí pro **produkty** tabulky metodu delete padl parametr celé číslo, určující **ProductID** z záznam, který chcete odstranit, zatímco metoda vložit padl řetězec **ProductName**, decimal pro **UnitPrice**, celé číslo pro **UnitsOnStock**, a tak dále.


[![Každý Insert, Update a Delete žádosti je odeslána databáze hned](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Obrázek 21**: každý vložení, aktualizace a odstranění žádosti je odeslána do databáze okamžitě ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image57.png))


Další vzor, který jsem budete odkazovat dávkové aktualizaci vzor, je aktualizovat celé datové sady, datové tabulky nebo kolekce DataRows v jedné metody volání. S tímto modelem stačí vývojář odstraní, vloží a změní DataRows v datové tabulce a pak předá do metodu aktualizace těchto DataRows ani objekt DataTable. Tato metoda pak vytvoří výčet DataRows předaný, určuje, zda jsou jsme se změnily, přidat nebo odstranit (prostřednictvím objekt DataRow [RowState vlastnost](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) hodnota) a vydá požadavek příslušnou databázi pro každý záznam.


[![Všechny změny jsou synchronizovány s databází při vyvolání metody Update](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Obrázek 22**: všechny změny jsou synchronizovány s databází při vyvolání metody Update ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter používá vzor aktualizace služby batch ve výchozím nastavení, ale také podporuje model s přímým přístupem DB. Protože jsme vybrali možnost "příkazy Generovat Insert, Update a Delete" Upřesnit vlastnosti při vytváření naší třídy TableAdapter **ProductsTableAdapter** obsahuje **Update()** metody která implementuje vzor aktualizace služby batch. Konkrétně obsahuje TableAdapter **Update()** metodu, která je možné předat typované datové sady, datové tabulky silného typu nebo jeden nebo více DataRows. Pokud jste nechali zaškrtnuté políčko "GenerateDBDirectMethods" při prvním vytvoření objektu TableAdapter s přímým přístupem vzor DB budou prováděny také prostřednictvím **Insert()**, **Update()**, a **Delete()**  metody.

Oba vzorky úpravy dat pomocí objektu TableAdapter **událost InsertCommand**, **událost UpdateCommand**, a **událost DeleteCommand** vlastnosti vydat jejich **vložit** , **Aktualizace**, a **odstranit** příkazů do databáze. Můžete zkontrolovat a upravit **událost InsertCommand**, **událost UpdateCommand**, a **událost DeleteCommand** vlastnosti kliknutím na TableAdapter v návrháři datových sad a pak v okně Vlastnosti. (Ujistěte se, že jste vybrali TableAdapter a že **ProductsTableAdapter** objektu je vybrali v rozevíracím seznamu v okně Vlastnosti.)


[![TableAdapter má událost InsertCommand událost UpdateCommand a událost DeleteCommand vlastnosti](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Obrázek 23**: The TableAdapter nemá **událost InsertCommand**, **událost UpdateCommand**, a **událost DeleteCommand** vlastnosti ([klikněte na zobrazit reklamy Obrázek](creating-a-data-access-layer-cs/_static/image63.png))


Pokud chcete zkontrolovat nebo změnit libovolné z těchto vlastností příkazu databáze, klikněte na **CommandText** dílčí vlastnosti, které se otevře Tvůrce dotazů.


[![Konfigurace INSERT, UPDATE a DELETE příkazy v editoru dotazů](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Obrázek 24**: konfigurace **vložit**, **aktualizace**, a **odstranit** příkazy v editoru dotazů ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image66.png))


Následující příklad kódu ukazuje, jak dvakrát cena všechny produkty, které nejsou není podporován a, které mají 25 jednotek v zásobách nebo i rychleji pomocí vzoru aktualizace služby batch:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Následující kód ukazuje, jak pomocí vzoru s přímým přístupem DB prostřednictvím kódu programu odstranit konkrétní produkt a potom aktualizovat jeden a pak přidejte nový:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Vytvoření vlastní Insert, Update a Delete metody

**Insert()**, **Update()**, a **Delete()** metody vytvořené DB přímé metody mohou být o něco náročnější, zejména u tabulek s mnoha sloupci. Hledání v předcházejícím příkladu, bez pomoci IntelliSense pro není zejména jasné, co **produkty** sloupec tabulky mapuje pro každý vstupní parametr **Update()** a **Insert()**  metody. Může nastat situace, kdy chceme jenom aktualizace jednoho sloupce nebo dvě, nebo má vlastní **Insert()** metodu, která bude možná, návratová hodnota nově vložený záznam **IDENTITY** (automatické zvyšování čísla) pole.

Chcete-li vytvořit vlastní metodu, vraťte se na návrháři datových sad. Klikněte pravým tlačítkem na TableAdapter a zvolte Přidat dotaz vrací do Průvodce vytvořením objektu TableAdapter. Na druhé obrazovce mohou Udáváme typu dotazu k vytvoření. Vytvoříme metodu, která přidá nový produkt a vrátí hodnotu nově přidaný záznam **ProductID**. Proto se rozhodnout vytvořit **vložit** dotazu.


[![Vytvoření metody přidání nový řádek do tabulky produktů](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Obrázek 25**: vytvoření metody přidání na nový řádek pro **produkty** tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image69.png))


Na další obrazovce **událost InsertCommand**společnosti **CommandText** se zobrazí. Tento dotaz rozšířit přidáním **vyberte rozsah\_IDENTITY()** na konec dotazu, která vrátí poslední hodnotu identity, které jsou vloženy do **IDENTITY** sloupce ve stejném oboru. (Najdete v článku [technickou dokumentaci](https://msdn.microsoft.com/library/ms190315.aspx) Další informace o **oboru\_IDENTITY()** a proč budete pravděpodobně chtít [použít OBOR\_IDENTITY() náhrada @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Zajistěte, že jste **vložit** příkaz středníkem před přidáním **vyberte** příkazu.


[![Upravte dotaz, který vrací hodnotu SCOPE_IDENTITY()](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Obrázek 26**: Upravte dotaz pro návrat **oboru\_IDENTITY()** hodnotu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image72.png))


A konečně, pojmenujte novou metodu **InsertProduct**.


[![Nastavte název nové metody InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Obrázek 27**: nastavte název nové metody **InsertProduct** ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image75.png))


Po návratu do návrháře DataSet uvidíte, že **ProductsTableAdapter** obsahuje novou metodu **InsertProduct**. Pokud tato nová metoda nemá parametr pro každý sloupec v **produkty** tabulky, je pravděpodobné jste zapomněli ukončení **vložit** příkaz oddělíte středníkem. Konfigurace **InsertProduct** metoda a ujistěte se, máte středníkem omezující **vložit** a **vyberte** příkazy.

Ve výchozím nastavení vložte metody problém není dotazem metody, což znamená, že vrátí počet ovlivněných řádků. Chceme, ale **InsertProduct** metoda k vrácení hodnoty vrácené dotazem, ne počet ovlivněných řádků. Chcete-li to provést, upravte **InsertProduct** metody **ExecuteMode** vlastnost **skalární**.


[![Změňte vlastnost ExecuteMode na skalár](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Obrázek 28**: Změna **ExecuteMode** vlastnost **skalární** ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image78.png))


Následující kód ukazuje tento nový **InsertProduct** metodu v akci:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Krok 5: Dokončení vrstvy přístupu k datům

Všimněte si, že **ProductsTableAdapters** třídy vrátí **CategoryID** a **KódDodavatele** hodnoty z **produkty** tabulky, ale neobsahuje **CategoryName** sloupec z **kategorie** tabulky nebo **CompanyName** sloupec z **Dodavatelé**tabulky, přestože jde pravděpodobně o sloupce chceme zobrazit při zobrazování informací o produktu. Můžeme rozšířit počáteční metody objektu TableAdapter **GetProducts()**, oba **CategoryName** a **CompanyName** hodnoty sloupce, které se aktualizuje silného typu objektu DataTable zahrnout tyto nové sloupce.

Může to znamenat problém, ale jako metody objektu TableAdapter pro vkládání, aktualizací a odstraňování dat vychází z této metody počáteční. Naštěstí automaticky generované metody pro vkládání, aktualizaci a odstraňování nejsou ovlivněny poddotazy v **vyberte** klauzuli. Protože se stará přidáte náš dotazy na **kategorie** a **Dodavatelé** jako poddotazy, spíše než **připojte se k** s, můžeme vám vyhnout se tak nutnosti přepracovat tyto metody pro úpravu dat. Klikněte pravým tlačítkem na **GetProducts()** metodu **ProductsTableAdapter** a zvolením možnosti konfigurovat. Potom upravte **vyberte** klauzule tak, že vypadají, jako jsou:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Aktualizace příkazu SELECT pro GetProducts() – metoda](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Obrázek 29**: aktualizace **vyberte** příkaz **GetProducts()** – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image81.png))


Po aktualizaci **GetProducts()** metoda se má použít tento nový dotaz objekt DataTable bude obsahovat dva nové sloupce: **CategoryName** a **dodavatel**.


![Objekt DataTable produkty má dva nové sloupce](creating-a-data-access-layer-cs/_static/image82.png)

**Obrázek 30**: **produkty** DataTable má dva nové sloupce


Za chvíli se aktualizovat **vyberte** klauzuli v **GetProductsByCategoryID (*categoryID*)** také metody.

Pokud aktualizujete **GetProducts()** **vyberte** pomocí **připojení** syntaxe Návrhář DataSet nebudou moct automaticky vygenerovat metody pro vkládání, aktualizaci a odstraňování použití modelu s přímým přístupem DB dat z databáze. Místo toho budete muset ručně vytvořit je podobně jako jsme to udělali s **InsertProduct** metoda dříve v tomto kurzu. Kromě toho ručně musíte poskytnout **událost InsertCommand**, **událost UpdateCommand**, a **událost DeleteCommand** hodnoty vlastnosti, pokud chcete používat službu batch, aktualizuje se vzor.

## <a name="adding-the-remaining-tableadapters"></a>Přidání zbývající objekty TableAdapter

Až dosud podívali jsme se jenom na práci s jeden TableAdapter pro izolovanou databázi tabulku. Databáze Northwind však obsahuje několik související tabulky, které budeme potřebovat pro práci s naší webové aplikaci. Typované datové sady mohou obsahovat více souvisejících DataTables. Proto k dokončení našich DAL potřebujeme přidat DataTables pro tabulky, které budeme používat v těchto kurzech. Přidání nového TableAdapter k datové sadě zadán, otevřete návrhář datových sad, klikněte pravým tlačítkem myši v návrháři a zvolte Přidat / TableAdapter. To vytvoří nový objekt DataTable a TableAdapter a průvodce, který jsme se zaměřili dříve v tomto kurzu vás provedou.

Trvat pár minut vytvořit následující objekty TableAdapter a metod pomocí následující dotazy. Všimněte si, že dotazy v **ProductsTableAdapter** zahrnují poddotazy a zkopírujte názvy kategorií a dodavatele každého produktu. Kromě toho, pokud jste postupovali podle, jste už přidali **ProductsTableAdapter** třídy **GetProducts()** a **GetProductsByCategoryID (*ID kategorie* )** metody.

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


[![Návrhář DataSet po přidání čtyři objekty TableAdapter](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Obrázek 31**: The datovou sadu návrháře po the čtyři objekty TableAdapter jsou přidané ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Přidání vlastního kódu vrstvy Dal

Objekty TableAdapter a přidat k datové sadě zadán DataTables jsou vyjádřeny jako soubor XML Schema Definition (**Northwind.xsd**). Tyto informace schématu můžete zobrazit kliknutím pravým tlačítkem na **Northwind.xsd** souboru v Průzkumníku řešení a zvolíte zobrazení kódu.


[![Soubor definice (XSD) schématu XML pro Lhota zadaná datová sada](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Obrázek 32**: soubor definice schématu XML (XSD) pro typová Lhota ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image88.png))


Tyto informace schématu je přeložit do kódu jazyka C# nebo Visual Basic v době návrhu při kompilaci nebo za běhu (v případě potřeby), v tomto okamžiku můžete krokovat přes něj s ladicím programem. Chcete-li zobrazit tento automaticky generovaný kód přejděte na zobrazení tříd a zotavení po havárii na třídy TableAdapter nebo typované datové sady. Pokud nevidíte zobrazení tříd na obrazovce, přejděte do nabídky zobrazení a vyberte ho z něj nebo stiskněte kombinaci kláves Ctrl + Shift + C. V zobrazení tříd můžete zobrazit vlastnosti, metody a události tříd typované datové sady a TableAdapter. Chcete-li zobrazit kód pro konkrétní metody, dvakrát klikněte na název metody v zobrazení tříd nebo klikněte pravým tlačítkem myši na něj a možnost Přejít na definici.


![Prozkoumejte automaticky generovaný kód tak, že vyberete přejít k definici zobrazení tříd](creating-a-data-access-layer-cs/_static/image89.png)

**Obrázek 33**: Zkontrolujte automaticky generovaný kód tak, že vyberete přejít k definici zobrazení tříd


Automaticky generovaný kód mohou být spoustu skvělých času, kód je často velmi obecná a je třeba přizpůsobit podle jedinečných potřeb aplikace. Riziko rozšíření automaticky generovaný kód, ale je, že nástroj, který vygeneruje kód rozhodnout, že je čas na "obnovit" a přepsat vaše vlastní nastavení. Pomocí rozhraní .NET 2.0 nový koncept částečné třídy je snadné rozdělení třídy do více souborů. Umožňuje nám to přidat vlastní metody, vlastnosti a události na automaticky generované třídy bez nutnosti starat o přepsání naše přizpůsobení sady Visual Studio.

Abychom si předvedli, jak přizpůsobit DAL, přidáme **GetProducts()** metodu **SuppliersRow** třídy. **SuppliersRow** třída představuje jeden záznam v **Dodavatelé** tabulky; každý poskytovatel může dodavatele nula řada produktů, tak **GetProducts()** ty vrátí produkty zadané dodavatele. Provedete to vytvořte nový soubor třídy v **aplikace\_kód** složku s názvem **SuppliersRow.cs** a přidejte následující kód:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Tato částečná třída instruuje kompilátor, že sestavení **Northwind.SuppliersRow** třídy, aby obsahoval **GetProducts()** metoda jsme definovali. Pokud svůj projekt sestavit a pak se vraťte k zobrazení tříd uvidíte **GetProducts()** teď uvedená jako metoda **Northwind.SuppliersRow**.


![Metoda GetProducts() je teď součástí Northwind.SuppliersRow třídy](creating-a-data-access-layer-cs/_static/image90.png)

**Obrázek 34**: **GetProducts()** metoda je nyní součástí **Northwind.SuppliersRow** třídy


**GetProducts()** metoda je nyní možné výčet sadu produktů pro konkrétní dodavatele, jak ukazuje následující kód:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Tato data lze také zobrazit v libovolném prostředí ASP. Data společnosti NET webové ovládací prvky. Na následující stránce používá ovládací prvek GridView s dvě pole:

- Vlastnost BoundField, která zobrazuje název každého dodavatele a
- TemplateField obsahuje BulletedList ovládací prvek, který je vázán na výsledky vrácené **GetProducts()** metodu pro každého dodavatele.

Prozkoumáme jak zobrazit tyto hlavní podrobnosti zprávy v budoucích kurzech. Prozatím se v tomto příkladu je navržená pro ilustraci pomocí vlastní metody přidat do **Northwind.SuppliersRow** třídy.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Název společnosti dodavatele je uvedena ve sloupci vlevo, jejich produkty vpravo](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Obrázek 35**: název společnosti dodavatele je uvedena ve sloupci vlevo, jejich produktů v pravém ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Souhrn

Při vytváření webové aplikace vytváří DAL by měl být jeden z první krůčky, tím, než začnete vytvářet prezentační vrstvy. Pomocí sady Visual Studio vytvoření DAL podle zadané datové sady je úkol, který lze provést v 10 až 15 minut bez nutnosti psaní jediného řádku kódu. Kurzy v budoucnu budou vycházejí z této vrstvy DAL. V [další kurz](creating-a-business-logic-layer-cs.md) vytvoříme určit řadu obchodních pravidel a zjistit, jak je implementovat v samostatné vrstvy obchodní logiky.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vytváření DAL pomocí silně typované objekty TableAdapter a DataTables ve VS 2005 a technologii ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Navrhování komponenty datové vrstvy a předávání dat prostřednictvím vrstev](https://msdn.microsoft.com/library/ms978496.aspx)
- [Vytvoření vrstvy přístupu k datům s návrháři datové sady Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Informace o konfiguraci v technologii ASP.NET 2.0 šifrování aplikací](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter – přehled](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Práce s typové datové sady](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Pomocí přístupu k datům silného typu v sadě Visual Studio 2005 a technologií ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Postup rozšíření metody třídy TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Načítání dat skalární z uložené procedury](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video od Pluralsightu na témata, které jsou obsažené v tomto kurzu

- [Vrstvy přístupu k datům v aplikacích ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Postup ruční svázání datové sady s mřížkou DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Postup práce s datovými sadami a filtry z aplikace ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Ron zelená, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez a Santos Carlose. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](creating-a-business-logic-layer-cs.md)
