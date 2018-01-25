---
title: "Pomocí Boweru v ASP.NET Core"
author: rick-anderson
description: "Spravovat balíčky klienta s Bower."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e3e936c81126b7ed01332565f997910a2886993
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="25bdd-103">Spravovat balíčky klienta s Bower v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25bdd-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="25bdd-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel rýže](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="25bdd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="25bdd-105">A udržovat Bower se doporučuje použít jiné řešení.</span><span class="sxs-lookup"><span data-stu-id="25bdd-105">While Bower is maintained, they recommend using a different solution.</span></span> <span data-ttu-id="25bdd-106">Yarn s Webpack je jeden oblíbenou alternativu, pro kterou [pokyny k migraci](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="25bdd-106">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="25bdd-107">[Bower](https://bower.io/) volá sám sebe "Správce balíčků pro web".</span><span class="sxs-lookup"><span data-stu-id="25bdd-107">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="25bdd-108">V ekosystému .NET zaplňování void zanechaný NuGet neschopnost poskytovat statické soubory obsahu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-108">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="25bdd-109">Pro projekty ASP.NET Core, jsou tyto statické soubory systému na straně klienta knihovny jako [jQuery](http://jquery.com/) a [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="25bdd-109">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="25bdd-110">Pro knihovny .NET, můžete dál používat [NuGet](https://www.nuget.org/) Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="25bdd-110">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="25bdd-111">Proces sestavení nové projekty vytvořené pomocí šablony projektů ASP.NET Core nastavení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="25bdd-111">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="25bdd-112">[jQuery](http://jquery.com/) a [Bootstrap](http://getbootstrap.com/) jsou nainstalovány, a Bower je podporována.</span><span class="sxs-lookup"><span data-stu-id="25bdd-112">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="25bdd-113">Balíčky klienta jsou uvedeny v *bower.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="25bdd-113">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="25bdd-114">Šablony projektů ASP.NET Core nakonfiguruje *bower.json* s Bootstrap, jQuery a k ověřování jQuery.</span><span class="sxs-lookup"><span data-stu-id="25bdd-114">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="25bdd-115">V tomto kurzu přidáme podpora [písma Super](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="25bdd-115">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="25bdd-116">Bower balíčky můžete nainstalovat **spravovat balíčky Bower** uživatelského rozhraní nebo ručně v *bower.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="25bdd-116">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="25bdd-117">Instalaci přes spravovat balíčky Bower uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="25bdd-117">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="25bdd-118">Vytvoření nové aplikace ASP.NET – webové jádro s **webové aplikace ASP.NET Core (.NET Core)** šablony.</span><span class="sxs-lookup"><span data-stu-id="25bdd-118">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="25bdd-119">Vyberte **webovou aplikaci** a **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="25bdd-119">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="25bdd-120">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat balíčky Bower** (případně z hlavní nabídky, **projektu** > **spravovat balíčky Bower**).</span><span class="sxs-lookup"><span data-stu-id="25bdd-120">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="25bdd-121">V **Bower: \<název projektu\>**  okně klikněte na kartu "Browse" a pak filtrovat seznam balíčků zadáním `font-awesome` do vyhledávacího pole:</span><span class="sxs-lookup"><span data-stu-id="25bdd-121">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![Správa balíčků bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="25bdd-123">Potvrďte, že "uložit změny do *bower.json*" je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="25bdd-123">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="25bdd-124">V rozevíracím seznamu vyberte verzi a klikněte na tlačítko **nainstalovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25bdd-124">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="25bdd-125">**Výstup** okně se zobrazí podrobné informace o instalaci.</span><span class="sxs-lookup"><span data-stu-id="25bdd-125">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="25bdd-126">Ruční instalace v bower.json</span><span class="sxs-lookup"><span data-stu-id="25bdd-126">Manual installation in bower.json</span></span>

<span data-ttu-id="25bdd-127">Otevřete *bower.json* souboru a přidejte do závislosti "font Super".</span><span class="sxs-lookup"><span data-stu-id="25bdd-127">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="25bdd-128">IntelliSense zobrazuje dostupné balíčky.</span><span class="sxs-lookup"><span data-stu-id="25bdd-128">IntelliSense shows the available packages.</span></span> <span data-ttu-id="25bdd-129">Pokud je vybraný balíček, zobrazí se dostupné verze.</span><span class="sxs-lookup"><span data-stu-id="25bdd-129">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="25bdd-130">Následující obrázky jsou starší a nebude odpovídat co vidíte.</span><span class="sxs-lookup"><span data-stu-id="25bdd-130">The images below are older and won't match what you see.</span></span>

![IntelliSense bower balíček Průzkumníka](bower/_static/add-package.png)

![bower verze IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="25bdd-133">Bower používá [sémantické verze](http://semver.org/) k uspořádání závislosti.</span><span class="sxs-lookup"><span data-stu-id="25bdd-133">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="25bdd-134">Sémantické verze, také známé jako SemVer identifikuje balíčky s schématu číslování \<hlavní >.\< méně závažné >. \<oprava >.</span><span class="sxs-lookup"><span data-stu-id="25bdd-134">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="25bdd-135">IntelliSense zjednodušuje tím, že zobrazuje pouze několik běžné volby sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="25bdd-135">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="25bdd-136">Horní položku v seznamu IntelliSense (4.6.3 v předchozím příkladu), je považován za nejnovější stabilní verze balíčku.</span><span class="sxs-lookup"><span data-stu-id="25bdd-136">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="25bdd-137">Symbol šipka nahoru (^) odpovídá nejnovější hlavní verzi a znak tilda (~) odpovídá nejnovější dílčí verzi.</span><span class="sxs-lookup"><span data-stu-id="25bdd-137">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="25bdd-138">Uložit *bower.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="25bdd-138">Save the *bower.json* file.</span></span> <span data-ttu-id="25bdd-139">Visual Studio sleduje *bower.json* změny v souboru.</span><span class="sxs-lookup"><span data-stu-id="25bdd-139">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="25bdd-140">Při ukládání, *bower instalace* se spustí příkaz.</span><span class="sxs-lookup"><span data-stu-id="25bdd-140">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="25bdd-141">Najdete v okně výstupu **Bower nebo npm** zobrazení pro přesný příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="25bdd-141">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="25bdd-142">Otevřete *.bowerrc* souboru pod *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="25bdd-142">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="25bdd-143">`directory` Je nastavena na *wwwroot/lib* což označuje umístění Bower nainstaluje balíček prostředků.</span><span class="sxs-lookup"><span data-stu-id="25bdd-143">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="25bdd-144">Do vyhledávacího pole v Průzkumníku řešení můžete použít k vyhledání a zobrazení písma Super balíčku.</span><span class="sxs-lookup"><span data-stu-id="25bdd-144">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="25bdd-145">Otevřete *Views\Shared\_Layout.cshtml* souboru a přidejte soubor písma Super šablon stylů CSS v prostředí [značky pomocná](xref:mvc/views/tag-helpers/intro) pro `Development`.</span><span class="sxs-lookup"><span data-stu-id="25bdd-145">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="25bdd-146">V Průzkumníku řešení přetažení *písma awesome.css* uvnitř `<environment names="Development">` elementu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-146">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="25bdd-147">V produkční aplikace přidat *písma awesome.min.css* do pomocné rutiny prostředí značky pro `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="25bdd-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="25bdd-148">Nahraďte obsah *Views\Home\About.cshtml* souboru nástroje Razor s následující kód:</span><span class="sxs-lookup"><span data-stu-id="25bdd-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="25bdd-149">Spusťte aplikaci a přejděte do zobrazení o ověření funguje písma Super balíčku.</span><span class="sxs-lookup"><span data-stu-id="25bdd-149">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="25bdd-150">Zkoumání procesu sestavení na straně klienta</span><span class="sxs-lookup"><span data-stu-id="25bdd-150">Exploring the client-side build process</span></span>

<span data-ttu-id="25bdd-151">Většina šablony projektů ASP.NET Core jsou již byla konfigurována pro použití Bower.</span><span class="sxs-lookup"><span data-stu-id="25bdd-151">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="25bdd-152">Tento další návod začíná prázdný projekt ASP.NET Core a přidá každého jednotlivého ručně, abyste získali chování pro použití Bower v projektu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-152">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="25bdd-153">Můžete zobrazit, co se stane s strukturu projektu a runtime výstup jako každé změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="25bdd-153">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="25bdd-154">Obecné kroky pro použití s Bower procesu sestavení na straně klienta jsou:</span><span class="sxs-lookup"><span data-stu-id="25bdd-154">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="25bdd-155">Definujte balíčky použité ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-155">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="25bdd-156">Odkaz na balíčky z webových stránek.</span><span class="sxs-lookup"><span data-stu-id="25bdd-156">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="25bdd-157">Definování balíčků</span><span class="sxs-lookup"><span data-stu-id="25bdd-157">Define packages</span></span>

<span data-ttu-id="25bdd-158">Jakmile seznam balíčků v *bower.json* je se stažení souboru, Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25bdd-158">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="25bdd-159">Následující příklad používá k načtení knihovny jQuery a Bootstrap na Bower *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="25bdd-159">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="25bdd-160">Vytvoření nové aplikace ASP.NET – webové jádro s **webové aplikace ASP.NET Core (.NET Core)** šablony.</span><span class="sxs-lookup"><span data-stu-id="25bdd-160">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="25bdd-161">Vyberte **prázdný** šablona projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="25bdd-161">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="25bdd-162">V Průzkumníku řešení klikněte pravým tlačítkem na projekt > **přidat novou položku** a vyberte **Bower konfigurační soubor**.</span><span class="sxs-lookup"><span data-stu-id="25bdd-162">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="25bdd-163">Poznámka: A *.bowerrc* soubor je taky přidaný.</span><span class="sxs-lookup"><span data-stu-id="25bdd-163">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="25bdd-164">Otevřete *bower.json*a přidejte jquery a bootstrap na `dependencies` oddílu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-164">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="25bdd-165">Výsledná *bower.json* soubor bude vypadat jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-165">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="25bdd-166">Verze bude časem změnit a nemusí odpovídat následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="25bdd-166">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="25bdd-167">Uložit *bower.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="25bdd-167">Save the *bower.json* file.</span></span>

 <span data-ttu-id="25bdd-168">Ověřte, tento projekt zahrnuje *bootstrap* a *jQuery* adresářů v *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="25bdd-168">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="25bdd-169">Bower používá *.bowerrc* instalace prostředků v souboru *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="25bdd-169">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="25bdd-170">Poznámka: Rozhraní "Správa balíčků Bower" poskytuje alternativu k ruční souboru úpravy.</span><span class="sxs-lookup"><span data-stu-id="25bdd-170">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="25bdd-171">Povolte statické soubory</span><span class="sxs-lookup"><span data-stu-id="25bdd-171">Enable static files</span></span>

* <span data-ttu-id="25bdd-172">Přidat `Microsoft.AspNetCore.StaticFiles` balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="25bdd-172">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="25bdd-173">Povolte statické soubory ke zpracování s [middleware se statickými soubory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="25bdd-173">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="25bdd-174">Přidejte volání [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) k `Configure` metodu `Startup`.</span><span class="sxs-lookup"><span data-stu-id="25bdd-174">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="25bdd-175">Referenčních balíčků</span><span class="sxs-lookup"><span data-stu-id="25bdd-175">Reference packages</span></span>

<span data-ttu-id="25bdd-176">V této části vytvoříte stránky HTML a ověří, zda má přístup k nasazených balíčků.</span><span class="sxs-lookup"><span data-stu-id="25bdd-176">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="25bdd-177">Přidat novou stránku HTML s názvem *Index.html* k *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="25bdd-177">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="25bdd-178">Poznámka: Je nutné přidat soubor HTML *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="25bdd-178">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="25bdd-179">Ve výchozím nastavení, nelze zpracovat statický obsah mimo *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="25bdd-179">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="25bdd-180">V tématu [práce s statické soubory](xref:fundamentals/static-files) Další informace.</span><span class="sxs-lookup"><span data-stu-id="25bdd-180">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="25bdd-181">Nahraďte obsah *Index.html* s následující kód:</span><span class="sxs-lookup"><span data-stu-id="25bdd-181">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="25bdd-182">Spusťte aplikaci a přejděte do `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="25bdd-182">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="25bdd-183">Alternativně s *Index.html* otevřené, stiskněte `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="25bdd-183">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="25bdd-184">Ověřte, že se použije jumbotron stylů, kód jazyka jQuery odpovídá při kliknutí na tlačítko a že zavedení tlačítko se změní stav.</span><span class="sxs-lookup"><span data-stu-id="25bdd-184">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![Styl jumbotron použitý](bower/_static/jumbotron.png)
