---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Nasazení databází (C#) | Microsoft Docs
author: rick-anderson
description: Nasazení webové aplikace ASP.NET zahrnuje získávání potřebné soubory a prostředky z vývojového prostředí do produkčního prostředí. Pro da...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 203bf64da887f31e5f0727fc57173d6a573095da
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="deploying-a-database-c"></a>Nasazení databází (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Nasazení webové aplikace ASP.NET zahrnuje získávání potřebné soubory a prostředky z vývojového prostředí do produkčního prostředí. Pro datové webové aplikace zahrnují schéma databáze a data. V tomto kurzu je první série, která jsou zde popsány kroky potřebné k úspěšnému nasazení databáze z vývojového prostředí do produkčního prostředí.


## <a name="introduction"></a>Úvod

Nasazení webové aplikace ASP.NET zahrnuje získávání potřebné soubory a prostředky z vývojového prostředí do produkčního prostředí. V průběhu posledních šest kurzy jsme se podívali na nasazení jednoduchou webovou aplikaci recenze adresáře. Tento ukázkový web byl skládá z řady serverové prostředky - stránek ASP.NET, konfiguračních souborů, `Web.sitemap` souboru a tak dále – spolu s prostředky na straně klienta, jako jsou bitové kopie a souborů CSS. Ale co o řízené daty webové aplikace? Jaké navíc je třeba učinit nasazení webové aplikace, která používá databázi?

Přes další kurzy několik nevyřešíme kroky potřebné k nasazení webových aplikací. V tomto kurzu spustí prověřením jak získat s schéma databáze a obsah z vývojového prostředí do produkčního prostředí, při následné kurzu zjistí změny potřebné konfigurace. Následující, že jsme budete prozkoumat problémy nasazení databáze, která používá aplikačních služeb (členství, role, profil a tak dále).

## <a name="examining-the-updated-book-reviews-web-application"></a>Zkoumání aktualizované kniha recenze webové aplikace

Aby bylo možné prokázat nasazení řízené daty webové aplikace, I sunout aktualizovat kniha recenze webové aplikace z webu jednoduchý, statické do dalšího řízené daty. Jak před, existují dvě verze aplikace v tomto kurzu s ke stažení: jeden, který používá model projektu webové aplikace a ten, který používá model webový projekt.

Aktualizovaného recenzemi kniha webové aplikace používá [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) databáze, která je uložena v lokalitě s `App_Data` složky (`~/App_Data/Reviews.mdf`). Pokud máte v počítači nainstalovaný SQL Server 2008 měli spustit ukázku bez chyby. Pokud máte starší verzi systému SQL Server, můžete buď nainstalovat volné SQL Server 2008 Express Edition nebo můžete použít databázi skripty, které jsou k dispozici v tomto kurzu s stáhnout k vytvoření databáze.

`Reviews.mdf` Databáze obsahuje čtyři tabulky:

- `Genres` -obsahuje záznam pro každý genre, jako je technologie, Fiction a Business.
- `Books` -obsahuje záznam pro každý revize se sloupci jako `Title`, `GenreId`, `ReviewDate`, a `Review`, mimo jiné.
- `Authors` -obsahuje informace o jednotlivých autora, který má na zkontrolovat kniha podílí.
- `BooksAuthors` -m: n tabulku spojení, která určuje, jaké Autoři mají zapisovat jaké knihy.
  

Obrázek 1 zobrazuje diagramu ER tyto čtyři tabulek.


[![Kniha recenze webové aplikace s databáze se skládá ze čtyř tabulek](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Obrázek 1**: seznamu recenze webové aplikace s databáze se skládá ze čtyř tabulky ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image3.jpg))


Předchozí verze recenze adresáře webu bylo samostatné stránce ASP.NET pro každý seznam. Například se stránku s názvem `~/Tech/TYASP35.aspx` které obsažené revize pro *naučit sami technologie ASP.NET 3.5 za 24 hodin*. Tuto novou verzi datové webu má recenze uložené v databázi a jednu stránku ASP.NET, Review.aspx?ID=*bookId*, zobrazuje revize pro zadanou runbook. Podobně je Genre.aspx?ID=*genreId* stránky, která uvádí zkontrolovat knihy v zadané genre.

Obrázky 2 a 3 zobrazit `Genre.aspx` a `Review.aspx` stránky v akci. Poznamenejte si adresu URL na panelu Adresa pro jednotlivé stránky. V obrázku 2 it s Genre.aspx? ID = c 85d164ba-1123-4 47-82a0-c8ec75de7e0e. Protože je 85d164ba-1123-4c47-82a0-c8ec75de7e0e `GenreId` recenze v lokalitě, které spadají pod tuto genre zobrazí hodnotu pro technologii genre, čtení nadpis stránky s "Technologie zkontroluje" a seznamu s odrážkami.


[![Stránka Genre technologie](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Obrázek 2**: technologie Genre stránky ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image6.jpg))


[![Kontrola pro sami naučit ASP.NET 3.5 za 24 hodin](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Obrázek 3**: revize pro *naučit sami technologie ASP.NET 3.5 za 24 hodin* ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image9.jpg))


Webové aplikace zkontroluje kniha obsahuje oddílu Správa, kde správci můžete přidat, upravit a odstranit žánry, recenze a vytvářet informace. V současné době návštěvník mají přístup k části Správa. V budoucích kurzu jsme budete přidat podporu pro uživatelské účty a pouze povolit oprávněným uživatelům do stránek pro správu.

Pokud si stáhnout aplikaci recenze adresáře prosím mějte na paměti, které jejím účelem je k předvedení nasazení aplikace řízené daty. Osvědčené postupy, pokud je to návrh aplikace nebude provedena. Není třeba žádné samostatné Data přístup Layer (DAL); stránky ASP.NET komunikovat přímo s databází prostřednictvím ovládacího prvku SqlDataSource nebo kód ADO.NET v jejich třídy kódu na pozadí. Podrobnější podívejte se na vytvoření datové aplikace pomocí vrstvené architektury, najdete v části Moje [ *práci s daty* kurzy](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Databáze na vývoj a výroby

Když spustíte vývoj datové webové aplikace je nutné zadat připojovací řetězec databáze, který obsahuje podrobnosti o aplikaci pro připojení k databázi. Tento připojovací řetězec Určuje mezi jiných věcí, databázový server, název databáze a informace o zabezpečení. Nejčastěji je jiné než databázi použít, když databáze používané aplikací během vývoje ho s v produkčním prostředí. Existuje mnoho výhod používání různých databází pro vývoj a produkční. Má jiné databáze v vývoj znamená ochranu t nutné se obávat omylem Neupravujte ani neodstraňuje dynamická data. To také umožňuje umístit do fiktivní testovací data nebo provést zásadní změny do datového modelu bez nutnosti starat o dopadech na aplikaci v produkčním prostředí. Nevýhodou, že do jiné databáze v prostředí vývoje a produkční je, že když je aplikace nasazena databáze a všechny příslušné změny schématu databáze s nebo data musí být také nasazen.

Před prvním nasazení existuje pouze jedna instance databáze a tato instance je ve vývojovém prostředí. Při nasazení aplikace do produkčního prostředí poprvé jsme musí nejen zkopírujte si potřebné soubory na straně serveru a na straně klienta, ale také zkopírovat databázi z vývojového prostředí do produkčního prostředí. Toto je, kde jsme stát po pravé teď s webovou aplikací kniha recenze - se databáze nachází v `App_Data` složka v našem vývojového prostředí ale obsahuje není ještě se nabídne do produkčního prostředí.

Jakmile je aplikace nasazená existují dvě kopie databáze. Během existence aplikace lze přidat nové funkce, očekávání změnu do datového modelu (jako je například přidávání nových sloupců do existující tabulky, provedení změn na existující sloupce, přidání nové tabulky a tak dále). Pokud další nasazení webové aplikace, budou změny použity k databázi ve vývojovém prostředí od posledního nasazení je nutné použít na produkční databázi. Některé postupy při správě tohoto procesu jsou popsané v budoucnu kurzu. Tento kurz se zaměřuje na nasazení celou databázi z vývojového prostředí do produkčního prostředí.

## <a name="deploying-the-database-to-the-production-environment"></a>Nasazení databáze do produkčního prostředí

Zbývající část tohoto kurzu vypadá na tom, jak nasadit databázi z vývojového prostředí do produkčního prostředí. Použijete-li společně je nutné zajistit, že váš účet s poskytovatelem webového hostitele obsahuje Microsoft SQL Server databáze podporovat. Musíte také určité informace, a to název databázového serveru, název databáze a uživatelské jméno a heslo pro připojení k databázi.

Jak jsme uvedli dříve v tomto kurzu, databáze webu s recenze adresáře je uložené v databázi SQL Server 2008 Express Edition `App_Data` složky. Bude spadat do důvodu nasazení těchto databází by být co nejjednodušší kopírování `App_Data` složky z vývojového prostředí do produkčního prostředí. Většina webové hostitele zprostředkovatele však nepodporují hostování databází v `App_Data` složky z důvodu bezpečnosti. Weboví hostitelé místo toho zadejte účet na serveru databáze systému SQL Server v jejich prostředí. Nasazení databáze z vývojového prostředí do produkčního prostředí vyžaduje získávání databáze registrovaný na webovém serveru databáze s hostiteli.

Jak můžete získat databáze z vývojového prostředí do produkčního prostředí? Existuje několik způsobů k tomu v závislosti na tom, co vaše nabízí hostitele webové služby. Pomocí některé hostitele, jako je například DiscountASP.NET, můžete FTP zálohu databáze nebo skutečnou `.mdf` souboru na vašem webu a pak z ovládacích panelů, obnovit záložní soubor nebo připojení `.mdf` souboru do databáze serveru SQL Server. Tyto nástroje pro nasazení databáze je jednoduché, zkopírování `App_Data` složku do produkčního prostředí a připojíte ho v Ovládacích panelech. Toto je možná nejjednodušší a nejrychlejší způsob, jak při prvním publikování databáze.

Jiná možnost je použít v Průvodci publikování databáze. V Průvodci publikování databáze je aplikace plochy Windows, která způsobí vygenerování příkazů SQL pro vytvoření s schéma databáze - tabulky, uložené procedury, zobrazení, uživatelem definované funkce a tak dále - a volitelně data v její tabulky. Můžete pak připojit k webové s zprostředkovatele hostitele databáze serveru SQL Server Management Studio a potom spusťte tento skript duplicitní databázi na produkční. Ještě lepší, pokud váš poskytovatel hostitele webu podporuje Microsoft s [publikování služby databázového](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) můžete mít skript generované v Průvodci publikováním databáze automaticky spustí na serveru databáze vaším jménem. Vzhledem k tomu, že v Průvodci publikováním databáze generuje skript, který vytvoří databáze s schéma a data, bude fungovat bez ohledu na to, zda poskytovatel webového hostitele nabízí funkcí, jako je připojení nahrané `.mdf` souboru.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Vytváření příkazů SQL pro vytvoření databáze schéma a Data pomocí Průvodce publikováním databáze

Umožní s provede pomocí Průvodce publikováním databáze pro nasazení databáze recenze adresáře do produkčního prostředí. Pokud používáte Visual Studio 2008 nebo nad rámec, v Průvodci publikování databáze je již nainstalován. Pokud používáte Visual Studio 2005, bude nutné první [stáhněte a nainstalujte](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) průvodce.

Otevřete Visual Studio a přejděte do `Reviews.mdf` databáze. Pokud používáte Visual Web Developer, přejděte do Průzkumníka databáze; Pokud používáte Visual Studio, použijte Průzkumníka serveru. Obrázek 4 ukazuje `Reviews.mdf` databázi v Průzkumníku databáze v aplikaci Visual Web Developer. Jak ukazuje obrázek 4, `Reviews.mdf` databáze se skládá ze čtyř tabulek, tři uložené procedury a uživatelem definované funkce.


[![Najít databázi v Průzkumníku databáze nebo v Průzkumníku serveru](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Obrázek 4**: najít databázi v Průzkumníku databáze nebo v Průzkumníku serveru ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image12.jpg))


Klikněte pravým tlačítkem na název databáze a vyberte možnost "Publikování poskytovateli" v místní nabídce. Spustí se Průvodce publikováním databáze (viz obrázek 5). Na tlačítko Další zálohy po úvodní obrazovka.


[![Úvodní obrazovka Průvodce publikováním databáze](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Obrázek 5**: databázi publikování průvodce úvodní obrazovka ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image15.jpg))


Druhý obrazovky v Průvodci uvádí databáze, které jsou přístupné pro Průvodce publikováním databáze a umožňuje zvolit, jestli se má skript všechny objekty v databázi vybraného nebo vybrat, které objekty se mají skriptu. Vyberte příslušnou databázi a ponechat možnost "Skriptu všechny objekty v databázi vybraného" zaškrtnutí.

> [!NOTE]
> Pokud se zobrazí chyba "nejsou žádné objekty v databázi *databaseName* typů Skriptovatelná tímto průvodcem" při kliknutí na další obrazovce vidíte na obrázku 6, ujistěte se, že cesta k souboru databáze není příliš dlouhý. Bylo zjištěno, že tuto chybu mohou nastat, pokud cesta k souboru databáze je příliš dlouhá.


[![Úvodní obrazovka Průvodce publikováním databáze](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Obrázek 6**: databázi publikování průvodce úvodní obrazovka ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image18.jpg))


Na další obrazovce můžete generovat soubor skriptu nebo, pokud váš webový hostitel podporuje, publikování databáze přímo na serveru databáze webového hostitele zprostředkovatele s. Jak je vidět na obrázku 7, dochází k zápisu do souboru skriptu `C:\REVIEWS.MDF.sql`.


[![Databáze do souboru skriptu nebo ji publikovat přímo na váš poskytovatel hostitele webu](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Obrázek 7**: databázi do souboru skriptu nebo ji publikovat přímo na váš poskytovatel hostitele webu ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image21.jpg))


Na další obrazovce zobrazí výzvu pro různé možnosti skriptování. Můžete určit, zda skript by měl obsahovat příkazy drop odebrat tyto stávajících objektů. Toto výchozí nastavení na hodnotu True, která je vhodná, při prvním nasazení databáze. Můžete také určit, zda je cílová databáze systému SQL Server 2000, SQL Server 2005 nebo SQL Server 2008. Nakonec můžete určit, jestli skript schéma a data, pouze data, nebo jen schéma. Schéma je kolekce databázové objekty, tabulky, uložené procedury, zobrazení a tak dále. Data jsou informace, které se nacházejí v tabulkách.

Jak ukazuje obrázek 8, I tu Průvodce nakonfigurovat tak, aby vyřadit stávající databázové objekty, ke generování skriptu pro databázi systému SQL Server 2008 a publikovat schéma a data.


[![Zadejte publikování možnosti](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Obrázek 8**: Zadejte možnosti publikování ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image24.jpg))


Poslední dvou obrazovkách shrnují akce, které se chystáte se provedou a následně se zobrazí stav skriptování. Spuštění Průvodce net výsledkem je, že obsahuje soubor skriptu, který obsahuje příkazy SQL, které jsou potřebné k vytvoření databáze na produkční a jeho naplnění stejná data jako na vývoj.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Provádění příkazů SQL v databázi produkční prostředí

Teď, když máme je skript, který obsahuje příkazy SQL k vytvoření databáze a jeho data všechny zbývající se spustit skript na produkční databázi. Někteří poskytovatelé webového hostitele nabízejí textové pole v jejich ovládacích panelech, kde můžete zadat příkazů SQL pro spuštění ve vaší databázi. Pokud máte soubor velké skriptu, pak tato možnost nemusí fungovat ( `REVIEWS.MDF.sql` souboru skriptu pro instanci je velikost, než 425 KB).

Lepší přístup je pro připojení přímo na provozním serveru databázi pomocí SQL Server Management Studio (SSMS). Pokud máte jiný - Express edici systému SQL Server v počítači nainstalována pravděpodobně již máte nainstalované aplikace SSMS. Jinak můžete [stáhněte a nainstalujte](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) bezplatnou kopii sady SQL Server Management Studio Express Edition.

Spusťte aplikaci SSMS a připojte se k vašemu webovému hostiteli s databáze serveru pomocí informací uvedených ve zprostředkovateli webového hostitele.


[![Připojení k serveru databáze webového hostitele zprostředkovatele s](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Obrázek 9**: připojení k váš poskytovatel hostitele webu s databázový Server ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image27.jpg))


Rozbalte položku na kartě databáze a vyhledejte vaší databáze. Klikněte na tlačítko Nový dotaz v levém horním rohu panelu nástrojů, vložte v příkazech SQL ze souboru skriptu, který je vytvořený pomocí Průvodce publikování databáze a klikněte na tlačítko Spustit a spusťte tyto příkazy na provozním serveru databáze. Pokud je soubor skriptu zejména velký může trvat několik minut spuštěním příkazů.


[![Připojení k serveru databáze webového hostitele zprostředkovatele s](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Obrázek 10**: připojení k váš poskytovatel hostitele webu s databázový Server ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image30.jpg))


Že všechny existuje s je k němu! Vývoj pro databáze v tomto okamžiku je duplicitní do produkčního prostředí. Pokud obnovíte databázi v aplikaci SSMS byste měli vidět nové databázové objekty. Obrázek 11 zobrazuje provozní databáze s tabulkami, uložené procedury a uživatelem definované funkce, které zrcadlí na databázi vývoj. A protože jsme pokynů v Průvodci publikováním databáze publikovat data, tabulky produkční databáze s mít stejná data jako tabulky s vývoj databáze v době, kdy byl proveden v průvodci. Obrázek 12 znázorňuje data v `Books` tabulky na produkční databázi.


[![Databázové objekty mají duplicitní na provozní databáze](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Obrázek 11**: databázi objekty mají duplicitní na provozní databázi ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image33.jpg))


[![Provozní databáze obsahuje stejná Data jako v databázi vývoj](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Obrázek 12**: provozní databáze obsahuje stejná Data jako na vývoj databázi ([Kliknutím zobrazit obrázek v plné velikosti](deploying-a-database-cs/_static/image36.jpg))


V tuto chvíli jsme nasadili jenom databázi vývoj do produkčního prostředí. Ještě nám zvážení nasazení vlastní webové aplikace nebo zkontrolován, jaké změny v konfiguraci jsou potřeba pro aplikace na produkční použití produkční databázi. V dalším kurzu jsme zaměříme tyto problémy!

## <a name="summary"></a>Souhrn

Nasazení webových aplikací vyžaduje kopírování databáze použitý během vývoje do produkčního prostředí. Mnoho poskytovatelů webového hostitele nabízí nástroje pro zjednodušit proces nasazení databáze. Například můžete s DiscountASP.NET FTP databáze `.mdf` souboru (nebo zálohy) a pak připojte databázi k serveru databáze z ovládacích panelů. Jinou možnost, která funguje bez ohledu na to, jaké funkce nabízí zprostředkovatele hostitele vaší webové je společnosti Microsoft s Průvodce publikováním databáze nástroj, který generuje skript SQL příkazů pro vytvoření vývoj databáze s schéma a data. Po vygenerování tento skript můžete ji spustit na produkční databázi.

Teď, když je databáze s kniha recenze webové aplikace na produkční jsme můžete nasadit aplikaci. Ale informace o konfiguraci webové aplikace s Určuje připojovací řetězec k databázi a databázi vývoj odkazuje tento připojovací řetězec. Je potřeba aktualizovat informace o této připojovacím řetězci, pokud nasazení webu do produkčního prostředí. V dalším kurzu porovná tyto rozdíly konfigurace a provede kroky potřebnými k publikování webu datové adresáře recenze do produkčního prostředí.

Radostí programování!

#### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Stáhnout průvodce 1.1 publikováním databáze Microsoft SQL Server](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Stáhněte si Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Předchozí](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [další](configuring-the-production-web-application-to-use-the-production-database-cs.md)
