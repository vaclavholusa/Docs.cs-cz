---
title: "Úvod do stránky Razor v ASP.NET Core"
author: Rick-Anderson
description: "Zjistěte, jak stránky Razor v ASP.NET Core Díky kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu než použití MVC."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: f24de7ab12a3bbd7915ce6c3c93a107eb47fe864
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="e7780-103">Úvod do stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7780-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e7780-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="e7780-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="e7780-105">Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.</span><span class="sxs-lookup"><span data-stu-id="e7780-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="e7780-106">Pokud hledáte kurz, který používá Model-View-Controller přístup, najdete v části [Začínáme s ASP.NET MVC základní](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="e7780-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="e7780-107">Tento dokument obsahuje úvod do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="e7780-108">Není návod krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="e7780-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="e7780-109">Pokud zjistíte oddíly je obtížné postupujte podle, najdete v části [Začínáme s stránky Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="e7780-109">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="e7780-110">Jádro ASP.NET 2.0 požadavky</span><span class="sxs-lookup"><span data-stu-id="e7780-110">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="e7780-111">Nainstalujte [.NET Core](https://www.microsoft.com/net/core) 2.0.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e7780-111">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="e7780-112">Pokud používáte Visual Studio, nainstalujte [Visual Studio](https://www.visualstudio.com/vs/) 2017 verze 15.3 nebo novější s následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="e7780-112">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="e7780-113">**ASP.NET a vývoje**</span><span class="sxs-lookup"><span data-stu-id="e7780-113">**ASP.NET and web development**</span></span>
* <span data-ttu-id="e7780-114">**Vývoj pro různé platformy .NET core**</span><span class="sxs-lookup"><span data-stu-id="e7780-114">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="e7780-115">Vytvoření projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e7780-115">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7780-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7780-116">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e7780-117">V tématu [Začínáme s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) podrobné pokyny o tom, jak vytvářet stránky Razor projektu pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7780-117">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7780-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e7780-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e7780-119">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e7780-119">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="e7780-120">Otevřete vygenerovaného *.csproj* soubor ze sady Visual Studio for Mac.</span><span class="sxs-lookup"><span data-stu-id="e7780-120">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7780-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7780-121">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="e7780-122">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e7780-122">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e7780-123">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e7780-123">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e7780-124">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e7780-124">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="e7780-125">Stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e7780-125">Razor Pages</span></span>

<span data-ttu-id="e7780-126">Stránky Razor je povolena v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e7780-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="e7780-127">Mějte na paměti na základní stránce: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="e7780-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="e7780-128">Předchozí kód mnoho vypadá soubor zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="e7780-129">Čím je jiné je `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e7780-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="e7780-130">`@page` Vytvoří soubor do akce MVC – to znamená, že ji zpracovává požadavky přímo, bez průchodu přes řadič.</span><span class="sxs-lookup"><span data-stu-id="e7780-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="e7780-131">`@page` musí být první direktivu Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="e7780-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="e7780-132">`@page` má vliv na chování jiných objektů, které Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="e7780-133">Podobná stránky, můžete použít `PageModel` třídy, je uvedené v následující dva soubory.</span><span class="sxs-lookup"><span data-stu-id="e7780-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="e7780-134">*Pages/Index2.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="e7780-135">*Pages/Index2.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="e7780-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="e7780-136">Podle konvence `PageModel` soubor třídy má stejný název jako soubor stránky Razor s *.cs* připojí.</span><span class="sxs-lookup"><span data-stu-id="e7780-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="e7780-137">Je třeba na předchozí stránku Razor *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e7780-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="e7780-138">Soubor obsahující `PageModel` třída nazývá *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7780-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="e7780-139">Přidružení cesty adresy URL na stránky určuje umístění stránky v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="e7780-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="e7780-140">Následující tabulka uvádí stránky Razor cestu a odpovídající adresy URL:</span><span class="sxs-lookup"><span data-stu-id="e7780-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="e7780-141">Název souboru a cesty</span><span class="sxs-lookup"><span data-stu-id="e7780-141">File name and path</span></span>               | <span data-ttu-id="e7780-142">odpovídající adresy URL</span><span class="sxs-lookup"><span data-stu-id="e7780-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e7780-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="e7780-144">`/` Nebo `/Index`</span><span class="sxs-lookup"><span data-stu-id="e7780-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="e7780-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="e7780-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="e7780-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="e7780-148">`/Store` Nebo `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="e7780-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="e7780-149">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="e7780-149">Notes:</span></span>

* <span data-ttu-id="e7780-150">Modul runtime vypadá pro stránky Razor soubory v *stránky* složky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e7780-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="e7780-151">`Index` Když neobsahuje adresu URL stránky, je výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="e7780-152">Zápis základní formulář</span><span class="sxs-lookup"><span data-stu-id="e7780-152">Writing a basic form</span></span>

<span data-ttu-id="e7780-153">Funkce stránky Razor jsou navrženy tak, aby se používají u webových prohlížečů snadno běžných vzorů.</span><span class="sxs-lookup"><span data-stu-id="e7780-153">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="e7780-154">[Model vazby](xref:mvc/models/model-binding), [značky Pomocníci](xref:mvc/views/tag-helpers/intro)a všechny pomocné rutiny HTML *právě pracovní* s vlastnostmi definovaný ve třídě stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="e7780-155">Vezměte v úvahu stránky, který implementuje "kontaktujte nás" formuláři pro základní `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="e7780-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="e7780-156">Pro ukázky v tomto dokumentu `DbContext` je inicializován v [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) souboru.</span><span class="sxs-lookup"><span data-stu-id="e7780-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="e7780-157">Datový model:</span><span class="sxs-lookup"><span data-stu-id="e7780-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="e7780-158">Kontext databáze:</span><span class="sxs-lookup"><span data-stu-id="e7780-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="e7780-159">*Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="e7780-160">*Pages/Create.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="e7780-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="e7780-161">Podle konvence `PageModel` třídy se nazývá `<PageName>Model` a je v stejného oboru názvů jako stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="e7780-162">`PageModel` Třída umožňuje oddělení logiku stránky od jeho prezentaci.</span><span class="sxs-lookup"><span data-stu-id="e7780-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="e7780-163">Definuje stránky obslužné rutiny pro požadavky odeslané na stránku a data použitá k vykreslení stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="e7780-164">Toto oddělení vám umožní spravovat stránky závislosti prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection) a [testování částí](xref:testing/razor-pages-testing) stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="e7780-165">Na stránce `OnPostAsync` *metoda obslužná rutina*, která se spouští `POST` požadavků (když uživatel odešle formulář).</span><span class="sxs-lookup"><span data-stu-id="e7780-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="e7780-166">Můžete přidat metody obslužné rutiny pro všechny akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7780-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="e7780-167">Nejběžnější obslužné rutiny jsou:</span><span class="sxs-lookup"><span data-stu-id="e7780-167">The most common handlers are:</span></span>

* <span data-ttu-id="e7780-168">`OnGet` k chybě při inicializaci stavu potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="e7780-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="e7780-169">[OnGet](#OnGet) ukázka.</span><span class="sxs-lookup"><span data-stu-id="e7780-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="e7780-170">`OnPost` pro zpracování odesílání formuláře.</span><span class="sxs-lookup"><span data-stu-id="e7780-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="e7780-171">`Async` Přípona je nepovinný, ale se často používá podle konvence pro asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="e7780-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="e7780-172">`OnPostAsync` Kód v předchozím příkladu bude vypadat podobně jako by za normálních okolností psaní v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e7780-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="e7780-173">Předchozí kód je typické pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="e7780-174">Většina primitiv MVC jako [model vazby](xref:mvc/models/model-binding), [ověření](xref:mvc/models/validation), a jsou sdíleny výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="e7780-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="e7780-175">Předchozí `OnPostAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="e7780-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="e7780-176">Základní tok `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="e7780-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="e7780-177">Zkontrolujte chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="e7780-177">Check for validation errors.</span></span>

*  <span data-ttu-id="e7780-178">Pokud nejsou žádné chyby, uložit data a přesměrování.</span><span class="sxs-lookup"><span data-stu-id="e7780-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="e7780-179">Pokud nejsou chyby, zobrazit stránku znovu s ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="e7780-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="e7780-180">Ověřování na straně klienta je stejný jako tradiční aplikací ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="e7780-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="e7780-181">V mnoha případech chyb při ověřování by být zjištěn v klientském počítači a nikdy odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="e7780-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="e7780-182">Při úspěšném zadání data `OnPostAsync` volání metod obslužné rutiny `RedirectToPage` Pomocná metoda vrátí instanci `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="e7780-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="e7780-183">`RedirectToPage` představuje novou výsledek akce, podobně jako `RedirectToAction` nebo `RedirectToRoute`, ale přizpůsobené pro stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="e7780-184">V předchozím příkladu, je přesměrován na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="e7780-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="e7780-185">`RedirectToPage` podrobně [generování adresy URL pro stránky](#url_gen) části.</span><span class="sxs-lookup"><span data-stu-id="e7780-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="e7780-186">Když odeslaného formuláře došlo k chybám ověřování, (které jsou předávány serveru),`OnPostAsync` volání metod obslužné rutiny `Page` metodu helper.</span><span class="sxs-lookup"><span data-stu-id="e7780-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="e7780-187">`Page` Vrací instanci třídy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="e7780-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="e7780-188">Vrácení `Page` je podobná jak akce v řadiče vrátit `View`.</span><span class="sxs-lookup"><span data-stu-id="e7780-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="e7780-189">`PageResult` Výchozí nastavení je <!-- Review  --> návratový typ pro obslužné rutiny metodu.</span><span class="sxs-lookup"><span data-stu-id="e7780-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="e7780-190">Obslužná rutina metodu, která vrátí `void` vykreslí stránku.</span><span class="sxs-lookup"><span data-stu-id="e7780-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="e7780-191">`Customer` Používá vlastnost `[BindProperty]` atribut se vyjádřit výslovný souhlas s vazbou modelu.</span><span class="sxs-lookup"><span data-stu-id="e7780-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="e7780-192">Stránky Razor, ve výchozím nastavení, vazbu vlastnosti jenom s příkazy nebo GET.</span><span class="sxs-lookup"><span data-stu-id="e7780-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="e7780-193">Vazba na vlastnosti může snížit množství kód, který máte k zápisu.</span><span class="sxs-lookup"><span data-stu-id="e7780-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="e7780-194">Vazba snižuje kódu pomocí stejné vlastnosti k vykreslení pole formuláře (`<input asp-for="Customer.Name" />`) a přijměte vstupu.</span><span class="sxs-lookup"><span data-stu-id="e7780-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="e7780-195">Domovská stránka (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="e7780-195">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="e7780-196">Kódu *Index.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-196">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="e7780-197">*Index.cshtml* soubor obsahuje následující kód k vytvoření odkazu pro úpravy pro každý kontakt:</span><span class="sxs-lookup"><span data-stu-id="e7780-197">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="e7780-198">[Pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) použít `asp-route-{value}` atribut pro vygenerování odkazu na stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="e7780-198">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="e7780-199">Odkaz obsahuje data trasy s kontakt ID.</span><span class="sxs-lookup"><span data-stu-id="e7780-199">The link contains route data with the contact ID.</span></span> <span data-ttu-id="e7780-200">Například `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="e7780-200">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="e7780-201">*Pages/Edit.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="e7780-202">První řádek obsahuje `@page "{id:int}"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e7780-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="e7780-203">Omezení směrování`"{id:int}"` informuje stránky tak, aby přijímal požadavky na stránku, které obsahují `int` směrovat data.</span><span class="sxs-lookup"><span data-stu-id="e7780-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="e7780-204">Pokud požadavek na stránku neobsahuje data trasy, která lze převést na `int`, modul runtime vrátí chybu HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="e7780-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="e7780-205">*Pages/Edit.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-205">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="e7780-206">*Index.cshtml* soubor zároveň obsahuje kód k vytvoření tlačítko Odstranit pro každý kontakt zákazníka:</span><span class="sxs-lookup"><span data-stu-id="e7780-206">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="e7780-207">Pokud tlačítko Odstranit je vykreslena ve formátu HTML, jeho `formaction` zahrnuje parametry:</span><span class="sxs-lookup"><span data-stu-id="e7780-207">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="e7780-208">Obraťte se na zákaznické ID určeného `asp-route-id` atribut.</span><span class="sxs-lookup"><span data-stu-id="e7780-208">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="e7780-209">`handler` Určeným elementem `asp-page-handler` atribut.</span><span class="sxs-lookup"><span data-stu-id="e7780-209">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="e7780-210">Tady je příklad vykreslené odstranění tlačítka s zákazník obraťte se na ID `1`:</span><span class="sxs-lookup"><span data-stu-id="e7780-210">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="e7780-211">Při výběru tlačítka, formuláře `POST` požadavek odeslán do serveru.</span><span class="sxs-lookup"><span data-stu-id="e7780-211">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="e7780-212">Podle konvence je vybraný název metody obslužné rutiny na základě hodnotu `handler` parametr podle schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="e7780-212">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="e7780-213">Protože `handler` je `delete` v tomto příkladu `OnPostDeleteAsync` metoda obslužná rutina se používá procesu `POST` požadavku.</span><span class="sxs-lookup"><span data-stu-id="e7780-213">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="e7780-214">Pokud `asp-page-handler` nastavena na jinou hodnotu, jako například `remove`, metodu obslužná rutina stránky s názvem `OnPostRemoveAsync` je vybrána.</span><span class="sxs-lookup"><span data-stu-id="e7780-214">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="e7780-215">`OnPostDeleteAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="e7780-215">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="e7780-216">Přijme `id` z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e7780-216">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="e7780-217">Dotazuje databázi pro zákazníka kontaktu s `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="e7780-217">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="e7780-218">Pokud je nalezen kontaktování zákazníků, budou se odebrat ze seznamu kontaktů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e7780-218">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="e7780-219">Databáze se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="e7780-219">The database is updated.</span></span>
* <span data-ttu-id="e7780-220">Volání `RedirectToPage` přesměrovat na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="e7780-220">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="e7780-221">XSRF/proti útokům CSRF a stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e7780-221">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="e7780-222">Nemáte psaní jakéhokoli kódu pro [antiforgery ověření](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="e7780-222">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="e7780-223">Antiforgery generování tokenů a ověření jsou automaticky součástí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-223">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="e7780-224">Pomocí stránky Razor rozložení, částečné., šablony a pomocníky značky</span><span class="sxs-lookup"><span data-stu-id="e7780-224">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="e7780-225">Stránky fungovat se všemi funkcemi zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-225">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="e7780-226">Rozložení, šablony, a částečné značka Pomocníci *soubor _ViewStart.cshtml*, *_ViewImports.cshtml* pracovní stejným způsobem, tak pro běžné zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-226">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="e7780-227">Tato stránka umožňuje declutter díky některé z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="e7780-227">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="e7780-228">Přidat [rozložení stránky](xref:mvc/views/layout) k *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7780-228">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="e7780-229">[Rozložení](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="e7780-229">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="e7780-230">Určuje rozložení každé stránce (Pokud stránce výslovný nesouhlas rozložení).</span><span class="sxs-lookup"><span data-stu-id="e7780-230">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="e7780-231">Importuje struktury HTML jako je JavaScript a šablon.</span><span class="sxs-lookup"><span data-stu-id="e7780-231">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="e7780-232">V tématu [rozložení stránky](xref:mvc/views/layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7780-232">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="e7780-233">[Rozložení](xref:mvc/views/layout#specifying-a-layout) je nastavena *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7780-233">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="e7780-234">**Poznámka:** rozložení je v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e7780-234">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="e7780-235">Stránky vyhledejte další zobrazení (rozložení, šablony, částečné.) hierarchicky, spouštění ve stejné složce jako aktuální stránku.</span><span class="sxs-lookup"><span data-stu-id="e7780-235">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="e7780-236">Rozložení v *stránky* složky lze z libovolné stránky Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e7780-236">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="e7780-237">Doporučujeme **není** chápat rozložení souboru *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="e7780-237">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="e7780-238">*Zobrazení a sdílených* je zobrazení vzor MVC.</span><span class="sxs-lookup"><span data-stu-id="e7780-238">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="e7780-239">Stránky Razor jsou určené spoléhají na hierarchii složek, není cesta konvence.</span><span class="sxs-lookup"><span data-stu-id="e7780-239">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="e7780-240">Obsahuje zobrazení vyhledávání ze stránky Razor *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e7780-240">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="e7780-241">Rozložení, šablony a částečné používáte s řadiče MVC a konvenční zobrazení syntaxe Razor *právě pracovní*.</span><span class="sxs-lookup"><span data-stu-id="e7780-241">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="e7780-242">Přidat *Pages/_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-242">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="e7780-243">`@namespace` se vysvětluje dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e7780-243">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="e7780-244">`@addTagHelper` – Direktiva přináší [předdefinované značky Pomocníci](xref:mvc/views/tag-helpers/builtin-th/Index) pro všechny stránky v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e7780-244">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="e7780-245">Když `@namespace` – direktiva se používá explicitně na stránce:</span><span class="sxs-lookup"><span data-stu-id="e7780-245">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="e7780-246">Direktiva Nastaví obor názvů pro stránku.</span><span class="sxs-lookup"><span data-stu-id="e7780-246">The directive sets the namespace for the page.</span></span> <span data-ttu-id="e7780-247">`@model` – Direktiva nemusí obsahovat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="e7780-247">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="e7780-248">Když `@namespace` – direktiva je součástí *_ViewImports.cshtml*, zadaný obor názvů poskytuje předponu pro obor názvů generovaným na stránce, který umožňuje importovat `@namespace` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e7780-248">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="e7780-249">Zbytek vygenerovaného oboru názvů (část příponu) je relativní cesta mezi složku, která obsahuje oddělené tečkou *_ViewImports.cshtml* a složce obsahující danou stránku.</span><span class="sxs-lookup"><span data-stu-id="e7780-249">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="e7780-250">Například kódu na pozadí *Pages/Customers/Edit.cshtml.cs* explicitně nastaví obor názvů:</span><span class="sxs-lookup"><span data-stu-id="e7780-250">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="e7780-251">*Pages/_ViewImports.cshtml* souboru nastaví následující obor názvů:</span><span class="sxs-lookup"><span data-stu-id="e7780-251">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="e7780-252">Vygenerovaný obor názvů pro *Pages/Customers/Edit.cshtml* Razor stránky je stejná jako souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="e7780-252">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="e7780-253">`@namespace` – Direktiva v byla navržená tak, třídy C# přidán do projektu a kód generovaný stránky *právě pracovní* bez nutnosti přidání `@using` direktivy pro souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="e7780-253">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="e7780-254">**Poznámka:** `@namespace` funguje taky s konvenční zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="e7780-254">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="e7780-255">Původní *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-255">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="e7780-256">Aktualizovaný *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="e7780-256">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="e7780-257">[Projektu úvodní stránky Razor](#rpvs17) obsahuje *Pages/_ValidationScriptsPartial.cshtml*, který zachytí až ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e7780-257">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="e7780-258">Generování adresy URL pro stránky</span><span class="sxs-lookup"><span data-stu-id="e7780-258">URL generation for Pages</span></span>

<span data-ttu-id="e7780-259">`Create` Stránky, která je uvedená dříve, používá `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="e7780-259">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="e7780-260">Aplikace má následující strukturu souboru nebo složky:</span><span class="sxs-lookup"><span data-stu-id="e7780-260">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="e7780-261">*/ Stránky*</span><span class="sxs-lookup"><span data-stu-id="e7780-261">*/Pages*</span></span>

  * <span data-ttu-id="e7780-262">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-262">*Index.cshtml*</span></span>
  * <span data-ttu-id="e7780-263">*Nebo smlouvy zákazníka*</span><span class="sxs-lookup"><span data-stu-id="e7780-263">*/Customer*</span></span>

    * <span data-ttu-id="e7780-264">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-264">*Create.cshtml*</span></span>
    * <span data-ttu-id="e7780-265">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-265">*Edit.cshtml*</span></span>
    * <span data-ttu-id="e7780-266">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e7780-266">*Index.cshtml*</span></span>

<span data-ttu-id="e7780-267">*Pages/Customers/Create.cshtml* a *Pages/Customers/Edit.cshtml* stránky přesměrovat na *Pages/Index.cshtml* po úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e7780-267">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="e7780-268">Řetězec `/Index` je součástí identifikátor URI k přístupu na předchozí stránku.</span><span class="sxs-lookup"><span data-stu-id="e7780-268">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="e7780-269">Řetězec `/Index` lze použít ke generování identifikátory URI k *Pages/Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-269">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="e7780-270">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e7780-270">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="e7780-271">Název stránky je cesta na stránku z kořenového adresáře */stránky* složky (včetně jako úvodní `/`, například `/Index`).</span><span class="sxs-lookup"><span data-stu-id="e7780-271">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="e7780-272">Předchozí ukázky generování adresy URL jsou mnohem víc bohaté funkce než jenom hardcoding adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e7780-272">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="e7780-273">Generování adresy URL používá [směrování](xref:mvc/controllers/routing) a vygenerovat a kódování parametry podle jak trasy, která je definována v cílovou cestu.</span><span class="sxs-lookup"><span data-stu-id="e7780-273">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="e7780-274">Generování adresy URL pro stránky podporuje relativních názvů.</span><span class="sxs-lookup"><span data-stu-id="e7780-274">URL generation for pages supports relative names.</span></span> <span data-ttu-id="e7780-275">Následující tabulka uvádí, které Index vybrány jiné `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7780-275">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="e7780-276">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="e7780-276">RedirectToPage(x)</span></span>| <span data-ttu-id="e7780-277">Stránka</span><span class="sxs-lookup"><span data-stu-id="e7780-277">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e7780-278">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="e7780-278">RedirectToPage("/Index")</span></span> | <span data-ttu-id="e7780-279">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="e7780-279">*Pages/Index*</span></span> |
| <span data-ttu-id="e7780-280">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="e7780-280">RedirectToPage("./Index");</span></span> | <span data-ttu-id="e7780-281">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="e7780-281">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="e7780-282">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="e7780-282">RedirectToPage("../Index")</span></span> | <span data-ttu-id="e7780-283">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="e7780-283">*Pages/Index*</span></span> |
| <span data-ttu-id="e7780-284">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="e7780-284">RedirectToPage("Index")</span></span>  | <span data-ttu-id="e7780-285">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="e7780-285">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="e7780-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, a `RedirectToPage("../Index")` jsou *relativních názvů*.</span><span class="sxs-lookup"><span data-stu-id="e7780-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="e7780-287">`RedirectToPage` Parametr *kombinaci* cestou k aktuální stránce k výpočtu název cílové stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-287">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="e7780-288">Relativní název propojení je užitečné, při vytváření lokalit se strukturou komplexní.</span><span class="sxs-lookup"><span data-stu-id="e7780-288">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="e7780-289">Pokud používáte relativních názvů propojení mezi stránkami ve složce, můžete přejmenovat této složky.</span><span class="sxs-lookup"><span data-stu-id="e7780-289">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="e7780-290">Všechny odkazy na i nadále fungovat, (protože jejich nezahrnuli název složky).</span><span class="sxs-lookup"><span data-stu-id="e7780-290">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="e7780-291">TempData</span><span class="sxs-lookup"><span data-stu-id="e7780-291">TempData</span></span>

<span data-ttu-id="e7780-292">ASP.NET Core zpřístupní [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnost [řadič](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="e7780-292">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="e7780-293">Tato vlastnost ukládá data, dokud je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e7780-293">This property stores data until it's read.</span></span> <span data-ttu-id="e7780-294">`Keep` a `Peek` metody můžete použít k prozkoumání dat bez odstranění.</span><span class="sxs-lookup"><span data-stu-id="e7780-294">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="e7780-295">`TempData` jsou užitečné pro přesměrování, pokud se data potřebná pro více než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="e7780-295">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="e7780-296">`[TempData]` Atribut je nového v technologii ASP.NET 2.0 jádra a je podporovaná v řadičích a stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-296">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="e7780-297">Následující kód nastaví hodnotu `Message` pomocí `TempData`:</span><span class="sxs-lookup"><span data-stu-id="e7780-297">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="e7780-298">Následující kód v *Pages/Customers/Index.cshtml* souboru se zobrazí hodnota `Message` pomocí `TempData`.</span><span class="sxs-lookup"><span data-stu-id="e7780-298">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="e7780-299">*Pages/Customers/Index.cshtml.cs* modelu stránka se vztahuje `[TempData]` atribut `Message` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e7780-299">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="e7780-300">V tématu [TempData](xref:fundamentals/app-state#temp) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7780-300">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="e7780-301">Více obslužné rutiny na stránce</span><span class="sxs-lookup"><span data-stu-id="e7780-301">Multiple handlers per page</span></span>

<span data-ttu-id="e7780-302">Na následující stránce generuje kód pro dvě stránky pomocí obslužné rutiny `asp-page-handler` pomocná značky:</span><span class="sxs-lookup"><span data-stu-id="e7780-302">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="e7780-303">Formuláře v předchozím příkladu má dva odeslání tlačítka, každý využívající `FormActionTagHelper` odeslat na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e7780-303">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="e7780-304">`asp-page-handler` Atribut je Pomocníka pro `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="e7780-304">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="e7780-305">`asp-page-handler` generuje adresy URL, které odesílají do jednotlivých metod obslužné rutiny, které jsou definované na stránce.</span><span class="sxs-lookup"><span data-stu-id="e7780-305">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="e7780-306">`asp-page` není zadán, protože je ukázka propojení na aktuální stránce.</span><span class="sxs-lookup"><span data-stu-id="e7780-306">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="e7780-307">Model stránky:</span><span class="sxs-lookup"><span data-stu-id="e7780-307">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="e7780-308">Předchozí kód používá *s názvem metody obslužná rutina*.</span><span class="sxs-lookup"><span data-stu-id="e7780-308">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="e7780-309">Metody s názvem obslužné rutiny vytvářejí provedením text v názvu po `On<HTTP Verb>` a před `Async` (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="e7780-309">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="e7780-310">V předchozím příkladu jsou metody stránky OnPost**JoinList**Async a OnPost**JoinListUC**asynchronní.</span><span class="sxs-lookup"><span data-stu-id="e7780-310">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="e7780-311">S *OnPost* a *asynchronní* odebrat, jsou názvy obslužná rutina `JoinList` a `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="e7780-311">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="e7780-312">Použití předcházející code cesty URL, kterou odesílá `OnPostJoinListAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="e7780-312">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="e7780-313">Cesty URL, kterou odesílá `OnPostJoinListUCAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="e7780-313">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="e7780-314">Přizpůsobení směrování</span><span class="sxs-lookup"><span data-stu-id="e7780-314">Customizing Routing</span></span>

<span data-ttu-id="e7780-315">Pokud chcete řetězec dotazu `?handler=JoinList` v adrese URL, můžete změnit trasy, která má název obslužné rutiny chápat část adresy obsahující cestu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e7780-315">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="e7780-316">Trasy, která můžete přizpůsobit přidáním šablonu trasy v dvojitých uvozovkách po `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e7780-316">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="e7780-317">Předchozí trasy vloží název obslužné rutiny cesty URL místo řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="e7780-317">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="e7780-318">`?` Následující `handler` znamená je volitelný parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="e7780-318">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="e7780-319">Můžete použít `@page` a přidejte další segmenty a parametry k postupu na stránce.</span><span class="sxs-lookup"><span data-stu-id="e7780-319">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="e7780-320">Ať je k dispozici má **připojí** trasu výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-320">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="e7780-321">Použití absolutní nebo virtuální cesta ke změně stránky trasy (například `"~/Some/Other/Path"`) není podporován.</span><span class="sxs-lookup"><span data-stu-id="e7780-321">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="e7780-322">Konfigurace a nastavení</span><span class="sxs-lookup"><span data-stu-id="e7780-322">Configuration and settings</span></span>

<span data-ttu-id="e7780-323">Nakonfigurujte rozšířené možnosti, pomocí metody rozšíření `AddRazorPagesOptions` na tvůrce MVC:</span><span class="sxs-lookup"><span data-stu-id="e7780-323">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="e7780-324">Teď můžete použít `RazorPagesOptions` nastavit kořenový adresář pro stránky, nebo přidejte aplikace model konvence pro stránky.</span><span class="sxs-lookup"><span data-stu-id="e7780-324">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="e7780-325">Jsme budete povolit další rozšíření tímto způsobem v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="e7780-325">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="e7780-326">Předkompilovat zobrazení, najdete v části [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="e7780-326">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="e7780-327">[Stažení nebo zobrazení ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="e7780-327">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="e7780-328">V tématu [Začínáme s stránky Razor v ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), který je založený na tento úvod.</span><span class="sxs-lookup"><span data-stu-id="e7780-328">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="e7780-329">Zadejte, zda stránky Razor se v kořenovém adresáři obsahu</span><span class="sxs-lookup"><span data-stu-id="e7780-329">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="e7780-330">Ve výchozím nastavení jsou stránky Razor root v */stránky* adresáře.</span><span class="sxs-lookup"><span data-stu-id="e7780-330">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="e7780-331">Přidat [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se v kořenovém adresáři obsahu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikace:</span><span class="sxs-lookup"><span data-stu-id="e7780-331">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="e7780-332">Určit, že stránky Razor na vlastní kořenový adresář</span><span class="sxs-lookup"><span data-stu-id="e7780-332">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="e7780-333">Přidat [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se vlastní kořenového adresáře v aplikaci (zadejte relativní cestu):</span><span class="sxs-lookup"><span data-stu-id="e7780-333">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="e7780-334">Viz také</span><span class="sxs-lookup"><span data-stu-id="e7780-334">See also</span></span>

* [<span data-ttu-id="e7780-335">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="e7780-335">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="e7780-336">Konvence autorizace stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e7780-336">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="e7780-337">Syntaxe Razor stránky vlastní trasy a stránka zprostředkovatele modelu</span><span class="sxs-lookup"><span data-stu-id="e7780-337">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="e7780-338">Jednotka stránky Razor a testování integrace</span><span class="sxs-lookup"><span data-stu-id="e7780-338">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
