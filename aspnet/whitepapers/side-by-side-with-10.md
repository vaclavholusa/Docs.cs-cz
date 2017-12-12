---
uid: whitepapers/side-by-side-with-10
title: "Spuštění rozhraní .NET Framework 1.0 a 1.1 ASP.NET vedle sebe | Microsoft Docs"
author: rick-anderson
description: "Tento dokument White Paper popisuje postup instalace rozhraní .NET 1.0 a .NET 1.1 na počítači, což webovou aplikaci ASP.NET ke spuštění na jednu z verzí nástroje od..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="d6012-103">Spuštění rozhraní .NET Framework 1.0 a 1.1 ASP.NET vedle sebe</span><span class="sxs-lookup"><span data-stu-id="d6012-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="d6012-104">Tento dokument White Paper popisuje postup instalace na počítači, což webovou aplikaci ASP.NET ke spuštění na jedné verze rozhraní .NET 1.0 a 1.1 rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="d6012-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="d6012-105">Platí pro ASP.NET 1.0 a 1.1 technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d6012-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="d6012-106">V technologii ASP.NET aplikace řečeno, aby byl spuštěn vedle sebe, pokud jsou nainstalované na stejném počítači, ale používají různé verze rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="d6012-107">Následující téma popisuje konfiguraci aplikace ASP.NET pro spuštění vedle sebe a poskytuje podrobné kroky:</span><span class="sxs-lookup"><span data-stu-id="d6012-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="d6012-108">Udržovat mapování webové aplikace rozhraní .NET Framework verze 1.0 během instalace</span><span class="sxs-lookup"><span data-stu-id="d6012-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="d6012-109">Mapa webovou aplikaci na konkrétní verzi rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d6012-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="d6012-110">Najít verzi rozhraní .NET Framework, který používá webový server</span><span class="sxs-lookup"><span data-stu-id="d6012-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="d6012-111">Tradičně Pokud na počítači se aktualizuje součást nebo aplikace, starší verzi je odebere a nahradí novější verze.</span><span class="sxs-lookup"><span data-stu-id="d6012-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="d6012-112">Pokud nové verze není kompatibilní s předchozí verzí, to obvykle dělí jiné aplikace, které používají součást nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6012-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="d6012-113">Rozhraní .NET Framework poskytuje podporu pro spuštění vedle sebe, což umožňuje více verzí sestavení nebo aplikace bude nainstalována na stejném počítači ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="d6012-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="d6012-114">Protože současně může být nainstalováno více verzí, můžete vybrat spravované aplikace, kterou verzi má použít, aniž by to ovlivnilo aplikace, které používají různé verze.</span><span class="sxs-lookup"><span data-stu-id="d6012-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="d6012-115">Ve výchozím nastavení při instalaci rozhraní .NET Framework verze 1.1, všechny existující aplikace ASP.NET je automaticky překonfigurovat tak, aby používali nejnovější verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="d6012-116">Pokud nechcete, aby vaše aplikace ASP.NET, jako výchozí rozhraní .NET Framework 1.1, klikněte na tlačítko [sem](#1) se dozvíte, jak chcete-li tomu zabránit během instalace.</span><span class="sxs-lookup"><span data-stu-id="d6012-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="d6012-117">Pokud váš webový server aktualizovat na rozhraní .NET Framework 1.1 a chcete jeden nebo více webových aplikací ke spuštění rozhraní .NET Framework 1.0, budete muset aktualizovat mapu skriptů Internetové informační služby (IIS).</span><span class="sxs-lookup"><span data-stu-id="d6012-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="d6012-118">Mapování skriptu je mechanismus pro mapování .aspx příponu souboru pro konkrétní webovou aplikaci na verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="d6012-119">Klikněte na tlačítko [sem](#2) se dozvíte, jak k mapování webovou aplikaci na konkrétní verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="d6012-120">Můžete použít správce sítě Internet informace nebo ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) k vyhledání, která verze rozhraní .NET Framework je spuštěna aplikace typu konkrétní Web.</span><span class="sxs-lookup"><span data-stu-id="d6012-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="d6012-121">Klikněte na tlačítko [sem](#3) se dozvíte, jak najít verzi rozhraní .NET Framework, který používá webový server.</span><span class="sxs-lookup"><span data-stu-id="d6012-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="d6012-122">Při migraci na rozhraní .NET Framework 1.1 jeden aspekt importu je, že každou verzi rozhraní .NET Framework používá vlastního souboru Machine.config.</span><span class="sxs-lookup"><span data-stu-id="d6012-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="d6012-123">V důsledku toho za správce webu provedla změny souboru Machine.config, musí být tyto změny migrovány do souboru rozhraní .NET Framework 1.1 Machine.config.</span><span class="sxs-lookup"><span data-stu-id="d6012-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="d6012-124">Správa mapování webové aplikace na rozhraní .NET Framework 1.0 během instalace</span><span class="sxs-lookup"><span data-stu-id="d6012-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="d6012-125">Ve výchozím nastavení se všechny existující aplikace ASP.NET mění automaticky během instalace použít novější verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="d6012-126">Pomocí novější verze rozhraní .NET Framework, může aplikace využít všech výhod vylepšení a novým funkcím zahrnutým v nové verzi.</span><span class="sxs-lookup"><span data-stu-id="d6012-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="d6012-127">Ve stejnou dobu správce webu, který může být vhodné podrobnou kontrolu nad aplikace, které jsou aktualizované, můžete zabránit automatické přemapování všech existujících aplikací ASP.NET během instalace rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="d6012-128">Pokud chcete zabránit automatické přemapování celá aplikace ASP.NET na novější verzi rozhraní .NET Framework, správce webu, můžete použít možnost příkazového řádku/noaspupgrade s instalačním programem Dotnetfx.exe.</span><span class="sxs-lookup"><span data-stu-id="d6012-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="d6012-129">**Aby se zabránilo celkový přemapování aplikace technologie ASP.NET pro novější verze**</span><span class="sxs-lookup"><span data-stu-id="d6012-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="d6012-130">Přejděte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d6012-130">Go to **Start**.</span></span>
2. <span data-ttu-id="d6012-131">Klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d6012-131">Click on **run**.</span></span>
3. <span data-ttu-id="d6012-132">Typ **cmd**.</span><span class="sxs-lookup"><span data-stu-id="d6012-132">Type **cmd**.</span></span>
4. <span data-ttu-id="d6012-133">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6012-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="d6012-134">Z příkazového řádku zadejte následující příkaz ke spuštění instalace rozhraní .NET Framework: **Dotnetfx.exe/c: "instalace/noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="d6012-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="d6012-135">Klikněte na tlačítko **Ano** v instalačním programu rozhraní Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="d6012-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="d6012-136">Tato akce spustí proces instalace rozhraní .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="d6012-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="d6012-137">Mapa webovou aplikaci na konkrétní verzi rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d6012-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="d6012-138">Obsahuje každou verzi rozhraní .NET Framework verze nástroje ASP.NET IIS Registration (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="d6012-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="d6012-139">Tento nástroj umožňuje správcům spustit webovou aplikaci v rámci konkrétní verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="d6012-140">To se označuje jako mapování webovou aplikaci pro verzi rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6012-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="d6012-141">Správci musí vybrat Aspnet\_regiis.exe odpovídající verzi rozhraní .NET Framework, který bude spojený s webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="d6012-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="d6012-142">Například správce, který chce určit, že webový server používat rozhraní .NET Framework 1.1 musí použít Aspnet\_regiis.exe která se dodává s .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="d6012-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="d6012-143">Aspnet\_regiis.exe pro verzi 1.0 je umístěn zde:</span><span class="sxs-lookup"><span data-stu-id="d6012-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="d6012-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="d6012-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="d6012-145">Aspnet\_regiis.exe pro verzi 1,1 je umístěn zde:</span><span class="sxs-lookup"><span data-stu-id="d6012-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="d6012-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="d6012-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="d6012-147">Aspnet\_regiis.exe nabízí dvě možnosti pro skript mapování webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="d6012-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="d6012-148">**-s** Nastaví mapu skriptů v cestě a v jeho podřízených adresářů.</span><span class="sxs-lookup"><span data-stu-id="d6012-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="d6012-149">**-sn** Nastaví mapu skriptů se pouze na cestě.</span><span class="sxs-lookup"><span data-stu-id="d6012-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="d6012-150">Definuje cestu webové cesta k aplikaci IIS metadata, která je definovaná ve formě W3SVC/ROOT / {WebSiteNumber} / {aplikace\_název}.</span><span class="sxs-lookup"><span data-stu-id="d6012-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="d6012-151">Například pro webovou aplikaci označuje umístěná na výchozí web portál, je cestu metabáze W3SVC/1/ROOT nebo portálu.</span><span class="sxs-lookup"><span data-stu-id="d6012-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="d6012-152">Všimněte si, že používáte nástroj nazvaný editoru metabáze můžete také získat cestu metabáze.</span><span class="sxs-lookup"><span data-stu-id="d6012-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="d6012-153">Tento nástroj můžete stáhnout z webu Microsoft Support na [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="d6012-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="d6012-154">Spustit Aspnet\_regiis.exe -s W3SVC/1/ROOT nebo portál aktualizovat na portálu služby IIS skriptu mapy a jeho subapplication.</span><span class="sxs-lookup"><span data-stu-id="d6012-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="d6012-155">Spustit Aspnet\_regiis.exe -sn mapovat W3SVC/1/KOŘENOVÉ nebo portálu na portálu služby IIS skript aktualizovat, aniž by to ovlivnilo aplikací na portálu? podadresáře s.</span><span class="sxs-lookup"><span data-stu-id="d6012-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="d6012-156">Najít verzi rozhraní .NET Framework, který používá webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d6012-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="d6012-157">Správce může pomocí Správce služeb sítě Internet k vyhledání, která verze rozhraní .NET Framework spouští webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="d6012-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="d6012-158">Různé verze operačního systému jinak spusťte Správce služeb sítě Internet.</span><span class="sxs-lookup"><span data-stu-id="d6012-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="d6012-159">Pokud chcete spustit Správce služeb, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="d6012-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="d6012-160">**Chcete-li spustit Správce služeb sítě Internet**</span><span class="sxs-lookup"><span data-stu-id="d6012-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="d6012-161">Přejděte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d6012-161">Go to **Start**.</span></span>
2. <span data-ttu-id="d6012-162">Klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d6012-162">Click on **run**.</span></span>
3. <span data-ttu-id="d6012-163">Typ **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="d6012-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="d6012-164">Ze Správce služeb Internetu, vyberte webovou aplikaci, jehož verze rozhraní .NET Framework, budete chtít vědět.</span><span class="sxs-lookup"><span data-stu-id="d6012-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="d6012-165">Klikněte pravým tlačítkem na webové aplikace a klikněte na **vlastnosti.**</span><span class="sxs-lookup"><span data-stu-id="d6012-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="d6012-166">V okně Vlastnosti vyberte **konfigurace.**</span><span class="sxs-lookup"><span data-stu-id="d6012-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="d6012-167">Z aplikace mapování tabulky, vyberte **.aspx**a klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="d6012-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="d6012-168">Z **spustitelný soubor** textového pole, podívejte se na verzi adresáře s posouvání.</span><span class="sxs-lookup"><span data-stu-id="d6012-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="d6012-169">Pokud je verze adresář v.1.1.4322, aplikace je mapována na rozhraní .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="d6012-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="d6012-170">Naopak pokud je verze adresář v1.0.3705, aplikace je mapována na rozhraní .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="d6012-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
