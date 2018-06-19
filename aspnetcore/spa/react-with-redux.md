---
title: Pomocí reagují s-– obnovení v šabloně projektů ASP.NET Core
author: SteveSandersonMS
description: Zjistěte, jak začít pracovat se šablonou projektu ASP.NET Core jedné stránky aplikace (SPA) pro reagují s Redux a vytvořit aplikaci reagují.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 7ec4f6d53a4723ace087b1dc256de7845cb44cc6
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555219"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="ef3fd-103">Pomocí reagují s-– obnovení v šabloně projektů ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef3fd-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="ef3fd-104">Tato dokumentace není o reagují s-– obnovení šablona projektu součástí technologie ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="ef3fd-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="ef3fd-105">Jde o novější reagují s-– obnovení šablony, do kterého můžete ručně aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ef3fd-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="ef3fd-106">Šablona je součástí 2.1 jádro ASP.NET ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef3fd-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="ef3fd-107">Aktualizovanou šablonu projektu reagují s Redux poskytuje příhodný výchozí bod pro aplikace ASP.NET Core pomocí reagovat, Redux, a [vytvořit aplikaci reagují](https://github.com/facebookincubator/create-react-app) konvence (CRA) k implementaci bohatou a klientské uživatelské rozhraní (UI).</span><span class="sxs-lookup"><span data-stu-id="ef3fd-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="ef3fd-108">S výjimkou příkaz vytvoření projektu všechny informace o šabloně reagují s-– obnovení je stejný jako šablonu reagují.</span><span class="sxs-lookup"><span data-stu-id="ef3fd-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="ef3fd-109">Chcete-li vytvořit tento typ projektu, spusťte `dotnet new reactredux` místo `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="ef3fd-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="ef3fd-110">Další informace o funkcích, které jsou společné pro obě reagují na základě šablony najdete v tématu [reagovat dokumentace šablony](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="ef3fd-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
