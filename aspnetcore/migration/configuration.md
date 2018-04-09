---
title: Migruje konfiguraci pro jádro ASP.NET
author: ardalis
description: Zjistěte, jak migrovat konfigurace z projektu aplikace ASP.NET MVC do projektu aplikace ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 5bb89401ac54b54810fe5724b293ae8ed7e5afef
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="ce4ff-103">Migruje konfiguraci pro jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ce4ff-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="ce4ff-104">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ce4ff-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ce4ff-105">V předchozím článku jsme začali [migrovat projektu aplikace ASP.NET MVC do architektury ASP.NET MVC základní](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="ce4ff-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="ce4ff-106">V tomto článku jsme migruje konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="ce4ff-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce4ff-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="ce4ff-108">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="ce4ff-108">Setup Configuration</span></span>

<span data-ttu-id="ce4ff-109">ASP.NET Core už používá *Global.asax* a *web.config* soubory, které jsou použity v předchozích verzích technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="ce4ff-110">V předchozích verzích technologie ASP.NET byl uveden logika spuštění aplikace do `Application_StartUp` metoda v rámci *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="ce4ff-111">Později v architektuře ASP.NET MVC *Startup.cs* souboru je zahrnutý v kořenovém adresáři projektu; a byla volána, když se aplikace spustí.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="ce4ff-112">ASP.NET Core přijal tento přístup zcela tím, že všechny logika spuštění v *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="ce4ff-113">*Web.config* souboru nahradila také v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="ce4ff-114">Konfigurace samotného může být nyní nakonfigurována, jako součást spuštění aplikace postup popsaný v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="ce4ff-115">Konfigurace může stále používat soubory XML, ale obvykle projektů ASP.NET Core umístí hodnoty konfigurace v souboru ve formátu JSON, jako *appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="ce4ff-116">ASP.NET Core konfiguračního systému může také snadný přístup k proměnné prostředí, které můžete zadat [zabezpečení a odolnosti umístění](xref:security/app-secrets) pro konkrétní prostředí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="ce4ff-117">To platí hlavně pro tajné klíče jako připojovací řetězce a klíče rozhraní API, které by neměla být zaškrtnuta do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="ce4ff-118">V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace o konfiguraci v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="ce4ff-119">V tomto článku jsme začínáte s projektu ASP.NET Core částečně migrovat z [předchozím článku](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="ce4ff-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="ce4ff-120">Chcete-li nastavit konfiguraci, přidejte následující konstruktor a vlastnost, která má *Startup.cs* soubor umístěný v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="ce4ff-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="ce4ff-121">Poznámka, která v tomto okamžiku *Startup.cs* soubor nebude kompilovat, protože musíme přidejte následující `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="ce4ff-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="ce4ff-122">Přidat *appSettings.JSON určený* souboru do kořenového adresáře projektu pomocí šablony odpovídající položky:</span><span class="sxs-lookup"><span data-stu-id="ce4ff-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Přidat AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="ce4ff-124">Migrace nastavení konfigurace ze souboru web.config</span><span class="sxs-lookup"><span data-stu-id="ce4ff-124">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="ce4ff-125">Naše projektu ASP.NET MVC zahrnout požadované databáze připojovací řetězec v *web.config*v `<connectionStrings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="ce4ff-126">V našem projektu ASP.NET Core přidáme k ukládání těchto informací v *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="ce4ff-127">Otevřete *appSettings.JSON určený*a Všimněte si, že již zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="ce4ff-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="ce4ff-128">Ve zvýrazněný řádek použité v ukázkách výše, změňte název databáze z **_CHANGE_ME** na název vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="ce4ff-129">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ce4ff-129">Summary</span></span>

<span data-ttu-id="ce4ff-130">ASP.NET Core umístí všechny logika spuštění aplikace v jednom souboru, ve kterém může být nezbytné služby a závislosti definované a nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="ce4ff-131">Nahradí *web.config* soubor s flexibilní konfigurace funkcí, které můžete využít celou řadu formáty souborů, například JSON, jakož i proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce4ff-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
