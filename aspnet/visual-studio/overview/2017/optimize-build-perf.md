---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimalizace výkonu sestavení pro řešení
author: tfitzmac
description: Optimalizace výkonu sestavení pro řešení
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910022"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="6e1a2-103">Optimalizace výkonu sestavení pro řešení</span><span class="sxs-lookup"><span data-stu-id="6e1a2-103">Optimize build performance for solution</span></span>
<span data-ttu-id="6e1a2-104">Visual Studio 2017 15.8 a později přidat novou položku nabídky v části **sestavení > kompilace ASP.NET > Optimalizovat výkon sestavení pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="6e1a2-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![snímek obrazovky s novou položku nabídky](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="6e1a2-106">ASP.NET zkompiluje za běhu, což znamená, že váš projekt ASP.NET provede s ní kopie kompilátor jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e1a2-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="6e1a2-107">Ale na počítači pro vývojáře při kopírování kompilátor neodpovídá kopie sady Visual Studio, výkon sestavení to má vliv v řádu sekund 1-3 na přírůstkové sestavení.</span><span class="sxs-lookup"><span data-stu-id="6e1a2-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="6e1a2-108">Tato funkce bude aktualizovat kopii projektu kompilátoru tak, aby odpovídaly v sadě Visual Studio, který by měl urychlit přírůstková sestavení.</span><span class="sxs-lookup"><span data-stu-id="6e1a2-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="6e1a2-109">To platí pouze pro projekty ASP.NET Framework, se nevztahují na ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e1a2-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
