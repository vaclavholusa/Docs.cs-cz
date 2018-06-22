---
title: Zvolte mezi ASP.NET a ASP.NET Core
author: rick-anderson
description: Zjistěte, jak si vybrat mezi ASP.NET a ASP.NET Core.
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291642"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="ad724-103">Zvolte mezi ASP.NET a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad724-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="ad724-104">Bez ohledu na to, kterou vytváříte, webové aplikace technologie ASP.NET má řešení pro vás: z podnikové webové aplikace cílený na Windows Server, na malých mikroslužeb cílení na Linux kontejnery a všechno, co v rozmezí.</span><span class="sxs-lookup"><span data-stu-id="ad724-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="ad724-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad724-105">ASP.NET Core</span></span>

<span data-ttu-id="ad724-106">ASP.NET Core je otevřeným zdrojem, a platformy architektura pro vytváření moderní cloudové webových aplikací v systému Windows, systému macOS nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="ad724-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="ad724-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad724-107">ASP.NET</span></span>

<span data-ttu-id="ad724-108">ASP.NET je vyspělá rozhraní, které poskytuje všechny potřebné k vytvoření podnikové úrovni, na serveru webové aplikace v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ad724-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="ad724-109">Výběr Framework</span><span class="sxs-lookup"><span data-stu-id="ad724-109">Framework selection</span></span>

<span data-ttu-id="ad724-110">Zkontrolujte následující tabulce, jak zjistit, které framework nejlépe vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="ad724-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="ad724-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad724-111">ASP.NET Core</span></span> | <span data-ttu-id="ad724-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad724-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="ad724-113">Sestavení pro Windows, systému macOS nebo Linux</span><span class="sxs-lookup"><span data-stu-id="ad724-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="ad724-114">Sestavení pro Windows</span><span class="sxs-lookup"><span data-stu-id="ad724-114">Build for Windows</span></span>|
|<span data-ttu-id="ad724-115">[Stránky Razor](xref:razor-pages/index) se o doporučený postup pro vytvoření webového uživatelského rozhraní od ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="ad724-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="ad724-116">Viz také [MVC](xref:mvc/overview), [webové rozhraní API](xref:tutorials/first-web-api), a [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ad724-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="ad724-117">Použití [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [webové rozhraní API](/aspnet/web-api/), [Webhooky](/aspnet/webhooks/), nebo [webové stránky](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="ad724-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="ad724-118">Několik verzí systému na počítač</span><span class="sxs-lookup"><span data-stu-id="ad724-118">Multiple versions per machine</span></span>|<span data-ttu-id="ad724-119">Jedna verze na počítač</span><span class="sxs-lookup"><span data-stu-id="ad724-119">One version per machine</span></span>|
|<span data-ttu-id="ad724-120">Vývoj pomocí sady Visual Studio, [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/), nebo [Visual Studio Code](https://code.visualstudio.com/) pomocí jazyka C# nebo F #</span><span class="sxs-lookup"><span data-stu-id="ad724-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="ad724-121">Vývoj pomocí sady Visual Studio pomocí jazyka C#, VB a F #</span><span class="sxs-lookup"><span data-stu-id="ad724-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="ad724-122">Vyšší výkon než technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad724-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="ad724-123">Dobrý výkon</span><span class="sxs-lookup"><span data-stu-id="ad724-123">Good performance</span></span>|
|[<span data-ttu-id="ad724-124">Vyberte modul runtime rozhraní .NET Framework nebo .NET Core</span><span class="sxs-lookup"><span data-stu-id="ad724-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="ad724-125">Použít modul runtime rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="ad724-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="ad724-126">ASP.NET základní scénáře</span><span class="sxs-lookup"><span data-stu-id="ad724-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="ad724-127">[Stránky Razor](xref:razor-pages/index) se o doporučený postup pro vytvoření webového uživatelského rozhraní od ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="ad724-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="ad724-128">Weby</span><span class="sxs-lookup"><span data-stu-id="ad724-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="ad724-129">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ad724-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="ad724-130">V reálném čase</span><span class="sxs-lookup"><span data-stu-id="ad724-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="ad724-131">Scénáře ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad724-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="ad724-132">Weby</span><span class="sxs-lookup"><span data-stu-id="ad724-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="ad724-133">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ad724-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="ad724-134">V reálném čase</span><span class="sxs-lookup"><span data-stu-id="ad724-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="ad724-135">Prostředky</span><span class="sxs-lookup"><span data-stu-id="ad724-135">Resources</span></span>

* [<span data-ttu-id="ad724-136">Úvod do technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad724-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="ad724-137">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad724-137">Introduction to ASP.NET Core</span></span>](xref:index)
