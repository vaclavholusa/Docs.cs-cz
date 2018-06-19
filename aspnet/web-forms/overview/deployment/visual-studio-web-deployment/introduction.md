---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: Úvod | Microsoft Docs'
author: tdykstra
description: Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET pomocí webové aplikace Azure App Service Web Apps nebo poskytovatele hostingu třetích stran, pomocí V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890681"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: Úvod
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace Azure App Service Web Apps nebo poskytovatele hostingu třetích stran, pomocí sady Visual Studio 2012 pomocí sady Azure SDK pro .NET. Většina postupy jsou podobné pro Visual Studio 2013.
> 
> Můžete vyvíjet webovou aplikaci je k dispozici v síti Internet. Ale webové programování kurzy obvykle zastavit bezprostředně potom, co se vám jste zobrazí postup, jak získat něco práce ve svém vývojovém počítači. Tato série kurzů začne, kde ostatní ponechejte: jste vytvořené webovou aplikaci, otestovat ji, a je připraven k odeslání. Co je další? Tyto kurzy vám ukážou, jak nasadit nejdřív do služby IIS ve svém místním vývojovém počítači pro testování a Azure nebo poskytovatele hostingu třetích stran pro pracovní a provozní. Ukázkové aplikace, budete nasazovat je projekt webové aplikace, který používá rozhraní Entity Framework, SQL Server a systém členství technologie ASP.NET. Ukázková aplikace používá webových formulářů ASP.NET, ale postupy zobrazí platí také pro rozhraní ASP.NET MVC a webového rozhraní API.
> 
> Tyto kurzy předpokládá, že víte, jak pracovat s technologií ASP.NET v sadě Visual Studio. Pokud to neuděláte, je vhodné oddělení na zahájení [základní technologie ASP.NET Web Forms kurzu](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) nebo [základní kurz k ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum nasazení ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) nebo [StackOverflow](http://stackoverflow.com).
> 
> Tento obsah je k dispozici jako volné elektronickou knihu v [Galerie elektronických knih TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Přehled

Tyto kurzy vás provede nasazením webové aplikace ASP.NET, která zahrnuje databáze systému SQL Server. Budete nasazovat nejprve do služby IIS ve svém místním vývojovém počítači pro testování a webových aplikací v Azure App Service a Azure SQL Database pro pracovní a provozní. Uvidíte, jak nasadit pomocí sady Visual Studio publikování jedním kliknutím a uvidíte, jak nasadit pomocí příkazového řádku.

Počet kurzy provést proces nasazení pravděpodobně složitý. Ve skutečnosti ale základní postupy jsou jednoduché. Ale v situacích, reálného, často musíte udělat úlohy navíc nasazení – například nastavení oprávnění složky na cílovém serveru. Jsme některé z těchto dalších úloh v naděje, který kurzů k nenechávejte informace, které mohou zabránit úspěšně nasazení reálné aplikaci zobrazené.

Kurzy jsou navrženy pro spouštění v pořadí, a každou část vytvoří v předchozí části. Můžete přeskočit částí, které nejsou relevantní pro vaši situaci, ale pak budete muset upravit postupy v dalších kurzech.

## <a name="intended-audience"></a>Cílová skupina

Kurzy jsou zaměřené na vývojáře využívající technologii ASP.NET, kteří pracují v prostředích, kde:

- V provozním prostředí je Azure App Service Web Apps nebo poskytovatele hostitelských služeb třetích stran.
- Nasazení se neomezuje na průběžnou integraci procesu, ale může provést přímo ze sady Visual Studio.

Nasazení z [ovládací prvek zdroje](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) pomocí [nastavené průběžné doručování](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) procesu není zahrnutý v těchto kurzech s výjimkou jeden kurzu, který ukazuje, jak nasadit z příkazového řádku. Informace o nastavené průběžné doručování najdete v následujících zdrojích informací:

- [Průběžnou integraci a nastavené průběžné doručování (vytváření reálných cloudových aplikací pomocí služby Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Nasazení webové aplikace v Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Nasazení webové aplikace v podnikové scénáře](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (starší sadu kurzů napsán pro sadu Visual Studio 2010, která má stále užitečné informace pro podniková prostředí.)

## <a name="using-a-third-party-hosting-provider"></a>Pomocí poskytovatele hostitelských služeb třetích stran

Kurzy vás provede procesem vytvoření účtu Azure a nasazení aplikace do webové aplikace v Azure App Service pro pracovní a provozní. Můžete však použít stejné základní postupy pro nasazení do hostujícího zprostředkovatele třetí strany podle vašeho výběru. Kde kurzů k projít procesy, které jsou jedinečné pro Azure, která vysvětlují a poradit rozdíly, co můžete očekávat u poskytovatele hostingu třetích stran.

## <a name="deploying-web-app-projects"></a>Nasazení webové aplikace projekty

Ukázkové aplikace, můžete stáhnout a nasadit pro tyto kurzy je projekt webové aplikace Visual Studio. Ale pokud nainstalujete nejnovější [aktualizace Publikovat Web pro Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), můžete použít stejné metody nasazení a nástroje pro projekty webových aplikací.

## <a name="deploying-aspnet-mvc-projects"></a>Nasazení projekty ASP.NET MVC

Ukázkové aplikace je projekt webových formulářů ASP.NET, ale všechno, co zjistíte, jak to provést je také použít na rozhraní ASP.NET MVC. Projekt Visual Studio MVC je právě jinou formu projekt webové aplikace. Jediným rozdílem je, že pokud nasazujete do poskytovatele hostitelských služeb, která nepodporuje rozhraní ASP.NET MVC nebo vaší je cílové verzi, je nutné vybrat jistotu, že jste nainstalovali odpovídající ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) nebo [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) balíček NuGet do projektu.

## <a name="programming-language"></a>Programovací jazyk

Ukázková aplikace používá C#, ale kurzů k nevyžadují znalosti jazyka C# a techniky nasazení zobrazí kurzů k nejsou specifické pro jazyk.

## <a name="database-deployment-methods"></a>Metody nasazení databáze

Existují tři způsoby, můžete nasadit databázi systému SQL Server spolu s nasazení webu v sadě Visual Studio:

- Migrace Code First Entity Framework
- Poskytovatele nasazení webu dbDacFx
- DbFullSql poskytovatele nasazení webu

V tomto kurzu budete používat první dva z těchto metod. DbFullSql poskytovatele nasazení webu je starší verze metoda, která již není doporučeno s výjimkou některé konkrétní scénáře, jako je například migrace ze systému SQL Server Compact do systému SQL Server.

Metody uvedené v tomto kurzu jsou pro databáze serveru SQL, není systém SQL Server Compact. Informace o tom, jak nasadit databázi systému SQL Server Compact najdete v tématu [Visual Studio Web nasazení s systém SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Metody uvedené v tomto kurzu vyžadují, použijte Web Deploy metodu publikování. Pokud dáváte přednost jinou metodu, jako je například FTP, systém souborů nebo FPSE, publikování najdete v části [nasazení databáze odděleně od nasazení webových aplikací](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) v nasazení webového obsahu mapy pro Visual Studio a ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migrace Code First Entity Framework

V rozhraní Entity Framework verze 4.3 zavedl Microsoft migrace Code First. Migrace Code First automatizuje proces vytváření přírůstkové změny do datového modelu a šíření tyto změny do databáze. V dřívějších verzích systému Code First je obvykle nechat rozhraní Entity Framework vyřadit a znovu vytvořit databázi při každé změně datového modelu. Nejedná se o problém v vývoj, protože testovací data je snadno znovu vytvořena, ale v produkčním prostředí chcete obvykle aktualizovat schéma databáze bez jejich odstranění databáze. Povolí funkci migrace Code First k aktualizaci databáze bez vyřadit a znovu ji vytvořit. Můžete je nechat Code First automaticky rozhodnout, jak provádět změny požadované schématu, nebo můžete napsat kód, který upravuje změny. Úvod do migrace Code First, najdete v části [migrace Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Při nasazení webového projektu sady Visual Studio můžete automatizovat proces nasazení databáze, která spravuje migrace Code First. Když vytvoříte profil publikování, vyberete zaškrtávací políčko, které je označené spustit migrace Code First (spuštěno při spuštění aplikace). Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci souboru Web.config aplikace na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátoru třídy.

Visual Studio nemá žádný s databází během procesu nasazení. Když nasazené aplikace získá přístup k databázi po nasazení poprvé, Code First automaticky vytvoří databázi nebo aktualizuje schéma databáze na nejnovější verzi. Pokud aplikace implementuje metodu migrace počáteční hodnoty, metoda spustí po vytvoření databáze nebo schéma se aktualizuje.

V tomto kurzu použijete migrace Code First pro nasazení databáze aplikace.

### <a name="the-dbdacfx-web-deploy-provider"></a>Poskytovatele nasazení webu dbDacFx

Pro databázi systému SQL Server, který není spravován nástrojem Entity Framework Code First můžete vybrat zaškrtávací políčko, které je označené aktualizace databáze, když konfigurujete profil publikování. Během počátečního nasazení poskytovatele dbDacFx vytvoří tabulky a další objekty databáze v cílové databázi tak, aby odpovídaly zdrojové databáze. Na další nasazení zprostředkovatele určuje, co se liší mezi databázemi zdrojové a cílové a aktualizuje schéma cílové databáze tak, aby odpovídaly zdrojové databáze. Ve výchozím nastavení nebude zprostředkovatele proveďte požadované změny dojít ke ztrátě dat, například při přerušení tabulky nebo sloupce.

Tato metoda není automatizovat nasazení data v tabulkách databáze, ale můžete vytvořit skripty k tomu a konfigurace Visual Studia, abyste je mohli spustit během nasazení. Dalším důvodem pro spouštění skriptů při nasazení je provést změny schématu, které nelze provést automaticky, protože jejich by dojít ke ztrátě dat.

V tomto kurzu použijete poskytovatele dbDacFx nasazení databáze členství technologie ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Řešení potíží s při tomto kurzu

Když dojde k chybě během nasazení, nebo bude web nefunguje správně, neposkytují chybové zprávy vždy zřejmé řešení. Které vám pomohou s některé běžné scénáře problém, [stránka s referencemi k řešení potíží s](troubleshooting.md) je k dispozici. Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte zkontrolovat stránce řešení potíží.

## <a name="comments-welcome"></a>Vítejte: komentáře

Komentáře k kurzů k jsou úvodní a při aktualizaci kurzu veškeré úsilí, budou provedeny brát v účtu opravy nebo návrhy na vylepšení, které jsou k dispozici v kurzu komentáře.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

Tento kurz je určen pro následující produkty:

- Windows 8 nebo Windows 7.
- Visual Studio 2012 nebo Visual Studio 2012 Express pro Web s [nejnovější aktualizace](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Můžete postupovat v kurzu pomocí Visual Studio 2010 SP1 nebo Visual Studio 2013, ale některé snímky obrazovky se bude lišit a některé funkce se bude lišit.

Pokud používáte Visual Studio 2013, nainstalujte [Azure SDK pro Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Pokud používáte Visual Studio 2010 SP1, nainstalujte následující software:

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [Nástroje SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

V závislosti na tom, kolik závislostí sady SDK už máte na počítači může instalace sady Azure SDK trvat dlouhou dobu, od několika minut až půl hodiny nebo i déle. I když chcete publikovat do jiných výrobců poskytovatele hostingu místo do Azure, protože sada SDK zahrnuje nejnovější aktualizace sady Visual Studio web publikovat funkce musíte sadu Azure SDK.

> [!NOTE]
> V tomto kurzu byla zapsána 1.8.1 verzi sady Azure SDK. Od té doby byly vydány novější verze další funkce. Abychom zmínili těmito funkcemi a odkaz na prostředky, které mají další informace o nich byly aktualizovány kurzů k.


Pokyny a snímky obrazovky jsou založené na systému Windows 8, ale kurzů k vysvětlují rozdíly pro systém Windows 7.

Některé další software je vyžadováno k dokončení tohoto kurzu, ale nemáte nastaven, který dosud nainstalován. Tento kurz vás provede kroky pro instalaci se v případě potřeby.

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

Aplikace, která budete nasazovat jmenuje Contoso univerzity a již bylo vytvořeno za vás. Je zjednodušenou verzi webu univerzity, volně podle aplikace Contoso univerzity podrobněji popsaná [Entity Framework – kurzy na webu technologie ASP.NET](https://asp.net/entity-framework/tutorials).

Až budete mít nainstalovány požadované součásti, stáhněte si [webové aplikace Contoso univerzity](https://go.microsoft.com/fwlink/p/?LinkId=282627). *.Zip* soubor obsahuje více verzí projektu. Chcete-li provede kroky kurzu, začněte projekt ve složce C#. Pokud chcete zjistit, co projektu vypadá na konci kurzů k, otevřete projekt ve složce ContosoUniversity-End.

Příprava projektu pro kurz kroků, proveďte následující kroky:

1. Uložte soubory řešení ContosoUniversity ze složky C# do složky s názvem ContosoUniversity v libovolné složky použijete pro práci s projektů sady Visual Studio.

    Ve výchozím nastavení to je tento adresář pro Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Pro snímky obrazovky v tomto kurzu, složce projektu se nachází v kořenovém adresáři na `C`: jednotku.)
2. Spuštění sady Visual Studio a otevřete projekt.
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a klikněte na **obnovení balíčků EnableNuGet**.
4. Sestavte řešení.
5. Pokud dojde k chybám kompilace, obnovte ručně balíčky NuGet:

    1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **spravovat balíčky NuGet pro řešení**.
    2. V horní části **spravovat balíčky NuGet** dialogové okno se zobrazí **chybí některá NuGet balíčky toto řešení. Klikněte na tlačítko Obnovit.** Klikněte **obnovení** tlačítko.
    3. Znovu sestavte řešení.
6. Stisknutím CTRL + F5 a spusťte aplikaci.

    Aplikace se otevře domovskou stránku univerzity Contoso.

    ![Domovská stránka vývojářů](introduction/_static/image1.png)

    (Může být dobu čekání při spuštění sady Visual Studio k instanci SQL serveru Express LocalDB a chcete může získat vypršení časového limitu, proces trvá příliš dlouho. V takovém případě stačí spusťte projekt znovu.)

Webových stránek jsou přístupné z řádku nabídek a umožňují provádět následující funkce:

- Zobrazí statistické údaje o student (stránku o).
- Zobrazit, upravit, odstranit a přidejte studenty.
- Zobrazit a upravit kurzy.
- Zobrazení a úpravě vyučující.
- Zobrazení a úpravě oddělení.

Tady jsou snímky obrazovky několik reprezentativní stránek.

![Studenti, kteří stránky vývojářů](introduction/_static/image2.png)

![Přidat studenty stránky vývojářů](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Zkontrolujte funkce aplikace, které ovlivňují nasazení

Následující funkce aplikace budou mít vliv nasazení nebo co musíte udělat, aby ho nasadit. Každý z nich je vysvětlené podrobněji v následujících kurzech v řadě.

- Contoso univerzity používá databázi systému SQL Server k ukládání dat aplikací, například studenty a lektorem názvy. Databáze obsahuje směs testovací data a provozními daty, a při nasazení do produkčního prostředí je potřeba vyloučit v testovacích datech.
- Aplikace používá systém členství technologie ASP.NET, která ukládá informace o uživatelském účtu v databázi systému SQL Server. Aplikace definuje uživatel s oprávněním správce, který má přístup k určité omezené informace. Je třeba nasadit databázi členství bez testovací účty, ale s účtem správce.
- Aplikace používá chyba třetích stran, protokolování a vytváření sestav nástroje. Tento nástroj je součástí sestavení, které musí být nasazeno s aplikací.
- Nástroj protokolování chyba zapisuje informace o chybě v souborů XML do sdílené složky. Máte a ujistěte se, že účet, který technologie ASP.NET spuštěna pod v nasazené lokality má oprávnění k zápisu do této složky a musíte tuto složku vyloučit z nasazení. (, Jinak chyba data protokolu z testovacího prostředí je možno nasadit do produkčního prostředí nebo soubory protokolů chyba produkční může odstranit.)
- Aplikace zahrnuje některá nastavení, které je potřeba změnit v nasazené *Web.config* souboru v závislosti na cílovém prostředí (test, pracovní nebo produkční) a další nastavení, které je potřeba změnit v závislosti na sestavení konfigurace (ladění nebo verze).
- Řešení nástroje Visual Studio obsahuje projektu knihovny tříd. Pouze sestavení, které generuje tento projekt by měl být nasazen, nikoli projekt.

## <a name="summary"></a>Souhrn

V tomto prvním kurzu v řadě byly staženy ukázkový projekt sady Visual Studio a zkontrolovat lokality funkce, které ovlivňují nasazení aplikace. V následujících kurzech připravíte nasazení nastavením některé z těchto věcí automaticky zpracovávat. Ostatní můžete postará o ručně.

> [!div class="step-by-step"]
> [Next](preparing-databases.md)
