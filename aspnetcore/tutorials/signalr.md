---
title: Začínáme s funkce SignalR technologie ASP.NET Core
author: tdykstra
description: V tomto kurzu vytvoříte aplikaci pomocí nástroje SignalR pro ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095488"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Začínáme s funkce SignalR technologie ASP.NET Core

V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR pro ASP.NET Core.

   ![Řešení](signalr/_static/signalr-get-started-finished.png)

Tento kurz demonstruje následující úkoly vývoje SignalR:

> [!div class="checklist"]
> * Vytvoření knihovnou SignalR ve webové aplikaci ASP.NET Core.
> * Vytvoření rozbočovače SignalR tak, aby nabízel obsah pro klienty.
> * Upravit `Startup` třídy a konfigurace aplikace.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7.3 nebo novější s **vývoj pro ASP.NET a web** pracovního vytížení
* [npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Vytvoření projektu aplikace ASP.NET Core, který je hostitelem SignalR klienta a serveru

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Použití **souboru** > **nový projekt** nabídku a vyberte **webové aplikace ASP.NET Core**. Pojmenujte projekt *SignalRChat*.

   ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Vyberte **webovou aplikaci** vytvoření projektu pomocí stránky Razor. Potom vyberte **OK**. Ujistěte se, že **ASP.NET Core 2.1** vyberete ze selektoru rozhraní framework, i když SignalR běží na starší verze rozhraní .NET.

   ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio obsahuje `Microsoft.AspNetCore.SignalR` balíček, který obsahuje jeho server knihoven jako součást jeho **webové aplikace ASP.NET Core** šablony. Nicméně knihovny JavaScript klienta pro funkci SignalR musí být nainstalovaný pomocí *npm*.

3. Spuštěním následujících příkazů **Konzola správce balíčků** okna z kořenového adresáře projektu:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Vytvořte novou složku s názvem "signalr" uvnitř *wwwroot/lib* složku ve vašem projektu. Kopírovat *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Z **integrovaný terminál**, spusťte následující příkaz:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Nainstalovat s použitím knihovny JavaScript klienta *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu. Kopírovat *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.

---

## <a name="create-the-signalr-hub"></a>Vytvoření rozbočovače SignalR

Centrum je třída, která slouží jako základní kanál, který umožňuje klientovi i serveru, volání metod na sobě navzájem.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Přidání třídy do projektu výběrem **souboru** > **nový** > **souboru** a vyberete **třídy Visual C#**. Název třídy `ChatHub` a soubor *ChatHub.cs*.

2. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupiny, jakož i příjem a odesílání data.

3. Vytvořte `SendMessage` metodu, která odešle zprávu do všech klientů připojených konverzace. Všimněte si, že ji vrací [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), protože je asynchronní funkce SignalR. Asynchronní kód poskytuje lepší škálovatelnost.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Otevřít *SignalRChat* složky ve Visual Studio Code.

2. Přidání třídy do projektu tak, že vyberete **souboru** > **nový soubor** z nabídky. Název třídy `ChatHub` a soubor *ChatHub.cs*.

3. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupiny, jakož i příjem a odesílání dat do klientů.

4. Přidat `SendMessage` metodu do třídy. `SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace. Všimněte si, že ji vrací [úloh](/dotnet/api/system.threading.tasks.task), protože je asynchronní funkce SignalR. Asynchronní kód poskytuje lepší škálovatelnost.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Nakonfigurujte projekt tak, aby používaly SignalR

SignalR server musí nakonfigurovat tak, aby věděl, že může předat požadavky SignalR.

1. Chcete-li nakonfigurovat projekt SignalR, upravte projektu `Startup.ConfigureServices` metody.

   `services.AddSignalR` je k dispozici pro služby SignalR [injektáž závislostí](xref:fundamentals/dependency-injection) systému.

1. Konfigurují trasy pro vaše uzlům `UseSignalR` v `Configure` metody. `app.UseSignalR` Přidá připojení SignalR pro [middleware](xref:fundamentals/middleware/index) kanálu.

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Vytvořit kód klienta SignalR

1. Přidejte soubor JavaScriptu s názvem *chat.js*, *wwwroot\js* složky. Přidejte do ní následující kód:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Předchozí kód HTML zobrazí název a zprávu pole a tlačítka Odeslat. Všimněte si, že odkazy na skript v dolní části: referenční dokumentace ke knihovně SignalR a *chat.js*.

## <a name="run-the-app"></a>Spuštění aplikace

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Vyberte **ladění** > **spustit bez ladění** spusťte prohlížeč a zatížení webu místně. Zkopírujte adresu URL z panelu Adresa.

1. Otevřete jiná instance prohlížeče (libovolného prohlížeče) a vložte adresu URL do adresního řádku.

1. Zvolte buď prohlížeče, zadejte název a zprávu a klikněte na tlačítko **odeslat** tlačítko. Název a zpráva se zobrazí na obě stránky okamžitě.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program. Spuštění programu se otevře okno prohlížeče.

1. Otevřete další okno prohlížeče a zatížení webu místně v ní.

1. Zvolte buď prohlížeče, zadejte název a zprávu a klikněte na tlačítko **odeslat** tlačítko. Název a zpráva se zobrazí na obě stránky okamžitě.

---

  ![Řešení](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Související prostředky

[Úvod do ASP.NET Core SignalR](xref:signalr/introduction)
