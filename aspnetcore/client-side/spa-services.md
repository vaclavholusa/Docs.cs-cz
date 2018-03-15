---
title: "K vytvoření jednostránkové aplikace v ASP.NET Core použijte JavaScriptServices"
author: scottaddie
description: "Seznamte se s výhodami JavaScriptServices vytvořit jedné stránce aplikace (SPA) zajišťoval ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c962fc160cf39ad1c69f4269616c993fde420035
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="5cbdd-103">K vytvoření jednostránkové aplikace v ASP.NET Core použijte JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="5cbdd-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="5cbdd-104">Podle [Scott Addie](https://github.com/scottaddie) a [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="5cbdd-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="5cbdd-105">Jedné stránce aplikace (SPA) je typu oblíbené webové aplikace z důvodu jeho vyplývajících bohatých zkušeností uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="5cbdd-106">Integrace architektury SPA klienta nebo knihovny, například [úhlová](https://angular.io/) nebo [reagovat](https://facebook.github.io/react/), pomocí rozhraní na straně serveru jako ASP.NET Core může být obtížné.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="5cbdd-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) byla vyvinuta ke snížení třecí procesu integrace.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="5cbdd-108">Umožňuje bezproblémové operace mezi různých klientských a serverových sad technologie.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="5cbdd-109">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cbdd-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="5cbdd-110">Co je JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="5cbdd-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="5cbdd-111">JavaScriptServices je kolekce technologií na straně klienta pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="5cbdd-112">Jeho cílem je pozice ASP.NET Core jako vývojáře upřednostňované serverovou platformu pro sestavování SPA.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="5cbdd-113">JavaScriptServices se skládá ze tří různých balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="5cbdd-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="5cbdd-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="5cbdd-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="5cbdd-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="5cbdd-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="5cbdd-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="5cbdd-117">Tyto balíčky jsou užitečné, pokud je:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-117">These packages are useful if you:</span></span>
* <span data-ttu-id="5cbdd-118">Spuštění JavaScriptu na serveru</span><span class="sxs-lookup"><span data-stu-id="5cbdd-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="5cbdd-119">Použít SPA framework nebo knihovny</span><span class="sxs-lookup"><span data-stu-id="5cbdd-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="5cbdd-120">Prostředky na straně klienta s Webpack sestavení</span><span class="sxs-lookup"><span data-stu-id="5cbdd-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="5cbdd-121">Velká část fokusu v tomto článku je umístěn na pomocí balíčku SpaServices.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="5cbdd-122">Co je SpaServices?</span><span class="sxs-lookup"><span data-stu-id="5cbdd-122">What is SpaServices?</span></span>

<span data-ttu-id="5cbdd-123">SpaServices byl vytvořen na pozici jako vývojáře upřednostňované serverovou platformu pro sestavování SPA ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="5cbdd-124">SpaServices není vyžadován k vývoji SPA s ASP.NET Core a nebude vám nutit konkrétní klienta framework.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="5cbdd-125">SpaServices poskytuje užitečné infrastruktury, jako:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="5cbdd-126">Dokončení fáze před vykreslením na straně serveru</span><span class="sxs-lookup"><span data-stu-id="5cbdd-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="5cbdd-127">Middleware Webpack vývojářů</span><span class="sxs-lookup"><span data-stu-id="5cbdd-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="5cbdd-128">Nahrazení aktivního modulu</span><span class="sxs-lookup"><span data-stu-id="5cbdd-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="5cbdd-129">Směrování pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="5cbdd-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="5cbdd-130">Tyto součástí infrastruktury zvýšit souhrnně, pracovní postup vývoje a prostředí runtime.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="5cbdd-131">Součástí může být přijata jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="5cbdd-132">Předpoklady pro použití SpaServices</span><span class="sxs-lookup"><span data-stu-id="5cbdd-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="5cbdd-133">Chcete-li pracovat s SpaServices, nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="5cbdd-134">[Node.js](https://nodejs.org/) (verze 6 nebo novější) s npm</span><span class="sxs-lookup"><span data-stu-id="5cbdd-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="5cbdd-135">Pokud chcete ověřit tyto součásti jsou nainstalovány a možné najít, spusťte z příkazového řádku následující:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="5cbdd-136">Poznámka: Pokud nasazujete webovou stránku Azure, nemusíte dělat nic zde &mdash; Node.js je nainstalovaná a k dispozici v serverových prostředích.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="5cbdd-137">[.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="5cbdd-137">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="5cbdd-138">Pokud jste v systému Windows, instalaci je možné provést tak, že vyberete Visual Studio 2017 **vývoj pro různé platformy .NET Core** zatížení.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-138">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="5cbdd-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="5cbdd-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="5cbdd-140">Dokončení fáze před vykreslením na straně serveru</span><span class="sxs-lookup"><span data-stu-id="5cbdd-140">Server-side prerendering</span></span>

<span data-ttu-id="5cbdd-141">Univerzální aplikace (také označované jako isomorphic) je aplikace JavaScript, může spustit jak na serveru a klienta.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="5cbdd-142">Úhlová reagují a dalších oblíbených rozhraní poskytují univerzální platformu pro vývoj styl této aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="5cbdd-143">Na nápad je nejdřív vykreslení součástí framework na serveru pomocí Node.js a následně delegovat další provádění klientovi.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="5cbdd-144">ASP.NET Core [značky Pomocníci](xref:mvc/views/tag-helpers/intro) poskytované SpaServices zjednodušení implementace modulů serverové dokončení fáze před vykreslením vyvoláním funkce jazyka JavaScript na serveru.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5cbdd-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cbdd-145">Prerequisites</span></span>

<span data-ttu-id="5cbdd-146">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-146">Install the following:</span></span>
* <span data-ttu-id="5cbdd-147">[ASPNET-dokončení fáze před vykreslením](https://www.npmjs.com/package/aspnet-prerendering) balíčku npm:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-147">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="5cbdd-148">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-148">Configuration</span></span>

<span data-ttu-id="5cbdd-149">Pomocníci značky jsou vytvářeny zjistitelný prostřednictvím registrace oboru názvů v projektu *_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-149">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="5cbdd-150">Tyto značky Pomocníci abstraktní rychle rozbor všech komunikaci přímo s nižší úrovně rozhraní API s využitím syntaxe jazyka HTML v zobrazení syntaxe Razor:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-150">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="5cbdd-151">`asp-prerender-module` Značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="5cbdd-151">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="5cbdd-152">`asp-prerender-module` Značky pomocné rutiny, používá v předchozím příkladu kódu, provede *ClientApp/dist/main-server.js* na serveru pomocí Node.js.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-152">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="5cbdd-153">Pro přehlednost na saké *hlavní server.js* je artefakt úloha transpilation TypeScript JavaScript v souboru [Webpack](http://webpack.github.io/) proces sestavení.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-153">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="5cbdd-154">Definuje vstupní bod zástupce Webpack `main-server`; a traversal graf závislostí pro tento alias začíná na *ClientApp nebo spouštěcí server.ts* souboru:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-154">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="5cbdd-155">V následujícím příkladu úhlová *ClientApp nebo spouštěcí server.ts* využívá soubor `createServerRenderer` funkce a `RenderResult` typ `aspnet-prerendering` npm balíček ke konfiguraci serveru vykreslování prostřednictvím Node.js.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-155">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="5cbdd-156">Značka jazyka HTML, které jsou určené pro vykreslování na straně serveru je předána volání funkce, vyřešte, které je zabalena v JavaScriptu silného typu `Promise` objektu.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-156">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="5cbdd-157">`Promise` Násobek objektu je, že asynchronně poskytuje kód HTML na stránku pro vkládání v elementu DOM na zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-157">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="5cbdd-158">`asp-prerender-data` Značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="5cbdd-158">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="5cbdd-159">Při kombinaci s `asp-prerender-module` značky pomocné rutiny, `asp-prerender-data` značky pomocná lze předat kontextové informace ze zobrazení syntaxe Razor JavaScript na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-159">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="5cbdd-160">Například následující kód předá data uživatele `main-server` modul:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-160">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="5cbdd-161">Přijatého `UserName` argument serializován pomocí předdefinovaných serializátor JSON a je uložen v `params.data` objektu.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-161">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="5cbdd-162">V následujícím příkladu úhlová, data se používají k vytvoření přizpůsobené pozdravu v rámci `h1` element:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-162">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="5cbdd-163">Poznámka: Názvy vlastností předaná Pomocníci značky jsou reprezentované pomocí **PascalCase** zápis.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-163">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="5cbdd-164">Který kontrastu jazyka JavaScript, kde jsou reprezentované stejné názvy vlastností s **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-164">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="5cbdd-165">Výchozí konfigurace serializace JSON je zodpovědná za tento rozdíl.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-165">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="5cbdd-166">Rozšířit na předchozí příklad kódu, data mohou být předána ze serveru do zobrazení pomocí hydrating `globals` vlastnost poskytnuté `resolve` funkce:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-166">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="5cbdd-167">`postList` Pole definované uvnitř `globals` objekt je připojený k prohlížeče globální `window` objektu.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-167">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="5cbdd-168">Tato proměnná zvedání do globálního oboru eliminuje zdvojení úsilí, zejména v jak, se vztahuje k načítání stejná data jednou na serveru a znovu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-168">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![globální proměnné postList připojený k objektu okna](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="5cbdd-170">Middleware Webpack vývojářů</span><span class="sxs-lookup"><span data-stu-id="5cbdd-170">Webpack Dev Middleware</span></span>

<span data-ttu-id="5cbdd-171">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) zavádí zjednodušenou vývoj pracovního postupu při němž Webpack sestavení prostředky na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-171">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="5cbdd-172">Middleware automaticky zkompiluje a slouží prostředky na straně klienta, když je znovu na stránce v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-172">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="5cbdd-173">Alternativní způsob je ručně vyvolání Webpack pomocí skriptu buildu npm projektu při změně závislost třetích stran nebo vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-173">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="5cbdd-174">Npm sestavení skriptu *package.json* souboru je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-174">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="5cbdd-175">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cbdd-175">Prerequisites</span></span>

<span data-ttu-id="5cbdd-176">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-176">Install the following:</span></span>
* <span data-ttu-id="5cbdd-177">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) balíčku npm:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-177">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="5cbdd-178">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-178">Configuration</span></span>

<span data-ttu-id="5cbdd-179">Middleware Webpack vývojářů je zaregistrovat do kanálu požadavku HTTP přes následující kód do *Startup.cs* souboru `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-179">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="5cbdd-180">`UseWebpackDevMiddleware` Před musí být volána metoda rozšíření [registrace hostování statických souborů](xref:fundamentals/static-files) prostřednictvím `UseStaticFiles` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-180">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="5cbdd-181">Z bezpečnostních důvodů se zaregistrujte middleware jenom v případě, že aplikace běží v režimu pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-181">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="5cbdd-182">*Webpack.config.js* souboru `output.publicPath` vlastnost informuje middleware můžete sledovat `dist` složku pro změny:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-182">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="5cbdd-183">Nahrazení aktivního modulu</span><span class="sxs-lookup"><span data-stu-id="5cbdd-183">Hot Module Replacement</span></span>

<span data-ttu-id="5cbdd-184">Představte si, že je Webpack [aktivního modulu nahrazení](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) funkce (HMR) jako vývoje [Webpack Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="5cbdd-184">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="5cbdd-185">HMR zavádí stejné výhody, ale další zjednodušuje pracovní postup vývoje tím, že automaticky aktualizuje obsah stránky po kompilování změny.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-185">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="5cbdd-186">Nepleťte si to s aktualizaci prohlížeče, což by narušilo aktuální stav v paměti a ladicí relace SPA.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-186">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="5cbdd-187">Je aktivní propojení mezi službou Middleware Webpack vývojářů a prohlížeče, což znamená, že změny budou přesunuty do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-187">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5cbdd-188">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cbdd-188">Prerequisites</span></span>

<span data-ttu-id="5cbdd-189">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-189">Install the following:</span></span>
* <span data-ttu-id="5cbdd-190">[webpack horkou middleware](https://www.npmjs.com/package/webpack-hot-middleware) balíčku npm:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-190">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="5cbdd-191">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-191">Configuration</span></span>

<span data-ttu-id="5cbdd-192">Součást HMR musí zaregistrovat do kanálu požadavku HTTP MVC v `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-192">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="5cbdd-193">Stejně jako se platí [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` před musí být volána metoda rozšíření `UseStaticFiles` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-193">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="5cbdd-194">Z bezpečnostních důvodů se zaregistrujte middleware jenom v případě, že aplikace běží v režimu pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-194">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="5cbdd-195">*Webpack.config.js* musí definovat souboru `plugins` pole, i když je ponechán prázdný:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-195">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="5cbdd-196">Po načtení aplikace v prohlížeči, karta konzoly nástroje developer tools poskytuje potvrzení HMR aktivace:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-196">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Aktivní modulu nahrazení připojené zpráv](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="5cbdd-198">Směrování pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="5cbdd-198">Routing helpers</span></span>

<span data-ttu-id="5cbdd-199">Ve většině SPA založené na ASP.NET Core budete muset klienta směrování kromě směrování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-199">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="5cbdd-200">Systémy směrování SPA a MVC může nezávisle pracovat bez narušení.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-200">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="5cbdd-201">Existuje, ale jeden okraj případu autority vyzve: Identifikace odpovědi HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-201">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="5cbdd-202">Představte si situaci, ve kterém bez přípony trasa `/some/page` se používá.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-202">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="5cbdd-203">Předpokládejme, požadavek nebude vzor match trasy na straně serveru, ale jeho vzor odpovídat trasy na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-203">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="5cbdd-204">Teď se podíváme příchozí žádosti pro `/images/user-512.png`, které se obecně očekává najít soubor obrázku na serveru.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-204">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="5cbdd-205">Pokud této cestě požadovaný prostředek se neshoduje se všechny serverové trasy nebo statický soubor, není pravděpodobné, že aplikace na straně klienta by zpracovat – obecně chcete vrátit 404 stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-205">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5cbdd-206">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cbdd-206">Prerequisites</span></span>

<span data-ttu-id="5cbdd-207">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-207">Install the following:</span></span>
* <span data-ttu-id="5cbdd-208">Balíček klienta směrování npm.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-208">The client-side routing npm package.</span></span> <span data-ttu-id="5cbdd-209">Jako příklad použijeme úhlová:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-209">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="5cbdd-210">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-210">Configuration</span></span>

<span data-ttu-id="5cbdd-211">Metody rozšíření s názvem `MapSpaFallbackRoute` je používán `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-211">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="5cbdd-212">Tip: Trasy jsou vyhodnocovány v pořadí, ve kterém máte nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-212">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="5cbdd-213">V důsledku toho `default` trasy v předchozím příkladu kódu je použita jako první porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-213">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="5cbdd-214">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="5cbdd-214">Creating a new project</span></span>

<span data-ttu-id="5cbdd-215">JavaScriptServices poskytuje šablony předem nakonfigurovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-215">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="5cbdd-216">SpaServices se používá v těchto šablon ve spojení s jinou architektury a knihovny, například úhlová, Aurelia, Knockout, reagují a Vue.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-216">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="5cbdd-217">Tyto šablony lze nainstalovat prostřednictvím rozhraní příkazového řádku .NET Core spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-217">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="5cbdd-218">Zobrazí se seznam dostupných šablon SPA:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-218">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="5cbdd-219">Šablony</span><span class="sxs-lookup"><span data-stu-id="5cbdd-219">Templates</span></span>                                 | <span data-ttu-id="5cbdd-220">Krátký název</span><span class="sxs-lookup"><span data-stu-id="5cbdd-220">Short Name</span></span> | <span data-ttu-id="5cbdd-221">Jazyk</span><span class="sxs-lookup"><span data-stu-id="5cbdd-221">Language</span></span> | <span data-ttu-id="5cbdd-222">Značky</span><span class="sxs-lookup"><span data-stu-id="5cbdd-222">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="5cbdd-223">Jádro ASP.NET MVC s úhlová</span><span class="sxs-lookup"><span data-stu-id="5cbdd-223">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="5cbdd-224">úhlová</span><span class="sxs-lookup"><span data-stu-id="5cbdd-224">angular</span></span>    | <span data-ttu-id="5cbdd-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="5cbdd-225">[C#]</span></span>     | <span data-ttu-id="5cbdd-226">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5cbdd-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5cbdd-227">Jádro ASP.NET MVC s Aurelia</span><span class="sxs-lookup"><span data-stu-id="5cbdd-227">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="5cbdd-228">aurelia</span><span class="sxs-lookup"><span data-stu-id="5cbdd-228">aurelia</span></span>    | <span data-ttu-id="5cbdd-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="5cbdd-229">[C#]</span></span>     | <span data-ttu-id="5cbdd-230">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5cbdd-230">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5cbdd-231">Jádro ASP.NET MVC s Knockout.js</span><span class="sxs-lookup"><span data-stu-id="5cbdd-231">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="5cbdd-232">Knockout</span><span class="sxs-lookup"><span data-stu-id="5cbdd-232">knockout</span></span>   | <span data-ttu-id="5cbdd-233">[C#]</span><span class="sxs-lookup"><span data-stu-id="5cbdd-233">[C#]</span></span>     | <span data-ttu-id="5cbdd-234">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5cbdd-234">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5cbdd-235">Jádro ASP.NET MVC s React.js</span><span class="sxs-lookup"><span data-stu-id="5cbdd-235">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="5cbdd-236">Reagovat</span><span class="sxs-lookup"><span data-stu-id="5cbdd-236">react</span></span>      | <span data-ttu-id="5cbdd-237">[C#]</span><span class="sxs-lookup"><span data-stu-id="5cbdd-237">[C#]</span></span>     | <span data-ttu-id="5cbdd-238">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5cbdd-238">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5cbdd-239">Jádro ASP.NET MVC s React.js a – obnovení</span><span class="sxs-lookup"><span data-stu-id="5cbdd-239">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="5cbdd-240">reactredux</span><span class="sxs-lookup"><span data-stu-id="5cbdd-240">reactredux</span></span> | <span data-ttu-id="5cbdd-241">[C#]</span><span class="sxs-lookup"><span data-stu-id="5cbdd-241">[C#]</span></span>     | <span data-ttu-id="5cbdd-242">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5cbdd-242">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5cbdd-243">Jádro ASP.NET MVC s Vue.js</span><span class="sxs-lookup"><span data-stu-id="5cbdd-243">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="5cbdd-244">VUE</span><span class="sxs-lookup"><span data-stu-id="5cbdd-244">vue</span></span>        | <span data-ttu-id="5cbdd-245">[C#]</span><span class="sxs-lookup"><span data-stu-id="5cbdd-245">[C#]</span></span>     | <span data-ttu-id="5cbdd-246">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5cbdd-246">Web/MVC/SPA</span></span> | 

<span data-ttu-id="5cbdd-247">Pokud chcete vytvořit nový projekt pomocí jedné z šablon SPA, obsahovat **krátký název** šablony v [dotnet nové](/dotnet/core/tools/dotnet-new) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-247">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="5cbdd-248">Následující příkaz vytvoří úhlová aplikace s ASP.NET MVC základní nakonfigurovaná na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-248">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="5cbdd-249">Nastavit režim konfigurace modulu runtime</span><span class="sxs-lookup"><span data-stu-id="5cbdd-249">Set the runtime configuration mode</span></span>

<span data-ttu-id="5cbdd-250">Existují dva režimy primární runtime konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-250">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="5cbdd-251">**Vývoj**:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-251">**Development**:</span></span>
    * <span data-ttu-id="5cbdd-252">Obsahuje mapování zdroje k usnadnění ladění.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-252">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="5cbdd-253">Není optimalizovat kód klienta pro výkon.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-253">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="5cbdd-254">**Produkční**:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-254">**Production**:</span></span>
    * <span data-ttu-id="5cbdd-255">Vyloučí zdroj mapy.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-255">Excludes source maps.</span></span>
    * <span data-ttu-id="5cbdd-256">Optimalizuje kód klienta prostřednictvím sdružování a minimalizace.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-256">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="5cbdd-257">ASP.NET Core používá proměnnou prostředí s názvem `ASPNETCORE_ENVIRONMENT` k uložení konfigurace režimu.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-257">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="5cbdd-258">V tématu  **[nastavení prostředí](xref:fundamentals/environments#setting-the-environment)**  Další informace.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-258">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="5cbdd-259">Spuštění s .NET Core rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5cbdd-259">Running with .NET Core CLI</span></span>

<span data-ttu-id="5cbdd-260">Spuštěním následujícího příkazu v kořenovém adresáři projektu obnovte požadované NuGet a npm balíčky:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-260">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="5cbdd-261">Sestavení a spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-261">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="5cbdd-262">Spuštění aplikace na místním hostiteli podle požadavků [režim konfigurace runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="5cbdd-262">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="5cbdd-263">Přejdete na `http://localhost:5000` v prohlížeči zobrazí cílová stránka.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-263">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="5cbdd-264">Běh Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5cbdd-264">Running with Visual Studio 2017</span></span>

<span data-ttu-id="5cbdd-265">Otevřete *.csproj* souboru vygenerované [dotnet nové](/dotnet/core/tools/dotnet-new) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-265">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="5cbdd-266">Požadované balíčky NuGet a npm se automaticky obnoví po projektem otevřeným.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-266">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="5cbdd-267">Tento proces obnovení může trvat několik minut a aplikace je připraven ke spuštění po dokončení.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-267">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="5cbdd-268">Kliknutím na zelené tlačítko spustit nebo klikněte na tlačítko `Ctrl + F5`, a prohlížeči se otevře na cílovou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-268">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="5cbdd-269">Spuštění aplikace na místním hostiteli podle požadavků [režimu runtime konfigurace](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="5cbdd-269">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="5cbdd-270">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-270">Testing the app</span></span>

<span data-ttu-id="5cbdd-271">SpaServices šablony jsou předem nakonfigurované ke spuštění testů na straně klienta pomocí [Karma](https://karma-runner.github.io/1.0/index.html) a [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="5cbdd-271">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="5cbdd-272">Jasmine je Oblíbené jednotku testování framework pro JavaScript, zatímco Karma je testu pro tyto testy.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-272">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="5cbdd-273">Karma je nakonfigurováno pro práci s [Webpack Dev Middleware](#webpack-dev-middleware) tak, že vývojář není vyžadován k zastavení a spuštění testu pokaždé, když jsou provedeny změny.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-273">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="5cbdd-274">Jestli je kód spuštěn před testovacího případu nebo testovací případ, samotné, test se spouští automaticky.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-274">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="5cbdd-275">Jako příklad použijeme úhlová aplikace, dvě Jasmine testovacích případů již pro jsou k dispozici `CounterComponent` v *counter.component.spec.ts* souboru:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-275">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="5cbdd-276">Otevřete příkazový řádek v kořenovém adresáři projektu a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-276">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="5cbdd-277">Skript spustí nástroj test runner Karma, který čte definované v nastavení *karma.conf.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-277">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="5cbdd-278">Kromě jiných nastavení *karma.conf.js* identifikuje testovacích souborů, které mají být provedeny prostřednictvím jeho `files` pole:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-278">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="5cbdd-279">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-279">Publishing the application</span></span>

<span data-ttu-id="5cbdd-280">Kombinování generovaného klienta prostředky a publikovaná ASP.NET Core artefakty do balíčku připravená k nasazení může být náročná.</span><span class="sxs-lookup"><span data-stu-id="5cbdd-280">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="5cbdd-281">Naštěstí SpaServices orchestruje procesu celé publikace s vlastní MSBuild cíl s názvem `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-281">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="5cbdd-282">Cíl MSBuild má následující zodpovědnosti:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-282">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="5cbdd-283">Obnovení balíčků npm</span><span class="sxs-lookup"><span data-stu-id="5cbdd-283">Restore the npm packages</span></span>
1. <span data-ttu-id="5cbdd-284">Vytvořit sestavení produkční úrovni prostředků třetích stran, na straně klienta</span><span class="sxs-lookup"><span data-stu-id="5cbdd-284">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="5cbdd-285">Vytvořit produkční úrovni sestavení vlastní prostředků klienta</span><span class="sxs-lookup"><span data-stu-id="5cbdd-285">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="5cbdd-286">Kopírování generované Webpack prostředky do složky publikování</span><span class="sxs-lookup"><span data-stu-id="5cbdd-286">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="5cbdd-287">Cíle MSBuild je volána při spuštění:</span><span class="sxs-lookup"><span data-stu-id="5cbdd-287">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="5cbdd-288">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5cbdd-288">Additional resources</span></span>

* [<span data-ttu-id="5cbdd-289">Úhlová dokumentace</span><span class="sxs-lookup"><span data-stu-id="5cbdd-289">Angular Docs</span></span>](https://angular.io/docs)
