---
title: "Články založené na projekty vytvořené pomocí jednotlivých uživatelských účtů"
author: rick-anderson
description: "Tento dokument obsahuje seznam článků založené na projekty vytvořené pomocí jednotlivých uživatelských účtů."
keywords: ASP.NET Core, autorizace, IAuthorizationService
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="728f3-104">Články založené na projekty vytvořené pomocí jednotlivých uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="728f3-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="728f3-105">Jádro ASP.NET Identity je součástí šablony projektů v sadě Visual Studio s parametrem "Jednotlivé uživatelské účty".</span><span class="sxs-lookup"><span data-stu-id="728f3-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="728f3-106">Ověření šablony jsou dostupné v rozhraní .NET Core rozhraní příkazového řádku s `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="728f3-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="728f3-107">V následujících článcích ukazují, jak použít kód, který vygenerovala v šablonách ASP.NET Core, které používají jednotlivých uživatelských účtů:</span><span class="sxs-lookup"><span data-stu-id="728f3-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="728f3-108">Dvoufaktorové ověřování pomocí serveru SMS</span><span class="sxs-lookup"><span data-stu-id="728f3-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="728f3-109">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="728f3-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="728f3-110">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněn autorizace</span><span class="sxs-lookup"><span data-stu-id="728f3-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)