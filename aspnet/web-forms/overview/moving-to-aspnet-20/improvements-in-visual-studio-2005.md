---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: "Vylepšení v sadě Visual Studio 2005 | Microsoft Docs"
author: microsoft
description: "Visual Studio 2005 poskytuje webové aplikace vývojářům dlouhý seznam vylepšení a vylepšení, které webové projekty."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 2c1f9a7291d8eab675bac3e1c37d6922131e3761
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="83206-103">Vylepšení v sadě Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="83206-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="83206-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="83206-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="83206-105">Visual Studio 2005 poskytuje webové aplikace vývojářům dlouhý seznam vylepšení a vylepšení, které webové projekty.</span><span class="sxs-lookup"><span data-stu-id="83206-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="83206-106">Visual Studio 2005 poskytuje webové aplikace vývojářům dlouhý seznam vylepšení a vylepšení, které webové projekty.</span><span class="sxs-lookup"><span data-stu-id="83206-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="83206-107">Jako výkonná jako Visual Studio .NET 2002 a 2003 se vyskytly mnoho stížností způsobem, že byly zpracovány webové projekty.</span><span class="sxs-lookup"><span data-stu-id="83206-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="83206-108">Visual Studio 2005 přidá Chcete-li vyřešit tyto stížností velký počet nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="83206-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="83206-109">Pro ty, kteří raději způsob, Visual Studio .NET 2003 zpracování kompilace webových aplikací, najdete v části [projekty webových aplikací](https://go.microsoft.com/fwlink/?LinkId=57870).</span><span class="sxs-lookup"><span data-stu-id="83206-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="83206-110">Tento modul a obsahuje vylepšení v oblasti vytváření webového projektu, správu a vývoj.</span><span class="sxs-lookup"><span data-stu-id="83206-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="83206-111">Modul novější dobře obsahuje vylepšení v oblasti vytváření webové projekty a jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="83206-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="83206-112">Serverová rozšíření aplikace FrontPage</span><span class="sxs-lookup"><span data-stu-id="83206-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="83206-113">Visual Studio .NET 2002 a 2003 vyžaduje serverová rozšíření FrontPage na pole za účelem vytvoření nebo sestavení webové projekty.</span><span class="sxs-lookup"><span data-stu-id="83206-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="83206-114">Vývojáři mají vybrat mezi dvěma režimy různý přístup (serverová rozšíření FrontPage nebo soubor režim přístupu), používá serverová rozšíření FrontPage k provádění úloh, jako je třeba nastavení kořenový adresář aplikace v IIS, atd.</span><span class="sxs-lookup"><span data-stu-id="83206-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="83206-115">Visual Studio 2005 odebere spoléhat na serverová rozšíření FrontPage pro místní projekty.</span><span class="sxs-lookup"><span data-stu-id="83206-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="83206-116">Visual Studio 2005 teď přistupuje k metabázi služby IIS přímo místo používá serverová rozšíření FrontPage.</span><span class="sxs-lookup"><span data-stu-id="83206-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="83206-117">Visual Studio 2005 také přidává podporu pro FTP, který umožňuje přístup k vzdálené projektu bez nutnosti serverová rozšíření FrontPage.</span><span class="sxs-lookup"><span data-stu-id="83206-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="83206-118">Pro tyto vývojáře, kteří chtějí používat serverová rozšíření FrontPage v jejich projektů je stále k dispozici možnost.</span><span class="sxs-lookup"><span data-stu-id="83206-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="83206-119">Na základě silné zpětnou vazbu od komunity vývojářů ASP.NET, je však není požadavek.</span><span class="sxs-lookup"><span data-stu-id="83206-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-120">Serverová rozšíření FrontPage se stále vyžadují pro vytvoření vzdáleného projektu, otevření, atd.</span><span class="sxs-lookup"><span data-stu-id="83206-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="83206-121">Vývojový server ASP.NET</span><span class="sxs-lookup"><span data-stu-id="83206-121">ASP.NET Development Server</span></span>

<span data-ttu-id="83206-122">Visual Studio 2005 se dodává s názvem vývojový Server ASP.NET nový webový server.</span><span class="sxs-lookup"><span data-stu-id="83206-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="83206-123">(Tento webový server byl dříve označované jako Cassini.)</span><span class="sxs-lookup"><span data-stu-id="83206-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="83206-124">Existuje několik výhod vývojový Server ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83206-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="83206-125">Nyní je možné, kteří nejsou správci vyvíjet a ladit u webového serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="83206-126">Vývojový Server ASP.NET dynamicky mapuje virtuální adresáře na libovolné místo v systému souborů, který umožňuje flexibilní projektu umístění.</span><span class="sxs-lookup"><span data-stu-id="83206-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="83206-127">Uživatelé v systému Windows XP Professional, kteří jsou již pomocí služby IIS, teď bude moct vytvářet nové webové aplikace, které nebude mít vliv na strukturu souboru nebo složky z jejich výchozí web ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="83206-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="83206-128">Chcete využít výhod vývojový Server ASP.NET není nutná žádná speciální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="83206-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="83206-129">Pokud webový projekt, který je hostován v systému souborů je ladit nebo procházení, Visual Studio 2005 automaticky spustí instanci vývojový Server ASP.NET na náhodných portu zpracovat požadavek.</span><span class="sxs-lookup"><span data-stu-id="83206-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="83206-130">Další informace se bude vztahovat na vývojový Server ASP.NET později v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="83206-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="83206-131">Vylepšení správy</span><span class="sxs-lookup"><span data-stu-id="83206-131">Improved File Management</span></span>

<span data-ttu-id="83206-132">Soubor projektu (.vbproj pro VB.NET) a .csproj pro jazyk C# v sadě Visual Studio 2002 a 2003 uložené informace na všechny soubory ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83206-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="83206-133">V zobrazení Průzkumník řešení je založena na informace o souboru v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="83206-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="83206-134">Z tohoto důvodu by Průzkumník řešení často zobrazit nesprávné informace v případech, kdy se používá externí editory.</span><span class="sxs-lookup"><span data-stu-id="83206-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="83206-135">Visual Studio 2002 a 2003 by často přepsat změny souborů nebo nezobrazí nejnovější verzi souborů.</span><span class="sxs-lookup"><span data-stu-id="83206-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="83206-136">Visual Studio 2005 nepodporuje počítače do souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="83206-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="83206-137">Místo toho přečte informace souborů a složek přímo z disku, což vede přesné zobrazení souborů ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="83206-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="83206-138">Protože složce odkazy v sadě Visual Studio 2002 a 2003 nepředstavuje skutečnou složku ve webové aplikaci, Visual Studio 2005 také odebere složce odkazy v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="83206-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="83206-139">Pro přístup k odkazy pro projekt v sadě Visual Studio 2005, používejte na stránkách vlastností projektu.</span><span class="sxs-lookup"><span data-stu-id="83206-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="83206-140">Vytváření webové projekty</span><span class="sxs-lookup"><span data-stu-id="83206-140">Creating Web Projects</span></span>

<span data-ttu-id="83206-141">Vývojáři webů mít mnoho nové možnosti, které jsou k dispozici pro vytvoření projektu v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="83206-142">Weby teď vytvořením kdekoli v systému souborů a můžete pak ladit nebo procházet pomocí nové vývojový Server ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83206-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="83206-143">Vývojáři mohou také vytvářet nové webů pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="83206-144">Kliknutím sem můžete zobrazit na video s návodem vytvoření webové projekty v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="83206-145">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="83206-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="83206-146">Projektů v systému souborů</span><span class="sxs-lookup"><span data-stu-id="83206-146">File System Projects</span></span>

<span data-ttu-id="83206-147">Jak už jste viděli v video s návodem, můžete vytvářet weby v systému souborů v místním počítači nebo ve vzdáleném umístění prostřednictvím sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="83206-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="83206-148">Webové servery, které jsou vytvořené v systému souborů jsou Procházet a ladit pomocí vývojový Server ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83206-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-149">Vývojový Server ASP.NET nemusí být úplně jasné pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="83206-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="83206-150">Pokud je vytvoření webového projektu v systému souborů v IISs adresářovou strukturu (tj. c:\inetpub\wwwroot) na webu stále procházet prostřednictvím vývojový Server ASP.NET při spuštěn z prostředí Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-150">If a Web project is created on the file system in IISs directory structure (i.e. c:\inetpub\wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="83206-151">Proto všechny konfigurace služby IIS (tj. metody ověřování) není použitelná.</span><span class="sxs-lookup"><span data-stu-id="83206-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="83206-152">Výchozí webový projekt také odebere mnoho režie podle pouze obsahuje stránku Default.aspx, default.cs souboru a aplikace\_složku Data.</span><span class="sxs-lookup"><span data-stu-id="83206-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App\_Data folder.</span></span> <span data-ttu-id="83206-153">Soubor web.config a speciální složky (tj. aplikace\_kódu) jsou přidány jako jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="83206-153">The web.config and special folders (i.e. app\_code) are added as they are needed.</span></span> <span data-ttu-id="83206-154">Webový projekt zahrnuje jenom soubory a složky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="83206-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="83206-155">Projekty HTTP</span><span class="sxs-lookup"><span data-stu-id="83206-155">HTTP Projects</span></span>

<span data-ttu-id="83206-156">Projekty HTTP může být buď projekty, které jsou vytvořené na místní web služby IIS nebo na vzdálený webový server.</span><span class="sxs-lookup"><span data-stu-id="83206-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="83206-157">Výchozí umístění projektu je `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="83206-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="83206-158">Pokud kliknete na tlačítko Procházet, existují dvě možnosti HTTP: místní a vzdálené lokality.</span><span class="sxs-lookup"><span data-stu-id="83206-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="83206-159">Hlavní rozdíl v těchto dvou možností je metoda, ve kterém se zobrazují informace webu v dialogovém okně vyberte umístění a v tom, jak se soubory se zkopírují na webový server.</span><span class="sxs-lookup"><span data-stu-id="83206-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="83206-160">Možnost místní služba IIS načte informace o lokalitě z metabáze v místním počítači a kopírují soubory pomocí systému souborů.</span><span class="sxs-lookup"><span data-stu-id="83206-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="83206-161">Možnost vzdálené lokality používá serverová rozšíření FrontPage a informace o lokalitě a soubory budou zkopírovány pomocí protokolu HTTP a volání vzdáleného volání Procedur rozšíření FrontPage serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-162">Vs ###\_tmp.htm souboru a získání\_aspx\_ver.aspx se už používají k určení informací o verzi.</span><span class="sxs-lookup"><span data-stu-id="83206-162">The vs###\_tmp.htm file and get\_aspx\_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="83206-163">Výchozí možnost HTTP je místní služba IIS.</span><span class="sxs-lookup"><span data-stu-id="83206-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="83206-164">Tato možnost přečte metabáze služby IIS určit servery, které jsou k dispozici a umístění, do kterého chcete-li vytvořit obsah.</span><span class="sxs-lookup"><span data-stu-id="83206-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="83206-165">Ve stromovém zobrazení můžete vybrat jinou složku nebo virtuální adresář.</span><span class="sxs-lookup"><span data-stu-id="83206-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="83206-166">Můžete můžete také vytvořit nový virtuální adresář, označit složky jako aplikace, a také odstranit stávající virtuální adresáře z tohoto dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="83206-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![Zvolte umístění dialogové okno](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="83206-168">**Obrázek 1**: Zvolte umístění dialogové okno</span><span class="sxs-lookup"><span data-stu-id="83206-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="83206-169">Na rozdíl od v dřívějších verzích sady Visual Studio, pokud zaškrtnete **použití Secure Sockets Layer** zaškrtávací políčko a certifikátu SSL neodpovídá adresu URL procházíte, zobrazí se výstraha zabezpečení dialogové okno s dotazem, pokud byste má pokračovat.</span><span class="sxs-lookup"><span data-stu-id="83206-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="83206-170">Pomocí Visual Studio .NET 2003, je-li certifikát nebyl odpovídající jeden, vytváření projektu skončí s chybou.</span><span class="sxs-lookup"><span data-stu-id="83206-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![Certifikát SSL výstrahy týkající se zabezpečení](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="83206-172">**Obrázek 2**: výstraha zabezpečení týkající se certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="83206-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="83206-173">Poznámka: na hlavičky hostitele</span><span class="sxs-lookup"><span data-stu-id="83206-173">Note on Host Headers</span></span>

<span data-ttu-id="83206-174">Pokud vytváříte webovou aplikaci v lokalitě vázána na konkrétní IP adresu, musíte zajistit, že je nakonfigurovaný hlavičku hostitele.</span><span class="sxs-lookup"><span data-stu-id="83206-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="83206-175">Jinak, vytvoří sada Visual Studio na lokalitu `http://localhost`, ale pokud je web procházet nebo ladit z prostředí IDE nebude správně překlad adresy IP.</span><span class="sxs-lookup"><span data-stu-id="83206-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="83206-176">Pokud vyberete možnost vzdálené lokality, změní se dialogové okno a umožní vám zadat cílová adresa URL pro nový web.</span><span class="sxs-lookup"><span data-stu-id="83206-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="83206-177">Tato adresa URL musí být na server, který má serverová rozšíření FrontPage povolena.</span><span class="sxs-lookup"><span data-stu-id="83206-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="83206-178">Pokud chcete pracovat s vaší místní webový server používá serverová rozšíření FrontPage, můžete použít možnost vzdálené lokality a zadat místní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="83206-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![Vytvoření webu na vzdáleném serveru](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="83206-180">**Obrázek 3**: vytvoření webu na vzdáleném serveru</span><span class="sxs-lookup"><span data-stu-id="83206-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="83206-181">Při vytvoření aplikace ve vzdálené lokalitě přes SSL, pokud certifikát protokolu SSL neodpovídá, dialogové okno pro potvrzení se poněkud liší od dialogové okno zobrazí, když pomocí možnosti místní služba IIS.</span><span class="sxs-lookup"><span data-stu-id="83206-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![Výstrahy zabezpečení vzdálené lokality](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="83206-183">**Obrázek 4**: výstraha zabezpečení vzdálené lokality</span><span class="sxs-lookup"><span data-stu-id="83206-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="83206-184">FTP</span><span class="sxs-lookup"><span data-stu-id="83206-184">FTP</span></span>

<span data-ttu-id="83206-185">Visual Studio 2005 představuje možnost vytvářet weby přes FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="83206-186">Pokud použijete tuto možnost, rozhraní IDE vytvoří soubory místně v dočasné složce uživatele a potom pomocí FTP k přesunutí souborů do umístění FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-187">Umístění dočasné složky je c:\Documents and Settings\&lt; Uživatel&gt;\Local Settings\Temp\VWDWebCache\&lt; Server&gt;\_&lt;název aplikace&gt;</span><span class="sxs-lookup"><span data-stu-id="83206-187">The temp folder location is c:\Documents and Settings\&lt;User&gt;\Local Settings\Temp\VWDWebCache\&lt;Server&gt;\_&lt;application name&gt;</span></span>


<span data-ttu-id="83206-188">Při použití volby FTP, zobrazí se dialogové okno Vybrat umístění.</span><span class="sxs-lookup"><span data-stu-id="83206-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="83206-189">Můžete zadat požadované informace o připojení FTP do toto dialogové okno, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="83206-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![Výběr umístění dialogové okno pro FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="83206-191">**Obrázek 5**: Zvolte umístění dialogové okno pro FTP</span><span class="sxs-lookup"><span data-stu-id="83206-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="83206-192">Testovacího prostředí: Nastavení FTP lokality a vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="83206-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="83206-193">Následující kroky konfigurace serveru FTP tak, aby uživatel má umístění, které pouze jejich můžete uložit do přes FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="83206-194">Nainstalovat službu FTP</span><span class="sxs-lookup"><span data-stu-id="83206-194">Install the FTP Service</span></span>

1. <span data-ttu-id="83206-195">Otevřete odebrat programy, vyberte možnost Přidat nebo odebrat součásti systému Windows</span><span class="sxs-lookup"><span data-stu-id="83206-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="83206-196">Vyberte Internetová informační služba (aplikační Server v systému Windows 2003) a klikněte na **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="83206-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="83206-197">Zkontrolujte **protokol FTP (File Transfer) služby** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="83206-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="83206-198">Klikněte na tlačítko **Další** k instalaci služby FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="83206-199">Vytvořte novou složku pro obsah</span><span class="sxs-lookup"><span data-stu-id="83206-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="83206-200">V Průzkumníku Windows, vytvořte novou složku s názvem **uživatel1** uvnitř c:\inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="83206-200">In Windows Explorer, create a new folder called **User1** inside of c:\inetpub\wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="83206-201">Nakonfiguruje složek a oprávnění u složek.</span><span class="sxs-lookup"><span data-stu-id="83206-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="83206-202">Otevření modulu snap-in služby IIS z nástroje pro správu.</span><span class="sxs-lookup"><span data-stu-id="83206-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="83206-203">Nyní máte k serverům FTP složce pod uzlem název počítače.</span><span class="sxs-lookup"><span data-stu-id="83206-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="83206-204">Rozbalte položku **servery FTP**.</span><span class="sxs-lookup"><span data-stu-id="83206-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="83206-205">Klikněte pravým tlačítkem myši **výchozí server FTP**, vyberte **nový**, pak **virtuální adresář**, pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="83206-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="83206-206">Zadejte **uživatel1** pro název virtuálního adresáře a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="83206-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="83206-207">Zadejte **c:\inetpub\wwwroot\User1** pro cestu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="83206-207">Enter **c:\inetpub\wwwroot\User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="83206-208">Klikněte na tlačítko **Další** a potom **Dokončit** dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="83206-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="83206-209">Klikněte pravým tlačítkem myši **uživatel1** virtuálního adresáře v rámci výchozí server FTP a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="83206-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="83206-210">Zkontrolujte **zápisu** zaškrtávací políčko a klikněte na tlačítko **OK** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83206-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="83206-211">Klikněte pravým tlačítkem na **výchozí server FTP** a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="83206-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="83206-212">Na **účtů zabezpečení** kartě a zrušte zaškrtnutí políčka **povolit anonymní připojení**.</span><span class="sxs-lookup"><span data-stu-id="83206-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="83206-213">Klikněte na tlačítko **Ano** v dialogovém okně s dotazem, pokud chcete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="83206-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="83206-214">Klikněte na tlačítko **OK** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83206-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="83206-215">Rozbalte **Default Web Site** pod **weby** uzlu.</span><span class="sxs-lookup"><span data-stu-id="83206-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="83206-216">Klikněte pravým tlačítkem myši **uživatel1** adresář a zaškrtněte možnost **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="83206-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="83206-217">V **nastavení aplikace** klikněte na tlačítko **vytvořit** označit složky jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="83206-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="83206-218">Klikněte na tlačítko **OK** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83206-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="83206-219">Zavřete modul snap-in Internetová informační služba.</span><span class="sxs-lookup"><span data-stu-id="83206-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="83206-220">Vytvoření webového projektu</span><span class="sxs-lookup"><span data-stu-id="83206-220">Create web project</span></span>

1. <span data-ttu-id="83206-221">Otevřete Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="83206-222">Z **soubor** nabídce vyberte možnost **nový web**.</span><span class="sxs-lookup"><span data-stu-id="83206-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="83206-223">V **umístění** rozevíracího seznamu vyberte **FTP**.</span><span class="sxs-lookup"><span data-stu-id="83206-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="83206-224">Klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="83206-224">Click **Browse**.</span></span>
5. <span data-ttu-id="83206-225">Zadejte **localhost** v **Server** textové pole.</span><span class="sxs-lookup"><span data-stu-id="83206-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="83206-226">Zadejte **uživatel1** do textového pole adresáře.</span><span class="sxs-lookup"><span data-stu-id="83206-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="83206-227">Klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="83206-227">Click **Open**.</span></span> <span data-ttu-id="83206-228">Umístění FTP zadá do dialogu Nový web.</span><span class="sxs-lookup"><span data-stu-id="83206-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="83206-229">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="83206-229">Click **OK**.</span></span>
9. <span data-ttu-id="83206-230">Zrušte zaškrtnutí políčka **anonymní přihlášení** v dialogovém okně přihlášení k serveru FTP zadejte své přihlašovací údaje a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="83206-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="83206-231">Jaká je adresa URL pro projekt?</span><span class="sxs-lookup"><span data-stu-id="83206-231">What is the URL for the project?</span></span> <span data-ttu-id="83206-232">(Adresa URL pro projekt se zobrazí v Průzkumníku řešení.)</span><span class="sxs-lookup"><span data-stu-id="83206-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="83206-233">Z **sestavení** nabídce vyberte možnost **Sestavit web** nebo **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="83206-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="83206-234">Na stránce Default.aspx v Průzkumníku řešení klikněte pravým tlačítkem a vyberte **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="83206-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="83206-235">V dialogovém okně požadována adresa URL webu, zadejte `http://localhost/user1` pro adresu URL a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="83206-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-236">Pokud dojde k chybě, která udává, nebylo možné načíst typ \_výchozí, ujistěte se, že používáte technologii ASP.NET 2.0 na váš web a ne starší verze.</span><span class="sxs-lookup"><span data-stu-id="83206-236">If you get a error indicating an inability to load the type \_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="83206-237">Můžete to udělat na kartě ASP.NET v Internetové informační službě.</span><span class="sxs-lookup"><span data-stu-id="83206-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="83206-238">Otevírání webové projekty</span><span class="sxs-lookup"><span data-stu-id="83206-238">Opening Web Projects</span></span>

<span data-ttu-id="83206-239">Otevírání webových projektů je podobná vytváření projektů.</span><span class="sxs-lookup"><span data-stu-id="83206-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="83206-240">V následujících částech vyvolávající oblasti dohlížet na pro při práci v prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="83206-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="83206-241">Popisuje také práci s webovými projekty pomocí protokolu HTTP a FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="83206-242">Otevřete webový projekt, vyberte v nabídce Soubor otevřete web.</span><span class="sxs-lookup"><span data-stu-id="83206-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="83206-243">Zobrazí se výzva s stejné dialogové okno Vybrat umístění popsané dříve a máte stejné čtyři možnosti k dispozici: systém souborů, místní služba IIS, FTP a vzdálené lokality.</span><span class="sxs-lookup"><span data-stu-id="83206-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="83206-244">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="83206-244">File System</span></span>

<span data-ttu-id="83206-245">Jak je uvedeno dříve v tomto modulu, Visual Studio už používá soubor projektu.</span><span class="sxs-lookup"><span data-stu-id="83206-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="83206-246">Proto pokud zvolíte možnost Otevřít web ze systému souborů, ve skutečnosti máte možnost výběru libovolné složky, který chcete, i když na složku, kterou zvolíte nebyl vytvořen jako webový projekt původně v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83206-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="83206-247">Například můžete otevřít složku Dokumenty jako webový server a Visual Studio brouka otevřít a zobrazit vaše soubory, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="83206-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Moje dokumenty otevřen jako webovou stránku](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="83206-249">**Obrázek 6**: *Moje dokumenty* otevřen jako webovou stránku</span><span class="sxs-lookup"><span data-stu-id="83206-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="83206-250">Protože Visual Studio vytvoří jenom další soubory a složky, pokud je to nezbytné, žádné další soubory nebo složky se přidají do umístění, ve kterém můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="83206-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="83206-251">Vedlejším účinkem této architektury je, že ho brání vnoření webů v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="83206-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="83206-252">Zvažte například následující adresářovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="83206-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="83206-253">Webového projektu v C:\MyWebSite</span><span class="sxs-lookup"><span data-stu-id="83206-253">Web project at C:\MyWebSite</span></span>

<span data-ttu-id="83206-254">Jiný webový projekt v C:\MyWebSite\Nested</span><span class="sxs-lookup"><span data-stu-id="83206-254">Another web project at C:\MyWebSite\Nested</span></span>

<span data-ttu-id="83206-255">Když otevřete na webu na c:\MyWebSite, vnořené složky se zobrazí jako podsložku této aplikace.</span><span class="sxs-lookup"><span data-stu-id="83206-255">When you open the Web site at c:\MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="83206-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="83206-256">HTTP</span></span>

<span data-ttu-id="83206-257">Při otevírání webů pomocí protokolu HTTP, nastavení jsou přečtena buď z metabáze služby IIS (místní služba IIS) nebo používá serverová rozšíření FrontPage (vzdálený server.) Pokud jsou vnořené webové aplikace, tyto se zobrazí také ikonu, která označuje je jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="83206-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="83206-258">Pokud jste obeznámeni s práce s webovými aplikacemi v FrontPage, toto chování v sadě Visual Studio 2005 je podobné.</span><span class="sxs-lookup"><span data-stu-id="83206-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="83206-259">I když Visual Studio se zobrazí ikona pro aplikace, které jsou vnořené pod aplikace, který je aktuálně otevřený v prostředí IDE, nebude možné rozšířit jejich a zobrazit jejich obsah.</span><span class="sxs-lookup"><span data-stu-id="83206-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="83206-260">Klikněte dvakrát na můžete, ale, aby jejich otevření.</span><span class="sxs-lookup"><span data-stu-id="83206-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="83206-261">Když to uděláte, zobrazí se dialogové okno s požadavkem, abyste buď otevřete webovou aplikaci (a nahraďte aktuálně otevřené řešení) nebo přidat webové aplikace na vaše aktuální řešení.</span><span class="sxs-lookup"><span data-stu-id="83206-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![Dvakrát klikněte na ikonu vnořené aplikace nabídne vám toto dialogové okno](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="83206-263">**Obrázek 7**: dvakrát klikněte na ikonu vnořené aplikace nabídne vám toto dialogové okno</span><span class="sxs-lookup"><span data-stu-id="83206-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="83206-264">Server FTP</span><span class="sxs-lookup"><span data-stu-id="83206-264">FTP Site</span></span>

<span data-ttu-id="83206-265">Když otevřete web přes FTP, soubory se zkopírovat všechny místně do dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="83206-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="83206-266">Úplná cesta pro umístění místní úložiště se zobrazí v podokně vlastností projektu a je vytvořen v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="83206-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="83206-267">C:\Documents and Settings\&lt; Uživatel&gt;\Local Settings\Temp\VWDWebCache\&lt; Server&gt;\_&lt;název aplikace&gt;</span><span class="sxs-lookup"><span data-stu-id="83206-267">C:\Documents and Settings\&lt;User&gt;\Local Settings\Temp\VWDWebCache\&lt;Server&gt;\_&lt;application name&gt;</span></span>

<span data-ttu-id="83206-268">Pokud používáte FTP, Visual Studio bude třeba zadat základní adresu URL pro váš projekt, ve kterém můžete vyhledat, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="83206-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="83206-269">Pokud základní adresu URL nezadáte, Visual Studio zobrazí výzvu k jeho při prvním pokusu o procházení stránky na webu.</span><span class="sxs-lookup"><span data-stu-id="83206-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![Určení základní adresy URL pro servery FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="83206-271">**Obrázek 8**: určení základní adresy URL pro servery FTP</span><span class="sxs-lookup"><span data-stu-id="83206-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="83206-272">Vylepšení v kompilaci</span><span class="sxs-lookup"><span data-stu-id="83206-272">Improvements in Compilation</span></span>

<span data-ttu-id="83206-273">Práce s webové aplikace v sadě Visual Studio 2005 je výrazně rychlejší než v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="83206-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="83206-274">To je kvůli v žádné malou část změny v architektuře kompilace.</span><span class="sxs-lookup"><span data-stu-id="83206-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="83206-275">V sadě Visual Studio 2002 a 2003 byly zkompilovat webových aplikací do jednoho sestavení primární umístěný ve složce/Bin.</span><span class="sxs-lookup"><span data-stu-id="83206-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="83206-276">V sadě Visual Studio 2005, aplikace\_kód složka byla přidána.</span><span class="sxs-lookup"><span data-stu-id="83206-276">In Visual Studio 2005, an App\_Code folder was added.</span></span> <span data-ttu-id="83206-277">Třídy a jiný kód, bez uživatelského rozhraní se přidají do aplikace\_složky kódu.</span><span class="sxs-lookup"><span data-stu-id="83206-277">Classes and other non-UI code are added to the App\_Code folder.</span></span> <span data-ttu-id="83206-278">Když Visual Studio vytvoří projekt, všechny soubory v aplikaci\_kód složky jsou zkompilovány do jediné aplikaci\_Code.dll souboru.</span><span class="sxs-lookup"><span data-stu-id="83206-278">When Visual Studio builds the project, all files in the App\_Code folder are compiled into a single App\_Code.dll file.</span></span> <span data-ttu-id="83206-279">Výsledek této změny je mnohem rychlejší než v předchozích verzích následné sestavení.</span><span class="sxs-lookup"><span data-stu-id="83206-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-280">Nástroj příkazového řádku nástroje MSBuild lze také vytvářet ASP.NET – webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="83206-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="83206-281">Tento nástroj se budeme v modulu 9.</span><span class="sxs-lookup"><span data-stu-id="83206-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="83206-282">Další vylepšení kompilace je nová možnost vytvoření stránky v nabídce sestavení.</span><span class="sxs-lookup"><span data-stu-id="83206-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="83206-283">Tato funkce umožňuje vývojáři sestavit pouze aktuální stránku (spolu s, během a závislosti) tak, aby změny, které mohou být zkompilovány rychleji.</span><span class="sxs-lookup"><span data-stu-id="83206-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="83206-284">Protože C# nenabízí pozadí kompilace pro účely aktualizace IntelliSense, atd., se budou využívat znatelně tuto funkci vzhledem k tomu, že bude možné pro technologii IntelliSense, která aktualizovat rychle jednoduše znovu sestavit jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="83206-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="83206-285">Vlastnosti sestavení projektu umožňují konfigurovat typ sestavení, který se nachází před provedením úvodní stránce.</span><span class="sxs-lookup"><span data-stu-id="83206-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="83206-286">Vývojářům můžete vytvořit pouze aktuální stránky tak, aby Visual Studio můžete spustit ladění aplikací rychleji po změně kódu.</span><span class="sxs-lookup"><span data-stu-id="83206-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![Akce sestavení úvodní stránky](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="83206-288">**Obrázek 9**: akci spuštění stránky sestavení</span><span class="sxs-lookup"><span data-stu-id="83206-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="83206-289">Jiné skvělé vylepšení Architektura technologie ASP.NET a Visual Studio je v oblasti upravit a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="83206-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="83206-290">V sadě Visual Studio 2005 vývojáři můžou spustit ladění projektu a provádět změny kódu na projekt bez odpojení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="83206-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="83206-291">Ve skutečnosti můžete oznámena spustit ladění projektu, přidejte novou třídu, přidat kód do třídy, přidejte kód na stránku, která vytvoří novou instanci třídy a provedení metody třídy, aniž by museli odpojování ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="83206-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="83206-292">Provádění nový kód je oznámena stejně snadná jako aktualizace v prohlížeči!</span><span class="sxs-lookup"><span data-stu-id="83206-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="83206-293">Kliknutím sem zobrazit video s návodem upravit a pokračovat funkce v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="83206-294">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="83206-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="83206-295">Upravit a pokračovat funkce v technologii ASP.NET 2.0 robustní a Visual Studio 2005 je kvůli změně architektury pro aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83206-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="83206-296">V technologii ASP.NET 1.x, aplikace vytvořené v aplikaci Visual Studio 2002 nebo 2003 byly zkompilovány do primárních sestavení, která byla uložená ve složce/Bin.</span><span class="sxs-lookup"><span data-stu-id="83206-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="83206-297">Všechny třídy, stránkách atd pro aplikace se zkompiluje do tuto jednu knihovnu DLL.</span><span class="sxs-lookup"><span data-stu-id="83206-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="83206-298">Potom v době běhu ASP.NET by všechny ovládací prvky, značek a kódu ASP.NET v rámci stránky zkompilovat a zkopírujte tyto knihovny DLL do dočasné složky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83206-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="83206-299">V sadě Visual Studio 2005 pomocí technologie ASP.NET 2.0, kompilace dva modely popisují výše (jeden pro sadu Visual Studio) a jeden pro technologii ASP.NET v době běhu byly sloučeny do jednoho společný model kompilace.</span><span class="sxs-lookup"><span data-stu-id="83206-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="83206-300">To znamená, že všechny problémy kompilace jsou nyní zachyceny během fáze vývoje místo v době běhu.</span><span class="sxs-lookup"><span data-stu-id="83206-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="83206-301">Umožňuje také pro návrháře a podporu technologie IntelliSense pro funkce, jako je uživatelské ovládací prvky a stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="83206-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="83206-302">Kliknutím sem zobrazíte na video s návodem návrháře podpory pro uživatelské ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="83206-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="83206-303">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="83206-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="83206-304">Je-li uživatelský ovládací prvek odebrán ze stránky, @Register – direktiva zůstane ve značce a měla by být ručně odebrána, aby se zabránilo chybám analyzátor, je-li uživatelského ovládacího prvku odstraněna z webu.</span><span class="sxs-lookup"><span data-stu-id="83206-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="83206-305">Jiné zlepšování v modelu kompilace Visual Studio je funkce Publikovat Web.</span><span class="sxs-lookup"><span data-stu-id="83206-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="83206-306">Protože funkce publikování překompiluje webu, vývojářům umožňuje využívat přidané výkon nebudou muset nic na vyžádání zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="83206-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="83206-307">Je také překompiluje všechny zdrojového kódu v aplikaci\_Code složku do knihovny DLL tak, aby žádný zdrojový kód je nutné nasadit.</span><span class="sxs-lookup"><span data-stu-id="83206-307">It also precompiles all source code in the App\_Code folder into a DLL so that no source code has to be deployed.</span></span>


![Dialogové okno publikování webu](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="83206-309">**Obrázek 10**: dialogové okno publikování webu</span><span class="sxs-lookup"><span data-stu-id="83206-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="83206-310">Aspnet\_compile.exe nástroj lze také předem zkompilovat webovou aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83206-310">The aspnet\_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="83206-311">Tento nástroj se budeme v modulu 9.</span><span class="sxs-lookup"><span data-stu-id="83206-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="83206-312">Když budete publikovat webovou stránku, Předkompilované soubory jsou uloženy ve složce Temporary ASP.NET Files jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="83206-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="83206-313">Soubory s *Compiled* příponu souboru jsou soubory formátu XML, které definují závislosti pro konkrétní knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="83206-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="83206-314">Všechny ovládací prvky webových formulářů nebo uživatele se zkompiluje do náhodných knihovny DLL, které začínají *aplikace\_webové\_*.</span><span class="sxs-lookup"><span data-stu-id="83206-314">Any Webform or user controls are compiled into random DLLs that begin with *App\_Web\_*.</span></span>

<span data-ttu-id="83206-315">Pokud necháte *tento předkompilovaných webů aktualizovat* políčko zaškrtnuto, značek uvnitř své ovládací prvky webových formulářů a uživatel nebude předem kompilovaném do knihovny DLL, umožní vám provádět změny po nasazení.</span><span class="sxs-lookup"><span data-stu-id="83206-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="83206-316">Pokud si přejete zamknout kód tak, aby nejsou povoleny změny nasazeným obsahem, zrušte zaškrtnutí tohoto políčka.</span><span class="sxs-lookup"><span data-stu-id="83206-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="83206-317">*Použít pevné názvy a jednoho sestavení* políčko umožňuje, aby se každý stránka kompiluje do sestavení s názvem pevné vypnutí dávkové kompilace.</span><span class="sxs-lookup"><span data-stu-id="83206-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="83206-318">Ponechat toto pole není zaškrtnuto, můžete využít výhod dávkové kompilace.</span><span class="sxs-lookup"><span data-stu-id="83206-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="83206-319">*Povolení silné názvy na předkompilovaných sestavení* políčko umožňuje silné jméno – předkompilovaných sestavení.</span><span class="sxs-lookup"><span data-stu-id="83206-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-320">V technologii ASP.NET 1.x, sestavení se silným názvem bylo možné nainstalovat do globální mezipaměti sestavení (GAC).</span><span class="sxs-lookup"><span data-stu-id="83206-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="83206-321">V aplikaci ASP.NET 2.0 můžete se nevyžaduje instalace sestavení se silným názvem do mezipaměti GAC.</span><span class="sxs-lookup"><span data-stu-id="83206-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![Soubory předem kompilované aplikace ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="83206-323">**Obrázek 11**: soubory předem kompilované aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="83206-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="83206-324">V aplikaci výše uvedené se žádný soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="83206-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="83206-325">Pokud by došlo, ho by mít byla volána *PrecompiledApp.config* po publikování webu lokality procesu.</span><span class="sxs-lookup"><span data-stu-id="83206-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="83206-326">Vylepšení v nasazení</span><span class="sxs-lookup"><span data-stu-id="83206-326">Improvements in Deployment</span></span>

<span data-ttu-id="83206-327">Jako Visual Studio 2002 a 2003, Visual Studio 2005 nabízí funkce kopírování projektu.</span><span class="sxs-lookup"><span data-stu-id="83206-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="83206-328">Ale funkci má byla zvýšenou úložnou v sadě Visual Studio 2005 a se nyní označuje jako Kopírovat webový server.</span><span class="sxs-lookup"><span data-stu-id="83206-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="83206-329">Dialogové okno Kopírovat web je rozdělit na levém rámec a pravém rámečku.</span><span class="sxs-lookup"><span data-stu-id="83206-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="83206-330">Levý z nich se nazývá zdrojového webu a pravém rámečku se nazývá vzdáleného webového serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="83206-331">Jedna z věcí, které by mohly uvádět někteří vývojáři je, že webový server zobrazí v pravém rámečku není nutně vzdálené lokality.</span><span class="sxs-lookup"><span data-stu-id="83206-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="83206-332">Může to být lokality v místním systému souborů nebo v místní instanci služby IIS.</span><span class="sxs-lookup"><span data-stu-id="83206-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="83206-333">Kromě toho serveru zobrazí v levém rámečku není nutně zdrojového webu protože dialogové okno umožňuje publikovat ze vzdáleného webu *k* zdrojového webu.</span><span class="sxs-lookup"><span data-stu-id="83206-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="83206-334">Pokud kopírujete projektu vzdálený webový server, této lokality musí mít serverová rozšíření FrontPage na něm nainstalován.</span><span class="sxs-lookup"><span data-stu-id="83206-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="83206-335">Pokud ne, musíte připojit pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="83206-336">Na druhé straně Pokud kopírujete projektu k místní instanci služby IIS, serverová rozšíření FrontPage nejsou požadovány.</span><span class="sxs-lookup"><span data-stu-id="83206-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-337">Pokud pokusu o vytvoření nového webu v místní instanci služby IIS a instalují rozšíření FrontPage 2002 Server Extensions, zobrazí se chybová zpráva s oznámením, že vytváření webů, není podporován na serveru služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="83206-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="83206-338">V takovém případě máte možnost serverová rozšíření FrontPage 2000 instalací nebo odebráním serverová rozšíření FrontPage.</span><span class="sxs-lookup"><span data-stu-id="83206-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="83206-339">Pro funkci kopírování webu na video s návodem, klikněte sem.</span><span class="sxs-lookup"><span data-stu-id="83206-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="83206-340">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="83206-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="83206-341">Vylepšení ladění</span><span class="sxs-lookup"><span data-stu-id="83206-341">Improvements in Debugging</span></span>

<span data-ttu-id="83206-342">Existují čtyři klíčových vylepšení v ladění v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="83206-343">Ladění místně bez účtu správce je možné z pole.</span><span class="sxs-lookup"><span data-stu-id="83206-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="83206-344">False ve výchozím nastavení je nyní atribut ladění pro element kompilace.</span><span class="sxs-lookup"><span data-stu-id="83206-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="83206-345">Vzdálené ladění instalace a konfigurace je jednodušší než dřív.</span><span class="sxs-lookup"><span data-stu-id="83206-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="83206-346">Nyní můžete ladit webu otevřít pomocí umístění služby FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="83206-347">Ladění jako bez oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="83206-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="83206-348">Přidání vývojový Server ASP.NET umožňuje jiným uživatelům než správcům snadno ladění aplikací ASP.NET okamžitě po nasazení.</span><span class="sxs-lookup"><span data-stu-id="83206-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="83206-349">Pokud aplikace ASP.NET spuštěné v místním systému souborů je ladění, Visual Studio spustí vývojový Server ASP.NET v kontextu přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="83206-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="83206-350">Tento uživatel pak můžete ladit aplikace bez jakékoli dodatečné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="83206-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="83206-351">Ladění je False ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="83206-351">Debug is False by Default</span></span>

<span data-ttu-id="83206-352">V technologii ASP.NET 1.x, *ladění* atribut *kompilace* byl element v souboru web.config nastaven na *true* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="83206-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="83206-353">Má vždy doporučujeme, aby vývojáři nastavte tento atribut na *false* před nasazením aplikace do produkčního prostředí, ale protože většina vývojářů není plně chápu důsledky ponechat atribut ladění nastaven na Hodnota TRUE, jednoduše ponechání jej jako-je.</span><span class="sxs-lookup"><span data-stu-id="83206-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="83206-354">Nejzávažnějšího problém s má atribut ladění je nastaven na hodnotu true, zakáže ASP.NETs dávkové kompilace modelu.</span><span class="sxs-lookup"><span data-stu-id="83206-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="83206-355">Proto je každé stránce zkompilovat do samostatné knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="83206-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="83206-356">Pokud webu, které aplikace se skládá z tisíce stránek (ne unheard z jakýmikoli prostředky), která znamená, vytvoří se několika tisíc malé knihovny DLL v příslušné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83206-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="83206-357">I když tyto knihovny jsou malé velikost, nejsou načtena do žádné konkrétní umístění v paměti.</span><span class="sxs-lookup"><span data-stu-id="83206-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="83206-358">Proto způsobit fragmentace v systémové paměti a může přispívat k OutOfMemoryException výskytů.</span><span class="sxs-lookup"><span data-stu-id="83206-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="83206-359">V aplikaci ASP.NET 2.0 atribut ladění ve výchozím nastavení má hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="83206-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="83206-360">Protože jste už viděli, kdy vývojář debugs aplikace ASP.NET v sadě Visual Studio 2005, bude vyzván k přidá soubor web.config s povoleným laděním.</span><span class="sxs-lookup"><span data-stu-id="83206-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="83206-361">Díky tomu způsobuje stejné nevýhody, jež byly součástí ASP.NET 1.x, ale teď vývojář je jasně upozorněn, že atribut nastaven na hodnotu false před přesunutím aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="83206-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="83206-362">Vzdálené ladění instalací a konfigurací</span><span class="sxs-lookup"><span data-stu-id="83206-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="83206-363">V aplikaci Visual Studio 2002 nebo 2003, vzdálené ladění účinné Machine Debug Manager (mdm.exe) a proces vs7jit.exe.</span><span class="sxs-lookup"><span data-stu-id="83206-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="83206-364">Z tohoto důvodu řešení vzdáleného ladění problémů s často byl černé políčko pro zákazníky a byl často není mnohem lepší pro PSS.</span><span class="sxs-lookup"><span data-stu-id="83206-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="83206-365">Visual Studio 2005 odebere spoléhat na mdm.exe a vs7jit.exe procesů.</span><span class="sxs-lookup"><span data-stu-id="83206-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="83206-366">Místo toho nyní používá službu sledování vzdáleného ladění (msvsmon.exe).</span><span class="sxs-lookup"><span data-stu-id="83206-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="83206-367">Požadavek pro vzdálené ladění v sadě Visual Studio 2005 je poměrně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="83206-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="83206-368">Budete muset spustit msvsmon.exe na vzdáleném serveru před ladění.</span><span class="sxs-lookup"><span data-stu-id="83206-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="83206-369">Sledování vzdáleného ladění můžete nainstalovat z disku CD Visual Studio nebo můžete jednoduše spustit msvsmon.exe ze sdílené složky bez vůbec nic. instalace na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="83206-370">Při spuštění msvsmon.exe, je pravděpodobné, že bude stěžují porty blokována pro vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="83206-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="83206-371">Naštěstí můžete snadno odblokovat porty zprava. v dialogovém okně upozornění jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="83206-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Upozornění, že je brána Windows Firewall blokuje vzdálené ladění](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="83206-373">**Obrázek 12**: oznámení, aby brána Windows Firewall je blokování vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="83206-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="83206-374">Jakmile mají odblokováno porty potřebné pro ladění, zobrazí se sledování vzdáleného ladění, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="83206-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="83206-375">Z tohoto rozhraní můžete sledovat připojení a změnit oprávnění snadno ladění.</span><span class="sxs-lookup"><span data-stu-id="83206-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![Sledování vzdáleného ladění](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="83206-377">**Obrázek 13**: sledování vzdáleného ladění</span><span class="sxs-lookup"><span data-stu-id="83206-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="83206-378">Je také možné vzdáleně ladit webové aplikace otevřít přes FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="83206-379">Kroky jsou stejné jako ty dříve uvedené.</span><span class="sxs-lookup"><span data-stu-id="83206-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="83206-380">Nicméně budete muset zadat základní adresu URL pro procházení FTP projektu, jak je uvedeno výše v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="83206-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="83206-381">Laboratoř 2</span><span class="sxs-lookup"><span data-stu-id="83206-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="83206-382">Vzdálené ladění pomocí sady Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="83206-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="83206-383">Tato laboratoř vás provede vzdáleného ladění pomocí sady Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="83206-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="83206-384">Kliknutím sem získáte na video s návodem tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="83206-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="83206-385">Otevřete Video přes celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="83206-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="83206-386">Toto testovací prostředí vyžaduje, abyste měli dva počítače, jeden spuštěné Visual Studio 2005 a další spuštěné služby IIS 5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="83206-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="83206-387">Otevřete Visual Studio 2005 a vytvořit nový web na vzdáleném serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="83206-388">Můžete vytvořit na webu na vzdálené instanci služby IIS nebo prostřednictvím protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="83206-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="83206-389">Ze vzdáleného webového serveru vyhledejte msvsmon.exe na vývojovém počítači pomocí cesty UNC a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="83206-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="83206-390">Je výchozím umístěním pro msvsmon.exe \\server\c$ \Program Files\Microsoft Debugger\x86 8\Common7\IDE\Remote Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83206-390">The default location for msvsmon.exe is \\server\c$\Program Files\Microsoft Visual Studio 8\Common7\IDE\Remote Debugger\x86.</span></span>
2. <span data-ttu-id="83206-391">Pokud se zobrazí výzva k odblokování porty pro vzdálené ladění, učinit.</span><span class="sxs-lookup"><span data-stu-id="83206-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="83206-392">Z vývojovém počítači otevřete kódu pro Default.aspx a nastavit zarážky na stránce\_Load – metoda.</span><span class="sxs-lookup"><span data-stu-id="83206-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page\_Load method.</span></span>
4. <span data-ttu-id="83206-393">Spusťte ladění z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="83206-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="83206-394">Zarážce by měl dosáhl podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="83206-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="83206-395">Vývojový server ASP.NET</span><span class="sxs-lookup"><span data-stu-id="83206-395">ASP.NET Development Server</span></span>

<span data-ttu-id="83206-396">Jako weve již popsané Visual Studio 2005 se dodává s názvem vývojový Server ASP.NET webového serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="83206-397">(Vývojový Server ASP.NET se někdy označuje jako Cassini.) Tento webový server je pohodlný způsob, jak procházet a ladění webových aplikací spuštěných v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="83206-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="83206-398">Vývojový Server ASP.NET je omezená webového serveru.</span><span class="sxs-lookup"><span data-stu-id="83206-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="83206-399">Neumožňuje vzdáleného připojení, není možné všechny žádosti, z libovolného uživatele než uživatele, který webový server.</span><span class="sxs-lookup"><span data-stu-id="83206-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="83206-400">Také nemá schopnost poskytovat služby na stránkách ASP.</span><span class="sxs-lookup"><span data-stu-id="83206-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="83206-401">Zpracovává jenom ASP.NET prostředky a prostředky HTML (včetně obrázků, souborů CSS atd.).</span><span class="sxs-lookup"><span data-stu-id="83206-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="83206-402">Vývojový Server ASP.NET může být spuštěn prostřednictvím příkazového řádku spuštěním souboru WebDev.WebServer.exe nacházející se v c:\Windows\Microsoft.NET\Framework\v2.0. \*\*\*\*\*.</span><span class="sxs-lookup"><span data-stu-id="83206-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*\*\*.</span></span> <span data-ttu-id="83206-403">Následující dialogové okno zobrazí parametry, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="83206-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="83206-404">**Obrázek 14**</span><span class="sxs-lookup"><span data-stu-id="83206-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="83206-405">Vývojový Server ASP.NET není podporován při spuštění explicitně prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="83206-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
