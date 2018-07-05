---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Vylepšení v sadě Visual Studio 2005 | Dokumentace Microsoftu
author: microsoft
description: Visual Studio 2005 poskytuje vývojáře webových aplikací s dlouhým seznamem vylepšení a rozšíření pro webové projekty.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 35ebb6e94c3da105b802c843e36f193f81a45782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397636"
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="c72a3-103">Vylepšení v sadě Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="c72a3-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="c72a3-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c72a3-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c72a3-105">Visual Studio 2005 poskytuje vývojáře webových aplikací s dlouhým seznamem vylepšení a rozšíření pro webové projekty.</span><span class="sxs-lookup"><span data-stu-id="c72a3-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="c72a3-106">Visual Studio 2005 poskytuje vývojáře webových aplikací s dlouhým seznamem vylepšení a rozšíření pro webové projekty.</span><span class="sxs-lookup"><span data-stu-id="c72a3-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="c72a3-107">Výkonné jsou Visual Studio .NET 2002 a 2003, došlo k mnoha stížností způsobem, že byly zpracovány webové projekty.</span><span class="sxs-lookup"><span data-stu-id="c72a3-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="c72a3-108">Visual Studio 2005 přidá řeší tyto stížností velký počet nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="c72a3-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="c72a3-109">Pro ty, kteří dávají přednost tak, že Visual Studio .NET 2003 zpracování kompilace webových aplikací, najdete v článku [projekty webových aplikací](https://go.microsoft.com/fwlink/?LinkId=57870).</span><span class="sxs-lookup"><span data-stu-id="c72a3-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="c72a3-110">V tomto modulu zahrnují také vylepšení vytváření webového projektu, správu a vývoj.</span><span class="sxs-lookup"><span data-stu-id="c72a3-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="c72a3-111">Novější modul a obsahuje vylepšení vytváření webových projektů a jejich nasazování.</span><span class="sxs-lookup"><span data-stu-id="c72a3-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="c72a3-112">Rozšíření serveru FrontPage</span><span class="sxs-lookup"><span data-stu-id="c72a3-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="c72a3-113">Visual Studio .NET 2002 a 2003 vyžaduje rozšíření serveru FrontPage v poli, aby bylo možné vytvářet nebo sestavovat webové projekty.</span><span class="sxs-lookup"><span data-stu-id="c72a3-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="c72a3-114">Možnost volby mezi dvěma režimy různý přístup (soubor nebo rozšíření serveru FrontPage režim přístupu) mají vývojáři, používá rozšíření serveru FrontPage provádět úkoly, jako je nastavení kořenový adresář aplikace v IIS atd.</span><span class="sxs-lookup"><span data-stu-id="c72a3-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="c72a3-115">Visual Studio 2005 odebere závislost na rozšíření serveru FrontPage pro místních projektů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="c72a3-116">Visual Studio 2005 teď přistupuje k metabázi služby IIS přímo namísto použití rozšíření serveru FrontPage.</span><span class="sxs-lookup"><span data-stu-id="c72a3-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="c72a3-117">Visual Studio 2005 také přidává podporu pro FTP, který umožňuje vzdálený projekt bez nutnosti rozšíření serveru FrontPage.</span><span class="sxs-lookup"><span data-stu-id="c72a3-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="c72a3-118">Pro vývojáře, kteří chtějí používat rozšíření serveru FrontPage ve svých projektech je stále k dispozici možnost.</span><span class="sxs-lookup"><span data-stu-id="c72a3-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="c72a3-119">Ale na základě silné zpětnou vazbu od komunity vývojářů technologie ASP.NET, není povinné.</span><span class="sxs-lookup"><span data-stu-id="c72a3-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-120">Rozšíření serveru FrontPage se stále vyžadují pro vytvoření vzdáleného projektu, otevření, atd.</span><span class="sxs-lookup"><span data-stu-id="c72a3-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="c72a3-121">Vývojový server ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c72a3-121">ASP.NET Development Server</span></span>

<span data-ttu-id="c72a3-122">Visual Studio 2005 se dodává s názvem serveru ASP.NET Development Server nový webový server.</span><span class="sxs-lookup"><span data-stu-id="c72a3-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="c72a3-123">(Tento webový server se dřív označovalo jako Cassini.)</span><span class="sxs-lookup"><span data-stu-id="c72a3-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="c72a3-124">Existuje několik výhod serveru ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="c72a3-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="c72a3-125">Nyní je možné, kteří nejsou správci vyvíjet a ladit na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="c72a3-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="c72a3-126">Virtuální adresáře serveru ASP.NET Development Server mapuje dynamicky do libovolného umístění v systému souborů umožňuje flexibilní projektu umístění.</span><span class="sxs-lookup"><span data-stu-id="c72a3-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="c72a3-127">Uživatelé systému Windows XP Professional, kteří již používají službu IIS teď budou moct vytvořit nové webové aplikace, které ovlivňují strukturu souboru nebo složky z jejich výchozí webový server ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="c72a3-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="c72a3-128">Abyste mohli využívat serveru ASP.NET Development Server není nutná žádná zvláštní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="c72a3-129">Když webový projekt, který je hostován v systému souborů se ladit nebo procházet, Visual Studio 2005, automaticky spustí instanci serveru ASP.NET Development Server na náhodný port ke zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="c72a3-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="c72a3-130">Další informace se bude vztahovat na serveru ASP.NET Development Server později v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="c72a3-131">Vylepšení správy</span><span class="sxs-lookup"><span data-stu-id="c72a3-131">Improved File Management</span></span>

<span data-ttu-id="c72a3-132">Soubor projektu (.vbproj pro VB.NET) a csproj pro jazyk C# v sadě Visual Studio 2002 a 2003, uloženy informace u všech souborů ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c72a3-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="c72a3-133">Zobrazení Průzkumníka řešení je založena na informace o souboru v souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="c72a3-134">Z tohoto důvodu by Průzkumník řešení často zobrazit nesprávné informace v případech, ve kterém byly použity externích editorech.</span><span class="sxs-lookup"><span data-stu-id="c72a3-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="c72a3-135">Visual Studio 2002 a 2003 by často přepsat změny souborů nebo nejnovější verzi souborů se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="c72a3-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="c72a3-136">Visual Studio 2005 se hned do souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="c72a3-137">Místo toho přečte informace, souborům a složkám přímo z disku a výsledkem je přesné zobrazení souborů ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="c72a3-138">Protože složka s odkazy v sadě Visual Studio 2002 a 2003 nepředstavuje skutečný složky ve webové aplikaci, Visual Studio 2005 na složku odkazy taky odebere z Průzkumníka řešení.</span><span class="sxs-lookup"><span data-stu-id="c72a3-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="c72a3-139">Pro přístup k odkazy pro váš projekt v sadě Visual Studio 2005, používejte stránky vlastností pro projekt.</span><span class="sxs-lookup"><span data-stu-id="c72a3-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="c72a3-140">Vytváření webových projektů</span><span class="sxs-lookup"><span data-stu-id="c72a3-140">Creating Web Projects</span></span>

<span data-ttu-id="c72a3-141">Weboví vývojáři mají spousta nových možností, které jsou k dispozici pro vytvoření projektu v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="c72a3-142">Web sites teď je možné vytvářet kdekoli v systému souborů a pak můžete ladit nebo procházet pomocí nového serveru ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="c72a3-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="c72a3-143">Vývojáři mohou také vytvářet nové weby pomocí FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="c72a3-144">Kliknutím sem zobrazíte video s návodem, vytváření webových projektů v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="c72a3-145">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="c72a3-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="c72a3-146">Projekty systému souborů</span><span class="sxs-lookup"><span data-stu-id="c72a3-146">File System Projects</span></span>

<span data-ttu-id="c72a3-147">Jak už jste viděli v video s názorným postupem, můžete vytvářet weby v systému souborů na místním počítači nebo ve vzdáleném umístění prostřednictvím sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c72a3-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="c72a3-148">Webové stránky, které jsou vytvořeny v systému souborů jsou Procházet a ladit pomocí serveru ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="c72a3-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-149">Vývojový Server ASP.NET může způsobit jisté zmatení pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="c72a3-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="c72a3-150">Pokud webový projekt je vytvořen v systému souborů do struktury adresářů IISs (například c:/inetpub/wwwroot), na webu společnosti stále procházet přes vývojový Server ASP.NET při spuštění z v rámci sady Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="c72a3-151">Proto všechny konfigurace služby IIS (tj. metody ověřování) se nevztahuje.</span><span class="sxs-lookup"><span data-stu-id="c72a3-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="c72a3-152">Výchozí webový projekt odebere také mnohem zatížení podle pouze obsahuje stránku Default.aspx default.cs soubor a složku aplikace/_Data.</span><span class="sxs-lookup"><span data-stu-id="c72a3-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="c72a3-153">Soubor web.config a speciální složky (například aplikace/_fragmenty) jsou přidány jako nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="c72a3-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="c72a3-154">Webový projekt obsahuje pouze soubory a složky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="c72a3-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="c72a3-155">Projekty HTTP</span><span class="sxs-lookup"><span data-stu-id="c72a3-155">HTTP Projects</span></span>

<span data-ttu-id="c72a3-156">Projekty, které jsou vytvořeny na web místní služby IIS nebo na vzdálený web může být buď HTTP projekty.</span><span class="sxs-lookup"><span data-stu-id="c72a3-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="c72a3-157">Výchozí umístění projektu `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c72a3-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="c72a3-158">Pokud kliknete na tlačítko Procházet, existují dvě možnosti protokolu HTTP: místní a vzdálené lokality.</span><span class="sxs-lookup"><span data-stu-id="c72a3-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="c72a3-159">Hlavní rozdíl v těchto dvou možností je metoda, ve kterém se zobrazí informace o webu v dialogovém okně Zvolit umístění a v tom, jak soubory se zkopírují na webový server.</span><span class="sxs-lookup"><span data-stu-id="c72a3-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="c72a3-160">Možnost místní služba IIS načte informace o lokalitě z metabáze na místním počítači a soubory se zkopírují pomocí systému souborů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="c72a3-161">Možnost vzdálené lokality používá rozšíření serveru FrontPage a informací o lokalitě a soubory se zkopírují pomocí protokolu HTTP a volání RPC rozšíření serveru FrontPage.</span><span class="sxs-lookup"><span data-stu-id="c72a3-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-162">Get/_aspx/_ver.aspx a vs###/_tmp.htm souboru se už používají k určení informací o verzi.</span><span class="sxs-lookup"><span data-stu-id="c72a3-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="c72a3-163">Výchozí možnost HTTP je místní služba IIS.</span><span class="sxs-lookup"><span data-stu-id="c72a3-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="c72a3-164">Tato možnost čtení metabáze služby IIS určit weby, které jsou k dispozici a oblasti, ve kterém chcete vytvořit obsah.</span><span class="sxs-lookup"><span data-stu-id="c72a3-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="c72a3-165">Výběrem ve stromovém zobrazení můžete vybrat jinou složku nebo virtuálního adresáře.</span><span class="sxs-lookup"><span data-stu-id="c72a3-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="c72a3-166">Je můžete také vytvořit nový virtuální adresář, označit složky jako aplikace, jakož i odstranit existující virtuální adresáře z tohoto dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c72a3-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![Zvolte umístění](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="c72a3-168">**Obrázek 1**: Zvolte umístění</span><span class="sxs-lookup"><span data-stu-id="c72a3-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="c72a3-169">Na rozdíl od v dřívějších verzích sady Visual Studio, pokud zaškrtnete **použít zabezpečení SSL** zaškrtávací políčko a certifikát protokolu SSL se neshoduje s URL procházení, zobrazí se výstraha zabezpečení dialogové okno s dotazem, pokud byste byli Chcete-li pokračovat, jako jsou.</span><span class="sxs-lookup"><span data-stu-id="c72a3-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="c72a3-170">Pomocí Visual Studio .NET 2003, pokud certifikát není odpovídající jedné, vytvoření projektu selže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![Certifikát SSL výstrahy týkající se zabezpečení](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="c72a3-172">**Obrázek 2**: výstraha zabezpečení týkající se certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="c72a3-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="c72a3-173">Poznámka: na hlavičky hostitele</span><span class="sxs-lookup"><span data-stu-id="c72a3-173">Note on Host Headers</span></span>

<span data-ttu-id="c72a3-174">Pokud vytváříte webovou aplikaci na webu vázán na konkrétní IP adresu, musíte zajistit konfiguraci hlavičku hostitele.</span><span class="sxs-lookup"><span data-stu-id="c72a3-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="c72a3-175">Jinak, vytvoří Visual Studio na webu `http://localhost`, ale když je web procházet nebo ladit z integrovaného vývojového prostředí nebude správně přeložit IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="c72a3-176">Pokud vyberete možnost vzdálené lokality, dialogové okno se změní na umožňují zadat cílovou adresu URL pro nový web.</span><span class="sxs-lookup"><span data-stu-id="c72a3-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="c72a3-177">Tato adresa URL musí být na serveru, který má povolené rozšíření serveru FrontPage.</span><span class="sxs-lookup"><span data-stu-id="c72a3-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="c72a3-178">Pokud chcete pracovat s vaší místní webový server pomocí rozšíření serveru FrontPage, můžete použít možnost vzdálené lokality a zadat místní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c72a3-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![Vytvoření webu na vzdáleném serveru](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="c72a3-180">**Obrázek 3**: vytvoření webu na vzdáleném serveru</span><span class="sxs-lookup"><span data-stu-id="c72a3-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="c72a3-181">Při vytváření aplikace ve vzdálené lokalitě přes SSL, pokud certifikát protokolu SSL se neshoduje s potvrzovací dialogové okno se mírně liší od dialogové okno zobrazí, když pomocí možnosti místní služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c72a3-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![Výstraha zabezpečení vzdáleného webu](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="c72a3-183">**Obrázek 4**: výstraha zabezpečení vzdáleného webu</span><span class="sxs-lookup"><span data-stu-id="c72a3-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="c72a3-184">FTP</span><span class="sxs-lookup"><span data-stu-id="c72a3-184">FTP</span></span>

<span data-ttu-id="c72a3-185">Visual Studio 2005 představuje možnost vytvářet weby přes protokol FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="c72a3-186">Když použijete tuto možnost, rozhraní IDE místně vytváří soubory ve složce temp uživatele a potom pomocí FTP přesuňte soubory do umístění serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-187">Umístění složky temp je c:/Documents and Settings /&lt;uživatele&gt;/místní nastavení/Temp/VWDWebCache/&lt;Server&gt;/_&lt;název aplikace&gt;</span><span class="sxs-lookup"><span data-stu-id="c72a3-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="c72a3-188">Při použití možnosti FTP, zobrazí se dialogové okno Zvolit umístění.</span><span class="sxs-lookup"><span data-stu-id="c72a3-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="c72a3-189">Zadejte požadované informace o připojení FTP do tohoto dialogového okna, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![Zvolte umístění pro službu FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="c72a3-191">**Obrázek 5**: Zvolte umístění pro službu FTP</span><span class="sxs-lookup"><span data-stu-id="c72a3-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="c72a3-192">Testovacího prostředí: Nastavení FTP lokalit a vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="c72a3-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="c72a3-193">Následující kroky konfigurace serveru FTP tak, aby uživatel měl umístění, ke kterému pouze jejich nahrání do přes protokol FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="c72a3-194">Služba FTP instaluje</span><span class="sxs-lookup"><span data-stu-id="c72a3-194">Install the FTP Service</span></span>

1. <span data-ttu-id="c72a3-195">Otevřete přidat nebo odebrat programy, vyberte Přidat nebo odebrat součásti Windows</span><span class="sxs-lookup"><span data-stu-id="c72a3-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="c72a3-196">Vyberte Internetové informační služby (aplikační Server na Windows serveru 2003) a klikněte na tlačítko **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="c72a3-197">Zkontrolujte **služby protokolu FTP (File Transfer)** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="c72a3-198">Klikněte na tlačítko **Další** k instalaci služby FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="c72a3-199">Vytvořte novou složku pro obsah</span><span class="sxs-lookup"><span data-stu-id="c72a3-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="c72a3-200">V Průzkumníku Windows, vytvořte novou složku s názvem **User1** uvnitř c:/inetpub/wwwroot.</span><span class="sxs-lookup"><span data-stu-id="c72a3-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="c72a3-201">Nakonfigurujte složek a oprávnění pro složky.</span><span class="sxs-lookup"><span data-stu-id="c72a3-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="c72a3-202">Otevřete modul snap-in Internetové informační služby z nástroje pro správu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="c72a3-203">Nyní máte složku servery FTP podle uzlu název počítače.</span><span class="sxs-lookup"><span data-stu-id="c72a3-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="c72a3-204">Rozbalte **servery FTP**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="c72a3-205">Klikněte pravým tlačítkem myši **výchozí server FTP**, vyberte **nový**, pak **virtuální adresář**, klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="c72a3-206">Zadejte **User1** název virtuálního adresáře a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="c72a3-207">Zadejte **c:/inetpub/wwwroot/User1** cestu a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="c72a3-208">Klikněte na tlačítko **Další** a potom **Dokončit** kroky průvodce dokončete.</span><span class="sxs-lookup"><span data-stu-id="c72a3-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="c72a3-209">Klikněte pravým tlačítkem myši **User1** virtuální adresář v rámci výchozí server FTP a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="c72a3-210">Zkontrolujte **zápisu** zaškrtávací políčko a klikněte na tlačítko **OK** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c72a3-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="c72a3-211">Klikněte pravým tlačítkem na **výchozí server FTP** a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="c72a3-212">Na **účty zabezpečení** kartu, zrušte zaškrtnutí políčka **povolit anonymní připojení**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="c72a3-213">Klikněte na tlačítko **Ano** v dialogovém okně s dotazem, jestli chcete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c72a3-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="c72a3-214">Klikněte na tlačítko **OK** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c72a3-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="c72a3-215">Rozbalte **výchozí webový server** pod **weby** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="c72a3-216">Klikněte pravým tlačítkem myši **User1** adresář a zaškrtnout možnost **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="c72a3-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="c72a3-217">V **nastavení aplikace** klikněte na tlačítko **vytvořit** k označení složce jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="c72a3-218">Klikněte na tlačítko **OK** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c72a3-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="c72a3-219">Zavřete modul snap-in Internetová informační služba.</span><span class="sxs-lookup"><span data-stu-id="c72a3-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="c72a3-220">Vytvoření webového projektu</span><span class="sxs-lookup"><span data-stu-id="c72a3-220">Create web project</span></span>

1. <span data-ttu-id="c72a3-221">Otevřít Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="c72a3-222">Z **souboru** nabídce vyberte možnost **nový web**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="c72a3-223">V **umístění** rozevíracího seznamu vyberte **FTP**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="c72a3-224">Klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-224">Click **Browse**.</span></span>
5. <span data-ttu-id="c72a3-225">Zadejte **localhost** v **Server** textového pole.</span><span class="sxs-lookup"><span data-stu-id="c72a3-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="c72a3-226">Zadejte **User1** v textovém poli adresáře.</span><span class="sxs-lookup"><span data-stu-id="c72a3-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="c72a3-227">Klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-227">Click **Open**.</span></span> <span data-ttu-id="c72a3-228">Umístění FTP, zadáme do dialogového okna Nový web.</span><span class="sxs-lookup"><span data-stu-id="c72a3-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="c72a3-229">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-229">Click **OK**.</span></span>
9. <span data-ttu-id="c72a3-230">Zrušte zaškrtnutí políčka **Ymní přihlášení** v dialogovém okně FTP přihlášení zadejte své přihlašovací údaje a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="c72a3-231">Jaká je adresa URL projektu?</span><span class="sxs-lookup"><span data-stu-id="c72a3-231">What is the URL for the project?</span></span> <span data-ttu-id="c72a3-232">(Adresa URL pro projekt se zobrazí v Průzkumníku řešení.)</span><span class="sxs-lookup"><span data-stu-id="c72a3-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="c72a3-233">Z **sestavení** nabídce vyberte možnost **Sestavit web** nebo **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="c72a3-234">Klikněte pravým tlačítkem myši na Default.aspx v Průzkumníku řešení a vyberte **zobrazit v prohlížeči**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="c72a3-235">V dialogovém okně vyžaduje adresu URL webu, zadejte `http://localhost/user1` pro adresu URL a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-236">Pokud se zobrazí chyba oznamující nebylo možné načíst typ /_Default, ujistěte se, že používáte technologii ASP.NET 2.0 na váš web a ne starší verze.</span><span class="sxs-lookup"><span data-stu-id="c72a3-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="c72a3-237">Můžete to udělat na kartě technologie ASP.NET v Internetové informační služby.</span><span class="sxs-lookup"><span data-stu-id="c72a3-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="c72a3-238">Otevírání webových projektů</span><span class="sxs-lookup"><span data-stu-id="c72a3-238">Opening Web Projects</span></span>

<span data-ttu-id="c72a3-239">Otevírání webových projektů je podobné jako vytvoření projektů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="c72a3-240">V dalších částech upozorňujte na oblasti, jak dohlížet navýšení kapacity pro při práci v rámci rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="c72a3-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="c72a3-241">Věnuje se také práci s projekty Web pomocí protokolu HTTP a protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="c72a3-242">Otevřete webový projekt, vyberte v nabídce Soubor otevřete webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="c72a3-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="c72a3-243">Zobrazí se výzva s stejné dialogového okna zvolte umístění popsané dříve a mají stejné čtyři dostupné možnosti: systém souborů, místní služby IIS, FTP a vzdálené lokality.</span><span class="sxs-lookup"><span data-stu-id="c72a3-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="c72a3-244">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="c72a3-244">File System</span></span>

<span data-ttu-id="c72a3-245">Jak je uvedeno dříve v tomto modulu, Visual Studio už používá soubor projektu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="c72a3-246">Proto pokud budete chtít otevřít web ze systému souborů, ve skutečnosti máte možnost vybrat libovolnou složku, pro kterou chcete, i v případě, že složka, kterou zvolíte nebyl vytvořen jako webový projekt zpočátku v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c72a3-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="c72a3-247">Například můžete také otevřít složku Dokumenty jako webový server a Visual Studio využívá elastic otevřít a zobrazit soubory, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Dokumenty otevřít webovou stránku](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="c72a3-249">**Obrázek 6**: *dokumenty* otevřít webovou stránku</span><span class="sxs-lookup"><span data-stu-id="c72a3-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="c72a3-250">Vzhledem k tomu, že sada Visual Studio pouze vytvoří další soubory a složky, pokud je to nezbytné, žádné další soubory nebo složky přidají do umístění, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="c72a3-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="c72a3-251">Vedlejším účinkem této architektury je, že to brání vnoření webů v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="c72a3-252">Představte si třeba následující adresářovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="c72a3-253">Webový projekt na C:/MyWebSite</span><span class="sxs-lookup"><span data-stu-id="c72a3-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="c72a3-254">Jiný webový projekt na C:/MyWebSite/vnořené</span><span class="sxs-lookup"><span data-stu-id="c72a3-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="c72a3-255">Při otevření webové stránky na c:/MyWebSite vnořené složky se zobrazí jako dílčí složku této aplikace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="c72a3-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="c72a3-256">HTTP</span></span>

<span data-ttu-id="c72a3-257">Při otevírání webů přes protokol HTTP, nastavení jsou přečtena z metabáze služby IIS (místní služby IIS) nebo pomocí rozšíření serveru FrontPage (vzdálené lokality.) Pokud jsou vnořené webové aplikace, se také zobrazí s ikonou je identifikuje jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="c72a3-258">Pokud jste se seznámili s práce s webovými aplikacemi v FrontPage, se podobá chování v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="c72a3-259">I když Visual Studio se zobrazí ikona pro aplikace, které jsou vnořené pod aplikace, která je aktuálně otevřen v integrovaném vývojovém prostředí, nebude možné rozšířit je chcete zobrazit jejich obsah.</span><span class="sxs-lookup"><span data-stu-id="c72a3-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="c72a3-260">Můžete však poklikejte na je tak, aby je.</span><span class="sxs-lookup"><span data-stu-id="c72a3-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="c72a3-261">Když použijete, zobrazí se dialogové okno s výzvou k buď otevřete webovou aplikaci (a nahraďte aktuálně otevřené řešení) nebo přidat webovou aplikaci s aktuálním řešením.</span><span class="sxs-lookup"><span data-stu-id="c72a3-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![Dvojitým kliknutím ikonu vnořené aplikace zobrazí toto dialogové okno](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="c72a3-263">**Obrázek 7**: dvojitým kliknutím ikonu vnořené aplikace zobrazí toto dialogové okno</span><span class="sxs-lookup"><span data-stu-id="c72a3-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="c72a3-264">Server FTP</span><span class="sxs-lookup"><span data-stu-id="c72a3-264">FTP Site</span></span>

<span data-ttu-id="c72a3-265">Při otevření webu přes protokol FTP souborů všechny zkopírují místně do dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="c72a3-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="c72a3-266">Úplná cesta k umístění místního úložiště se zobrazí v podokně vlastností projektu a je vytvořen v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="c72a3-267">C:/Documents and Settings /&lt;uživatele&gt;/místní nastavení/Temp/VWDWebCache/&lt;Server&gt;/_&lt;název aplikace&gt;</span><span class="sxs-lookup"><span data-stu-id="c72a3-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="c72a3-268">Při použití protokolu FTP, Visual Studio bude třeba zadat základní adresu URL pro váš projekt tak, aby ho můžete procházet, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="c72a3-269">Pokud základní adresu URL nezadáte, Visual Studio se vás o ně požádáme poprvé pokusí procházet stránku na webu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![Určení základní adresy URL pro servery FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="c72a3-271">**Obrázek 8**: určení základní adresy URL pro servery FTP</span><span class="sxs-lookup"><span data-stu-id="c72a3-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="c72a3-272">Vylepšení v kompilaci</span><span class="sxs-lookup"><span data-stu-id="c72a3-272">Improvements in Compilation</span></span>

<span data-ttu-id="c72a3-273">Práce s webovými aplikacemi v sadě Visual Studio 2005 je výrazně rychlejší než předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="c72a3-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="c72a3-274">Toto je kvůli žádné malou část změny v architektuře kompilace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="c72a3-275">V sadě Visual Studio 2002 a 2003 webové aplikace byly zkompilovány do jediného primární sestavení, které se nacházejí ve složce/Bin.</span><span class="sxs-lookup"><span data-stu-id="c72a3-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="c72a3-276">V sadě Visual Studio 2005 byla přidána k aplikaci/_fragmenty složce.</span><span class="sxs-lookup"><span data-stu-id="c72a3-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="c72a3-277">Třídy a jiný kód bez uživatelského rozhraní se přidají do složky aplikace/_fragmenty.</span><span class="sxs-lookup"><span data-stu-id="c72a3-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="c72a3-278">Když Visual Studio vytvoří projekt, všechny soubory ve složce aplikace/_fragmenty jsou kompilovány do jediného souboru App/_Code.dll.</span><span class="sxs-lookup"><span data-stu-id="c72a3-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="c72a3-279">Výsledkem této změny je, že následující sestavení jsou mnohem rychlejší než v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="c72a3-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-280">Nástroj příkazového řádku MSBuild lze použít také k vytváření aplikací ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="c72a3-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="c72a3-281">Tento nástroj se budeme v modulu 9.</span><span class="sxs-lookup"><span data-stu-id="c72a3-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="c72a3-282">Další vylepšení kompilace je nová možnost sestavit stránku v nabídce sestavení.</span><span class="sxs-lookup"><span data-stu-id="c72a3-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="c72a3-283">Tato funkce umožňuje vývojářům znovu sestavit pouze aktuální stránku (spolu s, kurzů a závislostí) tak, aby změny mohou být zkompilovány rychleji.</span><span class="sxs-lookup"><span data-stu-id="c72a3-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="c72a3-284">Protože C# nenabízí kompilace na pozadí pro účely aktualizace IntelliSense, atd., se budou využívat nesmírně tuto funkci vzhledem k tomu, že vám umožní pro technologii IntelliSense, abychom rychle aktualizovat pomocí jednoduše znovu sestavit jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="c72a3-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="c72a3-285">Vlastnosti sestavení pro projekt umožňují konfigurovat typ sestavení, který se nachází před provedením úvodní stránku.</span><span class="sxs-lookup"><span data-stu-id="c72a3-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="c72a3-286">Vývojáři se můžete rozhodnout vytvářet pouze aktuální stránku tak, aby Visual Studio můžete spustit, rychlejší ladění aplikací po změně kódu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![Úvodní stránka akce sestavení](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="c72a3-288">**Obrázek 9**: úvodní stránka akce sestavení</span><span class="sxs-lookup"><span data-stu-id="c72a3-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="c72a3-289">Další skvělou vylepšení sady Visual Studio a architektura ASP.NET je v oblasti upravit a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c72a3-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="c72a3-290">V sadě Visual Studio 2005 můžou vývojáři spuštění ladění projektu a provádět změny kódu v projektu bez odpojuje ladicí program.</span><span class="sxs-lookup"><span data-stu-id="c72a3-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="c72a3-291">Ve skutečnosti můžete doslova spuštění ladění projektu, přidejte novou třídu, přidejte kód do třídy, přidejte kód na stránku, která vytvoří novou instanci této třídy a provedení metody třídy, aniž by odpojuje ladicí program.</span><span class="sxs-lookup"><span data-stu-id="c72a3-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="c72a3-292">Spouštění nový kód je doslova stejně jednoduše jako si aktualizaci prohlížeče!</span><span class="sxs-lookup"><span data-stu-id="c72a3-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="c72a3-293">Kliknutím zde můžete zobrazit na video s návodem úpravy a pokračovat funkce v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="c72a3-294">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="c72a3-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="c72a3-295">Robustní upravit a pokračovat funkce v technologii ASP.NET 2.0 a Visual Studio 2005 je z důvodu architektury změny pro aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c72a3-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="c72a3-296">V technologii ASP.NET 1.x, aplikace vytvořené v aplikaci Visual Studio 2002/2003 byly zkompilovány do primární sestavení, která byla uložená ve složce/Bin.</span><span class="sxs-lookup"><span data-stu-id="c72a3-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="c72a3-297">Všechny třídy, stránkách atd pro aplikace, které byly zkompilovány do tohoto jednu knihovnu DLL.</span><span class="sxs-lookup"><span data-stu-id="c72a3-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="c72a3-298">Pak za běhu, technologie ASP.NET by všechny ovládací prvky, značek a kódu ASP.NET v rámci stránky zkompilovat a zkopírování těchto knihoven DLL do dočasné složky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c72a3-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="c72a3-299">V sadě Visual Studio 2005 pomocí technologie ASP.NET 2.0, modely dvě kompilace popisují výše (jeden pro Visual Studio) a jeden pro technologii ASP.NET v době běhu byly sloučeny do jednoho modelu common kompilace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="c72a3-300">To znamená, že všechny problémy při kompilaci jsou nyní zachycena ve fázi vývoje místo za běhu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="c72a3-301">Umožňuje také pro návrháře a podporu technologie IntelliSense pro funkce, jako je uživatelské ovládací prvky a hlavní stránky.</span><span class="sxs-lookup"><span data-stu-id="c72a3-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="c72a3-302">Kliknutím sem zobrazíte na video s návodem návrhářské podpory pro uživatelské ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="c72a3-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="c72a3-303">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="c72a3-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="c72a3-304">Pokud uživatelský ovládací prvek se odebere ze stránky, @Register – direktiva zůstává v kódu a měly by se odebrat ručně vyhnout chyby analyzátoru, pokud uživatelský ovládací prvek je odstraněn z webu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="c72a3-305">Další vylepšení v modelu kompilace Visual Studio je funkce Publikovat Web.</span><span class="sxs-lookup"><span data-stu-id="c72a3-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="c72a3-306">Protože předkompiluje funkci Publikovat na webu, vývojáři mohli využít přidání výkonu bez nutnosti kompilace nic na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="c72a3-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="c72a3-307">Také předkompiluje s veškerým zdrojovým kódem ve složce aplikace/_fragmenty do knihovny DLL tak, aby žádný zdrojový kód je nutné nasadit.</span><span class="sxs-lookup"><span data-stu-id="c72a3-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![Dialogové okno publikování webu](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="c72a3-309">**Obrázek 10**: dialogové okno publikování webu</span><span class="sxs-lookup"><span data-stu-id="c72a3-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="c72a3-310">Nástroj aspnet/_compile.exe lze také provést předkompilaci webovou aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c72a3-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="c72a3-311">Tento nástroj se budeme v modulu 9.</span><span class="sxs-lookup"><span data-stu-id="c72a3-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="c72a3-312">Když publikujete webovou stránku, Předkompilované soubory jsou uloženy ve složce dočasných souborů ASP.NET jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="c72a3-313">Soubory s *Compiled* přípona souboru jsou soubory formátu XML, které definují závislosti pro konkrétní knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="c72a3-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="c72a3-314">Všechny ovládací prvky webového formuláře nebo uživatele jsou kompilovány do náhodných knihovny DLL, které začínají *aplikace /_Web /_*.</span><span class="sxs-lookup"><span data-stu-id="c72a3-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="c72a3-315">Necháte-li *povolit tomuto předkompilovanému webu umožnit aktualizaci modelové* zaškrtávací políčko zaškrtnuto, kód uvnitř své ovládací prvky webových formulářů a uživatel nebude předem kompilovaných do knihovny DLL, umožní vám provádět změny po nasazení.</span><span class="sxs-lookup"><span data-stu-id="c72a3-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="c72a3-316">Pokud chcete zamezit kód tak, aby nejsou povoleny změny nasazeným obsahem, zrušte zaškrtnutí tohoto políčka.</span><span class="sxs-lookup"><span data-stu-id="c72a3-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="c72a3-317">*Použít pevné nování a jednostránkové sestavení* checkbox umožňuje, aby každá stránka je zkompilovat do sestavení s názvem oprava vypnutí dávkové kompilace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="c72a3-318">Opuštění toto políčko není zaškrtnuto, můžete využít výhod dávkové kompilace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="c72a3-319">*Povolení silné názvy předkompilovaných sestavení* zaškrtávací políčko umožní se silným názvem předkompilovaných sestavení.</span><span class="sxs-lookup"><span data-stu-id="c72a3-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-320">V technologii ASP.NET 1.x, sestavení se silným názvem museli nainstalovat do globální mezipaměti sestavení (GAC).</span><span class="sxs-lookup"><span data-stu-id="c72a3-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="c72a3-321">V technologii ASP.NET 2.0 můžete nejsou nutné pro instalaci sestavení se silným názvem do mezipaměti GAC.</span><span class="sxs-lookup"><span data-stu-id="c72a3-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![Předem kompilovaných souborů aplikace ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="c72a3-323">**Obrázek 11**: předem kompilovaných souborů aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c72a3-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="c72a3-324">Ve výše uvedené aplikace byla žádný soubor web.config.</span><span class="sxs-lookup"><span data-stu-id="c72a3-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="c72a3-325">Pokud by došlo, je by byly volány *PrecompiledApp.config* po publikování webu serveru procesu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="c72a3-326">Vylepšení v nasazení</span><span class="sxs-lookup"><span data-stu-id="c72a3-326">Improvements in Deployment</span></span>

<span data-ttu-id="c72a3-327">Jako s Visual Studio 2002 a 2003, nabízí Visual Studio 2005 funkce Kopírovat projekt.</span><span class="sxs-lookup"><span data-stu-id="c72a3-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="c72a3-328">Ale tato funkce se se zvýšenou úložnou v sadě Visual Studio 2005 a se teď nazývá Kopírovat web.</span><span class="sxs-lookup"><span data-stu-id="c72a3-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="c72a3-329">Dialogové okno Kopírovat web je rozdělený do blok levé a pravé rámce.</span><span class="sxs-lookup"><span data-stu-id="c72a3-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="c72a3-330">Levý rámec se nazývá zdrojového webu a správné rámce je na vzdáleném webu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="c72a3-331">Jednou z věcí, které by mohly uvádět někteří vývojáři je, že lokality zobrazí v pravém rámečku není nutně vzdálené lokality.</span><span class="sxs-lookup"><span data-stu-id="c72a3-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="c72a3-332">Může to být web v místním systému souborů nebo v místní instanci služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c72a3-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="c72a3-333">Kromě toho web zobrazí v levém podokně není nutně zdrojového webu protože dialogového okna můžete publikovat ze vzdáleného webu *k* zdrojového webu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="c72a3-334">Pokud kopírujete projekt na vzdálený web, této lokality musí mít na něj nainstalovat rozšíření serveru FrontPage.</span><span class="sxs-lookup"><span data-stu-id="c72a3-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="c72a3-335">Pokud tomu tak není, musíte připojit pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="c72a3-336">Na druhé straně Pokud kopírujete projekt k místní instanci služby IIS, rozšíření serveru FrontPage nejsou nutné.</span><span class="sxs-lookup"><span data-stu-id="c72a3-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-337">Pokud se pokusíte vytvořit novou webovou stránku v místní instanci služby IIS a jsou nainstalovaná rozšíření FrontPage 2002 Server Extensions, zobrazí se chybová zpráva s oznámením, že se na Sharepointovém serveru nepodporuje vytváření webů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="c72a3-338">V takovém případě máte možnost instalace rozšíření serveru FrontPage 2000 nebo odebírání rozšíření serveru FrontPage.</span><span class="sxs-lookup"><span data-stu-id="c72a3-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="c72a3-339">Video návod k funkci kopírování webu, klikněte sem.</span><span class="sxs-lookup"><span data-stu-id="c72a3-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="c72a3-340">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="c72a3-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="c72a3-341">Vylepšení ladění</span><span class="sxs-lookup"><span data-stu-id="c72a3-341">Improvements in Debugging</span></span>

<span data-ttu-id="c72a3-342">Existují čtyři klíčových vylepšení při ladění v sadě Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="c72a3-343">Ladění místně jako bez oprávnění správce je možné úprav.</span><span class="sxs-lookup"><span data-stu-id="c72a3-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="c72a3-344">Atribut ladění pro Compilation element je teď ve výchozím nastavení hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="c72a3-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="c72a3-345">Vzdálené ladění, nastavení a konfigurace je jednodušší než dřív.</span><span class="sxs-lookup"><span data-stu-id="c72a3-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="c72a3-346">Teď můžete ladit webovou stránku otevřít pomocí umístění služby FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="c72a3-347">Ladění jako bez oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="c72a3-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="c72a3-348">Přidání serveru ASP.NET Development Server umožňuje nejsou správci snadno ladit aplikace ASP.NET předem připravené.</span><span class="sxs-lookup"><span data-stu-id="c72a3-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="c72a3-349">Při ladění aplikace ASP.NET běžící v místním systému souborů sady Visual Studio spustí vývojový Server ASP.NET v kontextu přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c72a3-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="c72a3-350">Tento uživatel potom můžete ladit tuto aplikaci bez další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="c72a3-351">Ladění je ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="c72a3-351">Debug is False by Default</span></span>

<span data-ttu-id="c72a3-352">V technologii ASP.NET 1.x, *ladění* atribut *kompilace* element v souboru web.config byl nastaven na hodnotu *true* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c72a3-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="c72a3-353">Se vždy doporučuje, aby vývojáři tento atribut nastavte na *false* před nasazením aplikace do produkčního prostředí, ale protože většina vývojáři plně nerozumí důsledků byste museli opustit ladicí atribut nastavený na Hodnota TRUE, se jednoduše nacházela jako-je.</span><span class="sxs-lookup"><span data-stu-id="c72a3-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="c72a3-354">Nejzávažnější problém mít atribut ladění je nastavenou na hodnotu true, zakáže modelu ASP.NETs dávkové kompilace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="c72a3-355">Proto je každé stránky kompilovány do samostatné knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="c72a3-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="c72a3-356">Pokud se k webu, které aplikace se skládá z tisíců stránky (ne mám jsou jakýmikoli prostředky), který znamená, že několik tisíc malé knihoven DLL, vytvoří se v příslušné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c72a3-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="c72a3-357">I když tyto knihovny jsou malé velikosti, nejsou načtena do libovolné konkrétní místo v paměti.</span><span class="sxs-lookup"><span data-stu-id="c72a3-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="c72a3-358">Proto způsobit fragmentaci v systémové paměti a může přispět k OutOfMemoryException výskytů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="c72a3-359">V technologii ASP.NET 2.0 atribut ladění ve výchozím nastavení na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="c72a3-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="c72a3-360">Jak jste už viděli, kdy vývojář ladí aplikaci technologie ASP.NET v sadě Visual Studio 2005, zobrazí se výzva k přidání souboru web.config s povoleným laděním.</span><span class="sxs-lookup"><span data-stu-id="c72a3-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="c72a3-361">To s sebou nese náklady stejné nevýhody, které byly k dispozici v technologii ASP.NET 1.x, ale teď vývojář je zřetelně zobrazí upozornění, že atribut by se měla obnovit na hodnotu false před přesunutím aplikaci do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="c72a3-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="c72a3-362">Nastavení vzdáleného ladění a konfigurace</span><span class="sxs-lookup"><span data-stu-id="c72a3-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="c72a3-363">Vzdálené ladění ve Visual Studio 2002/2003, spoléhal na počítač ladění správce (mdm.exe) a proces vs7jit.exe.</span><span class="sxs-lookup"><span data-stu-id="c72a3-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="c72a3-364">Proto vzdáleného ladění potíží často byl černé políčko pro zákazníky a nebylo často mnohem vyšší u služby PSS.</span><span class="sxs-lookup"><span data-stu-id="c72a3-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="c72a3-365">Visual Studio 2005 odebere závislost na mdm.exe a vs7jit.exe procesy.</span><span class="sxs-lookup"><span data-stu-id="c72a3-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="c72a3-366">Místo toho teď používá službu sledování vzdáleného ladění (msvsmon.exe).</span><span class="sxs-lookup"><span data-stu-id="c72a3-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="c72a3-367">Požadavek na vzdáleného ladění sady Visual Studio 2005 je poměrně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="c72a3-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="c72a3-368">Potřebujete ke spuštění msvsmon.exe na vzdáleném serveru před ladění.</span><span class="sxs-lookup"><span data-stu-id="c72a3-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="c72a3-369">Sledování vzdáleného ladění můžete nainstalovat z disku CD Visual Studio nebo můžete jednoduše spustit msvsmon.exe ze sdílené složky bez nutnosti instalace nic vůbec na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="c72a3-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="c72a3-370">Při spuštění msvsmon.exe, je pravděpodobné, že bude stěžovat porty blokuje vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="c72a3-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="c72a3-371">Naštěstí můžete snadno odblokovat porty přímo v dialogovém okně upozornění jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Oznámení, že je brána Windows Firewall blokuje vzdálené ladění](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="c72a3-373">**Obrázek 12**: je oznámení, aby brána Windows Firewall blokuje vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="c72a3-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="c72a3-374">Jakmile máte odblokováno porty potřebné pro ladění, zobrazí se sledování vzdáleného ladění, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c72a3-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="c72a3-375">Z tohoto rozhraní můžete monitorovat připojení a ladění oprávnění snadno změnit.</span><span class="sxs-lookup"><span data-stu-id="c72a3-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![Sledování vzdáleného ladění](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="c72a3-377">**Obrázek 13**: sledování vzdáleného ladění</span><span class="sxs-lookup"><span data-stu-id="c72a3-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="c72a3-378">Je také možné vzdáleně ladit webové aplikace otevřít přes protokol FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="c72a3-379">Kroky jsou stejné jako ty, které dříve uvedené.</span><span class="sxs-lookup"><span data-stu-id="c72a3-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="c72a3-380">Musíte ale zadat základní adresu URL pro procházení FTP projektu, jak je uvedeno výše v tomto modulu.</span><span class="sxs-lookup"><span data-stu-id="c72a3-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="c72a3-381">Testovací prostředí 2</span><span class="sxs-lookup"><span data-stu-id="c72a3-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="c72a3-382">Vzdálené ladění pomocí Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="c72a3-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="c72a3-383">Toto testovací prostředí vás provede procesem vzdálené ladění pomocí Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="c72a3-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="c72a3-384">Video návod tohoto testovacího prostředí, klikněte sem.</span><span class="sxs-lookup"><span data-stu-id="c72a3-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="c72a3-385">Otevřít Video na celou obrazovku</span><span class="sxs-lookup"><span data-stu-id="c72a3-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="c72a3-386">Toto testovací prostředí je potřeba mít dva počítače, jeden spuštěné sadě Visual Studio 2005 a ostatní spuštěné služby IIS 5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c72a3-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="c72a3-387">Otevřít Visual Studio 2005 a vytvořit nový web na vzdáleném serveru.</span><span class="sxs-lookup"><span data-stu-id="c72a3-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="c72a3-388">Na webu můžete vytvořit ve vzdálené instanci služby IIS nebo přes protokol FTP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="c72a3-389">Ze vzdáleného webového serveru vyhledejte msvsmon.exe na vývojovém počítači pomocí cesty UNC a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="c72a3-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="c72a3-390">Výchozí umístění pro msvsmon.exe je //server/c$/Program soubory nebo Microsoft Visual Studio 8/Common7/IDE/vzdáleného ladicího programu/x86.</span><span class="sxs-lookup"><span data-stu-id="c72a3-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="c72a3-391">Pokud se zobrazí výzva k odblokování porty pro vzdálené ladění, udělejte to.</span><span class="sxs-lookup"><span data-stu-id="c72a3-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="c72a3-392">Z vývojového počítače otevřete kódu pro Default.aspx a nastavte zarážku v metodě stránky/_služba.</span><span class="sxs-lookup"><span data-stu-id="c72a3-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="c72a3-393">Spusťte ladění z vývojového počítače.</span><span class="sxs-lookup"><span data-stu-id="c72a3-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="c72a3-394">By měl dostanete k zarážce podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c72a3-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="c72a3-395">Vývojový server ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c72a3-395">ASP.NET Development Server</span></span>

<span data-ttu-id="c72a3-396">Jako weve již probírali Visual Studio 2005 se dodává s názvem serveru ASP.NET Development Server webového serveru.</span><span class="sxs-lookup"><span data-stu-id="c72a3-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="c72a3-397">(Vývojový Server ASP.NET se někdy označuje jako Cassini.) Tento webový server je pohodlný způsob procházení a ladění webových aplikací spuštěných v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="c72a3-398">Serveru ASP.NET Development Server je webový server s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="c72a3-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="c72a3-399">Nepovoluje vzdálená připojení, neumožňuje všechny žádosti od libovolného uživatele, než je uživatel, který spustil webový server.</span><span class="sxs-lookup"><span data-stu-id="c72a3-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="c72a3-400">Také nemá schopnost obsluhovat stránky ASP.</span><span class="sxs-lookup"><span data-stu-id="c72a3-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="c72a3-401">Pouze technologie ASP.NET a prostředků ve formátu HTML (včetně obrázků, souborů CSS, atd.) se obsluhují.</span><span class="sxs-lookup"><span data-stu-id="c72a3-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="c72a3-402">Serveru ASP.NET Development Server můžete spustit pomocí příkazového řádku spuštěním WebDev.WebServer.exe souboru v umístění c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="c72a3-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="c72a3-403">Zobrazí se následující dialogové okno parametry, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c72a3-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="c72a3-404">**Obrázek 14**</span><span class="sxs-lookup"><span data-stu-id="c72a3-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="c72a3-405">Vývojový Server ASP.NET se nepodporuje při spuštění explicitně pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c72a3-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
