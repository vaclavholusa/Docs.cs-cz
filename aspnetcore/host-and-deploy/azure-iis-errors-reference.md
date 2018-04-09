---
title: Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core
author: guardrex
description: Rozlišení běžných chyb při hostování aplikace ASP.NET Core v Azure aplikace služby a služby IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: fb833ef8797ea7851cbaf53bb5681df248d07a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="e289c-103">Běžné chyby referenční dokumentace pro Azure App Service a IIS s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e289c-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="e289c-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e289c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e289c-105">Následující není úplný seznam chyb.</span><span class="sxs-lookup"><span data-stu-id="e289c-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="e289c-106">Pokud dojde k chybě, zde nejsou uvedeny [otevřete nový problém](https://github.com/aspnet/Docs/issues/new) s podrobné pokyny k tomu chybu reprodukovat.</span><span class="sxs-lookup"><span data-stu-id="e289c-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="e289c-107">Shromážděte následující informace:</span><span class="sxs-lookup"><span data-stu-id="e289c-107">Collect the following information:</span></span>

* <span data-ttu-id="e289c-108">Chování prohlížeče</span><span class="sxs-lookup"><span data-stu-id="e289c-108">Browser behavior</span></span>
* <span data-ttu-id="e289c-109">Položky protokolu událostí aplikace</span><span class="sxs-lookup"><span data-stu-id="e289c-109">Application Event Log entries</span></span>
* <span data-ttu-id="e289c-110">Položky protokolu stdout modulu jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e289c-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="e289c-111">Porovnávání informací o následující běžné chyby.</span><span class="sxs-lookup"><span data-stu-id="e289c-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="e289c-112">Pokud je nalezena shoda, postupujte podle pomoc při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="e289c-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="e289c-113">Instalační program nemohl získat VC ++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="e289c-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="e289c-114">**Instalační program výjimka:** 0x80072efd nebo 0x80072f76 - Nespecifikovaná chyba</span><span class="sxs-lookup"><span data-stu-id="e289c-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="e289c-115">**Instalační program protokolu výjimka&#8224;:** Chyba 0x80072efd nebo 0x80072f76: spuštění EXE balíčku se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="e289c-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="e289c-116">&#8224;V protokolu je umístěn v C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="e289c-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="e289c-117">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-117">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-118">Pokud systém nemá přístup k Internetu při instalaci serveru pro hostování sady, tato výjimka nastane, když se instalační program nemůže získat *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="e289c-118">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="e289c-119">Získání Instalační program z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="e289c-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="e289c-120">Pokud instalační program selže, nemusí server dostat na .NET Core runtime potřebná pro Hosting nasazení závislé na framework (chyba).</span><span class="sxs-lookup"><span data-stu-id="e289c-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="e289c-121">Pokud hostování disketové jednotky, zkontrolujte, zda je modul runtime nainstalovány v programech &amp; funkce.</span><span class="sxs-lookup"><span data-stu-id="e289c-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="e289c-122">V případě potřeby získat za běhu instalačního programu z [.NET všechny soubory ke stažení](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="e289c-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="e289c-123">Po instalaci modulu runtime, restartování systému nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e289c-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="e289c-124">Upgrade operačního systému odebrat modul 32-bit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e289c-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="e289c-125">**Protokolu aplikace:** knihovnu DLL modulu **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** se nepodařilo načíst.</span><span class="sxs-lookup"><span data-stu-id="e289c-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="e289c-126">Data je chyba.</span><span class="sxs-lookup"><span data-stu-id="e289c-126">The data is the error.</span></span>

<span data-ttu-id="e289c-127">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-127">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-128">Soubory s non-OS **C:\Windows\SysWOW64\inetsrv** directory nezachovají se během operační systém upgradu.</span><span class="sxs-lookup"><span data-stu-id="e289c-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="e289c-129">Pokud je nainstalován modul ASP.NET Core starších než upgrade operačního systému a potom všechny fondu aplikací je spuštěn v režimu 32-bit po upgradu operačního systému, zjistil se tento problém.</span><span class="sxs-lookup"><span data-stu-id="e289c-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="e289c-130">Po upgradu operačního systému opravte modul ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e289c-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="e289c-131">V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="e289c-131">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="e289c-132">Vyberte **Repair** spuštění Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="e289c-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="e289c-133">Platformy je v konfliktu s identifikátorů RID</span><span class="sxs-lookup"><span data-stu-id="e289c-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="e289c-134">**Prohlížeč:** chyb HTTP 502.5 - selhání procesu</span><span class="sxs-lookup"><span data-stu-id="e289c-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e289c-135">**Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\{cesta}\' Nepodařilo se spustit proces s příkazového řádku ' "C:\\umístění {cesta} {sestavení}. { exe | dll} "", kód chyby = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="e289c-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="e289c-136">**ASP.NET Core modulu protokolu:** neošetřená výjimka: System.BadImageFormatException: Nepodařilo se načíst soubor nebo sestavení "{sestavení} .dll".</span><span class="sxs-lookup"><span data-stu-id="e289c-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="e289c-137">Byl proveden pokus o načtení program v nesprávném formátu.</span><span class="sxs-lookup"><span data-stu-id="e289c-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="e289c-138">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-138">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-139">Potvrďte, že aplikace běží místně na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e289c-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e289c-140">Selhání procesu může být výsledkem problému v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e289c-141">Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e289c-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e289c-142">Potvrďte, že `<PlatformTarget>` v *.csproj* není v konfliktu s identifikátor RID.</span><span class="sxs-lookup"><span data-stu-id="e289c-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="e289c-143">Například nezadávejte `<PlatformTarget>` z `x86` a publikovat s RID `win10-x64`, buď pomocí *dotnet publikování - c - r verzi win10-x64* nebo nastavením `<RuntimeIdentifiers>` v *.csproj*  k `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="e289c-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="e289c-144">Projekt publikuje bez upozornění nebo chyby, ale selže s výše zaznamenané výjimky v systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="e289c-145">Pokud tuto výjimku dojde k nasazení aplikace Azure při upgradu aplikace a nasazení novější sestavení, odstraňte ručně všechny soubory z předchozí nasazení.</span><span class="sxs-lookup"><span data-stu-id="e289c-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="e289c-146">Přetrvávání odstraněných nekompatibilní sestavení může mít za následek `System.BadImageFormatException` výjimka při nasazení aplikace upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="e289c-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="e289c-147">Identifikátor URI koncového bodu nesprávné nebo se zastavil webu</span><span class="sxs-lookup"><span data-stu-id="e289c-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="e289c-148">**Prohlížeč:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="e289c-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="e289c-149">**Protokolu aplikace:** žádná položka</span><span class="sxs-lookup"><span data-stu-id="e289c-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="e289c-150">**ASP.NET Core modulu protokolu:** vytvoření souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="e289c-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e289c-151">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-151">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-152">Potvrďte, že se používá koncový bod správný identifikátor URI pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e289c-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="e289c-153">Zkontrolujte vazby.</span><span class="sxs-lookup"><span data-stu-id="e289c-153">Check the bindings.</span></span>

* <span data-ttu-id="e289c-154">Potvrďte, že na web služby IIS není v *Zastaveno* stavu.</span><span class="sxs-lookup"><span data-stu-id="e289c-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="e289c-155">Funkce serveru CoreWebEngine nebo W3SVC zakázána</span><span class="sxs-lookup"><span data-stu-id="e289c-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="e289c-156">**Výjimka operačního systému:** CoreWebEngine služby IIS 7.0 a W3SVC funkce musí být nainstalována na použít modul ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e289c-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="e289c-157">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-157">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-158">Potvrďte, že jsou povoleny správné role a funkce.</span><span class="sxs-lookup"><span data-stu-id="e289c-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="e289c-159">V tématu [konfigurace služby IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="e289c-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="e289c-160">Fyzická cesta nesprávný web nebo aplikaci chybí</span><span class="sxs-lookup"><span data-stu-id="e289c-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="e289c-161">**Prohlížeč:** 403 Zakázáno – přístup byl odepřen. **--nebo--** 403.14 zakázáno – webový server je nakonfigurován tak, aby nezobrazovat obsah tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="e289c-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="e289c-162">**Protokolu aplikace:** žádná položka</span><span class="sxs-lookup"><span data-stu-id="e289c-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="e289c-163">**ASP.NET Core modulu protokolu:** vytvoření souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="e289c-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e289c-164">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-164">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-165">Zkontrolujte na web služby IIS **základní nastavení** a složce fyzické aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="e289c-166">Potvrďte, že je aplikace ve složce na webu služby IIS **fyzická cesta**.</span><span class="sxs-lookup"><span data-stu-id="e289c-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="e289c-167">Nesprávný role, modul není nainstalován nebo nesprávná oprávnění</span><span class="sxs-lookup"><span data-stu-id="e289c-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="e289c-168">**Prohlížeč:** 500.19 vnitřní chyba serveru: K požadované stránce nelze přistupovat, protože související konfigurační data pro stránku nejsou platná.</span><span class="sxs-lookup"><span data-stu-id="e289c-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="e289c-169">**Protokolu aplikace:** žádná položka</span><span class="sxs-lookup"><span data-stu-id="e289c-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="e289c-170">**ASP.NET Core modulu protokolu:** vytvoření souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="e289c-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e289c-171">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-171">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-172">Zkontrolujte, zda je povoleno správné roli.</span><span class="sxs-lookup"><span data-stu-id="e289c-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="e289c-173">V tématu [konfigurace služby IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="e289c-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="e289c-174">Zkontrolujte **programy &amp; funkce** a ověřte, že **Microsoft ASP.NET Core modulu** byl nainstalován.</span><span class="sxs-lookup"><span data-stu-id="e289c-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="e289c-175">Pokud **Microsoft ASP.NET Core modulu** není uvedena v seznamu nainstalovaných programů, nainstalujte modul.</span><span class="sxs-lookup"><span data-stu-id="e289c-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="e289c-176">V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="e289c-176">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="e289c-177">Ujistěte se, že **fond aplikací** > **Model procesu** > **Identity** je nastaven na **ApplicationPoolIdentity** nebo vlastní identity má správná oprávnění pro přístup do složky pro nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="e289c-178">Nesprávný processPath chybějící proměnné PATH, hostování sady není nainstalován, není restartování systému nebo služby IIS, VC ++ Redistributable není nainstalovaný nebo dotnet.exe narušení přístupu</span><span class="sxs-lookup"><span data-stu-id="e289c-178">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="e289c-179">**Prohlížeč:** chyb HTTP 502.5 - selhání procesu</span><span class="sxs-lookup"><span data-stu-id="e289c-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e289c-180">**Protokolu aplikace:** aplikace ' MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' Nepodařilo se spustit proces s příkazového řádku. ".\{ sestavení} .exe"', kód chyby = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="e289c-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="e289c-181">**ASP.NET Core modulu protokolu:** vytvořit soubor protokolu, ale prázdný</span><span class="sxs-lookup"><span data-stu-id="e289c-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e289c-182">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-182">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-183">Potvrďte, že aplikace běží místně na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e289c-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e289c-184">Selhání procesu může být výsledkem problému v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e289c-185">Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e289c-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e289c-186">Zkontrolujte *processPath* atributu u `<aspNetCore>` element v *web.config* potvrďte, že je *dotnet* pro nasazení závislé na framework (disketové jednotky) nebo *. \{sestavení} .exe* samostatná nasazení (SCD).</span><span class="sxs-lookup"><span data-stu-id="e289c-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="e289c-187">Pro disketové jednotky *dotnet.exe* nemusí být přístupné přes nastavení cesty.</span><span class="sxs-lookup"><span data-stu-id="e289c-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="e289c-188">Potvrďte, že * C:\Program Files\dotnet\* existuje v systémové CESTĚ nastavení.</span><span class="sxs-lookup"><span data-stu-id="e289c-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="e289c-189">Pro disketové jednotky *dotnet.exe* nemusí být dostupná pro identitu uživatele fondu aplikací.</span><span class="sxs-lookup"><span data-stu-id="e289c-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="e289c-190">Zkontrolujte, jestli uživatel identita fondu aplikací má přístup k *C:\Program Files\dotnet* adresáře.</span><span class="sxs-lookup"><span data-stu-id="e289c-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="e289c-191">Potvrďte, že neexistují žádná pravidla odepření nakonfigurované pro identitu fondu aplikací uživatele na *C:\Program Files\dotnet* a adresáře aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="e289c-192">Disketové jednotky nasazení a nainstalovat .NET Core bez restartování služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e289c-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="e289c-193">Restartujte server, nebo restartujte službu IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e289c-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e289c-194">Disketové jednotky může mít nasazenou bez instalace na .NET Core runtime v hostitelském systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="e289c-195">Pokud na .NET Core runtime nebyla nainstalována, spusťte **hostování v rozhraní .NET Core systému Windows Server instalační program sady** v systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="e289c-196">V tématu [instalaci sady .NET jádra Windows serveru, který hostuje](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="e289c-196">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="e289c-197">Pokud došlo k pokusu o instalaci na .NET Core runtime v systému bez připojení k Internetu, získat modul runtime z [.NET všechny soubory ke stažení](https://www.microsoft.com/net/download/all) a spusťte instalační program hostování sady nainstalovat modul ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e289c-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="e289c-198">Dokončete instalaci restartováním systému nebo restartování služby IIS tak, že spustíte **net stop byl /y** následuje **net start w3svc** z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e289c-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e289c-199">Byla nasazena disketové jednotky a *Microsoft Visual C++ 2015 Redistributable (x64)* není nainstalovaná v systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="e289c-200">Získání Instalační program z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="e289c-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="e289c-201">Nesprávný argumenty \<aspNetCore\> – element</span><span class="sxs-lookup"><span data-stu-id="e289c-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="e289c-202">**Prohlížeč:** chyb HTTP 502.5 - selhání procesu</span><span class="sxs-lookup"><span data-stu-id="e289c-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e289c-203">**Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' Nepodařilo se spustit proces s příkazového řádku ' "dotnet".\{ .dll sestavení}', kód chyby = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="e289c-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e289c-204">**ASP.NET Core modulu protokolu:** aplikace provést neexistuje: "cesta\{sestavení} .dll.</span><span class="sxs-lookup"><span data-stu-id="e289c-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="e289c-205">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-205">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-206">Potvrďte, že aplikace běží místně na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e289c-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e289c-207">Selhání procesu může být výsledkem problému v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e289c-208">Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e289c-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e289c-209">Zkontrolujte *argumenty* atributu u `<aspNetCore>` element v *web.config* potvrďte, že je buď (a) *.\{ sestavení} .dll* pro nasazení závislé na framework (disketové jednotky); nebo (b) není k dispozici, prázdný řetězec (*argumenty = ""*), nebo seznam argumentů aplikace (*argumenty = "arg1 arg2,..."*) samostatná nasazení (SCD).</span><span class="sxs-lookup"><span data-stu-id="e289c-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="e289c-210">Chybějící verze rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="e289c-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="e289c-211">**Prohlížeč:** 502.3 Chybná brána - došlo k chybě připojení při pokusu o pro směrování požadavku.</span><span class="sxs-lookup"><span data-stu-id="e289c-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="e289c-212">**Protokolu aplikace:** kód chyby = aplikace ' MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' Nepodařilo se spustit proces s příkazového řádku ' "dotnet".\{ .dll sestavení}', kód chyby = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="e289c-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e289c-213">**ASP.NET Core modulu protokolu:** chybí výjimka metoda, soubor nebo sestavení.</span><span class="sxs-lookup"><span data-stu-id="e289c-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="e289c-214">Metoda, soubor nebo sestavení. výjimkou je metoda rozhraní .NET Framework, soubor nebo sestavení.</span><span class="sxs-lookup"><span data-stu-id="e289c-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="e289c-215">Řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="e289c-215">Troubleshooting:</span></span>

* <span data-ttu-id="e289c-216">Nainstalujte verzi rozhraní .NET Framework chybí ze systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="e289c-217">Pro nasazení závislé na framework (disketové jednotky) zkontrolujte, že správné runtime nainstalovaná v systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="e289c-218">Pokud projekt je upgradovat z verze 1.1 na 2.0, které jsou nasazené v systému hostitele a výsledky výjimku, ujistěte se, že rozhraní framework 2.0 je v hostitelském systému.</span><span class="sxs-lookup"><span data-stu-id="e289c-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="e289c-219">Zastavený fond aplikací</span><span class="sxs-lookup"><span data-stu-id="e289c-219">Stopped Application Pool</span></span>

* <span data-ttu-id="e289c-220">**Prohlížeč:** 503 Služba nedostupná</span><span class="sxs-lookup"><span data-stu-id="e289c-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="e289c-221">**Protokolu aplikace:** žádná položka</span><span class="sxs-lookup"><span data-stu-id="e289c-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="e289c-222">**ASP.NET Core modulu protokolu:** vytvoření souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="e289c-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e289c-223">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="e289c-223">Troubleshooting</span></span>

* <span data-ttu-id="e289c-224">Potvrďte, že fond aplikací není v *Zastaveno* stavu.</span><span class="sxs-lookup"><span data-stu-id="e289c-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="e289c-225">Middleware integrační služby IIS není implementováno</span><span class="sxs-lookup"><span data-stu-id="e289c-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="e289c-226">**Prohlížeč:** chyb HTTP 502.5 - selhání procesu</span><span class="sxs-lookup"><span data-stu-id="e289c-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e289c-227">**Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' vytvořené proces pomocí příkazového řádku ' "C:\\{umístění cesta}\{sestavení}. { exe | dll} ", ale buď došlo k chybě nebo nebyla odezvy nebo není naslouchat na zadaný port '{PORT}', kód chyby = '0x800705b4.</span><span class="sxs-lookup"><span data-stu-id="e289c-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e289c-228">**ASP.NET Core modulu protokolu:** vytvořit soubor protokolu a zobrazuje normálního provozu.</span><span class="sxs-lookup"><span data-stu-id="e289c-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="e289c-229">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="e289c-229">Troubleshooting</span></span>

* <span data-ttu-id="e289c-230">Potvrďte, že aplikace běží místně na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e289c-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e289c-231">Selhání procesu může být výsledkem problému v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e289c-232">Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e289c-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e289c-233">Potvrďte, že buď:</span><span class="sxs-lookup"><span data-stu-id="e289c-233">Confirm that either:</span></span>
  * <span data-ttu-id="e289c-234">Middleware integrační služby IIS je referencedby volání `UseIISIntegration` metodu aplikace `WebHostBuilder` (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="e289c-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="e289c-235">Aplikace používá `CreateDefaultBuilder` – metoda (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="e289c-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="e289c-236">V tématu [hostování v ASP.NET Core](xref:fundamentals/hosting) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e289c-236">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="e289c-237">Obsahuje dílčí aplikace \<obslužné rutiny\> části</span><span class="sxs-lookup"><span data-stu-id="e289c-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="e289c-238">**Prohlížeč:** Chyba protokolu HTTP 500.19 – vnitřní chyba serveru</span><span class="sxs-lookup"><span data-stu-id="e289c-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="e289c-239">**Protokolu aplikace:** žádná položka</span><span class="sxs-lookup"><span data-stu-id="e289c-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="e289c-240">**ASP.NET Core modulu protokolu:** soubor protokolu vytvořen a zobrazuje normálního provozu pro kořenovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e289c-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="e289c-241">Soubor protokolu nebyl vytvořen pro aplikace sub.</span><span class="sxs-lookup"><span data-stu-id="e289c-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="e289c-242">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="e289c-242">Troubleshooting</span></span>

* <span data-ttu-id="e289c-243">Potvrďte, že dílčí aplikace *web.config* soubor neobsahuje `<handlers>` části.</span><span class="sxs-lookup"><span data-stu-id="e289c-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="e289c-244">Cesta k protokolu STDOUT nesprávný</span><span class="sxs-lookup"><span data-stu-id="e289c-244">stdout log path incorrect</span></span>

* <span data-ttu-id="e289c-245">**Prohlížeč:** aplikace reaguje normálně.</span><span class="sxs-lookup"><span data-stu-id="e289c-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="e289c-246">**Protokolu aplikace:** upozornění: Nepodařilo se vytvořit stdoutLogFile \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, kód chyby = - 2147024893.</span><span class="sxs-lookup"><span data-stu-id="e289c-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="e289c-247">**ASP.NET Core modulu protokolu:** vytvoření souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="e289c-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e289c-248">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="e289c-248">Troubleshooting</span></span>

* <span data-ttu-id="e289c-249">`stdoutLogFile` Cesta zadaná v `<aspNetCore>` element *web.config* neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e289c-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="e289c-250">Další informace najdete v tématu [vytvoření a přesměrování protokolu](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) tématu referenční konfigurace modulu jádra ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e289c-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="e289c-251">Obecné problém konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="e289c-251">Application configuration general issue</span></span>

* <span data-ttu-id="e289c-252">**Prohlížeč:** chyb HTTP 502.5 - selhání procesu</span><span class="sxs-lookup"><span data-stu-id="e289c-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e289c-253">**Protokolu aplikace:** aplikace se MACHINE/WEBROOT/APPHOST / {sestavení} se s kořenem fyzické ' C:\\{umístění cesta}\' vytvořené proces pomocí příkazového řádku ' "C:\\{umístění cesta}\{sestavení}. { exe | dll} ", ale buď došlo k chybě nebo nebyla odezvy nebo není naslouchat na zadaný port '{PORT}', kód chyby = '0x800705b4.</span><span class="sxs-lookup"><span data-stu-id="e289c-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e289c-254">**ASP.NET Core modulu protokolu:** vytvořit soubor protokolu, ale prázdný</span><span class="sxs-lookup"><span data-stu-id="e289c-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e289c-255">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="e289c-255">Troubleshooting</span></span>

* <span data-ttu-id="e289c-256">Tato obecná výjimka označuje, že proces se nepodařilo spustit, pravděpodobně z důvodu chyby konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="e289c-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="e289c-257">Odkazy na [adresářovou strukturu](xref:host-and-deploy/directory-structure), potvrďte, že aplikace nasazená soubory a složky, které jsou vhodné, a zda jsou k dispozici konfigurační soubory aplikace a obsahovat správná nastavení pro aplikaci a prostředí.</span><span class="sxs-lookup"><span data-stu-id="e289c-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="e289c-258">Další informace najdete v tématu [Poradce při potížích s](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e289c-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
