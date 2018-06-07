---
title: Začínáme s funkce SignalR technologie ASP.NET Core
author: rachelappel
description: V tomto kurzu vytvoříte aplikaci pomocí funkce SignalR pro ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: ba1db640e5608fd9f5e7fa024283a651bf7772c2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819055"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Začínáme s funkce SignalR technologie ASP.NET Core

Podle [Rachel Appel](https://twitter.com/rachelappel)

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

* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**. Název projektu *SignalRChat*.

   ![Dialogové okno Nový projekt v sadě Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor. Potom vyberte **OK**. Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.

   ![Dialogové okno Nový projekt v sadě Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

Visual Studio obsahuje `Microsoft.AspNetCore.SignalR` balíček obsahující jeho server knihoven jako součást jeho **webové aplikace ASP.NET Core** šablony. Ale knihovny JavaScript klienta pro SignalR musí být nainstalovaný pomocí *npm*.

3. Spusťte následující příkazy **Konzola správce balíčků** okno z kořenového adresáře projektu:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu. Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Z **integrované Terminálové**, spusťte následující příkaz:

    ```console
    dotnet new webapp -o SignalRChat
    ```

2. Instalace pomocí knihovny JavaScript klienta *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu. Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.

-----

## <a name="create-the-signalr-hub"></a>Vytvoření centra SignalR

Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**. Název souboru *ChatHub*. 

2. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.

3. Vytvořte `SendMessage` metoda, která odešle zprávu do všech klientů připojených konverzace. Všimněte si, vrátí hodnotu [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), protože SignalR je asynchronní. Asynchronní kódu poskytuje lepší škálovatelnost.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Otevřete *SignalRChat* složky ve Visual Studio Code.

2. Do projektu přidejte třídu výběrem **soubor** > **nový soubor** z nabídky.

3. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i příjem a odesílání dat do klientů.

4. Přidat `SendMessage` metody pro třídu. `SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace. Všimněte si, vrátí hodnotu [úloh](/dotnet/api/system.threading.tasks.task), protože SignalR je asynchronní. Asynchronní kódu poskytuje lepší škálovatelnost.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Konfigurace projektu pro použití funkce SignalR

SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.

1. Chcete-li konfigurovat projekt SignalR, změňte projektu `Startup.ConfigureServices` metoda.

   `services.AddSignalR` Přidá SignalR jako součást [middleware](xref:fundamentals/middleware/index) kanálu.

2. Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a>Vytvoření kódu klienta SignalR

1. Přidejte soubor JavaScript s názvem *chat.js*do *wwwroot\js* složky. Přidejte do ní následující kód:

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

2. Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání. Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.


## <a name="run-the-app"></a>Spuštění aplikace

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně. Zkopírujte adresu URL z panelu Adresa.

1. Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.

1. Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko. Název a zpráva se zobrazí na obou stránkách okamžitě.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu. Spuštění programu otevře okno prohlížeče.

1. Otevře další okno prohlížeče a webu místně v zatížení.

1. Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko. Název a zpráva se zobrazí na obou stránkách okamžitě.

---

  ![Řešení](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Související informační zdroje

[Úvod do základní ASP.NET SignalR](introduction.md)
