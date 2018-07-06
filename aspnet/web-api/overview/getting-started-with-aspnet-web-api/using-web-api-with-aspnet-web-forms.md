---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Použití rozhraní Web API s webovými formuláři ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840885"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="ed416-102">Použití rozhraní Web API s webovými formuláři ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ed416-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="ed416-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ed416-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ed416-104">Přestože je součástí balíčku rozhraní ASP.NET Web API ASP.NET MVC, je jednoduše přidávat webového rozhraní API pro tradiční aplikace webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ed416-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="ed416-105">Tento kurz vás provede kroky.</span><span class="sxs-lookup"><span data-stu-id="ed416-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="ed416-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="ed416-106">Overview</span></span>

<span data-ttu-id="ed416-107">Použití webového rozhraní API v aplikaci webových formulářů, existují dva hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="ed416-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="ed416-108">Přidání kontroleru webového rozhraní API, která je odvozena z **objektu ApiController** třídy.</span><span class="sxs-lookup"><span data-stu-id="ed416-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="ed416-109">Přidat do směrovací tabulky **aplikace\_Start** metoda.</span><span class="sxs-lookup"><span data-stu-id="ed416-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="ed416-110">Vytvořte projekt webových formulářů</span><span class="sxs-lookup"><span data-stu-id="ed416-110">Create a Web Forms Project</span></span>

<span data-ttu-id="ed416-111">Spusťte sadu Visual Studio a vyberte **nový projekt** z **Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="ed416-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ed416-112">Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ed416-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ed416-113">V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="ed416-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ed416-114">V části **Visual C#** vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="ed416-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ed416-115">V seznamu šablon projektu vyberte **aplikace webových formulářů ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="ed416-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="ed416-116">Zadejte název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed416-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="ed416-117">Vytvoření modelu a kontroler</span><span class="sxs-lookup"><span data-stu-id="ed416-117">Create the Model and Controller</span></span>

<span data-ttu-id="ed416-118">Tento kurz používá stejné třídy modelu a kontroler jako [Začínáme](tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ed416-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="ed416-119">Nejprve přidejte třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="ed416-119">First, add a model class.</span></span> <span data-ttu-id="ed416-120">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat třídu**.</span><span class="sxs-lookup"><span data-stu-id="ed416-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="ed416-121">Název třídy produktu a přidejte následující implementaci:</span><span class="sxs-lookup"><span data-stu-id="ed416-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="ed416-122">Kontroler Web API v dalším kroku přidejte do projektu., A *řadič* je objekt, který zpracovává požadavky HTTP pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ed416-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="ed416-123">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="ed416-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="ed416-124">Vyberte **přidat novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ed416-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="ed416-125">V části **nainstalované šablony**, rozbalte **Visual C#** a vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="ed416-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="ed416-126">Vyberte ze seznamu šablon **třída Kontroleru rozhraní Web API**.</span><span class="sxs-lookup"><span data-stu-id="ed416-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="ed416-127">Název kontroleru "ProductsController" a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ed416-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="ed416-128">**Přidat novou položku** průvodce vytvořit soubor s názvem ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="ed416-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="ed416-129">Odstranit metody, které jsou zahrnuty v průvodci a přidejte následující metody:</span><span class="sxs-lookup"><span data-stu-id="ed416-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="ed416-130">Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ed416-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="ed416-131">Přidat informace o směrování</span><span class="sxs-lookup"><span data-stu-id="ed416-131">Add Routing Information</span></span>

<span data-ttu-id="ed416-132">V dalším kroku přidáme trasu URI tak tento identifikátory URI formuláře &quot;/webové rozhraníAPI/produkty/&quot; se směrují do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="ed416-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="ed416-133">V **Průzkumníka řešení**, dvakrát klikněte pro otevření souboru modelu code-behind Global.asax.cs Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ed416-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="ed416-134">Přidejte následující **pomocí** příkazu.</span><span class="sxs-lookup"><span data-stu-id="ed416-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="ed416-135">Pak přidejte následující kód, který **aplikace\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="ed416-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="ed416-136">Další informace o směrovacích tabulek naleznete v tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ed416-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="ed416-137">Přidat AJAX na straně klienta</span><span class="sxs-lookup"><span data-stu-id="ed416-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="ed416-138">To je všechno, co potřebujete k vytvoření webového rozhraní API, které mohou klienti získat přístup.</span><span class="sxs-lookup"><span data-stu-id="ed416-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="ed416-139">Nyní Pojďme přidat stránku HTML, který používá jQuery pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ed416-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="ed416-140">Ujistěte se, že stránky předlohy (například *Site.Master*) zahrnuje `ContentPlaceHolder` s `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="ed416-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="ed416-141">Otevřete soubor Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="ed416-141">Open the file Default.aspx.</span></span> <span data-ttu-id="ed416-142">Jak je znázorněno, nahraďte často používaný text, který je v obsahu hlavní části:</span><span class="sxs-lookup"><span data-stu-id="ed416-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="ed416-143">V dalším kroku přidáte odkaz na zdrojový soubor jQuery v `HeaderContent` části:</span><span class="sxs-lookup"><span data-stu-id="ed416-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="ed416-144">Poznámka: Můžete snadno přidat odkaz na skript přetažením souboru z **Průzkumníka řešení** do okna editoru kódu.</span><span class="sxs-lookup"><span data-stu-id="ed416-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="ed416-145">Pod značku skriptu jQuery přidejte následující blok skriptu:</span><span class="sxs-lookup"><span data-stu-id="ed416-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="ed416-146">Po načtení dokumentu tento skript vytvoří požadavek AJAX &quot;produktů s rozhranímapi/&quot;.</span><span class="sxs-lookup"><span data-stu-id="ed416-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="ed416-147">Požadavek vrátí seznam produktů ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ed416-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="ed416-148">Skript přidá informace o produktu do tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="ed416-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="ed416-149">Při spuštění aplikace by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ed416-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
