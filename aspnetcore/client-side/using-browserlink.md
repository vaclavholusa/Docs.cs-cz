---
title: Browser Link v ASP.NET Core
author: ncarandini
description: "Vysvětluje, jak Browser Link je funkce sady Visual Studio, který odkazuje vývojového prostředí s jeden nebo více webových prohlížečů."
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5db65c268923e96c45b034639437fc3496ccac1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="browser-link-in-aspnet-core"></a>Browser Link v ASP.NET Core 

Podle [Nicolò Carandini](https://github.com/ncarandini), [Karel Wasson](https://github.com/MikeWasson), a [tní Dykstra](https://github.com/tdykstra)

Browser Link je funkce v sadě Visual Studio, který vytváří komunikační kanál mezi vývojového prostředí a jeden nebo více webových prohlížečů. Můžete použít Browser Link aktualizujte svoji webovou aplikaci v několika prohlížeče najednou, což je užitečné pro testování různých prohlížečích.

## <a name="browser-link-setup"></a>Instalační program odkaz prohlížeče

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x **webové aplikace**, **prázdný**, a **webového rozhraní API** šablony projektů, použití [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Meta balíček, který obsahuje odkaz na balíček pro [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Proto pomocí `Microsoft.AspNetCore.All` metabalíček vyžadována žádná další akce, chcete-li k dispozici pro použití odkazů prohlížeče.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x **webové aplikace** šablona projektu obsahuje odkaz na balíček pro [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) balíčku. **Prázdný** nebo **webového rozhraní API** šablony projektů vyžadují, abyste přidat odkaz na balíček `Microsoft.VisualStudio.Web.BrowserLink`.

Vzhledem k tomu, že toto je funkce sady Visual Studio, nejjednodušší způsob přidání balíčku, který má **prázdný** nebo **webového rozhraní API** projektu šablony je otevřít **Konzola správce balíčků** (**Zobrazení** > **ostatní okna** > **Konzola správce balíčků**) a spusťte následující příkaz:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternativně můžete použít **Správce balíčků NuGet**. Klikněte pravým tlačítkem myši na název projektu v **Průzkumníku řešení** a zvolte **spravovat balíčky NuGet**:

![Správce balíčků NuGet otevřít](using-browserlink/_static/open-nuget-package-manager.png)

Najít a nainstalovat balíček:

![Přidejte balíček s Správce balíčků NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Konfigurace

V `Configure` metodu *Startup.cs* souboru:

```csharp
app.UseBrowserLink();
```

Obvykle je kód uvnitř `if` blok, který umožňuje pouze Browser Link ve vývojovém prostředí, jak je vidět tady:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Postup použití Browser Link

Když máte projekt ASP.NET Core otevřete, Visual Studio zobrazí ovládacím prvkem panel nástrojů Browser Link vedle **cíl ladění** prvku panel nástrojů:

![Prohlížeč odkaz rozevírací nabídky](using-browserlink/_static/browserLink-dropdown-menu.png)

Z ovládacího prvku panel nástrojů Browser Link můžete:

* Aktualizace webové aplikace v prohlížečích několik najednou.
* Otevřete **řídicím odkazů prohlížeče**.
* Povolit nebo zakázat **Browser Link**. Poznámka: Ve výchozím nastavení v aplikaci Visual Studio 2017 (15.3) vypnutá odkazů prohlížeče.
* Povolit nebo zakázat [Automatická synchronizace šablon stylů CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Některé sady Visual Studio moduly plug-in, zejména *webové rozšíření Pack 2015* a *2017 Pack rozšíření webové*, nabízí rozšířené funkce pro Browser Link, ale některé další funkce nefungují s prostředím ASP. Projekty Asp.net Core.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Aktualizace webové aplikace v prohlížečích několik najednou

Zvolte jeden webový prohlížeč, aby spuštění při spuštění projektu, použijte rozevírací nabídky v **cíl ladění** prvku panel nástrojů:

![F5 rozevírací nabídky](using-browserlink/_static/debug-target-dropdown-menu.png)

Otevřete více prohlížečů najednou, zvolte **procházet s...**  ze stejné rozevíracího seznamu. Podržte stisknutou klávesu CTRL k výběru prohlížeče, který chcete a pak klikněte na **Procházet**:

![Otevřete najednou mnoha prohlížečů](using-browserlink/_static/open-many-browsers-at-once.png)

Zde je snímek obrazovky zobrazující Visual Studio s zobrazení indexu otevřete a otevřete prohlížečů:

![Synchronizovat s příklad prohlížečů](using-browserlink/_static/sync-with-two-browsers-example.png)

Najeďte myší na ovládací prvek Browser Link panelu nástrojů zobrazíte prohlížečů, které jsou připojené do projektu:

![Najeďte tipu](using-browserlink/_static/hoover-tip.png)

Změnit zobrazení indexu a všechny připojené prohlížeče aktualizují po kliknutí na tlačítko Aktualizovat Browser Link:

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

Browser Link funguje taky s prohlížeče, které spusťte mimo aplikaci Visual Studio a přejděte na adresu URL aplikace.

### <a name="the-browser-link-dashboard"></a>Řídicím panelu odkazů prohlížeče

Z rozevírací nabídky ke správě připojení pomocí prohlížeče otevřete Browser Link otevřete řídicím panelu odkazů prohlížeče:

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Pokud je připojený žádný prohlížeč, můžete spustit relaci – ladění výběrem *zobrazit v prohlížeči* odkaz:

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Jinak hodnota cesta ke stránce, který se zobrazuje každým prohlížečem, který zobrazuje připojené prohlížeče:

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Pokud chcete, můžete kliknutím na název uvedené prohlížeče aktualizujte tohoto jednoho prohlížeče.

### <a name="enable-or-disable-browser-link"></a>Povolit nebo zakázat Browser Link

Pokud znovu povolíte Browser Link po jejím zakázáním, je nutné aktualizovat prohlížeče se je znovu připojit.

### <a name="enable-or-disable-css-auto-sync"></a>Povolit nebo zakázat automatickou synchronizaci šablon stylů CSS

Pokud je povolena automatická synchronizace šablon stylů CSS, připojené prohlížeče se automaticky aktualizují při provést změnu souborů CSS.

## <a name="how-does-it-work"></a>Jak to funguje?

Browser Link používá k vytvoření komunikační kanál mezi Visual Studio a prohlížeči SignalR. Pokud je povoleno Browser Link, Visual Studio funguje jako server SignalR, více klientů (prohlížeče) mohou připojit. Browser Link také zaregistruje komponentu middleware v kanálu požadavku ASP.NET. Tato součást Vloží speciální `<script>` odkazy na každý požadavek na stránku ze serveru. Zobrazí odkazům na skript tak, že vyberete **zobrazit zdroj** v prohlížeči a posouvání na konec `<body>` označení obsahu:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Zdrojové soubory vašeho se nemění. Komponenta middlewaru vloží odkazům na skript dynamicky. 

Protože kód prohlížeče na straně je všechny JavaScript, funguje u všech prohlížečů, které podporuje SignalR bez nutnosti modul plug-in prohlížeče.
