---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "Vytvoření stránky nápovědy pro rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="8d055-102">Vytvoření stránky nápovědy pro rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="8d055-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="8d055-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8d055-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8d055-104">Při vytváření webového rozhraní API je často užitečné vytvořit stránku nápovědy, aby ostatní vývojáři věděli, jak volat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="8d055-105">Veškerá dokumentace může vytvořit ručně, ale je lepší automaticky vytvořit co největší míře.</span><span class="sxs-lookup"><span data-stu-id="8d055-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="8d055-106">Chcete-li tato úloha jednodušší, rozhraní ASP.NET Web API poskytuje knihovnu pro automatické generování stránky nápovědy v době běhu.</span><span class="sxs-lookup"><span data-stu-id="8d055-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="8d055-107">Vytvoření stránky nápovědy rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8d055-107">Creating API Help Pages</span></span>

<span data-ttu-id="8d055-108">Nainstalujte [ASP.NET a webové nástroje 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="8d055-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="8d055-109">Tato aktualizace se integruje stránky nápovědy do šablony projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="8d055-110">Dále vytvořte nový projekt ASP.NET MVC 4 a výběr šablony projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="8d055-111">Šablona projektu vytvoří řadič příklad rozhraní API s názvem `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="8d055-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="8d055-112">Šablona vytvoří také stránky nápovědy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-112">The template also creates the API help pages.</span></span> <span data-ttu-id="8d055-113">Všechny soubory kódu pro stránku nápovědy jsou umístěny ve složce oblastí projektu.</span><span class="sxs-lookup"><span data-stu-id="8d055-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="8d055-114">Při spuštění aplikace Domovská stránka obsahuje odkaz na tuto stránku nápovědy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="8d055-115">Na domovské stránce je relativní cesta/help.</span><span class="sxs-lookup"><span data-stu-id="8d055-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="8d055-116">Tento odkaz umožňuje souhrnnou stránku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="8d055-117">Tato stránka zobrazení MVC je definována v Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8d055-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="8d055-118">Můžete upravit této stránce můžete upravit rozložení, úvod, název, styly a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8d055-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="8d055-119">Hlavní část stránky je tabulka rozhraní API, seskupené podle řadiče.</span><span class="sxs-lookup"><span data-stu-id="8d055-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="8d055-120">Položky tabulky jsou generována dynamicky, pomocí **IApiExplorer** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8d055-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="8d055-121">(I mluvit o další informace o tomto rozhraní později.) Pokud přidáte nový řadič rozhraní API, v tabulce se automaticky aktualizuje v době běhu.</span><span class="sxs-lookup"><span data-stu-id="8d055-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="8d055-122">Sloupec "API" seznam Metoda HTTP a relativní identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="8d055-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="8d055-123">Sloupec "Popis" obsahuje dokumentace pro každé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="8d055-124">Na začátku dokumentace je právě zástupný text.</span><span class="sxs-lookup"><span data-stu-id="8d055-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="8d055-125">V další části I budete ukazují, jak přidat dokumentace z komentáře XML.</span><span class="sxs-lookup"><span data-stu-id="8d055-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="8d055-126">Každé rozhraní API obsahuje odkaz na stránku s podrobnější informace, včetně příklad těla požadavku a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8d055-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="8d055-127">Přidání stránky nápovědy do existujícího projektu</span><span class="sxs-lookup"><span data-stu-id="8d055-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="8d055-128">Stránky nápovědy můžete přidat do existujícího projektu webového rozhraní API pomocí Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="8d055-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="8d055-129">Tato možnost je užitečná, že spustíte z šablonu jiný projekt, než má šablona "Webového rozhraní API".</span><span class="sxs-lookup"><span data-stu-id="8d055-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="8d055-130">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a potom vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="8d055-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="8d055-131">V [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okno, zadejte jeden z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="8d055-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="8d055-132">Pro **C#** aplikace:`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="8d055-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="8d055-133">Pro **jazyka Visual Basic** aplikace:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="8d055-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="8d055-134">Existují dva balíčky, jeden pro jazyk C# a jeden v jazyce Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="8d055-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="8d055-135">Nezapomeňte použít ten, který odpovídá projektu.</span><span class="sxs-lookup"><span data-stu-id="8d055-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="8d055-136">Tento příkaz nainstaluje potřebné sestavení a přidá zobrazení MVC pro stránky nápovědy (umístěný ve složce oblasti nebo HelpPage).</span><span class="sxs-lookup"><span data-stu-id="8d055-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="8d055-137">Budete muset ručně přidejte odkaz na stránku nápovědy.</span><span class="sxs-lookup"><span data-stu-id="8d055-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="8d055-138">Identifikátor URI je/help.</span><span class="sxs-lookup"><span data-stu-id="8d055-138">The URI is /Help.</span></span> <span data-ttu-id="8d055-139">Chcete-li vytvořit odkaz v zobrazení syntaxe razor, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="8d055-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="8d055-140">Také zajistěte, aby k registraci oblasti.</span><span class="sxs-lookup"><span data-stu-id="8d055-140">Also, make sure to register areas.</span></span> <span data-ttu-id="8d055-141">V souboru Global.asax, přidejte následující kód, který **aplikace\_spustit** metodu, pokud tam není již:</span><span class="sxs-lookup"><span data-stu-id="8d055-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="8d055-142">Přidání dokumentaci k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8d055-142">Adding API Documentation</span></span>

<span data-ttu-id="8d055-143">Ve výchozím nastavení v nápovědě stránky máte řetězce zástupný symbol pro dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8d055-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="8d055-144">Můžete použít [dokumentační komentáře XML](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) vytvořit v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8d055-144">You can use [XML documentation comments](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="8d055-145">Chcete-li povolit tuto funkci, otevřete soubor oblasti nebo HelpPage nebo aplikace\_Start/HelpPageConfig.cs a zrušte komentář u následujícího řádku:</span><span class="sxs-lookup"><span data-stu-id="8d055-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="8d055-146">Teď povolte dokumentace XML.</span><span class="sxs-lookup"><span data-stu-id="8d055-146">Now enable XML documentation.</span></span> <span data-ttu-id="8d055-147">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8d055-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="8d055-148">Vyberte **sestavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="8d055-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="8d055-149">V části **výstup**, zkontrolujte **souborů dokumentace XML**.</span><span class="sxs-lookup"><span data-stu-id="8d055-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="8d055-150">Do textového pole zadejte "aplikace\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="8d055-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="8d055-151">Dále otevřete kód pro `ValuesController` kontroler API, která je definována v /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="8d055-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="8d055-152">Přidejte do metody kontroleru některé dokumentační komentáře.</span><span class="sxs-lookup"><span data-stu-id="8d055-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="8d055-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8d055-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="8d055-154">Tip: Pokud umístit pomocí kurzoru na řádku výše metodu a zadejte tři lomítka, Visual Studio automaticky vloží elementy XML.</span><span class="sxs-lookup"><span data-stu-id="8d055-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="8d055-155">Potom můžete vyplnit prázdné znaky.</span><span class="sxs-lookup"><span data-stu-id="8d055-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="8d055-156">Nyní sestavení a spusťte aplikaci znovu a přejděte na stránky nápovědy.</span><span class="sxs-lookup"><span data-stu-id="8d055-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="8d055-157">Řetězce dokumentace by se zobrazit v tabulce rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="8d055-158">Na stránce nápovědy přečte řetězce ze souboru XML v době běhu.</span><span class="sxs-lookup"><span data-stu-id="8d055-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="8d055-159">(Když nasadíte aplikaci, ujistěte se, k nasazení souboru XML.)</span><span class="sxs-lookup"><span data-stu-id="8d055-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="8d055-160">Pod pokličkou</span><span class="sxs-lookup"><span data-stu-id="8d055-160">Under the Hood</span></span>

<span data-ttu-id="8d055-161">Stránky nápovědy jsou postavený na **ApiExplorer** třídy, která je součástí rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="8d055-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="8d055-162">**ApiExplorer** třída poskytuje suroviny pro vytvoření stránky nápovědy.</span><span class="sxs-lookup"><span data-stu-id="8d055-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="8d055-163">Pro každé rozhraní API **ApiExplorer** obsahuje **ApiDescription** , který popisuje rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="8d055-164">Pro tento účel "API" se rozumí kombinace metodu HTTP a relativní identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="8d055-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="8d055-165">Tady jsou například některé odlišné rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="8d055-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="8d055-166">ZÍSKAT /api/Products</span><span class="sxs-lookup"><span data-stu-id="8d055-166">GET /api/Products</span></span>
- <span data-ttu-id="8d055-167">ZÍSKAT /api/produkty / {id}</span><span class="sxs-lookup"><span data-stu-id="8d055-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="8d055-168">POST/api/produkty</span><span class="sxs-lookup"><span data-stu-id="8d055-168">POST /api/Products</span></span>

<span data-ttu-id="8d055-169">Pokud je akce kontroleru podporuje více metod HTTP, **ApiExplorer** každá metoda zpracovává jako odlišné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="8d055-170">Skrytí rozhraní API z **ApiExplorer**, přidejte **ApiExplorerSettings** atribut akci a sadu *IgnoreApi* na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="8d055-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="8d055-171">Můžete také přidat tento atribut řadiče, vyloučit celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="8d055-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="8d055-172">Třída ApiExplorer získá dokumentaci řetězců z **IDocumentationProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8d055-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="8d055-173">Jak už jste viděli dříve, knihovny pomoci stránek poskytuje **IDocumentationProvider** , který získá dokumentaci z řetězců dokumentace XML.</span><span class="sxs-lookup"><span data-stu-id="8d055-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="8d055-174">Kód se nachází v /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="8d055-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="8d055-175">Můžete získat dokumentaci z jiného zdroje vytvořením vlastní **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="8d055-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="8d055-176">Chcete-li propojit je s nahoru, zavolejte **SetDocumentationProvider** rozšíření metody definované v **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="8d055-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="8d055-177">**ApiExplorer** automaticky volání **IDocumentationProvider** rozhraní pro získání dokumentace řetězce pro každé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8d055-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="8d055-178">Ukládá je do **dokumentace** vlastnost **ApiDescription** a **ApiParameterDescription** objekty.</span><span class="sxs-lookup"><span data-stu-id="8d055-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d055-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d055-179">Next Steps</span></span>

<span data-ttu-id="8d055-180">Nejste omezená na stránky nápovědy, které jsou tady uvedené.</span><span class="sxs-lookup"><span data-stu-id="8d055-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="8d055-181">Ve skutečnosti **ApiExplorer** není omezeno na vytváření stránky nápovědy.</span><span class="sxs-lookup"><span data-stu-id="8d055-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="8d055-182">Některé skvělé příspěvky si myslím mimo pole má zapisovat Yao Huang odkazů:</span><span class="sxs-lookup"><span data-stu-id="8d055-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="8d055-183">Přidání jednoduché testovacího klienta na stránce nápovědy k serveru ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="8d055-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="8d055-184">Provedení ASP.NET Web API stránce nápovědy k pracovat s vlastním hostováním služby</span><span class="sxs-lookup"><span data-stu-id="8d055-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="8d055-185">Vytvoření v době návrhu nápovědy stránky (nebo klienta) pro ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="8d055-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="8d055-186">Rozšířené přizpůsobení stránce nápovědy</span><span class="sxs-lookup"><span data-stu-id="8d055-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
