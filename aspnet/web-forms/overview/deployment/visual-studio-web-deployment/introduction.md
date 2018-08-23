---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: Úvod | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace Azure App Service Web Apps nebo jiného poskytovatele hostitelských služeb, za použití V...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: a227f6564607ed9e909fc4d6297d7370f8fb62b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754631"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: Úvod
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace Azure App Service Web Apps nebo jiného poskytovatele hostitelských služeb, pomocí sady Visual Studio 2012 pomocí sady Azure SDK pro .NET. Většina postupy jsou podobné pro Visual Studio 2013.
> 
> Při vývoji webové aplikace, aby byla k dispozici uživatelům přes Internet. Ale webové programování kurzy obvykle zastavit bezprostředně potom, co se vám ukázal tom, jak něco funguje na vašem vývojovém počítači. Tato série kurzů začne, kde ostatní vynechat: jsme vytvořili webovou aplikaci, otestovat ji, a je připravená. Co se chystá? Tyto kurzy vám ukážou, jak nasadit nejdřív do služby IIS na místním počítači pro vývoj pro testování a do Azure nebo poskytovatele hostingu třetích stran pro přípravným a produkčním prostředím. Ukázková aplikace, které budete nasazovat je projekt webové aplikace, která používá Entity Framework, SQL Server a systém členství technologie ASP.NET. Ukázková aplikace používá webových formulářů ASP.NET, ale postupy uvedené platí také pro ASP.NET MVC a webového rozhraní API.
> 
> Tyto kurzy se předpokládá, že víte, jak pracovat s ASP.NET v sadě Visual Studio. Pokud to neuděláte, je vhodné oddělení na zahájení [základní technologie ASP.NET webové formuláře kurzu](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) nebo [základní kurz ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET nasazení](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) nebo [StackOverflow](http://stackoverflow.com).
> 
> Tento obsah je také dostupné jako Bezplatná elektronická příručka v [E-kniha Galerie TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Přehled

Tyto kurzy vás provede nasazení webové aplikace ASP.NET, která zahrnuje databáze systému SQL Server. Nejprve se budete nasazovat, do služby IIS na místním počítači pro vývoj pro testování a do služby Web Apps v Azure App Service a Azure SQL Database pro přípravným a produkčním prostředím. Uvidíte jak nasadit s použitím sady Visual Studio publikování jedním kliknutím a uvidíte, jak nasadit pomocí příkazového řádku.

Počet kurzy, které by mohlo způsobit nepoužitelnost vypadá to, že složitý proces nasazení. Ve skutečnosti ale základní postupy jsou jednoduché. Nicméně v reálných situacích, často musíte dělat úkoly, další nasazení, například nastavení oprávnění pro složky na cílovém serveru. Jsme některé z těchto dalších úloh v naději, že v kurzech nenechávejte si informace, které mohou bránit úspěšné nasazení aplikace skutečný zobrazené.

V kurzech jsou navrženy pro spouštění v pořadí a každá část vychází z předchozí části. Můžete přeskočit k části, které nejsou relevantní pro vaši situaci, ale pak může být nutné upravit postupy v budoucích kurzech.

## <a name="intended-audience"></a>Cílová skupina

V kurzech jsou zaměřené na vývojáře využívající technologii ASP.NET, kteří pracují v prostředích, kde:

- Produkčním prostředí je Azure App Service Web Apps nebo poskytovatele hostitelských služeb třetích stran.
- Nasazení není omezena pouze na proces průběžné integrace, ale může provést přímo ze sady Visual Studio.

Nasazení z [správy zdrojového kódu](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) pomocí [průběžné doručování](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) procesu nejsou uvedeny v následujících kurzech s výjimkou jednoho kurz, který vás provede postupem nasazení z příkazového řádku. Informace o průběžné doručování najdete v článku na následujících odkazech:

- [Průběžná integrace a průběžné doručování (vytváření skutečných cloudových aplikací pomocí Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Nasazení webové aplikace ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Nasazení webové aplikace v podnikových scénářích](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (starší sadu kurzů, které jsou napsané pro Visual Studio 2010, která stále obsahuje užitečné informace pro podniková prostředí.)

## <a name="using-a-third-party-hosting-provider"></a>Pomocí poskytovatele hostitelských služeb třetích stran

Kurzy vás provedou procesem vytvoření účtu Azure a nasazení aplikace do služby Web Apps ve službě Azure App Service pro pracovní a provozní. Můžete však použít stejné základní postupy pro nasazení k poskytovateli hostingu třetí strany podle vašeho výběru. Tam, kde v kurzech se přenášejí prostřednictvím jedinečné procesů do Azure, vysvětlují a radí rozdíly, co můžete očekávat, že u poskytovatele hostingu třetích stran.

## <a name="deploying-web-app-projects"></a>Nasazení webové aplikace projekty

Ukázkové aplikace, kterou stáhnete a nasadíte pro tyto kurzy se projekt webové aplikace Visual Studio. Nicméně pokud si nainstalujete nejnovější [Web publikovat aktualizace pro sadu Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), můžete použít stejné metody nasazení a nástroje pro projekty aplikací pro web.

## <a name="deploying-aspnet-mvc-projects"></a>Nasazení projektů ASP.NET MVC

Ukázková aplikace je projekt webových formulářů ASP.NET, ale všechno, co se dozvíte, jak provést se také vztahuje na ASP.NET MVC. Projekt Visual Studio MVC je stejně jiná forma projektu webové aplikace. Jediným rozdílem je, že pokud nasazujete k poskytovateli hostingu, který nepodporuje rozhraní ASP.NET MVC nebo ho vaši cílovou verzi, musíte, že jste nainstalovali odpovídající ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) nebo [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) balíčku NuGet ve vašem projektu.

## <a name="programming-language"></a>Programovací jazyk

Ukázková aplikace používá C#, ale v kurzech nevyžadují žádné znalosti jazyka C# a techniky nasazení zobrazené v kurzech nejsou specifické pro jazyk.

## <a name="database-deployment-methods"></a>Metody nasazení databáze

Existují tři způsoby, můžete nasadit databázi serveru SQL Server spolu s nasazení webu v sadě Visual Studio:

- Migrace Entity Framework Code First
- Poskytovatel nasazení webu dbDacFx
- Poskytovatel nasazení webu dbFullSql

V tomto kurzu použijete první dva z těchto metod. Poskytovatel nasazení webu dbFullSql je starší verze metodu, která už se nedoporučuje s výjimkou některých speciálních situacích, jako je například migrace ze systému SQL Server Compact do systému SQL Server.

Metody uvedené v tomto kurzu jsou pro databáze systému SQL Server, není SQL Server Compact. Informace o tom, jak nasadit databázi SQL Server Compact, naleznete v tématu [Visual Studio webové nasazení s SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Metody uvedené v tomto kurzu vyžadují, použijte Web Deploy metodu publikování. Pokud dáváte přednost jinou metodu, jako je například FTP, systém souborů nebo FPSE, publikování najdete v článku [nasazení databáze odděleně od nasazení webových aplikací](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) v obsahu mapy webové nasazení pro Visual Studio a ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migrace Entity Framework Code First

V rozhraní Entity Framework verze 4.3 Microsoft zavedl službu migrace Code First. Migrace Code First automatizuje proces přírůstkové změny do datového modelu a šíření změn do databáze. V dřívějších verzích Code First obvykle umožňují Entity Framework vyřadit a znovu vytvořit databázi pokaždé, když změníte datový model. To není problém ve vývoji, protože je snadno znovu vytvořit testovací data, ale v produkčním prostředí je obvykle chcete aktualizovat schéma databáze bez vyřazení databáze. Povolí funkci migrace Code First pro aktualizaci databáze bez vyřadit a vytvoříte ho znovu. Můžete nechat Code First automaticky rozhodnout, jak provádět změny požadované schéma, nebo můžete napsat kód, který přizpůsobí tyto změny. Úvod do migrace Code First najdete v tématu [migrace Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Při nasazování webového projektu, Visual Studio můžete automatizovat proces nasazení databáze, která jsou spravována migrace Code First. Když vytvoříte profil publikování, vyberte zaškrtávací políčko s názvem spustit migrace Code First (spuštěno při spuštění aplikace). Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci souboru Web.config aplikace na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátor třídy.

Visual Studio nedělá s databází během procesu nasazení. Když nasazené aplikace získá přístup k databázi po nasazení poprvé, Code First automaticky vytvoří databázi nebo aktualizuje schéma databáze na nejnovější verzi. Pokud aplikace implementuje metodu migrace počáteční hodnoty, metoda se spouští po databáze se vytvoří nebo aktualizuje schéma.

V tomto kurzu použijete migrace Code First pro nasazení aplikace databáze.

### <a name="the-dbdacfx-web-deploy-provider"></a>Poskytovatel nasazení webu dbDacFx

Pro databázi serveru SQL Server, která nespravuje služba Entity Framework Code First můžete vybrat zaškrtávací políčko, které je označené jako aktualizace databáze při konfiguraci profilu publikování. Během počátečního nasazení vytvoří poskytovatel dbDacFx tabulek a ostatních databázových objektů v cílové databázi tak, aby odpovídaly zdrojové databáze. Na další nasazení určuje zprostředkovatele, čím se liší mezi databázemi zdrojové a cílové a aktualizuje schéma cílové databáze tak, aby odpovídaly zdrojové databáze. Ve výchozím nastavení Zprostředkovatel neprovede žádné změny, které způsobit ztrátu dat, jako je například při přerušení tabulky nebo sloupce.

Tato metoda není automatizovat nasazení dat. v tabulkách databáze, ale můžete vytvořit skripty a konfigurace sady Visual Studio spustí během nasazení. K provedení změn schématu, které nelze provést automaticky, protože by způsobit ztrátu dat je dalším důvodem pro spouštění skriptů při nasazení.

V tomto kurzu použijete dbDacFx poskytovatele nasazení členské databáze ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Řešení potíží během tohoto kurzu

Pokud dojde k chybě během nasazení nebo nasazené lokality nespustí správně, chybové zprávy neposkytují vždy zřejmé řešení. Aby vám pomohl některé běžné scénáře problém [Poradce při potížích s referenční stránce](troubleshooting.md) je k dispozici. Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte se podívat na stránce řešení potíží.

## <a name="comments-welcome"></a>Vítá vás komentáře

Komentáře na kurzy jsou úvodní a když dojde k aktualizaci kurzu veškeré úsilí, bude proveden rozdělení účet opravy nebo návrhy na vylepšení, která jsou k dispozici v kurzu komentáře.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

Tento kurz je určen pro následující produkty:

- Systém Windows 8 nebo Windows 7.
- Visual Studio 2012 nebo Visual Studio 2012 Express pro Web s [nejnovější aktualizaci](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK pro sadu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Můžete postupovat podle kurzu pomocí Visual Studio 2010 SP1 nebo Visual Studio 2013, ale některé snímky obrazovky se bude lišit a některé funkce se bude lišit.

Pokud používáte Visual Studio 2013, nainstalujte [sady Azure SDK for Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Pokud používáte Visual Studio 2010 SP1, nainstalujte následující software:

- [Azure SDK pro sadu Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

V závislosti na tom, kolik závislostí sady SDK už máte na svém počítači může instalace sady Azure SDK trvat dlouhou dobu: několik minut, půl hodiny nebo víc. Budete potřebovat sadu Azure SDK, i v případě, že máte v úmyslu publikovat do jiného poskytovatele hostingu místo do Azure, protože sada SDK obsahuje nejnovější aktualizace sady Visual Studio web publikovat funkce.

> [!NOTE]
> V tomto kurzu byl zapsán s verze 1.8.1 sady Azure SDK. Od té doby byly vydány novější verze s dalšími funkcemi. V kurzech se aktualizovaly zmiňovat tyto funkce a odkaz na prostředky, které mají další informace o nich.


Pokyny a snímky obrazovky jsou založeny na Windows 8, ale v kurzech vysvětlují rozdíly pro Windows 7.

Některý software se vyžaduje k dokončení tohoto kurzu, ale není nutné mít nainstalované ještě. Tento kurz vás provede kroky pro instalaci, když je potřebujete.

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

Aplikace, které budete nasazovat jmenuje Contoso University a už se vytvořila za vás. Je zjednodušenou verzi web university, volně založený na aplikaci Contoso University podle [kurzy Entity Frameworku na webu ASP.NET](https://asp.net/entity-framework/tutorials).

Pokud máte nainstalovány požadované součásti, stáhněte si [webové aplikace Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). *ZIP* soubor obsahuje více verzí tohoto projektu. Pro seznámení se základními kroky tohoto kurzu, začněte s projektem, umístěný ve složce jazyka C#. Pokud chcete zobrazit, co bude projekt vypadat na konci v kurzech, otevřete projekt ve složce ContosoUniversity-End.

Příprava projektu v průběhu kurzu kroků, postupujte následovně:

1. Uložte soubory řešení ContosoUniversity ze složky C# ve složce s názvem ContosoUniversity v jakékoli složce můžete použít pro práci s projekty sady Visual Studio.

    Ve výchozím nastavení to je tento adresář pro sadu Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Pro snímky obrazovky v tomto kurzu, složky projektu se nachází v kořenovém adresáři na `C`: jednotku.)
2. Spusťte sadu Visual Studio a otevřete projekt.
3. V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **obnovení balíčků EnableNuGet**.
4. Sestavte řešení.
5. Pokud dojde k chybám kompilace, ručně obnovte balíčky NuGet:

    1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **spravovat balíčky NuGet pro řešení**.
    2. V horní části **spravovat balíčky NuGet** dialogové okno zobrazí **chybí některé NuGet balíčky z tohoto řešení. Klikněte na obnovit.** Klikněte na tlačítko **obnovení** tlačítko.
    3. Znovu sestavte řešení.
6. Stisknutím kláves CTRL-F5 ke spuštění aplikace.

    Aplikace se otevře na domovskou stránku společnosti Contoso University.

    ![Domovská stránka vývoj](introduction/_static/image1.png)

    (Může být doba čekání když zálohujete instanci serveru SQL Server Express LocalDB spustí aplikace Visual Studio a může se zobrazit chyba časového limitu Pokud, který proces trvá příliš dlouho. V takovém případě stačí spusťte projekt znovu.)

Stránky webu jsou přístupné z řádku nabídek a umožňují provádět následující funkce:

- Zobrazení statistiky student (stránku o).
- Zobrazit, upravit, odstranit a přidejte studenty.
- Zobrazení a úpravě kurzů.
- Zobrazení a úpravě Instruktoři.
- Zobrazení a úpravě oddělení.

Tady jsou snímky obrazovky několik reprezentativní stránek.

![Stránce studenty vývojáře](introduction/_static/image2.png)

![Přidat studenty vývojáře stránky](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Kontrola funkce aplikací, které ovlivňují nasazení

Následující funkce aplikaci vliv na způsob nasazení nebo je nutné udělat, abyste ho nasadit. Každá z těchto je vysvětleno podrobněji v následujících kurzech v řadě.

- Contoso University používá databázi serveru SQL Server k ukládání dat aplikací, jako jsou jména studentů a instruktorem. Databáze obsahuje kombinaci testovací data a provozními daty, a při nasazení do produkčního prostředí je třeba vyloučit testovací data.
- Aplikace používá systém členství technologie ASP.NET, která ukládá informace o uživatelském účtu v databázi serveru SQL Server. Aplikace definuje uživatel s oprávněním správce, který má přístup k určité omezené informace. Je třeba nasadit databázi členství bez testovací účty, ale s účtem správce.
- Aplikace používá chybu třetích stran, protokolování a vytváření sestav nástroje. Tento nástroj je součástí sestavení, které musí být nasazeno společně s aplikací.
- Nástroj protokolování chyba zapisuje informace o chybě v souborech XML do složky souboru. Máte, abyste měli jistotu, že účet, který ASP.NET spuštěna v nasazené lokality má oprávnění k zápisu do této složky a budete muset tuto složku vyloučit z nasazení. (V opačném případě data protokolu chyby z testovacího prostředí je možno nasadit do produkčního prostředí a/nebo soubory protokolů produkční chyba mohla být odstraněna.)
- Aplikace obsahuje některá nastavení, které musí být změněny v nasazených *Web.config* soubor v závislosti na cílové prostředí (testovacím, přípravném nebo produkčním prostředí) a další nastavení, která se musí změnit v závislosti na sestavení konfigurace (ladění nebo vydání).
- Řešení sady Visual Studio obsahuje projekt knihovny tříd. Pouze sestavení, která generuje tento projekt by měl být nasazen, ne projekt.

## <a name="summary"></a>Souhrn

V tomto prvním kurzu v této sérii mají stáhnout ukázkový projekt sady Visual Studio a zkontrolovat funkce webů, které ovlivňují, jak nasadit aplikaci. V následujících kurzech se připravíte nasazení zařiďte si některé z těchto věcí automaticky zpracovávat. Ostatní vás postará o ručně.

> [!div class="step-by-step"]
> [Next](preparing-databases.md)
