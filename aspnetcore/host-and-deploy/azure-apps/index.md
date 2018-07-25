---
title: Hostitele ASP.NET Core ve službě Azure App Service
author: guardrex
description: Zjistěte, jak hostovat aplikace ASP.NET Core ve službě Azure App Service s odkazy na užitečné zdroje informací.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: ece61a3e362ec5e2ff8f415351a0f9257fc72098
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228608"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="dd095-103">Hostitele ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dd095-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="dd095-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computingu platformová služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd095-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="dd095-105">Užitečné zdroje</span><span class="sxs-lookup"><span data-stu-id="dd095-105">Useful resources</span></span>

<span data-ttu-id="dd095-106">Azure [dokumentace k Web Apps](/azure/app-service/) je domovská stránka pro aplikace Azure dokumentace, kurzy, ukázky, návody a další prostředky.</span><span class="sxs-lookup"><span data-stu-id="dd095-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="dd095-107">Jsou dva důležité kurzy, které se vztahují k hostování aplikací ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="dd095-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="dd095-108">Rychlý start: Vytvoření webové aplikace ASP.NET Core v Azure</span><span class="sxs-lookup"><span data-stu-id="dd095-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="dd095-109">Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service ve Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd095-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="dd095-110">Rychlý start: Vytvoření webové aplikace .NET Core ve službě App Service v Linuxu</span><span class="sxs-lookup"><span data-stu-id="dd095-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="dd095-111">Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v Linuxu pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="dd095-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="dd095-112">Jsou k dispozici v dokumentaci k ASP.NET Core v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="dd095-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="dd095-113">Publikování do Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd095-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="dd095-114">Zjistěte, jak publikovat aplikace ASP.NET Core do služby Azure App Service pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd095-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="dd095-115">Publikování do Azure nástroji CLI</span><span class="sxs-lookup"><span data-stu-id="dd095-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="dd095-116">Zjistěte, jak publikovat aplikace ASP.NET Core do Azure App Service pomocí klienta příkazového řádku Git.</span><span class="sxs-lookup"><span data-stu-id="dd095-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="dd095-117">Průběžné nasazování do Azure pomocí sady Visual Studio a systému Git</span><span class="sxs-lookup"><span data-stu-id="dd095-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="dd095-118">Zjistěte, jak vytvořit webovou aplikaci ASP.NET Core pomocí sady Visual Studio a nasaďte ji do služby Azure App Service pro průběžné nasazování pomocí Gitu.</span><span class="sxs-lookup"><span data-stu-id="dd095-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="dd095-119">Průběžné nasazování do Azure pomocí služeb VSTS</span><span class="sxs-lookup"><span data-stu-id="dd095-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="dd095-120">Nastavení sestavení CI pro aplikace ASP.NET Core a potom vytvořte vydání průběžné nasazování do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dd095-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="dd095-121">Azure sandboxu webové aplikace</span><span class="sxs-lookup"><span data-stu-id="dd095-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="dd095-122">Objevte omezení spuštění modulu runtime služby Azure App Service vynucovaných příslušnou platformou aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="dd095-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="dd095-123">Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="dd095-123">Application configuration</span></span>

<span data-ttu-id="dd095-124">V technologii ASP.NET Core 2.0 nebo novější následující balíčky NuGet poskytují funkce automatického protokolování pro aplikace nasazené do služby Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="dd095-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="dd095-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) používá [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) k zajištění integrace světla nahoru ASP.NET Core pomocí služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dd095-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="dd095-126">Poskytované funkce přidání protokolování `Microsoft.AspNetCore.AzureAppServicesIntegration` balíčku.</span><span class="sxs-lookup"><span data-stu-id="dd095-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="dd095-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) spustí [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) Přidání poskytovatelů protokolování diagnostiky služby Azure App Service v `Microsoft.Extensions.Logging.AzureAppServices` balíčku.</span><span class="sxs-lookup"><span data-stu-id="dd095-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="dd095-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) poskytuje implementace protokolovací nástroj pro podporu funkcí streamování protokolů a protokoly diagnostiky Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dd095-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="dd095-129">Pokud cílí na .NET Core a odkazování [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage), balíčky jsou již zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="dd095-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="dd095-130">Chybí balíčky z novější [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dd095-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="dd095-131">Pokud cílí na rozhraní .NET Framework nebo odkazující `Microsoft.AspNetCore.App` Microsoft.aspnetcore.all, odkazují na balíčky jednotlivé protokolování.</span><span class="sxs-lookup"><span data-stu-id="dd095-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="dd095-132">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="dd095-132">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="dd095-133">Middleware pro integraci služby IIS, který nakonfiguruje předané Middleware záhlaví a že modul ASP.NET Core jsou konfigurovány pro předávání schématu (HTTP/HTTPS) a Vzdálená IP adresa původu žádosti.</span><span class="sxs-lookup"><span data-stu-id="dd095-133">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="dd095-134">Další konfigurace může být nezbytný pro aplikací hostovaných za službou další proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="dd095-134">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="dd095-135">Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="dd095-135">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="dd095-136">Monitorování a protokolování</span><span class="sxs-lookup"><span data-stu-id="dd095-136">Monitoring and logging</span></span>

<span data-ttu-id="dd095-137">Sledování, protokolování a informace o odstraňování potíží najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="dd095-137">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="dd095-138">Postupy: sledování aplikací ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dd095-138">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="dd095-139">Zjistěte, jak kontrolovat kvóty a metriky pro aplikace a plány služby App Service.</span><span class="sxs-lookup"><span data-stu-id="dd095-139">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="dd095-140">Povolení protokolování diagnostiky pro webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dd095-140">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="dd095-141">Zjistěte, jak povolit a přístup k protokolování diagnostiky pro stavové kódy HTTP, neúspěšných požadavků a aktivity webového serveru.</span><span class="sxs-lookup"><span data-stu-id="dd095-141">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="dd095-142">Úvod do zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd095-142">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="dd095-143">Seznamte se s běžné přístupy k zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd095-143">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="dd095-144">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dd095-144">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="dd095-145">Zjistěte, jak diagnostikovat problémy s nasazením služby Azure App Service s aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd095-145">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="dd095-146">Referenční informace o běžných chybách pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd095-146">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="dd095-147">V tématu běžné chyby konfigurace nasazení pro aplikace hostované pomocí Azure App Service/IIS s Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="dd095-147">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="dd095-148">Aktualizační kanál klíč ochrany dat a sloty nasazení</span><span class="sxs-lookup"><span data-stu-id="dd095-148">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="dd095-149">[Klíče ochrany dat](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) jsou zachované *%HOME%\ASP.NET\DataProtection-Keys* složky.</span><span class="sxs-lookup"><span data-stu-id="dd095-149">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="dd095-150">Tato složka je zajištěná síťového úložiště a synchronizují ve všech počítačích, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd095-150">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="dd095-151">Klíče nejsou chráněné v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="dd095-151">Keys aren't protected at rest.</span></span> <span data-ttu-id="dd095-152">Tato složka poskytuje kanál klíč na všechny instance aplikace ve slotu jedno nasazení.</span><span class="sxs-lookup"><span data-stu-id="dd095-152">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="dd095-153">Samostatné nasazovacích slotů, jako je například přípravným a produkčním prostředím, Nesdílejte klíč kanál.</span><span class="sxs-lookup"><span data-stu-id="dd095-153">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="dd095-154">Po záměně mezi sloty nasazení, jakéhokoli systému pomocí ochrany dat nebude možné dešifrovat uloženými daty pracujete pomocí aktualizační kanál, který klíč uvnitř předchozí slot.</span><span class="sxs-lookup"><span data-stu-id="dd095-154">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="dd095-155">Middlewaru souboru Cookie. technologie ASP.NET používá ochranu dat k ochraně souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="dd095-155">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="dd095-156">To vede k uživatelům se odhlásili z aplikace, která používá standardní middlewaru souboru Cookie. technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dd095-156">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="dd095-157">Pro kanál klíč slotu nezávislé na řešení využívají, jako poskytovatele vnější prstenec klíč:</span><span class="sxs-lookup"><span data-stu-id="dd095-157">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="dd095-158">Úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="dd095-158">Azure Blob Storage</span></span>
* <span data-ttu-id="dd095-159">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dd095-159">Azure Key Vault</span></span>
* <span data-ttu-id="dd095-160">Úložiště SQL</span><span class="sxs-lookup"><span data-stu-id="dd095-160">SQL store</span></span>
* <span data-ttu-id="dd095-161">Redis cache</span><span class="sxs-lookup"><span data-stu-id="dd095-161">Redis cache</span></span>

<span data-ttu-id="dd095-162">Další informace najdete v tématu [zprostředkovateli úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="dd095-162">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="dd095-163">Nasazení ve verzi preview ASP.NET Core do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dd095-163">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="dd095-164">Aplikace ASP.NET Core ve verzi preview je možné nasadit do služby Azure App Service pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="dd095-164">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="dd095-165">Instalace rozšíření webu ve verzi preview</span><span class="sxs-lookup"><span data-stu-id="dd095-165">Install the preview site extension</span></span>](#install-the-preview-site-extension)
* [<span data-ttu-id="dd095-166">Nasazení samostatné aplikace</span><span class="sxs-lookup"><span data-stu-id="dd095-166">Deploy the app self-contained</span></span>](#deploy-the-app-self-contained)
* [<span data-ttu-id="dd095-167">Použití Docker pro kontejnery s Web Apps</span><span class="sxs-lookup"><span data-stu-id="dd095-167">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="dd095-168">Pokud dojde k potížím s pomocí rozšíření webu ve verzi preview, otevřete problém na [Githubu](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="dd095-168">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="dd095-169">Instalace rozšíření webu ve verzi preview</span><span class="sxs-lookup"><span data-stu-id="dd095-169">Install the preview site extension</span></span>

1. <span data-ttu-id="dd095-170">Na webu Azure Portal přejděte do okna služby App Service.</span><span class="sxs-lookup"><span data-stu-id="dd095-170">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="dd095-171">Vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dd095-171">Select the web app.</span></span>
1. <span data-ttu-id="dd095-172">Do vyhledávacího pole zadejte "ex" nebo přejděte dolů v seznamu management podoken můžete vizualizaci **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="dd095-172">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="dd095-173">Vyberte **nástroje pro vývoj** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="dd095-173">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="dd095-174">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="dd095-174">Select **Add**.</span></span>

   ![Okno Azure aplikace nenavazuje na předchozí kroky](index/_static/x1.png)

1. <span data-ttu-id="dd095-176">Vyberte **rozšíření ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dd095-176">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="dd095-177">Vyberte **OK** přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="dd095-177">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="dd095-178">Vyberte **OK** nainstalovat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="dd095-178">Select **OK** to install the extension.</span></span>

<span data-ttu-id="dd095-179">Po dokončení operací přidat, je nainstalovaná nejnovější verze preview .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd095-179">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="dd095-180">Ověření instalace spuštěním `dotnet --info` v konzole.</span><span class="sxs-lookup"><span data-stu-id="dd095-180">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="dd095-181">Z **služby App Service** okno:</span><span class="sxs-lookup"><span data-stu-id="dd095-181">From the **App Service** blade:</span></span>

1. <span data-ttu-id="dd095-182">Zadejte "con" v poli vyhledávání nebo přejděte dolů v seznamu management podokna na **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="dd095-182">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="dd095-183">Vyberte **nástroje pro vývoj** > **konzoly**.</span><span class="sxs-lookup"><span data-stu-id="dd095-183">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="dd095-184">Zadejte `dotnet --info` v konzole.</span><span class="sxs-lookup"><span data-stu-id="dd095-184">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="dd095-185">Pokud verze `2.1.300-preview1-008174` na nejnovější verzi preview, je následující výstup je získaném spuštěním `dotnet --info` příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="dd095-185">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Okno Azure aplikace nenavazuje na předchozí kroky](index/_static/cons.png)

<span data-ttu-id="dd095-187">Verze technologie ASP.NET Core je vidět na předchozím obrázku `2.1.300-preview1-008174`, zde je příklad.</span><span class="sxs-lookup"><span data-stu-id="dd095-187">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="dd095-188">Nejnovější verze preview ASP.NET Core v době nakonfigurovat rozšíření webu se zobrazí, když spustíte `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="dd095-188">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="dd095-189">`dotnet --info` Zobrazí cestu k rozšíření webu, na kterém je nainstalovaný verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="dd095-189">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="dd095-190">Zobrazuje aplikaci se službou z rozšíření webu místo výchozího *ProgramFiles* umístění.</span><span class="sxs-lookup"><span data-stu-id="dd095-190">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="dd095-191">Pokud se zobrazí *ProgramFiles*, restartujte lokalitu a spustit `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="dd095-191">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="dd095-192">**Použití rozšíření webu ve verzi preview pomocí šablony ARM**</span><span class="sxs-lookup"><span data-stu-id="dd095-192">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="dd095-193">Pokud šablonu ARM se používá k vytvoření a nasazení aplikace, `siteextensions` typ prostředku je možné přidat rozšíření webu do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd095-193">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="dd095-194">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dd095-194">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="dd095-195">Nasazení samostatné aplikace</span><span class="sxs-lookup"><span data-stu-id="dd095-195">Deploy the app self-contained</span></span>

<span data-ttu-id="dd095-196">A [samostatnou aplikaci](/dotnet/core/deploying/#self-contained-deployments-scd) je možné nasadit, která představuje modul runtime ve verzi preview v nasazení.</span><span class="sxs-lookup"><span data-stu-id="dd095-196">A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment.</span></span> <span data-ttu-id="dd095-197">Při nasazování samostatnou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="dd095-197">When deploying a self-contained app:</span></span>

* <span data-ttu-id="dd095-198">Webu nemusí být připravené.</span><span class="sxs-lookup"><span data-stu-id="dd095-198">The site doesn't need to be prepared.</span></span>
* <span data-ttu-id="dd095-199">Aplikace musí zveřejnit jinak než při publikování závisí na architektuře nasazení sdílený modul runtime a hostitele na serveru.</span><span class="sxs-lookup"><span data-stu-id="dd095-199">The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.</span></span>

<span data-ttu-id="dd095-200">Samostatná aplikace jsou možné u všech aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd095-200">Self-contained apps are an option for all ASP.NET Core apps.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="dd095-201">Použití Docker pro kontejnery s Web Apps</span><span class="sxs-lookup"><span data-stu-id="dd095-201">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="dd095-202">[Docker Hubu](https://hub.docker.com/r/microsoft/aspnetcore/) obsahuje nejnovější Image Dockeru ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="dd095-202">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="dd095-203">Image můžete použít jako základní image.</span><span class="sxs-lookup"><span data-stu-id="dd095-203">The images can be used as a base image.</span></span> <span data-ttu-id="dd095-204">Použít bitovou kopii a nasazení do Web Apps for Containers normálně.</span><span class="sxs-lookup"><span data-stu-id="dd095-204">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd095-205">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dd095-205">Additional resources</span></span>

* [<span data-ttu-id="dd095-206">Přehled Web Apps (5minutové video s přehledem)</span><span class="sxs-lookup"><span data-stu-id="dd095-206">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="dd095-207">Azure App Service: Nejlepší místo k hostování aplikací .NET (55 minutu video s přehledem)</span><span class="sxs-lookup"><span data-stu-id="dd095-207">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="dd095-208">Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 pětiminutové video)</span><span class="sxs-lookup"><span data-stu-id="dd095-208">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="dd095-209">Přehled diagnostiky služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dd095-209">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="dd095-210">Služba Azure App Service v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="dd095-210">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="dd095-211">Následující témata se týkají základní technologie služby IIS:</span><span class="sxs-lookup"><span data-stu-id="dd095-211">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="dd095-212">Microsoft TechNet Library: Windows Server</span><span class="sxs-lookup"><span data-stu-id="dd095-212">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
