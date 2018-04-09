---
title: Migrace z technologie ASP.NET na ASP.NET Core 2.0
author: isaac2004
description: Zobrazí pokyny k migraci existujících ASP.NET MVC nebo webového rozhraní API aplikací na technologii ASP.NET 2.0 jádra.
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/index
ms.openlocfilehash: 86b4ee5f431d1e23ed3ad2be5740af34176de531
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="5e42d-103">Migrace z technologie ASP.NET na ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="5e42d-103">Migrate from ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="5e42d-104">Podle [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="5e42d-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="5e42d-105">Tento článek slouží jako referenční příručka pro migraci aplikace ASP.NET na technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="5e42d-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e42d-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e42d-106">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

## <a name="target-frameworks"></a><span data-ttu-id="5e42d-107">Cílové rozhraní</span><span class="sxs-lookup"><span data-stu-id="5e42d-107">Target frameworks</span></span>
<span data-ttu-id="5e42d-108">Projekty ASP.NET Core 2.0 nabízí vývojářům flexibilitu cílení na .NET Core, rozhraní .NET Framework nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="5e42d-108">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="5e42d-109">V tématu [výběru mezi .NET Core a rozhraní .NET Framework pro server aplikace](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) k určení, které cílové rozhraní je nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="5e42d-109">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="5e42d-110">Při cílení na rozhraní .NET Framework, třeba projekty odkazovat jednotlivých balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="5e42d-110">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="5e42d-111">Cílení na .NET Core můžete omezit množství explicitní balíček odkazuje, díky technologii ASP.NET 2.0 základní [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="5e42d-111">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="5e42d-112">Nainstalujte `Microsoft.AspNetCore.All` metapackage ve vašem projektu:</span><span class="sxs-lookup"><span data-stu-id="5e42d-112">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="5e42d-113">Pokud je použita metapackage, se žádné balíčky, na kterou odkazuje metapackage nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="5e42d-113">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="5e42d-114">Úložišti .NET Core Runtime obsahuje tyto prostředky, a že předkompilovaných ke zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-114">The .NET Core Runtime Store includes these assets, and they're precompiled to improve performance.</span></span> <span data-ttu-id="5e42d-115">V tématu [Microsoft.AspNetCore.All metapackage pro ASP.NET Core 2.x](xref:fundamentals/metapackage) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5e42d-115">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="5e42d-116">Rozdíly struktura projektu</span><span class="sxs-lookup"><span data-stu-id="5e42d-116">Project structure differences</span></span>
<span data-ttu-id="5e42d-117">*.Csproj* formát souboru je jednodušší v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e42d-117">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="5e42d-118">Některé upozorňují na důležité změny patří:</span><span class="sxs-lookup"><span data-stu-id="5e42d-118">Some notable changes include:</span></span>
- <span data-ttu-id="5e42d-119">Explicitní zahrnutí souborů není nezbytné pro mají být považovány za součást projektu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-119">Explicit inclusion of files isn't necessary for them to be considered part of the project.</span></span> <span data-ttu-id="5e42d-120">Tím se snižuje riziko ke konfliktům sloučení XML při práci s velkými týmy.</span><span class="sxs-lookup"><span data-stu-id="5e42d-120">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="5e42d-121">Nejsou žádné na základě GUID odkazy na další projekty, což zlepšuje čitelnost souboru.</span><span class="sxs-lookup"><span data-stu-id="5e42d-121">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="5e42d-122">Soubor můžete upravit bez uvolnění v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5e42d-122">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Upravit CSPROJ možnost místní nabídky v Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="5e42d-124">Nahrazení souboru Global.asax</span><span class="sxs-lookup"><span data-stu-id="5e42d-124">Global.asax file replacement</span></span>
<span data-ttu-id="5e42d-125">ASP.NET Core zavedl nový mechanismus pro aplikaci zavádění.</span><span class="sxs-lookup"><span data-stu-id="5e42d-125">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="5e42d-126">Vstupní bod pro aplikace ASP.NET je *Global.asax* souboru.</span><span class="sxs-lookup"><span data-stu-id="5e42d-126">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="5e42d-127">Úlohy, jako je konfigurace směrování a registrace filtru a oblasti jsou zpracovávány v *Global.asax* souboru.</span><span class="sxs-lookup"><span data-stu-id="5e42d-127">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[](samples/globalasax-sample.cs)]

<span data-ttu-id="5e42d-128">Tento přístup páry v odstupu aplikace a serveru, na kterém je nasazená způsobem, který brání systému implementace.</span><span class="sxs-lookup"><span data-stu-id="5e42d-128">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="5e42d-129">Ve snaze oddělit [OWIN](http://owin.org/) byla zavedena zajistit čisticí způsob, jak používat více rozhraní společně.</span><span class="sxs-lookup"><span data-stu-id="5e42d-129">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="5e42d-130">OWIN poskytuje kanálu přidat pouze moduly, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="5e42d-130">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="5e42d-131">Hostitelské prostředí trvá [spuštění](xref:fundamentals/startup) funkce při konfiguraci služby a aplikace požadavku kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-131">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="5e42d-132">`Startup` zaregistruje sadu middlewaru s aplikací.</span><span class="sxs-lookup"><span data-stu-id="5e42d-132">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="5e42d-133">Pro každý požadavek aplikace volá všechny komponenty middlewaru se head ukazatel odkazovaného seznamu existující sadu obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="5e42d-133">For each request, the application calls each of the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="5e42d-134">Jednotlivé komponenty middlewaru můžete přidat jeden nebo více obslužné rutiny na žádost o zpracování kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-134">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="5e42d-135">To se provádí vrácení odkaz na obslužná rutina, která je nové head seznamu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-135">This is accomplished by returning a reference to the handler that's the new head of the list.</span></span> <span data-ttu-id="5e42d-136">Každý obslužná rutina zodpovídá za zapamatování a vyvolání další obslužná rutina v seznamu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-136">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="5e42d-137">Pomocí ASP.NET Core, vstupní bod aplikace je `Startup`, a už jsou závislé na *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="5e42d-137">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="5e42d-138">Při použití OWIN v rozhraní .NET Framework, použijte jako kanál nějak takto:</span><span class="sxs-lookup"><span data-stu-id="5e42d-138">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[](samples/webapi-owin.cs)]

<span data-ttu-id="5e42d-139">Konfiguruje výchozí trasy a výchozí XmlSerialization přes Json.</span><span class="sxs-lookup"><span data-stu-id="5e42d-139">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="5e42d-140">Podle potřeby (načítání služeb, nastavení konfigurace, statické soubory, atd.), přidejte do tohoto kanálu další Middleware.</span><span class="sxs-lookup"><span data-stu-id="5e42d-140">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="5e42d-141">ASP.NET Core používá podobný postup, ale není spoléhají na OWIN pro zpracování položky.</span><span class="sxs-lookup"><span data-stu-id="5e42d-141">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="5e42d-142">Místo toho, která se provádí prostřednictvím *Program.cs* `Main` metody (podobně jako konzolové aplikace) a `Startup` je načteno prostřednictvím zde.</span><span class="sxs-lookup"><span data-stu-id="5e42d-142">Instead, that's done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[](samples/program.cs)]

<span data-ttu-id="5e42d-143">`Startup` musí zahrnovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="5e42d-143">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="5e42d-144">V `Configure`, přidejte nezbytné middleware do kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-144">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="5e42d-145">V následujícím příkladu (na základě výchozí šablony webové stránky) několik rozšiřující metody slouží ke konfiguraci kanálu s podporou pro:</span><span class="sxs-lookup"><span data-stu-id="5e42d-145">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="5e42d-146">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="5e42d-146">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="5e42d-147">Chybové stránky</span><span class="sxs-lookup"><span data-stu-id="5e42d-147">Error pages</span></span>
* <span data-ttu-id="5e42d-148">Statické soubory</span><span class="sxs-lookup"><span data-stu-id="5e42d-148">Static files</span></span>
* <span data-ttu-id="5e42d-149">Jádro ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5e42d-149">ASP.NET Core MVC</span></span>
* <span data-ttu-id="5e42d-150">Identita</span><span class="sxs-lookup"><span data-stu-id="5e42d-150">Identity</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="5e42d-151">Hostitele a aplikace byla odpojené který nabízí flexibilitu přesunutí na různé platformy v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-151">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="5e42d-152">**Poznámka:** podrobnější odkaz na spuštění základní technologie ASP.NET a Middleware, najdete v části [spuštění v ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="5e42d-152">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="5e42d-153">Ukládání konfigurace</span><span class="sxs-lookup"><span data-stu-id="5e42d-153">Storing configurations</span></span>
<span data-ttu-id="5e42d-154">Technologie ASP.NET podporuje ukládání nastavení.</span><span class="sxs-lookup"><span data-stu-id="5e42d-154">ASP.NET supports storing settings.</span></span> <span data-ttu-id="5e42d-155">Tato nastavení se používají, například pro podporu prostředí, které byly nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e42d-155">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="5e42d-156">Běžnou praxí pro uložení všech vlastních páry klíč hodnota v `<appSettings>` části *Web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="5e42d-156">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[](samples/webconfig-sample.xml)]

<span data-ttu-id="5e42d-157">Aplikace, přečtěte si tato nastavení pomocí `ConfigurationManager.AppSettings` kolekce v `System.Configuration` obor názvů:</span><span class="sxs-lookup"><span data-stu-id="5e42d-157">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[](samples/read-webconfig.cs)]

<span data-ttu-id="5e42d-158">ASP.NET Core můžete ukládat konfigurační data pro aplikace v žádném souboru a je v rámci middleware zavádění načíst.</span><span class="sxs-lookup"><span data-stu-id="5e42d-158">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="5e42d-159">Výchozí soubor použitý v rámci šablon projektu je *appSettings.JSON určený*:</span><span class="sxs-lookup"><span data-stu-id="5e42d-159">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[](samples/appsettings-sample.json)]

<span data-ttu-id="5e42d-160">Načítání tento soubor do instance `IConfiguration` uvnitř vaší aplikace se provádí v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5e42d-160">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[](samples/startup-builder.cs)]

<span data-ttu-id="5e42d-161">Aplikace načte z `Configuration` získat nastavení:</span><span class="sxs-lookup"><span data-stu-id="5e42d-161">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[](samples/read-appsettings.cs)]

<span data-ttu-id="5e42d-162">Existují rozšíření tohoto přístupu, aby proces robustnější, například pomocí [vkládání závislostí](xref:fundamentals/dependency-injection) (DI) načíst služby s těmito hodnotami.</span><span class="sxs-lookup"><span data-stu-id="5e42d-162">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="5e42d-163">DI přístup poskytuje sadu objektů konfigurace silného typu.</span><span class="sxs-lookup"><span data-stu-id="5e42d-163">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="5e42d-164">**Poznámka:** podrobnější odkaz na ASP.NET Core konfigurace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5e42d-164">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="5e42d-165">Vkládání nativní závislostí</span><span class="sxs-lookup"><span data-stu-id="5e42d-165">Native dependency injection</span></span>
<span data-ttu-id="5e42d-166">Důležité cílem při sestavování velkých škálovatelné aplikace je volné párování komponent a služeb.</span><span class="sxs-lookup"><span data-stu-id="5e42d-166">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="5e42d-167">[Vkládání závislostí](xref:fundamentals/dependency-injection) je Oblíbené technika pro dosažení tohoto cíle, a je nativní součást ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e42d-167">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it's a native component of ASP.NET Core.</span></span>

<span data-ttu-id="5e42d-168">V aplikacích ASP.NET vývojáři spoléhají na knihovnu třetích stran k implementaci vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="5e42d-168">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="5e42d-169">Jedna taková knihovna je [Unity](https://github.com/unitycontainer/unity), zadaný ve Microsoft Patterns & postupy.</span><span class="sxs-lookup"><span data-stu-id="5e42d-169">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="5e42d-170">Příklad nastavení vkládání závislostí s Unity je implementace `IDependencyResolver` který zabalí `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="5e42d-170">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="5e42d-171">Vytvoření instance vaše `UnityContainer`, registraci služby a nastavte překladač závislostí `HttpConfiguration` k nové instanci `UnityResolver` pro váš kontejner:</span><span class="sxs-lookup"><span data-stu-id="5e42d-171">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="5e42d-172">Vložit `IProductRepository` tam, kde je potřeba:</span><span class="sxs-lookup"><span data-stu-id="5e42d-172">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="5e42d-173">Protože vkládání závislostí je součástí ASP.NET Core, můžete přidat služby v `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5e42d-173">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[](samples/configure-services.cs)]

<span data-ttu-id="5e42d-174">Úložiště můžete vložit odkudkoli, jako byla platí Unity.</span><span class="sxs-lookup"><span data-stu-id="5e42d-174">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="5e42d-175">**Poznámka:** pro podrobné referenční informace k vkládání závislostí v ASP.NET Core, najdete v části [vkládání závislostí v ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="5e42d-175">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="5e42d-176">Zpracování statických souborů.</span><span class="sxs-lookup"><span data-stu-id="5e42d-176">Serving static files</span></span>
<span data-ttu-id="5e42d-177">Důležitou součást vývoj webu je schopnost poskytovat statické, klientské prostředky.</span><span class="sxs-lookup"><span data-stu-id="5e42d-177">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="5e42d-178">Nejběžnější příkladem statické soubory jsou HTML, CSS, Javascript a obrázků.</span><span class="sxs-lookup"><span data-stu-id="5e42d-178">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="5e42d-179">Tyto soubory muset být uložena v umístění s aplikace (nebo CDN) a tak mohou být načteny požadavkem na něj odkazovat.</span><span class="sxs-lookup"><span data-stu-id="5e42d-179">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="5e42d-180">Tento proces se změnilo v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e42d-180">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="5e42d-181">V technologii ASP.NET jsou statické soubory uložené v různých adresářů a v zobrazení odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e42d-181">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="5e42d-182">V ASP.NET Core jsou statické soubory uložené v kořenu"web" (*&lt;obsahu kořenové&gt;/wwwroot*), pokud není uvedeno jinak.</span><span class="sxs-lookup"><span data-stu-id="5e42d-182">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="5e42d-183">Soubory jsou načteny do kanálu požadavku vyvoláním `UseStaticFiles` metoda rozšíření z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5e42d-183">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="5e42d-184">**Poznámka:** Pokud cílení na rozhraní .NET Framework, nainstalujte balíček NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="5e42d-184">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="5e42d-185">Například prostředek bitové kopie v *wwwroot nebo obrázky* , jako je přístupný v prohlížeči na umístění složky `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="5e42d-185">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="5e42d-186">**Poznámka:** podrobnější odkaz na zpracování statických souborů v ASP.NET Core, najdete v části [pracovat s statické soubory v ASP.NET Core](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="5e42d-186">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Work with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e42d-187">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5e42d-187">Additional resources</span></span>

* [<span data-ttu-id="5e42d-188">Portování knihovny .NET Core</span><span class="sxs-lookup"><span data-stu-id="5e42d-188">Porting Libraries to .NET Core</span></span>](/dotnet/core/porting/libraries)
