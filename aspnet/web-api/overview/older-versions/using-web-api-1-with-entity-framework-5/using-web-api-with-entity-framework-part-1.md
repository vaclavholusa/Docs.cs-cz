---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Část 1: Přehled a vytváření projektu | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873323"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="e9ddf-102">Část 1: Přehled a vytváření projektu</span><span class="sxs-lookup"><span data-stu-id="e9ddf-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="e9ddf-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9ddf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e9ddf-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="e9ddf-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="e9ddf-105">Představuje objekt nebo relační mapování rozhraní je rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="e9ddf-106">Mapuje domény objekty v kódu na entity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="e9ddf-107">Ve většině případů nemáte si dělat starosti v databázové vrstvě, protože rozhraní Entity Framework to postará za vás.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="e9ddf-108">Kódu umožňuje pracovat s objekty a změny jsou ukládány do databáze.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="e9ddf-109">O kurzu</span><span class="sxs-lookup"><span data-stu-id="e9ddf-109">About the Tutorial</span></span>

<span data-ttu-id="e9ddf-110">V tomto kurzu vytvoříte aplikaci jednoduché úložiště.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="e9ddf-111">Existují dvě hlavní části k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-111">There are two main parts to the application.</span></span> <span data-ttu-id="e9ddf-112">Normální uživatelé mohou zobrazit produkty nebo vytvořit objednávky:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="e9ddf-113">Správci mohou vytvořit, odstranit nebo upravit produkty:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="e9ddf-114">Dovedností, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e9ddf-114">Skills You'll Learn</span></span>

<span data-ttu-id="e9ddf-115">Zde je, co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="e9ddf-116">Jak používat rozhraní Entity Framework s rozhraním ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="e9ddf-117">Jak používat knockout.js vytvořit dynamické uživatelské rozhraní klienta.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="e9ddf-118">Jak používat ověřování pomocí formulářů pomocí webového rozhraní API k ověřování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="e9ddf-119">I když v tomto kurzu je samostatný, můžete chtít nejdřív přečíst následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="e9ddf-120">První rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e9ddf-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="e9ddf-121">Vytvoření rozhraní Web API podporujícího operace CRUD</span><span class="sxs-lookup"><span data-stu-id="e9ddf-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="e9ddf-122">Některé znalosti [ASP.NET MVC](../../../../mvc/index.md) je také užitečné.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="e9ddf-123">Přehled</span><span class="sxs-lookup"><span data-stu-id="e9ddf-123">Overview</span></span>

<span data-ttu-id="e9ddf-124">Na vysoké úrovni zde je architektura aplikace:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="e9ddf-125">ASP.NET MVC vygeneruje stránky HTML pro klienta.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="e9ddf-126">Rozhraní ASP.NET Web API zpřístupní operace CRUD na data (produkty a objednávky).</span><span class="sxs-lookup"><span data-stu-id="e9ddf-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="e9ddf-127">Rozhraní Entity Framework překládá používané webového rozhraní API do entity databáze modely C#.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="e9ddf-128">Následující diagram znázorňuje zastoupení objektů domény v různých vrstev aplikace: vrstva databáze, v objektovém modelu a nakonec přenosový formát, který se používá k přenosu dat do klienta prostřednictvím protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="e9ddf-129">Vytvoření projektu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9ddf-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="e9ddf-130">Můžete vytvořit kurz projekt pomocí Visual Web Developer Express nebo si úplnou verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="e9ddf-131">Z **spustit** klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="e9ddf-132">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e9ddf-133">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e9ddf-134">V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e9ddf-135">Název projektu "ProductStore" a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="e9ddf-136">V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **Internetové aplikace** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="e9ddf-137">Šablona "Internet aplikací" vytvoří aplikaci ASP.NET MVC, která podporuje ověřování pomocí formulářů.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="e9ddf-138">Pokud nyní aplikaci spustíte, už je některé funkce:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="e9ddf-139">Nové uživatelé mohou registrovat kliknutím na odkaz "Register" v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="e9ddf-140">Registrovaní uživatelé se můžou přihlásit kliknutím na odkaz "Přihlášení".</span><span class="sxs-lookup"><span data-stu-id="e9ddf-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="e9ddf-141">Informace o členství uchovávána v databázi, která je vytvořena automaticky.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="e9ddf-142">Další informace o ověřování pomocí formulářů v architektuře ASP.NET MVC najdete v tématu [návod: použití ověřování pomocí formulářů v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="e9ddf-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="e9ddf-143">Aktualizovat soubor CSS</span><span class="sxs-lookup"><span data-stu-id="e9ddf-143">Update the CSS File</span></span>

<span data-ttu-id="e9ddf-144">Tento krok je kosmetické, ale bude stránky vykreslení jako starší snímky obrazovky.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="e9ddf-145">V Průzkumníku řešení rozbalte složku obsahu a otevřete soubor s názvem Site.css.</span><span class="sxs-lookup"><span data-stu-id="e9ddf-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="e9ddf-146">Přidejte následující stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="e9ddf-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="e9ddf-147">Next</span><span class="sxs-lookup"><span data-stu-id="e9ddf-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
