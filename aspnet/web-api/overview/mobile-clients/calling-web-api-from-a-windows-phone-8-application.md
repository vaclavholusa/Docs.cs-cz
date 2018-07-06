---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Volání webového rozhraní API z Windows Phone 8 aplikace (C#) | Dokumentace Microsoftu
author: rmcmurray
description: Vytvoření kompletní scénář začátku do konce skládající se z aplikace ASP.NET Web API, která obsahuje katalog knih, které mají aplikace Windows Phone 8.
ms.author: aspnetcontent
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6b7a833818424cbf3a3bf9e1e14e5b2864742c38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805039"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="4597f-103">Volání webového rozhraní API z aplikace pro Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="4597f-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="4597f-104">podle [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="4597f-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="4597f-105">V tomto kurzu se dozvíte, jak vytvořit kompletní scénář začátku do konce skládající se z aplikace ASP.NET Web API, která obsahuje katalog knih, které mají aplikace Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="4597f-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="4597f-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="4597f-106">Overview</span></span>

<span data-ttu-id="4597f-107">RESTful služeb, jako je ASP.NET Web API zjednodušit vytváření aplikací založených na protokolu HTTP pro vývojáře tím, že poskytuje abstrakci Architektura aplikace na straně serveru a na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4597f-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="4597f-108">Místo vytváření vlastnickým protokolem založené na soket pro komunikaci, webové rozhraní API vývojářům jednoduše je potřeba publikovat metody požadavku HTTP pro svou aplikaci (například: GET, POST, PUT, DELETE), a vývojáři klientských aplikací můžou potřebují jenom využívat metody HTTP, které jsou nezbytné pro svou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4597f-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="4597f-109">V tomto kurzu začátku do konce se dozvíte, jak pomocí webového rozhraní API k vytvoření následující projekty:</span><span class="sxs-lookup"><span data-stu-id="4597f-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="4597f-110">V [první části tohoto kurzu](#STEP1), vytvoříte aplikaci rozhraní ASP.NET Web API, která podporuje všechny operace vytvoření, čtení, aktualizace a odstranění (CRUD) ke správě adresáře katalogu.</span><span class="sxs-lookup"><span data-stu-id="4597f-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="4597f-111">Tato aplikace bude používat [ukázkový soubor XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) z webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="4597f-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="4597f-112">V [druhé části tohoto kurzu](#STEP2), vytvoříte interaktivní aplikace Windows Phone 8, která načte data z aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4597f-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="4597f-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4597f-113">Prerequisites</span></span>

- <span data-ttu-id="4597f-114">Visual Studio 2013 with nainstalovánu sadu Windows Phone 8 SDK</span><span class="sxs-lookup"><span data-stu-id="4597f-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="4597f-115">Windows 8 nebo novější systém 64-bit s nainstalovanou technologii Hyper-V</span><span class="sxs-lookup"><span data-stu-id="4597f-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="4597f-116">Seznam dalších požadavcích najdete v tématu *požadavky na systém* části na [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) stránce pro stažení.</span><span class="sxs-lookup"><span data-stu-id="4597f-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="4597f-117">Pokud se chystáte otestovat připojení mezi webovým rozhraním API a projekty pro Windows Phone 8 v místním systému, budete muset postupujte podle pokynů *[připojení k rozhraní API webové aplikace v místním emulátorem Windows Phone 8 Počítač](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="4597f-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="4597f-118">Krok 1: Vytvoření webového rozhraní API knihkupectví projektu</span><span class="sxs-lookup"><span data-stu-id="4597f-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="4597f-119">Prvním krokem tohoto kurzu začátku do konce, je vytvořit projekt webového rozhraní API, která podporuje všechny operace CRUD; Všimněte si, že přidáte projekt aplikace Windows Phone k tomuto řešení v [kroku 2](#STEP2) tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4597f-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="4597f-120">Otevřít **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="4597f-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="4597f-121">Klikněte na tlačítko **souboru**, pak **nové**a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4597f-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="4597f-122">Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalováno**, pak **šablony**, pak **Visual C#** a pak **Webové**.</span><span class="sxs-lookup"><span data-stu-id="4597f-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="4597f-123">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="4597f-124">Zvýrazněte **webová aplikace ASP.NET**, zadejte **knihkupectví** pro název projektu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4597f-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="4597f-125">Když **nový projekt ASP.NET** dialogové okno se zobrazí, vyberte **webového rozhraní API** šablonu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4597f-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="4597f-126">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="4597f-127">Při otevření projektu webového rozhraní API, odeberte z projektu vzorku kontroleru:</span><span class="sxs-lookup"><span data-stu-id="4597f-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="4597f-128">Rozbalte **řadiče** složku v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="4597f-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="4597f-129">Klikněte pravým tlačítkem myši **ValuesController.cs** souboru a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="4597f-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="4597f-130">Klikněte na tlačítko **OK** po zobrazení výzvy potvrďte odstranění.</span><span class="sxs-lookup"><span data-stu-id="4597f-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="4597f-131">Přidání datového souboru XML do projektu webového rozhraní API. Tento soubor obsahuje obsah knihkupectví katalogu:</span><span class="sxs-lookup"><span data-stu-id="4597f-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="4597f-132">Klikněte pravým tlačítkem **aplikace\_Data** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="4597f-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="4597f-133">Když **přidat novou položku** dialogové okno se zobrazí, vyberte **soubor XML** šablony.</span><span class="sxs-lookup"><span data-stu-id="4597f-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="4597f-134">Název souboru **Books.xml**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4597f-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="4597f-135">Když **Books.xml** otevření souboru, nahraďte kód v souboru XML ze vzorku **books.xml** soubor na webové stránce MSDN:</span><span class="sxs-lookup"><span data-stu-id="4597f-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="4597f-136">Uložte a zavřete soubor XML.</span><span class="sxs-lookup"><span data-stu-id="4597f-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="4597f-137">Přidání knihkupectví modelu do projektu webového rozhraní API. Tento model obsahuje logiku pro aplikace knihkupectví vytvoření, čtení, aktualizace a odstranění (CRUD):</span><span class="sxs-lookup"><span data-stu-id="4597f-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="4597f-138">Klikněte pravým tlačítkem myši **modely** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na tlačítko **třídy**.</span><span class="sxs-lookup"><span data-stu-id="4597f-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="4597f-139">Když **přidat novou položku** dialogové okno se zobrazí, zadejte název souboru třídy **BookDetails.cs**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4597f-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="4597f-140">Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4597f-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="4597f-141">Uložte a zavřete **BookDetails.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="4597f-142">Přidáte kontroler knihkupectví do projektu webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="4597f-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="4597f-143">Klikněte pravým tlačítkem myši **řadiče** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na **řadič**.</span><span class="sxs-lookup"><span data-stu-id="4597f-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="4597f-144">Když **přidat vygenerované uživatelské rozhraní** se zobrazí dialogové okno, zvýrazněte **kontroler rozhraní Web API 2 – prázdný**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4597f-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="4597f-145">Když **přidat kontroler** dialogové okno se zobrazí, zadejte název kontroleru **BooksController**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4597f-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="4597f-146">Když **BooksController.cs** otevření souboru, nahraďte kód v souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4597f-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="4597f-147">Uložte a zavřete **BooksController.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="4597f-148">Sestavení aplikace webového rozhraní API ke kontrole chyb.</span><span class="sxs-lookup"><span data-stu-id="4597f-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="4597f-149">Krok 2: Přidání projektu Windows Phone 8 knihkupectví katalogu</span><span class="sxs-lookup"><span data-stu-id="4597f-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="4597f-150">Dalším krokem tohoto scénáře začátku do konce je vytvoření katalogu aplikací pro Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="4597f-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="4597f-151">Tato aplikace bude používat *aplikace Windows Phone datové vazby* šablonu pro výchozí uživatelské rozhraní a bude používat aplikace webového rozhraní API, kterou jste vytvořili v [kroku 1](#STEP1) tohoto kurzu jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="4597f-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="4597f-152">Klikněte pravým tlačítkem myši **knihkupectví** řešení v v Průzkumníku řešení klikněte **přidat**a potom **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="4597f-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="4597f-153">Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalováno**, pak **Visual C#** a potom **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="4597f-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="4597f-154">Zvýrazněte **aplikace Windows Phone datové vazby**, zadejte **BookCatalog** pro název a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4597f-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="4597f-155">Přidejte balíček Json.NET NuGet **BookCatalog** projektu:</span><span class="sxs-lookup"><span data-stu-id="4597f-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="4597f-156">Klikněte pravým tlačítkem na **odkazy** pro **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4597f-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="4597f-157">Když **spravovat balíčky NuGet** se zobrazí dialogové okno, rozbalte **Online** části a zvýraznit **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="4597f-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="4597f-158">Zadejte **Json.NET** do vyhledávacího pole a klikněte na ikonu hledání.</span><span class="sxs-lookup"><span data-stu-id="4597f-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="4597f-159">Zvýrazněte **Json.NET** ve výsledcích hledání a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="4597f-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="4597f-160">Po dokončení instalace, klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="4597f-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="4597f-161">Přidat **BookDetails** model k **BookCatalog** projektu; obsahuje obecný model knihkupectví třídy:</span><span class="sxs-lookup"><span data-stu-id="4597f-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="4597f-162">Klikněte pravým tlačítkem myši **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na **přidat**a potom klikněte na tlačítko **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="4597f-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="4597f-163">Název nové složky **modely**.</span><span class="sxs-lookup"><span data-stu-id="4597f-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="4597f-164">Klikněte pravým tlačítkem myši **modely** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na tlačítko **třídy**.</span><span class="sxs-lookup"><span data-stu-id="4597f-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="4597f-165">Když **přidat novou položku** dialogové okno se zobrazí, zadejte název souboru třídy **BookDetails.cs**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4597f-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="4597f-166">Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4597f-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="4597f-167">Uložte a zavřete **BookDetails.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="4597f-168">Aktualizace **MainViewModel.cs** třídy, aby obsahoval funkce pro komunikaci s aplikací knihkupectví webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="4597f-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="4597f-169">Rozbalte **modely ViewModels** složku v Průzkumníku řešení a poté dvojitým kliknutím **MainViewModel.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="4597f-170">Když **MainViewModel.cs** otevření souboru nahraďte kód v souboru následujícím kódem, mějte na paměti, že budete muset aktualizovat hodnotu `apiUrl` konstantní pomocí příkazu skutečnou adresu URL webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="4597f-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="4597f-171">Uložte a zavřete **MainViewModel.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="4597f-172">Aktualizace **MainPage.xaml** soubor upravit název aplikace:</span><span class="sxs-lookup"><span data-stu-id="4597f-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="4597f-173">Dvakrát klikněte **MainPage.xaml** souboru v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="4597f-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="4597f-174">Když **MainPage.xaml** soubor otevřít, vyhledejte následující řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="4597f-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="4597f-175">Nahraďte tyto řádky s následujícími možnostmi:</span><span class="sxs-lookup"><span data-stu-id="4597f-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="4597f-176">Uložte a zavřete **MainPage.xaml** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="4597f-177">Aktualizace **DetailsPage.xaml** soubor pro přizpůsobení zobrazených položek:</span><span class="sxs-lookup"><span data-stu-id="4597f-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="4597f-178">Dvakrát klikněte **DetailsPage.xaml** souboru v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="4597f-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="4597f-179">Když **DetailsPage.xaml** soubor otevřít, vyhledejte následující řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="4597f-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="4597f-180">Nahraďte tyto řádky s následujícími možnostmi:</span><span class="sxs-lookup"><span data-stu-id="4597f-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="4597f-181">Uložte a zavřete **DetailsPage.xaml** souboru.</span><span class="sxs-lookup"><span data-stu-id="4597f-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="4597f-182">Vytvoření aplikace Windows Phone ke kontrole chyb.</span><span class="sxs-lookup"><span data-stu-id="4597f-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="4597f-183">Krok 3: Testování – ucelené řešení</span><span class="sxs-lookup"><span data-stu-id="4597f-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="4597f-184">Jak je uvedeno v *požadavky* projekty části tohoto kurzu, a to při testování připojení mezi webovým rozhraním API a Windows Phone 8 v místním systému, budete muset postupovat podle pokynů *[ Připojení k rozhraní API webové aplikace v místním počítači emulátorem Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="4597f-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="4597f-185">Jakmile budete mít testování prostředí nakonfigurované, je potřeba nastavit jako spouštěný projekt aplikace Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="4597f-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="4597f-186">Uděláte to tak, zvýrazněte **BookCatalog** aplikace v Průzkumníku řešení a pak klikněte na tlačítko **nastavit jako spouštěný projekt**:</span><span class="sxs-lookup"><span data-stu-id="4597f-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="4597f-187">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-187">Click image to expand</span></span> |

<span data-ttu-id="4597f-188">Pokud stisknete klávesu F5, Visual Studio se spustí i Windows Phone Emulator, který se zobrazí &quot;počkejte&quot; zpráva, zatímco data aplikací se načte z vašeho webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="4597f-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="4597f-189">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-189">Click image to expand</span></span> |

<span data-ttu-id="4597f-190">Pokud všechno proběhne úspěšně, měli byste vidět katalogu zobrazí:</span><span class="sxs-lookup"><span data-stu-id="4597f-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="4597f-191">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-191">Click image to expand</span></span> |

<span data-ttu-id="4597f-192">Pokud klepnete na libovolný název knihy, aplikace se zobrazí popis knihy:</span><span class="sxs-lookup"><span data-stu-id="4597f-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="4597f-193">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-193">Click image to expand</span></span> |

<span data-ttu-id="4597f-194">Pokud aplikace nemůže komunikovat s webové rozhraní API, zobrazí se chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4597f-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="4597f-195">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-195">Click image to expand</span></span> |

<span data-ttu-id="4597f-196">Pokud klepnete na chybovou zprávu, zobrazí se další podrobnosti o chybě:</span><span class="sxs-lookup"><span data-stu-id="4597f-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="4597f-197">Klikněte na obrázek rozbalení</span><span class="sxs-lookup"><span data-stu-id="4597f-197">Click image to expand</span></span>                                                                 |

