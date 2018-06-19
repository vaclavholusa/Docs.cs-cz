---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Část 4: Přidání zobrazení správce | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879527"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="1222b-102">Část 4: Přidání zobrazení správce</span><span class="sxs-lookup"><span data-stu-id="1222b-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="1222b-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1222b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1222b-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="1222b-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="1222b-105">Přidání zobrazení správce</span><span class="sxs-lookup"><span data-stu-id="1222b-105">Add an Admin View</span></span>

<span data-ttu-id="1222b-106">Nyní jsme budete zapnout na straně klienta a přidání stránky, který můžete využívat data ze správce řadiče.</span><span class="sxs-lookup"><span data-stu-id="1222b-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="1222b-107">Stránce umožní uživatelům vytvořit, upravit nebo odstranit produkty, pomocí zasílání požadavků AJAX do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1222b-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="1222b-108">V Průzkumníku řešení rozbalte složku řadiče a otevřete soubor s názvem HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="1222b-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="1222b-109">Tento soubor obsahuje kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="1222b-109">This file contains an MVC controller.</span></span> <span data-ttu-id="1222b-110">Přidejte metodu s názvem `Admin`:</span><span class="sxs-lookup"><span data-stu-id="1222b-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="1222b-111">**HttpRouteUrl** metoda vytvoří identifikátor URI webové rozhraní API, a to uložíme v kontejner zobrazení pro pozdější.</span><span class="sxs-lookup"><span data-stu-id="1222b-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="1222b-112">V dalším kroku umístěte kurzor text v rámci `Admin` metody akce, poté klikněte pravým tlačítkem a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1222b-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="1222b-113">Tím se otevře **přidat zobrazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1222b-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="1222b-114">V **přidat zobrazení** dialogové okno, název zobrazení "Admin".</span><span class="sxs-lookup"><span data-stu-id="1222b-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="1222b-115">Zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**.</span><span class="sxs-lookup"><span data-stu-id="1222b-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="1222b-116">V části **třídy modelu**, vyberte "Produkt (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="1222b-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="1222b-117">Všechny ostatní možnosti ponechte jako výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1222b-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="1222b-118">Kliknutím na tlačítko **přidat** přidá soubor s názvem Admin.cshtml pod Domů zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1222b-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="1222b-119">Umožňuje otevřít tento soubor a přidejte následující kód HTML.</span><span class="sxs-lookup"><span data-stu-id="1222b-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="1222b-120">Tento HTML definuje strukturu stránce, ale žádné funkce ještě drátové nahoru.</span><span class="sxs-lookup"><span data-stu-id="1222b-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="1222b-121">Vytvoření odkazu na stránku Správce</span><span class="sxs-lookup"><span data-stu-id="1222b-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="1222b-122">V Průzkumníku řešení rozbalte složku, zobrazení a poté rozbalte složku sdílené.</span><span class="sxs-lookup"><span data-stu-id="1222b-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="1222b-123">Otevřete soubor s názvem \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="1222b-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="1222b-124">Vyhledejte **ul** element s id = "nabídka" a odkaz akce pro zobrazení správce:</span><span class="sxs-lookup"><span data-stu-id="1222b-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="1222b-125">V ukázkovém projektu byly provedeny několik dalších kosmetická změny, jako je například nahraďte řetězec "Zde bude vaše logo".</span><span class="sxs-lookup"><span data-stu-id="1222b-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="1222b-126">Tyto neovlivňují funkci aplikace.</span><span class="sxs-lookup"><span data-stu-id="1222b-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="1222b-127">Můžete stáhnout na projekt a porovnání souborů.</span><span class="sxs-lookup"><span data-stu-id="1222b-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="1222b-128">Spusťte aplikaci a klikněte na odkaz "Admin", který se zobrazí v horní části na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="1222b-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="1222b-129">Stránka Správce by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="1222b-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="1222b-130">Pravé teď stránce nic se neděje.</span><span class="sxs-lookup"><span data-stu-id="1222b-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="1222b-131">V další části použijeme Knockout.js vytvářet dynamické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1222b-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="1222b-132">Přidání ověřování</span><span class="sxs-lookup"><span data-stu-id="1222b-132">Add Authorization</span></span>

<span data-ttu-id="1222b-133">Stránky pro správu je nyní k dispozici všem uživatelům, kteří navštěvují webu.</span><span class="sxs-lookup"><span data-stu-id="1222b-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="1222b-134">Umožňuje změnit to omezit oprávnění na správce.</span><span class="sxs-lookup"><span data-stu-id="1222b-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="1222b-135">Začněte přidáním role "Správce" a uživatel s oprávněním správce.</span><span class="sxs-lookup"><span data-stu-id="1222b-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="1222b-136">V Průzkumníku řešení rozbalte složku filtry a otevřete soubor s názvem InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="1222b-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="1222b-137">Vyhledejte `SimpleMembershipInitializer` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="1222b-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="1222b-138">Po zavolání **WebSecurity.InitializeDatabaseConnection**, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="1222b-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="1222b-139">Toto je quick-and-dirty způsob, jak přidat roli "Správce" a vytvořte uživatele pro roli.</span><span class="sxs-lookup"><span data-stu-id="1222b-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="1222b-140">V Průzkumníku řešení rozbalte složku řadiče a otevřete soubor HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="1222b-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="1222b-141">Přidat **Authorize** atribut `Admin` metoda.</span><span class="sxs-lookup"><span data-stu-id="1222b-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="1222b-142">Otevřete soubor AdminController.cs a přidejte **Authorize** atribut celý `AdminController` třídy.</span><span class="sxs-lookup"><span data-stu-id="1222b-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="1222b-143">MVC a webového rozhraní API, jak definovat **Autorizovat** atributy v různých oborech názvů.</span><span class="sxs-lookup"><span data-stu-id="1222b-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="1222b-144">Aplikace MVC používá **System.Web.Mvc.AuthorizeAttribute**, zatímco webového rozhraní API používá **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="1222b-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="1222b-145">Pouze správci mohou nyní zobrazit stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="1222b-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="1222b-146">Navíc při odeslání požadavku HTTP k řadiči pro správu, žádost musí obsahovat soubor cookie ověřování.</span><span class="sxs-lookup"><span data-stu-id="1222b-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="1222b-147">Pokud ne, server odešle odpověď HTTP 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="1222b-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="1222b-148">Můžete to vidět v aplikaci Fiddler odesíláním žádosti GET za účelem `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="1222b-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1222b-149">[Předchozí](using-web-api-with-entity-framework-part-3.md)
> [další](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="1222b-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
