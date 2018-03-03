---
title: "Microsoft.AspNetCore.All metapackage pro ASP.NET Core 2.x a novější"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage zahrnuje všechny podporované balíčků ASP.NET Core a Entity Framework Core, spolu s jejich závislosti."
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4cbb4f4068c65df586adb1a42b946f8971873352
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Microsoft.AspNetCore.All metapackage pro ASP.NET Core 2.x

Tato funkce vyžaduje rozhraní .NET zaměřená na aplikace ASP.NET Core 2.x základní 2.x.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage pro ASP.NET Core zahrnuje:

* Všechny podporované balíčky tým ASP.NET Core.
* Všechny podporované balíčků základní Entity Framework. 
* Interní a 3. stran závislosti používané ASP.NET Core a Entity Framework Core. 

Všechny funkce ASP.NET Core 2.x a Entity Framework Core 2.x jsou součástí `Microsoft.AspNetCore.All` balíčku. Tento balíček použít výchozí šablony projektů.

Číslo verze `Microsoft.AspNetCore.All` metapackage představuje ASP.NET Core verze a verze Entity Framework Core (v souladu s verze .NET Core).

Aplikace, které používají `Microsoft.AspNetCore.All` metapackage automaticky využívat výhod [.NET Core Runtime úložiště](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Modul Runtime úložiště obsahuje všechny prostředky potřebné ke spuštění 2.x aplikace ASP.NET Core runtime. Při použití `Microsoft.AspNetCore.All` metapackage, **žádné** prostředky z odkazované balíčky ASP.NET Core NuGet nasazených aplikací &mdash; úložiště .NET Core Runtime obsahuje tyto prostředky. Prostředky v úložišti Runtime jsou předkompilovaných ke zlepšení času spuštění aplikace.

Proces oříznutí balíčku můžete odebrat balíčky, které nepoužíváte. Oříznutý balíčky jsou vyloučeny ve výstupu publikované aplikace.

Následující *.csproj* souboru odkazy `Microsoft.AspNetCore.All` metapackage pro ASP.NET Core:

[!code-xml[](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
