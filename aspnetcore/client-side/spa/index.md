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
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="ced30-103">Použití šablon jedné stránky aplikace s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ced30-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="ced30-104">Vydaná .NET Core 2.0.x SDK zahrnuje starší šablony projektů pro úhlová reagují a s Redux reagovat.</span><span class="sxs-lookup"><span data-stu-id="ced30-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="ced30-105">Tato dokumentace se o tyto starší šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="ced30-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="ced30-106">Tato dokumentace je pro nejnovější úhlová reagují a reagují s – obnovení šablony, které může být nainstalován ručně na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="ced30-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="ced30-107">Šablony jsou zahrnuté ve výchozím nastavení se ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ced30-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="ced30-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ced30-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="ced30-109">[Node.js](https://nodejs.org), verze 6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="ced30-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="ced30-110">Instalace</span><span class="sxs-lookup"><span data-stu-id="ced30-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ced30-111">Šablony jsou již nainstalovány s ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ced30-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ced30-112">Pokud máte aplikaci ASP.NET 2.0 jádra, spusťte následující příkaz k instalaci aktualizované šablony ASP.NET Core pro úhlová, reagovat a reagují s Redux:</span><span class="sxs-lookup"><span data-stu-id="ced30-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="ced30-113">Použití šablon</span><span class="sxs-lookup"><span data-stu-id="ced30-113">Use the templates</span></span>

* [<span data-ttu-id="ced30-114">Použijte šablonu úhlová projektu</span><span class="sxs-lookup"><span data-stu-id="ced30-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="ced30-115">Pomocí šablony projektu reagují</span><span class="sxs-lookup"><span data-stu-id="ced30-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="ced30-116">Použití reagují pomocí šablony projektu – obnovení</span><span class="sxs-lookup"><span data-stu-id="ced30-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
