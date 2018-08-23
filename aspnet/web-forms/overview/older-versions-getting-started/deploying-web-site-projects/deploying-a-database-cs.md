---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Nasazení databáze (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Nasazení webové aplikace ASP.NET zahrnuje získání potřebné soubory a prostředky z vývojového prostředí do produkčního prostředí. Pro da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: ca9ce2b41cfd10504304c30bc965e446a7188120
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756662"
---
<a name="deploying-a-database-c"></a>Nasazení databáze (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Nasazení webové aplikace ASP.NET zahrnuje získání potřebné soubory a prostředky z vývojového prostředí do produkčního prostředí. Pro datově řízených webových aplikací to zahrnuje databázové schéma a data. Tento kurz je první z řady, který popisuje kroky potřebné k úspěšnému nasazení do produkčního prostředí databázi z vývojového prostředí.


## <a name="introduction"></a>Úvod

Nasazení webové aplikace ASP.NET zahrnuje získání potřebné soubory a prostředky z vývojového prostředí do produkčního prostředí. V průběhu posledních šesti kurzů jsme se podívali na nasazení jednoduché webové aplikace recenzí. Tento ukázkový web se skládá z celou řadu materiálů na straně serveru – stránek ASP.NET, konfigurační soubory `Web.sitemap` souboru a tak dále - spolu s prostředky na straně klienta, jako jsou obrázky a soubory šablon stylů CSS. Ale co datově řízených webových aplikací? Jaké další kroky, třeba k nasazení webové aplikace, která používá databázi?

Za další kurzy několik jsme bude zabývat kroky potřebné k nasazení aplikace web s daty. V tomto kurzu začíná tím, že kontroluje v tom, jak s schéma databáze a obsah z vývojového prostředí do produkčního prostředí, zatímco následujícím kurzu zkoumá změny potřebné konfigurace. Následující prozkoumáme obtíže spojené s nasazením databázi, která používá aplikační služby (členství, role, profil a tak dále).

## <a name="examining-the-updated-book-reviews-web-application"></a>Zkoumání aktualizované knihy revize webové aplikace

Pro ukázání datově řízených webových aplikací, nasazení I ve aktualizovat recenzí webové aplikace z webu jednoduchý, statické některou řízené daty. Jako předtím, existují dvě verze aplikace v tomto kurzu s ke stažení: ten, který používá model projektu webové aplikace a ten, který používá model projektu webové stránky.

Aktualizované recenzí webová aplikace používá [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) databáze, která je uložena na webu s `App_Data` složky (`~/App_Data/Reviews.mdf`). Pokud máte v počítači nainstalovaný SQL Server 2008 by měla tuto ukázku spustit bez chyb. Pokud máte starší verzi systému SQL Server můžete nainstalovat zdarma serveru SQL Server 2008 Express Edition nebo můžete použít databázi, stáhněte si skripty, které jsou k dispozici v tomto kurzu s k vytvoření databáze.

`Reviews.mdf` Databáze obsahuje čtyři tabulky:

- `Genres` -obsahuje záznam pro každý žánr, jako je například technologie, Fiction a firmy.
- `Books` -obsahuje záznam pro každou recenzi, se sloupci stejně jako `Title`, `GenreId`, `ReviewDate`, a `Review`, mimo jiné.
- `Authors` -obsahuje informace o jednotlivých Autor, který se přidal do přezkoumání knihy.
- `BooksAuthors` -many-to-many vazební tabulka, která určuje, jaké autorům napsali jaké knihy.
  

Obrázek 1 ukazuje diagramu ER tyto čtyři tabulky.


[![Kniha revize webové aplikace s databáze je skládá ze čtyř tabulek](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Obrázek 1**: knihy revize webové aplikace s databáze je skládá ze čtyř tabulek ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image3.jpg))


Předchozí verzi webu recenzí měl samostatné stránky technologie ASP.NET pro jednotlivé knihy. Například se stránka s názvem `~/Tech/TYASP35.aspx` , která obsahovala revize pro *naučit sami technologie ASP.NET 3.5 za 24 hodin*. Tato nová verze s daty webu má revize uložené v databázi a jednu stránku ASP.NET, Review.aspx?ID=*bookId*, zobrazuje revize pro zadaného adresáře. Podobně je Genre.aspx?ID=*genreId* stránku se seznamem přezkoumání knih v zadané žánr.

Hodnoty 2 a 3 zobrazit `Genre.aspx` a `Review.aspx` stránky v akci. Poznačte si adresu URL do adresního řádku pro každou stránku. V obrázku 2 it s Genre.aspx? ID = c 85d164ba-1123-4 47-82a0-c8ec75de7e0e. Protože je 85d164ba-1123-4c47-82a0-c8ec75de7e0e `GenreId` tyto kontroly v lokalitě, které spadají pod tento žánr zobrazí hodnotu pro technologie genre, čtení záhlaví stránky s "Technologie kontroly" a seznamu s odrážkami.


[![Na stránce žánr technologie](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Obrázek 2**: na stránku technologie žánru ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image6.jpg))


[![Revize pro samostudium: ASP.NET 3.5 za 24 hodin](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Obrázek 3**: The revize pro *naučit sami technologie ASP.NET 3.5 za 24 hodin* ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image9.jpg))


Webová aplikace knihy zkontroluje také obsahuje oddíl správy, kde správci mohou přidávat, upravovat a odstranit žánry, revize a vytvářet informace. Všechny návštěvníky v současné době můžete přístup k bodu správy. V budoucích kurzu přidáme podporu pro uživatelské účty a Povolit jenom Autorizovaní uživatelé do stránky pro správu.

Pokud stahujete aplikaci recenzí prosím Uvědomte si, že jeho účelem je předvést, nasazení aplikace řízené daty. Osvědčené postupy až na hodnotu návrh aplikace nebude provedena. Například neexistuje žádné samostatné dat přístup vrstvy DAL (); stránky technologie ASP.NET komunikují přímo s databází prostřednictvím SqlDataSource ovládací prvek nebo kód technologie ADO.NET v jejich použití modelu code-behind tříd. Podrobnější pohled na vytváření aplikací řízených daty pomocí vrstvené architektury, najdete v části Moje [ *práce s daty* kurzy](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Databáze na vývoj a produkčního prostředí

Při spuštění vývoje s daty webové aplikace musíte zadat připojovací řetězec databáze, která poskytuje podrobnosti o aplikaci o tom, jak se připojit k databázi. Tento připojovací řetězec Určuje, mimo jiné, databázový server, název databáze a informace o zabezpečení. Nejčastěji se liší od databáze nepoužívá, pokud databáze používá aplikace během vývoje ho s v produkčním prostředí. Existuje mnoho výhod pro vývoj a produkční použití různých databázích. S jinými databáze znamená vývoj don t nemusíte dělat starosti o omylem Neupravujte ani neodstraňuje data dynamická. Můžete ho taky umístit do fiktivní testovací data nebo provést změny způsobující chyby do datového modelu bez nutnosti starat o vliv na aplikace v produkčním prostředí. Nevýhodou s jinou databázi ve vývojovém a produkčním prostředí je, že když je aplikace nasazena databáze a všechny související změny schématu databáze s nebo data musí být také nasazený.

Před prvním nasazením existuje pouze jedna instance databáze a tato instance je ve vývojovém prostředí. Při nasazení aplikace do produkčního prostředí poprvé jsme musíte nejen zkopírujte si potřebné soubory na straně serveru a na straně klienta, ale také zkopírovat databázi z vývojového prostředí do produkčního prostředí. To je, kde jsme stojí pravé teď s webovou aplikací recenzí – se databáze nachází v `App_Data` složky v Naše vývojové prostředí ale nebyl dosud bylo vloženo do produkčního prostředí.

Po nasazení aplikace existují dvě kopie databáze. Jak aplikace můžou vést k tomu, lze přidat nové funkce, proto je nutné změnit do datového modelu (jako je například přidávání nové sloupce do existující tabulky, provádět změny existujících sloupců, přidání nové tabulky a tak dále). Pokud další nasazení webové aplikace, změny použita pro databázi ve vývojovém prostředí od posledního nasazení je nutné použít na produkční databázi. Některé postupy při správě tohoto procesu jsou popsány v budoucích kurzech. Tento kurz se zaměřuje na nasazení do produkčního prostředí celé databáze z vývojového prostředí.

## <a name="deploying-the-database-to-the-production-environment"></a>Nasazení databáze do produkčního prostředí

Zbývající část tohoto kurzu bude vypadat na tom, jak nasadit databázi z vývojového prostředí do produkčního prostředí. Pokud postupujete podél je potřebujete a ověřte, že váš účet se svým poskytovatelem webového hostitele zahrnující Microsoft SQL Server database podporu. Musíte také mít některé informace aktuální, konkrétně název databázového serveru, název databáze a uživatelské jméno a heslo použité pro připojení k databázi.

Jak je uvedeno výše v tomto kurzu, databáze webu s recenzí je uložené v databázi SQL Server 2008 Express Edition `App_Data` složky. By stojí na důvodu, že nasazení tyto databáze by byly stejně snadné jako kopírování `App_Data` složku z vývojového prostředí do produkčního prostředí. Ale většina poskytovatelů webového hostitele nepodporuje hostování databází v `App_Data` složku z důvodů zabezpečení. Weboví hostitelé místo toho použijte účet s na databázovém serveru SQL Server v rámci jejich prostředí. Nasazení databáze z vývojového prostředí do produkčního prostředí vyžaduje získávání databázi registrována na webovém serveru databázi s hostiteli.

Takže jak můžete získat databáze z vývojového prostředí do produkčního prostředí? Existuje několik způsobů, jak to provést v závislosti na tom, jaké vaše nabídky hostitele webové služby. S některými hostiteli, jako je například DiscountASP.NET, můžete FTP zálohování databáze nebo skutečné `.mdf` soubor na svůj web a potom obnovit záložní soubor z ovládacích panelů nebo připojit `.mdf` soubor databáze serveru SQL Server. Pomocí těchto nástrojů nasazení databáze je stejně jednoduché jako kopírování `App_Data` složku do produkčního prostředí a připojíte ho v Ovládacích panelech. To je pravděpodobně nejjednodušší a nejrychlejší způsob, jak při prvním publikování databáze.

Další možností je použití Průvodce publikováním databáze. Průvodce publikováním databáze je aplikace klasické pracovní plochy Windows, který vygeneruje příkazů SQL vytvořte schéma databáze s – tabulky, uložené procedury, zobrazení, uživatelem definované funkce a tak dále – a volitelně data ve svých tabulek. Můžete pak se připojte k serveru webového hostitele zprostředkovatele s databáze SQL Server Management Studio a pak spusťte tento skript k duplikování databáze v produkčním prostředí. Ještě lepší, pokud poskytovatel webového hostitele podporuje Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) můžete mít skript vygenerovaný pomocí Průvodce publikováním databáze automaticky spustí na serveru databáze za vás. Vzhledem k tomu Průvodce publikováním databáze generuje skript, který vytvoří databázi s schéma a data, bude fungovat bez ohledu na to, zda zprostředkovateli webového hostitele nabízí funkce, jako je připojení nahrané `.mdf` souboru.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generování příkazů SQL vytvořte schéma databáze a dat pomocí Průvodce publikováním databáze

Umožní s provedou použitím Průvodce publikováním databáze k databázi recenzí nasazení do produkčního prostředí. Pokud používáte Visual Studio 2008 nebo novější, Průvodce publikováním databáze je již nainstalována. Pokud používáte Visual Studio 2005, bude nutné si nejdřív [stáhnout a nainstalovat](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) průvodce.

Otevřete sadu Visual Studio a přejděte `Reviews.mdf` databáze. Pokud používáte aplikaci Visual Web Developer, přejdete na Průzkumník databáze; Pokud používáte Visual Studio, použijte Průzkumníka serveru. Obrázek 4 ukazuje `Reviews.mdf` databáze v Průzkumníku databází v aplikaci Visual Web Developer. Jak ukazuje obrázek 4 `Reviews.mdf` databáze se skládá z čtyři tabulky, tři uložených procedur a uživatelem definované funkce.


[![Vyhledejte databázi v Průzkumníku databází nebo Průzkumníka serveru](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Obrázek 4**: vyhledejte databázi v Průzkumníku databází nebo Průzkumníka serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image12.jpg))


Klikněte pravým tlačítkem na název databáze a v místní nabídce zvolte možnost "Publikovat poskytovatele". Spustí se Průvodce publikováním databáze (viz obrázek 5). Klikněte na tlačítko vedle záloh za úvodní obrazovka.


[![Úvodní obrazovka Průvodce publikováním databáze](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Obrázek 5**: The Database publikování průvodce úvodní obrazovka ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image15.jpg))


Druhá obrazovka Průvodce zobrazí seznam databází, které jsou přístupné pro Průvodce publikováním databáze a umožňuje zvolit, jestli se má skript všechny objekty ve vybrané databázi nebo pro výběr objektů, které do skriptu. Vyberte příslušnou databázi a ponechat zaškrtnutým políčkem "Skript všechny objekty ve vybrané databázi".

> [!NOTE]
> Pokud se zobrazí chyba "nejsou žádné objekty v databázi *databaseName* typů skriptovatelný průvodcem" při kliknutí na další obrazovce vidíte na obrázku 6, ujistěte se, že není příliš dlouhou cestu k souboru databáze. Bylo zjištěno, že tato chyba může nastat, pokud cesta k souboru databáze je příliš dlouhá.


[![Úvodní obrazovka Průvodce publikováním databáze](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Obrázek 6**: The Database publikování průvodce úvodní obrazovka ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image18.jpg))


Na další obrazovce můžete vygenerovat soubor skriptu nebo, pokud webového hostitele ji podporuje, publikovat i databázi přímo k vašemu databázovému serveru webového hostitele zprostředkovatele s. Jak je vidět na obrázku 7, mám zapsána do souboru skriptu `C:\REVIEWS.MDF.sql`.


[![Databáze do souboru skriptu nebo ji publikovat přímo do Váš_poskytovatel_e webového hostitele](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Obrázek 7**: databáze do souboru skriptu nebo ji publikovat přímo do webového hostitele Váš_poskytovatel_e ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image21.jpg))


Na další obrazovce vás vyzve k zadání širokou škálu možností skriptování. Můžete určit, zda skript by měl obsahovat příkazy drop odebrat tyto stávajících objektů. Výchozí hodnota True, což je v pořádku po prvním nasazení databáze. Můžete také určit, jestli je cílová databáze systému SQL Server 2000, SQL Server 2005 nebo SQL Server 2008. Nakonec můžete určit, jestli se má skript schéma a data, jenom data, nebo pouze schéma. Schéma je shromažďování databázových objektů, tabulek, uložených procedur, zobrazení a tak dále. Data jsou informace, které se nacházejí v tabulkách.

Jak ukazuje obrázek 8, můžu ve je teď nakonfigurovaná tak, aby odstranit existující databázové objekty, průvodce se vygenerovat skript pro databázi systému SQL Server 2008 a publikovat schéma a data.


[![Zadejte publikování možnosti](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Obrázek 8**: Zadejte možnosti publikování ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image24.jpg))


Poslední dvě obrazovky shrnují akce, které se chystáte provést a zobrazte stav skriptování. Net výsledek spuštění Průvodce je, že máme soubor skriptu, který obsahuje příkazy SQL, které jsou potřebné k vytvoření databáze v produkčním prostředí a jeho naplnění stejná data jako na vývoj.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Provádění příkazů SQL na databázi produkční prostředí

Když teď máme skript, který obsahuje příkazy SQL, chcete-li vytvořit databázi a její data všechny zbývající okamžiku, kdy je ke spuštění skriptu na produkční databázi. Někteří poskytovatelé webového hostitele nabízejí textového pole v jejich ovládacích panelů, ve kterém můžete zadat příkazy SQL, spusťte na databázi. Pokud máte velké skript a pak tuto možnost nemusí fungovat ( `REVIEWS.MDF.sql` soubor skriptu pro instanci je velikost, více než 425 KB).

Lepším řešením je pro připojení přímo k provozním serveru databázi pomocí SQL Server Management Studio (SSMS). Pokud máte jiných - Express edici systému SQL Server nainstalovaný v počítači pravděpodobně již máte nainstalované aplikace SSMS. V opačném případě můžete [stáhnout a nainstalovat](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) bezplatnou kopii verze SQL Server Management Studio Express Edition.

Spusťte aplikaci SSMS a připojte se k vašemu web s databázovému serveru hostitele pomocí informací uvedených ve zprostředkovateli webového hostitele.


[![Připojení k vašemu databázovému serveru webového hostitele zprostředkovatele s](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Obrázek 9**: připojení k Váš_poskytovatel_e webového hostitele s databázový Server ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image27.jpg))


Rozbalte položku na kartě databáze a vyhledejte vaši databázi. Klikněte na tlačítko Nový dotaz v levém horním rohu panelu nástrojů, vložte v příkazech SQL ze souboru skriptu, který je vytvořený pomocí Průvodce publikováním databáze a klikněte na tlačítko Spustit a spusťte tyto příkazy na provozním serveru databáze. Pokud je mimořádně velký soubor skriptu může trvat několik minut, aby se příkazy.


[![Připojení k vašemu databázovému serveru webového hostitele zprostředkovatele s](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Obrázek 10**: připojení k Váš_poskytovatel_e webového hostitele s databázový Server ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image30.jpg))


Všechny existuje tento s je to! Vývoj databáze v tomto okamžiku je duplicitní do produkčního prostředí. Pokud obnovíte databázi v aplikaci SSMS byste měli vidět nové databázové objekty. Obrázku 11 můžete vidět produkční databázi s tabulkami, uložených procedur a uživatelem definovaných funkcí, které zrcadlí na vývoj databází. A protože jsme vydal pokyn pro nástroje Průvodce publikováním databáze k publikování dat, produkčních tabulek databáze s obsahovaly stejná data jako tabulky s vývoj databáze v době, kdy se spustil průvodce. Obrázek 12 se zobrazí data v `Books` tabulku v provozní databázi.


[![Duplikovali objekty databáze na produkční databáze](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Obrázek 11**: databázi objekty mají duplicitní na provozní databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image33.jpg))


[![Provozní databáze obsahuje stejná Data jako na vývoj databází](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Obrázek 12**: produkční databáze obsahuje stejná Data jako na vývoj databází ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-a-database-cs/_static/image36.jpg))


V tuto chvíli jsme nasadili jenom vývoj databáze do produkčního prostředí. Ještě jsme zvažovali nasazení vlastní webové aplikace nebo prověřit, jaké změny v konfiguraci jsou potřeba pro použití provozní databáze je aplikace v produkčním prostředí. V dalším kurzu budeme zabývat těmito problémy!

## <a name="summary"></a>Souhrn

Nasazení aplikace řízené daty webu vyžaduje kopírování databáze použitý při vývoji do produkčního prostředí. Mnoho poskytovatelů webového hostitele nabízí nástroje, které zjednodušuje proces nasazení databáze. Například můžete pomocí DiscountASP.NET FTP databáze `.mdf` souboru (nebo zálohy) a pak připojili databázi k databázovým serverem z ovládacích panelů. Další možnost, která funguje bez ohledu na to, jaké funkce nabídek webového hostitele zprostředkovatele je společnosti Microsoft s Database Publishing Wizard nástroj, který generuje skript SQL příkazy k vytvoření vývoje databáze s schéma a data. Jakmile tento skript byl vygenerován. můžete je spustit na produkční databázi.

Teď, když databáze s recenzí webové aplikace v produkčním prostředí jsme nasazení aplikace. Ale informace o konfiguraci webové aplikace s Určuje připojovací řetězec k databázi, a tento připojovací řetězec odkazuje na databázi vývoj. Potřebujeme aktualizovat informace o tomto připojovacím řetězci při nasazování webu do produkčního prostředí. V dalším kurzu zkoumá tyto rozdíly konfigurací a vás provede kroky potřebné k publikování lokality s daty recenzí do produkčního prostředí.

Všechno nejlepší programování!

#### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Stáhněte si průvodce 1.1 publikováním databáze Microsoft SQL Server](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Stáhněte si Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Předchozí](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [další](configuring-the-production-web-application-to-use-the-production-database-cs.md)
