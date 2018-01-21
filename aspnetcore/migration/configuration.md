---
title: Migrace konfigurace
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 90d9f730d31c2c70aec3d47610b9031a7d8e621b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-configuration"></a><span data-ttu-id="c06da-102">Migrace konfigurace</span><span class="sxs-lookup"><span data-stu-id="c06da-102">Migrating Configuration</span></span>

<span data-ttu-id="c06da-103">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c06da-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c06da-104">V předchozím článku jsme začali [migrace projektu aplikace ASP.NET MVC do architektury ASP.NET MVC základní](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c06da-104">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="c06da-105">V tomto článku jsme migruje konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c06da-105">In this article, we migrate configuration.</span></span>

<span data-ttu-id="c06da-106">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c06da-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="c06da-107">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="c06da-107">Setup Configuration</span></span>

<span data-ttu-id="c06da-108">ASP.NET Core už používá *Global.asax* a *web.config* soubory, které jsou použity v předchozích verzích technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c06da-108">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="c06da-109">V předchozích verzích technologie ASP.NET byl uveden logika spuštění aplikace do `Application_StartUp` metoda v rámci *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="c06da-109">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="c06da-110">Později v architektuře ASP.NET MVC *Startup.cs* souboru je zahrnutý v kořenovém adresáři projektu; a byla volána, když se aplikace spustí.</span><span class="sxs-lookup"><span data-stu-id="c06da-110">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="c06da-111">ASP.NET Core přijal tento přístup zcela tím, že všechny logika spuštění v *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="c06da-111">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="c06da-112">*Web.config* souboru nahradila také v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c06da-112">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="c06da-113">Konfigurace samotného může být nyní nakonfigurována, jako součást spuštění aplikace postup popsaný v *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c06da-113">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="c06da-114">Konfigurace může stále používat soubory XML, ale obvykle projektů ASP.NET Core umístí hodnoty konfigurace v souboru ve formátu JSON, jako *appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="c06da-114">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="c06da-115">Jádro ASP.NET – systém konfigurace lze také snadný přístup k proměnné prostředí, které můžete zadat umístění maximalizace zabezpečení a odolnosti pro konkrétní prostředí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c06da-115">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="c06da-116">To platí hlavně pro tajné klíče jako připojovací řetězce a klíče rozhraní API, které by neměla být zaškrtnuta do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c06da-116">This is especially true for secrets like connection strings and API keys that should not be checked into source control.</span></span> <span data-ttu-id="c06da-117">V tématu [konfigurace](xref:fundamentals/configuration/index) Další informace o konfiguraci v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c06da-117">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="c06da-118">V tomto článku jsme začínáte s projektu ASP.NET Core částečně migrovat z [předchozím článku](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c06da-118">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="c06da-119">Chcete-li nastavit konfiguraci, přidejte následující konstruktor a vlastnost, která má *Startup.cs* soubor umístěný v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="c06da-119">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="c06da-120">Poznámka, která v tomto okamžiku *Startup.cs* soubor nebude kompilovat, protože musíme přidejte následující `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="c06da-120">Note that at this point, the *Startup.cs* file will not compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="c06da-121">Přidat *appSettings.JSON určený* souboru do kořenového adresáře projektu pomocí šablony odpovídající položky:</span><span class="sxs-lookup"><span data-stu-id="c06da-121">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Přidat AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="c06da-123">Migrace nastavení konfigurace ze souboru web.config</span><span class="sxs-lookup"><span data-stu-id="c06da-123">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="c06da-124">Naše projektu ASP.NET MVC zahrnout požadované databáze připojovací řetězec v *web.config*v `<connectionStrings>` elementu.</span><span class="sxs-lookup"><span data-stu-id="c06da-124">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="c06da-125">V našem projektu ASP.NET Core přidáme k ukládání těchto informací v *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="c06da-125">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="c06da-126">Otevřete *appSettings.JSON určený*a Všimněte si, že již zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="c06da-126">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="c06da-127">Ve zvýrazněný řádek použité v ukázkách výše, změňte název databáze z **_CHANGE_ME** na název vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="c06da-127">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="c06da-128">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c06da-128">Summary</span></span>

<span data-ttu-id="c06da-129">ASP.NET Core umístí všechny logika spuštění aplikace v jednom souboru, ve kterém může být nezbytné služby a závislosti definované a nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="c06da-129">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="c06da-130">Nahradí *web.config* soubor s flexibilní konfigurace funkcí, které můžete využít celou řadu formáty souborů, například JSON, jakož i proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="c06da-130">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
