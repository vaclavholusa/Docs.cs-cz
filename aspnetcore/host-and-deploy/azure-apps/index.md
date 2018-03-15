---
title: "Jádro ASP.NET hostitele v Azure App Service"
author: guardrex
description: "Zjišťování řešení pro hostování aplikací ASP.NET Core v Azure App Service s odkazy na užitečné zdroje."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: cefbc27c8091a2ed1441663e3779d67aae2c64dd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Jádro ASP.NET hostitele v Azure App Service

[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computing platforma služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

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

## <a name="additional-resources"></a>Další zdroje

* [Přehled Web Apps (5 minut přehled – video)](/azure/app-service/app-service-web-overview)
* [Aplikační služba Azure: Nejlepší umístit na hostitele aplikace .NET (Přehled 55 dvouminutové video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 dvouminutové video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Přehled diagnostiky Azure App Service](/azure/app-service/app-service-diagnostics)

Aplikační služba Azure v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/). V následujících tématech se týkají základní technologii služby IIS:

* [Jádro ASP.NET hostitele v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index)
* [Úvod do modulu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referenční dokumentace k modulu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Moduly služby IIS pomocí ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Microsoft TechNet Library: Windows Server](/windows-server/windows-server-versions)
