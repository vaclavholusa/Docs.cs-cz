---
title: "Zkoumání podrobnosti a odstraňte metody"
author: rick-anderson
description: "Metoda kontroleru podrobnosti a zobrazení v základní aplikaci ASP.NET MVC jádra."
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 79eb352082da23400403ff8804d724256a2d3f07
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="e42af-103">Zkoumání podrobnosti a odstraňte metody</span><span class="sxs-lookup"><span data-stu-id="e42af-103">Examining the Details and Delete methods</span></span>

<span data-ttu-id="e42af-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e42af-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e42af-105">Otevřete řadičem film a prověřit `Details` metoda:</span><span class="sxs-lookup"><span data-stu-id="e42af-105">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="e42af-106">Modul generování uživatelského rozhraní MVC, který vytvořili této metodě akce přidá komentář zobrazující požadavek HTTP, která volá metodu.</span><span class="sxs-lookup"><span data-stu-id="e42af-106">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="e42af-107">V takovém případě je požadavek GET s tři segmenty adres URL, `Movies` řadiče, `Details` metoda a `id` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e42af-107">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="e42af-108">Tyto segmenty jsou definovány v odvolání *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e42af-108">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="e42af-109">EF usnadňuje vyhledání dat pomocí `SingleOrDefaultAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="e42af-109">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="e42af-110">Důležitou funkci zabezpečení integrovaná metoda je, že kód ověří, že metodu search nalezl film, než se pokusí provádět žádné kroky s ním.</span><span class="sxs-lookup"><span data-stu-id="e42af-110">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="e42af-111">Například se hacker může představovat chyby na webu tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` například `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film).</span><span class="sxs-lookup"><span data-stu-id="e42af-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="e42af-112">Pokud nebylo vyhledat null film, aplikace by vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="e42af-112">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="e42af-113">Zkontrolujte `Delete` a `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="e42af-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="e42af-114">Všimněte si, že `HTTP GET Delete` metoda nedojde k odstranění určený film, vrátí zobrazení filmu kde můžete odeslat (HttpPost) k odstranění.</span><span class="sxs-lookup"><span data-stu-id="e42af-114">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="e42af-115">Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádění operace upravit, vytvořit operaci, nebo jiná operace, která se mění data) otevře bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="e42af-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="e42af-116">`[HttpPost]` Metodu, která odstraňuje data jmenuje `DeleteConfirmed` umožnit metodu HTTP POST jedinečný podpis nebo název.</span><span class="sxs-lookup"><span data-stu-id="e42af-116">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="e42af-117">Níže jsou uvedeny podpisy dvou metod:</span><span class="sxs-lookup"><span data-stu-id="e42af-117">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="e42af-118">Modul CLR (CLR) vyžaduje přetížené metody jedinečný podpis (stejný název metody, ale jiné seznam parametrů).</span><span class="sxs-lookup"><span data-stu-id="e42af-118">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="e42af-119">Zde však dvě `Delete` metody – jeden pro GET--a jeden pro metodu POST, že mají stejný parametr podpis.</span><span class="sxs-lookup"><span data-stu-id="e42af-119">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="e42af-120">(I potřebují tak, aby přijímal jeden celé číslo jako parametr.)</span><span class="sxs-lookup"><span data-stu-id="e42af-120">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="e42af-121">Existují dva přístupy k tomuto problému, jedna je umožnit metody odlišné názvy.</span><span class="sxs-lookup"><span data-stu-id="e42af-121">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="e42af-122">To je mechanismus generování uživatelského rozhraní, které jste provedli v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e42af-122">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="e42af-123">Ale vzniká malé problému: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, normálně směrování nebude schopna nalézt dané metody.</span><span class="sxs-lookup"><span data-stu-id="e42af-123">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="e42af-124">Je řešení, najdete v příkladu, který je přidání `ActionName("Delete")` atribut `DeleteConfirmed` metoda.</span><span class="sxs-lookup"><span data-stu-id="e42af-124">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="e42af-125">Tento atribut provádí mapování pro směrování systému, aby se najít adresu URL, která zahrnuje /Delete/ pro požadavek POST `DeleteConfirmed` metoda.</span><span class="sxs-lookup"><span data-stu-id="e42af-125">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="e42af-126">Další běžné pracovní kolem pro metody, které mají stejné názvy a podpisy je uměle změnit podpis metodu POST zahrnout další (nepoužívané) parametr.</span><span class="sxs-lookup"><span data-stu-id="e42af-126">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="e42af-127">To je, co jsme to udělali v předchozím post při jsme přidali `notUsed` parametr.</span><span class="sxs-lookup"><span data-stu-id="e42af-127">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="e42af-128">Můžete to udělat samé sem pro `[HttpPost] Delete` metoda:</span><span class="sxs-lookup"><span data-stu-id="e42af-128">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="e42af-129">Publikování v Azure</span><span class="sxs-lookup"><span data-stu-id="e42af-129">Publish to Azure</span></span>

<span data-ttu-id="e42af-130">V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny, jak publikovat tuto aplikaci do Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e42af-130">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="e42af-131">Aplikace lze publikovat také z [příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="e42af-131">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="e42af-132">Děkujeme, že dokončení tento úvod do architektury ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="e42af-132">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="e42af-133">Děkujeme za jakékoli komentáře, která zůstanou.</span><span class="sxs-lookup"><span data-stu-id="e42af-133">We appreciate any comments you leave.</span></span> <span data-ttu-id="e42af-134">[Začínáme s MVC a EF základní](xref:data/ef-mvc/intro) je vynikající postupujte podle kroků až tento kurz.</span><span class="sxs-lookup"><span data-stu-id="e42af-134">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e42af-135">Předchozí</span><span class="sxs-lookup"><span data-stu-id="e42af-135">Previous</span></span>](validation.md)
