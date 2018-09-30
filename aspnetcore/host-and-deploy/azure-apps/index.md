---
title: Nasazení aplikace ASP.NET Core do Azure App Service
author: guardrex
description: Tento článek obsahuje odkazy na hostiteli Azure a nasazení prostředků.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: a70ca46da633161083c1860aca92242d47d5e7c7
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454755"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="0c8a2-103">Nasazení aplikace ASP.NET Core do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0c8a2-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="0c8a2-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) je [Microsoft cloud computingu platformová služba](https://azure.microsoft.com/) pro hostování webových aplikací, včetně ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="0c8a2-105">Užitečné zdroje</span><span class="sxs-lookup"><span data-stu-id="0c8a2-105">Useful resources</span></span>

<span data-ttu-id="0c8a2-106">Azure [dokumentace k Web Apps](/azure/app-service/) je domovská stránka pro aplikace Azure dokumentace, kurzy, ukázky, návody a další prostředky.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="0c8a2-107">Jsou dva důležité kurzy, které se vztahují k hostování aplikací ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="0c8a2-108">Rychlý start: Vytvoření webové aplikace ASP.NET Core v Azure</span><span class="sxs-lookup"><span data-stu-id="0c8a2-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="0c8a2-109">Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service ve Windows pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="0c8a2-110">Rychlý start: Vytvoření webové aplikace .NET Core ve službě App Service v Linuxu</span><span class="sxs-lookup"><span data-stu-id="0c8a2-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="0c8a2-111">Vytvoření a nasazení webové aplikace ASP.NET Core do služby Azure App Service v Linuxu pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="0c8a2-112">Jsou k dispozici v dokumentaci k ASP.NET Core v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="0c8a2-113">Publikování do Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c8a2-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="0c8a2-114">Zjistěte, jak publikovat aplikace ASP.NET Core do služby Azure App Service pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="0c8a2-115">Publikování do Azure nástroji CLI</span><span class="sxs-lookup"><span data-stu-id="0c8a2-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="0c8a2-116">Zjistěte, jak publikovat aplikace ASP.NET Core do Azure App Service pomocí klienta příkazového řádku Git.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="0c8a2-117">Průběžné nasazování do Azure pomocí sady Visual Studio a systému Git</span><span class="sxs-lookup"><span data-stu-id="0c8a2-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="0c8a2-118">Zjistěte, jak vytvořit webovou aplikaci ASP.NET Core pomocí sady Visual Studio a nasaďte ji do služby Azure App Service pro průběžné nasazování pomocí Gitu.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="0c8a2-119">Vytvořit svůj první kanál s kanály Azure</span><span class="sxs-lookup"><span data-stu-id="0c8a2-119">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="0c8a2-120">Nastavení sestavení CI pro aplikace ASP.NET Core a potom vytvořte vydání průběžné nasazování do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="0c8a2-121">Azure sandboxu webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0c8a2-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="0c8a2-122">Objevte omezení spuštění modulu runtime služby Azure App Service vynucovaných příslušnou platformou aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="0c8a2-123">Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="0c8a2-123">Application configuration</span></span>

<span data-ttu-id="0c8a2-124">V technologii ASP.NET Core 2.0 nebo novější následující balíčky NuGet poskytují funkce automatického protokolování pro aplikace nasazené do služby Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="0c8a2-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) používá [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) k zajištění integrace světla nahoru ASP.NET Core pomocí služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="0c8a2-126">Poskytované funkce přidání protokolování `Microsoft.AspNetCore.AzureAppServicesIntegration` balíčku.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="0c8a2-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) spustí [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) Přidání poskytovatelů protokolování diagnostiky služby Azure App Service v `Microsoft.Extensions.Logging.AzureAppServices` balíčku.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="0c8a2-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) poskytuje implementace protokolovací nástroj pro podporu funkcí streamování protokolů a protokoly diagnostiky Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="0c8a2-129">Pokud cílí na .NET Core a odkazování [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage), balíčky jsou již zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="0c8a2-130">Chybí balíčky z novější [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="0c8a2-131">Pokud cílí na rozhraní .NET Framework nebo odkazující `Microsoft.AspNetCore.App` Microsoft.aspnetcore.all, odkazují na balíčky jednotlivé protokolování.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="0c8a2-132">Přepsat konfiguraci aplikace pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0c8a2-132">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="0c8a2-133">**Nastavení aplikace** oblasti **nastavení aplikace** okno umožňuje nastavit proměnné prostředí pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-133">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="0c8a2-134">Mohou být spotřebovány proměnné prostředí [poskytovatele konfigurace proměnných prostředí](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-134">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="0c8a2-135">Pokud aplikace používá [webového hostitele](xref:fundamentals/host/web-host) a sestavení hostitele pomocí [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), použít proměnné prostředí, které konfigurace hostitele `ASPNETCORE_` předponu.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="0c8a2-136">Další informace najdete v tématu <xref:fundamentals/host/web-host> a [poskytovatele konfigurace proměnných prostředí](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="0c8a2-137">Pokud aplikace používá [obecný hostitele](xref:fundamentals/host/generic-host), proměnné prostředí nejsou načtené do konfigurace vaší aplikace ve výchozím nastavení a poskytovatel konfigurace je třeba přidat pomocí vývojáře.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="0c8a2-138">Vývojář Určuje předponu proměnné prostředí po přidání zprostředkovatele konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="0c8a2-139">Další informace najdete v tématu <xref:fundamentals/host/generic-host> a [poskytovatele konfigurace proměnných prostředí](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0c8a2-140">Proxy server a scénáře pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0c8a2-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0c8a2-141">Middleware pro integraci služby IIS, který nakonfiguruje předané Middleware záhlaví a že modul ASP.NET Core jsou konfigurovány pro předávání schématu (HTTP/HTTPS) a Vzdálená IP adresa původu žádosti.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="0c8a2-142">Další konfigurace může být nezbytný pro aplikací hostovaných za službou další proxy servery a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="0c8a2-143">Další informace najdete v tématu [konfigurace ASP.NET Core práci se servery proxy a nástroje pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="0c8a2-144">Monitorování a protokolování</span><span class="sxs-lookup"><span data-stu-id="0c8a2-144">Monitoring and logging</span></span>

<span data-ttu-id="0c8a2-145">Sledování, protokolování a informace o odstraňování potíží najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="0c8a2-146">Postupy: sledování aplikací ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0c8a2-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="0c8a2-147">Zjistěte, jak kontrolovat kvóty a metriky pro aplikace a plány služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="0c8a2-148">Povolení protokolování diagnostiky pro webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0c8a2-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="0c8a2-149">Zjistěte, jak povolit a přístup k protokolování diagnostiky pro stavové kódy HTTP, neúspěšných požadavků a aktivity webového serveru.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="0c8a2-150">Úvod do zpracování chyb v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c8a2-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="0c8a2-151">Seznamte se s běžné přístupy k zpracování chyb v aplikacích ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="0c8a2-152">Řešení potíží s ASP.NET Core ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0c8a2-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="0c8a2-153">Zjistěte, jak diagnostikovat problémy s nasazením služby Azure App Service s aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="0c8a2-154">Referenční informace o běžných chybách pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c8a2-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="0c8a2-155">V tématu běžné chyby konfigurace nasazení pro aplikace hostované pomocí Azure App Service/IIS s Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="0c8a2-156">Aktualizační kanál klíč ochrany dat a sloty nasazení</span><span class="sxs-lookup"><span data-stu-id="0c8a2-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="0c8a2-157">[Klíče ochrany dat](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) jsou zachované *%HOME%\ASP.NET\DataProtection-Keys* složky.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="0c8a2-158">Tato složka je zajištěná síťového úložiště a synchronizují ve všech počítačích, který je hostitelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="0c8a2-159">Klíče nejsou chráněné v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="0c8a2-160">Tato složka poskytuje kanál klíč na všechny instance aplikace ve slotu jedno nasazení.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="0c8a2-161">Samostatné nasazovacích slotů, jako je například přípravným a produkčním prostředím, Nesdílejte klíč kanál.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="0c8a2-162">Po záměně mezi sloty nasazení, jakéhokoli systému pomocí ochrany dat nebude možné dešifrovat uloženými daty pracujete pomocí aktualizační kanál, který klíč uvnitř předchozí slot.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="0c8a2-163">Middlewaru souboru Cookie. technologie ASP.NET používá ochranu dat k ochraně souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="0c8a2-164">To vede k uživatelům se odhlásili z aplikace, která používá standardní middlewaru souboru Cookie. technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="0c8a2-165">Pro kanál klíč slotu nezávislé na řešení využívají, jako poskytovatele vnější prstenec klíč:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="0c8a2-166">Úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="0c8a2-166">Azure Blob Storage</span></span>
* <span data-ttu-id="0c8a2-167">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0c8a2-167">Azure Key Vault</span></span>
* <span data-ttu-id="0c8a2-168">Úložiště SQL</span><span class="sxs-lookup"><span data-stu-id="0c8a2-168">SQL store</span></span>
* <span data-ttu-id="0c8a2-169">Redis cache</span><span class="sxs-lookup"><span data-stu-id="0c8a2-169">Redis cache</span></span>

<span data-ttu-id="0c8a2-170">Další informace najdete v tématu [zprostředkovateli úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="0c8a2-171">Nasazení ve verzi preview ASP.NET Core do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0c8a2-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="0c8a2-172">Aplikace ASP.NET Core ve verzi preview je možné nasadit do služby Azure App Service pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-172">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="0c8a2-173">[Instalace rozšíření webu ve verzi preview](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="0c8a2-173">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="0c8a2-174">Použití Docker pro kontejnery s Web Apps</span><span class="sxs-lookup"><span data-stu-id="0c8a2-174">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="0c8a2-175">Instalace rozšíření webu ve verzi preview</span><span class="sxs-lookup"><span data-stu-id="0c8a2-175">Install the preview site extension</span></span>

<span data-ttu-id="0c8a2-176">Pokud dojde k potížím s pomocí rozšíření webu ve verzi preview, otevřete problém na [Githubu](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-176">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="0c8a2-177">Na webu Azure Portal přejděte do okna služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-177">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="0c8a2-178">Vyberte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-178">Select the web app.</span></span>
1. <span data-ttu-id="0c8a2-179">Zadejte "ex" vyhledávacího pole zadejte nebo přejděte dolů v seznamu management oddíly pro **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-179">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="0c8a2-180">Vyberte **nástroje pro vývoj** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-180">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="0c8a2-181">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-181">Select **Add**.</span></span>
1. <span data-ttu-id="0c8a2-182">Vyberte **ASP.NET Core &lt;x.y&gt; (x86) Runtime** rozšíření ze seznamu, kde `<x.y>` je verze preview ASP.NET Core (například **Runtime ASP.NET Core 2.2 (x86)**).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-182">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="0c8a2-183">X86 modulu runtime je vhodný pro [nasazení závisí na architektuře](/dotnet/core/deploying/#framework-dependent-deployments-fdd) , které využívají mimo proces hostování modulu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-183">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="0c8a2-184">Vyberte **OK** přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-184">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="0c8a2-185">Vyberte **OK** nainstalovat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-185">Select **OK** to install the extension.</span></span>

<span data-ttu-id="0c8a2-186">Po dokončení operace, je nainstalovaná nejnovější verze preview .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-186">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="0c8a2-187">Ověření instalace:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-187">Verify the installation:</span></span>

1. <span data-ttu-id="0c8a2-188">Vyberte **Rozšířené nástroje** pod **nástroje pro vývoj**.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-188">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="0c8a2-189">Vyberte **Přejít** na **Rozšířené nástroje** okno.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-189">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="0c8a2-190">Vyberte **konzolou pro ladění** > **Powershellu** položky nabídky.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-190">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="0c8a2-191">Na příkazovém řádku Powershellu spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-191">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="0c8a2-192">Nahraďte verze modulu runtime ASP.NET Core pro `<x.y>` v příkazu:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-192">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="0c8a2-193">Pokud modul runtime nainstalovaný ve verzi preview je určený pro ASP.NET Core 2.2, příkaz je:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-193">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="0c8a2-194">Příkaz vrátí `True` při x64 je nainstalován modul runtime ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-194">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="0c8a2-195">Architektura platformy (x86/x64) aplikace služby App Services je nastavena v **nastavení aplikace** okně v části **obecné nastavení** pro aplikace, které jsou hostované na výpočetní prostředky řady A-series nebo lepší hostování vrstvy.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-195">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="0c8a2-196">Pokud aplikace běží v režimu v procesu a architektura platformy je nakonfigurovaný pro (x64) 64-bit, používá modul ASP.NET Core runtime 64bitová verze preview, pokud jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-196">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="0c8a2-197">Nainstalujte **ASP.NET Core &lt;x.y&gt; (x64) Runtime** rozšíření (například **modulu Runtime ASP.NET Core 2.2 (x64)**).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-197">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="0c8a2-198">Po instalaci x64 ve verzi preview modulu runtime, spusťte následující příkaz v příkazovém okně prostředí PowerShell Kudu k ověření instalace.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-198">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="0c8a2-199">Nahraďte verze modulu runtime ASP.NET Core pro `<x.y>` v příkazu:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-199">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="0c8a2-200">Pokud modul runtime nainstalovaný ve verzi preview je určený pro ASP.NET Core 2.2, příkaz je:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-200">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="0c8a2-201">Příkaz vrátí `True` při x64 je nainstalován modul runtime ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0c8a2-202">**Rozšíření ASP.NET Core** povoluje další funkce pro ASP.NET Core v Azure App Services, jako například povolení protokolování v Azure.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-202">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="0c8a2-203">Rozšíření se nainstaluje automaticky při nasazení ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-203">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="0c8a2-204">Pokud rozšíření není nainstalovaný, nainstalujte ho pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-204">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="0c8a2-205">**Použití rozšíření webu ve verzi preview pomocí šablony ARM**</span><span class="sxs-lookup"><span data-stu-id="0c8a2-205">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="0c8a2-206">Pokud šablonu ARM se používá k vytvoření a nasazení aplikace, `siteextensions` typ prostředku je možné přidat rozšíření webu do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-206">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="0c8a2-207">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-207">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="0c8a2-208">Použití Docker pro kontejnery s Web Apps</span><span class="sxs-lookup"><span data-stu-id="0c8a2-208">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="0c8a2-209">[Docker Hubu](https://hub.docker.com/r/microsoft/aspnetcore/) obsahuje nejnovější Image Dockeru ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-209">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="0c8a2-210">Image můžete použít jako základní image.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-210">The images can be used as a base image.</span></span> <span data-ttu-id="0c8a2-211">Použít bitovou kopii a nasazení do Web Apps for Containers normálně.</span><span class="sxs-lookup"><span data-stu-id="0c8a2-211">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c8a2-212">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0c8a2-212">Additional resources</span></span>

* [<span data-ttu-id="0c8a2-213">Přehled Web Apps (5minutové video s přehledem)</span><span class="sxs-lookup"><span data-stu-id="0c8a2-213">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="0c8a2-214">Azure App Service: Nejlepší místo k hostování aplikací .NET (55 minutu video s přehledem)</span><span class="sxs-lookup"><span data-stu-id="0c8a2-214">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="0c8a2-215">Azure Friday: Azure App Service diagnostiku a řešení potíží s prostředí (12 pětiminutové video)</span><span class="sxs-lookup"><span data-stu-id="0c8a2-215">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="0c8a2-216">Přehled diagnostiky služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0c8a2-216">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="0c8a2-217">Služba Azure App Service v systému Windows Server používá [Internetové informační služby (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="0c8a2-217">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="0c8a2-218">Následující témata se týkají základní technologie služby IIS:</span><span class="sxs-lookup"><span data-stu-id="0c8a2-218">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="0c8a2-219">Microsoft TechNet Library: Windows Server</span><span class="sxs-lookup"><span data-stu-id="0c8a2-219">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
