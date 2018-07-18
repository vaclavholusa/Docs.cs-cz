---
title: Hostitele ASP.NET Core ve službě Azure App Service
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Azure App Service s odkazy na užitečné zdroje informací.
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 83965e69249ca8196d0f226528735444936567ad
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095610"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Hostitele ASP.NET Core ve službě Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computingu platformová služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.

## <a name="useful-resources"></a>Užitečné zdroje

Azure [dokumentace k Web Apps](/azure/app-service/) je domovská stránka pro aplikace Azure dokumentace, kurzy, ukázky, návody a další prostředky. Jsou dva důležité kurzy, které se vztahují k hostování aplikací ASP.NET Core:

[Rychlý start: Vytvoření webové aplikace ASP.NET Core v Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service ve Windows pomocí sady Visual Studio.

[Rychlý start: Vytvoření webové aplikace .NET Core ve službě App Service v Linuxu](/azure/app-service/containers/quickstart-dotnetcore)  
Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v Linuxu pomocí příkazového řádku.

Jsou k dispozici v dokumentaci k ASP.NET Core v následujících článcích:

[Publikování do Azure pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Zjistěte, jak publikovat aplikace ASP.NET Core do služby Azure App Service pomocí sady Visual Studio.

[Publikování do Azure nástroji CLI](xref:tutorials/publish-to-azure-webapp-using-cli)  
Zjistěte, jak publikovat aplikace ASP.NET Core do Azure App Service pomocí klienta příkazového řádku Git.

[Průběžné nasazování do Azure pomocí sady Visual Studio a systému Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Zjistěte, jak vytvořit webovou aplikaci ASP.NET Core pomocí sady Visual Studio a nasaďte ji do služby Azure App Service pro průběžné nasazování pomocí Gitu.

[Průběžné nasazování do Azure pomocí služeb VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Nastavení sestavení CI pro aplikace ASP.NET Core a potom vytvořte vydání průběžné nasazování do služby Azure App Service.

[Azure sandboxu webové aplikace](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Objevte omezení spuštění modulu runtime služby Azure App Service vynucovaných příslušnou platformou aplikace Azure.

## <a name="application-configuration"></a>Konfigurace aplikace

S ASP.NET Core 2.0 nebo novější, tři balíčky v [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage) poskytují funkce automatického protokolování pro aplikace nasazené do služby Azure App Service:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) používá [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) poskytnout ASP.NET Core lightup integrace s Azure App Service. Poskytované funkce přidání protokolování `Microsoft.AspNetCore.AzureAppServicesIntegration` balíčku.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) spustí [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) Přidání poskytovatelů protokolování diagnostiky služby Azure App Service v `Microsoft.Extensions.Logging.AzureAppServices` balíčku.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) poskytuje implementace protokolovací nástroj pro podporu funkcí streamování protokolů a protokoly diagnostiky Azure App Service.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy server a scénáře pro nástroj pro vyrovnávání zatížení

Middleware pro integraci služby IIS, který nakonfiguruje předané Middleware záhlaví a že modul ASP.NET Core jsou konfigurovány pro předávání schématu (HTTP/HTTPS) a Vzdálená IP adresa původu žádosti. Další konfigurace může být nezbytný pro aplikací hostovaných za službou další proxy servery a nástroje pro vyrovnávání zatížení. Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitorování a protokolování

Sledování, protokolování a informace o odstraňování potíží najdete v následujících článcích:

[Postupy: sledování aplikací ve službě Azure App Service](/azure/app-service/web-sites-monitor)  
Zjistěte, jak kontrolovat kvóty a metriky pro aplikace a plány služby App Service.

[Povolení protokolování diagnostiky pro webové aplikace ve službě Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Zjistěte, jak povolit a přístup k protokolování diagnostiky pro stavové kódy HTTP, neúspěšných požadavků a aktivity webového serveru.

[Úvod do zpracování chyb v ASP.NET Core](xref:fundamentals/error-handling)  
Vysvětlení běžných appoaches pro zpracování chyb v aplikacích ASP.NET Core.

[Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Zjistěte, jak diagnostikovat problémy s nasazením služby Azure App Service s aplikací ASP.NET Core.

[Referenční informace o běžných chybách pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
V tématu běžné chyby konfigurace nasazení pro aplikace hostované pomocí Azure App Service/IIS s Poradce při potížích.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Aktualizační kanál klíč ochrany dat a sloty nasazení

[Klíče ochrany dat](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) jsou zachované *%HOME%\ASP.NET\DataProtection-Keys* složky. Tato složka je zajištěná síťového úložiště a synchronizují ve všech počítačích, který je hostitelem aplikace. Klíče nejsou chráněné v klidovém stavu. Tato složka poskytuje kanál klíč na všechny instance aplikace ve slotu jedno nasazení. Samostatné nasazovacích slotů, jako je například přípravným a produkčním prostředím, Nesdílejte klíč kanál.

Po záměně mezi sloty nasazení, jakéhokoli systému pomocí ochrany dat nebude možné dešifrovat uloženými daty pracujete pomocí aktualizační kanál, který klíč uvnitř předchozí slot. Middlewaru souboru Cookie. technologie ASP.NET používá ochranu dat k ochraně souborů cookie. To vede k uživatelům se odhlásili z aplikace, která používá standardní middlewaru souboru Cookie. technologie ASP.NET. Pro kanál klíč slotu nezávislé na řešení využívají, jako poskytovatele vnější prstenec klíč:

* Úložiště objektů Blob v Azure
* Azure Key Vault
* Úložiště SQL
* Redis cache

Další informace najdete v tématu [zprostředkovateli úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Nasazení ve verzi preview ASP.NET Core do služby Azure App Service

Aplikace ASP.NET Core ve verzi preview je možné nasadit do služby Azure App Service pomocí následujících postupů:

* [Instalace rozšíření webu ve verzi preview](#install-the-preview-site-extension)
* [Nasazení samostatné aplikace](#deploy-the-app-self-contained)
* [Použití Docker pro kontejnery s Web Apps](#use-docker-with-web-apps-for-containers)

Pokud dojde k potížím s pomocí rozšíření webu ve verzi preview, otevřete problém na [Githubu](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Instalace rozšíření webu ve verzi preview

1. Na webu Azure Portal přejděte do okna služby App Service.
1. Vyberte webovou aplikaci.
1. Do vyhledávacího pole zadejte "ex" nebo přejděte dolů v seznamu management podoken můžete vizualizaci **nástroje pro vývoj**.
1. Vyberte **nástroje pro vývoj** > **rozšíření**.
1. Vyberte **přidat**.

   ![Okno Azure aplikace nenavazuje na předchozí kroky](index/_static/x1.png)

1. Vyberte **rozšíření ASP.NET Core**.
1. Vyberte **OK** přijměte právní podmínky.
1. Vyberte **OK** nainstalovat rozšíření.

Po dokončení operací přidat, je nainstalovaná nejnovější verze preview .NET Core. Ověření instalace spuštěním `dotnet --info` v konzole. Z **služby App Service** okno:

1. Zadejte "con" v poli vyhledávání nebo přejděte dolů v seznamu management podokna na **nástroje pro vývoj**.
1. Vyberte **nástroje pro vývoj** > **konzoly**.
1. Zadejte `dotnet --info` v konzole.

Pokud verze `2.1.300-preview1-008174` na nejnovější verzi preview, je následující výstup je získaném spuštěním `dotnet --info` příkazového řádku:

![Okno Azure aplikace nenavazuje na předchozí kroky](index/_static/cons.png)

Verze technologie ASP.NET Core je vidět na předchozím obrázku `2.1.300-preview1-008174`, zde je příklad. Nejnovější verze preview ASP.NET Core v době nakonfigurovat rozšíření webu se zobrazí, když spustíte `dotnet --info`.

`dotnet --info` Zobrazí cestu k rozšíření webu, na kterém je nainstalovaný verzi Preview. Zobrazuje aplikaci se službou z rozšíření webu místo výchozího *ProgramFiles* umístění. Pokud se zobrazí *ProgramFiles*, restartujte lokalitu a spustit `dotnet --info`.

**Použití rozšíření webu ve verzi preview pomocí šablony ARM**

Pokud šablonu ARM se používá k vytvoření a nasazení aplikace, `siteextensions` typ prostředku je možné přidat rozšíření webu do webové aplikace. Příklad:

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Nasazení samostatné aplikace

A [samostatnou aplikaci](/dotnet/core/deploying/#self-contained-deployments-scd) je možné nasadit, která představuje modul runtime ve verzi preview v nasazení. Při nasazování samostatnou aplikaci:

* Webu nemusí být připravené.
* Aplikace musí zveřejnit jinak než při publikování závisí na architektuře nasazení sdílený modul runtime a hostitele na serveru.

Samostatná aplikace jsou možné u všech aplikací ASP.NET Core.

### <a name="use-docker-with-web-apps-for-containers"></a>Použití Docker pro kontejnery s Web Apps

[Docker Hubu](https://hub.docker.com/r/microsoft/aspnetcore/) obsahuje nejnovější Image Dockeru ve verzi preview. Image můžete použít jako základní image. Použít bitovou kopii a nasazení do Web Apps for Containers normálně.

## <a name="additional-resources"></a>Další zdroje

* [Přehled Web Apps (5minutové video s přehledem)](/azure/app-service/app-service-web-overview)
* [Azure App Service: Nejlepší místo k hostování aplikací .NET (55 minutu video s přehledem)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 pětiminutové video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Přehled diagnostiky služby Azure App Service](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Služba Azure App Service v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/). Následující témata se týkají základní technologie služby IIS:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Microsoft TechNet Library: Windows Server](/windows-server/windows-server-versions)
