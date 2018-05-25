---
title: Řešení potíží s ASP.NET Core v Azure App Service
author: guardrex
description: Zjistěte, jak k diagnostikování problémů s nasazením ASP.NET Core Azure App Service.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 47056c80c7abf5dd5ad5ae96af7b821d31b21b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Řešení potíží s ASP.NET Core v Azure App Service

Podle [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Tento článek obsahuje pokyny o tom, jak diagnostikovat ASP.NET Core problém při spuštění aplikace pomocí diagnostických nástrojů Azure App Service. Další pomoc při řešení potíží, najdete v části [Přehled služby Azure App Service diagnostics](/azure/app-service/app-service-diagnostics) a [postup: monitorování aplikací ve službě Azure App Service](/azure/app-service/web-sites-monitor) v dokumentaci k Azure.

## <a name="app-startup-errors"></a>Chyby při spuštění aplikace

**502.5 zpracování selhání**  
Pracovní proces se nezdaří. Aplikaci nelze spustit.

[ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) pokusy o spuštění pracovního procesu, ale nepodaří spustit. Zkoumání protokolu událostí aplikace často pomáhá tento typ problému. Přístup k protokolu je podrobně popsaný [protokolu událostí aplikace](#application-event-log) části.

*502.5 selhání procesu* chybová stránka je vrácena, pokud nesprávně nakonfigurované aplikace způsobí, že pracovní proces selhání:

![Okno prohlížeče zobrazující stránku 502.5 selhání procesu](troubleshoot/_static/process-failure-page.png)

**500 vnitřní chybu serveru**  
Spuštění aplikace, ale chybu brání splnění požadavku server.

Při spuštění nebo při vytváření odpovědi, k této chybě dojde v kódu aplikace. Odpovědi může obsahovat žádný obsah, nebo se může zobrazit odpověď *500 – Vnitřní chyba serveru* v prohlížeči. V protokolu událostí aplikace obvykle stavy normálně spustil aplikaci. Z hlediska serveru, který je správná. Aplikace se spustit, ale nemůže generovat platnou odpověď. [Spusťte aplikaci v konzole Kudu](#run-the-app-in-the-kudu-console) nebo [povolit protokol stdout ASP.NET Core modulu](#aspnet-core-module-stdout-log) k vyřešení tohoto problému.

**Obnovení připojení**

Pokud dojde k chybě po odeslání hlaviček, je příliš pozdní pro server k odeslání **500 – Vnitřní chyba serveru** když dojde k chybě. Často se to stane, když dojde k chybě během serializace komplexních objektů pro odpověď. Tento typ chyby se zobrazí jako *obnovení připojení* chyba na straně klienta. [Protokolování aplikací](xref:fundamentals/logging/index) mohou pomoci při odstraňování těchto typů chyb.

## <a name="default-startup-limits"></a>Výchozí omezení spuštění

Modul základní ASP.NET je konfigurován s výchozím *startupTimeLimit* 120 sekundách. Pokud necháte nastavenou výchozí hodnotu, aplikace může trvat až dvě minuty spustit před modul protokoly selhání procesu. Informace o konfiguraci modulu najdete v tématu [atributy elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Řešení chyb při spuštění aplikace

### <a name="application-event-log"></a>Protokol událostí aplikace

Chcete-li získat přístup k protokolu událostí aplikace, použijte **Diagnostikujte a řešení problémů** okno na portálu Azure:

1. Na portálu Azure otevřete okno aplikace v **App Services** okno.
1. Vyberte **Diagnostikujte a řešení problémů** okno.
1. V části **vyberte KATEGORII problému**, vyberte **webové aplikace dolů** tlačítko.
1. V části **navrhované řešení**, otevřete podokno pro **otevřete protokoly událostí aplikace**. Vyberte **otevřené záznamy událostí aplikace** tlačítko.
1. Zkontrolujte nejnovější chybu poskytované *IIS AspNetCoreModule* v **zdroj** sloupce.

Alternativu k použití **Diagnostikujte a řešení problémů** okno je zkontrolujte v souboru protokolu událostí aplikace přímo pomocí [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Vyberte **Rozšířené nástroje** v okně **nástroje pro vývoj** oblasti. Vyberte **přejděte&rarr;**  tlačítko. Otevře se konzola Kudu v novou kartu prohlížeče nebo okna.
1. Pomocí navigačního panelu v horní části stránky otevřete **konzolou pro ladění** a vyberte **CMD**.
1. Otevřete **LogFiles** složky.
1. Vyberte ikonu tužky vedle *eventlog.xml* souboru.
1. Vyhledejte v protokolu. Přejděte do dolní části zobrazíte nejnovější události v protokolu.

### <a name="run-the-app-in-the-kudu-console"></a>Spusťte aplikaci v konzole Kudu

Mnoho chyb spuštění nepřispívají užitečné informace v protokolu událostí aplikace. Aplikaci můžete spustit [Kudu](https://github.com/projectkudu/kudu/wiki) vzdálené spuštění konzoly chcete zjistit, chyba:

1. Vyberte **Rozšířené nástroje** v okně **nástroje pro vývoj** oblasti. Vyberte **přejděte&rarr;**  tlačítko. Otevře se konzola Kudu v novou kartu prohlížeče nebo okna.
1. Pomocí navigačního panelu v horní části stránky otevřete **konzolou pro ladění** a vyberte **CMD**.
1. Otevření složky k cestě **lokality** > **wwwroot**.
1. V konzole spusťte aplikaci spuštěním sestavení aplikace.
   * Pokud je aplikace [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), spusťte sestavení aplikace s *dotnet.exe*. V následujícím příkazu nahraďte název sestavení aplikace pro `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Pokud je aplikace [samostatná nasazení](/dotnet/core/deploying/#self-contained-deployments-scd)spusťte aplikaci je spustitelný soubor. V následujícím příkazu nahraďte název sestavení aplikace pro `<assembly_name>`: `<assembly_name>.exe`
1. Výstup z aplikace, zobrazuje všechny chyby konzoly je přesměrovaná do konzole Kudu.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modulu stdout protokolu

Protokol stdout ASP.NET Core modulu zaznamenává často užitečné chybové zprávy, nebyl nalezen v protokolu událostí aplikace. Povolení a zobrazit protokoly stdout:

1. Přejděte na **Diagnostikujte a řešení problémů** okno na portálu Azure.
1. V části **vyberte KATEGORII problému**, vyberte **webové aplikace dolů** tlačítko.
1. V části **navrhované řešení** > **povolit přesměrování protokolu Stdout**, kliknutím na tlačítko **otevřít konzolu Kudu upravit soubor Web.Config**.
1. V Kudu **konzole diagnostiky**, otevřete složky k cestě **lokality** > **wwwroot**. Posuňte se dolů a zobrazení *web.config* souboru v dolní části seznamu.
1. Klikněte na ikonu tužky vedle *web.config* souboru.
1. Nastavit **stdoutLogEnabled** k `true` a změňte **stdoutLogFile** cestu k: `\\?\%home%\LogFiles\stdout`.
1. Vyberte **Uložit** uložte aktualizovaný *web.config* souboru.
1. Vytvořte žádost na aplikaci.
1. Vraťte se k portálu Azure. Vyberte **Rozšířené nástroje** v okně **nástroje pro vývoj** oblasti. Vyberte **přejděte&rarr;**  tlačítko. Otevře se konzola Kudu v novou kartu prohlížeče nebo okna.
1. Pomocí navigačního panelu v horní části stránky otevřete **konzolou pro ladění** a vyberte **CMD**.
1. Vyberte **LogFiles** složky.
1. Zkontrolujte **změněné** sloupce a vyberte ikonu tužky můžete upravit stdout protokolů pomocí data poslední úpravy.
1. Když se otevře soubor protokolu, zobrazí se chyba.

**Důležité!** Zakážete stdout protokolování po dokončení odstraňování potíží.

1. V Kudu **konzole diagnostiky**, vraťte se do cesty **lokality** > **wwwroot** na nich *web.config* souboru. Otevřete **web.config** soubor znovu výběrem na ikonu tužky.
1. Nastavit **stdoutLogEnabled** k `false`.
1. Vyberte **Uložit** k uložení souboru.

> [!WARNING]
> Nepodařilo se zakázat protokol stdout může vést k selhání aplikace nebo serveru. Neexistuje žádné omezení velikosti souboru protokolu nebo počet soubory protokolů vytvořené. Používejte pouze stdout protokolování pro řešení problémů při spuštění aplikace.
>
> Pro obecné protokolování v aplikaci ASP.NET Core po spuštění, použijte knihovnu protokolování, který omezuje velikost souboru protokolu a otočí protokoly. Další informace najdete v tématu [poskytovatelů třetích stran protokolování](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Běžné chyby při spuštění 

Najdete v článku [ASP.NET Core běžné chyby odkaz](xref:host-and-deploy/azure-iis-errors-reference). Většina běžné problémy, které brání spuštění aplikací jsou popsané v referenčním tématu.

## <a name="slow-or-hanging-app"></a>Pomalá nebo měny aplikace

Pokud aplikace reaguje pomalu nebo přestane reagovat na vyžádání, najdete v části [Poradce při potížích pomalé webové aplikace problémy s výkonem v Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) pro ladění pokyny.

## <a name="remote-debugging"></a>Vzdálené ladění

Najdete v následujících tématech:

* [Vzdálené ladění webových aplikací části řešení webové aplikace ve službě Azure App Service pomocí sady Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (dokumentace k Azure)
* [Vzdálené ladění ASP.NET Core ve službě IIS v Azure ve Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (dokumentace k sadě Visual Studio)

## <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) poskytuje telemetrická data z aplikací, které jsou hostované v Azure App Service, včetně protokolování a funkce generování sestav. Application Insights lze pouze sestavy chyb, které se provádějí až po aplikace spustí, když je k dispozici funkcí protokolování aplikace. Další informace najdete v tématu [Application Insights pro ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Monitorování oken

Monitorování okna zadejte alternativu potíží metody popsané výše v tomto tématu. Těchto oknech umožňuje diagnostikovat chyby 500-series.

Zkontrolujte, zda jsou nainstalovány rozšíření základní technologie ASP.NET. Pokud nejsou nainstalované rozšíření, nainstalujte je ručně:

1. V **nástroje pro vývoj** části okna, vyberte **rozšíření** okno.
1. **ASP.NET Core rozšíření** by se měla objevit v seznamu.
1. Pokud nejsou nainstalované rozšíření, vyberte **přidat** tlačítko.
1. Vyberte **ASP.NET Core rozšíření** ze seznamu.
1. Vyberte **OK** přijměte právní podmínky.
1. Vyberte **OK** na **přidat rozšíření** okno.
1. Automaticky otevírané okno informační zpráva znamená, když jsou rozšíření úspěšně nainstalovány.

Pokud není povoleno protokolování stdout, postupujte podle těchto kroků:

1. Na portálu Azure vyberte **Rozšířené nástroje** okno v **nástroje pro vývoj** oblasti. Vyberte **přejděte&rarr;**  tlačítko. Otevře se konzola Kudu v novou kartu prohlížeče nebo okna.
1. Pomocí navigačního panelu v horní části stránky otevřete **konzolou pro ladění** a vyberte **CMD**.
1. Otevření složky k cestě **lokality** > **wwwroot** a posuňte se dolů a zobrazení *web.config* souboru v dolní části seznamu.
1. Klikněte na ikonu tužky vedle *web.config* souboru.
1. Nastavit **stdoutLogEnabled** k `true` a změňte **stdoutLogFile** cestu k: `\\?\%home%\LogFiles\stdout`.
1. Vyberte **Uložit** uložte aktualizovaný *web.config* souboru.

Přejděte k aktivaci protokolování diagnostiky:

1. Na portálu Azure vyberte **protokolů diagnostiky** okno.
1. Vyberte **na** přepínač pro **protokolování aplikace (systém souborů)** a **podrobné chybové zprávy**. Vyberte **Uložit** tlačítka v horní části okna.
1. Chcete-li zahrnout trasování chybných požadavků, také známé jako protokolování se nezdařilo požadavek událostí ukládání do vyrovnávací paměti (FREB), vyberte **na** přepínač pro **trasování chybných požadavků**. 
1. Vyberte **datový proud protokolu** okno, která je uvedena bezprostředně pod **protokolů diagnostiky** okno na portálu.
1. Vytvořte žádost na aplikaci.
1. V rámci datového proudu data protokolu je uvedena příčinu chyby.

**Důležité!** Ujistěte se, že zakázat stdout protokolování po dokončení odstraňování potíží. Postupujte podle pokynů v [ASP.NET Core modulu stdout protokolu](#aspnet-core-module-stdout-log) části.

Chcete-li zobrazit protokoly trasování neúspěšných požadavků (FREB protokoly):

1. Přejděte na **Diagnostikujte a řešení problémů** okno na portálu Azure.
1. Vyberte **se nezdařilo protokoly žádosti o trasování** z **SUPPORT TOOLS** oblast na bočním panelu.

V tématu [žádosti se nezdařilo trasování části Povolit protokolování diagnostiky pro webové aplikace v Azure App Service tématu](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) a [výkon aplikace nejčastější dotazy pro webové aplikace v Azure: jak lze zapnout trasování chybných požadavků?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Další informace.

Další informace najdete v tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Nepodařilo se zakázat protokol stdout může vést k selhání aplikace nebo serveru. Neexistuje žádné omezení velikosti souboru protokolu nebo počet soubory protokolů vytvořené.
>
> Pro běžné protokolování v aplikaci ASP.NET Core, použijte knihovnu protokolování, který omezuje velikost souboru protokolu a otočí protokoly. Další informace najdete v tématu [poskytovatelů třetích stran protokolování](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Další zdroje

* [Úvod do zpracování chyb v ASP.NET Core](xref:fundamentals/error-handling)
* [Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Řešení chyb HTTP "502 Chybná brána" a "503 Služba není k dispozici" ve službě Azure web apps](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Řešení potíží s výkonem pomalé webové aplikace v Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Výkon aplikace nejčastější dotazy pro webové aplikace v Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure Web App izolovaného prostoru (aplikační služby modulu runtime provádění omezení)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 dvouminutové video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
