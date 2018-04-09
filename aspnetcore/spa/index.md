---
title: Použití šablon jedné stránky aplikace s ASP.NET Core
author: SteveSandersonMS
description: Zjistěte, jak nainstalovat a začít pracovat se šablonami projektů ASP.NET Core jedné stránky aplikace (SPA).
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Použití šablon jedné stránky aplikace s ASP.NET Core

> [!NOTE]
> Vydaná .NET Core 2.0.x SDK zahrnuje starší šablony projektů pro úhlová reagují a s Redux reagovat. Tato dokumentace se o tyto starší šablony projektů. Tato dokumentace je pro nejnovější úhlová reagují a reagují s – obnovení šablony, které může být nainstalován ručně na technologii ASP.NET 2.0 jádra. Šablony jsou zahrnuté ve výchozím nastavení se ASP.NET Core 2.1.

## <a name="prerequisites"></a>Požadavky

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), verze 6 nebo novější

## <a name="installation"></a>Instalace

Pokud máte aplikaci ASP.NET 2.0 jádra, spusťte následující příkaz k instalaci aktualizované šablony ASP.NET Core pro úhlová, reagovat a reagují s Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>Použití šablon

- [Použijte šablonu úhlová projektu](xref:spa/angular)
- [Pomocí šablony projektu reagují](xref:spa/react)
- [Použití reagují pomocí šablony projektu – obnovení](xref:spa/react-with-redux)
