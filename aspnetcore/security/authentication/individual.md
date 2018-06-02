---
title: Články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů
author: rick-anderson
description: Zjistit články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 699def0133f53b922477ac294f70db41998945ef
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729545"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="8c04f-103">Články založené na ASP.NET Core projekty vytvořené pomocí jednotlivých uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="8c04f-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="8c04f-104">Jádro ASP.NET Identity je součástí šablony projektů v sadě Visual Studio s parametrem "Jednotlivé uživatelské účty".</span><span class="sxs-lookup"><span data-stu-id="8c04f-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="8c04f-105">Ověření šablony jsou dostupné v rozhraní .NET Core rozhraní příkazového řádku s `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="8c04f-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="8c04f-106">V následujících článcích ukazují, jak použít kód, který vygenerovala v šablonách ASP.NET Core, které používají jednotlivých uživatelských účtů:</span><span class="sxs-lookup"><span data-stu-id="8c04f-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="8c04f-107">Dvoufaktorové ověřování přes SMS</span><span class="sxs-lookup"><span data-stu-id="8c04f-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="8c04f-108">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c04f-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="8c04f-109">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace</span><span class="sxs-lookup"><span data-stu-id="8c04f-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
