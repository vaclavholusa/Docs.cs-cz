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
ms.openlocfilehash: 4cf81a3e269500a5108f280348fbddd172df10a0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687500"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="93e02-103">Jádro ASP.NET hostitele v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93e02-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="93e02-104">[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computing platforma služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e02-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="93e02-105">Užitečné zdroje</span><span class="sxs-lookup"><span data-stu-id="93e02-105">Useful resources</span></span>

<span data-ttu-id="93e02-106">Azure [webové aplikace dokumentace](/azure/app-service/) je domovská stránka pro aplikace Azure dokumentace, kurzy, ukázky, návody a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="93e02-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="93e02-107">Dva významné kurzy, které se vztahují k hostování aplikace ASP.NET Core jsou:</span><span class="sxs-lookup"><span data-stu-id="93e02-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="93e02-108">Rychlý úvod: Vytvoření webové aplikace ASP.NET Core v Azure</span><span class="sxs-lookup"><span data-stu-id="93e02-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="93e02-109">Pomocí sady Visual Studio k vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="93e02-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="93e02-110">Rychlý úvod: Vytvoření webové aplikace .NET Core ve službě App Service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="93e02-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="93e02-111">Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v systému Linux pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="93e02-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="93e02-112">Jsou dostupné v dokumentaci k ASP.NET Core v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="93e02-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="93e02-113">Publikování do Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93e02-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="93e02-114">Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93e02-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="93e02-115">Publikování do Azure nástroji CLI</span><span class="sxs-lookup"><span data-stu-id="93e02-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="93e02-116">Zjistěte, jak publikovat aplikaci ASP.NET Core Azure App Service pomocí příkazového řádku klienta Git.</span><span class="sxs-lookup"><span data-stu-id="93e02-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="93e02-117">Průběžné nasazování do Azure pomocí sady Visual Studio a systému Git</span><span class="sxs-lookup"><span data-stu-id="93e02-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="93e02-118">Naučte se vytvářet webové aplikace ASP.NET Core pomocí sady Visual Studio a nasadíte ho do Azure App Service pro průběžné nasazování pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="93e02-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="93e02-119">Průběžné nasazování do Azure pomocí služeb VSTS</span><span class="sxs-lookup"><span data-stu-id="93e02-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="93e02-120">Nastavení položky konfigurace sestavení pro ASP.NET Core aplikaci a potom vytvořit verzi průběžné nasazování do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="93e02-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="93e02-121">Azure izolovaného prostoru webové aplikace</span><span class="sxs-lookup"><span data-stu-id="93e02-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="93e02-122">Zjistit Azure App Service runtime provádění omezení vynucená platformou Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="93e02-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="93e02-123">Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="93e02-123">Application configuration</span></span>

<span data-ttu-id="93e02-124">S prostředím ASP.NET 2.0 jádra a novější, tři balíčky v nástroji [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) poskytují funkce automatického protokolování pro aplikace nasazené do služby Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="93e02-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="93e02-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) používá [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) zajistit ASP.NET Core lightup integraci s Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="93e02-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="93e02-126">Funkce přidané protokolování jsou poskytovány `Microsoft.AspNetCore.AzureAppServicesIntegration` balíčku.</span><span class="sxs-lookup"><span data-stu-id="93e02-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="93e02-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) provede [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) přidat zprostředkovatele protokolování diagnostiky Azure App Service v `Microsoft.Extensions.Logging.AzureAppServices` balíčku.</span><span class="sxs-lookup"><span data-stu-id="93e02-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="93e02-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) poskytuje implementace protokolovacího nástroje pro podporu protokolu streamování funkce a protokolování diagnostiky Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="93e02-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="93e02-129">Proxy server a scénáře pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="93e02-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="93e02-130">Middlewaru integrační služby IIS, který konfiguruje předávaných Middleware hlavičky a základní modul ASP.NET se tak, aby předával schématu (HTTP či HTTPS) a vzdálené IP adrese, kde tato žádost pochází.</span><span class="sxs-lookup"><span data-stu-id="93e02-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="93e02-131">Další konfigurace může být potřeba pro aplikace, které jsou hostovány za další proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="93e02-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="93e02-132">Další informace najdete v tématu [konfigurace ASP.NET Core k práci s proxy servery a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="93e02-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="93e02-133">Sledování a protokolování</span><span class="sxs-lookup"><span data-stu-id="93e02-133">Monitoring and logging</span></span>

<span data-ttu-id="93e02-134">Sledování, protokolování a informace o odstraňování potíží najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="93e02-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="93e02-135">Postupy: monitorování aplikací v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93e02-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="93e02-136">Zjistěte, jak zkontrolovat kvóty a metriky pro aplikace a plány služby App Service.</span><span class="sxs-lookup"><span data-stu-id="93e02-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="93e02-137">Povolit protokolování diagnostiky pro webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93e02-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="93e02-138">Zjistit, jak povolit a přístup k protokolování diagnostiky pro stavové kódy HTTP, neúspěšných požadavků a aktivity webového serveru.</span><span class="sxs-lookup"><span data-stu-id="93e02-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="93e02-139">Úvod do zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e02-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="93e02-140">Pochopení běžné appoaches pro zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e02-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="93e02-141">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93e02-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="93e02-142">Zjistěte, jak diagnostikovat problémy s nasazením služby Azure App Service pomocí aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e02-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="93e02-143">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e02-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="93e02-144">Najdete běžné chyby konfigurace nasazení pro aplikace, jehož hostitelem je Azure aplikace služby nebo IIS s Rady k řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="93e02-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="93e02-145">Prstenec klíč ochrany dat a nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="93e02-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="93e02-146">[Klíče ochrany dat](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zůstávají na *%HOME%\ASP.NET\DataProtection-Keys* složky.</span><span class="sxs-lookup"><span data-stu-id="93e02-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="93e02-147">Tato složka je zálohovaný díky síťového úložiště a se synchronizují napříč všechny počítače, které hostují aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e02-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="93e02-148">Klíče nejsou chráněné v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="93e02-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="93e02-149">Tato složka poskytuje prstenec klíč na všechny instance aplikace v jednom nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="93e02-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="93e02-150">Samostatné nasazovací sloty, jako je například pracovní a provozní, Nesdílejte prstenec klíč.</span><span class="sxs-lookup"><span data-stu-id="93e02-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="93e02-151">Pokud odkládací mezi sloty nasazení, jakéhokoli systému pomocí funkce Ochrana dat nebudou moci dešifrovat uložená data pomocí klíče prstenec uvnitř předchozí pozici.</span><span class="sxs-lookup"><span data-stu-id="93e02-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="93e02-152">Middleware souborů Cookie ASP.NET používá ochranu dat k ochraně jeho soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="93e02-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="93e02-153">To vede k uživatelé Podepisovaný mimo aplikaci, která používá standardní middlewaru souboru Cookie pro ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="93e02-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="93e02-154">Pro řešení prstenec klíč nezávislé na pozici použijte například poskytovatele vnější prstenec klíč:</span><span class="sxs-lookup"><span data-stu-id="93e02-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="93e02-155">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="93e02-155">Azure Blob Storage</span></span>
* <span data-ttu-id="93e02-156">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="93e02-156">Azure Key Vault</span></span>
* <span data-ttu-id="93e02-157">Úložiště SQL</span><span class="sxs-lookup"><span data-stu-id="93e02-157">SQL store</span></span>
* <span data-ttu-id="93e02-158">Mezipaměť redis</span><span class="sxs-lookup"><span data-stu-id="93e02-158">Redis cache</span></span>

<span data-ttu-id="93e02-159">Další informace najdete v tématu [klíče poskytovatelů úložiště](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="93e02-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="93e02-160">Nasazení verze preview ASP.NET Core do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93e02-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="93e02-161">Aplikace ASP.NET Core preview můžete nasadit do služby Azure App Service pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="93e02-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="93e02-162">Nainstalujte rozšíření lokality preview</span><span class="sxs-lookup"><span data-stu-id="93e02-162">Install the preview site extension</span></span>](#install-the-preview-site-extension)
* [<span data-ttu-id="93e02-163">Nasazení aplikace samostatný</span><span class="sxs-lookup"><span data-stu-id="93e02-163">Deploy the app self-contained</span></span>](#deploy-the-app-self-contained)
* [<span data-ttu-id="93e02-164">Pomocí Docker s webovými aplikacemi pro kontejnery</span><span class="sxs-lookup"><span data-stu-id="93e02-164">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="93e02-165">Pokud dojde k potížím s pomocí rozšíření lokality preview, otevřete problém na [Githubu](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="93e02-165">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="93e02-166">Nainstalujte rozšíření lokality preview</span><span class="sxs-lookup"><span data-stu-id="93e02-166">Install the preview site extension</span></span>

1. <span data-ttu-id="93e02-167">Z portálu Azure přejděte do okna služby App Service.</span><span class="sxs-lookup"><span data-stu-id="93e02-167">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="93e02-168">Vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e02-168">Select the web app.</span></span>
1. <span data-ttu-id="93e02-169">Do vyhledávacího pole zadejte "ex" nebo přejděte dolů v seznamu správy podokna na **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="93e02-169">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="93e02-170">Vyberte **nástroje pro vývoj** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="93e02-170">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="93e02-171">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="93e02-171">Select **Add**.</span></span>

   ![Azure okně aplikace s předchozích krocích](index/_static/x1.png)

1. <span data-ttu-id="93e02-173">Vyberte **rozšíření ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93e02-173">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="93e02-174">Vyberte **OK** přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="93e02-174">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="93e02-175">Vyberte **OK** k instalaci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="93e02-175">Select **OK** to install the extension.</span></span>

<span data-ttu-id="93e02-176">Po dokončení operací přidání, je nainstalována nejnovější verze preview .NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e02-176">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="93e02-177">Ověření instalace spuštěním `dotnet --info` v konzole.</span><span class="sxs-lookup"><span data-stu-id="93e02-177">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="93e02-178">Z **služby App Service** okno:</span><span class="sxs-lookup"><span data-stu-id="93e02-178">From the **App Service** blade:</span></span>

1. <span data-ttu-id="93e02-179">Zadejte "con" v poli vyhledávání nebo přejděte dolů v seznamu správy podokna na **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="93e02-179">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="93e02-180">Vyberte **nástroje pro vývoj** > **konzoly**.</span><span class="sxs-lookup"><span data-stu-id="93e02-180">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="93e02-181">Zadejte `dotnet --info` v konzole.</span><span class="sxs-lookup"><span data-stu-id="93e02-181">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="93e02-182">Pokud verze `2.1.300-preview1-008174` je na nejnovější verzi preview je následující výstup získaném spuštěním `dotnet --info` na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="93e02-182">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Azure okně aplikace s předchozích krocích](index/_static/cons.png)

<span data-ttu-id="93e02-184">Verzi ASP.NET Core viz předchozí obrázek `2.1.300-preview1-008174`, je příklad.</span><span class="sxs-lookup"><span data-stu-id="93e02-184">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="93e02-185">Nejnovější verzi preview ASP.NET Core během rozšíření lokality je nakonfigurovaný se zobrazí, když spustíte `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="93e02-185">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="93e02-186">`dotnet --info` Zobrazí cestu k rozšíření webu, na kterém je nainstalovaný ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="93e02-186">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="93e02-187">Zobrazuje aplikace běží z rozšíření webu místo výchozího *ProgramFiles* umístění.</span><span class="sxs-lookup"><span data-stu-id="93e02-187">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="93e02-188">Pokud se zobrazí *ProgramFiles*, restartujte lokalitu a spustit `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="93e02-188">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="93e02-189">**Rozšíření verze preview webu pomocí šablony ARM**</span><span class="sxs-lookup"><span data-stu-id="93e02-189">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="93e02-190">Pokud šablonu ARM se používá k vytvoření a nasazení aplikací `siteextensions` typ prostředku můžete použít k přidání rozšíření lokality do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="93e02-190">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="93e02-191">Příklad:</span><span class="sxs-lookup"><span data-stu-id="93e02-191">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="93e02-192">Nasazení aplikace samostatný</span><span class="sxs-lookup"><span data-stu-id="93e02-192">Deploy the app self-contained</span></span>

<span data-ttu-id="93e02-193">A [nezávislý aplikace](/dotnet/core/deploying/#self-contained-deployments-scd) mohou být nasazeny, představuje preview runtime v nasazení.</span><span class="sxs-lookup"><span data-stu-id="93e02-193">A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment.</span></span> <span data-ttu-id="93e02-194">Při nasazování nezávislý aplikace:</span><span class="sxs-lookup"><span data-stu-id="93e02-194">When deploying a self-contained app:</span></span>

* <span data-ttu-id="93e02-195">Lokality není nutné, abyste byli připraveni.</span><span class="sxs-lookup"><span data-stu-id="93e02-195">The site doesn't need to be prepared.</span></span>
* <span data-ttu-id="93e02-196">Aplikace musí být publikován jinak než při publikování pro nasazení závislé na framework sdílený modul runtime a hostitele na serveru.</span><span class="sxs-lookup"><span data-stu-id="93e02-196">The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.</span></span>

<span data-ttu-id="93e02-197">Samostatný aplikace jsou možnost u všech aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93e02-197">Self-contained apps are an option for all ASP.NET Core apps.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="93e02-198">Pomocí Docker s webovými aplikacemi pro kontejnery</span><span class="sxs-lookup"><span data-stu-id="93e02-198">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="93e02-199">[Úložiště Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) obsahuje nejnovější verzi preview imagí Dockeru.</span><span class="sxs-lookup"><span data-stu-id="93e02-199">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="93e02-200">Bitové kopie slouží jako základní bitová kopie.</span><span class="sxs-lookup"><span data-stu-id="93e02-200">The images can be used as a base image.</span></span> <span data-ttu-id="93e02-201">Použít bitovou kopii a nasazení do webové aplikace pro kontejnery normálně.</span><span class="sxs-lookup"><span data-stu-id="93e02-201">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93e02-202">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="93e02-202">Additional resources</span></span>

* [<span data-ttu-id="93e02-203">Přehled Web Apps (5 minut přehled – video)</span><span class="sxs-lookup"><span data-stu-id="93e02-203">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="93e02-204">Aplikační služba Azure: Nejlepší umístit na hostitele aplikace .NET (Přehled 55 dvouminutové video)</span><span class="sxs-lookup"><span data-stu-id="93e02-204">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="93e02-205">Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 dvouminutové video)</span><span class="sxs-lookup"><span data-stu-id="93e02-205">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="93e02-206">Přehled diagnostiky Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93e02-206">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="93e02-207">Aplikační služba Azure v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="93e02-207">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="93e02-208">V následujících tématech se týkají základní technologii služby IIS:</span><span class="sxs-lookup"><span data-stu-id="93e02-208">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="93e02-209">Jádro ASP.NET hostitele v systému Windows pomocí služby IIS</span><span class="sxs-lookup"><span data-stu-id="93e02-209">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="93e02-210">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e02-210">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="93e02-211">Referenční dokumentace k modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e02-211">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="93e02-212">Moduly IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e02-212">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="93e02-213">Microsoft TechNet Library: Windows Server</span><span class="sxs-lookup"><span data-stu-id="93e02-213">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
