---
title: Zvolte mezi ASP.NET a ASP.NET Core
author: rick-anderson
description: "Zjistěte, jak si vybrat mezi ASP.NET a ASP.NET Core."
keywords: "ASP.NET Core, základy, – přehled"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 875064bd3437acc4e2a53220e19e86431d8c159b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="cee17-104">Zvolte mezi ASP.NET a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cee17-104">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="cee17-105">Bez ohledu na to, webové aplikaci, kterou vytváříte, technologie ASP.NET má řešení pro vás: z webových aplikací enterprise cílení na Windows Server, na malých mikroslužeb cílení na Linux kontejnery a všechno, co v rozmezí.</span><span class="sxs-lookup"><span data-stu-id="cee17-105">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="cee17-106">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-106">ASP.NET Core</span></span>

<span data-ttu-id="cee17-107">ASP.NET Core je otevřeným zdrojem, a platformy architektura pro vytváření moderní cloudové webových aplikací na Windows, systému macOS nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="cee17-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="cee17-108">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-108">ASP.NET</span></span>

<span data-ttu-id="cee17-109">ASP.NET je vyspělá rozhraní, které poskytuje všechny potřebné k vytvoření podnikové třídy, na serveru webové aplikace v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="cee17-109">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="cee17-110">Která je pro mě nejlepší?</span><span class="sxs-lookup"><span data-stu-id="cee17-110">Which one is right for me?</span></span>

| <span data-ttu-id="cee17-111">Jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-111">ASP.NET Core</span></span> | <span data-ttu-id="cee17-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="cee17-113">Sestavení pro Windows, systému macOS nebo Linux</span><span class="sxs-lookup"><span data-stu-id="cee17-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="cee17-114">Sestavení pro Windows</span><span class="sxs-lookup"><span data-stu-id="cee17-114">Build for Windows</span></span>|
|<span data-ttu-id="cee17-115">[Stránky Razor](xref:mvc/razor-pages/index) se o doporučený postup pro vytvoření webového uživatelského rozhraní s prostředím ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="cee17-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="cee17-116">Viz také [MVC](xref:mvc/overview) a [webové rozhraní API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="cee17-116">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="cee17-117">Použití [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [webové rozhraní API](https://docs.microsoft.com/aspnet/web-api/), nebo [webové stránky](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="cee17-117">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="cee17-118">Několik verzí systému na počítač</span><span class="sxs-lookup"><span data-stu-id="cee17-118">Multiple versions per machine</span></span>|<span data-ttu-id="cee17-119">Jedna verze na počítač</span><span class="sxs-lookup"><span data-stu-id="cee17-119">One version per machine</span></span>|
|<span data-ttu-id="cee17-120">Vývoj pomocí sady Visual Studio, [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/), nebo [Visual Studio Code](https://code.visualstudio.com/) pomocí jazyka C# nebo F #</span><span class="sxs-lookup"><span data-stu-id="cee17-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="cee17-121">Vývoj pomocí sady Visual Studio pomocí jazyka C#, VB a F #</span><span class="sxs-lookup"><span data-stu-id="cee17-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="cee17-122">Vyšší výkon než technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="cee17-123">Dobrý výkon</span><span class="sxs-lookup"><span data-stu-id="cee17-123">Good performance</span></span>|
|[<span data-ttu-id="cee17-124">Vyberte modul runtime rozhraní .NET Framework nebo .NET Core</span><span class="sxs-lookup"><span data-stu-id="cee17-124">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="cee17-125">Použít modul runtime rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="cee17-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="cee17-126">ASP.NET základní scénáře</span><span class="sxs-lookup"><span data-stu-id="cee17-126">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="cee17-127">[Stránky Razor](xref:mvc/razor-pages/index) se o doporučený postup pro vytvoření webového uživatelského rozhraní s prostředím ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="cee17-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="cee17-128">Weby</span><span class="sxs-lookup"><span data-stu-id="cee17-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="cee17-129">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cee17-129">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="cee17-130">Scénáře ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="cee17-131">Weby</span><span class="sxs-lookup"><span data-stu-id="cee17-131">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="cee17-132">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cee17-132">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="cee17-133">V reálném čase</span><span class="sxs-lookup"><span data-stu-id="cee17-133">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="cee17-134">Prostředky</span><span class="sxs-lookup"><span data-stu-id="cee17-134">Resources</span></span>

* [<span data-ttu-id="cee17-135">Úvod do technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cee17-135">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="cee17-136">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cee17-136">Introduction to ASP.NET Core</span></span>](xref:index)
