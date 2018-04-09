---
title: Jádro ASP.NET hostitele v Azure App Service
author: guardrex
description: Zjišťování řešení pro hostování aplikací ASP.NET Core v Azure App Service s odkazy na užitečné zdroje.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Jádro ASP.NET hostitele v Azure App Service

[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computing platforma služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.

## <a name="useful-resources"></a>Užitečné zdroje

Azure [webové aplikace dokumentace](/azure/app-service/) je domovská stránka pro aplikace Azure dokumentace, kurzy, ukázky, návody a dalším prostředkům. Dva významné kurzy, které se vztahují k hostování aplikace ASP.NET Core jsou:

[Rychlý úvod: Vytvoření webové aplikace ASP.NET Core v Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Pomocí sady Visual Studio k vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v systému Windows.

[Rychlý úvod: Vytvoření webové aplikace .NET Core ve službě App Service v systému Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v systému Linux pomocí příkazového řádku.

Jsou dostupné v dokumentaci k ASP.NET Core v následujících článcích:

[Publikování do Azure pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí sady Visual Studio.

[Publikování do Azure nástroji CLI](xref:tutorials/publish-to-azure-webapp-using-cli)  
Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git.

[Průběžné nasazování do Azure pomocí sady Visual Studio a systému Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do Azure App Service pro průběžné nasazování pomocí Git.

[Průběžné nasazování do Azure pomocí služeb VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Nastavení položky konfigurace sestavení pro ASP.NET Core aplikaci a potom vytvořit verzi průběžné nasazování do služby Azure App Service.

[Azure izolovaného prostoru webové aplikace](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Zjistit Azure App Service runtime provádění omezení vynucená platformou Azure Apps.

## <a name="application-configuration"></a>Konfigurace aplikace

S prostředím ASP.NET 2.0 jádra a novější, tři balíčky v nástroji [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) poskytují funkce automatického protokolování pro aplikace nasazené do služby Azure App Service:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) používá [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) zajistit ASP.NET Core lightup integraci s Azure App Service. Funkce přidané protokolování jsou poskytovány `Microsoft.AspNetCore.AzureAppServicesIntegration` balíčku.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) provede [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) přidat zprostředkovatele protokolování diagnostiky Azure App Service v `Microsoft.Extensions.Logging.AzureAppServices` balíčku.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) poskytuje implementace protokolovacího nástroje pro podporu protokolu streamování funkce a protokolování diagnostiky Azure App Service.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy server a scénáře pro vyrovnávání zatížení

Middlewaru integrační služby IIS, který konfiguruje předávaných Middleware hlavičky a základní modul ASP.NET se tak, aby předával schématu (HTTP či HTTPS) a vzdálené IP adrese, kde tato žádost pochází. Další konfigurace může být potřeba pro aplikace, které jsou hostovány za další proxy servery a nástroje pro vyrovnávání zatížení. Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Sledování a protokolování

Sledování, protokolování a informace o odstraňování potíží najdete v následujících článcích:

[Postupy: monitorování aplikací v Azure App Service](/azure/app-service/web-sites-monitor)  
Zjistěte, jak zkontrolovat kvóty a metriky pro aplikace a plány služby App Service.

[Povolit protokolování diagnostiky pro webové aplikace v Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Zjistit, jak povolit a přístup k protokolování diagnostiky pro stavové kódy HTTP, neúspěšných požadavků a aktivity webového serveru.

[Úvod do zpracování chyb v ASP.NET Core](xref:fundamentals/error-handling)  
Pochopení běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.

[Řešení potíží s ASP.NET Core ve službě Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Zjistěte, jak diagnostikovat problémy s nasazením služby Azure App Service pomocí aplikací ASP.NET Core.

[Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Najdete běžné chyby konfigurace nasazení pro aplikace, jehož hostitelem je Azure aplikace služby nebo IIS s Rady k řešení problémů.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Prstenec klíč ochrany dat a nasazovací sloty

[Klíče ochrany dat](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zůstávají na *%HOME%\ASP.NET\DataProtection-Keys* složky. Tato složka je zálohovaný díky síťového úložiště a se synchronizují napříč všechny počítače, které hostují aplikaci. Klíče nejsou chráněné v klidovém stavu. Tato složka poskytuje prstenec klíč na všechny instance aplikace v jednom nasazovací slot. Samostatné nasazovací sloty, jako je například pracovní a provozní, Nesdílejte prstenec klíč.

Pokud odkládací mezi sloty nasazení, jakéhokoli systému pomocí funkce Ochrana dat nebudou moci dešifrovat uložená data pomocí klíče prstenec uvnitř předchozí pozici. Middleware souborů Cookie ASP.NET používá ochranu dat k ochraně jeho soubory cookie. To vede k uživatelé Podepisovaný mimo aplikaci, která používá standardní middlewaru souboru Cookie pro ASP.NET. Pro řešení prstenec klíč nezávislé na pozici použijte například poskytovatele vnější prstenec klíč:

* Azure Blob Storage
* Azure Key Vault
* Úložiště SQL
* Mezipaměť redis

Další informace najdete v tématu [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Nasazení verze preview ASP.NET Core do Azure App Service

Aplikace ASP.NET Core preview můžete nasadit do služby Azure App Service pomocí následujících postupů:

* [Instalace rozšíření lokality preview](#site-x)
* [Nasaďte aplikaci vlastní obsažené](#self)
* [Pomocí Docker s webovými aplikacemi pro kontejnery](#docker)

Pokud máte potíže pomocí rozšíření lokality preview, otevřete problém na [Githubu](https://github.com/aspnet/azureintegration/issues/new).

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a>Instalace rozšíření lokality preview

* Z portálu Azure přejděte do okna služby App Service.
* Do vyhledávacího pole zadejte "ex".
* Vyberte **rozšíření**.
* Vyberte možnost "Přidat".

![Azure okně aplikace s předchozích krocích](index/_static/x1.png)

* Vyberte **rozšíření modulu Runtime ASP.NET Core**.
* Vyberte **OK** > **OK**.

Po dokončení operací přidání je nainstalovaná nejnovější verze preview .NET Core 2.1. Instalaci můžete ověřit spuštěním `dotnet --info` v konzole. V okně App Service:

* Zadejte "con" do vyhledávacího pole.
* Vyberte **konzoly**.
* Zadejte `dotnet --info` v konzole.

![Azure okně aplikace s předchozích krocích](index/_static/cons.png)

Předchozí obrázek byl aktuální v době, kdy to byla zapsána. Může se zobrazit různé verze.

`dotnet --info` Zobrazí cestu k rozšíření webu, na kterém je nainstalovaný ve verzi Preview. Zobrazuje aplikace běží z rozšíření webu místo výchozího *ProgramFiles* umístění. Pokud se zobrazí *ProgramFiles*, restartujte lokalitu a spustit `dotnet --info`.

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a>Rozšíření lokality preview pomocí šablony ARM

Pokud používáte šablonu ARM k vytvoření a nasazení aplikace můžete použít `siteextensions` typ prostředku pro přidání rozšíření lokality do webové aplikace. Příklad:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a>Nasaďte aplikaci vlastní obsažené

Můžete nasadit [nezávislý aplikace](/dotnet/core/deploying/#self-contained-deployments-scd) , představuje preview runtime s ním při nasazení. Při nasazování aplikace Samoobslužné obsažené:

* Nemusíte připravit váš web.
* Vyžaduje, abyste pro publikování aplikace jinak než při nasazení aplikace po instalaci sady SDK na serveru.

Samostatný aplikace jsou možnost pro všechny aplikace .NET Core.

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a>Pomocí Docker s webovými aplikacemi pro kontejnery

[Úložiště Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) obsahuje nejnovější imagí Dockeru 2.1 preview. Můžete používat jako základní bitové kopie a nasazení do webové aplikace pro kontejnery běžným způsobem.

## <a name="additional-resources"></a>Další zdroje

* [Přehled Web Apps (5 minut přehled – video)](/azure/app-service/app-service-web-overview)
* [Aplikační služba Azure: Nejlepší umístit na hostitele aplikace .NET (Přehled 55 dvouminutové video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 dvouminutové video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Přehled diagnostiky Azure App Service](/azure/app-service/app-service-diagnostics)

Aplikační služba Azure v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/). V následujících tématech se týkají základní technologii služby IIS:

* [Jádro ASP.NET hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index)
* [Úvod do modulu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Moduly IIS s ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Microsoft TechNet Library: Windows Server](/windows-server/windows-server-versions)
