---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: '4. část: Modely a přístup k datům | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 4. část se věnuje modely a přístup k datům.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402035"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="a4521-104">4. část: Modely a přístup k datům</span><span class="sxs-lookup"><span data-stu-id="a4521-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="a4521-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a4521-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a4521-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4521-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a4521-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="a4521-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="a4521-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a4521-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a4521-109">4. část se věnuje modely a přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="a4521-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="a4521-110">Zatím jsme jste právě byla předávání "fiktivní data" z našich řadičů naše zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="a4521-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="a4521-111">Nyní jsme připraveni připojení skutečná databáze.</span><span class="sxs-lookup"><span data-stu-id="a4521-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="a4521-112">V tomto kurzu probereme jak používat SQL Server Compact Edition (často označované jako SQL CE) jako naše databázového stroje.</span><span class="sxs-lookup"><span data-stu-id="a4521-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="a4521-113">SQL CE je databáze založená na bezplatné, embedded, souborech, který nevyžaduje, aby všechny instalace nebo konfigurace, která je to velmi vhodné pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="a4521-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="a4521-114">Přístup k databázi pomocí Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="a4521-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="a4521-115">Použijeme podporu Entity Framework (EF), která je zahrnutá v projektech ASP.NET MVC 3 pro dotazování a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="a4521-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="a4521-116">EF je objekt flexibilní relační mapování dat (ORM určené) vývojářům umožňuje dotazování a aktualizace dat uložených v databázi způsobem objektově orientované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a4521-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="a4521-117">Rozhraní Entity Framework verze 4 podporuje vývoj paradigma volá založeno na kódu.</span><span class="sxs-lookup"><span data-stu-id="a4521-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="a4521-118">Založeno na kódu můžete vytvořit objekt modelu napsáním jednoduché třídy (označované také jako POCO z objektů CLR "prostý staré") a můžete dokonce vytvořit databázi v reálném čase z vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="a4521-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="a4521-119">Změny v našich tříd modelu</span><span class="sxs-lookup"><span data-stu-id="a4521-119">Changes to our Model Classes</span></span>

<span data-ttu-id="a4521-120">Budeme v tomto kurzu využití funkce vytvoření databáze v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a4521-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="a4521-121">Než to, ale vytvoříme několik menší změny do našich tříd modelu přidat pár věcí, které budeme dále používat.</span><span class="sxs-lookup"><span data-stu-id="a4521-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="a4521-122">Přidání třídy modelu interpret</span><span class="sxs-lookup"><span data-stu-id="a4521-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="a4521-123">Naše alb bude spojená s umělci, tak přidáme jednoduchý model tříd k popisu umělce.</span><span class="sxs-lookup"><span data-stu-id="a4521-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="a4521-124">Přidejte novou třídu do složky modelů s názvem Artist.cs pomocí kódu níže.</span><span class="sxs-lookup"><span data-stu-id="a4521-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="a4521-125">Aktualizace našich tříd modelu</span><span class="sxs-lookup"><span data-stu-id="a4521-125">Updating our Model Classes</span></span>

<span data-ttu-id="a4521-126">Aktualizace třídy alb, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="a4521-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="a4521-127">Do třídy rozšířením podle tematických dále, proveďte následující aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a4521-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="a4521-128">Přidání aplikace\_složka dat</span><span class="sxs-lookup"><span data-stu-id="a4521-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="a4521-129">Přidáme aplikace\_adresář dat pro náš projekt k uložení naše soubory databáze serveru SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="a4521-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="a4521-130">Aplikace\_dat je speciální adresář v technologii ASP.NET, která už má správné zabezpečení přístupová oprávnění pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="a4521-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="a4521-131">V nabídce Projekt vyberte Přidat složku ASP.NET a pak aplikaci\_Data.</span><span class="sxs-lookup"><span data-stu-id="a4521-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="a4521-132">Vytvoření připojovacího řetězce v souboru web.config</span><span class="sxs-lookup"><span data-stu-id="a4521-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="a4521-133">Konfigurační soubor webu přidáme několik řádků, tak, aby rozhraní Entity Framework ví, jak se připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="a4521-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="a4521-134">Poklikejte na soubor Web.config umístěný v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="a4521-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="a4521-135">Přejděte k dolnímu okraji tento soubor a přidat &lt;connectionStrings&gt; části přímo nad poslední řádek, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="a4521-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="a4521-136">Přidání třídy kontextu</span><span class="sxs-lookup"><span data-stu-id="a4521-136">Adding a Context Class</span></span>

<span data-ttu-id="a4521-137">Klikněte pravým tlačítkem na složku modely a přidejte novou třídu s názvem MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="a4521-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="a4521-138">Tato třída bude představují kontext databáze Entity Framework, bude zpracování našich vytvořit, číst, aktualizovat a operací odstranění pro USA.</span><span class="sxs-lookup"><span data-stu-id="a4521-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="a4521-139">Kód pro tuto třídu je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="a4521-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="a4521-140">To je vše – neexistuje žádná další konfigurace, speciální rozhraní, atd. Rozšířením základní třídy DbContext naše MusicStoreEntities třída je schopný zvládnout naše databázové operace pro nás.</span><span class="sxs-lookup"><span data-stu-id="a4521-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="a4521-141">Teď, když máme, připojili, přidáme několik dalších vlastností do našich tříd modelu využívat některé další informace v databázi.</span><span class="sxs-lookup"><span data-stu-id="a4521-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="a4521-142">Přidání naše ukládání katalogu dat</span><span class="sxs-lookup"><span data-stu-id="a4521-142">Adding our store catalog data</span></span>

<span data-ttu-id="a4521-143">My podnikneme využívat funkce v Entity Framework, která přidá "seed" data na nově vytvořenou databázi.</span><span class="sxs-lookup"><span data-stu-id="a4521-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="a4521-144">To se vyplní předem náš katalog úložiště se seznamem žánry, umělců nebo alb.</span><span class="sxs-lookup"><span data-stu-id="a4521-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="a4521-145">Stahování MvcMusicStore Assets.zip – který součástí našich souborů návrhu lokality použitou dříve v tomto kurzu – má soubor třídy s těmito daty počáteční hodnoty, umístěný ve složce s názvem kódu.</span><span class="sxs-lookup"><span data-stu-id="a4521-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="a4521-146">V rámci kód / složku modely, vyhledejte soubor SampleData.cs a umístěte ho do složky modely v našem projektu, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="a4521-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="a4521-147">Teď potřebujeme přidat jeden řádek kódu na rozhraní Entity Framework říct SampleData třídy.</span><span class="sxs-lookup"><span data-stu-id="a4521-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="a4521-148">Poklikejte na soubor Global.asax v kořenovém adresáři projektu otevřete ho a přidejte následující řádek do horní části aplikace\_začátek metody.</span><span class="sxs-lookup"><span data-stu-id="a4521-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="a4521-149">V tuto chvíli jsme dokončili práce potřebné ke konfiguraci Entity Framework pro náš projekt.</span><span class="sxs-lookup"><span data-stu-id="a4521-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="a4521-150">Dotazování na databázi</span><span class="sxs-lookup"><span data-stu-id="a4521-150">Querying the Database</span></span>

<span data-ttu-id="a4521-151">Teď můžeme aktualizovat naše StoreController tak, aby místo použití "fiktivní data" místo toho zavolá naši databázi a všechny jeho informace o dotazování.</span><span class="sxs-lookup"><span data-stu-id="a4521-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="a4521-152">Začneme deklarací pole na **StoreController** pro uložení instance třídy MusicStoreEntities s názvem storeDB:</span><span class="sxs-lookup"><span data-stu-id="a4521-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="a4521-153">Aktualizuje se Index Store dáte dotaz na databázi</span><span class="sxs-lookup"><span data-stu-id="a4521-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="a4521-154">Třída MusicStoreEntities se spravuje pomocí Entity Framework a zpřístupňuje vlastnost kolekce pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="a4521-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="a4521-155">Umožňuje aktualizovat naše StoreController Index akci k načtení všech žánry v databázi.</span><span class="sxs-lookup"><span data-stu-id="a4521-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="a4521-156">Tento postup již jsme provedli podle pevného kódování data řetězce.</span><span class="sxs-lookup"><span data-stu-id="a4521-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="a4521-157">Teď můžeme místo jednoduše použít na kontext Entity Framework Generes kolekce:</span><span class="sxs-lookup"><span data-stu-id="a4521-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="a4521-158">Je nutné dojde k naší zobrazit šablonu, protože jsme se stále vrací stejné StoreIndexViewModel jsme vrátil před - jsme už jenom vrací živá data z naší databáze nyní žádné změny.</span><span class="sxs-lookup"><span data-stu-id="a4521-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="a4521-159">Když jsme spusťte projekt znovu, přejděte na adresu URL "/ Store" jsme zobrazí seznam všech žánry v naší databázi:</span><span class="sxs-lookup"><span data-stu-id="a4521-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="a4521-160">Aktualizuje se Store Procházet a podrobnosti o použití živých dat</span><span class="sxs-lookup"><span data-stu-id="a4521-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="a4521-161">S/Store/procházet? žánr =*[některé žánr]* metodě akce, Prohledáváme pro rozšířením podle tematických podle názvu.</span><span class="sxs-lookup"><span data-stu-id="a4521-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="a4521-162">Pouze Očekáváme, že jeden výsledek, protože jsme neustále by neměl mít dvě položky se stejným názvem žánr a používat. Metoda Single() rozšíření v LINQ dotazu pro příslušný objekt žánr takto (nevkládejte to zatím):</span><span class="sxs-lookup"><span data-stu-id="a4521-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="a4521-163">Jedinou metodu používá výraz Lambda jako parametr, který určuje, že má jeden objekt žánr tak, aby jeho název odpovídá hodnotě, kterou jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="a4521-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="a4521-164">V případě výše se načítají jeden objekt žánr s názvem hodnotu, která odpovídá roz.</span><span class="sxs-lookup"><span data-stu-id="a4521-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="a4521-165">Provedeme výhod funkce služby Entity Framework, která umožňuje určit další související entity, které chceme, aby načíst i když je načten objekt žánr.</span><span class="sxs-lookup"><span data-stu-id="a4521-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="a4521-166">Tato funkce je volána tvarování výsledek dotazu a umožňuje nám to snížit počet, kolikrát budeme potřebovat pro přístup k databázi chcete načíst všechny informace, které potřebujeme.</span><span class="sxs-lookup"><span data-stu-id="a4521-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="a4521-167">Chceme předběžného načítání alb pro žánr se nám načíst, abychom budete aktualizovat dotaz mají zahrnout z Genres.Include("Albums") k označení, že má být také související alb.</span><span class="sxs-lookup"><span data-stu-id="a4521-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="a4521-168">To je mnohem efektivnější, protože ho se načtou naše žánr a alb data v žádosti o izolované databáze.</span><span class="sxs-lookup"><span data-stu-id="a4521-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="a4521-169">S vysvětlením eliminuje zde je, jak vypadá naše aktualizované akce kontroleru Procházet:</span><span class="sxs-lookup"><span data-stu-id="a4521-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="a4521-170">Aktualizujeme můžete nyní procházet zobrazení tak, aby alb, které jsou k dispozici v každý žánr Store.</span><span class="sxs-lookup"><span data-stu-id="a4521-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="a4521-171">Otevřete zobrazit šablonu (ve /Views/Store/Browse.cshtml) a přidejte seznam s odrážkami alb, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="a4521-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="a4521-172">Spuštění aplikace a přejdete do/Store/procházet? žánr = Jazz odhalí naše výsledky jsou teď právě načtený z databáze, zobrazení všech alb v našich vybrané žánr.</span><span class="sxs-lookup"><span data-stu-id="a4521-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="a4521-173">Uděláme stejnou změnit na naší/Store/podrobnosti / [id] adresy URL a naše fiktivní data nahraďte databázového dotazu, který načte alba, jejíž ID odpovídá hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="a4521-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="a4521-174">Spuštění aplikace a přejdete do /Store/Details/1 ukazuje, že naše výsledky se nyní se berou z databáze.</span><span class="sxs-lookup"><span data-stu-id="a4521-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="a4521-175">Teď, když naší stránce s podrobnostmi o Store je nastaven pro zobrazení alba podle ID alba, můžeme aktualizovat **Procházet** zobrazit odkaz na zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="a4521-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="a4521-176">Použijeme Html.ActionLink, přesně tak, jak jsme to udělali pro propojení z indexu Store procházet Store na konci předchozí části.</span><span class="sxs-lookup"><span data-stu-id="a4521-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="a4521-177">Úplný zdrojový Procházet zobrazení se zobrazí pod.</span><span class="sxs-lookup"><span data-stu-id="a4521-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="a4521-178">Nyní jsme schopni z naši stránku Store přejít na stránku Genre, který vypíše dostupná alb, a kliknutím na alba jsme můžete zobrazit podrobnosti o této alba.</span><span class="sxs-lookup"><span data-stu-id="a4521-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="a4521-179">[Předchozí](mvc-music-store-part-3.md)
> [další](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="a4521-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
