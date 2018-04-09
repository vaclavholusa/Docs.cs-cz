---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Volání webového rozhraní API z Windows Phone 8 aplikace (C#) | Microsoft Docs
author: rmcmurray
description: Vytvořte úplného začátku do konce scénáře skládající se z aplikace ASP.NET Web API, která nabízí katalog Books do aplikace Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="ce0e5-103">Volání webového rozhraní API z aplikace pro Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="ce0e5-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="ce0e5-104">podle [Roberta Mcmurrayho](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="ce0e5-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="ce0e5-105">V tomto kurzu se dozvíte, jak vytvořit úplného začátku do konce scénáře skládající se z aplikace ASP.NET Web API, která nabízí katalog Books do aplikace Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="ce0e5-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="ce0e5-106">Overview</span></span>

<span data-ttu-id="ce0e5-107">RESTful služby, jako je ASP.NET Web API zjednodušit vytváření aplikací založené na protokolu HTTP pro vývojáře, protože poskytuje abstrakci Architektura aplikace na straně serveru a na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="ce0e5-108">Místo vytváření vlastní protokol založený na soket pro komunikaci, vývojáři webového rozhraní API jednoduše je potřeba publikovat nezbytných metod HTTP pro svou aplikaci (například: GET, POST, PUT, DELETE), a vývojáři aplikace klienta stačí využívat metody HTTP, které jsou nezbytné pro svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="ce0e5-109">V tomto kurzu začátku do konce se dozvíte, jak vytvořit následující projekty pomocí webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="ce0e5-110">V [první části tohoto kurzu](#STEP1), vytvoříte aplikaci ASP.NET Web API, která podporuje všechny operace vytvoření, čtení, aktualizace a odstranění (CRUD) ke správě adresáře katalogu.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="ce0e5-111">Tato aplikace bude používat [ukázkový soubor XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) z webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="ce0e5-112">V [druhé části tohoto kurzu](#STEP2), vytvoříte interaktivní aplikace Windows Phone 8, která načte data z vaší aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="ce0e5-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce0e5-113">Prerequisites</span></span>

- <span data-ttu-id="ce0e5-114">Visual Studio 2013 s Windows Phone 8 SDK nainstalován</span><span class="sxs-lookup"><span data-stu-id="ce0e5-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="ce0e5-115">Windows 8 nebo novější na 64bitovém systému s technologií Hyper-V nainstalována</span><span class="sxs-lookup"><span data-stu-id="ce0e5-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="ce0e5-116">Seznam Další požadavky naleznete v tématu *požadavky na systém* části na [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) stránce pro stažení.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="ce0e5-117">Pokud chcete k testování připojení mezi webového rozhraní API a Windows Phone 8 projekty v lokálním systému, budete muset postupujte podle pokynů *[připojení k webové rozhraní API aplikace v místním emulátoru Windows Phone 8 Počítač](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte svého testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="ce0e5-118">Krok 1: Vytvoření webového projektu pro knihkupectví rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ce0e5-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="ce0e5-119">Prvním krokem v tomto kurzu začátku do konce je vytvoření projektu webového rozhraní API, která podporuje všechny operace CRUD; Všimněte si, že přidáte projekt aplikace Windows Phone k tomuto řešení v [kroku 2](#STEP2) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="ce0e5-120">Otevřete **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="ce0e5-121">Klikněte na tlačítko **soubor**, pak **nové**a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="ce0e5-122">Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalovaná**, pak **šablony**, pak **Visual C#**a potom **Webové**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="ce0e5-123">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="ce0e5-124">Zvýrazněte **webové aplikace ASP.NET**, zadejte **knihkupectví** název projektu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="ce0e5-125">Když **nový projekt ASP.NET** se zobrazí dialogové okno, vyberte **webového rozhraní API** šablony a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="ce0e5-126">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="ce0e5-127">Když se otevře projekt webového rozhraní API, odeberte z projektu řadič ukázka:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="ce0e5-128">Rozbalte **řadiče** složky v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="ce0e5-129">Klikněte pravým tlačítkem myši **ValuesController.cs** souboru a potom klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="ce0e5-130">Klikněte na tlačítko **OK** po zobrazení výzvy potvrďte odstranění.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="ce0e5-131">Přidání dat XML do projektu webového rozhraní API. Tento soubor obsahuje obsah katalogu pro knihkupectví:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="ce0e5-132">Klikněte pravým tlačítkem myši **aplikace\_Data** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="ce0e5-133">Když **přidat novou položku** se zobrazí dialogové okno, zvýrazněte **souboru XML** šablony.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="ce0e5-134">Název souboru **Books.xml**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="ce0e5-135">Když **Books.xml** otevření souboru, nahraďte kód v souboru XML z ukázky **books.xml** soubor na webu MSDN:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="ce0e5-136">Uložte a zavřete soubor XML.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="ce0e5-137">Přidání modelu pro knihkupectví do projektu webového rozhraní API. Tento model obsahuje logiku pro knihkupectví aplikaci vytvořit, číst, aktualizace nebo odstranění (CRUD):</span><span class="sxs-lookup"><span data-stu-id="ce0e5-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="ce0e5-138">Klikněte pravým tlačítkem myši **modely** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="ce0e5-139">Když **přidat novou položku** se zobrazí dialogové okno, zadejte název souboru třída **BookDetails.cs**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="ce0e5-140">Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="ce0e5-141">Uložte a zavřete **BookDetails.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="ce0e5-142">Řadič pro knihkupectví přidejte do projektu webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="ce0e5-143">Klikněte pravým tlačítkem myši **řadiče** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **řadič**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="ce0e5-144">Když **přidat vygenerované uživatelské rozhraní** se zobrazí dialogové okno, zvýrazněte **webové rozhraní API 2 řadiče - prázdný**a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="ce0e5-145">Když **přidat kontroler** se zobrazí dialogové okno, názvu kontroleru **BooksController**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="ce0e5-146">Když **BooksController.cs** otevření souboru, nahraďte kód v souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="ce0e5-147">Uložte a zavřete **BooksController.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="ce0e5-148">Sestavení aplikace webového rozhraní API ke kontrole chyb.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="ce0e5-149">Krok 2: Přidání projektu Windows Phone 8 knihkupectví katalogu</span><span class="sxs-lookup"><span data-stu-id="ce0e5-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="ce0e5-150">Dalším krokem tohoto začátku do konce scénáře je vytvoření katalogu aplikací pro Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="ce0e5-151">Tato aplikace bude používat *Windows Phone Vycentrovat aplikace* šablonu pro výchozí uživatelské rozhraní a bude používat aplikace webového rozhraní API, který jste vytvořili v [kroku 1](#STEP1) tohoto kurzu jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="ce0e5-152">Klikněte pravým tlačítkem myši **knihkupectví** řešení v v Průzkumníku řešení klikněte **přidat**a potom **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="ce0e5-153">Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalovaná**, pak **Visual C#**a potom **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="ce0e5-154">Zvýrazněte **Windows Phone Vycentrovat aplikace**, zadejte **BookCatalog** pro název a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="ce0e5-155">Přidejte balíček Json.NET NuGet **BookCatalog** projektu:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="ce0e5-156">Klikněte pravým tlačítkem na **odkazy** pro **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="ce0e5-157">Když **spravovat balíčky NuGet** se zobrazí dialogové okno, rozbalte položku **Online** části a zvýraznit **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="ce0e5-158">Zadejte **Json.NET** do vyhledávání pole a klikněte na ikonu hledání.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="ce0e5-159">Zvýrazněte **Json.NET** ve výsledcích hledání a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="ce0e5-160">Po dokončení instalace, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="ce0e5-161">Přidat **BookDetails** modelu k **BookCatalog** projektu; obsahuje model obecné třídy pro knihkupectví:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="ce0e5-162">Klikněte pravým tlačítkem myši **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na **přidat**a potom klikněte na **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="ce0e5-163">Název nové složky **modely**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="ce0e5-164">Klikněte pravým tlačítkem myši **modely** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="ce0e5-165">Když **přidat novou položku** se zobrazí dialogové okno, zadejte název souboru třída **BookDetails.cs**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="ce0e5-166">Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="ce0e5-167">Uložte a zavřete **BookDetails.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="ce0e5-168">Aktualizace **MainViewModel.cs** třída zahrnout funkce pro komunikaci s aplikací pro knihkupectví webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="ce0e5-169">Rozbalte **ViewModels** složky v Průzkumníku řešení a pak dvojitým kliknutím **MainViewModel.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="ce0e5-170">Když **MainViewModel.cs** otevření souboru, nahraďte kód v souboru následující; Všimněte si, že budete muset aktualizovat hodnotu `apiUrl` konstantní s skutečná adresa URL webové rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="ce0e5-171">Uložte a zavřete **MainViewModel.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="ce0e5-172">Aktualizace **MainPage.xaml** soubor pro přizpůsobení název aplikace:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="ce0e5-173">Dvakrát klikněte **MainPage.xaml** soubor v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="ce0e5-174">Když **MainPage.xaml** otevření souboru, vyhledejte následující řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="ce0e5-175">Tyto řádky nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="ce0e5-176">Uložte a zavřete **MainPage.xaml** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="ce0e5-177">Aktualizace **DetailsPage.xaml** soubor pro přizpůsobení zobrazených položek:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="ce0e5-178">Dvakrát klikněte **DetailsPage.xaml** soubor v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="ce0e5-179">Když **DetailsPage.xaml** otevření souboru, vyhledejte následující řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="ce0e5-180">Tyto řádky nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="ce0e5-181">Uložte a zavřete **DetailsPage.xaml** souboru.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="ce0e5-182">Sestavení aplikace Windows Phone ke kontrole chyb.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="ce0e5-183">Krok 3: Testování-komplexní řešení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="ce0e5-184">Jak je uvedeno v *požadavky* části tohoto kurzu, a to při testování připojení mezi webového rozhraní API a Windows Phone 8 projekty v lokálním systému, budete muset postupujte podle pokynů *[ Připojení k webové rozhraní API aplikace v místním počítači emulátoru Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte svého testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="ce0e5-185">Jakmile máte nakonfigurované testovacího prostředí, musíte nastavit jako spouštěný projekt aplikace Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="ce0e5-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="ce0e5-186">Chcete-li tak učinit, zvýrazněte **BookCatalog** aplikace v Průzkumníku řešení a pak klikněte na tlačítko **nastavit jako spouštěný projekt**:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="ce0e5-187">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-187">Click image to expand</span></span> |

<span data-ttu-id="ce0e5-188">Po stisknutí klávesy F5, Visual Studio se spustí i Windows Phone emulátoru, která se zobrazí &quot;počkejte&quot; zprávy z vaší webové rozhraní API je načítání dat aplikací:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="ce0e5-189">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-189">Click image to expand</span></span> |

<span data-ttu-id="ce0e5-190">Pokud všechno proběhne úspěšně, měli byste vidět katalogu zobrazí:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="ce0e5-191">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-191">Click image to expand</span></span> |

<span data-ttu-id="ce0e5-192">Pokud klepnete na libovolný název knihy, aplikace se zobrazí popis adresáře:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="ce0e5-193">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-193">Click image to expand</span></span> |

<span data-ttu-id="ce0e5-194">Pokud aplikace nemůže komunikovat s vašeho webového rozhraní API, zobrazí se chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="ce0e5-195">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-195">Click image to expand</span></span> |

<span data-ttu-id="ce0e5-196">Pokud klepnete na chybovou zprávu, zobrazí se další podrobnosti o této chybě:</span><span class="sxs-lookup"><span data-stu-id="ce0e5-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="ce0e5-197">Kliknutím na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="ce0e5-197">Click image to expand</span></span>                                                                 |

