---
title: DevOps s využitím ASP.NET Core a Azure | Monitorování a ladění
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/monitor
ms.openlocfilehash: c2fc88493aee04d7ea2781d17e808581e89d2082
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722614"
---
# <a name="monitor-and-debug"></a>Monitorování a ladění

S aplikaci nasadili a vytvořili kanál DevOps, je důležité pochopit, jak monitorovat a řešení potíží s aplikací.

V této části budete provádět následující úlohy:

* Najít základní monitorování a řešení potíží s dat na webu Azure Portal
* Zjistěte, jak Azure Monitor poskytuje hlubší pohled na metriky v rámci všech služeb Azure
* Připojení webové aplikace pomocí Application Insights pro aplikace pro profilaci
* Zapnutí protokolování a zjistíte, kde ke stažení protokolů
* Stream protokolů v reálném čase
* Zjistíte, kde nastavit výstrahy
* Další informace o vzdálené ladění Azure App Service web apps.

## <a name="basic-monitoring-and-troubleshooting"></a>Základní monitorování a řešení potíží

Aplikace služby App Service web apps je snadné sledování v reálném čase. Na webu Azure portal vykreslí metriky na snadno pochopitelné tabulky a grafy.

1. Otevřít [webu Azure portal](https://portal.azure.com)a potom přejděte na *mywebapp\<unique_number\>*  služby App Service.

1. **Přehled** karta obsahuje užitečné informace "na přehledem", včetně grafů zobrazení nedávné metriky.

    ![Přehled panelu](./media/monitoring/overview.png)

    * **Http 5xx**: počet chyb na straně serveru, obvykle výjimek v kódu ASP.NET Core.
    * **Data v**: příchozí přenosy přicházejí do webové aplikace.
    * **Data odesílaná**: Data výchozí přenos dat z vaší webové aplikace pro klienty.
    * **Požadavky**: počet žádostí HTTP.
    * **Průměrná doba odezvy**: Průměrná doba pro webovou aplikaci, aby odpovídal na požadavky HTTP.

    Několik samoobslužné nástroje pro řešení potíží a optimalizace, které také najdete na této stránce.

    ![Samoobslužné nástroje](./media/monitoring/wizards.png)

    * **Diagnostikovat a řešit problémy** je Poradce při potížích samoobslužné služby.
    * **Application Insights** je pro profilaci výkonu a chování aplikace a je popsána dále v této části.
    * **App Service Advisoru** doporučení pro optimalizaci prostředí pro aplikaci.

## <a name="advanced-monitoring"></a>Rozšířené monitorování

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) je centralizovaná služba pro monitorování všech metrik a nastavení výstrah napříč službami Azure. V rámci Azure Monitor správci podrobně sledovat výkon a identifikovat trendy. Jednotlivé služby Azure nabízí svůj vlastní [nastavení metriky](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) do Azure monitoru.

## <a name="profile-with-application-insights"></a>Profil Application insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) je služba Azure pro analýzu výkonu a stability webových aplikací a jak uživatelé používají. Data ze služby Application Insights je rozsáhlejší a podrobnější než u Azure Monitor. Data můžete zadat, vývojáři a správci s informace o klíči pro zlepšení aplikace. Application Insights je přidat do prostředku služby Azure App Service bez jakýchkoli změn kódu.

1. Otevřít [webu Azure portal](https://portal.azure.com)a potom přejděte na *mywebapp\<unique_number\>*  služby App Service.
1. Z **přehled** klikněte na tlačítko **Application Insights** dlaždici.

    ![Dlaždici služby Application Insights](./media/monitoring/app-insights.png)

1. Vyberte **vytvořit nový prostředek** přepínač. Použijte výchozí název prostředku a vyberte umístění pro prostředek služby Application Insights. Umístění se nemusí shodovat s vaší webové aplikace.

    ![Nastavení Application Insights](./media/monitoring/new-app-insights.png)

1. Pro **modul Runtime ani architektura**vyberte **ASP.NET Core**. Přijměte výchozí nastavení.
1. Vyberte **OK**. Pokud se zobrazí výzva k potvrzení, vyberte **pokračovat**.
1. Po vytvoření prostředku, klikněte na název prostředku Application Insights a přejít přímo na stránku Application Insights.

    ![Nový prostředek Application Insights je připravený](./media/monitoring/new-app-insights-done.png)

Při použití aplikace, data hromadí. Vyberte **aktualizovat** znovu načíst okno s novými daty.

![Karta Přehled Application Insights](./media/monitoring/app-insights-overview.png)

Application Insights poskytuje užitečné informace na straně serveru bez další konfigurace. Chcete-li získat co nejvíce ze služby Application Insights [instrumentaci vaší aplikace pomocí Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). Pokud správně nakonfigurovaná, které služba poskytuje začátku do konce monitorování v rámci webového serveru a prohlížeče, včetně výkonu na straně klienta. Další informace najdete v tématu [dokumentace k Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview).

## <a name="logging"></a>protokolování

Protokoly webového serveru a aplikace jsou zakázané ve výchozím nastavení ve službě Azure App Service. Povolení protokolů pomocí následujících kroků:

1. Otevřít [webu Azure portal](https://portal.azure.com)a přejděte *mywebapp\<unique_number\>*  služby App Service.
1. V nabídce na levé straně, přejděte dolů k položce **monitorování** oddílu. Vyberte **diagnostické protokoly**.

    ![Diagnostické protokoly odkaz](./media/monitoring/logging.png)

1. Zapnout **protokolování aplikace (systém souborů)**. Pokud se zobrazí výzva, klikněte na políčko pro instalaci rozšíření povolíte protokolování ve webové aplikaci app.
1. Nastavte **protokolování webového serveru** k **systému souborů**.
1. Zadejte **dobu uchování** ve dnech. Například 30.
1. Klikněte na tlačítko **Uložit**.

Pro webovou aplikaci se generují protokoly ASP.NET Core a web server (App Service). Stahování lze provádět pomocí FTP/FTPS informace zobrazené. Heslo je stejná jako přihlašovací údaje pro nasazení, vytvořili dříve v tomto průvodci. Může být protokoly [Streamovat přímo do svého místního počítače pomocí Powershellu nebo rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download). Můžete se také protokoly [zobrazit ve službě Application Insights](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Streamování protokolů

Protokoly aplikací a webového serveru můžete streamování v reálném čase prostřednictvím portálu.

1. Otevřít [webu Azure portal](https://portal.azure.com)a přejděte *mywebapp\<unique_number\>*  služby App Service.
1. V nabídce na levé straně, přejděte dolů k položce **monitorování** a vyberte **stream protokolů**.

    ![Propojení stream protokolů](./media/monitoring/log-stream.png)

Můžete se také protokoly [Streamovat prostřednictvím Azure Powershellu nebo rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), včetně Cloud Shell.

## <a name="alerts"></a>Upozornění

Azure Monitor poskytuje také [upozornění v reálném čase](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal) na základě metrik, správy událostí a jiných kritérií.

> *Poznámka: Aktuálně upozornění na metriky webové aplikace je k dispozici pouze ve službě upozornění (klasická).*

[Upozornění (klasická) služby](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) najdete ve službě Azure Monitor nebo v části **monitorování** část nastavení služby App Service.

![Upozornění (klasická) odkaz](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Živé ladění

Azure App Service může být [vzdáleně ladit pomocí sady Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) když protokoly neposkytují dostatek informací. Ale vzdálené ladění vyžaduje, aby aplikace kompilována se symboly ladění. Ladění nelze provádět v produkčním prostředí, s výjimkou jako poslední možnost.

## <a name="conclusion"></a>Závěr

V této části jste dokončili následující úlohy:

* Najít základní monitorování a řešení potíží s dat na webu Azure Portal
* Zjistěte, jak Azure Monitor poskytuje hlubší pohled na metriky v rámci všech služeb Azure
* Připojení webové aplikace pomocí Application Insights pro aplikace pro profilaci
* Zapnutí protokolování a zjistíte, kde ke stažení protokolů
* Stream protokolů v reálném čase
* Zjistíte, kde nastavit výstrahy
* Další informace o vzdálené ladění Azure App Service web apps.

## <a name="additional-reading"></a>Další čtení

* [Řešení potíží s ASP.NET Core ve službě Azure App Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-apps/troubleshoot)
* [Referenční informace o běžných chybách pro Azure App Service a IIS s ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-iis-errors-reference)
* [Monitorování výkonu webových aplikací Azure pomocí Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps)
* [Povolení protokolování diagnostiky pro webové aplikace ve službě Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log)
* [Řešení potíží s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Vytvoření klasického upozornění metrik ve službě Azure Monitor pro Azure services – Azure portal](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)
