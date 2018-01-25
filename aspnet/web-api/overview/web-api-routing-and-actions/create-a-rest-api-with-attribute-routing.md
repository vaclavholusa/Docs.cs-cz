---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: "Vytvořit rozhraní REST API s atribut směrování v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: c1d0b3e1644ef7f9ebb4be74c3fdf3df90cf3537
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="0135b-102">Vytvořit rozhraní REST API s atributem směrování v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="0135b-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0135b-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0135b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0135b-104">Webové rozhraní API 2 podporuje nový typ směrování, nazývá *směrováním atributů*.</span><span class="sxs-lookup"><span data-stu-id="0135b-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="0135b-105">Obecné přehled směrováním atributů, najdete v tématu [atribut směrování ve webovém rozhraní API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="0135b-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="0135b-106">V tomto kurzu budete používat směrováním atributů k vytvoření rozhraní REST API pro kolekci knih.</span><span class="sxs-lookup"><span data-stu-id="0135b-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="0135b-107">Rozhraní API se podporují následující akce:</span><span class="sxs-lookup"><span data-stu-id="0135b-107">The API will support the following actions:</span></span>

| <span data-ttu-id="0135b-108">Akce</span><span class="sxs-lookup"><span data-stu-id="0135b-108">Action</span></span> | <span data-ttu-id="0135b-109">Příklad identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="0135b-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="0135b-110">Získejte seznam všech knih.</span><span class="sxs-lookup"><span data-stu-id="0135b-110">Get a list of all books.</span></span> | <span data-ttu-id="0135b-111">/ api/knihy</span><span class="sxs-lookup"><span data-stu-id="0135b-111">/api/books</span></span> |
| <span data-ttu-id="0135b-112">Získání seznamu pomocí ID.</span><span class="sxs-lookup"><span data-stu-id="0135b-112">Get a book by ID.</span></span> | <span data-ttu-id="0135b-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="0135b-113">/api/books/1</span></span> |
| <span data-ttu-id="0135b-114">Získání podrobností o knihy.</span><span class="sxs-lookup"><span data-stu-id="0135b-114">Get the details of a book.</span></span> | <span data-ttu-id="0135b-115">/API/Books/1/details</span><span class="sxs-lookup"><span data-stu-id="0135b-115">/api/books/1/details</span></span> |
| <span data-ttu-id="0135b-116">Získáte seznam seznamů genre.</span><span class="sxs-lookup"><span data-stu-id="0135b-116">Get a list of books by genre.</span></span> | <span data-ttu-id="0135b-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="0135b-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="0135b-118">Získáte seznam seznamů datum publikace.</span><span class="sxs-lookup"><span data-stu-id="0135b-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="0135b-119">/API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (alternativní formulář)</span><span class="sxs-lookup"><span data-stu-id="0135b-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="0135b-120">Získáte seznam seznamů určitého autora.</span><span class="sxs-lookup"><span data-stu-id="0135b-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="0135b-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="0135b-121">/api/authors/1/books</span></span> |

<span data-ttu-id="0135b-122">Všechny metody jsou jen pro čtení (požadavky HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="0135b-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="0135b-123">Datová vrstva použijeme Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0135b-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="0135b-124">Seznam záznamů, bude mít následující pole:</span><span class="sxs-lookup"><span data-stu-id="0135b-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="0135b-125">ID</span><span class="sxs-lookup"><span data-stu-id="0135b-125">ID</span></span>
- <span data-ttu-id="0135b-126">Název</span><span class="sxs-lookup"><span data-stu-id="0135b-126">Title</span></span>
- <span data-ttu-id="0135b-127">Genre</span><span class="sxs-lookup"><span data-stu-id="0135b-127">Genre</span></span>
- <span data-ttu-id="0135b-128">Datum publikování</span><span class="sxs-lookup"><span data-stu-id="0135b-128">Publication date</span></span>
- <span data-ttu-id="0135b-129">Cena</span><span class="sxs-lookup"><span data-stu-id="0135b-129">Price</span></span>
- <span data-ttu-id="0135b-130">Popis</span><span class="sxs-lookup"><span data-stu-id="0135b-130">Description</span></span>
- <span data-ttu-id="0135b-131">AuthorID (cizí klíč k tabulce Autoři)</span><span class="sxs-lookup"><span data-stu-id="0135b-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="0135b-132">Pro většinu požadavky ale rozhraní API vrátí podmnožinu těchto dat (název, autor a genre).</span><span class="sxs-lookup"><span data-stu-id="0135b-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="0135b-133">Chcete-li získat úplný záznam klienta požadavky `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="0135b-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0135b-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0135b-134">Prerequisites</span></span>

<span data-ttu-id="0135b-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="0135b-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="0135b-136">Vytvoření projektu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0135b-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="0135b-137">Spusťte spuštěním sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0135b-137">Start by running Visual Studio.</span></span> <span data-ttu-id="0135b-138">Z **soubor** nabídce vyberte možnost **nový** a pak vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="0135b-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="0135b-139">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="0135b-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="0135b-140">V části **Visual C#**, vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="0135b-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="0135b-141">V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="0135b-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="0135b-142">Název projektu &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="0135b-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="0135b-143">V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony.</span><span class="sxs-lookup"><span data-stu-id="0135b-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="0135b-144">V části "Přidat složky a odkazy na základní" vyberte **webového rozhraní API** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0135b-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="0135b-145">Klikněte na tlačítko **vytvoření projektu**.</span><span class="sxs-lookup"><span data-stu-id="0135b-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="0135b-146">Tím se vytvoří kostru projektu, který je nakonfigurován pro funkce webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0135b-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="0135b-147">Domain Models</span><span class="sxs-lookup"><span data-stu-id="0135b-147">Domain Models</span></span>

<span data-ttu-id="0135b-148">Dál přidejte třídy pro modely domény.</span><span class="sxs-lookup"><span data-stu-id="0135b-148">Next, add classes for domain models.</span></span> <span data-ttu-id="0135b-149">V Průzkumníku řešení klikněte pravým tlačítkem na složku modely.</span><span class="sxs-lookup"><span data-stu-id="0135b-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="0135b-150">Vyberte **přidat**, pak vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="0135b-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="0135b-151">Název třídy `Author`.</span><span class="sxs-lookup"><span data-stu-id="0135b-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="0135b-152">Nahraďte kód v Author.cs s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="0135b-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="0135b-153">Nyní přidejte jinou třídu s názvem `Book`.</span><span class="sxs-lookup"><span data-stu-id="0135b-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="0135b-154">Přidat řadič webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0135b-154">Add a Web API Controller</span></span>

<span data-ttu-id="0135b-155">V tomto kroku přidáme kontroleru webového rozhraní API, používající rozhraní Entity Framework jako datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="0135b-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="0135b-156">Sestavte projekt stisknutím kombinace kláves CTRL+SHIFT+B.</span><span class="sxs-lookup"><span data-stu-id="0135b-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="0135b-157">Rozhraní Entity Framework využívá reflexe k vyhledávání vlastností modely, takže vyžaduje kompilované sestavení vytvořit schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="0135b-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="0135b-158">V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="0135b-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0135b-159">Vyberte **přidat**, pak vyberte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="0135b-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="0135b-160">V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte možnost "Web API 2 řadiče s akcemi čtení/zápisu používající rozhraní Entity Framework."</span><span class="sxs-lookup"><span data-stu-id="0135b-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="0135b-161">V **přidat kontroler** dialogu pro **názvu Kontroleru**, zadejte &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0135b-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="0135b-162">Vyberte &quot;použít asynchronní akce kontroleru&quot; zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="0135b-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="0135b-163">Pro **třída modelu**, vyberte &quot;kniha&quot;.</span><span class="sxs-lookup"><span data-stu-id="0135b-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="0135b-164">(Pokud se nezobrazí `Book` třídy uvedené v rozevírací nabídce, ujistěte se, že jste vytvořili projekt.) Klikněte na tlačítko "+".</span><span class="sxs-lookup"><span data-stu-id="0135b-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="0135b-165">Klikněte na tlačítko **přidat** v **nový kontext dat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0135b-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="0135b-166">Klikněte na tlačítko **přidat** v **přidat kontroler** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0135b-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="0135b-167">Přidá třídu s názvem generování uživatelského rozhraní `BooksController` , který definuje kontroler API.</span><span class="sxs-lookup"><span data-stu-id="0135b-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="0135b-168">Také přidá třídu s názvem `BooksAPIContext` ve složce modely, která definuje kontextu dat rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0135b-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="0135b-169">Počáteční hodnoty databáze</span><span class="sxs-lookup"><span data-stu-id="0135b-169">Seed the Database</span></span>

<span data-ttu-id="0135b-170">Z nabídky Nástroje, vyberte **Správce balíčků knihoven**a potom vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="0135b-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="0135b-171">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0135b-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="0135b-172">Tento příkaz vytvoří složku Migrations a přidá nový kód soubor s názvem Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="0135b-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="0135b-173">Umožňuje otevřít tento soubor a přidejte následující kód, který `Configuration.Seed` metoda.</span><span class="sxs-lookup"><span data-stu-id="0135b-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="0135b-174">V okně konzoly Správce balíčků zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="0135b-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="0135b-175">Tyto příkazy vytvořit místní databázi a vyvolání metody počáteční hodnoty k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="0135b-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="0135b-176">Přidat DTO třídy</span><span class="sxs-lookup"><span data-stu-id="0135b-176">Add DTO Classes</span></span>

<span data-ttu-id="0135b-177">Pokud aplikaci nyní spustit a odeslat požadavek GET na /api/books/1, odpověď bude vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="0135b-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="0135b-178">(Po přidání odsazení čitelnější.)</span><span class="sxs-lookup"><span data-stu-id="0135b-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="0135b-179">Místo toho chci, aby tento požadavek vrátí podmnožinu pole.</span><span class="sxs-lookup"><span data-stu-id="0135b-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="0135b-180">Navíc má být vrátit jméno autora, nikoli Autor ID.</span><span class="sxs-lookup"><span data-stu-id="0135b-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="0135b-181">K tomu, upravíme metody kontroleru vrátit *objekt pro přenos dat* (DTO) namísto EF modelu.</span><span class="sxs-lookup"><span data-stu-id="0135b-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="0135b-182">DTO je objekt, který je určena pouze pro přenášejí data.</span><span class="sxs-lookup"><span data-stu-id="0135b-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="0135b-183">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** | **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="0135b-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="0135b-184">Název složky &quot;DTOs&quot;.</span><span class="sxs-lookup"><span data-stu-id="0135b-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="0135b-185">Přidejte třídu s názvem `BookDto` do složky DTOs, s následující definice:</span><span class="sxs-lookup"><span data-stu-id="0135b-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="0135b-186">Přidejte jinou třídu s názvem `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="0135b-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="0135b-187">Potom aktualizujte `BooksController` třídy vracení `BookDto` instance.</span><span class="sxs-lookup"><span data-stu-id="0135b-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="0135b-188">Použijeme [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metoda do projektu `Book` instance k `BookDto` instance.</span><span class="sxs-lookup"><span data-stu-id="0135b-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="0135b-189">Tady je aktualizovaný kód pro třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0135b-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="0135b-190">Odstraněné `PutBook`, `PostBook`, a `DeleteBook` metody, protože nejsou pro tento kurz potřeba.</span><span class="sxs-lookup"><span data-stu-id="0135b-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="0135b-191">Teď Pokud spuštění aplikace a požádat o /api/books/1, text odpovědi by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="0135b-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="0135b-192">Přidat trasy atributů</span><span class="sxs-lookup"><span data-stu-id="0135b-192">Add Route Attributes</span></span>

<span data-ttu-id="0135b-193">V dalším kroku se nám budete převést řadiče, pokud chcete používat směrování atribut.</span><span class="sxs-lookup"><span data-stu-id="0135b-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="0135b-194">Nejprve přidejte **RoutePrefix** atribut kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0135b-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="0135b-195">Tento atribut definuje počáteční segmenty identifikátor URI pro všechny metody v tomto řadiči.</span><span class="sxs-lookup"><span data-stu-id="0135b-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="0135b-196">Pak přidejte **[trasy]** atributy pro akce kontroleru, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0135b-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="0135b-197">Šablona trasy pro jednotlivé metody kontroleru je předpona plus řetězec zadaný v **trasy** atribut.</span><span class="sxs-lookup"><span data-stu-id="0135b-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="0135b-198">Pro `GetBook` metoda šablonu trasy zahrnuje parametrizovaný řetězec &quot;{id: int}&quot;, který odpovídá, pokud segment identifikátoru URI obsahuje celočíselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0135b-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="0135b-199">Metoda</span><span class="sxs-lookup"><span data-stu-id="0135b-199">Method</span></span> | <span data-ttu-id="0135b-200">Šablona trasy</span><span class="sxs-lookup"><span data-stu-id="0135b-200">Route Template</span></span> | <span data-ttu-id="0135b-201">Příklad identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="0135b-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="0135b-202">"rozhraní api nebo seznamy."</span><span class="sxs-lookup"><span data-stu-id="0135b-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="0135b-203">"api/books / {id: int}"</span><span class="sxs-lookup"><span data-stu-id="0135b-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="0135b-204">Získat podrobnosti adresáře</span><span class="sxs-lookup"><span data-stu-id="0135b-204">Get Book Details</span></span>

<span data-ttu-id="0135b-205">Chcete-li získat podrobnosti adresáře, klient odešle požadavek GET na `/api/books/{id}/details`, kde *{id}* je ID knihy.</span><span class="sxs-lookup"><span data-stu-id="0135b-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="0135b-206">Přidejte následující metodu do `BooksController` třídy.</span><span class="sxs-lookup"><span data-stu-id="0135b-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="0135b-207">Pokud si vyžádáte `/api/books/1/details`, odpověď bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="0135b-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="0135b-208">Získání seznamů pomocí Genre</span><span class="sxs-lookup"><span data-stu-id="0135b-208">Get Books By Genre</span></span>

<span data-ttu-id="0135b-209">Chcete-li získat seznam seznamů v konkrétní genre, klient odešle požadavek GET na `/api/books/genre`, kde *genre* je název genre.</span><span class="sxs-lookup"><span data-stu-id="0135b-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="0135b-210">(Například `/get/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="0135b-210">(For example, `/get/books/fantasy`.)</span></span>

<span data-ttu-id="0135b-211">Přidejte následující metodu do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="0135b-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="0135b-212">Zde jsme se definování trasy, která obsahuje parametr {genre} v šabloně identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="0135b-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="0135b-213">Všimněte si, že webového rozhraní API je možné rozlišit tyto dva identifikátory URI a směrovat je do různých metod:</span><span class="sxs-lookup"><span data-stu-id="0135b-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="0135b-214">Důvodem je, že `GetBook` metoda obsahuje omezení, že segment "id" musí být celočíselná hodnota:</span><span class="sxs-lookup"><span data-stu-id="0135b-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="0135b-215">Pokud budete požadovat /api/books/fantasy, odpověď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0135b-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="0135b-216">Získat knihy autorem</span><span class="sxs-lookup"><span data-stu-id="0135b-216">Get Books By Author</span></span>

<span data-ttu-id="0135b-217">Pokud chcete získat seznam seznamů, pro konkrétní autora, klient odešle požadavek GET na `/api/authors/id/books`, kde *id* je ID autora.</span><span class="sxs-lookup"><span data-stu-id="0135b-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="0135b-218">Přidejte následující metodu do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="0135b-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="0135b-219">V tomto příkladu je zajímavé protože &quot;knihy&quot; je zpracováván prostředkem podřízené &quot;autoři&quot;.</span><span class="sxs-lookup"><span data-stu-id="0135b-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="0135b-220">Tento vzor je celkem běžné rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="0135b-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="0135b-221">Znak tilda (~) v šabloně trasy přepsání předponu trasy v **RoutePrefix** atribut.</span><span class="sxs-lookup"><span data-stu-id="0135b-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="0135b-222">Získat knihy podle data publikování</span><span class="sxs-lookup"><span data-stu-id="0135b-222">Get Books By Publication Date</span></span>

<span data-ttu-id="0135b-223">Pokud chcete získat seznam seznamů, datu publikace, klient odešle požadavek GET na `/api/books/date/yyyy-mm-dd`, kde *rrrr mm-dd* je datum.</span><span class="sxs-lookup"><span data-stu-id="0135b-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="0135b-224">Zde je možné provést toto:</span><span class="sxs-lookup"><span data-stu-id="0135b-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="0135b-225">`{pubdate:datetime}` Parametr je omezena tak, aby odpovídala **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0135b-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="0135b-226">Tento postup funguje, ale je ve skutečnosti mírnější, než rádi bychom znali.</span><span class="sxs-lookup"><span data-stu-id="0135b-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="0135b-227">Tyto identifikátory URI například bude odpovídat trasy:</span><span class="sxs-lookup"><span data-stu-id="0135b-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="0135b-228">Není co nesprávný s povolením tyto identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="0135b-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="0135b-229">Můžete však omezit trasy na konkrétní formát přidáním regulární výraz omezení pro šablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="0135b-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="0135b-230">Nyní pouze data ve formě &quot;rrrr mm-dd&quot; bude odpovídat.</span><span class="sxs-lookup"><span data-stu-id="0135b-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="0135b-231">Všimněte si, že jsme nepoužívejte regulární výraz k ověření, že My skutečná data.</span><span class="sxs-lookup"><span data-stu-id="0135b-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="0135b-232">Zpracovává když se pokusí převést segment identifikátoru URI do webového rozhraní API **data a času** instance.</span><span class="sxs-lookup"><span data-stu-id="0135b-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="0135b-233">Neplatné datum například 2012-47-99 se nepodaří převést, a klient získá chybu 404.</span><span class="sxs-lookup"><span data-stu-id="0135b-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="0135b-234">Může také podporovat oddělovač lomítko (`/api/books/date/yyyy/mm/dd`) tak, že přidáte další **[trasy]** atribut s jiný regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="0135b-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="0135b-235">Je zde podrobností jemně ale důležité.</span><span class="sxs-lookup"><span data-stu-id="0135b-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="0135b-236">Druhá šablona trasy obsahovala zástupný znak (\*) na začátku parametr {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="0135b-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="0135b-237">Tato hodnota informuje modulu směrování, že tento {pubdate} by měl odpovídat zbytek identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="0135b-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="0135b-238">Ve výchozím nastavení odpovídá parametru šablony jeden segment identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="0135b-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="0135b-239">V tomto případě chceme {pubdate} rozmístěny v několika segmentům URI:</span><span class="sxs-lookup"><span data-stu-id="0135b-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="0135b-240">Kódu kontroleru</span><span class="sxs-lookup"><span data-stu-id="0135b-240">Controller Code</span></span>

<span data-ttu-id="0135b-241">Tady je kompletní kód pro třídu BooksController.</span><span class="sxs-lookup"><span data-stu-id="0135b-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="0135b-242">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0135b-242">Summary</span></span>

<span data-ttu-id="0135b-243">Atribut směrování nabízí další ovládací prvek a větší flexibilitu při navrhování identifikátory URI pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0135b-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
