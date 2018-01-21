---
<span data-ttu-id="81af0-101">Title: Úvod do stránky Razor v ASP.NET Core Autor: popis Rick Anderson: kurz ASP.NET Core na stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-101">title: Introduction to Razor Pages in ASP.NET Core author: Rick-Anderson description: ASP.NET Core tutorial on Razor Pages.</span></span> <span data-ttu-id="81af0-102">Obsahuje základní MVC, ASP.NET Core 2.x, pro vývoj webů a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="81af0-102">Includes MVC Core, ASP.NET Core 2.x, introduction to web development, and Visual Studio 2017.</span></span>
<span data-ttu-id="81af0-103">je doc poskytuje přehled používání stránky Razor v ASP.NET Core k usnadnění vývoje zaměřené na stránce scénáře.</span><span class="sxs-lookup"><span data-stu-id="81af0-103">is doc provides an overview of using Razor Pages in ASP.NET Core to ease the development of page-focused scenarios.</span></span>
<span data-ttu-id="81af0-104">MS.Author: riande manager: wpickett ms.date: 12/09/2017 ms.topic: get-started-article ms.technology: aspnet ms.prod: asp.net core uid: mvc nebo razor stránky nebo index</span><span class="sxs-lookup"><span data-stu-id="81af0-104">ms.author: riande manager: wpickett ms.date: 09/12/2017 ms.topic: get-started-article ms.technology: aspnet ms.prod: asp.net-core uid: mvc/razor-pages/index</span></span>
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="81af0-105">Úvod do stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81af0-105">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="81af0-106">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="81af0-106">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="81af0-107">Stránky Razor je nová funkce rozhraní ASP.NET MVC jádra, která vytváří kódování zaměřené na stránce scénáře jednodušší a zvýšit produktivitu.</span><span class="sxs-lookup"><span data-stu-id="81af0-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="81af0-108">Pokud hledáte kurz, který používá Model-View-Controller přístup, najdete v části [Začínáme s ASP.NET MVC základní](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="81af0-108">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="81af0-109">Tento dokument obsahuje úvod do stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-109">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="81af0-110">Není návod krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="81af0-110">It's not a step by step tutorial.</span></span> <span data-ttu-id="81af0-111">Pokud zjistíte oddíly je obtížné postupujte podle, najdete v části [Začínáme s stránky Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="81af0-111">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="81af0-112">Jádro ASP.NET 2.0 požadavky</span><span class="sxs-lookup"><span data-stu-id="81af0-112">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="81af0-113">Nainstalujte [.NET Core](https://www.microsoft.com/net/core) 2.0.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="81af0-113">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="81af0-114">Pokud používáte Visual Studio, nainstalujte [Visual Studio](https://www.visualstudio.com/vs/) 2017 verze 15.3 nebo novější s následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="81af0-114">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="81af0-115">**ASP.NET a vývoje**</span><span class="sxs-lookup"><span data-stu-id="81af0-115">**ASP.NET and web development**</span></span>
* <span data-ttu-id="81af0-116">**Vývoj pro různé platformy .NET core**</span><span class="sxs-lookup"><span data-stu-id="81af0-116">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="81af0-117">Vytvoření projektu stránky Razor</span><span class="sxs-lookup"><span data-stu-id="81af0-117">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="81af0-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81af0-118">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="81af0-119">V tématu [Začínáme s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) podrobné pokyny o tom, jak vytvářet stránky Razor projektu pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81af0-119">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="81af0-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="81af0-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="81af0-121">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="81af0-121">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="81af0-122">Otevřete vygenerovaného *.csproj* soubor ze sady Visual Studio for Mac.</span><span class="sxs-lookup"><span data-stu-id="81af0-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="81af0-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="81af0-123">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="81af0-124">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="81af0-124">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="81af0-125">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="81af0-125">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="81af0-126">Spustit `dotnet new razor` z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="81af0-126">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="81af0-127">Stránky Razor</span><span class="sxs-lookup"><span data-stu-id="81af0-127">Razor Pages</span></span>

<span data-ttu-id="81af0-128">Stránky Razor je povolena v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="81af0-128">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="81af0-129">Mějte na paměti na základní stránce:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="81af0-129">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="81af0-130">Předchozí kód mnoho vypadá soubor zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-130">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="81af0-131">Čím je jiné je `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="81af0-131">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="81af0-132">`@page`Vytvoří soubor do akce MVC – to znamená, že ji zpracovává požadavky přímo, bez průchodu přes řadič.</span><span class="sxs-lookup"><span data-stu-id="81af0-132">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="81af0-133">`@page`musí být první direktivu Razor na stránce.</span><span class="sxs-lookup"><span data-stu-id="81af0-133">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="81af0-134">`@page`má vliv na chování jiných objektů, které Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-134">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="81af0-135">Podobná stránky, můžete použít `PageModel` třídy, je uvedené v následující dva soubory.</span><span class="sxs-lookup"><span data-stu-id="81af0-135">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="81af0-136">*Pages/Index2.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-136">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="81af0-137">*Pages/Index2.cshtml.cs* "" souboru kódu:</span><span class="sxs-lookup"><span data-stu-id="81af0-137">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="81af0-138">Podle konvence `PageModel` soubor třídy má stejný název jako soubor stránky Razor s *.cs* připojí.</span><span class="sxs-lookup"><span data-stu-id="81af0-138">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="81af0-139">Je třeba na předchozí stránku Razor *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="81af0-139">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="81af0-140">Soubor obsahující `PageModel` třída nazývá *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="81af0-140">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="81af0-141">Přidružení cesty adresy URL na stránky určuje umístění stránky v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="81af0-141">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="81af0-142">Následující tabulka uvádí stránky Razor cestu a odpovídající adresy URL:</span><span class="sxs-lookup"><span data-stu-id="81af0-142">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="81af0-143">Název souboru a cesty</span><span class="sxs-lookup"><span data-stu-id="81af0-143">File name and path</span></span>               | <span data-ttu-id="81af0-144">odpovídající adresy URL</span><span class="sxs-lookup"><span data-stu-id="81af0-144">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="81af0-145">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-145">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="81af0-146">`/`nebo`/Index`</span><span class="sxs-lookup"><span data-stu-id="81af0-146">`/` or `/Index`</span></span> |
| <span data-ttu-id="81af0-147">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-147">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="81af0-148">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-148">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="81af0-149">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-149">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="81af0-150">`/Store`nebo`/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="81af0-150">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="81af0-151">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="81af0-151">Notes:</span></span>

* <span data-ttu-id="81af0-152">Modul runtime vypadá pro stránky Razor soubory v *stránky* složky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="81af0-152">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="81af0-153">`Index`Když neobsahuje adresu URL stránky, je výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-153">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="81af0-154">Zápis základní formulář</span><span class="sxs-lookup"><span data-stu-id="81af0-154">Writing a basic form</span></span>

<span data-ttu-id="81af0-155">Funkce stránky Razor jsou navrženy tak, aby se používají u webových prohlížečů snadno běžných vzorů.</span><span class="sxs-lookup"><span data-stu-id="81af0-155">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="81af0-156">[Model vazby](xref:mvc/models/model-binding), [značky Pomocníci](xref:mvc/views/tag-helpers/intro)a všechny pomocné rutiny HTML *právě pracovní* s vlastnostmi definovaný ve třídě stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-156">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="81af0-157">Vezměte v úvahu stránky, který implementuje "kontaktujte nás" formuláři pro základní `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="81af0-157">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="81af0-158">Pro ukázky v tomto dokumentu `DbContext` je inicializován v [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) souboru.</span><span class="sxs-lookup"><span data-stu-id="81af0-158">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="81af0-159">Datový model:</span><span class="sxs-lookup"><span data-stu-id="81af0-159">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="81af0-160">Kontext databáze:</span><span class="sxs-lookup"><span data-stu-id="81af0-160">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="81af0-161">*Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="81af0-162">*Pages/Create.cshtml.cs* souboru kódu na pozadí pro zobrazení:</span><span class="sxs-lookup"><span data-stu-id="81af0-162">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="81af0-163">Podle konvence `PageModel` třídy se nazývá `<PageName>Model` a je v stejného oboru názvů jako stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="81af0-164">`PageModel` Třída umožňuje oddělení logiku stránky od jeho prezentaci.</span><span class="sxs-lookup"><span data-stu-id="81af0-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="81af0-165">Definuje stránky obslužné rutiny pro požadavky odeslané na stránku a data použitá k vykreslení stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="81af0-166">Toto oddělení vám umožní spravovat stránky závislosti prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection) a [testování částí](xref:testing/razor-pages-testing) stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-166">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="81af0-167">Na stránce `OnPostAsync` *metoda obslužná rutina*, která se spouští `POST` požadavků (když uživatel odešle formulář).</span><span class="sxs-lookup"><span data-stu-id="81af0-167">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="81af0-168">Můžete přidat metody obslužné rutiny pro všechny akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="81af0-168">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="81af0-169">Nejběžnější obslužné rutiny jsou:</span><span class="sxs-lookup"><span data-stu-id="81af0-169">The most common handlers are:</span></span>

* <span data-ttu-id="81af0-170">`OnGet`k chybě při inicializaci stavu potřebné pro stránku.</span><span class="sxs-lookup"><span data-stu-id="81af0-170">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="81af0-171">[OnGet](#OnGet) ukázka.</span><span class="sxs-lookup"><span data-stu-id="81af0-171">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="81af0-172">`OnPost`pro zpracování odesílání formuláře.</span><span class="sxs-lookup"><span data-stu-id="81af0-172">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="81af0-173">`Async` Přípona je nepovinný, ale se často používá podle konvence pro asynchronní funkce.</span><span class="sxs-lookup"><span data-stu-id="81af0-173">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="81af0-174">`OnPostAsync` Kód v předchozím příkladu bude vypadat podobně jako by za normálních okolností psaní v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="81af0-174">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="81af0-175">Předchozí kód je typické pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-175">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="81af0-176">Většina primitiv MVC jako [model vazby](xref:mvc/models/model-binding), [ověření](xref:mvc/models/validation), a jsou sdíleny výsledky akce.</span><span class="sxs-lookup"><span data-stu-id="81af0-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="81af0-177">Předchozí `OnPostAsync` metoda:</span><span class="sxs-lookup"><span data-stu-id="81af0-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="81af0-178">Základní tok `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="81af0-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="81af0-179">Zkontrolujte chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="81af0-179">Check for validation errors.</span></span>

*  <span data-ttu-id="81af0-180">Pokud nejsou žádné chyby, uložit data a přesměrování.</span><span class="sxs-lookup"><span data-stu-id="81af0-180">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="81af0-181">Pokud nejsou chyby, zobrazit stránku znovu s ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="81af0-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="81af0-182">Ověřování na straně klienta je stejný jako tradiční aplikací ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="81af0-182">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="81af0-183">V mnoha případech chyb při ověřování by být zjištěn v klientském počítači a nikdy odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="81af0-183">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="81af0-184">Při úspěšném zadání data `OnPostAsync` volání metod obslužné rutiny `RedirectToPage` Pomocná metoda vrátí instanci `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="81af0-184">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="81af0-185">`RedirectToPage`představuje novou výsledek akce, podobně jako `RedirectToAction` nebo `RedirectToRoute`, ale přizpůsobené pro stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-185">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="81af0-186">V předchozím příkladu, je přesměrován na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="81af0-186">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="81af0-187">`RedirectToPage`podrobně [generování adresy URL pro stránky](#url_gen) části.</span><span class="sxs-lookup"><span data-stu-id="81af0-187">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="81af0-188">Když odeslaného formuláře došlo k chybám ověřování, (které jsou předávány serveru),`OnPostAsync` volání metod obslužné rutiny `Page` metodu helper.</span><span class="sxs-lookup"><span data-stu-id="81af0-188">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="81af0-189">`Page`Vrací instanci třídy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="81af0-189">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="81af0-190">Vrácení `Page` je podobná jak akce v řadiče vrátit `View`.</span><span class="sxs-lookup"><span data-stu-id="81af0-190">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="81af0-191">`PageResult`Výchozí nastavení je <!-- Review  --> návratový typ pro obslužné rutiny metodu.</span><span class="sxs-lookup"><span data-stu-id="81af0-191">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="81af0-192">Obslužná rutina metodu, která vrátí `void` vykreslí stránku.</span><span class="sxs-lookup"><span data-stu-id="81af0-192">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="81af0-193">`Customer` Používá vlastnost `[BindProperty]` atribut se vyjádřit výslovný souhlas s vazbou modelu.</span><span class="sxs-lookup"><span data-stu-id="81af0-193">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="81af0-194">Stránky Razor, ve výchozím nastavení, vazbu vlastnosti jenom s příkazy nebo GET.</span><span class="sxs-lookup"><span data-stu-id="81af0-194">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="81af0-195">Vazba na vlastnosti může snížit množství kód, který máte k zápisu.</span><span class="sxs-lookup"><span data-stu-id="81af0-195">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="81af0-196">Vazba snižuje kódu pomocí stejné vlastnosti k vykreslení pole formuláře (`<input asp-for="Customer.Name" />`) a přijměte vstupu.</span><span class="sxs-lookup"><span data-stu-id="81af0-196">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="81af0-197">Domovská stránka (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="81af0-197">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="81af0-198">Kódu *Index.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-198">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="81af0-199">*Index.cshtml* soubor obsahuje následující kód k vytvoření odkazu pro úpravy pro každý kontakt:</span><span class="sxs-lookup"><span data-stu-id="81af0-199">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="81af0-200">[Pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) použít `asp-route-{value}` atribut pro vygenerování odkazu na stránku upravit.</span><span class="sxs-lookup"><span data-stu-id="81af0-200">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="81af0-201">Odkaz obsahuje data trasy s kontakt ID.</span><span class="sxs-lookup"><span data-stu-id="81af0-201">The link contains route data with the contact ID.</span></span> <span data-ttu-id="81af0-202">Například `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="81af0-202">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="81af0-203">*Pages/Edit.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-203">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="81af0-204">První řádek obsahuje `@page "{id:int}"` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="81af0-204">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="81af0-205">Omezení směrování`"{id:int}"` informuje stránky tak, aby přijímal požadavky na stránku, které obsahují `int` směrovat data.</span><span class="sxs-lookup"><span data-stu-id="81af0-205">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="81af0-206">Pokud požadavek na stránku neobsahuje data trasy, která lze převést na `int`, modul runtime vrátí chybu HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="81af0-206">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="81af0-207">*Pages/Edit.cshtml.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="81af0-208">*Index.cshtml* soubor zároveň obsahuje kód k vytvoření tlačítko Odstranit pro každý kontakt zákazníka:</span><span class="sxs-lookup"><span data-stu-id="81af0-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="81af0-209">Pokud tlačítko Odstranit je vykreslena ve formátu HTML, jeho `formaction` zahrnuje parametry:</span><span class="sxs-lookup"><span data-stu-id="81af0-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="81af0-210">Obraťte se na zákaznické ID určeného `asp-route-id` atribut.</span><span class="sxs-lookup"><span data-stu-id="81af0-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="81af0-211">`handler` Určeným elementem `asp-page-handler` atribut.</span><span class="sxs-lookup"><span data-stu-id="81af0-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="81af0-212">Tady je příklad vykreslené odstranění tlačítka s zákazník obraťte se na ID `1`:</span><span class="sxs-lookup"><span data-stu-id="81af0-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="81af0-213">Při výběru tlačítka, formuláře `POST` požadavek odeslán do serveru.</span><span class="sxs-lookup"><span data-stu-id="81af0-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="81af0-214">Podle konvence je vybraný název metody obslužné rutiny na základě hodnotu `handler` parametr podle schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="81af0-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="81af0-215">Protože `handler` je `delete` v tomto příkladu `OnPostDeleteAsync` metoda obslužná rutina se používá procesu `POST` požadavku.</span><span class="sxs-lookup"><span data-stu-id="81af0-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="81af0-216">Pokud `asp-page-handler` nastavena na jinou hodnotu, jako například `remove`, metodu obslužná rutina stránky s názvem `OnPostRemoveAsync` je vybrána.</span><span class="sxs-lookup"><span data-stu-id="81af0-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="81af0-217">`OnPostDeleteAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="81af0-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="81af0-218">Přijme `id` z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="81af0-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="81af0-219">Dotazuje databázi pro zákazníka kontaktu s `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="81af0-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="81af0-220">Pokud je nalezen kontaktování zákazníků, budou se odebrat ze seznamu kontaktů zákazníka.</span><span class="sxs-lookup"><span data-stu-id="81af0-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="81af0-221">Databáze se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="81af0-221">The database is updated.</span></span>
* <span data-ttu-id="81af0-222">Volání `RedirectToPage` přesměrovat na indexovou stránku kořenové (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="81af0-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="81af0-223">XSRF/proti útokům CSRF a stránky Razor</span><span class="sxs-lookup"><span data-stu-id="81af0-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="81af0-224">Nemáte psaní jakéhokoli kódu pro [antiforgery ověření](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="81af0-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="81af0-225">Antiforgery generování tokenů a ověření jsou automaticky součástí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="81af0-226">Pomocí stránky Razor rozložení, částečné., šablony a pomocníky značky</span><span class="sxs-lookup"><span data-stu-id="81af0-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="81af0-227">Stránky fungovat se všemi funkcemi zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="81af0-228">Rozložení, šablony, a částečné značka Pomocníci *soubor _ViewStart.cshtml*, *_ViewImports.cshtml* pracovní stejným způsobem, tak pro běžné zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="81af0-229">Tato stránka umožňuje declutter díky některé z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="81af0-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="81af0-230">Přidat [rozložení stránky](xref:mvc/views/layout) k *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="81af0-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="81af0-231">[Rozložení](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="81af0-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="81af0-232">Určuje rozložení každé stránce (Pokud stránce výslovný nesouhlas rozložení).</span><span class="sxs-lookup"><span data-stu-id="81af0-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="81af0-233">Importuje struktury HTML jako je JavaScript a šablon.</span><span class="sxs-lookup"><span data-stu-id="81af0-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="81af0-234">V tématu [rozložení stránky](xref:mvc/views/layout) Další informace.</span><span class="sxs-lookup"><span data-stu-id="81af0-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="81af0-235">[Rozložení](xref:mvc/views/layout#specifying-a-layout) je nastavena *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="81af0-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="81af0-236">**Poznámka:** rozložení je v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="81af0-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="81af0-237">Stránky vyhledejte další zobrazení (rozložení, šablony, částečné.) hierarchicky, spouštění ve stejné složce jako aktuální stránku.</span><span class="sxs-lookup"><span data-stu-id="81af0-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="81af0-238">Rozložení v *stránky* složky lze z libovolné stránky Razor pod *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="81af0-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="81af0-239">Doporučujeme **není** chápat rozložení souboru *zobrazení a sdílených* složky.</span><span class="sxs-lookup"><span data-stu-id="81af0-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="81af0-240">*Zobrazení a sdílených* je zobrazení vzor MVC.</span><span class="sxs-lookup"><span data-stu-id="81af0-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="81af0-241">Stránky Razor jsou určené spoléhají na hierarchii složek, není cesta konvence.</span><span class="sxs-lookup"><span data-stu-id="81af0-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="81af0-242">Obsahuje zobrazení vyhledávání ze stránky Razor *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="81af0-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="81af0-243">Rozložení, šablony a částečné používáte s řadiče MVC a konvenční zobrazení syntaxe Razor *právě pracovní*.</span><span class="sxs-lookup"><span data-stu-id="81af0-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="81af0-244">Přidat *Pages/_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="81af0-245">`@namespace`se vysvětluje dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="81af0-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="81af0-246">`@addTagHelper` – Direktiva přináší [předdefinované značky Pomocníci](xref:mvc/views/tag-helpers/builtin-th/Index) pro všechny stránky v *stránky* složky.</span><span class="sxs-lookup"><span data-stu-id="81af0-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="81af0-247">Když `@namespace` – direktiva se používá explicitně na stránce:</span><span class="sxs-lookup"><span data-stu-id="81af0-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="81af0-248">Direktiva Nastaví obor názvů pro stránku.</span><span class="sxs-lookup"><span data-stu-id="81af0-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="81af0-249">`@model` – Direktiva nemusí obsahovat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="81af0-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="81af0-250">Když `@namespace` – direktiva je součástí *_ViewImports.cshtml*, zadaný obor názvů poskytuje předponu pro obor názvů generovaným na stránce, který umožňuje importovat `@namespace` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="81af0-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="81af0-251">Zbytek vygenerovaného oboru názvů (část příponu) je relativní cesta mezi složku, která obsahuje oddělené tečkou *_ViewImports.cshtml* a složce obsahující danou stránku.</span><span class="sxs-lookup"><span data-stu-id="81af0-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="81af0-252">Například kódu na pozadí *Pages/Customers/Edit.cshtml.cs* explicitně nastaví obor názvů:</span><span class="sxs-lookup"><span data-stu-id="81af0-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="81af0-253">*Pages/_ViewImports.cshtml* souboru nastaví následující obor názvů:</span><span class="sxs-lookup"><span data-stu-id="81af0-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="81af0-254">Vygenerovaný obor názvů pro *Pages/Customers/Edit.cshtml* Razor stránky je stejná jako souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="81af0-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="81af0-255">`@namespace` – Direktiva v byla navržená tak, třídy C# přidán do projektu a kód generovaný stránky *právě pracovní* bez nutnosti přidání `@using` direktivy pro souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="81af0-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="81af0-256">**Poznámka:** `@namespace` funguje taky s konvenční zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="81af0-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="81af0-257">Původní *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="81af0-258">Aktualizovaný *Pages/Create.cshtml* zobrazení souboru:</span><span class="sxs-lookup"><span data-stu-id="81af0-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="81af0-259">[Projektu úvodní stránky Razor](#rpvs17) obsahuje *Pages/_ValidationScriptsPartial.cshtml*, který zachytí až ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="81af0-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="81af0-260">Generování adresy URL pro stránky</span><span class="sxs-lookup"><span data-stu-id="81af0-260">URL generation for Pages</span></span>

<span data-ttu-id="81af0-261">`Create` Stránky, která je uvedená dříve, používá `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="81af0-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="81af0-262">Aplikace má následující strukturu souboru nebo složky:</span><span class="sxs-lookup"><span data-stu-id="81af0-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="81af0-263">*/ Stránky*</span><span class="sxs-lookup"><span data-stu-id="81af0-263">*/Pages*</span></span>

  * <span data-ttu-id="81af0-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="81af0-265">*Nebo smlouvy zákazníka*</span><span class="sxs-lookup"><span data-stu-id="81af0-265">*/Customer*</span></span>

    * <span data-ttu-id="81af0-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="81af0-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="81af0-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="81af0-268">*Index.cshtml*</span></span>

<span data-ttu-id="81af0-269">*Pages/Customers/Create.cshtml* a *Pages/Customers/Edit.cshtml* stránky přesměrovat na *Pages/Index.cshtml* po úspěšné.</span><span class="sxs-lookup"><span data-stu-id="81af0-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="81af0-270">Řetězec `/Index` je součástí identifikátor URI k přístupu na předchozí stránku.</span><span class="sxs-lookup"><span data-stu-id="81af0-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="81af0-271">Řetězec `/Index` lze použít ke generování identifikátory URI k *Pages/Index.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="81af0-272">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81af0-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="81af0-273">Název stránky je cesta na stránku z kořenového adresáře */stránky* složky (včetně jako úvodní `/`, například `/Index`).</span><span class="sxs-lookup"><span data-stu-id="81af0-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="81af0-274">Předchozí ukázky generování adresy URL jsou mnohem víc bohaté funkce než jenom hardcoding adresu URL.</span><span class="sxs-lookup"><span data-stu-id="81af0-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="81af0-275">Generování adresy URL používá [směrování](xref:mvc/controllers/routing) a vygenerovat a kódování parametry podle jak trasy, která je definována v cílovou cestu.</span><span class="sxs-lookup"><span data-stu-id="81af0-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="81af0-276">Generování adresy URL pro stránky podporuje relativních názvů.</span><span class="sxs-lookup"><span data-stu-id="81af0-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="81af0-277">Následující tabulka uvádí, které Index vybrány jiné `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="81af0-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="81af0-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="81af0-278">RedirectToPage(x)</span></span>| <span data-ttu-id="81af0-279">Stránka</span><span class="sxs-lookup"><span data-stu-id="81af0-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="81af0-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="81af0-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="81af0-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="81af0-281">*Pages/Index*</span></span> |
| <span data-ttu-id="81af0-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="81af0-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="81af0-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="81af0-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="81af0-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="81af0-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="81af0-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="81af0-285">*Pages/Index*</span></span> |
| <span data-ttu-id="81af0-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="81af0-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="81af0-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="81af0-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="81af0-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, a `RedirectToPage("../Index")` jsou *relativních názvů*.</span><span class="sxs-lookup"><span data-stu-id="81af0-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="81af0-289">`RedirectToPage` Parametr *kombinaci* cestou k aktuální stránce k výpočtu název cílové stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-289">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="81af0-290">Relativní název propojení je užitečné, při vytváření lokalit se strukturou komplexní.</span><span class="sxs-lookup"><span data-stu-id="81af0-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="81af0-291">Pokud používáte relativních názvů propojení mezi stránkami ve složce, můžete přejmenovat této složky.</span><span class="sxs-lookup"><span data-stu-id="81af0-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="81af0-292">Všechny odkazy na i nadále fungovat, (protože jejich nezahrnuli název složky).</span><span class="sxs-lookup"><span data-stu-id="81af0-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="81af0-293">TempData</span><span class="sxs-lookup"><span data-stu-id="81af0-293">TempData</span></span>

<span data-ttu-id="81af0-294">ASP.NET Core zpřístupní [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnost [řadič](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="81af0-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="81af0-295">Tato vlastnost ukládá data, dokud je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="81af0-295">This property stores data until it is read.</span></span> <span data-ttu-id="81af0-296">`Keep` a `Peek` metody můžete použít k prozkoumání dat bez odstranění.</span><span class="sxs-lookup"><span data-stu-id="81af0-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="81af0-297">`TempData`jsou užitečné pro přesměrování, pokud se data potřebná pro více než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="81af0-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="81af0-298">`[TempData]` Atribut je nového v technologii ASP.NET 2.0 jádra a je podporovaná v řadičích a stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="81af0-299">Následující kód nastaví hodnotu `Message` pomocí `TempData`:</span><span class="sxs-lookup"><span data-stu-id="81af0-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="81af0-300">Následující kód v *Pages/Customers/Index.cshtml* souboru se zobrazí hodnota `Message` pomocí `TempData`.</span><span class="sxs-lookup"><span data-stu-id="81af0-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="81af0-301">*Pages/Customers/Index.cshtml.cs* souboru kódu se vztahuje `[TempData]` atribut `Message` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="81af0-301">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="81af0-302">V tématu [TempData](xref:fundamentals/app-state#temp) Další informace.</span><span class="sxs-lookup"><span data-stu-id="81af0-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="81af0-303">Více obslužné rutiny na stránce</span><span class="sxs-lookup"><span data-stu-id="81af0-303">Multiple handlers per page</span></span>

<span data-ttu-id="81af0-304">Na následující stránce generuje kód pro dvě stránky pomocí obslužné rutiny `asp-page-handler` pomocná značky:</span><span class="sxs-lookup"><span data-stu-id="81af0-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="81af0-305">Formuláře v předchozím příkladu má dva odeslání tlačítka, každý využívající `FormActionTagHelper` odeslat na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="81af0-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="81af0-306">`asp-page-handler` Atribut je Pomocníka pro `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="81af0-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="81af0-307">`asp-page-handler`generuje adresy URL, které odesílají do jednotlivých metod obslužné rutiny, které jsou definované na stránce.</span><span class="sxs-lookup"><span data-stu-id="81af0-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="81af0-308">`asp-page`není zadán, protože je ukázka propojení na aktuální stránce.</span><span class="sxs-lookup"><span data-stu-id="81af0-308">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="81af0-309">Soubor kódu:</span><span class="sxs-lookup"><span data-stu-id="81af0-309">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="81af0-310">Předchozí kód používá *s názvem metody obslužná rutina*.</span><span class="sxs-lookup"><span data-stu-id="81af0-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="81af0-311">Metody s názvem obslužné rutiny vytvářejí provedením text v názvu po `On<HTTP Verb>` a před `Async` (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="81af0-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="81af0-312">V předchozím příkladu jsou metody stránky OnPost**JoinList**Async a OnPost**JoinListUC**asynchronní.</span><span class="sxs-lookup"><span data-stu-id="81af0-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="81af0-313">S *OnPost* a *asynchronní* odebrat, jsou názvy obslužná rutina `JoinList` a `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="81af0-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="81af0-314">Použití předcházející code cesty URL, kterou odesílá `OnPostJoinListAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="81af0-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="81af0-315">Cesty URL, kterou odesílá `OnPostJoinListUCAsync` je `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="81af0-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="81af0-316">Přizpůsobení směrování</span><span class="sxs-lookup"><span data-stu-id="81af0-316">Customizing Routing</span></span>

<span data-ttu-id="81af0-317">Pokud chcete řetězec dotazu `?handler=JoinList` v adrese URL, můžete změnit trasy, která má název obslužné rutiny chápat část adresy obsahující cestu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="81af0-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="81af0-318">Trasy, která můžete přizpůsobit přidáním šablonu trasy v dvojitých uvozovkách po `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="81af0-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="81af0-319">Předchozí trasy vloží název obslužné rutiny cesty URL místo řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="81af0-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="81af0-320">`?` Následující `handler` znamená je volitelný parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="81af0-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="81af0-321">Můžete použít `@page` a přidejte další segmenty a parametry k postupu na stránce.</span><span class="sxs-lookup"><span data-stu-id="81af0-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="81af0-322">Ať je k dispozici je **připojí** trasu výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-322">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="81af0-323">Použití absolutní nebo virtuální cesta ke změně stránky trasy (například `"~/Some/Other/Path"`) není podporován.</span><span class="sxs-lookup"><span data-stu-id="81af0-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="81af0-324">Konfigurace a nastavení</span><span class="sxs-lookup"><span data-stu-id="81af0-324">Configuration and settings</span></span>

<span data-ttu-id="81af0-325">Nakonfigurujte rozšířené možnosti, pomocí metody rozšíření `AddRazorPagesOptions` na tvůrce MVC:</span><span class="sxs-lookup"><span data-stu-id="81af0-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="81af0-326">Teď můžete použít `RazorPagesOptions` nastavit kořenový adresář pro stránky, nebo přidejte aplikace model konvence pro stránky.</span><span class="sxs-lookup"><span data-stu-id="81af0-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="81af0-327">Jsme budete povolit další rozšíření tímto způsobem v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="81af0-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="81af0-328">Předkompilovat zobrazení, najdete v části [kompilace zobrazení syntaxe Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="81af0-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="81af0-329">[Stažení nebo zobrazení ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="81af0-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="81af0-330">V tématu [Začínáme s stránky Razor v ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), který je založený na tento úvod.</span><span class="sxs-lookup"><span data-stu-id="81af0-330">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="81af0-331">Zadejte, zda stránky Razor se v kořenovém adresáři obsahu</span><span class="sxs-lookup"><span data-stu-id="81af0-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="81af0-332">Ve výchozím nastavení jsou stránky Razor root v */stránky* adresáře.</span><span class="sxs-lookup"><span data-stu-id="81af0-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="81af0-333">Přidat [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se v kořenovém adresáři obsahu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikace:</span><span class="sxs-lookup"><span data-stu-id="81af0-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="81af0-334">Určit, že stránky Razor na vlastní kořenový adresář</span><span class="sxs-lookup"><span data-stu-id="81af0-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="81af0-335">Přidat [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) k [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) k určení, že vaše stránky Razor se vlastní kořenového adresáře v aplikaci (zadejte relativní cestu):</span><span class="sxs-lookup"><span data-stu-id="81af0-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="81af0-336">Viz také</span><span class="sxs-lookup"><span data-stu-id="81af0-336">See also</span></span>

* [<span data-ttu-id="81af0-337">Začínáme se stránkami Razor</span><span class="sxs-lookup"><span data-stu-id="81af0-337">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="81af0-338">Konvence autorizace stránky Razor</span><span class="sxs-lookup"><span data-stu-id="81af0-338">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="81af0-339">Syntaxe Razor stránky vlastní trasy a stránka zprostředkovatele modelu</span><span class="sxs-lookup"><span data-stu-id="81af0-339">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="81af0-340">Jednotka stránky Razor a testování integrace</span><span class="sxs-lookup"><span data-stu-id="81af0-340">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
