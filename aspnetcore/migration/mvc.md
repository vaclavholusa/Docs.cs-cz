---
title: "Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: e3220fb32900aac42cf96497964936ad5b375a86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="429d2-102">Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="429d2-102">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="429d2-103">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="429d2-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="429d2-104">Tento článek ukazuje, jak začít s migrací do projektu aplikace ASP.NET MVC [ASP.NET MVC základní](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="429d2-104">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="429d2-105">V procesu se označuje mnoho věcí, které se změnily z rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="429d2-105">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="429d2-106">Migrace z rozhraní ASP.NET MVC je více proces krok a tento článek se zabývá počáteční nastavení, základní řadiče a zobrazení, statický obsah a závislosti na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="429d2-106">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="429d2-107">Další články zahrnovat migraci konfigurace a kód identit v mnoha projekty ASP.NET MVC nalezen.</span><span class="sxs-lookup"><span data-stu-id="429d2-107">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="429d2-108">Čísla verzí v ukázkách nemusí být aktuální.</span><span class="sxs-lookup"><span data-stu-id="429d2-108">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="429d2-109">Musíte aktualizovat projekty, odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="429d2-109">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="429d2-110">Vytvořit úvodní projektu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="429d2-110">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="429d2-111">K předvedení upgradu, začneme vytvořením aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="429d2-111">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="429d2-112">Vytvořit s názvem *WebApp1* , obor názvů bude shodovat s projektu ASP.NET Core vytvoříme v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="429d2-112">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Dialogové okno Visual Studio nový projekt](mvc/_static/new-project.png)

![Dialogové okno nové webové aplikace: projektu šablony MVC vybrali panelu šablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="429d2-115">*Volitelné:* změnit název řešení od *WebApp1* k *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="429d2-115">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="429d2-116">Visual Studio se zobrazí název nového řešení (*Mvc5*), které usnadní říct tento projekt z projektu další.</span><span class="sxs-lookup"><span data-stu-id="429d2-116">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="429d2-117">Vytvoření projektu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="429d2-117">Create the ASP.NET Core project</span></span>

<span data-ttu-id="429d2-118">Vytvořte novou *prázdný* ASP.NET Core webové aplikace se stejným názvem jako předchozí projekt (*WebApp1*), obory názvů v dva projekty shodovat.</span><span class="sxs-lookup"><span data-stu-id="429d2-118">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="429d2-119">S o stejný obor názvů usnadňuje zkopírujte kód mezi dva projekty.</span><span class="sxs-lookup"><span data-stu-id="429d2-119">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="429d2-120">Budete mít k vytvoření tohoto projektu v jiném adresáři než předchozí projekt, který používá stejný název.</span><span class="sxs-lookup"><span data-stu-id="429d2-120">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Dialogové okno Nový projekt](mvc/_static/new_core.png)

![Dialogové okno nové webové aplikace ASP.NET: prázdná šablona projektu vybrané panelu ASP.NET Core šablony](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="429d2-123">*Volitelné:* vytvoření nové aplikace ASP.NET Core pomocí *webové aplikace* šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="429d2-123">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="429d2-124">Název projektu *WebApp1*a vyberte některou možnost ověřování z **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="429d2-124">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="429d2-125">Přejmenujte tuto aplikaci a *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="429d2-125">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="429d2-126">Vytvoření projektu budou ušetřit čas v převodu.</span><span class="sxs-lookup"><span data-stu-id="429d2-126">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="429d2-127">Můžete si prohlédnout kód generovaný šablony najdete v části konečný výsledek nebo zkopírujte kód do projektu převod.</span><span class="sxs-lookup"><span data-stu-id="429d2-127">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="429d2-128">Je také užitečné, pokud zablokuje v kroku převod k porovnání s projektem šablona vytvořena.</span><span class="sxs-lookup"><span data-stu-id="429d2-128">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="429d2-129">Konfigurace lokality k použití MVC</span><span class="sxs-lookup"><span data-stu-id="429d2-129">Configure the site to use MVC</span></span>

* <span data-ttu-id="429d2-130">Nainstalujte `Microsoft.AspNetCore.Mvc` a `Microsoft.AspNetCore.StaticFiles` balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="429d2-130">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="429d2-131">`Microsoft.AspNetCore.Mvc`je rozhraní ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="429d2-131">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="429d2-132">`Microsoft.AspNetCore.StaticFiles`je obslužná rutina statických souborů.</span><span class="sxs-lookup"><span data-stu-id="429d2-132">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="429d2-133">Modul runtime ASP.NET je modulární a musí explicitně přihlášení poskytovat statické soubory (viz [práce s statické soubory](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="429d2-133">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="429d2-134">Otevřete *.csproj* souboru (klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **upravit WebApp1.csproj**) a přidejte `PrepareForPublish` cíl:</span><span class="sxs-lookup"><span data-stu-id="429d2-134">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="429d2-135">`PrepareForPublish` Cíl je nutný k získání klientské knihovny prostřednictvím Bower.</span><span class="sxs-lookup"><span data-stu-id="429d2-135">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="429d2-136">Budeme mluvit o který později.</span><span class="sxs-lookup"><span data-stu-id="429d2-136">We'll talk about that later.</span></span>

* <span data-ttu-id="429d2-137">Otevřete *Startup.cs* souboru a změnit kód tak, aby odpovídala následující:</span><span class="sxs-lookup"><span data-stu-id="429d2-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="429d2-138">`UseStaticFiles` Metoda rozšíření přidá obslužné rutiny statických souborů.</span><span class="sxs-lookup"><span data-stu-id="429d2-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="429d2-139">Jak je uvedeno nahoře, modulem runtime ASP.NET je modulární a musí explicitně přihlášení poskytovat statické soubory.</span><span class="sxs-lookup"><span data-stu-id="429d2-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="429d2-140">`UseMvc` Přidá metody rozšíření směrování.</span><span class="sxs-lookup"><span data-stu-id="429d2-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="429d2-141">Další informace najdete v tématu [spuštění aplikace](../fundamentals/startup.md) a [směrování](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="429d2-141">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="429d2-142">Přidání kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="429d2-142">Add a controller and view</span></span>

<span data-ttu-id="429d2-143">V této části přidáte minimální řadiče a zobrazení, která bude sloužit jako zástupné symboly ASP.NET MVC jsou řadič MVC a zobrazení, že budete migrovat v další části.</span><span class="sxs-lookup"><span data-stu-id="429d2-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="429d2-144">Přidat *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="429d2-145">Přidat **třídy kontroleru MVC** s názvem *HomeController.cs* k *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-145">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Přidat novou položku – dialogové okno](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="429d2-147">Přidat *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="429d2-148">Přidat *zobrazení Domů* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="429d2-149">Přidat *Index.cshtml* stránka zobrazení MVC do *zobrazení Domů* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-149">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Přidat novou položku – dialogové okno](mvc/_static/view.png)

<span data-ttu-id="429d2-151">Struktura projektu je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="429d2-151">The project structure is shown below:</span></span>

![Průzkumník řešení zobrazující soubory a složky WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="429d2-153">Nahraďte obsah *Views/Home/Index.cshtml* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="429d2-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="429d2-154">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="429d2-154">Run the app.</span></span>

![Webové aplikace, otevřete v Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="429d2-156">V tématu [řadiče](../mvc/controllers/index.md) a [zobrazení](../mvc/views/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="429d2-156">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="429d2-157">Teď, když máme minimální funkční projekt ASP.NET Core, můžeme začít migrace funkce z projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="429d2-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="429d2-158">Budeme muset přesunout následující:</span><span class="sxs-lookup"><span data-stu-id="429d2-158">We will need to move the following:</span></span>

* <span data-ttu-id="429d2-159">obsah na straně klienta (šablon stylů CSS, písma a skripty)</span><span class="sxs-lookup"><span data-stu-id="429d2-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="429d2-160">kontrolery</span><span class="sxs-lookup"><span data-stu-id="429d2-160">controllers</span></span>

* <span data-ttu-id="429d2-161">zobrazení</span><span class="sxs-lookup"><span data-stu-id="429d2-161">views</span></span>

* <span data-ttu-id="429d2-162">modely</span><span class="sxs-lookup"><span data-stu-id="429d2-162">models</span></span>

* <span data-ttu-id="429d2-163">Sdružování</span><span class="sxs-lookup"><span data-stu-id="429d2-163">bundling</span></span>

* <span data-ttu-id="429d2-164">filtry</span><span class="sxs-lookup"><span data-stu-id="429d2-164">filters</span></span>

* <span data-ttu-id="429d2-165">Přihlaste se vstup/výstup identity (to bude provedeno v dalším kurzu.)</span><span class="sxs-lookup"><span data-stu-id="429d2-165">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="429d2-166">Kontrolery a zobrazení</span><span class="sxs-lookup"><span data-stu-id="429d2-166">Controllers and views</span></span>

* <span data-ttu-id="429d2-167">Zkopírujte všechny metody z rozhraní ASP.NET MVC `HomeController` do nového `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="429d2-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="429d2-168">Upozorňujeme, že v architektuře ASP.NET MVC předdefinované šablony řadiče akce metoda návratový typ je [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); v aplikaci ASP.NET MVC jádra, metody akce návratový `IActionResult` místo.</span><span class="sxs-lookup"><span data-stu-id="429d2-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="429d2-169">`ActionResult`implementuje `IActionResult`, takže není nutné změnit návratový typ vaší metody akce.</span><span class="sxs-lookup"><span data-stu-id="429d2-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="429d2-170">Kopírování *About.cshtml*, *Contact.cshtml*, a *Index.cshtml* Razor zobrazit soubory z projektu ASP.NET MVC do projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="429d2-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="429d2-171">Spuštění aplikace ASP.NET Core a testování jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="429d2-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="429d2-172">Jsme jste nemigrovali rozložení souboru nebo styly ještě, takže vykreslené zobrazení bude obsahovat pouze obsah souborů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="429d2-172">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="429d2-173">Nebudete mít rozložení souboru vygenerovaného odkazy `About` a `Contact` zobrazení, takže budete muset je vyvolat z prohlížeče (Nahraďte **4492** číslem portu, na které se používají ve vašem projektu).</span><span class="sxs-lookup"><span data-stu-id="429d2-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kontaktujte stránky](mvc/_static/contact-page.png)

<span data-ttu-id="429d2-175">Poznámka: nedostatek položky stylů a nabídky.</span><span class="sxs-lookup"><span data-stu-id="429d2-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="429d2-176">To jsme budete opravíme v další části.</span><span class="sxs-lookup"><span data-stu-id="429d2-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="429d2-177">Statický obsah</span><span class="sxs-lookup"><span data-stu-id="429d2-177">Static content</span></span>

<span data-ttu-id="429d2-178">V předchozích verzích rozhraní ASP.NET MVC statický obsah hostitelem byl z kořenového adresáře webového projektu a byl smíšeného s soubory na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="429d2-178">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="429d2-179">V ASP.NET Core, statický obsah uložený v *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="429d2-180">Budete chtít zkopírujte statický obsah z původního aplikaci ASP.NET MVC *wwwroot* složku ve vašem projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="429d2-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="429d2-181">V této ukázkové převod:</span><span class="sxs-lookup"><span data-stu-id="429d2-181">In this sample conversion:</span></span>

* <span data-ttu-id="429d2-182">Kopírování *favicon.ico* soubor z původní projekt MVC *wwwroot* složky v projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="429d2-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="429d2-183">Starý ASP.NET MVC projektu používá [Bootstrap](http://getbootstrap.com/) pro jeho stylů a úložišť, službou Bootstrap nástroje soubory *obsahu* a *skripty* složek.</span><span class="sxs-lookup"><span data-stu-id="429d2-183">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="429d2-184">Šablony, která generuje staré projektu ASP.NET MVC, odkazuje na Bootstrap v rozložení souboru (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="429d2-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="429d2-185">Může zkopírovat *bootstrap.js* a *bootstrap.css* soubory z rozhraní ASP.NET MVC projektu do *wwwroot* nepoužívá složky v novém projektu, ale tento přístup Vylepšené mechanismus pro správu klienta závislostí v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="429d2-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="429d2-186">V novém projektu, přidáme podporu pro Bootstrap (a další klientské knihovny) pomocí [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="429d2-186">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="429d2-187">Přidat [Bower](https://bower.io/) konfigurační soubor s názvem *bower.json* do kořenového adresáře projektu (klikněte pravým tlačítkem na projekt a potom **Přidat > novou položku > Bower konfigurační soubor**).</span><span class="sxs-lookup"><span data-stu-id="429d2-187">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="429d2-188">Přidat [Bootstrap](http://getbootstrap.com/) a [jQuery](https://jquery.com/) do souboru (viz níže zvýrazněné řádky).</span><span class="sxs-lookup"><span data-stu-id="429d2-188">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="429d2-189">Při ukládání souboru, Bower automaticky stáhnout závislosti na *wwwroot/lib* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-189">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="429d2-190">Můžete použít **Průzkumník služby Search řešení** pole najít cestu prostředky:</span><span class="sxs-lookup"><span data-stu-id="429d2-190">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![prostředky jQuery zobrazený ve výsledcích hledání Průzkumníku řešení](mvc/_static/search.png)

<span data-ttu-id="429d2-192">V tématu [spravovat klientské balíčky s Bower](../client-side/bower.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="429d2-192">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="429d2-193">Migrace na soubor rozložení</span><span class="sxs-lookup"><span data-stu-id="429d2-193">Migrate the layout file</span></span>

* <span data-ttu-id="429d2-194">Kopírování *soubor _ViewStart.cshtml* soubor z původního projektu ASP.NET MVC *zobrazení* složky do projektů ASP.NET Core *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-194">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="429d2-195">*Soubor _ViewStart.cshtml* soubor nebyl změněn v aplikaci ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="429d2-195">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="429d2-196">Vytvoření *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-196">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="429d2-197">*Volitelné:* kopie *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *zobrazení* složky do projektu ASP.NET Core *Zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-197">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="429d2-198">Odeberte všechny deklarace oboru názvů v *_ViewImports.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="429d2-198">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="429d2-199">*_ViewImports.cshtml* soubor poskytuje obory názvů pro všechny soubory, zobrazení a přináší [značky Pomocníci](../mvc/views/tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="429d2-199">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="429d2-200">Pomocníci značky se používají v nové rozložení souboru.</span><span class="sxs-lookup"><span data-stu-id="429d2-200">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="429d2-201">*_ViewImports.cshtml* souboru je nového pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="429d2-201">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="429d2-202">Kopírování *_Layout.cshtml* soubor z původního projektu ASP.NET MVC *zobrazení a sdílených* složky do projektů ASP.NET Core *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="429d2-202">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="429d2-203">Otevřete *_Layout.cshtml* souboru a proveďte následující změny (dokončený kód je zobrazené dole):</span><span class="sxs-lookup"><span data-stu-id="429d2-203">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="429d2-204">Nahraďte `@Styles.Render("~/Content/css")` s `<link>` elementu, který chcete načíst *bootstrap.css* (viz níže).</span><span class="sxs-lookup"><span data-stu-id="429d2-204">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="429d2-205">Odebrat `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="429d2-205">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="429d2-206">Komentář `@Html.Partial("_LoginPartial")` řádku (obklopit řádek s `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="429d2-206">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="429d2-207">Vrátí k němu v budoucnu kurzu.</span><span class="sxs-lookup"><span data-stu-id="429d2-207">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="429d2-208">Nahraďte `@Scripts.Render("~/bundles/jquery")` s `<script>` – element (viz níže).</span><span class="sxs-lookup"><span data-stu-id="429d2-208">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="429d2-209">Nahraďte `@Scripts.Render("~/bundles/bootstrap")` s `<script>` – element (viz níže)...</span><span class="sxs-lookup"><span data-stu-id="429d2-209">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="429d2-210">Odkaz nahrazení šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="429d2-210">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="429d2-211">Značky skriptu nahrazení:</span><span class="sxs-lookup"><span data-stu-id="429d2-211">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="429d2-212">Aktualizovaný *_Layout.cshtml* souboru jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="429d2-212">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="429d2-213">Zobrazte webu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="429d2-213">View the site in the browser.</span></span> <span data-ttu-id="429d2-214">Nyní by se měly správně načíst s očekávanou styly na místě.</span><span class="sxs-lookup"><span data-stu-id="429d2-214">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="429d2-215">*Volitelné:* můžete chtít zkuste použít nový soubor rozložení.</span><span class="sxs-lookup"><span data-stu-id="429d2-215">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="429d2-216">Pro tento projekt můžete zkopírovat soubor rozložení z *FullAspNetCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="429d2-216">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="429d2-217">Nový soubor rozložení používá [značky Pomocníci](../mvc/views/tag-helpers/index.md) a má další vylepšení.</span><span class="sxs-lookup"><span data-stu-id="429d2-217">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="429d2-218">Konfigurace sdružování & minimalizaci</span><span class="sxs-lookup"><span data-stu-id="429d2-218">Configure Bundling & Minification</span></span>

<span data-ttu-id="429d2-219">Informace o tom, jak nakonfigurovat sdružování a minimalizace najdete v tématu [sdružování a Minifikace](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="429d2-219">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="429d2-220">Řešení chyb HTTP 500</span><span class="sxs-lookup"><span data-stu-id="429d2-220">Solving HTTP 500 errors</span></span>

<span data-ttu-id="429d2-221">Existuje mnoho problémů, které můžou způsobit chybová zpráva HTTP 500 které neobsahují žádné informace na zdroj problému.</span><span class="sxs-lookup"><span data-stu-id="429d2-221">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="429d2-222">Například pokud *Views/_ViewImports.cshtml* soubor obsahuje obor názvů, který neexistuje v projektu, získáte chyby HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="429d2-222">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="429d2-223">Chcete-li získat podrobné chybové zprávy, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="429d2-223">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="429d2-224">V tématu **pomocí stránky výjimka vývojáře** v [zpracování chyb](../fundamentals/error-handling.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="429d2-224">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="429d2-225">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="429d2-225">Additional Resources</span></span>

* [<span data-ttu-id="429d2-226">Vývoj straně klienta</span><span class="sxs-lookup"><span data-stu-id="429d2-226">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="429d2-227">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="429d2-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
