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
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="5342b-103">Jádro ASP.NET hostitele v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5342b-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="5342b-104">[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computing platforma služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5342b-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a><span data-ttu-id="5342b-105">Užitečné zdroje</span><span class="sxs-lookup"><span data-stu-id="5342b-105">Useful resources</span></span>

<span data-ttu-id="5342b-106">Azure [webové aplikace dokumentace](/azure/app-service/) je domovská stránka pro aplikace Azure dokumentace, kurzy, ukázky, návody a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="5342b-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="5342b-107">Dva významné kurzy, které se vztahují k hostování aplikace ASP.NET Core jsou:</span><span class="sxs-lookup"><span data-stu-id="5342b-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="5342b-108">Rychlý úvod: Vytvoření webové aplikace ASP.NET Core v Azure</span><span class="sxs-lookup"><span data-stu-id="5342b-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="5342b-109">Pomocí sady Visual Studio k vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5342b-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="5342b-110">Rychlý úvod: Vytvoření webové aplikace .NET Core ve službě App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5342b-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="5342b-111">Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v systému Linux pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5342b-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="5342b-112">Jsou dostupné v dokumentaci k ASP.NET Core v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="5342b-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="5342b-113">Publikování do Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5342b-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="5342b-114">Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5342b-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="5342b-115">Publikování do Azure nástroji CLI</span><span class="sxs-lookup"><span data-stu-id="5342b-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="5342b-116">Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git.</span><span class="sxs-lookup"><span data-stu-id="5342b-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="5342b-117">Průběžné nasazování do Azure pomocí sady Visual Studio a systému Git</span><span class="sxs-lookup"><span data-stu-id="5342b-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="5342b-118">Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do Azure App Service pro průběžné nasazování pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="5342b-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="5342b-119">Průběžné nasazování do Azure pomocí služeb VSTS</span><span class="sxs-lookup"><span data-stu-id="5342b-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="5342b-120">Nastavení položky konfigurace sestavení pro ASP.NET Core aplikaci a potom vytvořit verzi průběžné nasazování do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5342b-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="5342b-121">Azure izolovaného prostoru webové aplikace</span><span class="sxs-lookup"><span data-stu-id="5342b-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="5342b-122">Zjistit Azure App Service runtime provádění omezení vynucená platformou Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="5342b-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="5342b-123">Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="5342b-123">Application configuration</span></span>

<span data-ttu-id="5342b-124">S prostředím ASP.NET 2.0 jádra a novější, tři balíčky v nástroji [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) poskytují funkce automatického protokolování pro aplikace nasazené do služby Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="5342b-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="5342b-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) používá [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) zajistit ASP.NET Core lightup integraci s Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5342b-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="5342b-126">Funkce přidané protokolování jsou poskytovány `Microsoft.AspNetCore.AzureAppServicesIntegration` balíčku.</span><span class="sxs-lookup"><span data-stu-id="5342b-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="5342b-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) provede [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) přidat zprostředkovatele protokolování diagnostiky Azure App Service v `Microsoft.Extensions.Logging.AzureAppServices` balíčku.</span><span class="sxs-lookup"><span data-stu-id="5342b-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="5342b-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) poskytuje implementace protokolovacího nástroje pro podporu protokolu streamování funkce a protokolování diagnostiky Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5342b-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="5342b-129">Sledování a protokolování</span><span class="sxs-lookup"><span data-stu-id="5342b-129">Monitoring and logging</span></span>

<span data-ttu-id="5342b-130">Sledování, protokolování a informace o odstraňování potíží najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="5342b-130">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="5342b-131">Postupy: monitorování aplikací v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5342b-131">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="5342b-132">Zjistěte, jak zkontrolovat kvóty a metriky pro aplikace a plány služby App Service.</span><span class="sxs-lookup"><span data-stu-id="5342b-132">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="5342b-133">Povolit protokolování diagnostiky pro webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5342b-133">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="5342b-134">Zjistit, jak povolit a přístup k protokolování diagnostiky pro stavové kódy HTTP, neúspěšných požadavků a aktivity webového serveru.</span><span class="sxs-lookup"><span data-stu-id="5342b-134">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="5342b-135">Úvod do zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5342b-135">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="5342b-136">Pochopení běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5342b-136">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="5342b-137">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5342b-137">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="5342b-138">Zjistěte, jak diagnostikovat problémy s nasazením služby Azure App Service pomocí aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5342b-138">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="5342b-139">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5342b-139">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="5342b-140">Najdete běžné chyby konfigurace nasazení pro aplikace, jehož hostitelem je Azure aplikace služby nebo IIS s Rady k řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="5342b-140">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="5342b-141">Prstenec klíč ochrany dat a nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="5342b-141">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="5342b-142">[Klíče ochrany dat](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zůstávají na *%HOME%\ASP.NET\DataProtection-Keys* složky.</span><span class="sxs-lookup"><span data-stu-id="5342b-142">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="5342b-143">Tato složka je zálohovaný díky síťového úložiště a se synchronizují napříč všechny počítače, které hostují aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5342b-143">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="5342b-144">Klíče nejsou chráněné v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="5342b-144">Keys aren't protected at rest.</span></span> <span data-ttu-id="5342b-145">Tato složka poskytuje prstenec klíč na všechny instance aplikace v jednom nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="5342b-145">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="5342b-146">Samostatné nasazovací sloty, jako je například pracovní a provozní, Nesdílejte prstenec klíč.</span><span class="sxs-lookup"><span data-stu-id="5342b-146">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="5342b-147">Pokud odkládací mezi sloty nasazení, jakéhokoli systému pomocí funkce Ochrana dat nebudou moci dešifrovat uložená data pomocí klíče prstenec uvnitř předchozí pozici.</span><span class="sxs-lookup"><span data-stu-id="5342b-147">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="5342b-148">Middleware souborů Cookie ASP.NET používá ochranu dat k ochraně jeho soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="5342b-148">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="5342b-149">To vede k uživatelé Podepisovaný mimo aplikaci, která používá standardní middlewaru souboru Cookie pro ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5342b-149">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="5342b-150">Pro řešení prstenec klíč nezávislé na pozici použijte například poskytovatele vnější prstenec klíč:</span><span class="sxs-lookup"><span data-stu-id="5342b-150">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="5342b-151">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5342b-151">Azure Blob Storage</span></span>
* <span data-ttu-id="5342b-152">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5342b-152">Azure Key Vault</span></span>
* <span data-ttu-id="5342b-153">Úložiště SQL</span><span class="sxs-lookup"><span data-stu-id="5342b-153">SQL store</span></span>
* <span data-ttu-id="5342b-154">Mezipaměť redis</span><span class="sxs-lookup"><span data-stu-id="5342b-154">Redis cache</span></span>

<span data-ttu-id="5342b-155">Další informace najdete v tématu [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="5342b-155">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5342b-156">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5342b-156">Additional resources</span></span>

* [<span data-ttu-id="5342b-157">Přehled Web Apps (5 minut přehled – video)</span><span class="sxs-lookup"><span data-stu-id="5342b-157">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="5342b-158">Aplikační služba Azure: Nejlepší umístit na hostitele aplikace .NET (Přehled 55 dvouminutové video)</span><span class="sxs-lookup"><span data-stu-id="5342b-158">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="5342b-159">Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 dvouminutové video)</span><span class="sxs-lookup"><span data-stu-id="5342b-159">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="5342b-160">Přehled diagnostiky Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5342b-160">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="5342b-161">Aplikační služba Azure v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="5342b-161">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="5342b-162">V následujících tématech se týkají základní technologii služby IIS:</span><span class="sxs-lookup"><span data-stu-id="5342b-162">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="5342b-163">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="5342b-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="5342b-164">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5342b-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="5342b-165">Referenční dokumentace k modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5342b-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="5342b-166">Moduly služby IIS pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5342b-166">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="5342b-167">Microsoft TechNet Library: Windows Server</span><span class="sxs-lookup"><span data-stu-id="5342b-167">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
