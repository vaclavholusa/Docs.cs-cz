---
uid: whitepapers/aspnet-and-iis6
title: "Používající technologii ASP.NET 1.1 pomocí služby IIS 6.0 | Microsoft Docs"
author: rick-anderson
description: "Zatímco systém Windows Server 2003 zahrnuje službu IIS 6.0 a ASP.NET 1.1, tyto součásti jsou zakázané ve výchozím nastavení. Tento dokument White Paper popisuje, jak povolit služby IIS 6.0..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="bf8d6-104">Používající technologii ASP.NET 1.1 pomocí služby IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="bf8d6-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="bf8d6-105">Zatímco systém Windows Server 2003 zahrnuje službu IIS 6.0 a ASP.NET 1.1, tyto součásti jsou zakázané ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="bf8d6-106">Tento dokument White Paper popisuje postup při povolování služby IIS 6.0 a ASP.NET 1.1 a doporučuje několik nastavení konfigurace, abyste získali optimální výkon ze služby IIS a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="bf8d6-107">Platí pro technologii ASP.NET 1.1 a služby IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="bf8d6-108">ASP.NET 1.1 se dodává s Windows Server 2003, která také zahrnuje nejnovější verzi služby Internet Information Server (IIS) verze 6.0.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="bf8d6-109">Služby IIS 6.0 a ASP.NET 1.1 slouží k integraci bez problémů a ASP.NET výchozí nastavení do nového modelu pracovní proces služby IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="bf8d6-110">Ve výchozím nastavení není nainstalována technologie ASP.NET 1.1</span><span class="sxs-lookup"><span data-stu-id="bf8d6-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="bf8d6-111">Na rozdíl od předchozích verzí operačních systémů společnosti Microsoft server Internet Information Server (IIS) není povoleno ve výchozím nastavení; ani ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="bf8d6-112">Existují dvě možnosti pro povolení služby IIS:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="bf8d6-113">Povolení služby IIS, možnost #1 - Průvodce konfigurací serveru</span><span class="sxs-lookup"><span data-stu-id="bf8d6-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="bf8d6-114">Windows Server 2003 se dodává nové, Průvodce konfigurací serveru, se správně konfigurace serveru v režimu požadované.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="bf8d6-115">Pokud chcete spustit Průvodce – Poznámka: Chcete-li spustit průvodce, musíte být přihlášení jako správce – přejděte na: spuštění | Programy | Nástroje pro správu a vyberte možnost 'konfigurace serveru'.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="bf8d6-116">Po výběru, měli byste vidět, Průvodce konfigurací serveru' úvodní obrazovce:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="bf8d6-117">Klikněte na tlačítko ' Další &gt;':</span><span class="sxs-lookup"><span data-stu-id="bf8d6-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="bf8d6-118">Klikněte na tlačítko ' Další &gt;.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="bf8d6-119">Na této obrazovce budete muset vybrat, aplikační server (IIS, ASP.NET) jako možnosti pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="bf8d6-120">Klikněte na tlačítko ' Další &gt;'.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="bf8d6-121">Po výběru konfigurace serveru jako Server aplikace, tato obrazovka se zobrazí výzvy, jaké další funkce by měly být nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="bf8d6-122">Žádná možnost je vybrána ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-122">Neither option is selected by default.</span></span> <span data-ttu-id="bf8d6-123">Automaticky povolit technologii ASP.NET, budete muset vybrat možnost "Povolit ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="bf8d6-124">Klikněte na tlačítko ' Další &gt;'.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="bf8d6-125">Tato obrazovka se zobrazí, jaké možnosti jsou k instalaci.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="bf8d6-126">Klikněte na tlačítko ' Další &gt;'.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="bf8d6-127">Tato obrazovka se zobrazí během instalace možností, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="bf8d6-128">Je normální, že se ostatní dialogové okno, které polí se zobrazí, jak probíhá instalace služeb zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="bf8d6-129">Kromě toho můžete být vyzváni k zadání umístění instalačního disku CD Windows serveru 2003.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="bf8d6-130">Klikněte na tlačítko ' Další &gt;se při dokončení.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="bf8d6-131">Klikněte na tlačítko 'Dokončit' – Windows Server 2003 je nyní nakonfigurováno pro podporu služby IIS 6.0 a ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="bf8d6-132">Povolení služby IIS, možnost #2 - ruční konfiguraci služby IIS a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf8d6-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="bf8d6-133">Pokud nechcete používat, Server Průvodce konfigurací' Volitelně můžete nainstalovat službu IIS 6.0 a 1.1 ASP.NET pomocí "Přidat nebo odebrat programy" z ovládacích panelů.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="bf8d6-134">Nejprve otevřete ovládací Panel:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="bf8d6-135">Potom klikněte na 'přidat nebo odebrat součásti systému Windows, které se otevře Průvodce součástmi systému Windows:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="bf8d6-136">Zvýrazněte a 'aplikační Server a pak klikněte na 'Podrobnosti'?</span><span class="sxs-lookup"><span data-stu-id="bf8d6-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="bf8d6-137">tlačítko:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="bf8d6-138">Instalace technologie ASP.NET, zkontrolujte ' ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="bf8d6-139">Klikněte na tlačítko "OK" se vraťte do Průvodce součástmi systému Windows.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="bf8d6-140">Klikněte na tlačítko ' Další &gt;' z Průvodce součástmi systému Windows při zahájení instalace:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="bf8d6-141">Je normální, že se ostatní dialogové okno, které polí se zobrazí, jak probíhá instalace služeb zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="bf8d6-142">Kromě toho můžete být vyzváni k zadání umístění instalačního disku CD Windows serveru 2003.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="bf8d6-143">Po dokončení instalace se zobrazí poslední obrazovce Průvodce součástmi systému Windows:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="bf8d6-144">Služby IIS 6.0 a ASP.NET 1.1 teď nakonfigurované a dostupné.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="bf8d6-145">Doporučená nastavení</span><span class="sxs-lookup"><span data-stu-id="bf8d6-145">Recommended Settings</span></span>

<span data-ttu-id="bf8d6-146">Při spuštění ASP.NET 1.1 se službou IIS 6.0 existuje několik nastavení konfigurace, které se doporučuje, aby získali optimální výkon z ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="bf8d6-147">Konfigurace omezení paměti pracovní proces</span><span class="sxs-lookup"><span data-stu-id="bf8d6-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="bf8d6-148">Konfigurace recyklace pracovních procesů</span><span class="sxs-lookup"><span data-stu-id="bf8d6-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="bf8d6-149">Konfigurace omezení paměti pracovní proces</span><span class="sxs-lookup"><span data-stu-id="bf8d6-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="bf8d6-150">Ve výchozím nastavení služby IIS 6.0 není nastaven limit množství paměti, která může služba IIS použít.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="bf8d6-151">SOUBOR ASP. Funkce mezipaměti na NET závisí na omezení paměti, tedy do mezipaměti proaktivně odebrat nepoužité položky z paměti.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="bf8d6-152">Doporučujeme nakonfigurovat paměti recyklace funkce služby IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="bf8d6-153">Ke konfiguraci této otevřete Správce Internetové informační služby (Start | Programy | Nástroje pro správu | Internetová informační služba).</span><span class="sxs-lookup"><span data-stu-id="bf8d6-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="bf8d6-154">Jakmile otevřete, rozbalte složku 'fondů aplikací.:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="bf8d6-155">Pro každý fond aplikací:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="bf8d6-156">Klikněte pravým tlačítkem na fond aplikací, například 'DefaultAppPool' a vyberte možnost 'vlastnosti':</span><span class="sxs-lookup"><span data-stu-id="bf8d6-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="bf8d6-157">Dál povolte kliknutím na buď recyklace paměti ' maximální použité paměti (v megabajtech): ".</span><span class="sxs-lookup"><span data-stu-id="bf8d6-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="bf8d6-158">Hodnota nesmí být větší než velikost fyzické paměti (ne virtuální) na serveru, je dobré aproximace 60 % fyzické paměti, tj. pro server s 512 MB fyzické paměti vyberte 310.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="bf8d6-159">Doporučujeme taky, že maximální není delší než 800MB při použití adresní prostor 2GB.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="bf8d6-160">Pokud je adresní prostor adresy paměti serveru 3GB, maximální limit paměti pro pracovní proces může být až 1 800MB:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="bf8d6-161">Klikněte na tlačítko 'Použít' a 'OK' ukončíte dialogovém okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="bf8d6-162">Tento postup opakujte pro všechny fondy aplikací k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="bf8d6-163">Konfigurace pracovního procesu recyklace</span><span class="sxs-lookup"><span data-stu-id="bf8d6-163">Configuring worker recycling</span></span>

<span data-ttu-id="bf8d6-164">Ve výchozím nastavení nastaven recyklovat každých 29 hodin jeho pracovní proces služby IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="bf8d6-165">Toto je trochu agresivní pro aplikace používající technologii ASP.NET a se doporučuje, aby bylo zakázané recyklace automatické pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="bf8d6-166">Chcete-li zakázat automatické pracovní proces recykluje, nejprve otevřete Správce Internetové informační služby (Start | Programy | Nástroje pro správu | Internetová informační služba).</span><span class="sxs-lookup"><span data-stu-id="bf8d6-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="bf8d6-167">Jakmile otevřete, rozbalte složku 'fondů aplikací.:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="bf8d6-168">Pro každý fond aplikací:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-168">For each application pool:</span></span>

1. <span data-ttu-id="bf8d6-169">Klikněte pravým tlačítkem na fond aplikací, například 'DefaultAppPool' a vyberte možnost 'vlastnosti':</span><span class="sxs-lookup"><span data-stu-id="bf8d6-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="bf8d6-170">Zrušte zaškrtnutí políčka ' recyklovat pracovní proces (v minutách): ":</span><span class="sxs-lookup"><span data-stu-id="bf8d6-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="bf8d6-171">Klikněte na tlačítko 'Použít' a 'OK' ukončíte dialogovém okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="bf8d6-172">Tento postup opakujte pro všechny fondy aplikací k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="bf8d6-173">Udělení oprávnění k zápisu do systému souborů</span><span class="sxs-lookup"><span data-stu-id="bf8d6-173">Granting write access to the file system</span></span>

<span data-ttu-id="bf8d6-174">Pokud vaše aplikace vyžaduje přístup k zápisu do systému souborů a používáte budete muset upravit řízení přístupu seznamu (ACL) na soubor nebo složku udělit ASP.NET přístup k systému souborů NTFS.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="bf8d6-175">Například udělit ASP.NET oprávnění k zápisu do c:\inetpub\wwwroot nejdřív otevřete Průzkumníka a přejděte do adresáře:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="bf8d6-176">Pak klikněte pravým tlačítkem na adresář, např 'wwwroot' a vyberte možnost Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="bf8d6-177">Po otevření dialogu Vlastnosti, vyberte kartu 'Zabezpečení':</span><span class="sxs-lookup"><span data-stu-id="bf8d6-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="bf8d6-178">C:\inetpub\wwwroot\ adresář je speciální adresář, ve které speciální skupinu služby IIS 6.0, IIS\_WPG' je již oprávnění ke čtení &amp; oprávnění spouštět, zobrazovat obsah složky a číst.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="bf8d6-179">Ale abyste mohli udělit oprávnění k zápisu, musíte klikněte na zaškrtávací políčko Povolit pro zápis:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="bf8d6-180">Služby IIS 6.0 teď má oprávnění k zápisu v této složce.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="bf8d6-181">Udělit oprávnění k zápisu v jiných složkách, postupujte podle těchto kroků - Všimněte si, že budete muset přidat IIS\_WPG skupiny, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="bf8d6-182">Udělení oprávnění zapisovat do služby IIS\_WPG vám umožní všechny aplikace technologie ASP.NET pro zápis do tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="bf8d6-183">Podporuje integrované ověřování s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="bf8d6-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="bf8d6-184">Integrované ověřování systému SQL Server tak, aby využívala ověřování systému Windows NT umožňuje ověření účty přihlášení systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="bf8d6-185">To umožňuje uživatelům obejít standardní proces přihlášení serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="bf8d6-186">Tento přístup síťového uživatele získat přístup k databázi systému SQL Server bez zadávat samostatné přihlašovací identifikátor nebo heslo, protože systém SQL Server získá uživatele a informace o hesle z procesu zabezpečení sítě systému Windows NT.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="bf8d6-187">Výběr integrované ověřování pro aplikace ASP.NET je vhodné použít, protože žádné přihlašovací údaje jsou někdy uloženy v rámci připojovací řetězec pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="bf8d6-188">Připojovací řetězec použitý pro připojení k SQL místo bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="bf8d6-189">Tento připojovací řetězec oznamuje serveru SQL pro použití přihlašovacích údajů Windows aplikace pokusu o přístup k systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="bf8d6-190">V případě ASP.NET/IIS 6 bude účet ve službě IIS\_WPG skupiny.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="bf8d6-191">Pokud chcete povolit integrované ověřování mezi SQL serverem a ASP.NET, je potřeba nejdříve se ujistěte, že systém SQL Server je nakonfigurován pro integrované ověřování nebo ověřování v režimu Mixed - obraťte se na správce databáze to bylo možné určit.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="bf8d6-192">Pokud je SQL Server v jedné z těchto dvou režimů, můžete použít integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="bf8d6-193">Otevřete správce systému SQL Server Enterprise (spustit | Programy | Microsoft SQL Server | Správce organizace), vyberte příslušný server a rozbalte složku zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="bf8d6-194">Pokud se BUILTINT\IIS\_WPG "skupina není uvedena, klikněte pravým tlačítkem na přihlášení a vyberte nové přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="bf8d6-195">V, název:' textového pole zadejte buď ' \IIS [název serveru nebo domény]\_WPG, nebo klikněte na tlačítko se třemi tečkami otevřete výběr uživatele nebo skupiny systému Windows NT:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="bf8d6-196">Vyberte aktuálního počítače IIS\_WPG skupiny a klikněte na tlačítko "Přidat" a OK zavřete nástroje pro výběr.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="bf8d6-197">Pak musíte taky nastavit oprávnění pro přístup k databázi a výchozí databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="bf8d6-198">Nastavit výchozí databáze vyberte v rozevíracím seznamu je vybrána například níže Northwind:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="bf8d6-199">Potom klikněte na kartě přístup k databázi:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="bf8d6-200">Klikněte na zaškrtávací políčko povolení pro každou databázi, kterou chcete povolit přístup k.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="bf8d6-201">Budete také muset vyberte role databáze, kontrola db\_vlastníka zajistí vaše přihlašovací údaje zahrnují všechna potřebná oprávnění ke správě a použít vybrané databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="bf8d6-202">Klikněte na tlačítko OK zavřete dialogové okno vlastností.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="bf8d6-203">Vaše aplikace ASP.NET je nyní nakonfigurován pro podporu integrované ověřování systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="bf8d6-204">Nespouštět ASP.NET 1.0 v nativním režimu služby IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="bf8d6-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="bf8d6-205">ASP.NET 1.0 ve službě IIS 6.0 je podporována pouze v režimu kompatibility IIS 5.</span><span class="sxs-lookup"><span data-stu-id="bf8d6-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="bf8d6-206">Konfigurace ASP.NET 1.0 ke spuštění v režimu kompatibility služby IIS 5.0, otevřete Správce internetové služby a klikněte pravým tlačítkem na webové servery a vyberte vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="bf8d6-207">Přepněte na kartu služby a zkontrolujte? Spustit webovou službu v režimu služby IIS 5.0 izolace?:</span><span class="sxs-lookup"><span data-stu-id="bf8d6-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
