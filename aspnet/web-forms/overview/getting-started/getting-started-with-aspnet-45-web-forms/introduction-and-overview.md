---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 | Microsoft Docs"
author: Erikre
description: "Tento podrobný kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Expres..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ee3e244c4ed29384d11c7acc1440692d3f9b23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="e76fe-103">Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e76fe-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="e76fe-104">Podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="e76fe-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="e76fe-105">[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="e76fe-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="e76fe-106">Tento podrobný kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web.</span><span class="sxs-lookup"><span data-stu-id="e76fe-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="e76fe-107">ASP.NET – webové formuláře kvízu s časovým limitem</span><span class="sxs-lookup"><span data-stu-id="e76fe-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="e76fe-108">Otestujte svoje znalosti a posílit klíčové koncepty provedením kvízu ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e76fe-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="e76fe-109">Tato kvízu s časovým limitem byl speciálně z obsahu, které jsou součástí tohoto kurzu řady.</span><span class="sxs-lookup"><span data-stu-id="e76fe-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="e76fe-110">Každý dotaz v kvízu obsahuje vysvětlení spolu s odkazy na další pokyny.</span><span class="sxs-lookup"><span data-stu-id="e76fe-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="e76fe-111">Úvod</span><span class="sxs-lookup"><span data-stu-id="e76fe-111">Introduction</span></span>

<span data-ttu-id="e76fe-112">Tato série kurzů vás provede kroky potřebné k vytváření aplikací webových formulářů ASP.NET pomocí Visual Studio Express 2013 pro Web a technologie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e76fe-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="e76fe-113">Aplikace vytvoříte jmenuje **adresář Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="e76fe-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="e76fe-114">Je zjednodušená příklad úložiště front web, který se prodává položky online.</span><span class="sxs-lookup"><span data-stu-id="e76fe-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="e76fe-115">Tento kurz řady označuje nové funkce, které jsou k dispozici v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e76fe-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="e76fe-116">Komentáře jsou úvodní a vytočit veškeré úsilí aktualizovat tento kurz řady podle vaší návrhy.</span><span class="sxs-lookup"><span data-stu-id="e76fe-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="e76fe-117">Stažení dokončit projektu</span><span class="sxs-lookup"><span data-stu-id="e76fe-117">Download completed project</span></span>

<span data-ttu-id="e76fe-118">Můžete si stáhnout projekt C#, který obsahuje dokončené kurzu.</span><span class="sxs-lookup"><span data-stu-id="e76fe-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="e76fe-119">[Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="e76fe-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="e76fe-120">Zkontrolujte obsah provedením související kvízu webových formulářů ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e76fe-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="e76fe-121">Po dokončení tohoto kurzu své znalosti a posílit klíčové koncepty provedením [ASP.NET Web Forms kvízu](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="e76fe-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="e76fe-122">Tato kvízu s časovým limitem byl speciálně z obsahu, které jsou součástí tohoto kurzu řady.</span><span class="sxs-lookup"><span data-stu-id="e76fe-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="e76fe-123">Každý dotaz v kvízu obsahuje vysvětlení spolu s odkazy na další pokyny.</span><span class="sxs-lookup"><span data-stu-id="e76fe-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="e76fe-124">ASP.NET – webové formuláře kvízu s časovým limitem</span><span class="sxs-lookup"><span data-stu-id="e76fe-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="e76fe-125">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="e76fe-125">Audience</span></span>

<span data-ttu-id="e76fe-126">Předpokládanou cílovou skupinou série tento kurz je zkušeného vývojáře, kteří jsou nové webových formulářů ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e76fe-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="e76fe-127">Vývojář zájem o tento kurz řady musí mít následující dovedností:</span><span class="sxs-lookup"><span data-stu-id="e76fe-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="e76fe-128">Neznáte objekt zaměřené na konkrétní programovací jazyk (mimoprocesová aplikace VOLÁNA)</span><span class="sxs-lookup"><span data-stu-id="e76fe-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="e76fe-129">Seznamte s koncepty vývoje webu (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="e76fe-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="e76fe-130">Seznamte s koncepty relační databáze</span><span class="sxs-lookup"><span data-stu-id="e76fe-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="e76fe-131">Seznamte s koncepty n vrstvá architektura</span><span class="sxs-lookup"><span data-stu-id="e76fe-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="e76fe-132">Pokud vás zajímá kontrola oblasti uvedené výše, vezměte v úvahu kontrola následující obsah:</span><span class="sxs-lookup"><span data-stu-id="e76fe-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="e76fe-133">Začínáme s jazykem Visual C#</span><span class="sxs-lookup"><span data-stu-id="e76fe-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="e76fe-134">[Vývoj webových](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="e76fe-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="e76fe-135">Relační databáze</span><span class="sxs-lookup"><span data-stu-id="e76fe-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="e76fe-136">Třívrstvá architektura</span><span class="sxs-lookup"><span data-stu-id="e76fe-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="e76fe-137">Funkce aplikací</span><span class="sxs-lookup"><span data-stu-id="e76fe-137">Application Features</span></span>

<span data-ttu-id="e76fe-138">Funkce webového formuláře ASP.NET uvedené v této série zahrnují:</span><span class="sxs-lookup"><span data-stu-id="e76fe-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="e76fe-139">Projekt webové aplikace (není webový projekt)</span><span class="sxs-lookup"><span data-stu-id="e76fe-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="e76fe-140">webové formuláře</span><span class="sxs-lookup"><span data-stu-id="e76fe-140">Web Forms</span></span>
- <span data-ttu-id="e76fe-141">Stránky předlohy, konfigurace</span><span class="sxs-lookup"><span data-stu-id="e76fe-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="e76fe-142">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="e76fe-142">Bootstrap</span></span>
- <span data-ttu-id="e76fe-143">Rozhraní Entity Framework Code nejprve LocalDB</span><span class="sxs-lookup"><span data-stu-id="e76fe-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="e76fe-144">Ověření žádosti</span><span class="sxs-lookup"><span data-stu-id="e76fe-144">Request Validation</span></span>
- <span data-ttu-id="e76fe-145">Silného typu ovládací prvky datových, Model vazby datových poznámek a hodnota zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="e76fe-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="e76fe-146">Protokol SSL a OAuth</span><span class="sxs-lookup"><span data-stu-id="e76fe-146">SSL and OAuth</span></span>
- <span data-ttu-id="e76fe-147">ASP.NET Identity, konfiguraci a autorizaci</span><span class="sxs-lookup"><span data-stu-id="e76fe-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="e76fe-148">Ověření nerušivého</span><span class="sxs-lookup"><span data-stu-id="e76fe-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="e76fe-149">Směrování</span><span class="sxs-lookup"><span data-stu-id="e76fe-149">Routing</span></span>
- <span data-ttu-id="e76fe-150">Zpracování chyb ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e76fe-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="e76fe-151">Scénáře aplikací a úloh</span><span class="sxs-lookup"><span data-stu-id="e76fe-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="e76fe-152">Úlohy předvedená v této série zahrnují:</span><span class="sxs-lookup"><span data-stu-id="e76fe-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="e76fe-153">Vytváření, kontrola a spuštění nového projektu</span><span class="sxs-lookup"><span data-stu-id="e76fe-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="e76fe-154">Vytvoření struktury databáze</span><span class="sxs-lookup"><span data-stu-id="e76fe-154">Creating the database structure</span></span>
- <span data-ttu-id="e76fe-155">Inicializace a synchronizace replik indexů databáze</span><span class="sxs-lookup"><span data-stu-id="e76fe-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="e76fe-156">Přizpůsobení uživatelského rozhraní pomocí stylů, grafiky a stránky předlohy</span><span class="sxs-lookup"><span data-stu-id="e76fe-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="e76fe-157">Přidání stránky a navigace</span><span class="sxs-lookup"><span data-stu-id="e76fe-157">Adding pages and navigation</span></span>
- <span data-ttu-id="e76fe-158">Zobrazení nabídky podrobnosti a data produktu.</span><span class="sxs-lookup"><span data-stu-id="e76fe-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="e76fe-159">Vytváření nákupní košík</span><span class="sxs-lookup"><span data-stu-id="e76fe-159">Creating a shopping cart</span></span>
- <span data-ttu-id="e76fe-160">Podpora přidání SSL a OAuth</span><span class="sxs-lookup"><span data-stu-id="e76fe-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="e76fe-161">Přidání způsob platby</span><span class="sxs-lookup"><span data-stu-id="e76fe-161">Adding a payment method</span></span>
- <span data-ttu-id="e76fe-162">Včetně roli správce a uživatele k aplikaci</span><span class="sxs-lookup"><span data-stu-id="e76fe-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="e76fe-163">Omezení přístupu k konkrétní stránky a složky</span><span class="sxs-lookup"><span data-stu-id="e76fe-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="e76fe-164">Nahrání souboru do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e76fe-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="e76fe-165">Implementace ověřování vstupu</span><span class="sxs-lookup"><span data-stu-id="e76fe-165">Implementing input validation</span></span>
- <span data-ttu-id="e76fe-166">Registrace trasy pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e76fe-166">Registering routes for the web application</span></span>
- <span data-ttu-id="e76fe-167">Implementace zpracování chyb a protokolování chyb</span><span class="sxs-lookup"><span data-stu-id="e76fe-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="e76fe-168">Přehled</span><span class="sxs-lookup"><span data-stu-id="e76fe-168">Overview</span></span>

<span data-ttu-id="e76fe-169">Pokud jste novým uživatelem webových formulářů ASP.NET ale mít znalost koncepce programování, máte správné kurzu.</span><span class="sxs-lookup"><span data-stu-id="e76fe-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="e76fe-170">Pokud jste již obeznámeni s webovými formuláři ASP.NET, můžete využívat výhod tento kurz řady nové funkce, které jsou k dispozici v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e76fe-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="e76fe-171">Pokud jste obeznámeni s programování konceptů a webových formulářů ASP.NET, najdete v části Další kurzy součástí webové formuláře [Začínáme](../../../index.md) části na webu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e76fe-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="e76fe-172">Konkrétní **nejnovější** technologie ASP.NET 4.5 funkce poskytované v této webové formuláře kurz řady zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="e76fe-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="e76fe-173">Jednoduché uživatelského rozhraní pro vytváření projektů v rámci této nabídky [podporu pro více rozhraní ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (webových formulářů, MVC a webového rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="e76fe-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="e76fe-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), rozložení a motivů rozhraní, které zajišťuje přizpůsobivý návrh a motivů.</span><span class="sxs-lookup"><span data-stu-id="e76fe-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="e76fe-175">[ASP.NET Identity](../../../../identity/index.md), nový systém členství technologie ASP.NET, který funguje stejně ve všech rozhraní ASP.NET a funguje s webového hostingu softwaru než služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e76fe-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="e76fe-176">[Rozhraní Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizace rozhraní Entity Framework, který umožňuje načíst a pracovat s daty jako silného typu objektů, přístup k datům asynchronně, zpracování chyb přechodný připojení a protokolu příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="e76fe-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="e76fe-177">Úplný seznam funkcí technologie ASP.NET 4.5, najdete v části [technologie ASP.NET a webové nástroje pro poznámky k verzi 2013 Visual Studio](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="e76fe-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="e76fe-178">Adresář Wingtip Toys ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e76fe-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="e76fe-179">Následující snímek obrazovky poskytují rychlý přehled rozhraní ASP.NET Web forms aplikace, která se vytvoří tento kurz řady.</span><span class="sxs-lookup"><span data-stu-id="e76fe-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="e76fe-180">Při spuštění aplikace z Visual Studio Express 2013 pro Web, zobrazí se následující domovskou stránku webové.</span><span class="sxs-lookup"><span data-stu-id="e76fe-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Adresář Wingtip Toys – výchozí stránka](introduction-and-overview/_static/image1.png)

<span data-ttu-id="e76fe-182">Můžete zaregistrovat jako nový uživatel nebo se přihlásit jako stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="e76fe-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="e76fe-183">Navigace je poskytovaný v horní části pro každou kategorii produktu načítání dostupných produktů z databáze.</span><span class="sxs-lookup"><span data-stu-id="e76fe-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="e76fe-184">Odkaz produktů vyberete, bude moci zobrazit seznam všech dostupných produktů.</span><span class="sxs-lookup"><span data-stu-id="e76fe-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Adresář Wingtip Toys - produktů](introduction-and-overview/_static/image2.png)

<span data-ttu-id="e76fe-186">Najdete v části Podrobnosti o jednotlivých produktů vyberete některý z uvedených produktů.</span><span class="sxs-lookup"><span data-stu-id="e76fe-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Adresář Wingtip Toys – podrobnosti o produktu](introduction-and-overview/_static/image3.png)

<span data-ttu-id="e76fe-188">Jako uživatel můžete zaregistrovat a přihlaste se pomocí funkce výchozí šablony webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="e76fe-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="e76fe-189">V tomto kurzu taky vysvětluje, jak se přihlásit pomocí existujícího účtu z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="e76fe-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="e76fe-190">Kromě toho se může přihlásit jako správce přidávat a odebírat produkty z databáze.</span><span class="sxs-lookup"><span data-stu-id="e76fe-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Adresář Wingtip Toys - přihlášení](introduction-and-overview/_static/image4.png)

<span data-ttu-id="e76fe-192">Jakmile jste přihlášení jako uživatel, můžete přidat produkty nákupní košík a ověření pomocí služby PayPal.</span><span class="sxs-lookup"><span data-stu-id="e76fe-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="e76fe-193">Všimněte si, že tato ukázková aplikace je určený ke fungovat s izolovaného prostoru PayPal pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="e76fe-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="e76fe-194">Žádná transakce skutečné peníze bude probíhat.</span><span class="sxs-lookup"><span data-stu-id="e76fe-194">No actual money transaction will take place.</span></span>

![Adresář Wingtip Toys – nákupní košík](introduction-and-overview/_static/image5.png)

<span data-ttu-id="e76fe-196">PayPal bude tak jasné, váš účet, pořadí a platebních údajů.</span><span class="sxs-lookup"><span data-stu-id="e76fe-196">PayPal will confirm your account, order, and payment information.</span></span>

![Adresář Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="e76fe-198">Po návratu z PayPal, můžete zkontrolovat a dokončit vaši objednávku.</span><span class="sxs-lookup"><span data-stu-id="e76fe-198">After returning from PayPal, you can review and complete your order.</span></span>

![Adresář Wingtip Toys - Zkontrolujte pořadí](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="e76fe-200">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e76fe-200">Prerequisites</span></span>

<span data-ttu-id="e76fe-201">Než začnete, ujistěte se, že máte v počítači nainstalován následující software:</span><span class="sxs-lookup"><span data-stu-id="e76fe-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="e76fe-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) nebo [sady Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="e76fe-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span> <span data-ttu-id="e76fe-203">Rozhraní .NET Framework se instaluje automaticky.</span><span class="sxs-lookup"><span data-stu-id="e76fe-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="e76fe-204">Tento kurz řady používá Microsoft Visual Studio Express 2013 pro Web.</span><span class="sxs-lookup"><span data-stu-id="e76fe-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="e76fe-205">K dokončení tohoto kurzu řady můžete použít buď Microsoft Visual Studio Express 2013 pro Web nebo Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e76fe-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e76fe-206">Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 pro Web se často se označuje jako Visual Studio v rámci tohoto kurzu řady.</span><span class="sxs-lookup"><span data-stu-id="e76fe-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="e76fe-207">Pokud již máte nainstalované verze sady Visual Studio, proces instalace vedle stávající verzi nainstalovat Visual Studio 2013 nebo Microsoft Visual Studio Express 2013 pro Web.</span><span class="sxs-lookup"><span data-stu-id="e76fe-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="e76fe-208">Weby, které jste vytvořili v dřívějších verzích lze otevřít v sadě Visual Studio 2013 a pokračovat v otevírání v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="e76fe-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e76fe-209">Tento návod předpokládá, že jste vybrali *vývoj webů* kolekce nastavení při prvním spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e76fe-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="e76fe-210">Další informace najdete v tématu [postupy: nastavení prostředí vyberte vývoj webové](https://msdn.microsoft.com/en-us/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="e76fe-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/en-us/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="e76fe-211">Stažení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e76fe-211">Download the Sample Application</span></span>

<span data-ttu-id="e76fe-212">Po instalaci požadavky, jste připraveni se začne vytvářet nový webový projekt, který se zobrazí v řadě tento kurz.</span><span class="sxs-lookup"><span data-stu-id="e76fe-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="e76fe-213">Pokud byste chtěli **volitelně** spusťte ukázkovou aplikaci, která vytvoří tento kurz řady, můžete ji stáhnout z webu MSDN ukázky.</span><span class="sxs-lookup"><span data-stu-id="e76fe-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="e76fe-214">Tento soubor ke stažení obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="e76fe-214">This download contains the following:</span></span>

- <span data-ttu-id="e76fe-215">Ukázkovou aplikaci v *Northwind* složky.</span><span class="sxs-lookup"><span data-stu-id="e76fe-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="e76fe-216">Prostředky použít k vytvoření ukázkové aplikace v *Northwind prostředky* složku *Northwind* složky.</span><span class="sxs-lookup"><span data-stu-id="e76fe-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="e76fe-217">Soubor stáhněte z webu MSDN ukázky:</span><span class="sxs-lookup"><span data-stu-id="e76fe-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="e76fe-218">[Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="e76fe-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="e76fe-219">Stažení *.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="e76fe-219">The download is a *.zip* file.</span></span> <span data-ttu-id="e76fe-220">Zobrazíte dokončený projekt, který vytvoří tento kurz řady, vyhledejte a vyberte *C#*složku *.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="e76fe-220">To see the completed project that this tutorial series creates, find and select the *C#*folder in the *.zip* file.</span></span> <span data-ttu-id="e76fe-221">Uložit *C#* folderto složky použijete při práci s projekty sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e76fe-221">Save the *C#* folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="e76fe-222">Ve výchozím nastavení složka projektů Visual Studio 2013 je následující:</span><span class="sxs-lookup"><span data-stu-id="e76fe-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="e76fe-223">**C:\Users\*****&lt;uživatelské jméno&gt;*** \Documents\Visual Studio 2013\Projects**</span><span class="sxs-lookup"><span data-stu-id="e76fe-223">**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**</span></span>

<span data-ttu-id="e76fe-224">Přejmenujte ***C#*** složku pro ***Northwind***.</span><span class="sxs-lookup"><span data-stu-id="e76fe-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="e76fe-225">Pokud již máte složku s názvem *Northwind* ve složce projekty dočasně přejmenujte tento existující složku před přejmenováním *C#* složku pro *Northwind*.</span><span class="sxs-lookup"><span data-stu-id="e76fe-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="e76fe-226">Chcete-li spustit dokončený projekt, otevřete *Northwind* složku a dvojím kliknutím *WingtipToys.sln* souboru.</span><span class="sxs-lookup"><span data-stu-id="e76fe-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="e76fe-227">Otevřete projekt Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e76fe-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="e76fe-228">Pak klikněte pravým tlačítkem *Default.aspx* souborů v okně Průzkumníka řešení a v místní nabídce klikněte na tlačítko zobrazit v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e76fe-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="e76fe-229">Kurz podporu a komentáře</span><span class="sxs-lookup"><span data-stu-id="e76fe-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="e76fe-230">Pomocí informací v části otázky a A součástí [Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) vzorek (C#) pro jakékoli dotazy nebo připomínky.</span><span class="sxs-lookup"><span data-stu-id="e76fe-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="e76fe-231">Komentáře k tento kurz řady jsou úvodní a při aktualizaci tento kurz řady veškeré úsilí, budou provedeny brát v účtu opravy nebo návrhy na vylepšení, které jsou k dispozici v kurzu komentáře.</span><span class="sxs-lookup"><span data-stu-id="e76fe-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="e76fe-232">Když dojde k chybě během vývoje nebo na webu nefunguje správně, chybové zprávy, které může poskytnout komplexní různá vodítka na zdroj problému nebo nemusí vysvětlují, jak ji odstranit.</span><span class="sxs-lookup"><span data-stu-id="e76fe-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="e76fe-233">Abychom vám pomohli s některé běžné scénáře problém, můžete také použít [ASP.NET fóra](https://forums.asp.net/) nebo části otázky a A součástí [Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) ukázka.</span><span class="sxs-lookup"><span data-stu-id="e76fe-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="e76fe-234">Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte zkontrolovat výše uvedené umístění.</span><span class="sxs-lookup"><span data-stu-id="e76fe-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e76fe-235">Další</span><span class="sxs-lookup"><span data-stu-id="e76fe-235">Next</span></span>](create-the-project.md)
