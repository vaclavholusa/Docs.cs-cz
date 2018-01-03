---
title: "Úvod do ASP.NET Core"
author: rick-anderson
description: "Nabízí úvodní informace o ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 3a18ed30819a3d395e9bfb5dba0547667a4425e8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="e0098-104">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0098-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="e0098-105">Autoři: [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) a [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="e0098-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="e0098-106">ASP.NET Core je platformově univerzální, vysoce výkonná architektura typu [open-source](https://github.com/aspnet/home), která slouží k vytváření moderních cloudových aplikací připojených k internetu.</span><span class="sxs-lookup"><span data-stu-id="e0098-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="e0098-107">S ASP.NET Core můžete:</span><span class="sxs-lookup"><span data-stu-id="e0098-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="e0098-108">Vytvářet webové aplikace a služby, aplikace [IoT](https://www.microsoft.com/internet-of-things/) a mobilní back-endy</span><span class="sxs-lookup"><span data-stu-id="e0098-108">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="e0098-109">Používat oblíbené vývojářské nástroje v systémech Windows, Mac OS a Linux</span><span class="sxs-lookup"><span data-stu-id="e0098-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e0098-110">Nasazovat v cloudu nebo místně</span><span class="sxs-lookup"><span data-stu-id="e0098-110">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="e0098-111">Používat ke spuštění [.NET Core nebo .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)</span><span class="sxs-lookup"><span data-stu-id="e0098-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="e0098-112">Proč používat ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="e0098-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="e0098-113">Miliony vývojářů už dříve k vytváření webových aplikací používali [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) (což bude platit i nadále).</span><span class="sxs-lookup"><span data-stu-id="e0098-113">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="e0098-114">ASP.NET Core je přepracovanou verzí ASP.NET 4.x se změněnou architekturou. Výsledkem je odlehčené a modulárnější prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0098-114">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

<span data-ttu-id="e0098-115">ASP.NET Core nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e0098-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="e0098-116">Jednotné prostředí pro vytváření webového uživatelského rozhraní a webových rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e0098-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="e0098-117">Integrace [moderní architektury klienta](xref:client-side/index) a vývojových pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="e0098-117">Integration of [modern, client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="e0098-118">[Konfigurační systém](xref:fundamentals/configuration/index) založený na prostředí, který je připravený pro cloud</span><span class="sxs-lookup"><span data-stu-id="e0098-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="e0098-119">Integrovaná [injektáž závislostí](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="e0098-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="e0098-120">Odlehčený, [vysoce výkonný](https://github.com/aspnet/benchmarks), modulární kanál požadavků HTTP</span><span class="sxs-lookup"><span data-stu-id="e0098-120">A lightweight, [high-performance](https://github.com/aspnet/benchmarks), and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="e0098-121">Možnost hostování ve službě [IIS](xref:publishing/iis), [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), [Docker](xref:publishing/docker) nebo samoobslužné hostování ve vlastním procesu</span><span class="sxs-lookup"><span data-stu-id="e0098-121">Ability to host on [IIS](xref:publishing/iis), [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), [Docker](xref:publishing/docker), or self-host in your own process.</span></span>
* <span data-ttu-id="e0098-122">Souběžná správa různých verzí aplikace, když cílovou platformou je [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)</span><span class="sxs-lookup"><span data-stu-id="e0098-122">Side-by-side app versioning when targeting [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
* <span data-ttu-id="e0098-123">Nástroje, které usnadňují vývoj moderních webů</span><span class="sxs-lookup"><span data-stu-id="e0098-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="e0098-124">Možnost kompilace a spuštění v systémech Windows, Mac OS a Linux</span><span class="sxs-lookup"><span data-stu-id="e0098-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e0098-125">Architektura zaměřená na open-source a [komunitu](https://live.asp.net/)</span><span class="sxs-lookup"><span data-stu-id="e0098-125">Open-source and [community-focused](https://live.asp.net/).</span></span>

<span data-ttu-id="e0098-126">Architektura ASP.NET Core se celá dodává v balíčcích [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="e0098-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="e0098-127">To umožňuje optimalizovat aplikaci tak, aby zahrnovala jen potřebné balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="e0098-127">This allows you to optimize your app to include only the necessary NuGet packages.</span></span> <span data-ttu-id="e0098-128">Ve skutečnosti vyžadují aplikace ASP.NET Core 2.x, jejichž cílem je .NET Core, jen [jeden balíček NuGet](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e0098-128">In fact, ASP.NET Core 2.x apps targeting .NET Core only require a [single NuGet package](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="e0098-129">K výhodám menší plochy aplikace patří důkladnější zabezpečení, menší údržba a větší výkon.</span><span class="sxs-lookup"><span data-stu-id="e0098-129">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="e0098-130">Vytváření webových rozhraní API a webového uživatelského rozhraní v ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e0098-130">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="e0098-131">Architektura ASP.NET Core MVC nabízí funkce umožňující vytvářet [webová rozhraní API](xref:tutorials/index#building-web-apis) a [webové aplikace](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="e0098-131">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="e0098-132">[Vzor Model-View-Controller (MVC)](xref:mvc/overview) přispívá k tomu, aby vaše webová rozhraní API a webové aplikace byly [testovatelné](testing/index.md).</span><span class="sxs-lookup"><span data-stu-id="e0098-132">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="e0098-133">[Stránky Razor](xref:mvc/razor-pages/index) (nově ve verzi ASP.NET Core 2.0) představují stránkovaný programový model, se kterým je vytváření webového uživatelského rozhraní jednodušší a produktivnější.</span><span class="sxs-lookup"><span data-stu-id="e0098-133">[Razor Pages](xref:mvc/razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="e0098-134">[Kód Razor](xref:mvc/views/razor) poskytuje produktivní syntaxi jak pro [stránky Razor](xref:mvc/razor-pages/index), tak pro [zobrazení MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="e0098-134">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:mvc/razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="e0098-135">[Pomocné rutiny značky](xref:mvc/views/tag-helpers/intro) umožňují, aby se kód na straně serveru v souborech Razor podílel na vytváření a vykreslování prvků HTML.</span><span class="sxs-lookup"><span data-stu-id="e0098-135">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="e0098-136">Díky integrované podpoře [různých datových formátů a vyjednávání obsahu](mvc/models/formatting.md) mohou vaše webová rozhraní API používat různí klienti, včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0098-136">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="e0098-137">[Vazby modelu](xref:mvc/models/model-binding) automaticky mapují data požadavků HTTP na parametry metod akce.</span><span class="sxs-lookup"><span data-stu-id="e0098-137">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="e0098-138">[Ověření modelu](xref:mvc/models/validation) automaticky ověří stranu klienta i stranu serveru.</span><span class="sxs-lookup"><span data-stu-id="e0098-138">[Model validation](xref:mvc/models/validation) automatically performs client- and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="e0098-139">Vývoj klientské strany</span><span class="sxs-lookup"><span data-stu-id="e0098-139">Client-side development</span></span>

<span data-ttu-id="e0098-140">Architektura ASP.NET Core se hladce integruje do oblíbených klientských rozhraní a knihoven, včetně [Angular](xref:spa/angular), [React](xref:spa/react) a [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="e0098-140">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="e0098-141">Podrobnější informace najdete v části [Vývoj klientské strany](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="e0098-141">See [Client-side development](xref:client-side/index) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0098-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0098-142">Next steps</span></span>

<span data-ttu-id="e0098-143">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="e0098-143">For more information, see the following resources:</span></span>

* [<span data-ttu-id="e0098-144">Kurzy k ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0098-144">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="e0098-145">Základy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0098-145">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="e0098-146">[Týdenní přehled novinek v komunitě ASP.NET](https://live.asp.net/) se zabývá pokrokem v týmových projektech a týmovými plány.</span><span class="sxs-lookup"><span data-stu-id="e0098-146">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="e0098-147">Nabízí nové blogové příspěvky a software třetích stran.</span><span class="sxs-lookup"><span data-stu-id="e0098-147">It features new blogs and third-party software.</span></span>
