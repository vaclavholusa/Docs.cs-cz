---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Vytváření stránek nápovědy webového rozhraní API ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8803c406660398ad3314306a1bcdf99af418082d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393126"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="49d79-102">Vytváření stránek nápovědy pro rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="49d79-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="49d79-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="49d79-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="49d79-104">Při vytváření webového rozhraní API, často je užitečné vytvořit stránku nápovědy, tak, aby ostatní vývojáři vědět, jak volat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="49d79-105">Dokumentace může vytvořit ručně, ale je lepší automaticky vytvořit co největší míře.</span><span class="sxs-lookup"><span data-stu-id="49d79-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="49d79-106">Pro usnadnění tohoto úkolu, rozhraní ASP.NET Web API poskytuje knihovnu pro automatické generování stránek nápovědy v době běhu.</span><span class="sxs-lookup"><span data-stu-id="49d79-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="49d79-107">Vytváření stránek nápovědy k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="49d79-107">Creating API Help Pages</span></span>

<span data-ttu-id="49d79-108">Nainstalujte [ASP.NET a Web Tools 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="49d79-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="49d79-109">Tato aktualizace stránky nápovědy integruje šablony projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="49d79-110">Dále vytvořte nový projekt ASP.NET MVC 4 a vyberte šablonu projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="49d79-111">Šablona projektu vytvoří řadič ukázkové rozhraní API s názvem `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="49d79-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="49d79-112">Šablona vytvoří také stránek nápovědy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-112">The template also creates the API help pages.</span></span> <span data-ttu-id="49d79-113">Všechny soubory kódu stránky s nápovědou jsou umístěny ve složce oblasti projektu.</span><span class="sxs-lookup"><span data-stu-id="49d79-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="49d79-114">Při spuštění aplikace na domovské stránce obsahuje odkaz na stránku nápovědy na rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="49d79-115">Z domovské stránky je relativní cesta/help.</span><span class="sxs-lookup"><span data-stu-id="49d79-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="49d79-116">Tento odkaz vám přináší na souhrnnou stránku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="49d79-117">Pro tuto stránku zobrazení MVC je definována v Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="49d79-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="49d79-118">Můžete upravit této stránky můžete upravit rozložení, úvod, název, styly a tak dále.</span><span class="sxs-lookup"><span data-stu-id="49d79-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="49d79-119">Hlavní části stránky je tabulka rozhraní API, seskupené podle kontroleru.</span><span class="sxs-lookup"><span data-stu-id="49d79-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="49d79-120">Položky tabulky generuje dynamicky pomocí **IApiExplorer** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="49d79-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="49d79-121">(I více zabývat toto rozhraní později.) Pokud chcete přidat nový kontroler API, se automaticky aktualizuje v tabulce v době běhu.</span><span class="sxs-lookup"><span data-stu-id="49d79-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="49d79-122">Sloupce "Rozhraní API" seznam metodu HTTP a relativní identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="49d79-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="49d79-123">Ve sloupci "Popis" obsahuje dokumentaci pro každé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="49d79-124">Na začátku dokumentace je jen zástupný text.</span><span class="sxs-lookup"><span data-stu-id="49d79-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="49d79-125">V další části můžu ukážeme, jak přidat dokumentace ke službě z komentářů XML.</span><span class="sxs-lookup"><span data-stu-id="49d79-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="49d79-126">Každé rozhraní API obsahuje odkaz na stránku se podrobnější informace, včetně příklad těla požadavku a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="49d79-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="49d79-127">Přidání stránky nápovědy k existujícímu projektu</span><span class="sxs-lookup"><span data-stu-id="49d79-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="49d79-128">Stránky nápovědy k existujícímu projektu webového rozhraní API můžete přidat pomocí Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="49d79-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="49d79-129">Tato možnost je užitečná, že spusťte ze šablony jiný projekt než má šablona "Webového rozhraní API".</span><span class="sxs-lookup"><span data-stu-id="49d79-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="49d79-130">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="49d79-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="49d79-131">V [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okno, zadejte jeden z následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="49d79-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="49d79-132">Pro **jazyka C#** aplikace: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="49d79-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="49d79-133">Pro **jazyka Visual Basic** aplikace: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="49d79-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="49d79-134">Existují dva balíčky, jeden pro jazyk C# a jeden pro jazyk Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="49d79-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="49d79-135">Ujistěte se, že chcete používat ten, který odpovídá projektu.</span><span class="sxs-lookup"><span data-stu-id="49d79-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="49d79-136">Tento příkaz nainstaluje potřebná sestavení a přidá zobrazení MVC pro stránky nápovědy (umístěný ve složce plochy/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="49d79-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="49d79-137">Bude potřeba ručně přidat odkaz na stránku nápovědy.</span><span class="sxs-lookup"><span data-stu-id="49d79-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="49d79-138">Identifikátor URI je/help.</span><span class="sxs-lookup"><span data-stu-id="49d79-138">The URI is /Help.</span></span> <span data-ttu-id="49d79-139">Pokud chcete vytvořit odkaz v zobrazení razor, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="49d79-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="49d79-140">Také ujistěte se, že k registraci oblasti.</span><span class="sxs-lookup"><span data-stu-id="49d79-140">Also, make sure to register areas.</span></span> <span data-ttu-id="49d79-141">V souboru Global.asax přidejte následující kód, který **aplikace\_Start** metodu, pokud tam není již:</span><span class="sxs-lookup"><span data-stu-id="49d79-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="49d79-142">Přidání dokumentace k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="49d79-142">Adding API Documentation</span></span>

<span data-ttu-id="49d79-143">Ve výchozím nastavení, v nápovědě stránky obsahují zástupný text řetězce pro dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="49d79-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="49d79-144">Můžete použít [dokumentační komentáře XML](https://msdn.microsoft.com/library/b2s063f7.aspx) vytvořit v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="49d79-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="49d79-145">Chcete-li tuto funkci povolit, otevřete soubor oblasti nebo HelpPage/aplikace\_Start/HelpPageConfig.cs a zrušte komentář u následujícího řádku:</span><span class="sxs-lookup"><span data-stu-id="49d79-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="49d79-146">Nyní povolte dokumentace XML.</span><span class="sxs-lookup"><span data-stu-id="49d79-146">Now enable XML documentation.</span></span> <span data-ttu-id="49d79-147">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="49d79-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="49d79-148">Vyberte **sestavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="49d79-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="49d79-149">V části **výstup**, zkontrolujte **soubor dokumentace XML**.</span><span class="sxs-lookup"><span data-stu-id="49d79-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="49d79-150">Do textového pole zadejte "aplikace\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="49d79-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="49d79-151">Dále otevřete kód `ValuesController` kontroleru rozhraní API, která je definována v /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="49d79-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="49d79-152">Přidáte nějaké komentáře k dokumentaci pro metody kontroleru.</span><span class="sxs-lookup"><span data-stu-id="49d79-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="49d79-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="49d79-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="49d79-154">Tip: Pokud umístit blikající kurzor na řádek nad metodu a zadejte tři lomítka, Visual Studio automaticky vloží prvky XML.</span><span class="sxs-lookup"><span data-stu-id="49d79-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="49d79-155">Potom můžete vyplnit prázdné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="49d79-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="49d79-156">Nyní vytvářet a spusťte aplikaci znovu a přejděte na stránky nápovědy.</span><span class="sxs-lookup"><span data-stu-id="49d79-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="49d79-157">Dokumentace ke službě řetězce by se zobrazit v tabulce rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="49d79-158">Na stránce nápovědy přečte řetězce ze souboru XML v době běhu.</span><span class="sxs-lookup"><span data-stu-id="49d79-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="49d79-159">(Když nasadíte aplikaci, ujistěte se, že k nasazení souboru XML.)</span><span class="sxs-lookup"><span data-stu-id="49d79-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="49d79-160">Pohled pod kapotu</span><span class="sxs-lookup"><span data-stu-id="49d79-160">Under the Hood</span></span>

<span data-ttu-id="49d79-161">Stránky nápovědy jsou zabudovány nad **ApiExplorer** třídu, která je součástí rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="49d79-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="49d79-162">**ApiExplorer** třída poskytuje suroviny pro vytvoření stránky nápovědy.</span><span class="sxs-lookup"><span data-stu-id="49d79-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="49d79-163">Pro každé rozhraní API **ApiExplorer** obsahuje **ApiDescription** popisující rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="49d79-164">V tomto případě "API" je definován jako kombinace relativní URI a metodou HTTP.</span><span class="sxs-lookup"><span data-stu-id="49d79-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="49d79-165">Například tady jsou některé různá rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="49d79-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="49d79-166">ZÍSKAT /api/Products</span><span class="sxs-lookup"><span data-stu-id="49d79-166">GET /api/Products</span></span>
- <span data-ttu-id="49d79-167">ZÍSKAT/webové rozhraní API/produkty / {id}</span><span class="sxs-lookup"><span data-stu-id="49d79-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="49d79-168">Publikovat/api/produkty</span><span class="sxs-lookup"><span data-stu-id="49d79-168">POST /api/Products</span></span>

<span data-ttu-id="49d79-169">Pokud je akce kontroleru podporuje více metod HTTP, **ApiExplorer** považuje každá metoda různých rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="49d79-170">Skrýt rozhraní API z **ApiExplorer**, přidejte **ApiExplorerSettings** akce a nastavte atribut *IgnoreApi* na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="49d79-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="49d79-171">Můžete také přidat tento atribut na řadič, vyloučit celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="49d79-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="49d79-172">Třída ApiExplorer získá dokumentaci řetězců z **IDocumentationProvider** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="49d79-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="49d79-173">Jak jste viděli již dříve, poskytuje knihovna stránky nápovědy **IDocumentationProvider** , který získá dokumentaci z řetězců dokumentaci XML.</span><span class="sxs-lookup"><span data-stu-id="49d79-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="49d79-174">Kód se nachází v /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="49d79-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="49d79-175">Můžete získat dokumentaci z jiného zdroje napsáním vlastní **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="49d79-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="49d79-176">Chcete-li ji nastavit, zavolejte **SetDocumentationProvider** metody rozšíření definované v **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="49d79-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="49d79-177">**ApiExplorer** automaticky zavolá **IDocumentationProvider** rozhraní pro získání dokumentace řetězce pro každé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49d79-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="49d79-178">Ukládá je v **dokumentaci** vlastnost **ApiDescription** a **ApiParameterDescription** objekty.</span><span class="sxs-lookup"><span data-stu-id="49d79-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49d79-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49d79-179">Next Steps</span></span>

<span data-ttu-id="49d79-180">Nejste omezeni na stránky nápovědy je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="49d79-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="49d79-181">Ve skutečnosti **ApiExplorer** není omezena pouze na vytváření stránek nápovědy.</span><span class="sxs-lookup"><span data-stu-id="49d79-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="49d79-182">Ty Huang Yao zapsala si že některé skvělé příspěvky najdete, vám uvažujete úprav:</span><span class="sxs-lookup"><span data-stu-id="49d79-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="49d79-183">Přidání jednoduchého testovacího klienta stránka nápovědy k ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="49d79-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="49d79-184">Vytváření technologie ASP.NET webové rozhraní API pomáhají stránky pracovat na služby v místním prostředí</span><span class="sxs-lookup"><span data-stu-id="49d79-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="49d79-185">Vytvoření v době návrhu nápovědy stránky (nebo klienta) pro rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="49d79-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="49d79-186">Rozšířená přizpůsobení stránce nápovědy</span><span class="sxs-lookup"><span data-stu-id="49d79-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
