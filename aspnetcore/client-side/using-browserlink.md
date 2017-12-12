---
title: Browser Link v ASP.NET Core
author: ncarandini
description: "Vysvětluje, jak Browser Link je funkce sady Visual Studio, který odkazuje vývojového prostředí s jeden nebo více webových prohlížečů."
keywords: "ASP.NET Core, browser link a synchronizace šablon stylů CSS"
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b69d085e8bee4cdac2dff08b46a95a8869e263b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="1649b-104">Browser Link v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1649b-104">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="1649b-105">Podle [Nicolò Carandini](https://github.com/ncarandini), [Karel Wasson](https://github.com/MikeWasson), a [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1649b-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="1649b-106">Browser Link je funkce v sadě Visual Studio, který vytváří komunikační kanál mezi vývojového prostředí a jeden nebo více webových prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="1649b-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="1649b-107">Můžete použít Browser Link aktualizujte svoji webovou aplikaci v několika prohlížeče najednou, což je užitečné pro testování různých prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="1649b-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="1649b-108">Instalační program odkaz prohlížeče</span><span class="sxs-lookup"><span data-stu-id="1649b-108">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1649b-109">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1649b-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1649b-110">ASP.NET Core 2.x **webové aplikace**, **prázdný**, a **webového rozhraní API** šablony projektů, použití [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Meta balíček, který obsahuje odkaz na balíček pro [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="1649b-110">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="1649b-111">Proto pomocí `Microsoft.AspNetCore.All` metabalíček vyžadována žádná další akce, chcete-li k dispozici pro použití odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1649b-111">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1649b-112">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1649b-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1649b-113">ASP.NET Core 1.x **webové aplikace** šablona projektu obsahuje odkaz na balíček pro [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="1649b-113">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="1649b-114">**Prázdný** nebo **webového rozhraní API** šablony projektů vyžadují, abyste přidat odkaz na balíček `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="1649b-114">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="1649b-115">Vzhledem k tomu, že toto je funkce sady Visual Studio, nejjednodušší způsob přidání balíčku, který má **prázdný** nebo **webového rozhraní API** projektu šablony je otevřít **Konzola správce balíčků** (**Zobrazení** > **ostatní okna** > **Konzola správce balíčků**) a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1649b-115">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="1649b-116">Alternativně můžete použít **Správce balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1649b-116">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="1649b-117">Klikněte pravým tlačítkem myši na název projektu v **Průzkumníku řešení** a zvolte **spravovat balíčky NuGet**:</span><span class="sxs-lookup"><span data-stu-id="1649b-117">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Správce balíčků NuGet otevřít](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="1649b-119">Najít a nainstalovat balíček:</span><span class="sxs-lookup"><span data-stu-id="1649b-119">Find and install the package:</span></span>

![Přidejte balíček s Správce balíčků NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="1649b-121">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="1649b-121">Configuration</span></span>

<span data-ttu-id="1649b-122">V `Configure` metodu *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="1649b-122">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="1649b-123">Obvykle je kód uvnitř `if` blok, který umožňuje pouze Browser Link ve vývojovém prostředí, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="1649b-123">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="1649b-124">Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1649b-124">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="1649b-125">Postup použití Browser Link</span><span class="sxs-lookup"><span data-stu-id="1649b-125">How to use Browser Link</span></span>

<span data-ttu-id="1649b-126">Když máte projekt ASP.NET Core otevřete, Visual Studio zobrazí ovládacím prvkem panel nástrojů Browser Link vedle **cíl ladění** prvku panel nástrojů:</span><span class="sxs-lookup"><span data-stu-id="1649b-126">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Prohlížeč odkaz rozevírací nabídky](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="1649b-128">Z ovládacího prvku panel nástrojů Browser Link můžete:</span><span class="sxs-lookup"><span data-stu-id="1649b-128">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="1649b-129">Aktualizace webové aplikace v prohlížečích několik najednou.</span><span class="sxs-lookup"><span data-stu-id="1649b-129">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="1649b-130">Otevřete **řídicím odkazů prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="1649b-130">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="1649b-131">Povolit nebo zakázat **Browser Link**.</span><span class="sxs-lookup"><span data-stu-id="1649b-131">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="1649b-132">Poznámka: Ve výchozím nastavení v aplikaci Visual Studio 2017 (15.3) vypnutá odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1649b-132">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="1649b-133">Povolit nebo zakázat [Automatická synchronizace šablon stylů CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="1649b-133">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="1649b-134">Některé sady Visual Studio moduly plug-in, zejména *webové rozšíření Pack 2015* a *2017 Pack rozšíření webové*, nabízí rozšířené funkce pro Browser Link, ale některé další funkce nefungují s prostředím ASP. Projekty Asp.net Core.</span><span class="sxs-lookup"><span data-stu-id="1649b-134">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="1649b-135">Aktualizace webové aplikace v prohlížečích několik najednou</span><span class="sxs-lookup"><span data-stu-id="1649b-135">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="1649b-136">Zvolte jeden webový prohlížeč, aby spuštění při spuštění projektu, použijte rozevírací nabídky v **cíl ladění** prvku panel nástrojů:</span><span class="sxs-lookup"><span data-stu-id="1649b-136">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 rozevírací nabídky](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="1649b-138">Otevřete více prohlížečů najednou, zvolte **procházet s...**  ze stejné rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1649b-138">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="1649b-139">Podržte stisknutou klávesu CTRL k výběru prohlížeče, který chcete a pak klikněte na **Procházet**:</span><span class="sxs-lookup"><span data-stu-id="1649b-139">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Otevřete najednou mnoha prohlížečů](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="1649b-141">Zde je snímek obrazovky zobrazující Visual Studio s zobrazení indexu otevřete a otevřete prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="1649b-141">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Synchronizovat s příklad prohlížečů](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="1649b-143">Najeďte myší na ovládací prvek Browser Link panelu nástrojů zobrazíte prohlížečů, které jsou připojené do projektu:</span><span class="sxs-lookup"><span data-stu-id="1649b-143">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Najeďte tipu](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="1649b-145">Změnit zobrazení indexu a všechny připojené prohlížeče aktualizují po kliknutí na tlačítko Aktualizovat Browser Link:</span><span class="sxs-lookup"><span data-stu-id="1649b-145">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![prohlížeče synchronizace pro změny](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="1649b-147">Browser Link funguje taky s prohlížeče, které spusťte mimo aplikaci Visual Studio a přejděte na adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="1649b-147">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="1649b-148">Řídicím panelu odkazů prohlížeče</span><span class="sxs-lookup"><span data-stu-id="1649b-148">The Browser Link Dashboard</span></span>

<span data-ttu-id="1649b-149">Z rozevírací nabídky ke správě připojení pomocí prohlížeče otevřete Browser Link otevřete řídicím panelu odkazů prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="1649b-149">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Otevřete browserslink-řídicí panel](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="1649b-151">Pokud je připojený žádný prohlížeč, můžete spustit relaci – ladění výběrem *zobrazit v prohlížeči* odkaz:</span><span class="sxs-lookup"><span data-stu-id="1649b-151">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink řídicí panel žádné připojení](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="1649b-153">Jinak hodnota cesta ke stránce, který se zobrazuje každým prohlížečem, který zobrazuje připojené prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="1649b-153">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink řídicí panel dvě připojení](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="1649b-155">Pokud chcete, můžete kliknutím na název uvedené prohlížeče aktualizujte tohoto jednoho prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1649b-155">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="1649b-156">Povolit nebo zakázat Browser Link</span><span class="sxs-lookup"><span data-stu-id="1649b-156">Enable or disable Browser Link</span></span>

<span data-ttu-id="1649b-157">Pokud znovu povolíte Browser Link po jejím zakázáním, je nutné aktualizovat prohlížeče se je znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="1649b-157">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="1649b-158">Povolit nebo zakázat automatickou synchronizaci šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="1649b-158">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="1649b-159">Pokud je povolena automatická synchronizace šablon stylů CSS, připojené prohlížeče se automaticky aktualizují při provést změnu souborů CSS.</span><span class="sxs-lookup"><span data-stu-id="1649b-159">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="1649b-160">Jak to funguje?</span><span class="sxs-lookup"><span data-stu-id="1649b-160">How does it work?</span></span>

<span data-ttu-id="1649b-161">Browser Link používá k vytvoření komunikační kanál mezi Visual Studio a prohlížeči SignalR.</span><span class="sxs-lookup"><span data-stu-id="1649b-161">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="1649b-162">Pokud je povoleno Browser Link, Visual Studio funguje jako server SignalR, více klientů (prohlížeče) mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="1649b-162">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="1649b-163">Browser Link také zaregistruje komponentu middleware v kanálu požadavku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1649b-163">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="1649b-164">Tato součást Vloží speciální `<script>` odkazy na každý požadavek na stránku ze serveru.</span><span class="sxs-lookup"><span data-stu-id="1649b-164">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="1649b-165">Zobrazí odkazům na skript tak, že vyberete **zobrazit zdroj** v prohlížeči a posouvání na konec `<body>` označení obsahu:</span><span class="sxs-lookup"><span data-stu-id="1649b-165">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="1649b-166">Zdrojové soubory vašeho se nemění.</span><span class="sxs-lookup"><span data-stu-id="1649b-166">Your source files aren't modified.</span></span> <span data-ttu-id="1649b-167">Komponenta middlewaru vloží odkazům na skript dynamicky.</span><span class="sxs-lookup"><span data-stu-id="1649b-167">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="1649b-168">Protože kód prohlížeče na straně je všechny JavaScript, funguje u všech prohlížečů, které podporuje SignalR bez nutnosti modul plug-in prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1649b-168">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
