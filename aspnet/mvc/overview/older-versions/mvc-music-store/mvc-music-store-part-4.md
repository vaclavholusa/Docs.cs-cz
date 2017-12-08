---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: "Část 4: Modely a přístup k datům | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 4 obsahuje modely a přístup k datům."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 727336344493b439130b2cf0ec6e7b5925dd5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="28f63-104">Část 4: Modely a přístup k datům</span><span class="sxs-lookup"><span data-stu-id="28f63-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="28f63-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="28f63-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="28f63-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="28f63-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="28f63-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="28f63-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="28f63-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="28f63-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="28f63-109">Část 4 obsahuje modely a přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="28f63-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="28f63-110">Zatím jsme jste právě byla předání "fiktivní data" z našich řadičů naše zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="28f63-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="28f63-111">Nyní jsme připraveni spojit skutečné databáze.</span><span class="sxs-lookup"><span data-stu-id="28f63-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="28f63-112">V tomto kurzu jsme budete pokrývajících jak používat SQL Server Compact Edition (často říká SQL CE) jako naše databázového stroje.</span><span class="sxs-lookup"><span data-stu-id="28f63-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="28f63-113">SQL CE je databáze založená na souborech volné, embedded, která nevyžaduje žádné instalace nebo konfigurace, který je to skutečně vhodné pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="28f63-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="28f63-114">Přístup k databázi pomocí Entity Framework kódu – první</span><span class="sxs-lookup"><span data-stu-id="28f63-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="28f63-115">Použijeme podporu Entity Framework (EF), který je součástí projekty ASP.NET MVC 3 pro dotazování a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="28f63-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="28f63-116">EF je relační mapování dat (ORM) rozhraní API, které umožňuje vývojářům dotazů a aktualizace dat uložených v databázi způsobem objektově orientované na objekt flexibilní.</span><span class="sxs-lookup"><span data-stu-id="28f63-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="28f63-117">Rozhraní Entity Framework verze 4 podporuje vývoj zlepší, názvem code first.</span><span class="sxs-lookup"><span data-stu-id="28f63-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="28f63-118">První kód vám umožní vytvořit objekt modelu vytvořením jednoduchého třídy (také označované jako objektů POCO z objektů CLR "prostý starý") a můžete dokonce vytvořit databázi na za chodu z tříd.</span><span class="sxs-lookup"><span data-stu-id="28f63-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="28f63-119">Změny v našich třídy modelu</span><span class="sxs-lookup"><span data-stu-id="28f63-119">Changes to our Model Classes</span></span>

<span data-ttu-id="28f63-120">Budeme v tomto kurzu využívat funkci vytvoření databáze v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="28f63-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="28f63-121">Než to, ale umožňuje provádět několik malých změn naše tříd modelu pro přidání v několik věcí, které budeme později na používat.</span><span class="sxs-lookup"><span data-stu-id="28f63-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="28f63-122">Přidání třídy modelu umělcem</span><span class="sxs-lookup"><span data-stu-id="28f63-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="28f63-123">Naše alb bude spojený s umělci, takže přidáme třídu jednoduchého modelu k popisu umělcem.</span><span class="sxs-lookup"><span data-stu-id="28f63-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="28f63-124">Přidejte novou třídu do modely složku s názvem Artist.cs pomocí kódu vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="28f63-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="28f63-125">Aktualizace našich třídy modelu</span><span class="sxs-lookup"><span data-stu-id="28f63-125">Updating our Model Classes</span></span>

<span data-ttu-id="28f63-126">Aktualizace třídy pro alb, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="28f63-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="28f63-127">Dále vytvořte následující aktualizace Genre třídy.</span><span class="sxs-lookup"><span data-stu-id="28f63-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="28f63-128">Přidání aplikace\_složky dat</span><span class="sxs-lookup"><span data-stu-id="28f63-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="28f63-129">Přidáme aplikace\_adresář dat do našich projektu pro uložení naše soubory databáze SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="28f63-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="28f63-130">Aplikace\_dat je speciální adresář v technologii ASP.NET, která už má správné zabezpečení přístupová oprávnění pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="28f63-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="28f63-131">V projektu nabídce vyberte Přidat složku ASP.NET a pak aplikaci\_Data.</span><span class="sxs-lookup"><span data-stu-id="28f63-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="28f63-132">Vytvoření připojovacího řetězce v souboru web.config</span><span class="sxs-lookup"><span data-stu-id="28f63-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="28f63-133">Konfigurační soubor webu přidáme pár řádků, tak, aby rozhraní Entity Framework umí připojit do databáze hotový.</span><span class="sxs-lookup"><span data-stu-id="28f63-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="28f63-134">Poklikejte na soubor Web.config umístěný v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="28f63-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="28f63-135">Přejděte do dolní části tohoto souboru a přidejte &lt;connectionStrings&gt; části přímo nad poslední řádek, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="28f63-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="28f63-136">Přidání třídy kontextu</span><span class="sxs-lookup"><span data-stu-id="28f63-136">Adding a Context Class</span></span>

<span data-ttu-id="28f63-137">Klikněte pravým tlačítkem na složku modely a přidejte novou třídu s názvem MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="28f63-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="28f63-138">Tato třída bude představují kontext databáze Entity Framework, bude zpracovávat naše vytvořit, číst, aktualizovat a odstranit operací pro nás.</span><span class="sxs-lookup"><span data-stu-id="28f63-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="28f63-139">Kód pro tuto třídu je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="28f63-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="28f63-140">Je to – neexistuje žádná další konfigurace, speciální rozhraní, atd. Rozšířením základní třídy DbContext Naše třída MusicStoreEntities je schopná zpracovat naše databázových operací pro nás.</span><span class="sxs-lookup"><span data-stu-id="28f63-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="28f63-141">Teď, když My jsme který připojili, umožňuje přidávat několik další vlastnosti pro naše třídy modelu chcete využít výhod některé další informace o našich databáze.</span><span class="sxs-lookup"><span data-stu-id="28f63-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="28f63-142">Přidání naše data katalogu úložiště</span><span class="sxs-lookup"><span data-stu-id="28f63-142">Adding our store catalog data</span></span>

<span data-ttu-id="28f63-143">Rozhraní Entity Framework, který přidá "počáteční" dat do nově vytvořené databáze jsme bude využít výhod funkce.</span><span class="sxs-lookup"><span data-stu-id="28f63-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="28f63-144">Toto bude naše katalog store se seznamem žánry, umělci a alb přednastavit.</span><span class="sxs-lookup"><span data-stu-id="28f63-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="28f63-145">Stahování MvcMusicStore Assets.zip - která součástí naší soubory návrhu lokality použité v tomto kurzu - má soubor třídy s těmito daty počáteční hodnoty, umístěný ve složce s názvem kódu.</span><span class="sxs-lookup"><span data-stu-id="28f63-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="28f63-146">V rámci kód / složku modely, vyhledejte soubor SampleData.cs a umístěte jej do složky modely v našem projektu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="28f63-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="28f63-147">Teď je potřeba přidat jeden řádek kódu rozhraní Entity Framework říct o SampleData třídy.</span><span class="sxs-lookup"><span data-stu-id="28f63-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="28f63-148">Poklikejte na soubor Global.asax v kořenovém adresáři projektu otevřete a přidejte následující řádek na začátek aplikace\_Start – metoda.</span><span class="sxs-lookup"><span data-stu-id="28f63-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="28f63-149">V tomto okamžiku dokončení práce potřebné ke konfiguraci Entity Framework pro naše projekt.</span><span class="sxs-lookup"><span data-stu-id="28f63-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="28f63-150">Dotaz na databázi</span><span class="sxs-lookup"><span data-stu-id="28f63-150">Querying the Database</span></span>

<span data-ttu-id="28f63-151">Teď umožňuje aktualizovat naše StoreController tak, aby místo použití "fiktivní data" místo zavolá do databáze k dotazování všechny jeho informace.</span><span class="sxs-lookup"><span data-stu-id="28f63-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="28f63-152">Začneme deklarováním pole na **StoreController** pro uložení instance třídy MusicStoreEntities s názvem storeDB:</span><span class="sxs-lookup"><span data-stu-id="28f63-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="28f63-153">Aktualizace Index úložiště pro dotazování databáze</span><span class="sxs-lookup"><span data-stu-id="28f63-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="28f63-154">Třída MusicStoreEntities se spravuje pomocí rozhraní Entity Framework a zpřístupňuje vlastnost kolekce pro každou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="28f63-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="28f63-155">Umožňuje aktualizovat naše StoreController indexu akce načtení všech žánry v naší databázi.</span><span class="sxs-lookup"><span data-stu-id="28f63-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="28f63-156">Dříve jsme se to pevně kódováno datech řetězce.</span><span class="sxs-lookup"><span data-stu-id="28f63-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="28f63-157">Nyní můžete místo toho právě používáme kontext Entity Framework Generes kolekce:</span><span class="sxs-lookup"><span data-stu-id="28f63-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="28f63-158">Žádné změny, musí dojít naše zobrazit šablonu, protože jsme se stále vrací stejnou StoreIndexViewModel jsme před - jsme se právě vrácení dynamická data z databáze teď vrátil.</span><span class="sxs-lookup"><span data-stu-id="28f63-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="28f63-159">Když jsme spusťte projekt znovu a navštívíte adresu "/ ukládání", jsme se zobrazí seznam všech žánry v naší databázi:</span><span class="sxs-lookup"><span data-stu-id="28f63-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="28f63-160">Aktualizace procházet úložiště a podrobnosti používat živá data</span><span class="sxs-lookup"><span data-stu-id="28f63-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="28f63-161">S/úložiště/procházet? genre =*[některá genre]* metody akce, jsme při hledání Genre podle názvu.</span><span class="sxs-lookup"><span data-stu-id="28f63-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="28f63-162">Očekáváme pouze jeden výsledek, vzhledem k tomu, že jsme někdy by neměl mít dvě položky na stejný název Genre, a proto jsme můžete použít. Rozšíření Single() v technologii LINQ dotaz na příslušný objekt Genre takto (nevkládejte to ještě):</span><span class="sxs-lookup"><span data-stu-id="28f63-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="28f63-163">Jeden metoda přebírá jako parametr, který určuje, že má jeden objekt Genre tak, aby jeho název odpovídá hodnotě, kterou jsme jste definovali výrazu Lambda.</span><span class="sxs-lookup"><span data-stu-id="28f63-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="28f63-164">V případě výše jsme se jeden objekt Genre načítání s hodnotou název odpovídající Disco.</span><span class="sxs-lookup"><span data-stu-id="28f63-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="28f63-165">Provedeme výhod funkce rozhraní Entity Framework, která umožňuje nám udávajících dalších entit v relaci, kterou chceme také načíst, pokud je objekt Genre načteny.</span><span class="sxs-lookup"><span data-stu-id="28f63-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="28f63-166">Tato funkce je volána Shaping výsledek dotazu a umožňuje nám chcete snížit počet, kolikrát je potřeba přístup k databázi načíst všechny informace, které potřebujeme.</span><span class="sxs-lookup"><span data-stu-id="28f63-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="28f63-167">Chceme předem načíst alb pro Genre načteme, takže jsme budete aktualizovat dotaz zahrnout z Genres.Include("Albums") indikující, že má také související alb.</span><span class="sxs-lookup"><span data-stu-id="28f63-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="28f63-168">To je efektivnější, protože ho načte naše Genre a alb data v požadavku jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="28f63-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="28f63-169">S vysvětlením stranou zde je, jak vypadá naše aktualizované akce kontroleru procházení:</span><span class="sxs-lookup"><span data-stu-id="28f63-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="28f63-170">Nyní jsme můžete aktualizovat zobrazení procházet úložiště tak, aby alb, které jsou k dispozici v jednotlivých Genre.</span><span class="sxs-lookup"><span data-stu-id="28f63-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="28f63-171">Otevřete zobrazení šablony (v /Views/Store/Browse.cshtml) a přidejte seznamu s odrážkami alb, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="28f63-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="28f63-172">Spuštění aplikace a procházení/úložiště / procházet? genre = Jazz ukazuje, které jsou nyní probíhá načtený naše výsledky z databáze, zobrazení všech alb v našem vybrané Genre.</span><span class="sxs-lookup"><span data-stu-id="28f63-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="28f63-173">Vytočit stejné změnit na našem/Store/podrobnosti / [id] adresy URL a nahraďte naše fiktivní data databázový dotaz, který načte Album s ID odpovídá hodnotě parametru.</span><span class="sxs-lookup"><span data-stu-id="28f63-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="28f63-174">Spuštění aplikace a procházení k /Store/Details/1 ukazuje, že naše výsledky se nyní patrně z databáze.</span><span class="sxs-lookup"><span data-stu-id="28f63-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="28f63-175">Teď, když chcete zobrazit album podle ID alb je nastavili naší stránce s podrobnostmi o úložiště, můžeme aktualizovat **Procházet** zobrazení pro odkaz na zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="28f63-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="28f63-176">Použijeme Html.ActionLink, přesně, jako jsme to udělali propojení z úložiště indexu procházet úložiště na konci v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="28f63-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="28f63-177">Úplný zdrojový Procházet zobrazení se zobrazí pod.</span><span class="sxs-lookup"><span data-stu-id="28f63-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="28f63-178">Nemohli jsme teď moct přejít z naší stránce s úložiště na stránku Genre, kde jsou uvedeny dostupné alb, a kliknutím na album jsme můžete zobrazit podrobnosti o této alb.</span><span class="sxs-lookup"><span data-stu-id="28f63-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

>[!div class="step-by-step"]
<span data-ttu-id="28f63-179">[Předchozí](mvc-music-store-part-3.md)
[další](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="28f63-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
