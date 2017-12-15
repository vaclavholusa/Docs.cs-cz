---
title: "Použijte šablonu úhlová projektu"
author: SteveSandersonMS
description: "Zjistěte, jak začít pracovat se šablonou projektu ASP.NET Core jednostránkové aplikace (SPA) preview pro úhlová a úhlová příkazového řádku."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: de7b32099f225e838ec40ce4f04bf67bffb9c260
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="use-the-angular-project-template-preview"></a>Použijte šablonu úhlová projektu (preview)

> [!NOTE]
> Tato dokumentace není o vydaných úhlová projektu šablony. **Tato dokumentace je o verze preview úhlová šablony.** Věříme, že pro odeslání v časná 2018 vydaná verze.

Šablona aktualizované úhlová projektu poskytuje příhodný výchozí bod pro ASP.NET Core použití úhlová 5 a úhlová příkazového řádku k implementaci bohatou a klientské uživatelské rozhraní (UI) aplikace.

Šablona je ekvivalentní k vytvoření projektu ASP.NET Core tak, aby fungoval jako back-end rozhraní API a projekt úhlová CLI tak, aby fungoval jako uživatelského rozhraní. Šablona nabízí praktické hostování oba typy projektů v jediné aplikaci projektu. V důsledku toho projekt aplikace můžete vytvořená a publikovaná jako na jednu jednotku.

## <a name="create-a-new-app"></a>Vytvoření nové aplikace

Abyste mohli začít, ujistěte se, když jste [nainstalován v šabloně projektů aktualizované úhlová](xref:spa/index#installation). Tyto pokyny se nevztahují na předchozí šablona úhlová projektu součástí .NET Core 2.0.x SDK.

Vytvoření nového projektu z příkazového řádku pomocí příkazu `dotnet new angular` v prázdného adresáře. Můžete například vytvořit následující příkazy v aplikaci *-nové aplikace my* adresáře a přepnete se do této složky:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Spusťte aplikaci v sadě Visual Studio nebo .NET Core rozhraní příkazového řádku:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otevřete vygenerovaného *.csproj* souboru a spuštění aplikace jako normální odtud.

Proces sestavení obnoví npm závislosti během prvního spuštění, což může trvat několik minut. Následující sestavení je mnohem rychlejší.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Zajistěte, abyste měli proměnné prostředí s názvem `ASPNETCORE_Environment` s hodnotou `Development`. V systému Windows (v výzvy – prostředí PowerShell), spusťte `SET ASPNETCORE_Environment=Development`. V systému macOS nebo Linux, spusťte `export ASPNETCORE_Environment=Development`.

Spustit `dotnet build` k ověření aplikace sestavení správně. Při prvním spuštění procesu sestavení obnoví npm závislosti, které může trvat několik minut. Následující sestavení je mnohem rychlejší.

Spustit `dotnet run` a spusťte aplikaci. Zaznamená se zpráva podobná následující:

```console
Now listening on: http://localhost:<port>
```

Přejděte na tuto adresu URL v prohlížeči.

Spuštění aplikace instanci serveru úhlová rozhraní příkazového řádku na pozadí. Je zaznamenána zpráva podobná následující: *NG Live vývojový Server naslouchá na localhost:&lt;otherport&gt;, otevřete prohlížeč na http://localhost:&lt;otherport&gt; /* . Tuto zprávu ignorovat&mdash;má **není** adresu URL pro kombinované aplikace ASP.NET Core a úhlová příkazového řádku.

---

Šablona projektu vytvoří aplikace ASP.NET Core a úhlová aplikace. Aplikace ASP.NET Core se má použít pro přístup k datům, autorizaci a další otázky straně serveru. Úhlová aplikace, které se nacházejí v *ClientApp* podadresáři, je určena k použití pro všechny otázky uživatelského rozhraní.

## <a name="add-pages-images-styles-modules-etc"></a>Přidání stránky, obrázky, styly, moduly, atd.

*ClientApp* adresář obsahuje standardní aplikace úhlová rozhraní příkazového řádku. Najdete v oficiální [úhlová dokumentaci](https://github.com/angular/angular-cli/wiki) Další informace.

Jsou drobné rozdíly mezi úhlová aplikace vytvořené pomocí této šablony a jeden vytvořené úhlová rozhraní příkazového řádku, samotné (prostřednictvím `ng new`), nicméně schopnosti aplikace jsou stejné. Obsahuje aplikace vytvořené pomocí šablony [Bootstrap](https://getbootstrap.com/)– na základě rozložení a základní příklad směrování.

## <a name="run-ng-commands"></a>Spusťte příkazy ng

V příkazovém řádku přepnout *ClientApp* podadresáři:

```console
cd ClientApp
```

Pokud máte `ng` globálně nainstalovaný nástroj, můžete spustit některý z jeho příkazů. Například můžete spustit `ng lint`, `ng test`, ani v žádné z dalších [úhlová rozhraní příkazového řádku](https://github.com/angular/angular-cli/wiki#additional-commands). Není nutné ke spuštění `ng serve` ale vzhledem k tomu, že vaše aplikace ASP.NET Core se zabývá obsluhující částí serverové a klientské aplikace. Interně používá `ng serve` ve vývoji.

Pokud nemáte `ng` nástroj nainstalovali, spusťte `npm run ng` místo. Například můžete spustit `npm run ng lint` nebo `npm run ng test`.

## <a name="install-npm-packages"></a>Instalovat balíčky npm

Chcete-li instalovat balíčky jiných výrobců npm, použijte na příkazovém řádku ve *ClientApp* podadresáři. Příklad:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikování a nasazení

Ve vývoji aplikace běží v režimu optimalizovaná pro vývojáře pohodlí. Například JavaScript sady zahrnují zdroj mapy (tak, aby při ladění, zobrazí se původní kód TypeScript). Aplikace sleduje TypeScript, HTML a CSS změny na disku a automaticky znovu zkompiluje a znovu načte, jakmile je detekována tyto soubory změnit.

V produkčním prostředí sloužit verzi vaší aplikace, která je optimalizovaná pro výkon. Ta se nakonfiguruje automaticky provést. Když publikujete, je konfigurace sestavení vysílá minifikovaný, napřed předčasné (AoT) zkompilovat sestavení kódu na straně klienta. Na rozdíl od sestavení vývoj nevyžaduje produkční build Node.js k instalaci na serveru (Pokud jste povolili [dokončení fáze před vykreslením serverové](#server-side-rendering)).

Můžete použít standardní [ASP.NET Core publikování a metody nasazení](xref:publishing/index).

## <a name="run-ng-serve-independently"></a>Spustit nezávisle "ng používat"

Projekt je nakonfigurován na spuštění svou vlastní instanci serveru úhlová rozhraní příkazového řádku na pozadí při spuštění v režimu pro vývoj aplikace ASP.NET Core. To je vhodné, protože nemusíte ručně spustit samostatný server.

Není nevýhodou toto výchozí nastavení. Pokaždé, když upravíte kódu C# a vaše ASP.NET Core aplikace potřebuje k restartování, restartování serveru úhlová rozhraní příkazového řádku. Přibližně 10 sekund, je potřeba spustit zálohování. Pokud provádíme za časté úpravy kódu jazyka C# a nechcete čekat na úhlová rozhraní příkazového řádku pro restartování, spusťte server úhlová CLI externě, nezávisle na procesu ASP.NET Core. Postupujte následovně:

1. V příkazovém řádku přepnout *ClientApp* podadresáři a spouštět vývojový server úhlová rozhraní příkazového řádku:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Použití `npm start` spustíte vývojový server úhlová rozhraní příkazového řádku, není `ng serve`tak, aby konfigurace v *package.json* je dodržena. Chcete-li předat další parametry úhlová rozhraní příkazového řádku serveru, přidejte ho do příslušné `scripts` řádek ve vaší *package.json* souboru.

2. Upravte v aplikaci ASP.NET Core použít externí instanci úhlová rozhraní příkazového řádku místo spuštění jeho vlastní. Ve vaší *spuštění* třídy, nahraďte `spa.UseAngularCliServer` volání následujícím kódem:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Při spuštění aplikace ASP.NET Core nespustí serveru úhlová rozhraní příkazového řádku. Instance, kterou jste spustili ručně bude místo něj použita. To umožňuje spustit a restartovat rychlejší. Se už čeká úhlová rozhraní příkazového řádku pro pokaždé, když znovu sestavit vaší klientské aplikace.

## <a name="server-side-rendering"></a>Vykreslování na straně serveru

Jako funkce výkonu můžete předem vykreslení úhlová aplikace na serveru, jakož i běžící na klientech. To znamená, že prohlížeč obdrží značka jazyka HTML představující počáteční rozhraní vaší aplikace, takže se zobrazení ani před stáhl a spustil vaší sady JavaScript. Většina implementace tohoto pochází z úhlová funkci [úhlová univerzální](https://universal.angular.io/).

> [!TIP]
> Povolení vykreslování na straně serveru (SSR) představuje počet další komplikace jak během vývoje a nasazení. Čtení [nevýhody SSR](#drawbacks-of-ssr) lze zjistit SSR je vhodné pro vaše požadavky.

Chcete-li povolit SSR, zpřístupněte počet dodatky do projektu.

V *spuštění* třídy, *po* řádek, který konfiguruje `spa.Options.SourcePath`, a *před* volání `UseAngularCliServer` nebo `UseProxyToSpaDevelopmentServer`, přidejte následující:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

V režimu pro vývoj, tento kód pokusí vytvořit sadu SSR spuštěním skriptu `build:ssr`, která je definována v *ClientApp\package.json*. Toto sestavení úhlová aplikaci s názvem `ssr`, který ještě není definován. 

Na konci `apps` pole v *ClientApp/.angular-cli.json*, definovat další aplikaci s názvem `ssr`. Pomocí následujících možností:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Tato nová konfigurace aplikace s podporou SSR vyžaduje dva další soubory: *tsconfig.server.json* a *main.server.ts*. *Tsconfig.server.json* souboru Určuje možnosti kompilaci TypeScript. *Main.server.ts* soubor slouží jako vstupní bod kódu během SSR.

Přidat nový soubor s názvem *tsconfig.server.json* uvnitř *ClientApp/src* (společně se stávající *tsconfig.app.json*), který obsahuje následující:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Tento soubor nakonfiguruje na úhlová AoT kompilátoru hledání modul s názvem `app.server.module`. Přidejte tuto tak, že vytvoříte nový soubor v *ClientApp/src/app/app.server.module.ts* (společně se stávající *app.module.ts*) obsahující následující: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Tento modul dědí z vašeho klienta `app.module` a definuje, které velmi úhlová moduly jsou k dispozici během SSR.

Odvolat, nové `ssr` položku v *.angular cli.json* odkazovaný soubor vstupní bod s názvem *main.server.ts*. Ještě nepřidali tento soubor a nyní je čas Uděláte to tak. Vytvoření nového souboru na *ClientApp/src/main.server.ts* (společně se stávající *main.ts*), který obsahuje následující:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Tento soubor kód je co ASP.NET Core provede pro každý požadavek, když je spuštěna `UseSpaPrerendering` middlewaru, který jste přidali do *spuštění* třídy. Má zacházet s přijetím `params` z kódu .NET (například se požadovanou adresu URL) a volání rozhraní API úhlová SSR získat výsledné HTML. 

Výhradně platí, že toto je dostačující k povolení SSR v režimu pro vývoj. Je nezbytné k provedení jednoho poslední změny tak, aby vaše aplikace funguje správně, při publikování. V hlavním vaší aplikace *.csproj* souboru, nastavte `BuildServerSideRenderer` hodnotu vlastnosti na `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Tím se nakonfiguruje ke spuštění procesu sestavení `build:ssr` během publikování a nasazení SSR soubory na server. Pokud to nepovolíte, SSR selže v produkčním prostředí.

Když aplikace běží v režimu pro vývoj nebo produkční, kód úhlová předem vykreslí jako kód HTML na serveru. Kód klienta spouští jako normální.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Předání dat z kódu .NET do kódu TypeScript

Během SSR můžete k předávání dat na žádost z vaší aplikace ASP.NET Core do úhlová aplikace. Například může předávat informace o souboru cookie nebo něco čtení z databáze. Chcete-li to provést, upravte vaše *spuštění* třídy. V zpětné volání pro `UseSpaPrerendering`, nastavte hodnotu pro `options.SupplyData` například následující:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that is passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` Zpětného volání vám umožní předat libovolný, požadavků, JSON serializable data (například řetězce, logických výrazů nebo číslice). Vaše *main.server.ts* kód obdrží jako `params.data`. Například v předchozím příkladu kódu předává logickou hodnotu jako `params.data.isHttpsRequest` do `createServerRenderer` zpětného volání. To můžete předat do dalších částí vaší aplikace žádným způsobem nepodporuje úhlová. Například v tématu Jak *main.server.ts* předá `BASE_URL` hodnotu pro všechny součásti, jejichž konstruktor je deklarovaná ji přijmout.

### <a name="drawbacks-of-ssr"></a>Nevýhody SSR

Ne všechny aplikace těžit z SSR. Hlavní výhoda bude považován výkonu. Návštěvníky dosažení aplikace přes pomalé připojení k síti nebo v pomalé mobilních zařízeních najdete v části rozhraní počáteční rychle, i v případě, že trvá, než se načíst nebo analyzovat sady JavaScript. Ale mnoho SPA se používá hlavně přes společnosti rychlé, interní sítě na počítačích rychlé kde aplikace se zobrazí prakticky okamžitě.

Ve stejnou dobu mají povolení SSR významné nedostatky. Přidá složitost vývojových procesech. Váš kód musí být spuštěn ve dvou různých prostředích: klientské a serverové (v prostředí s Node.js volat z ASP.NET Core). Zde jsou některé věci na berte v úvahu:

* SSR vyžaduje instalaci Node.js na provozních serverech. Toto je automaticky případu v některých případech nasazení, například Azure App Services, ale nikoli pro ostatní uživatele, například Azure Service Fabric.
* Povolení `BuildServerSideRenderer` sestavení příznak příčiny vaše *node_modules* directory k publikování. Tato složka obsahuje 20 000 + souborů, což zvyšuje čas nasazení.
* Pro spouštění vašeho kódu v prostředí Node.js, se nelze spoléhat na existenci rozhraní API jazyka JavaScript specifické pro prohlížeč, jako `window` nebo `localStorage`. Pokud váš kód (nebo některých jiných výrobců knihovna, kterou odkazujete) pokusí o použijte tato rozhraní API, budete při SSR dojde k chybě. Například nepoužívejte jQuery protože odkazuje na rozhraní API specifické pro prohlížeč na mnoha místech. Chcete-li zabránit chybám, musíte buď vyhnout SSR nebo vyhnout rozhraní API nebo knihovny specifické pro prohlížeč. Volání rozhraní API pro takové může obtékat v kontroluje, ujistěte se, že nejsou vyvolat během SSR. V kódu jazyka JavaScript nebo TypeScript například použijte kontrolu, jako jsou následující:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
