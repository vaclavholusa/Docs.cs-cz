---
title: Úvod do služby v ASP.NET Core Razor Pages
author: Rick-Anderson
description: Zjistěte, jak v ASP.NET Core Razor Pages díky psaní kódu zaměřená na stránce scénáře jednodušší a produktivnější než pomocí MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 49bed6cc150a74ff8b72848f276c55c2490b6fa5
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889139"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="0b0bd-103">Úvod do služby v ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0b0bd-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0b0bd-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Ryanem Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="0b0bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="0b0bd-105">Stránky Razor je nový aspekt ASP.NET Core MVC, která provádí kódování zaměřené na stránce scénáře jednodušší a produktivnější.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="0b0bd-106">Pokud hledáte kurz, který používá Model-View-Controller přístup, přečtěte si téma [Začínáme s ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="0b0bd-107">Tento dokument obsahuje úvod do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="0b0bd-108">Není podrobný kurz.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="0b0bd-109">Pokud některé části příliš pokročilý najdete v tématu [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="0b0bd-110">Přehled ASP.NET Core, najdete v článku [Úvod do ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b0bd-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0b0bd-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="0b0bd-112">Vytvoření projektu pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b0bd-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b0bd-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b0bd-114">Zobrazit [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start) pro podrobné pokyny o tom, jak vytvářet stránky Razor projekt pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0b0bd-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0b0bd-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b0bd-116">Spustit `dotnet new webapp` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b0bd-117">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="0b0bd-118">Otevřete vygenerovaný *.csproj* souboru ze sady Visual Studio pro Mac.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0b0bd-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0b0bd-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b0bd-120">Spustit `dotnet new webapp` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-120">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b0bd-121">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0b0bd-122">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0bd-122">.NET Core CLI</span></span>](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b0bd-123">Spustit `dotnet new webapp` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-123">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b0bd-124">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="0b0bd-125">Stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-125">Razor Pages</span></span>

<span data-ttu-id="0b0bd-126">Stránky Razor je povolený v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="0b0bd-127">Vezměte v úvahu základní stránka: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="0b0bd-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="0b0bd-128">Předchozí kód je hodně jako soubor zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="0b0bd-129">Co vlastně rozdíl je `@page` směrnice.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="0b0bd-130">`@page` Akce MVC – to znamená, že zpracovává požadavky přímo, bez nutnosti kontaktovat řadič vytvoří soubor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="0b0bd-131">`@page` musí být první direktivy Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="0b0bd-132">`@page` má vliv na chování jiných objektů, které Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="0b0bd-133">Podobná stránka, pomocí `PageModel` třídy, se zobrazí v následujících dvou souborech.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="0b0bd-134">*Pages/Index2.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="0b0bd-135">*Pages/Index2.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="0b0bd-136">Podle konvence `PageModel` soubor třídy má stejný název jako soubor stránky Razor s *.cs* připojí.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="0b0bd-137">Je třeba na předchozí stránku Razor *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="0b0bd-138">Soubor obsahující `PageModel` třída se nazývá *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="0b0bd-139">Přidružení cesty adresy URL na stránky jsou určeny na stránce umístění v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="0b0bd-140">Následující tabulka uvádí cesty stránky Razor a odpovídající adresy URL:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="0b0bd-141">Název souboru a cestu</span><span class="sxs-lookup"><span data-stu-id="0b0bd-141">File name and path</span></span>               | <span data-ttu-id="0b0bd-142">odpovídající adresy URL</span><span class="sxs-lookup"><span data-stu-id="0b0bd-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="0b0bd-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="0b0bd-144">`/` Nebo `/Index`</span><span class="sxs-lookup"><span data-stu-id="0b0bd-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="0b0bd-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="0b0bd-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="0b0bd-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="0b0bd-148">`/Store` Nebo `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="0b0bd-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="0b0bd-149">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-149">Notes:</span></span>

* <span data-ttu-id="0b0bd-150">Modul runtime vyhledá soubory Razor Pages *stránky* složky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="0b0bd-151">`Index` Když neobsahuje adresu URL stránky, je výchozí stránka.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="0b0bd-152">Zápis základní formulář</span><span class="sxs-lookup"><span data-stu-id="0b0bd-152">Writing a basic form</span></span>

<span data-ttu-id="0b0bd-153">Stránky Razor je navržená tak, aby běžné vzory, které se používají u webových prohlížečů snadno se implementuje při sestavování aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="0b0bd-154">[Vazby modelu](xref:mvc/models/model-binding), [pomocných rutin značek](xref:mvc/views/tag-helpers/intro)a všechny pomocných rutin HTML *jenom pracovní* pomocí vlastnosti definované ve třídě stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="0b0bd-155">Vezměte v úvahu, který implementuje "kontaktujte nás" formuláře pro základní stránku `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="0b0bd-156">Ukázky v tomto dokumentu `DbContext` se inicializuje [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) souboru.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="0b0bd-157">Datový model:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="0b0bd-158">Kontext databáze:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="0b0bd-159">*Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="0b0bd-160">*Pages/Create.cshtml.cs* model stránky:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="0b0bd-161">Podle konvence `PageModel` třída se nazývá `<PageName>Model` a je v oboru názvů stejný jako stránka.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="0b0bd-162">`PageModel` Třída umožňuje oddělení logiky stránku od jeho prezentaci.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="0b0bd-163">Definuje stránky obslužné rutiny pro požadavky odeslané na stránce a data použitá k vykreslení stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="0b0bd-164">Toto oddělení vám umožní spravovat závislosti stránky prostřednictvím [injektáž závislostí](xref:fundamentals/dependency-injection) a [Jednotkový test](xref:test/razor-pages-tests) stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="0b0bd-165">Na stránce `OnPostAsync` *metodu obslužné rutiny*, která se spouští `POST` požadavků (když uživatel odešle formulář).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="0b0bd-166">Můžete přidat metody obslužné rutiny pro libovolný příkaz protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="0b0bd-167">Nejčastěji používané rutiny jsou:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-167">The most common handlers are:</span></span>

* <span data-ttu-id="0b0bd-168">`OnGet` Inicializace stavu potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="0b0bd-169">[OnGet](#OnGet) vzorku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="0b0bd-170">`OnPost` zpracování formulářů.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="0b0bd-171">`Async` Přípona je volitelné, ale často používá konvence pro asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="0b0bd-172">`OnPostAsync` Kód v předchozím příkladu vypadá podobně jako byste normálně psaní v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="0b0bd-173">Předchozí kód je typický pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="0b0bd-174">Většina primitiv MVC, jako jsou [vazby modelu](xref:mvc/models/model-binding), [ověření](xref:mvc/models/validation), výsledky akcí se sdílejí.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="0b0bd-175">Předchozí `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="0b0bd-176">Základní tok `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="0b0bd-177">Zkontrolován výskyt chyb ověření.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-177">Check for validation errors.</span></span>

*  <span data-ttu-id="0b0bd-178">Pokud nejsou žádné chyby, uložit data a přesměrování.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="0b0bd-179">Pokud chyby existují, zobrazit stránku znovu pomocí ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="0b0bd-180">Ověřování na straně klienta se shoduje s tradiční aplikace ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="0b0bd-181">V mnoha případech se chyby ověření by být zjištěn v klientském počítači a nikdy odeslána na server.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="0b0bd-182">Při zadání dat se úspěšně, `OnPostAsync` volání metody obslužné rutiny `RedirectToPage` Pomocná metoda vrátit instanci `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="0b0bd-183">`RedirectToPage` je nový výsledek akce, podobně jako `RedirectToAction` nebo `RedirectToRoute`, ale přizpůsobené pro stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="0b0bd-184">V předchozím příkladu, je přesměrován na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="0b0bd-185">`RedirectToPage` podrobně popisuje [generování adresy URL pro stránky](#url_gen) oddílu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="0b0bd-186">Když odeslané formuláře došlo k chybám ověřování, (, které se předá serveru),`OnPostAsync` volání metody obslužné rutiny `Page` Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="0b0bd-187">`Page` Vrátí instanci `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="0b0bd-188">Vrací `Page` je podobný jak vrátit akce v řadiče `View`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="0b0bd-189">`PageResult` Výchozí hodnota je <!-- Review  --> návratový typ pro metodu obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="0b0bd-190">Metoda obslužné rutiny, která vrací `void` vykreslující danou stránku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="0b0bd-191">`Customer` Používá vlastnost `[BindProperty]` atribut můžete vyjádřit výslovný souhlas vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="0b0bd-192">Stránky Razor, ve výchozím nastavení, vázat vlastnosti pouze s příkazy bez GET.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="0b0bd-193">Vytvoření vazby na vlastnosti může snížit množství kódu, který musíte napsat.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="0b0bd-194">Vazba snižuje kódu pomocí stejné vlastnosti k vykreslení pole formuláře (`<input asp-for="Customer.Name" />`) a přijměte vstupu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="0b0bd-195">Z bezpečnostních důvodů musíte připojíte k vytvoření vazby data požadavku GET na vlastnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-195">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="0b0bd-196">Ověření vstupu uživatele před mapování na vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-196">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="0b0bd-197">Vyjádření výslovného souhlasu s toto chování je užitečné, když adresování scénáře, které jsou závislé na řetězec nebo trasy hodnoty dotazu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-197">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="0b0bd-198">Chcete-li vytvořit vazbu vlastnosti pro požadavky GET, nastavte `[BindProperty]` atributu `SupportsGet` vlastnost `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="0b0bd-198">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="0b0bd-199">Na domovské stránce (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="0b0bd-199">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="0b0bd-200">Přidružené `PageModel` třídy (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="0b0bd-200">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="0b0bd-201">*Index.cshtml* soubor obsahuje následující kód k vytvoření odkaz pro úpravy pro každého kontaktu:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-201">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="0b0bd-202">[Ukotvení pomocné rutiny značky](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) použít `asp-route-{value}` atribut pro vygenerování odkazu na stránku pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-202">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="0b0bd-203">Odkaz obsahuje data trasy s kontaktu ID.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-203">The link contains route data with the contact ID.</span></span> <span data-ttu-id="0b0bd-204">Například `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-204">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="0b0bd-205">*Pages/Edit.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-205">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="0b0bd-206">První řádek obsahuje `@page "{id:int}"` směrnice.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-206">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="0b0bd-207">Omezení směrování`"{id:int}"` říká stránce tak, aby přijímal požadavky na stránku, které obsahují `int` směrovat data.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-207">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="0b0bd-208">Pokud požadavek na stránku neobsahuje data trasy, který lze převést na `int`, modul runtime vrátí chybu HTTP 404 (Nenalezeno).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-208">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="0b0bd-209">Chcete-li nastavit ID volitelný, přidejte `?` pro dané omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-209">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="0b0bd-210">*Pages/Edit.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="0b0bd-211">*Index.cshtml* soubor obsahuje také značky, chcete-li vytvořit tlačítko pro odstranění pro každého zákazníka kontaktu:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="0b0bd-212">Když tlačítko pro odstranění se vykreslí v HTML, jeho `formaction` obsahuje parametry pro:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="0b0bd-213">Aby se zákazník obrátil zadané podle ID `asp-route-id` atribut.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="0b0bd-214">`handler` Určená `asp-page-handler` atribut.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="0b0bd-215">Tady je příklad tlačítko pro odstranění vykreslené zákazníků obraťte se na ID `1`:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="0b0bd-216">Při výběru tlačítka, formulář `POST` odeslán požadavek na server.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="0b0bd-217">Podle konvence je vybrán název metody obslužné rutiny na základě hodnoty `handler` parametr podle schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="0b0bd-218">Vzhledem k tomu, `handler` je `delete` v tomto příkladu `OnPostDeleteAsync` obslužná rutina se používá k procesu `POST` požadavku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="0b0bd-219">Pokud `asp-page-handler` je nastavena na jinou hodnotu, jako například `remove`, metodu obslužné rutiny stránky s názvem `OnPostRemoveAsync` zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="0b0bd-220">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="0b0bd-221">Přijímá `id` z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="0b0bd-222">Dotazuje databázi pro kontakt zákazníka s `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="0b0bd-223">Pokud je nalezen kontakt zákazníka, budou odebráni ze seznamu kontaktů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="0b0bd-224">Databáze se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-224">The database is updated.</span></span>
* <span data-ttu-id="0b0bd-225">Volání `RedirectToPage` přesměrovat na indexovou stránku root (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="0b0bd-226">Požadované vlastnosti označit stránky</span><span class="sxs-lookup"><span data-stu-id="0b0bd-226">Mark page properties required</span></span>

<span data-ttu-id="0b0bd-227">Vlastnosti `PageModel` může být doplněny pomocí [vyžaduje](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atribut:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-227">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="0b0bd-228">Zobrazit [ověření modelu](xref:mvc/models/validation) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-228">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="0b0bd-229">Spravovat požadavky HEAD s obslužnou rutinou OnGet</span><span class="sxs-lookup"><span data-stu-id="0b0bd-229">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="0b0bd-230">Obslužnou rutinu HEAD je obvykle vytvořen a volat pro požadavky HEAD:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-230">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="0b0bd-231">Pokud žádná obslužná rutina HEAD (`OnHead`) je definovaný, stránky Razor spadne zpět na volání obslužné rutiny GET stránky (`OnGet`) v ASP.NET Core 2.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-231">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="0b0bd-232">Vyjádřit výslovný souhlas s tímto chováním [SetCompatibilityVersion metoda](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) v `Startup.Configure` pro ASP.NET Core 2.1 k 2.x:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-232">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="0b0bd-233">`SetCompatibilityVersion` Nastavuje možnost Razor Pages `AllowMappingHeadRequestsToGetHandler` k `true`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-233">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="0b0bd-234">Místo výběru všech 2.1 chování s `SetCompatibilityVersion`, je můžete explicitně vyjádřit výslovný souhlas pro konkrétní chování.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-234">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="0b0bd-235">Požádá o následující kód do Požadavky HEAD mapování na obslužnou rutinu GET.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-235">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="0b0bd-236">XSRF/CSRF a stránky Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-236">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="0b0bd-237">Nemusíte psát kód pro [antiforgery ověření](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-237">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="0b0bd-238">Antiforgery generování tokenů a ověření jsou automaticky součástí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-238">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="0b0bd-239">Pomocí rozložení, částečných zobrazení, šablon a pomocných rutin značek se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-239">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="0b0bd-240">Stránky fungovat se všemi funkcemi zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-240">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="0b0bd-241">Rozložení, částečných zobrazení, šablony, pomocných rutin značek *soubor _ViewStart.cshtml*, *_ViewImports.cshtml* pracovat stejným způsobem, jako je tomu u konvenční zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-241">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="0b0bd-242">Tato stránka umožňuje declutter s využitím některé z těchto možností.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-242">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b0bd-243">Přidat [stránku rozložení](xref:mvc/views/layout) k *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-243">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b0bd-244">Přidat [stránku rozložení](xref:mvc/views/layout) k *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-244">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="0b0bd-245">[Rozložení](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="0b0bd-245">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="0b0bd-246">Určuje rozložení jednotlivých stránek (Pokud na stránce výslovný rozložení).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-246">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="0b0bd-247">Importuje HTML struktury, jako jsou JavaScript a šablony stylů.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-247">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="0b0bd-248">Zobrazit [stránku rozložení](xref:mvc/views/layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-248">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="0b0bd-249">[Rozložení](xref:mvc/views/layout#specifying-a-layout) je nastavena *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-249">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0b0bd-250">Rozložení se *stránek/Shared* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-250">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="0b0bd-251">Stránky vyhledejte jiným zobrazením (rozložení, šablony, částečných zobrazení) hierarchicky, spouští se ve stejné složce jako aktuální stránka.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-251">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="0b0bd-252">Rozložení v *stránek/Shared* složky je možné z jakékoli stránky Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-252">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="0b0bd-253">Soubor rozložení by měl Přejít *stránek/Shared* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-253">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0b0bd-254">Rozložení se *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-254">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="0b0bd-255">Stránky vyhledejte jiným zobrazením (rozložení, šablony, částečných zobrazení) hierarchicky, spouští se ve stejné složce jako aktuální stránka.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-255">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="0b0bd-256">Rozložení v *stránky* složky je možné z jakékoli stránky Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-256">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="0b0bd-257">Doporučujeme **není** vložit ho do rozložení *zobrazení/Shared* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-257">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="0b0bd-258">*Zobrazení/Shared* je vzorek zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-258">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="0b0bd-259">Spoléhat na hierarchii složek, ne cestu konvence jsou určené pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-259">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="0b0bd-260">Obsahuje zobrazení hledání ze stránky Razor *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-260">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="0b0bd-261">Rozložení, šablony a částečných zobrazení, které používáte s konvenčním Razor zobrazení a kontrolery MVC *jenom pracovní*.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-261">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="0b0bd-262">Přidat *Pages/_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-262">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="0b0bd-263">`@namespace` je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-263">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="0b0bd-264">`@addTagHelper` – Direktiva přináší [integrovaných pomocných rutin značek](xref:mvc/views/tag-helpers/builtin-th/Index) na všechny stránky v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-264">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="0b0bd-265">Když `@namespace` explicitně použít direktiva na stránce:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-265">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="0b0bd-266">Direktiva Nastaví obor názvů pro stránku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-266">The directive sets the namespace for the page.</span></span> <span data-ttu-id="0b0bd-267">`@model` – Direktiva není nutné, aby obsahoval obor názvů.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-267">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="0b0bd-268">Když `@namespace` – direktiva je obsažen v *_ViewImports.cshtml*, určený obor názvů poskytuje předponu pro obor názvů generovaného na stránce, která importuje `@namespace` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-268">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="0b0bd-269">Zbytek vygenerovaného oboru názvů (přípona) je relativní cesta mezi složku obsahující tečkou oddělená *_ViewImports.cshtml* a složku, která obsahuje na stránce.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-269">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="0b0bd-270">Například `PageModel` třídy *Pages/Customers/Edit.cshtml.cs* explicitně nastaví obor názvů:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-270">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="0b0bd-271">*Pages/_ViewImports.cshtml* souboru nastaví následující obor názvů:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-271">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="0b0bd-272">Vygenerovaný obor názvů pro *Pages/Customers/Edit.cshtml* stránky Razor je stejný jako `PageModel` třídy.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-272">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="0b0bd-273">`@namespace` *taky spolupracuje s konvenčním zobrazení syntaxe Razor.*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-273">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="0b0bd-274">Původní *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-274">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="0b0bd-275">Aktualizovaný *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-275">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="0b0bd-276">[Razor Pages starter projektu](#rpvs17) obsahuje *Pages/_ValidationScriptsPartial.cshtml*, který zachytí nahoru ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-276">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="0b0bd-277">Generování adresy URL pro stránky</span><span class="sxs-lookup"><span data-stu-id="0b0bd-277">URL generation for Pages</span></span>

<span data-ttu-id="0b0bd-278">`Create` Stránky uvedenému výše používá `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="0b0bd-279">Aplikace má následující strukturu souboru nebo složky:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="0b0bd-280">*/ Stránky*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-280">*/Pages*</span></span>

  * <span data-ttu-id="0b0bd-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="0b0bd-282">*/ Zákazníků*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-282">*/Customers*</span></span>

    * <span data-ttu-id="0b0bd-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="0b0bd-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="0b0bd-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-285">*Index.cshtml*</span></span>

<span data-ttu-id="0b0bd-286">*Pages/Customers/Create.cshtml* a *Pages/Customers/Edit.cshtml* stránek přesměrování na *Pages/Index.cshtml* po úspěšném provedení.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="0b0bd-287">Řetězec `/Index` je součástí identifikátoru URI pro přístup k předchozí stránce.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="0b0bd-288">Řetězec `/Index` slouží ke generování identifikátorů URI pro *Pages/Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="0b0bd-289">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="0b0bd-290">Název stránky je cesta ke stránce z kořenového adresáře */stránky* složku včetně úvodní `/` (například `/Index`).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="0b0bd-291">Předchozí generace vzorky adresy URL umožňují rozšířené možnosti a funkční možnosti nad hardcoding adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="0b0bd-292">Generování adresy URL používá [směrování](xref:mvc/controllers/routing) a může generovat a kódování parametry podle v cílové cestě bude definován takhle trasy.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="0b0bd-293">Generování adresy URL pro stránky podporuje relativních názvů.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="0b0bd-294">V následující tabulce jsou uvedeny je vybrána na stránce, které Index s různými `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="0b0bd-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="0b0bd-295">RedirectToPage(x)</span></span>| <span data-ttu-id="0b0bd-296">Stránka</span><span class="sxs-lookup"><span data-stu-id="0b0bd-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="0b0bd-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="0b0bd-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="0b0bd-298">*Stránky/indexu*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-298">*Pages/Index*</span></span> |
| <span data-ttu-id="0b0bd-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="0b0bd-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="0b0bd-300">*Stránky/zákazníci/indexu*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="0b0bd-301">RedirectToPage(".. / Index")</span><span class="sxs-lookup"><span data-stu-id="0b0bd-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="0b0bd-302">*Stránky/indexu*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-302">*Pages/Index*</span></span> |
| <span data-ttu-id="0b0bd-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="0b0bd-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="0b0bd-304">*Stránky/zákazníci/indexu*</span><span class="sxs-lookup"><span data-stu-id="0b0bd-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="0b0bd-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, a `RedirectToPage("../Index")` jsou <em>relativní názvy</em>.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="0b0bd-306">`RedirectToPage` Parametr <em>kombinovat</em> s cestou k aktuální stránky pro výpočet název cílové stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-306">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="0b0bd-307">Relativní název propojení je užitečné, když vytváření webů s komplexní strukturou.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="0b0bd-308">Pokud používáte relativní názvy propojení mezi stránkami ve složce, můžete přejmenovat tuto složku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="0b0bd-309">Všechny odkazy i nadále fungovat (vzhledem k tomu patří mezi ně neměli název složky).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="0b0bd-310">Atribut viewData</span><span class="sxs-lookup"><span data-stu-id="0b0bd-310">ViewData attribute</span></span>

<span data-ttu-id="0b0bd-311">Data mohou být předána na stránku s [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-311">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="0b0bd-312">Upravené vlastnosti v kontrolerech a modelech stránky Razor pomocí `[ViewData]` hodnoty uložené a načíst z [objektu ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-312">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="0b0bd-313">V následujícím příkladu `AboutModel` obsahuje `Title` vlastnost upravené pomocí `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-313">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="0b0bd-314">`Title` Je nastavena na název stránky o:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-314">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="0b0bd-315">Na stránce o přístup `Title` vlastnost jako vlastnost modelu:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-315">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="0b0bd-316">Název je v rozložení pro čtení ze slovníku ViewData:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-316">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="0b0bd-317">TempData</span><span class="sxs-lookup"><span data-stu-id="0b0bd-317">TempData</span></span>

<span data-ttu-id="0b0bd-318">Zpřístupňuje ASP.NET Core [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnosti [řadič](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-318">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="0b0bd-319">Tato vlastnost ukládá data, dokud je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-319">This property stores data until it's read.</span></span> <span data-ttu-id="0b0bd-320">`Keep` a `Peek` metody můžete použít k prozkoumání dat bez potřeby jejich odstranění.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-320">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="0b0bd-321">`TempData` je užitečné pro přesměrování, když je potřeba data pro více než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-321">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="0b0bd-322">`[TempData]` Atribut je nového v ASP.NET Core 2.0 a je podporován v řadiče a stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-322">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="0b0bd-323">Následující kód nastaví hodnotu `Message` pomocí `TempData`:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-323">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="0b0bd-324">Následující kód v *Pages/Customers/Index.cshtml* souboru zobrazí hodnotu `Message` pomocí `TempData`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-324">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="0b0bd-325">*Pages/Customers/Index.cshtml.cs* modelu stránka se vztahuje `[TempData]` atribut `Message` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-325">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="0b0bd-326">Zobrazit [TempData](xref:fundamentals/app-state#tempdata) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-326">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="0b0bd-327">Na stránce více obslužných rutin</span><span class="sxs-lookup"><span data-stu-id="0b0bd-327">Multiple handlers per page</span></span>

<span data-ttu-id="0b0bd-328">Na následující stránce generuje kód pro dvě stránky obslužné rutiny `asp-page-handler` pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-328">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="0b0bd-329">Formuláře v předchozím příkladu má dva odeslání tlačítka, každý použití `FormActionTagHelper` pro odeslání do jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-329">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="0b0bd-330">`asp-page-handler` Atribut, který je k `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-330">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="0b0bd-331">`asp-page-handler` generuje adresy URL, které odesílají na jednotlivé metody obslužné rutiny, které jsou definované na stránce.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-331">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="0b0bd-332">`asp-page` není zadaný, protože vzorku je odkazování na aktuální stránce.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-332">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="0b0bd-333">Model stránky:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-333">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="0b0bd-334">Předchozí kód používá *s názvem metody obslužné rutiny*.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-334">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="0b0bd-335">Metody s názvem obslužné rutiny jsou vytvořeny pomocí textu v názvu po `On<HTTP Verb>` a před `Async` (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-335">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="0b0bd-336">V předchozím příkladu jsou metody stránky OnPost**JoinList**Async a OnPost**JoinListUC**asynchronní.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-336">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="0b0bd-337">S *OnPost* a *asynchronní* odebrat, jsou názvy obslužných rutin `JoinList` a `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-337">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="0b0bd-338">Použití předcházející kódu, cesty URL, která odešle `OnPostJoinListAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-338">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="0b0bd-339">Cesta URL, která odešle `OnPostJoinListUCAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-339">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="0b0bd-340">Vlastní trasy</span><span class="sxs-lookup"><span data-stu-id="0b0bd-340">Custom routes</span></span>

<span data-ttu-id="0b0bd-341">Použití `@page` direktivu do:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-341">Use the `@page` directive to:</span></span>

* <span data-ttu-id="0b0bd-342">Zadejte vlastní trasu na stránku.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-342">Specify a custom route to a page.</span></span> <span data-ttu-id="0b0bd-343">Například lze nastavit trasy, která má na stránce o `/Some/Other/Path` s `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-343">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="0b0bd-344">Připojte segmenty na stránce výchozí trasu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-344">Append segments to a page's default route.</span></span> <span data-ttu-id="0b0bd-345">Například můžete přidat segmentu "item" výchozí trasu na stránce s `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-345">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="0b0bd-346">Na stránce výchozí trasu připojení parametrů.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-346">Append parameters to a page's default route.</span></span> <span data-ttu-id="0b0bd-347">Například pomocí parametru ID `id`, může být třeba stránka s `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-347">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="0b0bd-348">Relativní kořenové cesty určené tildou (`~`) se podporuje na začátku cesty.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-348">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="0b0bd-349">Například `@page "~/Some/Other/Path"` je stejný jako `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-349">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="0b0bd-350">Můžete změnit řetězec dotazu `?handler=JoinList` v adrese URL trasy segmentu `/JoinList` zadáním šablonu trasy `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-350">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="0b0bd-351">Pokud vám nevyhovují řetězec dotazu `?handler=JoinList` v adrese URL, můžete změnit trasy, které chcete změnit název obslužné rutiny na část cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-351">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="0b0bd-352">Trasy můžete přizpůsobit tak, že přidáte šablonu trasy, který je umístěn do dvojitých uvozovek po `@page` směrnice.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-352">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="0b0bd-353">Použití předcházející kódu, cesty URL, která odešle `OnPostJoinListAsync` je `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-353">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="0b0bd-354">Cesta URL, která odešle `OnPostJoinListUCAsync` je `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-354">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="0b0bd-355">`?` Následující `handler` znamená, že je parametr trasy nepovinný.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-355">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="0b0bd-356">Konfigurace a nastavení</span><span class="sxs-lookup"><span data-stu-id="0b0bd-356">Configuration and settings</span></span>

<span data-ttu-id="0b0bd-357">Rozšířené možnosti nakonfigurujete, použijte metodu rozšíření `AddRazorPagesOptions` na tvůrce MVC:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-357">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="0b0bd-358">Nyní můžete použít `RazorPagesOptions` nastavit kořenový adresář pro stránky, nebo přidat aplikaci modelu konvence pro stránky.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-358">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="0b0bd-359">Jsme budou moci další rozšíření takto v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-359">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="0b0bd-360">Předkompilace zobrazení, najdete v článku [kompilace zobrazení Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="0b0bd-360">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="0b0bd-361">[Stažení nebo zobrazení vzorového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="0b0bd-361">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="0b0bd-362">Zobrazit [Začínáme se stránkami Razor](xref:tutorials/razor-pages/razor-pages-start), které navazuje na tento úvod.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-362">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="0b0bd-363">Určete, že pro stránky Razor se v kořenovém adresáři obsahu</span><span class="sxs-lookup"><span data-stu-id="0b0bd-363">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="0b0bd-364">Ve výchozím nastavení, je integrován Razor Pages */stránky* adresáře.</span><span class="sxs-lookup"><span data-stu-id="0b0bd-364">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="0b0bd-365">Přidat [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se v kořenovém adresáři obsahu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikace:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-365">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="0b0bd-366">Určit, že Razor Pages na vlastní kořenový adresář</span><span class="sxs-lookup"><span data-stu-id="0b0bd-366">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="0b0bd-367">Přidat [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že jsou Razor Pages na vlastní kořenovým adresářem v aplikaci (zadejte relativní cestu):</span><span class="sxs-lookup"><span data-stu-id="0b0bd-367">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="0b0bd-368">Viz také:</span><span class="sxs-lookup"><span data-stu-id="0b0bd-368">See also</span></span>

* [<span data-ttu-id="0b0bd-369">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0bd-369">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="0b0bd-370">Syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-370">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="0b0bd-371">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-371">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="0b0bd-372">Konvence autorizace stránek Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-372">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="0b0bd-373">Razor stránek vlastní trasy a stránky model poskytovatele</span><span class="sxs-lookup"><span data-stu-id="0b0bd-373">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* [<span data-ttu-id="0b0bd-374">Testy jednotek stránek Razor</span><span class="sxs-lookup"><span data-stu-id="0b0bd-374">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
