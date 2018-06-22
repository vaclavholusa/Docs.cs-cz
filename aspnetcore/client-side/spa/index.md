---
title: Použití šablon jedné stránky aplikace s ASP.NET Core
author: SteveSandersonMS
description: Zjistěte, jak nainstalovat a začít pracovat se šablonami projektů ASP.NET Core jedné stránky aplikace (SPA).
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291531"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Použití šablon jedné stránky aplikace s ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Vydaná .NET Core 2.0.x SDK zahrnuje starší šablony projektů pro úhlová reagují a s Redux reagovat. Tato dokumentace se o tyto starší šablony projektů. Tato dokumentace je pro nejnovější úhlová reagují a reagují s – obnovení šablony, které může být nainstalován ručně na technologii ASP.NET 2.0 jádra. Šablony jsou zahrnuté ve výchozím nastavení se ASP.NET Core 2.1.

::: moniker-end

## <a name="prerequisites"></a>Požadavky

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), verze 6 nebo novější

## <a name="installation"></a>Instalace

::: moniker range=">= aspnetcore-2.1"

Šablony jsou již nainstalovány s ASP.NET Core 2.1.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pokud máte aplikaci ASP.NET 2.0 jádra, spusťte následující příkaz k instalaci aktualizované šablony ASP.NET Core pro úhlová, reagovat a reagují s Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>Použití šablon

* [Použijte šablonu úhlová projektu](xref:spa/angular)
* [Pomocí šablony projektu reagují](xref:spa/react)
* [Použití reagují pomocí šablony projektu – obnovení](xref:spa/react-with-redux)
