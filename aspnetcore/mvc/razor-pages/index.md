---
title: Úvod do stránky Razor v ASP.NET Core
author: Rick-Anderson
description: Zjistěte, jak stránky Razor v ASP.NET Core Díky kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu než použití MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 5e2b53a4771a97b0a4091f593720b9c0e4e345bf
ms.sourcegitcommit: c4a31aaf902f2e84aaf4a9d882ca980fdf6488c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/23/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="e6162-103">Úvod do stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6162-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e6162-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="e6162-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="e6162-105">Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.</span><span class="sxs-lookup"><span data-stu-id="e6162-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="e6162-106">Pokud hledáte kurz, který používá Model-View-Controller přístup, najdete v části [Začínáme s ASP.NET MVC základní](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="e6162-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="e6162-107">Tento dokument obsahuje úvod do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="e6162-108">Není návod krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="e6162-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="e6162-109">Pokud některá z částí příliš advanced najdete v tématu [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="e6162-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="e6162-110">Přehled ASP.NET Core, najdete [Úvod do ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="e6162-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6162-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6162-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="e6162-112">Vytvoření projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6162-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6162-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e6162-114">V tématu [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) podrobné pokyny o tom, jak vytvářet stránky Razor projektu pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6162-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e6162-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e6162-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e6162-116">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6162-116">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="e6162-117">Otevřete vygenerovaného *.csproj* soubor ze sady Visual Studio for Mac.</span><span class="sxs-lookup"><span data-stu-id="e6162-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e6162-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e6162-118">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="e6162-119">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6162-119">Run `dotnet new razor` from the command line.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6162-120">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6162-120">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e6162-121">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6162-121">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="e6162-122">Stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-122">Razor Pages</span></span>

<span data-ttu-id="e6162-123">Stránky Razor je povolena v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6162-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="e6162-124">Mějte na paměti na základní stránce: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="e6162-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="e6162-125">Předchozí kód mnoho vypadá soubor zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="e6162-126">Čím je jiné je `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e6162-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="e6162-127">`@page` Vytvoří soubor do akce MVC – to znamená, že ji zpracovává požadavky přímo, bez průchodu přes řadič.</span><span class="sxs-lookup"><span data-stu-id="e6162-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="e6162-128">`@page` musí být první direktivu Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="e6162-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="e6162-129">`@page` má vliv na chování jiných objektů, které Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="e6162-130">Podobná stránky, můžete použít `PageModel` třídy, je uvedené v následující dva soubory.</span><span class="sxs-lookup"><span data-stu-id="e6162-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="e6162-131">*Pages/Index2.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="e6162-132">*Pages/Index2.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="e6162-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="e6162-133">Podle konvence `PageModel` soubor třídy má stejný název jako soubor stránky Razor s *.cs* připojí.</span><span class="sxs-lookup"><span data-stu-id="e6162-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="e6162-134">Je třeba na předchozí stránku Razor *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e6162-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="e6162-135">Soubor obsahující `PageModel` třída nazývá *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="e6162-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="e6162-136">Přidružení cesty adresy URL na stránky určuje umístění stránky v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="e6162-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="e6162-137">Následující tabulka uvádí stránky Razor cestu a odpovídající adresy URL:</span><span class="sxs-lookup"><span data-stu-id="e6162-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="e6162-138">Název souboru a cesty</span><span class="sxs-lookup"><span data-stu-id="e6162-138">File name and path</span></span>               | <span data-ttu-id="e6162-139">odpovídající adresy URL</span><span class="sxs-lookup"><span data-stu-id="e6162-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e6162-140">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="e6162-141">`/` Nebo `/Index`</span><span class="sxs-lookup"><span data-stu-id="e6162-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="e6162-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="e6162-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="e6162-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="e6162-145">`/Store` Nebo `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="e6162-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="e6162-146">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="e6162-146">Notes:</span></span>

* <span data-ttu-id="e6162-147">Modul runtime vypadá pro stránky Razor soubory v *stránky* složky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e6162-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="e6162-148">`Index` Když neobsahuje adresu URL stránky, je výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="e6162-149">Zápis základní formulář</span><span class="sxs-lookup"><span data-stu-id="e6162-149">Writing a basic form</span></span>

<span data-ttu-id="e6162-150">Funkce stránky Razor jsou navrženy tak, aby se používají u webových prohlížečů snadno běžných vzorů.</span><span class="sxs-lookup"><span data-stu-id="e6162-150">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="e6162-151">[Model vazby](xref:mvc/models/model-binding), [značky Pomocníci](xref:mvc/views/tag-helpers/intro)a všechny pomocné rutiny HTML *právě pracovní* s vlastnostmi definovaný ve třídě stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="e6162-152">Vezměte v úvahu stránky, který implementuje "kontaktujte nás" formuláři pro základní `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="e6162-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="e6162-153">Pro ukázky v tomto dokumentu `DbContext` je inicializován v [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) souboru.</span><span class="sxs-lookup"><span data-stu-id="e6162-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="e6162-154">Datový model:</span><span class="sxs-lookup"><span data-stu-id="e6162-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="e6162-155">Kontext databáze:</span><span class="sxs-lookup"><span data-stu-id="e6162-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="e6162-156">*Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="e6162-157">*Pages/Create.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="e6162-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="e6162-158">Podle konvence `PageModel` třídy se nazývá `<PageName>Model` a je v stejného oboru názvů jako stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="e6162-159">`PageModel` Třída umožňuje oddělení logiku stránky od jeho prezentaci.</span><span class="sxs-lookup"><span data-stu-id="e6162-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="e6162-160">Definuje stránky obslužné rutiny pro požadavky odeslané na stránku a data použitá k vykreslení stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="e6162-161">Toto oddělení vám umožní spravovat stránky závislosti prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection) a [testování částí](xref:testing/razor-pages-testing) stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="e6162-162">Na stránce `OnPostAsync` *metoda obslužná rutina*, která se spouští `POST` požadavků (když uživatel odešle formulář).</span><span class="sxs-lookup"><span data-stu-id="e6162-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="e6162-163">Můžete přidat metody obslužné rutiny pro všechny akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="e6162-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="e6162-164">Nejběžnější obslužné rutiny jsou:</span><span class="sxs-lookup"><span data-stu-id="e6162-164">The most common handlers are:</span></span>

* <span data-ttu-id="e6162-165">`OnGet` k chybě při inicializaci stavu potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="e6162-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="e6162-166">[OnGet](#OnGet) ukázka.</span><span class="sxs-lookup"><span data-stu-id="e6162-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="e6162-167">`OnPost` pro zpracování odesílání formuláře.</span><span class="sxs-lookup"><span data-stu-id="e6162-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="e6162-168">`Async` Přípona je nepovinný, ale se často používá podle konvence pro asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="e6162-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="e6162-169">`OnPostAsync` Kód v předchozím příkladu bude vypadat podobně jako by za normálních okolností psaní v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="e6162-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="e6162-170">Předchozí kód je typické pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="e6162-171">Většina primitiv MVC jako [model vazby](xref:mvc/models/model-binding), [ověření](xref:mvc/models/validation), a jsou sdíleny výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="e6162-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="e6162-172">Předchozí `OnPostAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="e6162-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="e6162-173">Základní tok `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="e6162-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="e6162-174">Zkontrolujte chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="e6162-174">Check for validation errors.</span></span>

*  <span data-ttu-id="e6162-175">Pokud nejsou žádné chyby, uložit data a přesměrování.</span><span class="sxs-lookup"><span data-stu-id="e6162-175">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="e6162-176">Pokud nejsou chyby, zobrazit stránku znovu s ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="e6162-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="e6162-177">Ověřování na straně klienta je stejný jako tradiční aplikací ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="e6162-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="e6162-178">V mnoha případech chyb při ověřování by být zjištěn v klientském počítači a nikdy odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="e6162-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="e6162-179">Při úspěšném zadání data `OnPostAsync` volání metod obslužné rutiny `RedirectToPage` Pomocná metoda vrátí instanci `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="e6162-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="e6162-180">`RedirectToPage` představuje novou výsledek akce, podobně jako `RedirectToAction` nebo `RedirectToRoute`, ale přizpůsobené pro stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="e6162-181">V předchozím příkladu, je přesměrován na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="e6162-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="e6162-182">`RedirectToPage` podrobně [generování adresy URL pro stránky](#url_gen) části.</span><span class="sxs-lookup"><span data-stu-id="e6162-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="e6162-183">Když odeslaného formuláře došlo k chybám ověřování, (které jsou předávány serveru),`OnPostAsync` volání metod obslužné rutiny `Page` metodu helper.</span><span class="sxs-lookup"><span data-stu-id="e6162-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="e6162-184">`Page` Vrací instanci třídy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="e6162-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="e6162-185">Vrácení `Page` je podobná jak akce v řadiče vrátit `View`.</span><span class="sxs-lookup"><span data-stu-id="e6162-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="e6162-186">`PageResult` Výchozí nastavení je <!-- Review  --> návratový typ pro obslužné rutiny metodu.</span><span class="sxs-lookup"><span data-stu-id="e6162-186">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="e6162-187">Obslužná rutina metodu, která vrátí `void` vykreslí stránku.</span><span class="sxs-lookup"><span data-stu-id="e6162-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="e6162-188">`Customer` Používá vlastnost `[BindProperty]` atribut se vyjádřit výslovný souhlas s vazbou modelu.</span><span class="sxs-lookup"><span data-stu-id="e6162-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="e6162-189">Stránky Razor, ve výchozím nastavení, vazbu vlastnosti jenom s příkazy nebo GET.</span><span class="sxs-lookup"><span data-stu-id="e6162-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="e6162-190">Vazba na vlastnosti může snížit množství kód, který máte k zápisu.</span><span class="sxs-lookup"><span data-stu-id="e6162-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="e6162-191">Vazba snižuje kódu pomocí stejné vlastnosti k vykreslení pole formuláře (`<input asp-for="Customer.Name" />`) a přijměte vstupu.</span><span class="sxs-lookup"><span data-stu-id="e6162-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="e6162-192">Z bezpečnostních důvodů musí se přihlášení k vytvoření vazby dat požadavek GET na stránce Vlastnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="e6162-192">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="e6162-193">Ověřte vstup uživatele před mapování vlastností.</span><span class="sxs-lookup"><span data-stu-id="e6162-193">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="e6162-194">Vyjádření výslovného souhlasu s toto chování je užitečné při vytváření funkcí, které jsou závislé na hodnoty dotazu řetězec nebo trasy.</span><span class="sxs-lookup"><span data-stu-id="e6162-194">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="e6162-195">Chcete-li vytvořit vazbu vlastnosti pro požadavky GET, nastavte `[BindProperty]` atributu `SupportsGet` vlastnost `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="e6162-195">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="e6162-196">Domovská stránka (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="e6162-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="e6162-197">Kódu *Index.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="e6162-198">*Index.cshtml* soubor obsahuje následující kód k vytvoření odkazu pro úpravy pro každý kontakt:</span><span class="sxs-lookup"><span data-stu-id="e6162-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="e6162-199">[Pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) použít `asp-route-{value}` atribut pro vygenerování odkazu na stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="e6162-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="e6162-200">Odkaz obsahuje data trasy s kontakt ID.</span><span class="sxs-lookup"><span data-stu-id="e6162-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="e6162-201">Například `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="e6162-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="e6162-202">*Pages/Edit.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="e6162-203">První řádek obsahuje `@page "{id:int}"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e6162-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="e6162-204">Omezení směrování`"{id:int}"` informuje stránky tak, aby přijímal požadavky na stránku, které obsahují `int` směrovat data.</span><span class="sxs-lookup"><span data-stu-id="e6162-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="e6162-205">Pokud požadavek na stránku neobsahuje data trasy, která lze převést na `int`, modul runtime vrátí chybu HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="e6162-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="e6162-206">Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="e6162-206">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="e6162-207">*Pages/Edit.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="e6162-208">*Index.cshtml* soubor zároveň obsahuje kód k vytvoření tlačítko Odstranit pro každý kontakt zákazníka:</span><span class="sxs-lookup"><span data-stu-id="e6162-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="e6162-209">Pokud tlačítko Odstranit je vykreslena ve formátu HTML, jeho `formaction` zahrnuje parametry:</span><span class="sxs-lookup"><span data-stu-id="e6162-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="e6162-210">Obraťte se na zákaznické ID určeného `asp-route-id` atribut.</span><span class="sxs-lookup"><span data-stu-id="e6162-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="e6162-211">`handler` Určeným elementem `asp-page-handler` atribut.</span><span class="sxs-lookup"><span data-stu-id="e6162-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="e6162-212">Tady je příklad vykreslené odstranění tlačítka s zákazník obraťte se na ID `1`:</span><span class="sxs-lookup"><span data-stu-id="e6162-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="e6162-213">Při výběru tlačítka, formuláře `POST` požadavek odeslán do serveru.</span><span class="sxs-lookup"><span data-stu-id="e6162-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="e6162-214">Podle konvence je vybraný název metody obslužné rutiny na základě hodnotu `handler` parametr podle schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="e6162-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="e6162-215">Protože `handler` je `delete` v tomto příkladu `OnPostDeleteAsync` metoda obslužná rutina se používá procesu `POST` požadavku.</span><span class="sxs-lookup"><span data-stu-id="e6162-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="e6162-216">Pokud `asp-page-handler` nastavena na jinou hodnotu, jako například `remove`, metodu obslužná rutina stránky s názvem `OnPostRemoveAsync` je vybrána.</span><span class="sxs-lookup"><span data-stu-id="e6162-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="e6162-217">`OnPostDeleteAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="e6162-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="e6162-218">Přijme `id` z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="e6162-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="e6162-219">Dotazuje databázi pro zákazníka kontaktu s `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="e6162-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="e6162-220">Pokud je nalezen kontaktování zákazníků, budou se odebrat ze seznamu kontaktů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e6162-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="e6162-221">Databáze se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="e6162-221">The database is updated.</span></span>
* <span data-ttu-id="e6162-222">Volání `RedirectToPage` přesměrovat na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="e6162-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="e6162-223">XSRF/proti útokům CSRF a stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="e6162-224">Nemáte psaní jakéhokoli kódu pro [antiforgery ověření](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="e6162-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="e6162-225">Antiforgery generování tokenů a ověření jsou automaticky součástí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="e6162-226">Pomocí stránky Razor rozložení, částečné., šablony a pomocníky značky</span><span class="sxs-lookup"><span data-stu-id="e6162-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="e6162-227">Stránky fungovat se všemi funkcemi zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="e6162-228">Rozložení, šablony, a částečné značka Pomocníci *soubor _ViewStart.cshtml*, *_ViewImports.cshtml* pracovní stejným způsobem, tak pro běžné zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="e6162-229">Tato stránka umožňuje declutter díky některé z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="e6162-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="e6162-230">Přidat [rozložení stránky](xref:mvc/views/layout) k *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6162-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="e6162-231">[Rozložení](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="e6162-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="e6162-232">Určuje rozložení každé stránce (Pokud stránce výslovný nesouhlas rozložení).</span><span class="sxs-lookup"><span data-stu-id="e6162-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="e6162-233">Importuje struktury HTML jako je JavaScript a šablon.</span><span class="sxs-lookup"><span data-stu-id="e6162-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="e6162-234">V tématu [rozložení stránky](xref:mvc/views/layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e6162-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="e6162-235">[Rozložení](xref:mvc/views/layout#specifying-a-layout) je nastavena *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6162-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="e6162-236">**Poznámka:** rozložení je v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e6162-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="e6162-237">Stránky vyhledejte další zobrazení (rozložení, šablony, částečné.) hierarchicky, spouštění ve stejné složce jako aktuální stránku.</span><span class="sxs-lookup"><span data-stu-id="e6162-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="e6162-238">Rozložení v *stránky* složky lze z libovolné stránky Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e6162-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="e6162-239">Doporučujeme **není** chápat rozložení souboru *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="e6162-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="e6162-240">*Zobrazení a sdílených* je zobrazení vzor MVC.</span><span class="sxs-lookup"><span data-stu-id="e6162-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="e6162-241">Stránky Razor jsou určené spoléhají na hierarchii složek, není cesta konvence.</span><span class="sxs-lookup"><span data-stu-id="e6162-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="e6162-242">Obsahuje zobrazení vyhledávání ze stránky Razor *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e6162-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="e6162-243">Rozložení, šablony a částečné používáte s řadiče MVC a konvenční zobrazení syntaxe Razor *právě pracovní*.</span><span class="sxs-lookup"><span data-stu-id="e6162-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="e6162-244">Přidat *Pages/_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="e6162-245">`@namespace` se vysvětluje dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e6162-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="e6162-246">`@addTagHelper` – Direktiva přináší [předdefinované značky Pomocníci](xref:mvc/views/tag-helpers/builtin-th/Index) pro všechny stránky v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="e6162-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="e6162-247">Když `@namespace` – direktiva se používá explicitně na stránce:</span><span class="sxs-lookup"><span data-stu-id="e6162-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="e6162-248">Direktiva Nastaví obor názvů pro stránku.</span><span class="sxs-lookup"><span data-stu-id="e6162-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="e6162-249">`@model` – Direktiva nemusí obsahovat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="e6162-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="e6162-250">Když `@namespace` – direktiva je součástí *_ViewImports.cshtml*, zadaný obor názvů poskytuje předponu pro obor názvů generovaným na stránce, který umožňuje importovat `@namespace` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e6162-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="e6162-251">Zbytek vygenerovaného oboru názvů (část příponu) je relativní cesta mezi složku, která obsahuje oddělené tečkou *_ViewImports.cshtml* a složce obsahující danou stránku.</span><span class="sxs-lookup"><span data-stu-id="e6162-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="e6162-252">Například kódu na pozadí *Pages/Customers/Edit.cshtml.cs* explicitně nastaví obor názvů:</span><span class="sxs-lookup"><span data-stu-id="e6162-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="e6162-253">*Pages/_ViewImports.cshtml* souboru nastaví následující obor názvů:</span><span class="sxs-lookup"><span data-stu-id="e6162-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="e6162-254">Vygenerovaný obor názvů pro *Pages/Customers/Edit.cshtml* Razor stránky je stejná jako souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="e6162-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="e6162-255">`@namespace` – Direktiva v byla navržená tak, třídy C# přidán do projektu a kód generovaný stránky *právě pracovní* bez nutnosti přidání `@using` direktivy pro souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="e6162-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="e6162-256">**Poznámka:** `@namespace` funguje taky s konvenční zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="e6162-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="e6162-257">Původní *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="e6162-258">Aktualizovaný *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="e6162-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="e6162-259">[Projektu úvodní stránky Razor](#rpvs17) obsahuje *Pages/_ValidationScriptsPartial.cshtml*, který zachytí až ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e6162-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="e6162-260">Generování adresy URL pro stránky</span><span class="sxs-lookup"><span data-stu-id="e6162-260">URL generation for Pages</span></span>

<span data-ttu-id="e6162-261">`Create` Stránky, která je uvedená dříve, používá `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="e6162-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="e6162-262">Aplikace má následující strukturu souboru nebo složky:</span><span class="sxs-lookup"><span data-stu-id="e6162-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="e6162-263">*/ Stránky*</span><span class="sxs-lookup"><span data-stu-id="e6162-263">*/Pages*</span></span>

  * <span data-ttu-id="e6162-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="e6162-265">*/ Odběratele.*</span><span class="sxs-lookup"><span data-stu-id="e6162-265">*/Customers*</span></span>

    * <span data-ttu-id="e6162-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="e6162-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="e6162-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6162-268">*Index.cshtml*</span></span>

<span data-ttu-id="e6162-269">*Pages/Customers/Create.cshtml* a *Pages/Customers/Edit.cshtml* stránky přesměrovat na *Pages/Index.cshtml* po úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e6162-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="e6162-270">Řetězec `/Index` je součástí identifikátor URI k přístupu na předchozí stránku.</span><span class="sxs-lookup"><span data-stu-id="e6162-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="e6162-271">Řetězec `/Index` lze použít ke generování identifikátory URI k *Pages/Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="e6162-272">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e6162-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="e6162-273">Název stránky je cesta na stránku z kořenového adresáře */stránky* složky (včetně jako úvodní `/`, například `/Index`).</span><span class="sxs-lookup"><span data-stu-id="e6162-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="e6162-274">Předchozí ukázky generování adresy URL jsou mnohem víc bohaté funkce než jenom hardcoding adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6162-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="e6162-275">Generování adresy URL používá [směrování](xref:mvc/controllers/routing) a vygenerovat a kódování parametry podle jak trasy, která je definována v cílovou cestu.</span><span class="sxs-lookup"><span data-stu-id="e6162-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="e6162-276">Generování adresy URL pro stránky podporuje relativních názvů.</span><span class="sxs-lookup"><span data-stu-id="e6162-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="e6162-277">Následující tabulka uvádí, které Index vybrány jiné `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e6162-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="e6162-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="e6162-278">RedirectToPage(x)</span></span>| <span data-ttu-id="e6162-279">Stránka</span><span class="sxs-lookup"><span data-stu-id="e6162-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e6162-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="e6162-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="e6162-281">*Stránky/indexu*</span><span class="sxs-lookup"><span data-stu-id="e6162-281">*Pages/Index*</span></span> |
| <span data-ttu-id="e6162-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="e6162-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="e6162-283">*Stránky nebo zákazníků nebo indexu*</span><span class="sxs-lookup"><span data-stu-id="e6162-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="e6162-284">RedirectToPage(".. / Indexu)</span><span class="sxs-lookup"><span data-stu-id="e6162-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="e6162-285">*Stránky/indexu*</span><span class="sxs-lookup"><span data-stu-id="e6162-285">*Pages/Index*</span></span> |
| <span data-ttu-id="e6162-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="e6162-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="e6162-287">*Stránky nebo zákazníků nebo indexu*</span><span class="sxs-lookup"><span data-stu-id="e6162-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="e6162-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, a `RedirectToPage("../Index")` jsou <em>relativních názvů</em>.</span><span class="sxs-lookup"><span data-stu-id="e6162-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="e6162-289">`RedirectToPage` Parametr <em>kombinaci</em> cestou k aktuální stránce k výpočtu název cílové stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-289">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="e6162-290">Relativní název propojení je užitečné, při vytváření lokalit se strukturou komplexní.</span><span class="sxs-lookup"><span data-stu-id="e6162-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="e6162-291">Pokud používáte relativních názvů propojení mezi stránkami ve složce, můžete přejmenovat této složky.</span><span class="sxs-lookup"><span data-stu-id="e6162-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="e6162-292">Všechny odkazy na i nadále fungovat, (protože jejich nezahrnuli název složky).</span><span class="sxs-lookup"><span data-stu-id="e6162-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="e6162-293">TempData</span><span class="sxs-lookup"><span data-stu-id="e6162-293">TempData</span></span>

<span data-ttu-id="e6162-294">ASP.NET Core zpřístupní [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnost [řadič](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="e6162-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="e6162-295">Tato vlastnost ukládá data, dokud je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e6162-295">This property stores data until it's read.</span></span> <span data-ttu-id="e6162-296">`Keep` a `Peek` metody můžete použít k prozkoumání dat bez odstranění.</span><span class="sxs-lookup"><span data-stu-id="e6162-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="e6162-297">`TempData` jsou užitečné pro přesměrování, pokud se data potřebná pro více než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="e6162-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="e6162-298">`[TempData]` Atribut je nového v technologii ASP.NET 2.0 jádra a je podporovaná v řadičích a stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="e6162-299">Následující kód nastaví hodnotu `Message` pomocí `TempData`:</span><span class="sxs-lookup"><span data-stu-id="e6162-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="e6162-300">Následující kód v *Pages/Customers/Index.cshtml* souboru se zobrazí hodnota `Message` pomocí `TempData`.</span><span class="sxs-lookup"><span data-stu-id="e6162-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="e6162-301">*Pages/Customers/Index.cshtml.cs* modelu stránka se vztahuje `[TempData]` atribut `Message` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e6162-301">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="e6162-302">V tématu [TempData](xref:fundamentals/app-state#temp) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e6162-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="e6162-303">Více obslužné rutiny na stránce</span><span class="sxs-lookup"><span data-stu-id="e6162-303">Multiple handlers per page</span></span>

<span data-ttu-id="e6162-304">Na následující stránce generuje kód pro dvě stránky pomocí obslužné rutiny `asp-page-handler` pomocná značky:</span><span class="sxs-lookup"><span data-stu-id="e6162-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="e6162-305">Formuláře v předchozím příkladu má dva odeslání tlačítka, každý využívající `FormActionTagHelper` odeslat na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6162-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="e6162-306">`asp-page-handler` Atribut je Pomocníka pro `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="e6162-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="e6162-307">`asp-page-handler` generuje adresy URL, které odesílají do jednotlivých metod obslužné rutiny, které jsou definované na stránce.</span><span class="sxs-lookup"><span data-stu-id="e6162-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="e6162-308">`asp-page` není zadán, protože je ukázka propojení na aktuální stránce.</span><span class="sxs-lookup"><span data-stu-id="e6162-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="e6162-309">Model stránky:</span><span class="sxs-lookup"><span data-stu-id="e6162-309">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="e6162-310">Předchozí kód používá *s názvem metody obslužná rutina*.</span><span class="sxs-lookup"><span data-stu-id="e6162-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="e6162-311">Metody s názvem obslužné rutiny vytvářejí provedením text v názvu po `On<HTTP Verb>` a před `Async` (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="e6162-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="e6162-312">V předchozím příkladu jsou metody stránky OnPost**JoinList**Async a OnPost**JoinListUC**asynchronní.</span><span class="sxs-lookup"><span data-stu-id="e6162-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="e6162-313">S *OnPost* a *asynchronní* odebrat, jsou názvy obslužná rutina `JoinList` a `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="e6162-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="e6162-314">Použití předcházející code cesty URL, kterou odesílá `OnPostJoinListAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="e6162-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="e6162-315">Cesty URL, kterou odesílá `OnPostJoinListUCAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="e6162-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="e6162-316">Přizpůsobení směrování</span><span class="sxs-lookup"><span data-stu-id="e6162-316">Customizing Routing</span></span>

<span data-ttu-id="e6162-317">Pokud chcete řetězec dotazu `?handler=JoinList` v adrese URL, můžete změnit trasy, která má název obslužné rutiny chápat část adresy obsahující cestu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e6162-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="e6162-318">Trasy, která můžete přizpůsobit přidáním šablonu trasy v dvojitých uvozovkách po `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="e6162-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="e6162-319">Předchozí trasy vloží název obslužné rutiny cesty URL místo řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="e6162-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="e6162-320">`?` Následující `handler` znamená je volitelný parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="e6162-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="e6162-321">Můžete použít `@page` a přidejte další segmenty a parametry k postupu na stránce.</span><span class="sxs-lookup"><span data-stu-id="e6162-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="e6162-322">Ať je k dispozici má **připojí** trasu výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="e6162-323">Použití absolutní nebo virtuální cesta ke změně stránky trasy (například `"~/Some/Other/Path"`) není podporován.</span><span class="sxs-lookup"><span data-stu-id="e6162-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="e6162-324">Konfigurace a nastavení</span><span class="sxs-lookup"><span data-stu-id="e6162-324">Configuration and settings</span></span>

<span data-ttu-id="e6162-325">Nakonfigurujte rozšířené možnosti, pomocí metody rozšíření `AddRazorPagesOptions` na tvůrce MVC:</span><span class="sxs-lookup"><span data-stu-id="e6162-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="e6162-326">Teď můžete použít `RazorPagesOptions` nastavit kořenový adresář pro stránky, nebo přidejte aplikace model konvence pro stránky.</span><span class="sxs-lookup"><span data-stu-id="e6162-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="e6162-327">Jsme budete povolit další rozšíření tímto způsobem v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="e6162-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="e6162-328">Předkompilovat zobrazení, najdete v části [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="e6162-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="e6162-329">[Stažení nebo zobrazení ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="e6162-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="e6162-330">V tématu [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start), který je založený na tento úvod.</span><span class="sxs-lookup"><span data-stu-id="e6162-330">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="e6162-331">Zadejte, zda stránky Razor se v kořenovém adresáři obsahu</span><span class="sxs-lookup"><span data-stu-id="e6162-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="e6162-332">Ve výchozím nastavení jsou stránky Razor root v */stránky* adresáře.</span><span class="sxs-lookup"><span data-stu-id="e6162-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="e6162-333">Přidat [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se v kořenovém adresáři obsahu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikace:</span><span class="sxs-lookup"><span data-stu-id="e6162-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="e6162-334">Určit, že stránky Razor na vlastní kořenový adresář</span><span class="sxs-lookup"><span data-stu-id="e6162-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="e6162-335">Přidat [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se vlastní kořenového adresáře v aplikaci (zadejte relativní cestu):</span><span class="sxs-lookup"><span data-stu-id="e6162-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="e6162-336">Viz také</span><span class="sxs-lookup"><span data-stu-id="e6162-336">See also</span></span>

* [<span data-ttu-id="e6162-337">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6162-337">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="e6162-338">Syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-338">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="e6162-339">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-339">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="e6162-340">Konvence autorizace stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-340">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="e6162-341">Syntaxe Razor stránky vlastní trasy a stránka zprostředkovatele modelu</span><span class="sxs-lookup"><span data-stu-id="e6162-341">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="e6162-342">Testy jednotek a integrace stránky Razor</span><span class="sxs-lookup"><span data-stu-id="e6162-342">Razor Pages unit and integration tests</span></span>](xref:testing/razor-pages-testing)
