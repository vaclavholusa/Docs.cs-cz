---
title: Články podle projekty ASP.NET Core, které jsou vytvořené pomocí jednotlivých uživatelských účtů
author: rick-anderson
description: Vyhledat články založené na projekty ASP.NET Core, které jsou vytvořené pomocí jednotlivých uživatelských účtů.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523061"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="01614-103">Články podle projekty ASP.NET Core, které jsou vytvořené pomocí jednotlivých uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="01614-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="01614-104">ASP.NET Core Identity je součástí šablony projektů v sadě Visual Studio s parametrem "Jednotlivých uživatelských účtů".</span><span class="sxs-lookup"><span data-stu-id="01614-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="01614-105">Ověřování šablony jsou k dispozici v rozhraní .NET Core CLI s `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="01614-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="01614-106">Bez ověřování</span><span class="sxs-lookup"><span data-stu-id="01614-106">No Authentication</span></span>

<span data-ttu-id="01614-107">Ověřování je zadaný v rozhraní příkazového řádku .NET Core s `-au` možnost.</span><span class="sxs-lookup"><span data-stu-id="01614-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="01614-108">V sadě Visual Studio **změna ověřování** dialogové okno je k dispozici pro nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="01614-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="01614-109">Výchozí pro nové webové aplikace v sadě Visual Studio je **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="01614-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="01614-110">Projekty vytvořené bez ověřování:</span><span class="sxs-lookup"><span data-stu-id="01614-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="01614-111">Neobsahují webových stránek a uživatelského rozhraní pro přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="01614-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="01614-112">Neobsahují ověřovacího kódu.</span><span class="sxs-lookup"><span data-stu-id="01614-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="01614-113">Ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="01614-113">Windows Authentication</span></span>

<span data-ttu-id="01614-114">Ověřování Windows je určená pro nové webové aplikace v rozhraní příkazového řádku .NET Core s `-au Windows` možnost.</span><span class="sxs-lookup"><span data-stu-id="01614-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="01614-115">V sadě Visual Studio **změna ověřování** dialogové okno obsahuje **ověřování Windows** možnosti.</span><span class="sxs-lookup"><span data-stu-id="01614-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="01614-116">Pokud je vybrána možnost ověření Windows, aplikace je nakonfigurovaná pro používání [modul IIS ověřování Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="01614-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="01614-117">Ověřování Windows je určená pro intranetové weby.</span><span class="sxs-lookup"><span data-stu-id="01614-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01614-118">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01614-118">Additional resources</span></span>

<span data-ttu-id="01614-119">Následující články popisují, jak používat kód vygenerovaný v šablony ASP.NET Core, které používají jednotlivých uživatelských účtů:</span><span class="sxs-lookup"><span data-stu-id="01614-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="01614-120">Dvoufaktorové ověřování přes SMS</span><span class="sxs-lookup"><span data-stu-id="01614-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="01614-121">Potvrzení účtu a obnovení hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01614-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="01614-122">Vytvoření aplikace ASP.NET Core s uživatelskými daty chráněnými autorizací</span><span class="sxs-lookup"><span data-stu-id="01614-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
