---
title: "Úvod do ASP.NET Core"
author: rick-anderson
description: "Nabízí úvodní informace o ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="42a07-104">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42a07-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="42a07-105">Autoři: [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) a [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="42a07-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="42a07-106">ASP.NET Core je platformově univerzální, vysoce výkonná architektura typu [open-source](https://github.com/aspnet/home), která slouží k vytváření moderních cloudových aplikací připojených k internetu.</span><span class="sxs-lookup"><span data-stu-id="42a07-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="42a07-107">S ASP.NET Core můžete:</span><span class="sxs-lookup"><span data-stu-id="42a07-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="42a07-108">Vytvářet webové aplikace a služby, aplikace [IoT](https://www.microsoft.com/en-us/internet-of-things/) a mobilní back-endy.</span><span class="sxs-lookup"><span data-stu-id="42a07-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="42a07-109">Používat oblíbené vývojářské nástroje v systémech Windows, Mac OS a Linux.</span><span class="sxs-lookup"><span data-stu-id="42a07-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="42a07-110">Nasazovat v cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="42a07-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="42a07-111">Používat ke spuštění [.NET Core nebo .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="42a07-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="42a07-112">Proč používat ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="42a07-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="42a07-113">Miliony vývojářů si zvykli používat (a dále používají) ASP.NET k vytváření webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="42a07-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="42a07-114">ASP.NET Core je přepracovanou verzí ASP.NET se změněnou architekturou. Výsledkem je odlehčené modulární prostředí.</span><span class="sxs-lookup"><span data-stu-id="42a07-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="42a07-115">ASP.NET Core nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="42a07-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="42a07-116">Jednotné prostředí pro vytváření webového uživatelského rozhraní a webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="42a07-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="42a07-117">Integrace [moderní architektury klienta](xref:client-side/index) a vývojových pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="42a07-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="42a07-118">[Konfigurační systém](xref:fundamentals/configuration/index) založený na prostředí, který je připravený pro cloud.</span><span class="sxs-lookup"><span data-stu-id="42a07-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="42a07-119">Integrovaná [injektáž závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="42a07-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="42a07-120">Odlehčený, vysoce výkonný, modulární kanál požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="42a07-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="42a07-121">Možnost hostování ve službě IIS nebo samoobslužné hostování ve vlastním procesu.</span><span class="sxs-lookup"><span data-stu-id="42a07-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="42a07-122">Umožňuje spuštění v prostředí [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), které podporuje opravdovou souběžnou správu různých verzí aplikace.</span><span class="sxs-lookup"><span data-stu-id="42a07-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="42a07-123">Nabízí nástroje, které usnadňují vývoj moderních webů.</span><span class="sxs-lookup"><span data-stu-id="42a07-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="42a07-124">Možnost kompilace a spuštění v systémech Windows, Mac OS a Linux.</span><span class="sxs-lookup"><span data-stu-id="42a07-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="42a07-125">Architektura zaměřená na open-source a komunitu.</span><span class="sxs-lookup"><span data-stu-id="42a07-125">Open-source and community-focused.</span></span>

<span data-ttu-id="42a07-126">Architektura ASP.NET Core se celá dodává v balíčcích [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="42a07-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="42a07-127">To umožňuje optimalizovat aplikaci a zahrnout jen ty balíčky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="42a07-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="42a07-128">K výhodám menší plochy aplikace patří důkladnější zabezpečení, menší údržba a větší výkon.</span><span class="sxs-lookup"><span data-stu-id="42a07-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="42a07-129">Vytváření webových rozhraní API a webového uživatelského rozhraní v ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="42a07-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="42a07-130">Architektura ASP.NET Core MVC nabízí funkce, které umožňují vytvářet [webová rozhraní API](xref:tutorials/index#building-web-apis) a [webové aplikace](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="42a07-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="42a07-131">[Vzor Model-View-Controller (MVC)](xref:mvc/overview) přispívá k tomu, aby vaše webová rozhraní API a webové aplikace byly [testovatelné](testing/index.md).</span><span class="sxs-lookup"><span data-stu-id="42a07-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="42a07-132">[Stránky Razor](xref:mvc/razor-pages/index) (nově ve verzi 2.0) představují stránkovaný programový model, se kterým je vytváření webového uživatelského rozhraní jednodušší a produktivnější.</span><span class="sxs-lookup"><span data-stu-id="42a07-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="42a07-133">[Syntaxe Razor](xref:mvc/views/razor) poskytuje jazyk, který je produktivnější jak pro [stránky Razor](xref:mvc/razor-pages/index), tak pro [zobrazení MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="42a07-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="42a07-134">[Pomocné rutiny značky](xref:mvc/views/tag-helpers/intro) umožňují, aby se kód na straně serveru v souborech Razor podílel na vytváření a vykreslování prvků HTML.</span><span class="sxs-lookup"><span data-stu-id="42a07-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="42a07-135">Díky integrované podpoře [různých datových formátů a vyjednávání obsahu](mvc/models/formatting.md) mohou vaše webová rozhraní API používat různí klienti, včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="42a07-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="42a07-136">[Vazby modelu](xref:mvc/models/model-binding) automaticky mapují data požadavků HTTP na parametry metod akce.</span><span class="sxs-lookup"><span data-stu-id="42a07-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="42a07-137">[Ověření modelu](xref:mvc/models/validation) automaticky ověří stranu klienta i stranu serveru.</span><span class="sxs-lookup"><span data-stu-id="42a07-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="42a07-138">Vývoj klientské strany</span><span class="sxs-lookup"><span data-stu-id="42a07-138">Client-side development</span></span>

<span data-ttu-id="42a07-139">Architektura ASP.NET Core je navržená tak, aby bylo možné ji bez problémů integrovat do různých klientských rozhraní, včetně [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) a [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="42a07-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="42a07-140">Podrobnější informace najdete v části [Vývoj klientské strany](client-side/index.md).</span><span class="sxs-lookup"><span data-stu-id="42a07-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42a07-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42a07-141">Next steps</span></span>

<span data-ttu-id="42a07-142">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="42a07-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="42a07-143">Kurzy k ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42a07-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="42a07-144">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42a07-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="42a07-145">[Týdenní přehled novinek v komunitě ASP.NET](https://live.asp.net/) se zabývá pokroky a plány týmu a nabízí nové blogové příspěvky a software třetích stran.</span><span class="sxs-lookup"><span data-stu-id="42a07-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
