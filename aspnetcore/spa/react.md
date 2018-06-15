---
title: Šablona projektu reagují pomocí ASP.NET Core
author: SteveSandersonMS
description: Zjistěte, jak začít pracovat se šablonou projektu ASP.NET Core jedné stránky aplikace (SPA) pro reagují a vytvořit aplikaci reagují.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 073259aaca7b72ca8ac61b290499c627cb6489ae
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/14/2018
ms.locfileid: "35612963"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>Šablona projektu reagují pomocí ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Tato dokumentace není o šablona projektu reagují součástí technologie ASP.NET 2.0 jádra. Jde o šabloně novější reagují, do kterého můžete ručně aktualizovat. Šablona je součástí 2.1 jádro ASP.NET ve výchozím nastavení.

::: moniker-end

Aktualizovanou šablonu projektu reagují poskytuje příhodný výchozí bod pro ASP.NET Core aplikací pomocí reagují a [vytvořit aplikaci reagují](https://github.com/facebookincubator/create-react-app) konvence (CRA) k implementaci bohatou a klientské uživatelské rozhraní (UI).

Šablona je ekvivalentní k vytvoření projektu ASP.NET Core tak, aby fungoval jako back-end rozhraní API i standardní CRA reagovat projekt tak, aby fungoval jako uživatelského rozhraní, ale s výhodou hostování oba seznamy v jedné aplikaci projekt, který můžete vytvořená a publikovaná jako na jednu jednotku.

## <a name="create-a-new-app"></a>Vytvoření nové aplikace

::: moniker range="= aspnetcore-2.0"

Pokud používáte technologii ASP.NET 2.0 jádra, ujistěte se, když jste [nainstalovat aktualizovanou šablonu projektu reagují](xref:spa/index#installation).

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Pokud máte ASP.NET Core 2.1 nainstalovaný, není nutné k instalaci v šabloně projektů reagují.

::: moniker-end

Vytvoření nového projektu z příkazového řádku pomocí příkazu `dotnet new react` v prázdného adresáře. Můžete například vytvořit následující příkazy v aplikaci *-nové aplikace my* adresáře a přepnete se do této složky:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Spusťte aplikaci v sadě Visual Studio nebo .NET Core rozhraní příkazového řádku:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otevřete vygenerovaného *.csproj* souboru a spuštění aplikace jako normální odtud.

Proces sestavení obnoví npm závislosti během prvního spuštění, což může trvat několik minut. Následující sestavení je mnohem rychlejší.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Zajistěte, abyste měli proměnné prostředí s názvem `ASPNETCORE_Environment` s hodnotou `Development`. V systému Windows (v výzvy – prostředí PowerShell), spusťte `SET ASPNETCORE_Environment=Development`. V systému macOS nebo Linux, spusťte `export ASPNETCORE_Environment=Development`.

Spustit [dotnet sestavení](/dotnet/core/tools/dotnet-build) k ověření aplikace sestavení správně. Při prvním spuštění procesu sestavení obnoví npm závislosti, které může trvat několik minut. Následující sestavení je mnohem rychlejší.

Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) a spusťte aplikaci.

---

Šablona projektu vytvoří aplikace ASP.NET Core a reagují aplikaci. Aplikace ASP.NET Core se má použít pro přístup k datům, autorizaci a další otázky straně serveru. Reagují aplikace, které se nacházejí v *ClientApp* podadresáři, je určena k použití pro všechny otázky uživatelského rozhraní.

## <a name="add-pages-images-styles-modules-etc"></a>Přidání stránky, obrázky, styly, moduly, atd.

*ClientApp* adresář je standardní CRA reagovat aplikace. Najdete v oficiální [CRA dokumentaci](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) Další informace.

Jsou drobné rozdíly mezi reagují aplikace vytvořené pomocí této šablony a jeden vytvořené CRA sama o sobě; schopnosti aplikace jsou však beze změny. Obsahuje aplikace vytvořené pomocí šablony [Bootstrap](https://getbootstrap.com/)– na základě rozložení a základní příklad směrování.

## <a name="install-npm-packages"></a>Instalace balíčků npm

Chcete-li instalovat balíčky jiných výrobců npm, použijte na příkazovém řádku ve *ClientApp* podadresáři. Příklad:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikování a nasazení

Ve vývoji aplikace běží v režimu optimalizovaná pro vývojáře pohodlí. Například JavaScript sady zahrnují zdroj mapy (tak, aby při ladění, zobrazí se původní zdrojový kód). Aplikace sleduje JavaScript, HTML a CSS změny souborů na disku a automaticky znovu zkompiluje a znovu načte, jakmile je detekována tyto soubory změnit.

V produkčním prostředí sloužit verzi vaší aplikace, která je optimalizovaná pro výkon. Ta se nakonfiguruje automaticky provést. Když publikujete, je konfigurace sestavení vysílá minifikovaný, sestavení transpiled kódu na straně klienta. Na rozdíl od sestavení vývoj nevyžaduje produkční build Node.js k instalaci na serveru.

Můžete použít standardní [metody nasazení a hostování ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Spusťte CRA server nezávisle

Projekt je nakonfigurován na spuštění svou vlastní instanci vývojový server CRA na pozadí při spuštění v režimu pro vývoj aplikace ASP.NET Core. To je výhodné, protože znamená to, že nemusíte ručně spustit samostatný server.

Není nevýhodou toto výchozí nastavení. Pokaždé, když upravíte kódu C# a vaše ASP.NET Core aplikace potřebuje k restartování, CRA server se restartuje. Několik sekund jsou nutné ke spuštění zálohování. Pokud provádíme za časté úpravy kódu jazyka C# a nechcete čekat CRA server restartovat, spusťte server CRA externě, nezávisle na procesu ASP.NET Core. Postupujte následovně:

1. V příkazovém řádku přepnout *ClientApp* podadresáři a spouštět CRA vývojový server:

    ```console
    cd ClientApp
    npm start
    ```

2. Upravte v aplikaci ASP.NET Core použít externí instance serveru CRA místo spuštění jeho vlastní. Ve vaší *spuštění* třídy, nahraďte `spa.UseReactDevelopmentServer` volání následujícím kódem:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Při spuštění aplikace ASP.NET Core nespustí CRA serveru. Instance, kterou jste spustili ručně bude místo něj použita. To umožňuje spustit a restartovat rychlejší. Se už čeká aplikace reagují pokaždé, když znovu sestavit.
