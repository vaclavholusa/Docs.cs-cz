---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimalizace výkonu sestavení pro řešení
author: AngelosP
description: Optimalizace výkonu sestavení pro řešení
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312138"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="a7f4d-103">Optimalizace výkonu sestavení pro řešení</span><span class="sxs-lookup"><span data-stu-id="a7f4d-103">Optimize build performance for solution</span></span>

<span data-ttu-id="a7f4d-104">Visual Studio 2017 15.8 nebo vyšší zahrnují položky nabídky: **sestavení** > **kompilace ASP.NET** > **optimalizovat výkon sestavení pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="a7f4d-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![snímek obrazovky s novou položku nabídky](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="a7f4d-106">ASP.NET zkompiluje za běhu, což znamená, že technologie ASP.NET provede s ní kopie kompilátor jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a7f4d-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="a7f4d-107">Ale na počítači pro vývojáře při kopírování kompilátor neodpovídá kopie sady Visual Studio, sestavení je dopad na výkon v řádu sekund 1-3 na přírůstkové sestavení.</span><span class="sxs-lookup"><span data-stu-id="a7f4d-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="a7f4d-108">Tato funkce aktualizuje váš projekt kopie kompilátor tak, aby odpovídala sadě Visual Studio, což obvykle urychlí přírůstková sestavení.</span><span class="sxs-lookup"><span data-stu-id="a7f4d-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="a7f4d-109">**To se vztahuje na ASP.NET Framework 4.7.1 nebo později pouze pro projekty, neplatí pro ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="a7f4d-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
