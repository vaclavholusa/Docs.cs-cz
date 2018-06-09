---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Představení technologie ASP.NET Web Pages – publikování webu pomocí služby WebMatrix | Microsoft Docs
author: tfitzmac
description: V tomto kurzu je poslední část v sadě kurz, který představuje rozhraní ASP.NET Web Pages a Microsoft WebMatrix. Popisuje, jak publikování webu t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30899587"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="ab10f-104">Představení technologie ASP.NET Web Pages – publikování webu pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="ab10f-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="ab10f-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ab10f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ab10f-106">V tomto kurzu je poslední část v sadě kurz, který představuje rozhraní ASP.NET Web Pages a Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ab10f-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="ab10f-107">Popisuje, jak publikovat vaší lokality k Internetu, aby ostatní s ním pracovat.</span><span class="sxs-lookup"><span data-stu-id="ab10f-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="ab10f-108">Předpokládá, že jste dokončili řady prostřednictvím [vytváření konzistentní vypadat pro weby technologie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251585).</span><span class="sxs-lookup"><span data-stu-id="ab10f-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="ab10f-109">Naučíte, jak publikovat používá web:</span><span class="sxs-lookup"><span data-stu-id="ab10f-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="ab10f-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ab10f-110">Microsoft Azure</span></span>
> - <span data-ttu-id="ab10f-111">Web hostingové společnosti</span><span class="sxs-lookup"><span data-stu-id="ab10f-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="ab10f-112">O publikování webu</span><span class="sxs-lookup"><span data-stu-id="ab10f-112">About Publishing Your Site</span></span>

<span data-ttu-id="ab10f-113">Až nyní kroky dokončíte všechnu práci na místním počítači, včetně testování vaší stránky.</span><span class="sxs-lookup"><span data-stu-id="ab10f-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="ab10f-114">Ke spuštění vaší<em>.cshtml</em> stránky, jste použili webový server, který je integrovaný do služby WebMatrix, a to služba IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ab10f-114">To run your<em>.cshtml</em> pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="ab10f-115">Můžete ale samozřejmě nikdo můžete zobrazit na server, který jste vytvořili s výjimkou je.</span><span class="sxs-lookup"><span data-stu-id="ab10f-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="ab10f-116">Umožníte dalším uživatelům práci s webem musíte publikovat na Internetu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="ab10f-117">Pokud již máte přístup k veřejné webovému serveru, publikování znamená, že budete mít tak, aby měl účet s *Cloudová platforma* nebo *poskytovatele hostitelských služeb*.</span><span class="sxs-lookup"><span data-stu-id="ab10f-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="ab10f-118">Cloudové platformy, jako je Microsoft Azure poskytuje infrastrukturu na vyžádání pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab10f-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="ab10f-119">Poskytovatel hostingu je společnosti, který je vlastníkem veřejně přístupná webové servery a který bude pronajímat, můžete místo pro svůj web.</span><span class="sxs-lookup"><span data-stu-id="ab10f-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="ab10f-120">Hostování plány spustit z několika dolarů za měsíc (nebo i volné) pro malé weby na mnoho stovky dolarů za měsíc pro vysoký počet komerčních webů.</span><span class="sxs-lookup"><span data-stu-id="ab10f-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="ab10f-121">Máte přístup k veřejné webovému serveru prostřednictvím poskytovatele služeb Internetu (ISP), které umožňují získat doma internetové služby.</span><span class="sxs-lookup"><span data-stu-id="ab10f-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="ab10f-122">Poskytovatel hostitelských však musí podporovat rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="ab10f-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="ab10f-123">Nemáte mnoho poskytovatelů internetových služeb, ale je vždy vhodné kontrola.</span><span class="sxs-lookup"><span data-stu-id="ab10f-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="ab10f-124">V tomto kurzu jsme vám poskytují přehled o tom, jak publikovat.</span><span class="sxs-lookup"><span data-stu-id="ab10f-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="ab10f-125">Není praktické zajistit najdete přesné informace pro všechno, protože proces se trochu liší pro všechny poskytovatele hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="ab10f-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="ab10f-126">Ale získáte představu o tom, jak probíhá proces.</span><span class="sxs-lookup"><span data-stu-id="ab10f-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="ab10f-127">Tento kurz obsahuje čtyři části:</span><span class="sxs-lookup"><span data-stu-id="ab10f-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="ab10f-128">Nastavení výchozí stránka</span><span class="sxs-lookup"><span data-stu-id="ab10f-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="ab10f-129">Publikování (zvolte jednu z následujících)</span><span class="sxs-lookup"><span data-stu-id="ab10f-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="ab10f-130">a.</span><span class="sxs-lookup"><span data-stu-id="ab10f-130">a.</span></span> [<span data-ttu-id="ab10f-131">Publikování webu do služby Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ab10f-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="ab10f-132">b.</span><span class="sxs-lookup"><span data-stu-id="ab10f-132">b.</span></span> [<span data-ttu-id="ab10f-133">Publikování webu Webhosting společnosti</span><span class="sxs-lookup"><span data-stu-id="ab10f-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="ab10f-134">Aktualizace živý web: opětovné publikování</span><span class="sxs-lookup"><span data-stu-id="ab10f-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="ab10f-135">Nastavení výchozí stránka</span><span class="sxs-lookup"><span data-stu-id="ab10f-135">Setting up the default page</span></span>

<span data-ttu-id="ab10f-136">Když uživatel přejde na základní adresa pro svůj web, se uživateli zobrazí výchozí stránku pro váš web.</span><span class="sxs-lookup"><span data-stu-id="ab10f-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="ab10f-137">Například pokud Default.htm je nastaven jako výchozí stránky pro lokalitu na www.contoso.com, přejděte do <strong>www.contoso.com</strong> je stejný jako přejdete na <strong>www.contoso.com/Default.htm</strong>.</span><span class="sxs-lookup"><span data-stu-id="ab10f-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to <strong>www.contoso.com</strong> is the same as navigating to <strong>www.contoso.com/Default.htm</strong>.</span></span>

<span data-ttu-id="ab10f-138">V současné době váš web používá **Default.cshtml** jako výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="ab10f-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="ab10f-139">Tato stránka je vhodná pro výchozí stránku, ale v tomto kurzu jste nepřidali žádný obsah na této stránce, by se zobrazily prázdné stránky.</span><span class="sxs-lookup"><span data-stu-id="ab10f-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="ab10f-140">Otevřete stránku Default.cshtml a nahradí obsah následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="ab10f-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="ab10f-141">Nyní je připraven pro publikaci vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-141">Now your site is ready for publication.</span></span> <span data-ttu-id="ab10f-142">První zobrazí se postup nasazení webu na Azure a jak ji nasadit do webhosting společnosti.</span><span class="sxs-lookup"><span data-stu-id="ab10f-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="ab10f-143">Buď možnost funguje pro váš web z umístění a je potřeba jenom použijte jednu z možností nasazení.</span><span class="sxs-lookup"><span data-stu-id="ab10f-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="ab10f-144">Publikování webu do služby Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ab10f-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="ab10f-145">V tomto kurzu se nejprve zobrazí nasazení webu na Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ab10f-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="ab10f-146">Po přihlášení pomocí účtu Microsoft, můžete vytvořit až 10 bezplatných webů v Azure.</span><span class="sxs-lookup"><span data-stu-id="ab10f-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="ab10f-147">Tyto volné lokality poskytují pohodlný způsob pro testování vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="ab10f-148">Vždy můžete odstranit tento server příklad později a nepoužívejte všechny volné stránek.</span><span class="sxs-lookup"><span data-stu-id="ab10f-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="ab10f-149">Během několika minut můžete vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ab10f-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ab10f-150">Podrobnosti najdete v tématu [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="ab10f-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="ab10f-151">Na pásu karet WebMatrix, klikněte **publikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ab10f-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

!["Publikování" tlačítko WebMatrix pásu karet](publishing/_static/image1.png)

<span data-ttu-id="ab10f-153">**Publikovat vaše lokality** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab10f-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="ab10f-154">Pokud nejste přihlášeni ke svému účtu Microsoft, bude obsahovat dialogové okno **Začínáme s Azure** odkaz.</span><span class="sxs-lookup"><span data-stu-id="ab10f-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="ab10f-155">Kliknutím na tento odkaz.</span><span class="sxs-lookup"><span data-stu-id="ab10f-155">Click this link.</span></span>

![Publikování webu](publishing/_static/image2.png)

<span data-ttu-id="ab10f-157">Pokud nejste přihlášeni k účtu Microsoft, můžete je znovu příležitost k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ab10f-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="ab10f-158">Musíte se přihlásit k účtu Microsoft k publikování webu v Azure.</span><span class="sxs-lookup"><span data-stu-id="ab10f-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![Přihlásit se](publishing/_static/image3.png)

<span data-ttu-id="ab10f-160">Po přihlášení k účtu Microsoft, dialogové okno obsahuje odkazy na vytvoření nového webu v Azure nebo připojit k existující weby v Azure.</span><span class="sxs-lookup"><span data-stu-id="ab10f-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![Vytvoří nový web](publishing/_static/image4.png)

<span data-ttu-id="ab10f-162">Vyberte **vytvoří nový web**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-162">Select **Create a new site**.</span></span>

<span data-ttu-id="ab10f-163">Pokud název projektu **WebPagesMovies**, bude výchozí název pro svůj web **webpagesmovies.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="ab10f-164">Tento výchozí název je pravděpodobně není k dispozici, jak je indikován červený vykřičník.</span><span class="sxs-lookup"><span data-stu-id="ab10f-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![název výchozí webové stránky](publishing/_static/image5.png)

<span data-ttu-id="ab10f-166">Změňte název webového serveru na jinou hodnotu, která je k dispozici a vyberte umístění, které je blízko vaši polohu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![název změněné lokality](publishing/_static/image6.png)

<span data-ttu-id="ab10f-168">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-168">Click **OK**.</span></span>

<span data-ttu-id="ab10f-169">Služba WebMatrix performss test ke zjištění, zda je server kompatibilní s vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![testování kompatibility](publishing/_static/image7.png)

<span data-ttu-id="ab10f-171">Vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-171">Select **Continue**.</span></span>

<span data-ttu-id="ab10f-172">Výsledky testování kompatibility se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="ab10f-172">The results of the compatibility test are displayed.</span></span>

![výsledek kompatibility](publishing/_static/image8.png)

<span data-ttu-id="ab10f-174">Vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-174">Select **Continue**.</span></span>

<span data-ttu-id="ab10f-175">Služba WebMatrix zobrazí soubory a databáze, které bude publikována do lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="ab10f-176">Vzhledem k tomu, že je to první, kdy jsou publikování webu, jsou uvedeny všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="ab10f-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="ab10f-177">Zrušte zaškrtnutí políčka soubor, který není připravena k publikování.</span><span class="sxs-lookup"><span data-stu-id="ab10f-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="ab10f-178">V dalších publikacích se zobrazí pouze soubory, které se změnily.</span><span class="sxs-lookup"><span data-stu-id="ab10f-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="ab10f-179">V tématu [aktualizace živý web: opětovné publikování](#update).</span><span class="sxs-lookup"><span data-stu-id="ab10f-179">See [Updating the Live Site: Republishing](#update).</span></span>

![publikování náhledu](publishing/_static/image9.png)

<span data-ttu-id="ab10f-181">Vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-181">Select **Continue**.</span></span>

<span data-ttu-id="ab10f-182">Po nasazení webu do Azure, se zobrazí zpráva, která určuje, že je dokončená nasazení.</span><span class="sxs-lookup"><span data-stu-id="ab10f-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![publikování dokončení](publishing/_static/image10.png)

<span data-ttu-id="ab10f-184">Lokality a databáze byly publikovány do služby Azure a jsou nyní k dispozici na veřejnost.</span><span class="sxs-lookup"><span data-stu-id="ab10f-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="ab10f-185">Kliknutím na odkaz ve zprávě o dokončení publikování a zobrazí vaše nasazené lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="ab10f-186">Můžete nebo každý, kdo má přístup k Internetu, můžete přidat nebo upravit záznamy v databázi.</span><span class="sxs-lookup"><span data-stu-id="ab10f-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="ab10f-187">Publikování webu Webhosting společnosti</span><span class="sxs-lookup"><span data-stu-id="ab10f-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="ab10f-188">Pokud se rozhodnete nebude publikován do Azure, můžete místo toho publikování webu na web hostingové společnosti.</span><span class="sxs-lookup"><span data-stu-id="ab10f-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="ab10f-189">Klikněte **najít hostování webů** odkaz.</span><span class="sxs-lookup"><span data-stu-id="ab10f-189">Click the **Find web hosting** link.</span></span>

![Tlačítko "najít hostování webů, v dialogovém okně Nastavení publikování](publishing/_static/image12.png)

<span data-ttu-id="ab10f-191">Přejděte na stránku na webu Microsoft, který uvádí poskytovatelé hostitelských služeb, které podporují ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ab10f-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![Stránky na webu společnosti Microsoft, které uvádí poskytovatelé hostitelských služeb](publishing/_static/image13.png)

<span data-ttu-id="ab10f-193">Samozřejmě může být obtížné zjistit, teď právě jaký hostování funkcí může vyžadovat na dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="ab10f-194">Tady je několik věcí, které je třeba zvážit:</span><span class="sxs-lookup"><span data-stu-id="ab10f-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="ab10f-195">Pro účely WebPagesMovies lokality nemusíte mít samostatné rozšíření pro SQL Server, který se velmi často stojí.</span><span class="sxs-lookup"><span data-stu-id="ab10f-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="ab10f-196">Ve vaší lokalitě používáte SQL Server Compact Edition, což je úplný a samostatný.</span><span class="sxs-lookup"><span data-stu-id="ab10f-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="ab10f-197">Přístup k serveru SQL je však může být zapotřebí pro budoucí webu práce, kterou, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="ab10f-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="ab10f-198">Pokud se domníváte, že pravděpodobně, zajistěte, aby funkce SQL serveru můžete později přidat.</span><span class="sxs-lookup"><span data-stu-id="ab10f-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="ab10f-199">Zkontrolujte, zda poskytovatel hostingu podporuje protokol publikování nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="ab10f-200">Můžete publikovat pomocí protokolu FTP, ale je pohodlnější službu Web Deploy používat.</span><span class="sxs-lookup"><span data-stu-id="ab10f-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="ab10f-201">Některé servery nabízejí bezplatné zkušební období.</span><span class="sxs-lookup"><span data-stu-id="ab10f-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="ab10f-202">Bezplatná zkušební verze je dobrý způsob, jak zkuste publikování a hostování při jste stále experimentování se službou WebMatrix a webové stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ab10f-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="ab10f-203">Vyberte ten, který chcete.</span><span class="sxs-lookup"><span data-stu-id="ab10f-203">Pick one that you like.</span></span> <span data-ttu-id="ab10f-204">V tomto kurzu jsme vybrat DiscountASP.NET, protože při jsme vytvořili kurz, že společnosti měli povýšení, které uživatelé mohou hostovat lokalitu zdarma pro několik měsíců.</span><span class="sxs-lookup"><span data-stu-id="ab10f-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="ab10f-205">Námi zvolený poskytovatele hostitelských služeb v tomto kurzu se nemohou interpretovat jako souhlas této společnosti nad všemi ostatními.</span><span class="sxs-lookup"><span data-stu-id="ab10f-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="ab10f-206">Ale jsme měli vybrat jednu pro obrázek a DiscountASP.NET je jedním z řada společností, které podporuje technologie ASP.NET Web Pages a protokol nasazení webu pro publikování.</span><span class="sxs-lookup"><span data-stu-id="ab10f-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="ab10f-207">Obvykle se poté, co jste se přihlásili s poskytovateli hostitelských služeb, společnost vám pošle e-mailu, který obsahuje uživatelské jméno a heslo, adresu URL webového serveru a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ab10f-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="ab10f-208">Pokud hostingové společnosti podporuje protokol nasazení webu, se může odeslat můžete soubor, který obsahuje nastavení publikování, nebo můžete stáhnout z Internetu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="ab10f-209">Soubor nastavení publikování zjednodušuje proces pro vás.</span><span class="sxs-lookup"><span data-stu-id="ab10f-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="ab10f-210">Pokud jste se přihlásili a jste připraveni publikovat, klikněte na tlačítko **publikovat** tlačítko pásu karet služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ab10f-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="ab10f-211">**Nastavení publikování** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ab10f-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="ab10f-212">Pokud poskytovatel hostingu odeslat soubor nastavení publikování, klikněte na tlačítko **importu nastavení publikování** propojení a import souboru.</span><span class="sxs-lookup"><span data-stu-id="ab10f-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="ab10f-213">Pokud nemáte soubor nastavení publikování, vyplňte pole pomocí hodnoty, které jste hostingové společnosti odeslat v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="ab10f-214">Tady je co **nastavení publikování** dialogové okno může vypadat například po dokončení:</span><span class="sxs-lookup"><span data-stu-id="ab10f-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![Nastavení publikování vyplněno v dialogovém okně, nastavení publikování.](publishing/_static/image14.png)

<span data-ttu-id="ab10f-216">Klikněte na tlačítko **ověření připojení**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-216">Click **Validate Connection**.</span></span> <span data-ttu-id="ab10f-217">Pokud se vše, co je to v pořádku, dialogové okno sestavy **připojení úspěšně**, což znamená, že může komunikovat s server poskytovatele hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="ab10f-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![Úspěch zprávy Pokud publikování jsou nastavení správná](publishing/_static/image15.png)

<span data-ttu-id="ab10f-219">Pokud dojde k potížím, služba WebMatrix nemá je nejlepší říct, co problém je:</span><span class="sxs-lookup"><span data-stu-id="ab10f-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![Chybová zpráva, pokud došlo k potížím s nastavením publikování](publishing/_static/image16.png)

<span data-ttu-id="ab10f-221">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="ab10f-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="ab10f-222">Služba WebMatrix nabízí chcete provést test a ujistěte se, že správně může komunikovat s hostování lokality:</span><span class="sxs-lookup"><span data-stu-id="ab10f-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![Zprávy, které chcete provést test procesu publikování nabídky](publishing/_static/image17.png)

<span data-ttu-id="ab10f-224">Klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-224">Click **Yes**.</span></span> <span data-ttu-id="ab10f-225">Služba WebMatrix ukládání některých ukázkových souborů do poskytovatele hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="ab10f-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="ab10f-226">Pokud se provádí testování kompatibility, služba WebMatrix hlásí výsledky:</span><span class="sxs-lookup"><span data-stu-id="ab10f-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![Výsledky testu publikování](publishing/_static/image18.png)

<span data-ttu-id="ab10f-228">Pokud jste připravení přejít, pokračujte a klikněte na tlačítko **pokračovat** pro skutečných zahájíte proces publikování.</span><span class="sxs-lookup"><span data-stu-id="ab10f-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="ab10f-229">Služba WebMatrix obrázky co soubory jsou ve vaší lokalitě a jsou již na hostitelském serveru (ihned, none) a poskytuje náhled proces publikování:</span><span class="sxs-lookup"><span data-stu-id="ab10f-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![Verze Preview jaké soubory, které odešlete proces publikování](publishing/_static/image19.png)

<span data-ttu-id="ab10f-231">Seznam souborů k publikování, které zahrnuje webové stránky, které jste vytvořili jako *Movies.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ab10f-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="ab10f-232">Seznam také obsahuje soubory pro pomocné rutiny, které jste nainstalovali, soubory a spustit SQL Server Compact Edition pro vaši databázi a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ab10f-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="ab10f-233">V důsledku toho počáteční publikování proces může být výrazně.</span><span class="sxs-lookup"><span data-stu-id="ab10f-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="ab10f-234">Klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-234">Click **Continue**.</span></span> <span data-ttu-id="ab10f-235">Služba WebMatrix zkopíruje soubory na server poskytovatele hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="ab10f-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="ab10f-236">Až skončíte, výsledky jsou uvedeny ve stavovém řádku:</span><span class="sxs-lookup"><span data-stu-id="ab10f-236">When it's done, the results are reported in the status bar:</span></span>

![Stavová zpráva panelu při procesu publikování byl úspěšně dokončen.](publishing/_static/image20.png)

<span data-ttu-id="ab10f-238">Pokud chcete zobrazit živý web, klikněte na odkaz ve stavovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ab10f-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="ab10f-239">Přidat *filmy* na adresu URL, a zobrazí se *Movies.cshtml* soubor, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="ab10f-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![Živý web stránkou filmy](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="ab10f-241">Aktualizace živý web: opětovné publikování</span><span class="sxs-lookup"><span data-stu-id="ab10f-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="ab10f-242">Po publikování webu (do Azure nebo webhosting společnosti) jsou dvě kopie &mdash; verze na váš počítač a verze na poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="ab10f-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="ab10f-243">Pravděpodobně budete chtít pokračovat ve vývoji lokality (Pokud nic jiného, jako součást sady další kurz).</span><span class="sxs-lookup"><span data-stu-id="ab10f-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="ab10f-244">Když to uděláte, budete muset web znovu publikovat, aby bylo možné kopírovat změny z vašeho počítače a poskytovatelem služeb.</span><span class="sxs-lookup"><span data-stu-id="ab10f-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="ab10f-245">Proces publikování ve službě WebMatrix můžete určit, jaké soubory se změnili na svém webu a publikovat pouze tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="ab10f-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="ab10f-246">Chcete-li zjistit, jak funguje opětovné publikování, otevřete *Movies.cshtml* lokality, provedení některých malých změn a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="ab10f-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="ab10f-247">Můžete například změnit název na `Movies - Updated`.</span><span class="sxs-lookup"><span data-stu-id="ab10f-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="ab10f-248">Klikněte **publikovat** tlačítka na pásu karet.</span><span class="sxs-lookup"><span data-stu-id="ab10f-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="ab10f-249">Služba WebMatrix Určuje, co se změnilo a zobrazí náhled souborů, které budete publikovat.</span><span class="sxs-lookup"><span data-stu-id="ab10f-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![Dialogové okno "Publikovat" zobrazující změněné soubory připravené pro opětovné publikování](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="ab10f-251">Ve výchozím nastavení, služba WebMatrix publikuje vaše databáze (*SDF* souboru) pouze při prvním publikování webu.</span><span class="sxs-lookup"><span data-stu-id="ab10f-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="ab10f-252">Jakmile vaše lokalita je publikována a osoby jsou interakci s webem, databázi na živý web má obvykle reálná data lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="ab10f-253">Budete muset buďte velmi opatrní nechcete přepsat databázi za provozu s *SDF* soubor, který je v počítači, který obvykle obsahuje pouze testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ab10f-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="ab10f-254">Proto se zobrazí upozornění **publikování přepíše všechny vzdálené databáze**, a proč je zaškrtnutí políčka pro *WebPagesMovies.sdf* ve výchozím nastavení zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="ab10f-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="ab10f-255">Klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="ab10f-255">Click **Continue**.</span></span> <span data-ttu-id="ab10f-256">Služba WebMatrix zobrazuje zpráva o úspěšném provedení jakou měla poprvé, kterou jste publikovali a publikuje změněné soubory.</span><span class="sxs-lookup"><span data-stu-id="ab10f-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="ab10f-257">Přejděte na web za provozu (můžete kliknout na odkaz ve zprávě úspěch Pokud se stále zobrazuje) a ověřit, že byl publikovaný změny.</span><span class="sxs-lookup"><span data-stu-id="ab10f-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="ab10f-258">**Vzdálené úpravy souborů**</span><span class="sxs-lookup"><span data-stu-id="ab10f-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="ab10f-259">Jako alternativu k změna vaší lokality a poté opětovné publikování můžete upravit vzdálené soubory přímo ve službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ab10f-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="ab10f-260">V tomto scénáři otevřete soubor, který je na poskytovatele služeb a služba WebMatrix stáhne kopii ho můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="ab10f-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="ab10f-261">Pokaždé, když jste uložili soubor, služba WebMatrix odešle změny do lokality.</span><span class="sxs-lookup"><span data-stu-id="ab10f-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="ab10f-262">Vzdálené úpravy je snadný způsob, jak změnit živý web.</span><span class="sxs-lookup"><span data-stu-id="ab10f-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="ab10f-263">Změny, které uděláte takto však nejsou synchronizovány se soubory ve vaší místní lokalitě.</span><span class="sxs-lookup"><span data-stu-id="ab10f-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="ab10f-264">Chcete-li synchronizovat místní soubory s vzdálené lokality, můžete stáhnout vzdálených souborů.</span><span class="sxs-lookup"><span data-stu-id="ab10f-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="ab10f-265">Tento proces funguje podobně jako publikování, s výjimkou pozpátku.</span><span class="sxs-lookup"><span data-stu-id="ab10f-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="ab10f-266">Jsme nebude popisují více o vzdálené úpravy a stahování vzdálené zařízení WebMatrix sem.</span><span class="sxs-lookup"><span data-stu-id="ab10f-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="ab10f-267">Jsou velmi užitečné, pokud máte více uživatelů pro práci na stejném webu na různých počítačích.</span><span class="sxs-lookup"><span data-stu-id="ab10f-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="ab10f-268">Další informace najdete v tématu [publikování a upravit vzdálené lokality pomocí služby WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span><span class="sxs-lookup"><span data-stu-id="ab10f-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="ab10f-269">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="ab10f-269">Additional Resources</span></span>

- <span data-ttu-id="ab10f-270">[Fórum služby WebMatrix rozhraní ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), skvělým místem pro zveřejňování otázky a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ab10f-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab10f-271">Předchozí</span><span class="sxs-lookup"><span data-stu-id="ab10f-271">Previous</span></span>](layouts.md)
