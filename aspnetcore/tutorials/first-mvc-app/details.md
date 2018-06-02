---
title: Zkontrolujte podrobnosti a odstraňte metody aplikace ASP.NET Core
author: rick-anderson
description: Další informace o metodě řadiče podrobnosti a zobrazit v základní aplikaci ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: b392f956888a740a4a8c7c553996fc85ce63bd4b
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729633"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a><span data-ttu-id="92af8-103">Zkontrolujte podrobnosti a odstraňte metody aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92af8-103">Examine the Details and Delete methods of an ASP.NET Core app</span></span>

<span data-ttu-id="92af8-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92af8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="92af8-105">Otevřete řadičem film a prověřit `Details` metoda:</span><span class="sxs-lookup"><span data-stu-id="92af8-105">Open the Movie controller and examine the `Details` method:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="92af8-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="92af8-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="92af8-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="92af8-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end

<span data-ttu-id="92af8-108">Modul generování uživatelského rozhraní MVC, který vytvořili této metodě akce přidá komentář zobrazující požadavek HTTP, která volá metodu.</span><span class="sxs-lookup"><span data-stu-id="92af8-108">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="92af8-109">V takovém případě je požadavek GET s tři segmenty adres URL, `Movies` řadiče, `Details` metoda a `id` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="92af8-109">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="92af8-110">Tyto segmenty jsou definovány v odvolání *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="92af8-110">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="92af8-111">EF usnadňuje vyhledání dat pomocí `SingleOrDefaultAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="92af8-111">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="92af8-112">Důležitou funkci zabezpečení integrovaná metoda je, že kód ověří, že metodu search nalezl film, než se pokusí provádět žádné kroky s ním.</span><span class="sxs-lookup"><span data-stu-id="92af8-112">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="92af8-113">Například se hacker může představovat chyby na webu tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` například `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film).</span><span class="sxs-lookup"><span data-stu-id="92af8-113">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="92af8-114">Pokud nebylo vyhledat null film, aplikace by vyvolat výjimku.</span><span class="sxs-lookup"><span data-stu-id="92af8-114">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="92af8-115">Zkontrolujte `Delete` a `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="92af8-115">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="92af8-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="92af8-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="92af8-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="92af8-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end

<span data-ttu-id="92af8-118">Všimněte si, že `HTTP GET Delete` metoda nedojde k odstranění určený film, vrátí zobrazení filmu kde můžete odeslat (HttpPost) k odstranění.</span><span class="sxs-lookup"><span data-stu-id="92af8-118">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="92af8-119">Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádění operace upravit, vytvořit operaci, nebo jiná operace, která se mění data) otevře bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="92af8-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="92af8-120">`[HttpPost]` Metodu, která odstraňuje data jmenuje `DeleteConfirmed` umožnit metodu HTTP POST jedinečný podpis nebo název.</span><span class="sxs-lookup"><span data-stu-id="92af8-120">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="92af8-121">Níže jsou uvedeny podpisy dvou metod:</span><span class="sxs-lookup"><span data-stu-id="92af8-121">The two method signatures are shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="92af8-122">Modul CLR (CLR) vyžaduje přetížené metody jedinečný podpis (stejný název metody, ale jiné seznam parametrů).</span><span class="sxs-lookup"><span data-stu-id="92af8-122">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="92af8-123">Zde však dvě `Delete` metody – jeden pro GET--a jeden pro metodu POST, že mají stejný parametr podpis.</span><span class="sxs-lookup"><span data-stu-id="92af8-123">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="92af8-124">(I potřebují tak, aby přijímal jeden celé číslo jako parametr.)</span><span class="sxs-lookup"><span data-stu-id="92af8-124">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="92af8-125">Existují dva přístupy k tomuto problému, jedna je umožnit metody odlišné názvy.</span><span class="sxs-lookup"><span data-stu-id="92af8-125">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="92af8-126">To je mechanismus generování uživatelského rozhraní, které jste provedli v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="92af8-126">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="92af8-127">Ale vzniká malé problému: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, normálně směrování nebude schopna nalézt dané metody.</span><span class="sxs-lookup"><span data-stu-id="92af8-127">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="92af8-128">Je řešení, najdete v příkladu, který je přidání `ActionName("Delete")` atribut `DeleteConfirmed` metoda.</span><span class="sxs-lookup"><span data-stu-id="92af8-128">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="92af8-129">Tento atribut provádí mapování pro směrování systému, aby se najít adresu URL, která zahrnuje /Delete/ pro požadavek POST `DeleteConfirmed` metoda.</span><span class="sxs-lookup"><span data-stu-id="92af8-129">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="92af8-130">Další běžné pracovní kolem pro metody, které mají stejné názvy a podpisy je uměle změnit podpis metodu POST zahrnout další (nepoužívané) parametr.</span><span class="sxs-lookup"><span data-stu-id="92af8-130">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="92af8-131">To je, co jsme to udělali v předchozím post při jsme přidali `notUsed` parametr.</span><span class="sxs-lookup"><span data-stu-id="92af8-131">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="92af8-132">Můžete to udělat samé sem pro `[HttpPost] Delete` metoda:</span><span class="sxs-lookup"><span data-stu-id="92af8-132">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="92af8-133">Publikování v Azure</span><span class="sxs-lookup"><span data-stu-id="92af8-133">Publish to Azure</span></span>

<span data-ttu-id="92af8-134">V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny, jak publikovat tuto aplikaci do Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92af8-134">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="92af8-135">Aplikace lze publikovat také z [příkazového řádku](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="92af8-135">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="92af8-136">Předchozí</span><span class="sxs-lookup"><span data-stu-id="92af8-136">Previous</span></span>](validation.md)
