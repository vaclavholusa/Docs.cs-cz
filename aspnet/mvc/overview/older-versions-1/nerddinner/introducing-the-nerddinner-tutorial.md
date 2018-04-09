---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Představujeme v kurzu NerdDinner | Microsoft Docs
author: shanselman
description: Nejlepší způsob, jak další nové architektury je sestavení něco s ním. Tento kurz vás provede postup vytvoření malé, ale dokončení aplikace pomocí ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="93fb8-104">Představujeme v kurzu NerdDinner</span><span class="sxs-lookup"><span data-stu-id="93fb8-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="93fb8-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="93fb8-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="93fb8-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="93fb8-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="93fb8-107">Nejlepší způsob, jak další nové architektury je sestavení něco s ním.</span><span class="sxs-lookup"><span data-stu-id="93fb8-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="93fb8-108">V tomto kurzu provede postup sestavení malá, ale dokončení aplikace pomocí ASP.NET MVC 1 a zavádí některé základní koncepty za ní.</span><span class="sxs-lookup"><span data-stu-id="93fb8-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="93fb8-109">Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="93fb8-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="93fb8-110">Kurz NerdDinner</span><span class="sxs-lookup"><span data-stu-id="93fb8-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="93fb8-111">Nejlepší způsob, jak další nové architektury je sestavení něco s ním.</span><span class="sxs-lookup"><span data-stu-id="93fb8-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="93fb8-112">V tomto kurzu provede postup sestavení malá, ale dokončení aplikace pomocí rozhraní ASP.NET MVC a zavádí některé základní koncepty za ní.</span><span class="sxs-lookup"><span data-stu-id="93fb8-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="93fb8-113">Aplikace, kterou přidáme sestavení se nazývá "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="93fb8-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="93fb8-114">NerdDinner poskytuje snadný způsob pro osoby k vyhledání a uspořádání večeří online:</span><span class="sxs-lookup"><span data-stu-id="93fb8-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="93fb8-115">NerdDinner umožňuje vytvářet, upravovat a odstraňovat večeří registrovaní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="93fb8-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="93fb8-116">Vynucuje sadu pravidel ověřování a obchodní konzistentní napříč aplikace:</span><span class="sxs-lookup"><span data-stu-id="93fb8-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="93fb8-117">Návštěvníky můžete použít mapování na základě AJAX pro vyhledání nadcházející večeří teď téměř je:</span><span class="sxs-lookup"><span data-stu-id="93fb8-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="93fb8-118">Kliknutím na večeři přejdou na stránce s podrobnostmi o kde můžete zjistit další informace:</span><span class="sxs-lookup"><span data-stu-id="93fb8-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="93fb8-119">Pokud jsou zajímat, kteří se účastní večeři můžou přihlásit nebo zaregistrovat na webu:</span><span class="sxs-lookup"><span data-stu-id="93fb8-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="93fb8-120">Můžete pak klikněte na odkaz na základě AJAX zasílání zpráv rysy k účasti na událost:</span><span class="sxs-lookup"><span data-stu-id="93fb8-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="93fb8-121">Implementace NerdDinner</span><span class="sxs-lookup"><span data-stu-id="93fb8-121">Implementing NerdDinner</span></span>

<span data-ttu-id="93fb8-122">Přidáme zahájíte NerdDinner aplikace pomocí souboru -&gt;příkaz Nový projekt v sadě Visual Studio k vytvoření nové projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="93fb8-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="93fb8-123">Potom postupně přidáme funkce a funkce.</span><span class="sxs-lookup"><span data-stu-id="93fb8-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="93fb8-124">Na této cestě jsme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="93fb8-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="93fb8-125">Postup vytvoření nového projektu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="93fb8-125">How to create a new ASP.NET MVC Project</span></span>](# "vytvořte nový projekt ASP.NET MVC")
2. [<span data-ttu-id="93fb8-126">Postup vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="93fb8-126">How to create a database</span></span>](# "vytvoření databáze")
3. [<span data-ttu-id="93fb8-127">Jak vytvořit model se obchodní pravidla ověření</span><span class="sxs-lookup"><span data-stu-id="93fb8-127">How to build a model with business rule validations</span></span>](# "sestavení Model se obchodní pravidla ověření")
4. [<span data-ttu-id="93fb8-128">Postup implementace výpis/podrobnosti uživatelského rozhraní pomocí řadiče a zobrazení</span><span class="sxs-lookup"><span data-stu-id="93fb8-128">How to use controllers and views to implement a listing/details UI</span></span>](# "pomocí řadiče a zobrazení implementovat výpis/podrobnosti uživatelského rozhraní")
5. <span data-ttu-id="93fb8-129">[Jak poskytnout CRUD (vytvořit, číst, aktualizovat, odstraňovat) data tvoří položka podporu](# "poskytují CRUD (vytvořit, číst, Update, Delete) Data formuláře Položka podporu")</span><span class="sxs-lookup"><span data-stu-id="93fb8-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="93fb8-130">Jak používat ViewData a implementaci třídy ViewModel</span><span class="sxs-lookup"><span data-stu-id="93fb8-130">How to use ViewData and implement ViewModel classes</span></span>](# "použít ViewData a implementovat ViewModel třídy")
7. [<span data-ttu-id="93fb8-131">Jak znovu pomocí uživatelského rozhraní pomocí hlavní stránky a částečné</span><span class="sxs-lookup"><span data-stu-id="93fb8-131">How to re-use UI using master pages and partials</span></span>](# "znovu pomocí uživatelského rozhraní pomocí hlavní stránky a částečné.")
8. [<span data-ttu-id="93fb8-132">Postupy pro implementaci stránkování efektivní dat</span><span class="sxs-lookup"><span data-stu-id="93fb8-132">How to implement efficient data paging</span></span>](# "implementovat efektivní Data stránkování")
9. [<span data-ttu-id="93fb8-133">Jak zabezpečit aplikace s použitím ověřování a autorizace</span><span class="sxs-lookup"><span data-stu-id="93fb8-133">How to secure applications using authentication and authorization</span></span>](# "zabezpečené aplikace pomocí ověřování a autorizace")
10. [<span data-ttu-id="93fb8-134">Jak používat k poskytování dynamické aktualizace AJAX</span><span class="sxs-lookup"><span data-stu-id="93fb8-134">How to use AJAX to deliver dynamic updates</span></span>](# "použít AJAX poskytovat dynamické aktualizace")
11. [<span data-ttu-id="93fb8-135">Jak používat AJAX implementovat scénáře mapování</span><span class="sxs-lookup"><span data-stu-id="93fb8-135">How to use AJAX to implement mapping scenarios</span></span>](# "použít AJAX implementovat scénáře mapování")
12. [<span data-ttu-id="93fb8-136">Postup povolení automatizované testování částí</span><span class="sxs-lookup"><span data-stu-id="93fb8-136">How to enable automated unit testing</span></span>](# "povolit automatizované testování částí")

<span data-ttu-id="93fb8-137">Můžete vytvořit svoji vlastní kopii NerdDinner od začátku pomocí každý krok jsme návod v této kapitole.</span><span class="sxs-lookup"><span data-stu-id="93fb8-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="93fb8-138">Alternativně můžete stáhnout dokončenou verzi zdrojový kód: [NerdDinner na Githubu](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="93fb8-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="93fb8-139">Můžete také volitelně také [volné PDF verzi v tomto kurzu Stáhnout](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Pokud si chcete přečíst kurz do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="93fb8-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="93fb8-140">Visual Studio 2008 nebo volné Visual Web Developer 2008 Express slouží k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="93fb8-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="93fb8-141">SQL Server nebo volné Server SQL Express můžete použít pro databázi.</span><span class="sxs-lookup"><span data-stu-id="93fb8-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="93fb8-142">Můžete nainstalovat rozhraní ASP.NET MVC, Visual Web Developer 2008 Express a SQL Server Express (všechny zdarma) pomocí V2 z [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="93fb8-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="93fb8-143">Nyní můžeme začít...</span><span class="sxs-lookup"><span data-stu-id="93fb8-143">Now let's get started....</span></span>

<span data-ttu-id="93fb8-144">Teď, když jsme jste popsané, co je NerdDinner, můžeme souhrnné naše rukávy a napsat kód.</span><span class="sxs-lookup"><span data-stu-id="93fb8-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="93fb8-145">Budete Začneme s použitím souboru -&gt;nový projekt v sadě Visual Studio k vytvoření aplikace NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="93fb8-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="93fb8-146">Next</span><span class="sxs-lookup"><span data-stu-id="93fb8-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
