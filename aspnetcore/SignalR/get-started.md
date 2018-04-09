---
title: Začínáme s funkce SignalR technologie ASP.NET Core
author: rachelappel
description: V tomto kurzu vytvoříte aplikaci pomocí funkce SignalR pro ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Začínáme s funkce SignalR technologie ASP.NET Core

Podle [Rachel Appel](https://twitter.com/rachelappel)

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

V tomto kurzu se dozvíte, jaké základní informace o vytváření v reálném čase aplikace pomocí funkce SignalR pro ASP.NET Core.

   ![Řešení](get-started/_static/signalr-get-started-finished.png)

Tento kurz ukazuje vývoj SignalR následující:

> [!div class="checklist"]
> * Vytvořte SignalR na webovou aplikaci ASP.NET Core.
> * Vytvořte rozbočovače SignalR tak, aby nabízel obsah pro klienty.
> * Upravit `Startup` třídy a konfigurace aplikace.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) nebo novější
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.6 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) nebo novější
* [Visual Studio Code](https://code.visualstudio.com/download) 
* [C# pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
1. Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**. Název projektu *SignalRChat*.

   ![Dialogové okno Nový projekt v sadě Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor. Potom vyberte **OK**. Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.

   ![Dialogové okno Nový projekt v sadě Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

3. Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **přidat** > **nová položka** > **konfigurační soubor npm** . Název souboru *package.json*.

4. Spusťte následující příkaz **Konzola správce balíčků** okno z kořenového adresáře projektu:

    ```console
      npm install @aspnet/signalr
    ```
5. Kopírování <em>signalr.js</em> souboru z <em>node_modules\\ @aspnet\signalr\dist\browser</em>  k <em>wwwroot\lib</em> složku ve vašem projektu.

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
1. Z **integrované Terminálové**, spusťte následující příkaz:

    ```console
      dotnet new razor -o SignalRChat
    ```

2. Instalace pomocí knihovny JavaScript klienta *npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a>Vytvoření centra SignalR

Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
1. Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**.

2. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.

3. Vytvořte `SendMessage` metoda, která odešle zprávu do všech klientů připojených konverzace. Všimněte si, vrátí hodnotu [úloh](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), protože SignalR je asynchronní. Asynchronní kódu poskytuje lepší škálovatelnost.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
1. Otevřete *SignalRChat* složky ve Visual Studio Code.

2. Do projektu přidejte třídu výběrem **soubor** > **nový soubor** z nabídky.

3. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i příjem a odesílání dat do klientů.

4. Přidat `SendMessage` metody pro třídu. `SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace. Všimněte si, vrátí hodnotu [úloh](/dotnet/api/system.threading.tasks.task), protože SignalR je asynchronní. Asynchronní kódu poskytuje lepší škálovatelnost.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a>Konfigurace projektu pro použití funkce SignalR

SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.

1. Chcete-li konfigurovat projekt SignalR, změňte projektu `Startup.ConfigureServices` metoda.

   `services.AddSignalR` Přidá SignalR jako součást [middleware](xref:fundamentals/middleware/index) kanálu.

2. Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Vytvoření kódu klienta SignalR

1. Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání. Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.

2. Přidejte soubor JavaScript s názvem *chat.js*do *wwwroot\js* složky. Přidejte do ní následující kód:

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Spuštění aplikace

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně. Zkopírujte adresu URL z panelu Adresa.

1. Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.

1. Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko. Název a zpráva se zobrazí na obou stránkách okamžitě.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu. Spuštění programu otevře okno prohlížeče.

1. Otevře další okno prohlížeče a webu místně v zatížení.

1. Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko. Název a zpráva se zobrazí na obou stránkách okamžitě.

-----

  ![Řešení](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Související informační zdroje

[Úvod do základní ASP.NET SignalR](introduction.md)