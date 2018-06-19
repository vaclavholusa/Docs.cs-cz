---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Použití rozhraní Web API s webovými formuláři ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
ms.locfileid: "26575854"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="2294f-102">Použití rozhraní Web API s webovými formuláři ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2294f-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="2294f-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2294f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2294f-104">I když ASP.NET MVC je součástí balíčku webového rozhraní API ASP.NET, je snadné přidání webového rozhraní API do tradiční aplikace webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2294f-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="2294f-105">Tento kurz vás provede kroky.</span><span class="sxs-lookup"><span data-stu-id="2294f-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="2294f-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="2294f-106">Overview</span></span>

<span data-ttu-id="2294f-107">Použití webového rozhraní API v aplikaci webových formulářů, existují dva hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="2294f-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="2294f-108">Přidání kontroleru webového rozhraní API, která je odvozena z **objektu ApiController** třídy.</span><span class="sxs-lookup"><span data-stu-id="2294f-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="2294f-109">Přidat tabulku směrování, aby **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="2294f-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="2294f-110">Vytvoření projektu webové formuláře</span><span class="sxs-lookup"><span data-stu-id="2294f-110">Create a Web Forms Project</span></span>

<span data-ttu-id="2294f-111">Spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky.</span><span class="sxs-lookup"><span data-stu-id="2294f-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2294f-112">Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="2294f-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2294f-113">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="2294f-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2294f-114">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="2294f-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2294f-115">V seznamu šablon projektu, vyberte **aplikaci webových formulářů ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="2294f-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="2294f-116">Zadejte název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2294f-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="2294f-117">Vytvoření modelu a řadiče</span><span class="sxs-lookup"><span data-stu-id="2294f-117">Create the Model and Controller</span></span>

<span data-ttu-id="2294f-118">Tento kurz používá stejný model a řadič třídy, jako [Začínáme](tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="2294f-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="2294f-119">Nejprve přidejte třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="2294f-119">First, add a model class.</span></span> <span data-ttu-id="2294f-120">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat třídu**.</span><span class="sxs-lookup"><span data-stu-id="2294f-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="2294f-121">Název třídy produktu a přidejte následující implementace:</span><span class="sxs-lookup"><span data-stu-id="2294f-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="2294f-122">Dál přidejte kontroleru webového rozhraní API se projektu, *řadič* je objekt, který zpracovává požadavky HTTP pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2294f-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="2294f-123">V **Průzkumníku**, klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="2294f-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="2294f-124">Vyberte **přidat novou položku**.</span><span class="sxs-lookup"><span data-stu-id="2294f-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="2294f-125">V části **nainstalovaných šablonách**, rozbalte položku **Visual C#** a vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="2294f-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="2294f-126">Zvolte ze seznamu šablon **třída Kontroleru rozhraní Web API**.</span><span class="sxs-lookup"><span data-stu-id="2294f-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="2294f-127">Název kontroleru "ProductsController" a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2294f-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="2294f-128">**Přidat novou položku** Průvodce vytvoří soubor s názvem ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="2294f-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="2294f-129">Odstranit metody, které průvodce součástí a přidejte následující metody:</span><span class="sxs-lookup"><span data-stu-id="2294f-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="2294f-130">Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="2294f-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="2294f-131">Přidání informací o směrování</span><span class="sxs-lookup"><span data-stu-id="2294f-131">Add Routing Information</span></span>

<span data-ttu-id="2294f-132">Potom přidáme trasu identifikátor URI, že identifikátory URI ve tvaru &quot;/api/produkty/&quot; jsou směrovány do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="2294f-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="2294f-133">V **Průzkumníku**, poklikejte na soubor Global.asax k otevření souboru kódu Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="2294f-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="2294f-134">Přidejte následující **pomocí** příkaz.</span><span class="sxs-lookup"><span data-stu-id="2294f-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="2294f-135">Pak přidejte následující kód, který **aplikace\_spustit** metoda:</span><span class="sxs-lookup"><span data-stu-id="2294f-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="2294f-136">Další informace o směrovacích tabulek najdete v tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2294f-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="2294f-137">Přidat AJAX na straně klienta</span><span class="sxs-lookup"><span data-stu-id="2294f-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="2294f-138">To je všechno je potřeba vytvořit webové rozhraní API, které mohou klienti získat přístup.</span><span class="sxs-lookup"><span data-stu-id="2294f-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="2294f-139">Nyní Pojďme přidat stránku HTML, který používá jQuery pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2294f-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="2294f-140">Zajistěte, aby vaše stránky předlohy (například *Site.Master*) zahrnuje `ContentPlaceHolder` s `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="2294f-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="2294f-141">Otevřete soubor Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="2294f-141">Open the file Default.aspx.</span></span> <span data-ttu-id="2294f-142">Jak je znázorněno, nahraďte často používaný text, který je v části hlavní obsahu:</span><span class="sxs-lookup"><span data-stu-id="2294f-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="2294f-143">V dalším kroku přidejte odkaz na zdrojový soubor jQuery v `HeaderContent` části:</span><span class="sxs-lookup"><span data-stu-id="2294f-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="2294f-144">Poznámka: Můžete snadno přidat odkaz na skript přetahování a vkládání souborů z **Průzkumníku řešení** do okna editoru kódu.</span><span class="sxs-lookup"><span data-stu-id="2294f-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="2294f-145">Pod značky script jQuery přidejte následující blok skriptu:</span><span class="sxs-lookup"><span data-stu-id="2294f-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="2294f-146">Po načtení dokumentu tento skript je požadavek AJAX &quot;rozhraní api nebo produkty&quot;.</span><span class="sxs-lookup"><span data-stu-id="2294f-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="2294f-147">Požadavek vrací seznam produktů ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="2294f-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="2294f-148">Skript přidá do tabulky HTML informace o produktu.</span><span class="sxs-lookup"><span data-stu-id="2294f-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="2294f-149">Při spuštění aplikace by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="2294f-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
