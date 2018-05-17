---
title: Zvolte mezi ASP.NET a ASP.NET Core
author: rick-anderson
description: Zjistěte, jak si vybrat mezi ASP.NET a ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="43485-103">Zvolte mezi ASP.NET a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43485-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="43485-104">Bez ohledu na to, kterou vytváříte, webové aplikace technologie ASP.NET má řešení pro vás: z podnikové webové aplikace cílený na Windows Server, na malých mikroslužeb cílení na Linux kontejnery a všechno, co v rozmezí.</span><span class="sxs-lookup"><span data-stu-id="43485-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="43485-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43485-105">ASP.NET Core</span></span>

<span data-ttu-id="43485-106">ASP.NET Core je otevřeným zdrojem, a platformy architektura pro vytváření moderní cloudové webových aplikací v systému Windows, systému macOS nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="43485-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="43485-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="43485-107">ASP.NET</span></span>

<span data-ttu-id="43485-108">ASP.NET je vyspělá rozhraní, které poskytuje všechny potřebné k vytvoření podnikové úrovni, na serveru webové aplikace v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="43485-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="43485-109">Výběr Framework</span><span class="sxs-lookup"><span data-stu-id="43485-109">Framework selection</span></span>

<span data-ttu-id="43485-110">Zkontrolujte následující tabulce, jak zjistit, které framework nejlépe vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="43485-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="43485-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43485-111">ASP.NET Core</span></span> | <span data-ttu-id="43485-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="43485-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="43485-113">Sestavení pro Windows, systému macOS nebo Linux</span><span class="sxs-lookup"><span data-stu-id="43485-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="43485-114">Sestavení pro Windows</span><span class="sxs-lookup"><span data-stu-id="43485-114">Build for Windows</span></span>|
|<span data-ttu-id="43485-115">[Stránky Razor](xref:mvc/razor-pages/index) se o doporučený postup pro vytvoření webového uživatelského rozhraní od ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="43485-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="43485-116">Viz také [MVC](xref:mvc/overview), [webové rozhraní API](xref:tutorials/first-web-api), a [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="43485-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="43485-117">Použití [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [webové rozhraní API](/aspnet/web-api/), [Webhooky](/aspnet/webhooks/), nebo [webové stránky](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="43485-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="43485-118">Několik verzí systému na počítač</span><span class="sxs-lookup"><span data-stu-id="43485-118">Multiple versions per machine</span></span>|<span data-ttu-id="43485-119">Jedna verze na počítač</span><span class="sxs-lookup"><span data-stu-id="43485-119">One version per machine</span></span>|
|<span data-ttu-id="43485-120">Vývoj pomocí sady Visual Studio, [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/), nebo [Visual Studio Code](https://code.visualstudio.com/) pomocí jazyka C# nebo F #</span><span class="sxs-lookup"><span data-stu-id="43485-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="43485-121">Vývoj pomocí sady Visual Studio pomocí jazyka C#, VB a F #</span><span class="sxs-lookup"><span data-stu-id="43485-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="43485-122">Vyšší výkon než technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="43485-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="43485-123">Dobrý výkon</span><span class="sxs-lookup"><span data-stu-id="43485-123">Good performance</span></span>|
|[<span data-ttu-id="43485-124">Vyberte modul runtime rozhraní .NET Framework nebo .NET Core</span><span class="sxs-lookup"><span data-stu-id="43485-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="43485-125">Použít modul runtime rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="43485-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="43485-126">ASP.NET základní scénáře</span><span class="sxs-lookup"><span data-stu-id="43485-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="43485-127">[Stránky Razor](xref:mvc/razor-pages/index) se o doporučený postup pro vytvoření webového uživatelského rozhraní od ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="43485-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="43485-128">Weby</span><span class="sxs-lookup"><span data-stu-id="43485-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="43485-129">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="43485-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="43485-130">V reálném čase</span><span class="sxs-lookup"><span data-stu-id="43485-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="43485-131">Scénáře ASP.NET</span><span class="sxs-lookup"><span data-stu-id="43485-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="43485-132">Weby</span><span class="sxs-lookup"><span data-stu-id="43485-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="43485-133">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="43485-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="43485-134">V reálném čase</span><span class="sxs-lookup"><span data-stu-id="43485-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="43485-135">Prostředky</span><span class="sxs-lookup"><span data-stu-id="43485-135">Resources</span></span>

* [<span data-ttu-id="43485-136">Úvod do technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="43485-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="43485-137">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43485-137">Introduction to ASP.NET Core</span></span>](xref:index)
