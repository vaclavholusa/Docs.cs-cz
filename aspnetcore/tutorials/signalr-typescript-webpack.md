---
title: Jádro ASP.NET SignalR pomocí TypeScript a Webpack
author: ssougnez
description: V tomto kurzu nakonfigurujete Webpack sady a sestavení webové aplikace ASP.NET Core SignalR, jehož klient je napsána v TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126425"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>Jádro ASP.NET SignalR pomocí TypeScript a Webpack

Podle [Sébastien Sougnez](https://twitter.com/ssougnez) a [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) vývojářům umožňuje vytvořit balíček a vytvářet prostředky klientské webové aplikace. Tento kurz ukazuje, jak pomocí Webpack ve webové aplikaci ASP.NET Core SignalR, jehož klient je napsána v [TypeScript](https://www.typescriptlang.org/).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vygenerovat úvodní ASP.NET Core SignalR aplikaci
> * Konfigurace klienta SignalR TypeScript
> * Konfigurace sestavení kanálu pomocí Webpack
> * Konfigurace serveru SignalR
> * Povolit komunikaci mezi klientem a serverem

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) s [npm](https://www.npmjs.com/)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) s [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Vytvoření webové aplikace ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Konfigurace Visual Studia a hledat npm v *cesta* proměnné prostředí. Ve výchozím nastavení používá verzi npm nalezené v adresáři jeho instalace sady Visual Studio. Postupujte podle těchto pokynů v sadě Visual Studio:

1. Přejděte na **nástroje** > **možnosti** > **projekty a řešení** > **webové správy balíčků**  >  **Nástroje pro externí Web**.
1. Vyberte *$(PATH)* položku ze seznamu. Klikněte na šipku nahoru přesunout položku na druhém pozici v seznamu. Jako vyhraďte první položku odkazuje na místní balíčky projektu.

    ![Konfigurace sady Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio konfigurace je dokončena. Je čas a vytvořte tak projekt.

1. Použití **soubor** > **nový** > **projektu** nabídky možnost a vyberte **webové aplikace ASP.NET Core** Šablona.
1. Název projektu *SignalRWebPack*a klikněte na **OK** tlačítko.
1. Vyberte *.NET Core* z rozhraní target framework rozevíracího seznamu a vyberte možnost *ASP.NET Core 2.1* z rozevíracího seznamu pro výběr framework. Vyberte **prázdný** šablonu a klikněte na **OK** tlačítko.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Spusťte následující příkaz **integrované Terminálové**:

```console
dotnet new web -o SignalRWebPack
```

Prázdný ASP.NET Core webové aplikace, cílení na .NET Core je vytvořen v *SignalRWebPack* adresáře.

---

## <a name="configure-webpack-and-typescript"></a>Konfigurace Webpack a TypeScript

Následující kroky konfigurace převodu TypeScript pro jazyk JavaScript a sdružování prostředky na straně klienta.

1. Spusťte následující příkaz v kořenu projektu pro vytvoření *package.json* souboru:

    ```console
    npm init -y
    ```

1. Přidat zvýrazněná vlastnost, která má *package.json* souboru:

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    Nastavení `private` vlastnost `true` brání upozornění instalace balíčku v dalším kroku.

1. Nainstalujte npm požadované balíčky. Spusťte následující příkaz z kořenového adresáře projektu:

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Některé podrobnosti příkazu příkazového Všimněte si:

    * Číslo verze odpovídá `@` přihlášení pro každý název balíčku. npm nainstaluje tyto konkrétní balíček verze.
    * `-E` Možnost zakáže výchozí chování je npm zápis [sémantické verze](https://semver.org/) rozsahu operátorům *package.json*. Například `"webpack": "4.12.0"` se používá místo `"webpack": "^4.12.0"`. Tato možnost zabraňuje nezamýšleným upgrade na novější verze balíčku.

    Najdete v oficiální [npm install](https://docs.npmjs.com/cli/install) dokumentace pro více podrobností.

1. Nahraďte `scripts` vlastnost *package.json* souboru následujícím fragmentem kódu:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Vysvětlení skripty:

    * `build`: Obsahuje ureitou prostředkům na straně klienta v režimu pro vývoj a sleduje změny. Sledovací proces souboru způsobí, že sada znovu vygenerovat pokaždé, když změní soubor projektu. `mode` Možnost zakáže produkční optimalizace, jako je například že zatřesete stromu a minimalizace. Používat pouze `build` ve vývoji.
    * `release`: Obsahuje ureitou prostředkům na straně klienta v produkčním režimu.
    * `publish`: Běží `release` skript sady prostředky klienta v produkčním režimu. Zavolá rozhraní .NET Core CLI [publikování](/dotnet/core/tools/dotnet-publish) příkaz pro publikování aplikace.

1. Vytvořte soubor s názvem *webpack.config.js*, v kořenu projektu s následujícím obsahem:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Soubor předchozí nakonfiguruje Webpack kompilace. Některé podrobnosti o konfiguraci pro Všimněte si:

    * `output` Vlastnost má přednost před výchozí hodnotu *dist*. Sada je místo toho vygenerované v *wwwroot* adresáře.
    * `resolve.extensions` Zahrnuje pole *.js* import ke klientovi SignalR JavaScript.

1. Vytvořte novou *src* adresář v kořenu projektu. Jejím účelem je ukládat prostředky klienta projektu.

1. Vytvoření *src/index.html* s následujícím obsahem.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Předchozí HTML definuje často používaný kód na domovské stránce.

1. Vytvořte novou *src/css* adresáře. Jejím účelem je k uložení projektu *.css* soubory.

1. Vytvoření *src/css/main.css* s následujícím obsahem:

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Podle předchozích *main.css* souboru styly aplikace.

1. Vytvoření *src/tsconfig.json* s následujícím obsahem:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Předchozí kód konfiguruje kompilátoru TypeScript vytvoření [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 kompatibilní JavaScript.

1. Vytvoření *src/index.ts* s následujícím obsahem:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Předchozí TypeScript načte odkazy na elementy DOM a připojí dvě obslužné rutiny událostí:

    * `keyup`: Tato událost aktivuje se v případě, že uživatel zadá něco do textového pole označený `tbMessage`. `send` Funkce je volána, když uživatel stiskne **Enter** klíč.
    * `click`: Tato událost se aktivuje, když uživatel klikne **odeslat** tlačítko. `send` Funkce je volána.

## <a name="configure-the-aspnet-core-app"></a>Konfigurace aplikace ASP.NET Core

1. Kód součástí `Startup.Configure` metoda zobrazí *Hello, World!*. Nahraďte `app.Run` volání metody pomocí volání [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Předchozí kód umožňuje serveru najít a sloužit *index.html* souboru, zda uživatel zadá jeho úplnou adresu URL nebo adresy URL kořenového adresáře webové aplikace.

1. Volání [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) v `Startup.ConfigureServices` metoda. Přidá do projektu služby SignalR.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Mapy */hub* směrovat `ChatHub` rozbočovače. Přidejte následující řádky na konci `Startup.Configure` metoda:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Vytvořte nový adresář, s názvem *Hubs*, v kořenu projektu. Jejím účelem je k uložení rozbočovače SignalR, která je vytvořena v dalším kroku.

1. Vytvoření centra *Hubs/ChatHub.cs* následujícím kódem:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Přidejte následující kód v horní části *Startup.cs* souboru přeložit `ChatHub` odkaz:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Povolení komunikace klienta a serveru

Aplikace zobrazí aktuálně jednoduchý formulář pro odeslání zprávy. Při pokusu o tomu nedojde k žádné akci. Server naslouchá na určitém trasu, ale neprovede žádnou akci s odeslaných zpráv.

1. Spusťte následující příkaz v kořenovém adresáři projektu:

    ```console
    npm install @aspnet/signalr
    ```

    Předchozí příkaz nainstaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), což umožňuje klienta k odesílání zpráv do serveru.

1. Přidejte zvýrazněný kód, který *src/index.ts* souboru:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Předchozí kód podporuje přijímání zpráv ze serveru. `HubConnectionBuilder` Třída vytvoří nového tvůrce pro konfiguraci připojení k serveru. `withUrl` Funkce konfiguruje adresu URL rozbočovače.

    SignalR umožňuje výměnu zpráv mezi klientem a serverem. Každá zpráva má určitý název. Například můžete mít zprávy s názvem `messageReceived` , provádění logiky, která je odpovědná za zobrazování novou zprávu v zóně zprávy. Naslouchání na konkrétní zprávou, můžete to udělat pomocí `on` funkce. Můžete poslouchat libovolný počet názvů zprávy. Je také možné předat parametry zprávy, jako je jméno autora a obsah přijaté zprávy. Jakmile klient obdrží zprávu, nový `div` element je vytvořen s jméno autora a zpráva obsahu v jeho `innerHTML` atribut. Přidá do hlavní `div` element zobrazení zprávy.

1. Teď, když klient může přijímat zprávy, nakonfigurujte ji k odesílání zpráv. Přidejte zvýrazněný kód, který *src/index.ts* souboru:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    Odesílání zprávy prostřednictvím technologie WebSockets připojení vyžaduje volání `send` metoda. První parametr metody je název zprávy. Data zpráv se vyskytuje v podoblastech ostatní parametry. V tomto příkladu zprávu označeno `newMessage` je odeslat na server. Zpráva se skládá z uživatelské jméno a uživatelský vstup v textovém poli. Pokud odesílání funguje, není zaškrtnuté políčko textové hodnoty.

1. Zvýrazněná metody pro přidání `ChatHub` třídy:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Předchozí kód všesměrově přijatých zpráv pro všechny připojené uživatele, jakmile server obdrží je. Není nutné mít obecný `on` metoda přijímat všechny zprávy. Metoda s názvem po přípony názvu zprávy.

    V tomto příkladu TypeScript klient odešle zprávu identifikovat jako `newMessage`. C# `NewMessage` metoda očekává data odeslaná klientem. Přišla žádost o k [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metodu [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Přijaté zprávy se odesílají na všechny klienty připojené k rozbočovači.

## <a name="test-the-app"></a>Testování aplikace

Potvrďte, že aplikace funguje pomocí následujících kroků.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Spustit Webpack *verze* režimu. Pomocí **Konzola správce balíčků** okno, spusťte následující příkaz v kořenu projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Vyberte **ladění** > **spustit bez ladění** spusťte aplikaci v prohlížeči bez připojení ladicího programu. *Wwwroot/index.html* soubor je předán na `http://localhost:<port_number>`.

1. Otevřete jiná instance prohlížeče (libovolného prohlížeče). Vložte adresu URL na panelu Adresa.

1. Vyberte buď prohlížeče, zadejte v něco **zpráva** textového pole a klikněte na tlačítko **odeslat** tlačítko. Jedinečné uživatelské jméno a zpráva se zobrazí na obou stránkách okamžitě.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

1. Spustit Webpack *verze* režimu spuštěním následujícího příkazu v kořenu projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Sestavení a spuštění aplikace tak, že spustíte následující příkaz v kořenu projektu:

    ```console
    dotnet run
    ```

    Webový server spustí aplikaci a zpřístupní ho na localhost.

1. Otevřete prohlížeč na `http://localhost:<port_number>`. *Wwwroot/index.html* je soubor zpracován. Zkopírujte adresu URL z panelu Adresa.

1. Otevřete jiná instance prohlížeče (libovolného prohlížeče). Vložte adresu URL na panelu Adresa.

1. Vyberte buď prohlížeče, zadejte v něco **zpráva** textového pole a klikněte na tlačítko **odeslat** tlačítko. Jedinečné uživatelské jméno a zpráva se zobrazí na obou stránkách okamžitě.

---

![zpráva zobrazená v prohlížeči systému windows](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Další zdroje

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>