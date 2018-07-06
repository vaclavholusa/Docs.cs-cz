---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 | Dokumentace Microsoftu
author: Erikre
description: Tato série podrobných kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio působivý...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: cda0c6239e2f10186641ab315837440d83b2fda6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841393"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="e106a-103">Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e106a-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="e106a-104">podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="e106a-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="e106a-105">[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="e106a-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="e106a-106">Tato série podrobných kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="e106a-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="e106a-107">Webové formuláře ASP.NET kvíz</span><span class="sxs-lookup"><span data-stu-id="e106a-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="e106a-108">Otestujte svoje znalosti a posílit klíčové koncepty pomocí technologie ASP.NET webové formuláře kvíz.</span><span class="sxs-lookup"><span data-stu-id="e106a-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="e106a-109">Tohoto kvízu je navržená speciálně z obsahu obsažené v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="e106a-110">Každý dotaz do ukončení kvízu obsahuje vysvětlení společně s odkazy na další pokyny.</span><span class="sxs-lookup"><span data-stu-id="e106a-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="e106a-111">Úvod</span><span class="sxs-lookup"><span data-stu-id="e106a-111">Introduction</span></span>

<span data-ttu-id="e106a-112">Tato série kurzů vás provede kroky potřebné k vytvoření aplikace webových formulářů ASP.NET pomocí Visual Studio Express 2013 pro webové aplikace a technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e106a-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="e106a-113">Aplikace vytvoříte jmenuje **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="e106a-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="e106a-114">Je zjednodušený příklad webového serveru front úložiště, který se prodává zboží online.</span><span class="sxs-lookup"><span data-stu-id="e106a-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="e106a-115">V této sérii kurzů označuje nové funkce, které jsou k dispozici v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e106a-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="e106a-116">Komentáře jsou úvodní a uděláme veškeré úsilí, aby aktualizace na základě vašich návrhů v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="e106a-117">Stažení se dokončilo projektu</span><span class="sxs-lookup"><span data-stu-id="e106a-117">Download completed project</span></span>

<span data-ttu-id="e106a-118">Můžete stáhnout projektu C#, který obsahuje dokončení kurzu.</span><span class="sxs-lookup"><span data-stu-id="e106a-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="e106a-119">[Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – na adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="e106a-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="e106a-120">Zkontrolujte obsah provedením související kvíz webových formulářů ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e106a-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="e106a-121">Po dokončení tohoto kurzu, otestujte svoje znalosti a posílit klíčové koncepty provedením [ASP.NET Web Forms kvíz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="e106a-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="e106a-122">Tohoto kvízu je navržená speciálně z obsahu obsažené v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="e106a-123">Každý dotaz do ukončení kvízu obsahuje vysvětlení společně s odkazy na další pokyny.</span><span class="sxs-lookup"><span data-stu-id="e106a-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="e106a-124">Webové formuláře ASP.NET kvíz</span><span class="sxs-lookup"><span data-stu-id="e106a-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="e106a-125">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="e106a-125">Audience</span></span>

<span data-ttu-id="e106a-126">Jeho zamýšlenou cílovou skupinou v této sérii kurzů je zkušení vývojáři, kteří jsou novinky webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e106a-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="e106a-127">Vývojářem dotčené v této řadě kurzů by měl mít následující schopnosti:</span><span class="sxs-lookup"><span data-stu-id="e106a-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="e106a-128">Známý s objektem orientovaný programovací jazyk (OOP)</span><span class="sxs-lookup"><span data-stu-id="e106a-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="e106a-129">Dobře známé koncepty vývoj pro Web (HTML, CSS a JavaScript)</span><span class="sxs-lookup"><span data-stu-id="e106a-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="e106a-130">Dobře známé koncepty relační databáze</span><span class="sxs-lookup"><span data-stu-id="e106a-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="e106a-131">Dobře známé koncepty n vrstvou architekturu</span><span class="sxs-lookup"><span data-stu-id="e106a-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="e106a-132">Pokud jste zajímat téma oblastech uvedených výše, vezměte v úvahu revize následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="e106a-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="e106a-133">Začínáme s Visual C#</span><span class="sxs-lookup"><span data-stu-id="e106a-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="e106a-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="e106a-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="e106a-135">Relační databáze</span><span class="sxs-lookup"><span data-stu-id="e106a-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="e106a-136">Vícevrstvé architektury</span><span class="sxs-lookup"><span data-stu-id="e106a-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="e106a-137">Funkce aplikací</span><span class="sxs-lookup"><span data-stu-id="e106a-137">Application Features</span></span>

<span data-ttu-id="e106a-138">Webový formulář ASP.NET funkce uvedené v této sérii:</span><span class="sxs-lookup"><span data-stu-id="e106a-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="e106a-139">Projekt webové aplikace (ne webový projekt)</span><span class="sxs-lookup"><span data-stu-id="e106a-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="e106a-140">webové formuláře</span><span class="sxs-lookup"><span data-stu-id="e106a-140">Web Forms</span></span>
- <span data-ttu-id="e106a-141">Stránky předlohy, konfigurace</span><span class="sxs-lookup"><span data-stu-id="e106a-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="e106a-142">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="e106a-142">Bootstrap</span></span>
- <span data-ttu-id="e106a-143">Entity Framework Code nejprve LocalDB</span><span class="sxs-lookup"><span data-stu-id="e106a-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="e106a-144">Žádost o ověření</span><span class="sxs-lookup"><span data-stu-id="e106a-144">Request Validation</span></span>
- <span data-ttu-id="e106a-145">Ovládací prvky dat silného typu vazby anotacemi dat modelu a hodnota se zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="e106a-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="e106a-146">Protokol SSL a protokolem OAuth</span><span class="sxs-lookup"><span data-stu-id="e106a-146">SSL and OAuth</span></span>
- <span data-ttu-id="e106a-147">ASP.NET Identity, konfigurace a autorizace</span><span class="sxs-lookup"><span data-stu-id="e106a-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="e106a-148">Nerušivý ověření</span><span class="sxs-lookup"><span data-stu-id="e106a-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="e106a-149">Směrování</span><span class="sxs-lookup"><span data-stu-id="e106a-149">Routing</span></span>
- <span data-ttu-id="e106a-150">Zpracování chyb v ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e106a-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="e106a-151">Scénáře aplikací a úloh</span><span class="sxs-lookup"><span data-stu-id="e106a-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="e106a-152">Úlohy, které jsme vám ukázali v této sérii patří:</span><span class="sxs-lookup"><span data-stu-id="e106a-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="e106a-153">Vytváření, revizí a spuštění nového projektu</span><span class="sxs-lookup"><span data-stu-id="e106a-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="e106a-154">Vytvoření struktury databáze</span><span class="sxs-lookup"><span data-stu-id="e106a-154">Creating the database structure</span></span>
- <span data-ttu-id="e106a-155">Inicializace a synchronizace replik indexů databáze</span><span class="sxs-lookup"><span data-stu-id="e106a-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="e106a-156">Přizpůsobení uživatelského rozhraní pomocí styly, grafiku a na stránku předlohy</span><span class="sxs-lookup"><span data-stu-id="e106a-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="e106a-157">Přidání stránky a navigace</span><span class="sxs-lookup"><span data-stu-id="e106a-157">Adding pages and navigation</span></span>
- <span data-ttu-id="e106a-158">Zobrazení Podrobnosti o nabídce a data produktu.</span><span class="sxs-lookup"><span data-stu-id="e106a-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="e106a-159">Vytvoření nákupního košíku</span><span class="sxs-lookup"><span data-stu-id="e106a-159">Creating a shopping cart</span></span>
- <span data-ttu-id="e106a-160">Podpora přidání SSL a protokolem OAuth</span><span class="sxs-lookup"><span data-stu-id="e106a-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="e106a-161">Přidání způsob platby</span><span class="sxs-lookup"><span data-stu-id="e106a-161">Adding a payment method</span></span>
- <span data-ttu-id="e106a-162">Včetně role správce a uživatele k aplikaci</span><span class="sxs-lookup"><span data-stu-id="e106a-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="e106a-163">Omezení přístupu ke konkrétní stránky a složka</span><span class="sxs-lookup"><span data-stu-id="e106a-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="e106a-164">Po nahrání souboru do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e106a-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="e106a-165">Implementace ověření vstupu</span><span class="sxs-lookup"><span data-stu-id="e106a-165">Implementing input validation</span></span>
- <span data-ttu-id="e106a-166">Registrace trasy pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="e106a-166">Registering routes for the web application</span></span>
- <span data-ttu-id="e106a-167">Implementace zpracování chyb a protokolování chyb</span><span class="sxs-lookup"><span data-stu-id="e106a-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="e106a-168">Přehled</span><span class="sxs-lookup"><span data-stu-id="e106a-168">Overview</span></span>

<span data-ttu-id="e106a-169">Pokud jste ještě na webové formuláře ASP.NET ale mít seznámení se s koncepty programování, mít správný kurz.</span><span class="sxs-lookup"><span data-stu-id="e106a-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="e106a-170">Pokud jste již obeznámeni s webových formulářů ASP.NET, vám může přinést za nové funkce, které jsou k dispozici v technologii ASP.NET 4.5 v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="e106a-171">Pokud nejste obeznámeni s programování konceptů a webových formulářů ASP.NET, najdete v dalších kurzů, které jsou k dispozici ve webových formulářích [Začínáme](../../../index.md) části na webu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e106a-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="e106a-172">Konkrétní **nejnovější** technologie ASP.NET 4.5 obsahuje zadaný v této webové formuláře sérii patří následující:</span><span class="sxs-lookup"><span data-stu-id="e106a-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="e106a-173">Jednoduché uživatelské rozhraní pro vytváření projektů nabídku [podporu pro více platforem ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (webové formuláře, MVC a webového rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="e106a-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="e106a-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), rozložení a motivy architektura, která poskytuje interaktivní možnosti návrhu a motivů.</span><span class="sxs-lookup"><span data-stu-id="e106a-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="e106a-175">[ASP.NET Identity](../../../../identity/index.md), nový systém členství technologie ASP.NET, který funguje stejně ve všech platforem ASP.NET a funguje s webhosting softwaru než služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e106a-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="e106a-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizace k Entity Frameworku, který umožňuje načtení a manipulaci s daty jako silně typované objekty, přístup k datům asynchronně, zpracování přechodné chyby připojení a protokolování příkazů jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="e106a-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="e106a-177">Úplný seznam funkcí technologie ASP.NET 4.5, naleznete v tématu [ASP.NET and Web Tools pro Visual Studio 2013 – poznámky k](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="e106a-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="e106a-178">Ukázkové aplikace Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="e106a-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="e106a-179">Následující snímek obrazovky poskytnout rychlý přehled aplikace webových formulářů ASP.NET, který vytvoříte v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="e106a-180">Při spuštění aplikace z Visual Studio Express 2013 for Web, zobrazí se následující webové domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e106a-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Vzorkový – výchozí stránku](introduction-and-overview/_static/image1.png)

<span data-ttu-id="e106a-182">Můžete zaregistrovat jako nový uživatel nebo Přihlaste se jako stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="e106a-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="e106a-183">Navigace je k dispozici v horní části pro každou kategorii produktu načtením dostupných produktů z databáze.</span><span class="sxs-lookup"><span data-stu-id="e106a-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="e106a-184">Výběrem odkazu produkty budou moci zobrazit seznam všech dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="e106a-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Vzorkový - produkty](introduction-and-overview/_static/image2.png)

<span data-ttu-id="e106a-186">Uvidíte také podrobnosti jednotlivých produktů výběrem některé z uvedených produktů.</span><span class="sxs-lookup"><span data-stu-id="e106a-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Vzorkový – podrobnosti o produktu](introduction-and-overview/_static/image3.png)

<span data-ttu-id="e106a-188">Jako uživatel můžete registrovat a přihlaste se pomocí funkce výchozí šablony webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="e106a-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="e106a-189">V tomto kurzu také vysvětluje, jak se přihlásit pomocí existujícího účtu Gmail.</span><span class="sxs-lookup"><span data-stu-id="e106a-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="e106a-190">Kromě toho můžete se přihlásit jako správce přidávat a odebírat produkty z databáze.</span><span class="sxs-lookup"><span data-stu-id="e106a-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Přihlaste se na adresář Wingtip Toys-](introduction-and-overview/_static/image4.png)

<span data-ttu-id="e106a-192">Jakmile jste přihlášení jako uživatel, můžete přidat produkty do nákupního košíku a ověření přes PayPal.</span><span class="sxs-lookup"><span data-stu-id="e106a-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="e106a-193">Všimněte si, že je funkční spolu s sandboxu pro vývojáře společnosti PayPal určený této ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e106a-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="e106a-194">Žádný skutečný peněžní transakce nebude trvat místo.</span><span class="sxs-lookup"><span data-stu-id="e106a-194">No actual money transaction will take place.</span></span>

![Vzorkový – nákupní košík](introduction-and-overview/_static/image5.png)

<span data-ttu-id="e106a-196">PayPal potvrdí účtu, pořadí a platební údaje.</span><span class="sxs-lookup"><span data-stu-id="e106a-196">PayPal will confirm your account, order, and payment information.</span></span>

![Vzorkový - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="e106a-198">Po návratu z PayPal, můžete zkontrolovat a dokončete vaši objednávku.</span><span class="sxs-lookup"><span data-stu-id="e106a-198">After returning from PayPal, you can review and complete your order.</span></span>

![Vzorkový – pořadí revize](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="e106a-200">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e106a-200">Prerequisites</span></span>

<span data-ttu-id="e106a-201">Než začnete, ujistěte se, že máte v počítači nainstalovaný následující software:</span><span class="sxs-lookup"><span data-stu-id="e106a-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="e106a-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="e106a-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="e106a-203">Rozhraní .NET Framework se instaluje automaticky.</span><span class="sxs-lookup"><span data-stu-id="e106a-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="e106a-204">V této sérii kurzů používá Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="e106a-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="e106a-205">K dokončení této sérii kurzů můžete použít buď Microsoft Visual Studio Express 2013 for Web nebo Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e106a-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e106a-206">Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 for Web bude často se označuje jako Visual Studio v celé této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="e106a-207">Pokud už máte verzi sady Visual Studio nainstalovaný, proces instalace u stávající verzi nainstalovat Visual Studio 2013 nebo Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="e106a-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="e106a-208">Servery, které jste vytvořili v předchozích verzích se dají otevřít v sadě Visual Studio 2013 a pokračujte otevřením v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="e106a-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e106a-209">Tento názorný průvodce předpokládá, že jste vybrali *vývoj pro Web* kolekce nastavení při prvním spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e106a-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="e106a-210">Další informace najdete v tématu [postupy: Výběr nastavení prostředí vývoje webu](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="e106a-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="e106a-211">Stažení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e106a-211">Download the Sample Application</span></span>

<span data-ttu-id="e106a-212">Po instalaci předpokladů, budete chtít začít vytvářet nový webový projekt, který se zobrazí v této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="e106a-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="e106a-213">Pokud byste chtěli **volitelně** spuštění ukázkové aplikace, která vytvoří v této sérii kurzů, můžete ho stáhnout z webu MSDN ukázky.</span><span class="sxs-lookup"><span data-stu-id="e106a-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="e106a-214">Tento soubor ke stažení obsahuje následující prvky:</span><span class="sxs-lookup"><span data-stu-id="e106a-214">This download contains the following:</span></span>

- <span data-ttu-id="e106a-215">Ukázková aplikace v *Northwind* složky.</span><span class="sxs-lookup"><span data-stu-id="e106a-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="e106a-216">Prostředky použité k vytvoření ukázkové aplikace v *Northwind prostředky* složky *Northwind* složky.</span><span class="sxs-lookup"><span data-stu-id="e106a-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="e106a-217">Stáhněte soubor z webu MSDN ukázky:</span><span class="sxs-lookup"><span data-stu-id="e106a-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="e106a-218">[Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – na adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="e106a-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="e106a-219">Stažení <em>ZIP</em> souboru.</span><span class="sxs-lookup"><span data-stu-id="e106a-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="e106a-220">Zobrazíte dokončený projekt, který vytvoří v této sérii kurzů, vyhledejte a vyberte <em>jazyka C#</em>složky <em>ZIP</em> souboru.</span><span class="sxs-lookup"><span data-stu-id="e106a-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="e106a-221">Uložit <em>jazyka C#</em> folderto složce můžete použít pro práci s projekty sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e106a-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="e106a-222">Ve výchozím nastavení je složky projektů Visual Studio 2013 následující:</span><span class="sxs-lookup"><span data-stu-id="e106a-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="e106a-223"><strong>C:\Users&#92;</strong><strong><em>&lt;uživatelské jméno&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="e106a-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="e106a-224">Přejmenovat ***jazyka C#*** složku ***Northwind***.</span><span class="sxs-lookup"><span data-stu-id="e106a-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="e106a-225">Pokud už máte složku s názvem *Northwind* ve vaší složce projekty dočasně Přejmenujte tuto složku před přejmenováním *jazyka C#* složku *Northwind*.</span><span class="sxs-lookup"><span data-stu-id="e106a-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="e106a-226">Chcete-li spustit dokončený projekt, otevřete *Northwind* složky a dvojím kliknutím *WingtipToys.sln* souboru.</span><span class="sxs-lookup"><span data-stu-id="e106a-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="e106a-227">Visual Studio 2013 se otevře projekt.</span><span class="sxs-lookup"><span data-stu-id="e106a-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="e106a-228">Pak klikněte pravým tlačítkem *Default.aspx* souborů v okně Průzkumník řešení a v místní nabídce klikněte na tlačítko zobrazit v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e106a-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="e106a-229">Kurz podpory a komentáře</span><span class="sxs-lookup"><span data-stu-id="e106a-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="e106a-230">Pomocí informací v části Q a A je součástí [Začínáme se službou webových formulářů ASP.NET 4.5 a Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ukázka (C#) pro jakékoli dotazy nebo připomínky.</span><span class="sxs-lookup"><span data-stu-id="e106a-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="e106a-231">Komentáře v této sérii kurzů jsou úvodní a při aktualizaci v této sérii kurzů veškeré úsilí, bude proveden rozdělení účet opravy nebo návrhy na vylepšení, která jsou k dispozici v kurzu komentáře.</span><span class="sxs-lookup"><span data-stu-id="e106a-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="e106a-232">Pokud dojde k chybě při vývoji nebo na webu se nespustí správně, chybové zprávy mohou být složité možné příčiny problému nebo nemusí vysvětlují, jak ho opravit.</span><span class="sxs-lookup"><span data-stu-id="e106a-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="e106a-233">Abychom vám pomohli s uvedeny některé obvyklé scénáře problém, můžete použít také [fóra ASP.NET](https://forums.asp.net/) nebo části Q a A je součástí [Začínáme se službou webových formulářů ASP.NET 4.5 a Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) vzorku.</span><span class="sxs-lookup"><span data-stu-id="e106a-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="e106a-234">Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte se podívat výše uvedené umístění.</span><span class="sxs-lookup"><span data-stu-id="e106a-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e106a-235">Next</span><span class="sxs-lookup"><span data-stu-id="e106a-235">Next</span></span>](create-the-project.md)
