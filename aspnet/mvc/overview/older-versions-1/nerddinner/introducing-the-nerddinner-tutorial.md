---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Úvod do kurzu NerdDinner | Dokumentace Microsoftu
author: shanselman
description: Nejlepší způsob, jak další nové rozhraní je s ním něco sestavení. Tento kurz vás provede postupem vytvoření malé, ale dokončení aplikace pomocí konfigurace ASP.NE...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805107"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="189c4-104">Úvod do kurzu NerdDinner</span><span class="sxs-lookup"><span data-stu-id="189c4-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="189c4-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="189c4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="189c4-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="189c4-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="189c4-107">Nejlepší způsob, jak další nové rozhraní je s ním něco sestavení.</span><span class="sxs-lookup"><span data-stu-id="189c4-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="189c4-108">Tento kurz vás provede postupy vytvoření malé, ale dokončení aplikace pomocí ASP.NET MVC 1 a představuje některé základními koncepcemi jeho fungování.</span><span class="sxs-lookup"><span data-stu-id="189c4-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="189c4-109">Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="189c4-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="189c4-110">Do kurzu NerdDinner</span><span class="sxs-lookup"><span data-stu-id="189c4-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="189c4-111">Nejlepší způsob, jak další nové rozhraní je s ním něco sestavení.</span><span class="sxs-lookup"><span data-stu-id="189c4-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="189c4-112">Tento kurz vás provede postupy vytvoření malé, ale dokončení aplikace pomocí ASP.NET MVC a zavádí některé základními koncepcemi jeho fungování.</span><span class="sxs-lookup"><span data-stu-id="189c4-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="189c4-113">Aplikace, kterou budeme vytvářet se nazývá "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="189c4-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="189c4-114">NerdDinner poskytuje snadný způsob uživatelům najít a uspořádat večeří online:</span><span class="sxs-lookup"><span data-stu-id="189c4-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="189c4-115">NerdDinner umožňuje vytvářet, upravovat a odstraňovat večeří registrovaných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="189c4-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="189c4-116">Vynucuje konzistentní sadu pravidel ověřování a obchodní napříč aplikací:</span><span class="sxs-lookup"><span data-stu-id="189c4-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="189c4-117">Návštěvníci můžete hledat nadcházející večeří se nachází v jejich mapování na základě AJAX:</span><span class="sxs-lookup"><span data-stu-id="189c4-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="189c4-118">Kliknutím webu dinner přejdou na stránku podrobností kde lze získat informace Další informace:</span><span class="sxs-lookup"><span data-stu-id="189c4-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="189c4-119">Pokud se zúčastnili dinner můžou přihlásit nebo zaregistrovat na webu:</span><span class="sxs-lookup"><span data-stu-id="189c4-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="189c4-120">Můžete pak kliknout na odkaz na reakce na základě jazyka AJAX k Zúčastněte se akce:</span><span class="sxs-lookup"><span data-stu-id="189c4-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="189c4-121">Implementace NerdDinner</span><span class="sxs-lookup"><span data-stu-id="189c4-121">Implementing NerdDinner</span></span>

<span data-ttu-id="189c4-122">Budeme zahájíte naší aplikace NerdDinner pomocí souboru -&gt;příkaz Nový projekt v sadě Visual Studio k vytvoření zcela nového projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="189c4-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="189c4-123">Pak postupně přidáme funkce a prvky.</span><span class="sxs-lookup"><span data-stu-id="189c4-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="189c4-124">Na cestě probereme:</span><span class="sxs-lookup"><span data-stu-id="189c4-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="189c4-125">Jak vytvořit nový projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="189c4-125">How to create a new ASP.NET MVC Project</span></span>](# "vytvořte nový projekt ASP.NET MVC")
2. [<span data-ttu-id="189c4-126">Jak vytvořit databázi</span><span class="sxs-lookup"><span data-stu-id="189c4-126">How to create a database</span></span>](# "vytvoření databáze")
3. [<span data-ttu-id="189c4-127">Jak vytvořit model s ověřením obchodních pravidel</span><span class="sxs-lookup"><span data-stu-id="189c4-127">How to build a model with business rule validations</span></span>](# "sestavení modelu s ověřením obchodních pravidel")
4. [<span data-ttu-id="189c4-128">Použití kontrolerů a zobrazení seznamu a podrobností uživatelského rozhraní implementovat</span><span class="sxs-lookup"><span data-stu-id="189c4-128">How to use controllers and views to implement a listing/details UI</span></span>](# "použití Kontrolerů a zobrazení k implementaci uživatelského rozhraní seznamu a podrobností")
5. <span data-ttu-id="189c4-129">[Postup zajištění akcí CRUD (vytváření, čtení, aktualizace nebo odstranění) podporujících zápis dat do formuláře](# "poskytují CRUD (vytváření, čtení, Update, Delete) Data formuláře položky podporu")</span><span class="sxs-lookup"><span data-stu-id="189c4-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="189c4-130">Použití slovníku ViewData a implementace tříd ViewModel</span><span class="sxs-lookup"><span data-stu-id="189c4-130">How to use ViewData and implement ViewModel classes</span></span>](# "použití slovníku ViewData a implementace tříd ViewModel")
7. [<span data-ttu-id="189c4-131">Jak znovu pomocí uživatelského rozhraní pomocí stránek předloh a částečných zobrazení</span><span class="sxs-lookup"><span data-stu-id="189c4-131">How to re-use UI using master pages and partials</span></span>](# "stránek opakované použití uživatelského rozhraní předloh a částečných zobrazení")
8. [<span data-ttu-id="189c4-132">Implementace efektivního stránkování dat</span><span class="sxs-lookup"><span data-stu-id="189c4-132">How to implement efficient data paging</span></span>](# "implementovat efektivní Data stránkování")
9. [<span data-ttu-id="189c4-133">Zabezpečení aplikací ověřováním a autorizací</span><span class="sxs-lookup"><span data-stu-id="189c4-133">How to secure applications using authentication and authorization</span></span>](# "zabezpečené aplikace pomocí ověřování a autorizace")
10. [<span data-ttu-id="189c4-134">Použití jazyka AJAX k dynamickým aktualizacím</span><span class="sxs-lookup"><span data-stu-id="189c4-134">How to use AJAX to deliver dynamic updates</span></span>](# "použití jazyka AJAX k doručení dynamických aktualizací")
11. [<span data-ttu-id="189c4-135">Použití jazyka AJAX k implementaci scénářů mapování</span><span class="sxs-lookup"><span data-stu-id="189c4-135">How to use AJAX to implement mapping scenarios</span></span>](# "použití jazyka AJAX k implementaci scénářů mapování")
12. [<span data-ttu-id="189c4-136">Jak povolit automatické testování částí</span><span class="sxs-lookup"><span data-stu-id="189c4-136">How to enable automated unit testing</span></span>](# "povolte automatizované testování částí")

<span data-ttu-id="189c4-137">Můžete vytvořit svoji vlastní kopii NerdDinner úplně od začátku po dokončení každého kroku jsme názorný postup v této kapitole.</span><span class="sxs-lookup"><span data-stu-id="189c4-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="189c4-138">Alternativně můžete stáhnout úplnou verzi zdrojového kódu: [NerdDinner na Githubu](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="189c4-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="189c4-139">Můžete také v případě potřeby také [stáhnout zdarma PDF verzi tohoto kurzu](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Pokud si chcete přečíst kurz v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="189c4-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="189c4-140">Visual Studio 2008 nebo na bezplatné Visual Web Developer 2008 Express můžete použít k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="189c4-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="189c4-141">Pro databázi můžete použít SQL Server nebo bezplatný systém SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="189c4-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="189c4-142">ASP.NET MVC, Visual Web Developer 2008 Express a SQL Server Express (vše zdarma) pomocí verzí V2 můžete nainstalovat [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="189c4-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="189c4-143">Nyní můžeme začít...</span><span class="sxs-lookup"><span data-stu-id="189c4-143">Now let's get started....</span></span>

<span data-ttu-id="189c4-144">Teď, když jsme si popsali, co je NerdDinner, Pojďme vyhrňte rukávy naše a napsání kódu.</span><span class="sxs-lookup"><span data-stu-id="189c4-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="189c4-145">Začneme budete pomocí souboru -&gt;nový projekt v sadě Visual Studio k vytvoření aplikace NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="189c4-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="189c4-146">Next</span><span class="sxs-lookup"><span data-stu-id="189c4-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
