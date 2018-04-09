---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Vytvoření klienta JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-the-javascript-client"></a><span data-ttu-id="35b94-102">Vytvoření klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="35b94-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="35b94-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="35b94-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="35b94-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="35b94-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="35b94-105">V této části vytvoříte klienta pro aplikaci, pomocí HTML, JavaScript a [Knockout.js](http://knockoutjs.com/) knihovny.</span><span class="sxs-lookup"><span data-stu-id="35b94-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="35b94-106">Klientská aplikace jsme budete vytvářet ve fázích:</span><span class="sxs-lookup"><span data-stu-id="35b94-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="35b94-107">Zobrazuje seznam seznamů.</span><span class="sxs-lookup"><span data-stu-id="35b94-107">Showing a list of books.</span></span>
- <span data-ttu-id="35b94-108">Zobrazuje podrobnosti adresáře.</span><span class="sxs-lookup"><span data-stu-id="35b94-108">Showing a book detail.</span></span>
- <span data-ttu-id="35b94-109">Přidání nového seznamu.</span><span class="sxs-lookup"><span data-stu-id="35b94-109">Adding a new book.</span></span>

<span data-ttu-id="35b94-110">Knihovny Knockout využívá schéma Model-View-ViewModel (modelem MVVM):</span><span class="sxs-lookup"><span data-stu-id="35b94-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="35b94-111">**Modelu** je serverové reprezentace dat v doméně obchodní (v našem případ, knihy a autoři).</span><span class="sxs-lookup"><span data-stu-id="35b94-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="35b94-112">**Zobrazení** je prezentační vrstvy (HTML).</span><span class="sxs-lookup"><span data-stu-id="35b94-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="35b94-113">**Modelu zobrazení** se objekt jazyka JavaScript, která obsahuje modely.</span><span class="sxs-lookup"><span data-stu-id="35b94-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="35b94-114">Model zobrazení je abstrakce kód uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="35b94-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="35b94-115">Nemá žádné znalosti reprezentace HTML.</span><span class="sxs-lookup"><span data-stu-id="35b94-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="35b94-116">Místo toho představuje abstraktní zobrazení, funkce, jako &quot;seznam seznamů&quot;.</span><span class="sxs-lookup"><span data-stu-id="35b94-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="35b94-117">Zobrazení je vázané na data do modelu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="35b94-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="35b94-118">Aktualizace do modelu zobrazení, se automaticky promítnou v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="35b94-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="35b94-119">Model zobrazení také získá události ze zobrazení, například kliknutím na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="35b94-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="35b94-120">Tento přístup umožňuje snadno změnit rozložení a uživatelského rozhraní aplikace, protože vazby, můžete změnit bez přepisování žádný kód.</span><span class="sxs-lookup"><span data-stu-id="35b94-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="35b94-121">Například může zobrazit seznam položek, jako `<ul>`, poté ji později změnit na tabulku.</span><span class="sxs-lookup"><span data-stu-id="35b94-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="35b94-122">Přidání knihovny Knockout</span><span class="sxs-lookup"><span data-stu-id="35b94-122">Add the Knockout Library</span></span>

<span data-ttu-id="35b94-123">V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**.</span><span class="sxs-lookup"><span data-stu-id="35b94-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="35b94-124">Potom vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="35b94-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="35b94-125">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="35b94-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="35b94-126">Tento příkaz přidá Knockout soubory do složky skriptů.</span><span class="sxs-lookup"><span data-stu-id="35b94-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="35b94-127">Vytvoření modelu zobrazení</span><span class="sxs-lookup"><span data-stu-id="35b94-127">Create the View Model</span></span>

<span data-ttu-id="35b94-128">Přidejte soubor JavaScript s názvem app.js do složky skriptů.</span><span class="sxs-lookup"><span data-stu-id="35b94-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="35b94-129">(V Průzkumníku řešení klikněte pravým tlačítkem na složku skripty, vyberte **přidat**, pak vyberte **soubor JavaScript**.) Vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="35b94-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="35b94-130">V Knockout `observable` třída umožňuje datové vazby.</span><span class="sxs-lookup"><span data-stu-id="35b94-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="35b94-131">Při změně obsahu existuje zjištěný, upozorní lze zobrazit všechny ovládací prvky vázané na data, mohou sami aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="35b94-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="35b94-132">( `observableArray` Třída je pole verze *lze zobrazit*.) Naše model zobrazení začínat, má dva pozorovatelné objekty:</span><span class="sxs-lookup"><span data-stu-id="35b94-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="35b94-133">`books` obsahuje seznam knih.</span><span class="sxs-lookup"><span data-stu-id="35b94-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="35b94-134">`error` obsahuje chybovou zprávu, pokud selže volání AJAX.</span><span class="sxs-lookup"><span data-stu-id="35b94-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="35b94-135">`getAllBooks` Metoda provede volání AJAX získat seznam knih.</span><span class="sxs-lookup"><span data-stu-id="35b94-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="35b94-136">Pak vynutí výsledek do `books` pole.</span><span class="sxs-lookup"><span data-stu-id="35b94-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="35b94-137">`ko.applyBindings` Metoda je součástí knihovny Knockout.</span><span class="sxs-lookup"><span data-stu-id="35b94-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="35b94-138">Přebírá jako parametr modelu zobrazení a nastaví datové vazby.</span><span class="sxs-lookup"><span data-stu-id="35b94-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="35b94-139">Přidat sady skriptu</span><span class="sxs-lookup"><span data-stu-id="35b94-139">Add a Script Bundle</span></span>

<span data-ttu-id="35b94-140">Sdružování je funkce v technologii ASP.NET 4.5, která usnadňuje kombinovat nebo sady více souborů do jediného souboru.</span><span class="sxs-lookup"><span data-stu-id="35b94-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="35b94-141">Sdružování snižuje počet požadavků na server, což může zlepšit čas načítání stránky.</span><span class="sxs-lookup"><span data-stu-id="35b94-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="35b94-142">Otevřete soubor aplikace\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="35b94-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="35b94-143">Přidejte následující kód do metody RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="35b94-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="35b94-144">[Předchozí](part-5.md)
> [další](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="35b94-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
