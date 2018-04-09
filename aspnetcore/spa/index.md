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
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="9d1b5-103">Použití šablon jedné stránky aplikace s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d1b5-103">Use the Single Page Application templates with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="9d1b5-104">Vydaná .NET Core 2.0.x SDK zahrnuje starší šablony projektů pro úhlová reagují a s Redux reagovat.</span><span class="sxs-lookup"><span data-stu-id="9d1b5-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="9d1b5-105">Tato dokumentace se o tyto starší šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="9d1b5-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="9d1b5-106">Tato dokumentace je pro nejnovější úhlová reagují a reagují s – obnovení šablony, které může být nainstalován ručně na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="9d1b5-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="9d1b5-107">Šablony jsou zahrnuté ve výchozím nastavení se ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="9d1b5-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d1b5-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9d1b5-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="9d1b5-109">[Node.js](https://nodejs.org), verze 6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="9d1b5-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="9d1b5-110">Instalace</span><span class="sxs-lookup"><span data-stu-id="9d1b5-110">Installation</span></span>

<span data-ttu-id="9d1b5-111">Pokud máte aplikaci ASP.NET 2.0 jádra, spusťte následující příkaz k instalaci aktualizované šablony ASP.NET Core pro úhlová, reagovat a reagují s Redux:</span><span class="sxs-lookup"><span data-stu-id="9d1b5-111">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="9d1b5-112">Použití šablon</span><span class="sxs-lookup"><span data-stu-id="9d1b5-112">Use the templates</span></span>

- [<span data-ttu-id="9d1b5-113">Použijte šablonu úhlová projektu</span><span class="sxs-lookup"><span data-stu-id="9d1b5-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="9d1b5-114">Pomocí šablony projektu reagují</span><span class="sxs-lookup"><span data-stu-id="9d1b5-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="9d1b5-115">Použití reagují pomocí šablony projektu – obnovení</span><span class="sxs-lookup"><span data-stu-id="9d1b5-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
