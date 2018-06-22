---
title: Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC
author: ardalis
description: Zjistěte, jak začít pracovat migrace projektu aplikace ASP.NET MVC do architektury ASP.NET MVC jádra.
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 6fecc820177ca5033cfd00d632904a950c1b29bb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274772"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="f1fc7-103">Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f1fc7-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="f1fc7-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="f1fc7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="f1fc7-105">Tento článek ukazuje, jak začít s migrací do projektu aplikace ASP.NET MVC [ASP.NET MVC základní](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="f1fc7-106">V procesu se označuje mnoho věcí, které se změnily z rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="f1fc7-107">Migrace z rozhraní ASP.NET MVC je více proces krok a tento článek se zabývá počáteční nastavení, základní řadiče a zobrazení, statický obsah a závislosti na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="f1fc7-108">Další články zahrnovat migraci konfigurace a kód identit v mnoha projekty ASP.NET MVC nalezen.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="f1fc7-109">Čísla verzí v ukázkách nemusí být aktuální.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="f1fc7-110">Musíte aktualizovat projekty, odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="f1fc7-111">Vytvořit úvodní projektu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f1fc7-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="f1fc7-112">K předvedení upgradu, začneme vytvořením aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="f1fc7-113">Vytvořit s názvem *WebApp1* , obor názvů odpovídá projektu ASP.NET Core vytvoříme v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Dialogové okno Visual Studio nový projekt](mvc/_static/new-project.png)

![Dialogové okno nové webové aplikace: projektu šablony MVC vybrali panelu šablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="f1fc7-116">*Volitelné:* změnit název řešení od *WebApp1* k *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="f1fc7-117">Visual Studio zobrazí název nového řešení (*Mvc5*), což usnadňuje říct tento projekt z projektu další.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="f1fc7-118">Vytvoření projektu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1fc7-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="f1fc7-119">Vytvořte novou *prázdný* ASP.NET Core webové aplikace se stejným názvem jako předchozí projekt (*WebApp1*), obory názvů v dva projekty shodovat.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="f1fc7-120">S o stejný obor názvů usnadňuje zkopírujte kód mezi dva projekty.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="f1fc7-121">Budete mít k vytvoření tohoto projektu v jiném adresáři než předchozí projekt, který používá stejný název.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Dialogové okno Nový projekt](mvc/_static/new_core.png)

![Dialogové okno nové webové aplikace ASP.NET: prázdná šablona projektu vybrané panelu ASP.NET Core šablony](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="f1fc7-124">*Volitelné:* vytvoření nové aplikace ASP.NET Core pomocí *webové aplikace* šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="f1fc7-125">Název projektu *WebApp1*a vyberte některou možnost ověřování z **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="f1fc7-126">Přejmenujte tuto aplikaci a *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="f1fc7-127">Vytvoření tohoto projektu šetří čas v převodu.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="f1fc7-128">Můžete si prohlédnout kód generovaný šablony najdete v části konečný výsledek nebo zkopírujte kód do projektu převod.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="f1fc7-129">Je také užitečné, pokud zablokuje v kroku převod k porovnání s projektem šablona vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="f1fc7-130">Konfigurace lokality k použití MVC</span><span class="sxs-lookup"><span data-stu-id="f1fc7-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="f1fc7-131">Pokud je cílem .NET Core, metapackage ASP.NET Core se přidá do projektu, názvem `Microsoft.AspNetCore.All` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="f1fc7-132">Tento balíček obsahuje balíčky jako `Microsoft.AspNetCore.Mvc` a `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="f1fc7-133">Pokud cílení na rozhraní .NET Framework, třeba jednotlivě uvedené v souboru \*.csproj balíček odkazuje.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="f1fc7-134">`Microsoft.AspNetCore.Mvc` je rozhraní ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="f1fc7-135">`Microsoft.AspNetCore.StaticFiles` je obslužná rutina statických souborů.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="f1fc7-136">Modul runtime ASP.NET Core je modulární a musí explicitně přihlášení poskytovat statické soubory (viz [statické soubory](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="f1fc7-137">Otevřete *Startup.cs* souboru a změnit kód tak, aby odpovídala následující:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="f1fc7-138">`UseStaticFiles` Metoda rozšíření přidá obslužné rutiny statických souborů.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="f1fc7-139">Jak je uvedeno nahoře, modulem runtime ASP.NET je modulární a musí explicitně přihlášení poskytovat statické soubory.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="f1fc7-140">`UseMvc` Přidá metody rozšíření směrování.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="f1fc7-141">Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup) a [směrování](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="f1fc7-142">Přidání kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="f1fc7-142">Add a controller and view</span></span>

<span data-ttu-id="f1fc7-143">V této části přidáte minimální řadiče a zobrazení, která bude sloužit jako zástupné symboly ASP.NET MVC jsou řadič MVC a zobrazení, že budete migrovat v další části.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="f1fc7-144">Přidat *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="f1fc7-145">Přidat **třídy Kontroleru** s názvem *HomeController.cs* k *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Přidat novou položku – dialogové okno](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="f1fc7-147">Přidat *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="f1fc7-148">Přidat *zobrazení Domů* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="f1fc7-149">Přidat **zobrazení syntaxe Razor** s názvem *Index.cshtml* k *zobrazení Domů* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Přidat novou položku – dialogové okno](mvc/_static/view.png)

<span data-ttu-id="f1fc7-151">Struktura projektu je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-151">The project structure is shown below:</span></span>

![Průzkumník řešení zobrazující soubory a složky WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="f1fc7-153">Nahraďte obsah *Views/Home/Index.cshtml* soubor s následující:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="f1fc7-154">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-154">Run the app.</span></span>

![Webovou aplikaci, otevřete v Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="f1fc7-156">V tématu [řadiče](xref:mvc/controllers/actions) a [zobrazení](xref:mvc/views/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="f1fc7-157">Teď, když máme minimální funkční projekt ASP.NET Core, můžeme začít migrace funkce z projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="f1fc7-158">Je potřeba přesunout následující:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-158">We need to move the following:</span></span>

* <span data-ttu-id="f1fc7-159">obsah na straně klienta (šablon stylů CSS, písma a skripty)</span><span class="sxs-lookup"><span data-stu-id="f1fc7-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="f1fc7-160">kontrolery</span><span class="sxs-lookup"><span data-stu-id="f1fc7-160">controllers</span></span>

* <span data-ttu-id="f1fc7-161">zobrazení</span><span class="sxs-lookup"><span data-stu-id="f1fc7-161">views</span></span>

* <span data-ttu-id="f1fc7-162">modely</span><span class="sxs-lookup"><span data-stu-id="f1fc7-162">models</span></span>

* <span data-ttu-id="f1fc7-163">Sdružování</span><span class="sxs-lookup"><span data-stu-id="f1fc7-163">bundling</span></span>

* <span data-ttu-id="f1fc7-164">filtry</span><span class="sxs-lookup"><span data-stu-id="f1fc7-164">filters</span></span>

* <span data-ttu-id="f1fc7-165">Přihlaste se vstup/výstup Identity (to se provádí v dalším kurzu).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="f1fc7-166">Kontrolery a zobrazení</span><span class="sxs-lookup"><span data-stu-id="f1fc7-166">Controllers and views</span></span>

* <span data-ttu-id="f1fc7-167">Zkopírujte všechny metody z rozhraní ASP.NET MVC `HomeController` do nového `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="f1fc7-168">Upozorňujeme, že v architektuře ASP.NET MVC předdefinované šablony řadiče akce metoda návratový typ je [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); v aplikaci ASP.NET MVC jádra, metody akce návratový `IActionResult` místo.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="f1fc7-169">`ActionResult` implementuje `IActionResult`, takže není nutné změnit návratový typ vaší metody akce.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="f1fc7-170">Kopírování *About.cshtml*, *Contact.cshtml*, a *Index.cshtml* Razor zobrazit soubory z projektu ASP.NET MVC do projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="f1fc7-171">Spuštění aplikace ASP.NET Core a testování jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="f1fc7-172">Jsme jste nemigrovali rozložení souboru nebo styly ještě, takže vykreslené zobrazení obsahovat pouze obsah souborů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="f1fc7-173">Nebudete mít rozložení souboru vygenerovaného odkazy `About` a `Contact` zobrazení, takže budete muset je vyvolat z prohlížeče (Nahraďte **4492** číslem portu, na které se používají ve vašem projektu).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kontaktujte stránky](mvc/_static/contact-page.png)

<span data-ttu-id="f1fc7-175">Poznámka: nedostatek položky stylů a nabídky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="f1fc7-176">To jsme budete opravíme v další části.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="f1fc7-177">Statický obsah</span><span class="sxs-lookup"><span data-stu-id="f1fc7-177">Static content</span></span>

<span data-ttu-id="f1fc7-178">V předchozích verzích rozhraní ASP.NET MVC statický obsah hostitelem byl z kořenového adresáře webového projektu a byl smíšeného s soubory na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="f1fc7-179">V ASP.NET Core, statický obsah uložený v *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="f1fc7-180">Budete chtít zkopírujte statický obsah z původního aplikaci ASP.NET MVC *wwwroot* složku ve vašem projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="f1fc7-181">V této ukázkové převod:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-181">In this sample conversion:</span></span>

* <span data-ttu-id="f1fc7-182">Kopírování *favicon.ico* soubor z původní projekt MVC *wwwroot* složky v projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="f1fc7-183">Starý ASP.NET MVC projektu používá [Bootstrap](https://getbootstrap.com/) pro jeho stylů a úložišť, službou Bootstrap nástroje soubory *obsahu* a *skripty* složek.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="f1fc7-184">Šablony, která generuje staré projektu ASP.NET MVC, odkazuje na Bootstrap v rozložení souboru (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="f1fc7-185">Může zkopírovat *bootstrap.js* a *bootstrap.css* soubory z rozhraní ASP.NET MVC projektu do *wwwroot* složky v novém projektu.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="f1fc7-186">Místo toho přidáme podporu Bootstrap (a další klientské knihovny) pomocí sítím CDN v další části.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="f1fc7-187">Migrace na soubor rozložení</span><span class="sxs-lookup"><span data-stu-id="f1fc7-187">Migrate the layout file</span></span>

* <span data-ttu-id="f1fc7-188">Kopírování *soubor _ViewStart.cshtml* soubor z původního projektu ASP.NET MVC *zobrazení* složky do projektů ASP.NET Core *zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="f1fc7-189">*Soubor _ViewStart.cshtml* soubor nebyl změněn v aplikaci ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="f1fc7-190">Vytvoření *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="f1fc7-191">*Volitelné:* kopie *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *zobrazení* složky do projektů ASP.NET Core  *Zobrazení* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="f1fc7-192">Odeberte všechny deklarace oboru názvů v *_ViewImports.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f1fc7-193">*_ViewImports.cshtml* soubor poskytuje obory názvů pro všechny soubory, zobrazení a přináší [značky Pomocníci](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="f1fc7-194">Pomocníci značky se používají v nové rozložení souboru.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="f1fc7-195">*_ViewImports.cshtml* souboru je nového pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="f1fc7-196">Kopírování *_Layout.cshtml* soubor z původního projektu ASP.NET MVC *zobrazení a sdílených* složky do projektů ASP.NET Core *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="f1fc7-197">Otevřete *_Layout.cshtml* souboru a proveďte následující změny (dokončený kód je zobrazené dole):</span><span class="sxs-lookup"><span data-stu-id="f1fc7-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="f1fc7-198">Nahraďte `@Styles.Render("~/Content/css")` s `<link>` elementu, který chcete načíst *bootstrap.css* (viz níže).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="f1fc7-199">Odebrat `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="f1fc7-200">Komentář `@Html.Partial("_LoginPartial")` řádku (obklopit řádek s `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="f1fc7-201">Vrátí k němu v budoucnu kurzu.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="f1fc7-202">Nahraďte `@Scripts.Render("~/bundles/jquery")` s `<script>` – element (viz níže).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="f1fc7-203">Nahraďte `@Scripts.Render("~/bundles/bootstrap")` s `<script>` – element (viz níže).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="f1fc7-204">Nahrazení kód pro zahrnutí Bootstrap CSS:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="f1fc7-205">Nahrazení značky jQuery a Bootstrap JavaScript zahrnutí:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="f1fc7-206">Aktualizovaný *_Layout.cshtml* souboru jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="f1fc7-207">Zobrazte webu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-207">View the site in the browser.</span></span> <span data-ttu-id="f1fc7-208">Nyní by se měly správně načíst s očekávanou styly na místě.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="f1fc7-209">*Volitelné:* můžete chtít zkuste použít nový soubor rozložení.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="f1fc7-210">Pro tento projekt můžete zkopírovat soubor rozložení z *FullAspNetCore* projektu.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="f1fc7-211">Nový soubor rozložení používá [značky Pomocníci](xref:mvc/views/tag-helpers/intro) a má další vylepšení.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="f1fc7-212">Konfigurace sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="f1fc7-212">Configure bundling and minification</span></span>

<span data-ttu-id="f1fc7-213">Informace o tom, jak nakonfigurovat sdružování a minimalizace najdete v tématu [sdružování a Minifikace](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="f1fc7-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="f1fc7-214">Řešení chyb HTTP 500</span><span class="sxs-lookup"><span data-stu-id="f1fc7-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="f1fc7-215">Existuje mnoho problémů, které můžou způsobit chybová zpráva HTTP 500 které neobsahují žádné informace na zdroj problému.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="f1fc7-216">Například pokud *Views/_ViewImports.cshtml* soubor obsahuje obor názvů, který neexistuje v projektu, získáte chyby HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="f1fc7-217">Ve výchozím nastavení v aplikacích ASP.NET Core `UseDeveloperExceptionPage` rozšíření je přidán do `IApplicationBuilder` a provést, když je konfigurace *vývoj*.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="f1fc7-218">To je podrobně popsaná v následující kód:</span><span class="sxs-lookup"><span data-stu-id="f1fc7-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="f1fc7-219">ASP.NET Core převede neošetřených výjimek ve webové aplikaci do chybové odpovědi HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="f1fc7-220">Za normálních okolností podrobnosti o chybě nejsou součástí těchto odpovědí, aby se zabránilo úniku potenciálně citlivých informací o serveru.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="f1fc7-221">V tématu **pomocí stránky výjimka vývojáře** v [zpracovávat chyby](../fundamentals/error-handling.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f1fc7-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1fc7-222">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f1fc7-222">Additional resources</span></span>

* [<span data-ttu-id="f1fc7-223">Vývoj klientské strany</span><span class="sxs-lookup"><span data-stu-id="f1fc7-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="f1fc7-224">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="f1fc7-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
