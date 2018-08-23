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
# <a name="optimize-build-performance-for-solution"></a>Optimalizace výkonu sestavení pro řešení
Visual Studio 2017 15.8 a později přidat novou položku nabídky v části **sestavení > kompilace ASP.NET > Optimalizovat výkon sestavení pro řešení**.

![snímek obrazovky s novou položku nabídky](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET zkompiluje za běhu, což znamená, že váš projekt ASP.NET provede s ní kopie kompilátor jeho zobrazení. Ale na počítači pro vývojáře při kopírování kompilátor neodpovídá kopie sady Visual Studio, výkon sestavení to má vliv v řádu sekund 1-3 na přírůstkové sestavení. Tato funkce bude aktualizovat kopii projektu kompilátoru tak, aby odpovídaly v sadě Visual Studio, který by měl urychlit přírůstková sestavení.

To platí pouze pro projekty ASP.NET Framework, se nevztahují na ASP.NET Core.
