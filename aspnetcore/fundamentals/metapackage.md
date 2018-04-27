---
title: Microsoft.AspNetCore.All metapackage pro ASP.NET Core 2.x a novější
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage zahrnuje všechny podporované balíčků ASP.NET Core a Entity Framework Core, spolu s jejich závislosti.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="f68cb-103">Microsoft.AspNetCore.All metapackage pro ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f68cb-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="f68cb-104">Tato funkce vyžaduje rozhraní .NET zaměřená na aplikace ASP.NET Core 2.x základní 2.x.</span><span class="sxs-lookup"><span data-stu-id="f68cb-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="f68cb-105">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="f68cb-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="f68cb-106">Všechny podporované balíčky tým ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f68cb-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="f68cb-107">Všechny podporované balíčků základní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f68cb-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="f68cb-108">Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f68cb-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="f68cb-109">Všechny funkce ASP.NET Core 2.x a Entity Framework Core 2.x jsou součástí `Microsoft.AspNetCore.All` balíčku.</span><span class="sxs-lookup"><span data-stu-id="f68cb-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="f68cb-110">Tento balíček použít výchozí šablony projektů cílení na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="f68cb-110">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="f68cb-111">Číslo verze `Microsoft.AspNetCore.All` metapackage představuje ASP.NET Core verze a verze Entity Framework Core (v souladu s verze .NET Core).</span><span class="sxs-lookup"><span data-stu-id="f68cb-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="f68cb-112">Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využívat výhod [.NET Core Runtime úložiště](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="f68cb-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="f68cb-113">Modul Runtime úložiště obsahuje všechny prostředky potřebné ke spuštění 2.x aplikace ASP.NET Core runtime.</span><span class="sxs-lookup"><span data-stu-id="f68cb-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="f68cb-114">Při použití `Microsoft.AspNetCore.All` metapackage, **žádné** prostředky z odkazované balíčky ASP.NET Core NuGet nasazených aplikací &mdash; úložiště .NET Core Runtime obsahuje tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="f68cb-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="f68cb-115">Prostředky v úložišti Runtime jsou předkompilovaných ke zlepšení času spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f68cb-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="f68cb-116">Proces oříznutí balíčku můžete odebrat balíčky, které nepoužíváte.</span><span class="sxs-lookup"><span data-stu-id="f68cb-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="f68cb-117">Oříznutý balíčky jsou vyloučeny ve výstupu publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="f68cb-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="f68cb-118">Následující *.csproj* souboru odkazy `Microsoft.AspNetCore.All` metapackage pro ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f68cb-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
