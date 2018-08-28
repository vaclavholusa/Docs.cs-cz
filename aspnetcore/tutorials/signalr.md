---
title: 'Kurz: Začínáme s funkce SignalR technologie ASP.NET Core'
author: tdykstra
description: V tomto kurzu vytvoříte aplikaci chatu, který používá funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055829"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Kurz: Začínáme s funkce SignalR technologie ASP.NET Core

V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR. Získáte informace o následujících postupech:

> [!div class="checklist"]
> * Vytvoření webové aplikace, která používá SignalR v ASP.NET Core.
> * Vytvoření rozbočovače SignalR na serveru.
> * Připojte se k rozbočovači SignalR z klientů JavaScript.
> * Pomocí centra pro odesílání zpráv z libovolného klienta na všechny připojené klienty.

Na konci budete mít funkční aplikaci chatu:

![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Požadavky

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 verze 15.7.3 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro ASP.NET a web** pracovního vytížení
* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [C# pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Visual Studio pro Mac verze 7.5.4 nebo novější](https://www.visualstudio.com/downloads/)
* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all) (zahrnuty v instalaci sady Visual Studio)
* [npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)

---

## <a name="create-the-project"></a>Vytvoření projektu

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* V nabídce vyberte **soubor > Nový projekt**.

* V **nový projekt** dialogového okna, vyberte **nainstalováno > Visual C# > Web > Webová aplikace ASP.NET Core**. Pojmenujte projekt *SignalRChat*.

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Vyberte **webovou aplikaci** vytvořit projekt, který se používá pro stránky Razor.

* Vyberte cílovou architekturu **.NET Core**vyberte **ASP.NET Core 2.1**a klikněte na tlačítko **OK**.

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Otevřete složku, která můžete použít pro nový projekt.

* V **integrovaný terminál**, spusťte následující příkaz:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* V nabídce vyberte **soubor > Nový řešení**.

* Vyberte **.NET Core > aplikace > webové aplikace ASP.NET Core** (nevybírejte **ASP.NET Core webové aplikace (MVC)**).

* Vyberte **Další**.

* Pojmenujte projekt *SignalRChat*a pak vyberte **vytvořit**.

---

## <a name="add-the-signalr-client-library"></a>Přidat klientské knihovně SignalR

Je součástí serveru knihovny SignalR [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app). Ale budete muset získat Javascriptovou klientskou knihovnu z [npm, Správce balíčků Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* V **Konzola správce balíčků** (PMC), přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Přejděte do nové složky projektu.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* V **terminálu**, přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).

---

* Spustit inicializátor npm. Chcete-li vytvořit *package.json* souboru:

  ```console
  npm init -y
  ```

  Příkaz vytvoří výstup podobný následujícímu příkladu:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Nainstalujte balíček knihovny klienta:

  ```console
  npm install @aspnet/signalr
  ```

  Příkaz vytvoří výstup podobný následujícímu příkladu:

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

`npm install` Příkaz stáhli Javascriptovou klientskou knihovnu pro podsložku *node_modules*. Zkopírujte z něj do složky v části *wwwroot* , kterou můžete odkazovat z webové stránky chatovací aplikaci.

* Vytvoření *signalr* složky *wwwroot/lib*.

* Kopírovat *signalr.js* souboru z *node_modules/@aspnet/signalr/dist/browser* k novému *wwwroot/lib/signalr* složky.

## <a name="create-the-signalr-hub"></a>Vytvoření rozbočovače SignalR

A [centra](xref:signalr/hubs) je třída, která slouží jako základní kanál, který zpracovává vnitřní komunikaci klient server.

* Ve složce projektu SignalRChat vytvořit *rozbočovače* složky.

* V *rozbočovače* složku, vytvořte *ChatHub.cs* souboru následujícím kódem:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  `ChatHub` Třída dědí z funkce SignalR [centra](/dotnet/api/microsoft.aspnetcore.signalr.hub) třídy. `Hub` Třída spravuje připojení, skupiny a zasílání zpráv.

  `SendMessage` Metoda může být volán všech připojených klientů. Přijaté zprávy odešle všem klientům. Kód SignalR je asynchronní zajistit maximální škálovatelnosti.

## <a name="configure-the-project-to-use-signalr"></a>Nakonfigurujte projekt tak, aby používaly SignalR

Na serveru funkce SignalR nastavené předat požadavky SignalR SignalR.

* Přidejte následující zvýrazněný kód do *Startup.cs* souboru.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  SignalR pro přidání těchto změn [injektáž závislostí](xref:fundamentals/dependency-injection) systému a [middleware](xref:fundamentals/middleware/index) kanálu.

## <a name="create-the-signalr-client-code"></a>Vytvořit kód klienta SignalR

* Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Předchozí kód:

  * Vytvoří textová pole pro název a text zprávy a tlačítka Odeslat.
  * Vytvoří seznam s `id="messagesList"` pro zobrazení zprávy, které byly přijaty z rozbočovače SignalR.
  * Obsahuje odkazy na skript ke knihovně SignalR a *chat.js* aplikační kód, který vytvoříte v dalším kroku.

* V *wwwroot/js* složku, vytvořte *chat.js* souboru následujícím kódem:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Předchozí kód:

  * Vytvoří a spustí připojení.
  * Přidá tlačítko Odeslat obslužnou rutinu, která odesílá zprávy do centra.
  * Přidá objekt připojení obslužnou rutinu, která přijímá zprávy z centra a přidá je do seznamu.

## <a name="run-the-app"></a>Spuštění aplikace

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* V nabídce vyberte **spuštění > Spustit bez ladění**.

---

* Zkopírujte adresu URL z adresního řádku, otevřete jinou instanci prohlížeče nebo karty a vložte adresu URL do adresního řádku.

* Zvolte buď prohlížeče, zadejte název a zprávu a vybrat **odeslat** tlačítko.

  Název a zpráva se zobrazí na obě stránky okamžitě.

  ![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Pokud aplikace nebude fungovat, otevřete prohlížeč vývojářské nástroje (F12) a přejděte do konzoly. Můžou se zobrazovat chyby související s kódem HTML a JavaScript. Předpokládejme například, kam si ukládáte *signalr.js* v jiné složce než řízené. V takovém případě nebude fungovat odkaz na tento soubor a uvidíte v konzole chybu 404.
> ![Chyba nenalezení signalr.js](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Další kroky

Pokud chcete, aby klienti mohli připojovat k aplikaci s knihovnou SignalR z jiných domén, budete muset povolit sdílení prostředků mezi zdroji (CORS). Další informace najdete v tématu [sdílení prostředků různého původu](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Další informace o funkci SignalR, rozbočovačů a klientů JavaScript naleznete v následujících zdrojích:

* [Úvod ke knihovně SignalR pro ASP.NET Core](xref:signalr/introduction)
* [Použití rozbočovače signalr pro ASP.NET Core](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript klienta](xref:signalr/javascript-client)
