---
uid: signalr/get-started-signalr-core
title: "Začínáme s funkce SignalR technologie ASP.NET Core"
author: rachelappel
ms.author: rachelap
description: "V tomto kurzu vytvoříte aplikaci pomocí funkce SignalR pro ASP.NET Core."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
ms.openlocfilehash: 5e16569aa492e3639aa97abd241610b361fb0c56
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Kurz: Začínáme s SignalR pro ASP.NET Core

Podle [Rachel Appel](https://twitter.com/rachelappel)

V tomto kurzu se dozvíte, jaké základní informace o vytváření v reálném čase aplikace pomocí funkce SignalR pro ASP.NET Core.

   ![Řešení](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Tento kurz ukazuje vývoj SignalR následující:

> [!div class="checklist"]
> * Vytvoření webové aplikace ASP.NET Core.
> * Vytvořte rozbočovače SignalR tak, aby nabízel obsah pro klienty.
> * V knihovně SignalR JavaScript k odesílání zpráv a zobrazení aktualizací z rozbočovače.

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) nebo novější
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.6 nebo novější s ASP.NET a webové úlohy vývoj
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru

1. Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**. Název projektu `SignalRChat`.

  ![Dialogové okno Nový projekt v sadě Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor. Potom vyberte **Ok**. Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.

  ![Dialogové okno Nový projekt v sadě Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  V šabloně projektů jsou součástí knihovny, které jsou hostiteli kódu na straně serveru SignalR. Nainstalujte klienta JavaScript samostatně s [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Kopírování *signalr.js* z *node_modules\\ @aspnet\signalr\dist\browser*  k *wwwroot\lib* složku ve vašem projektu.

## <a name="create-the-signalr-hub"></a>Vytvoření centra SignalR

Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.

1. Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**. 

1. Dědit z `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.

1. Vytvořte `Send` metoda, která odešle zprávu do všech klientů připojených konverzace. Všimněte si, vrátí hodnotu `Task`, protože SignalR je asynchronní. Asynchronní kódu poskytuje lepší škálovatelnost.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Konfigurace projektu pro použití funkce SignalR

SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.

1. Chcete-li konfigurovat projekt SignalR, změňte `ConfigureServices` metoda aplikace `Startup` třída vložením volání `services.AddSignalR`.

  `services.AddSignalR` Přidá SignalR jako součást [ASP.NET Core middleware](xref:fundamentals/middleware/index) kanálu.

1. Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Vytvoření kódu klienta SignalR

1. Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání. Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.

1. Přidejte soubor jazyka JavaScript *wwwroot\js* složku s názvem *chat.js* a přidejte do ní následující kód:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Spuštění aplikace

1. Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně. Zkopírujte adresu URL z panelu Adresa.

1. Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.

1. Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko. Název a zpráva se zobrazí na obou stránkách okamžitě.

  ![Řešení](get-started-signalr-core/_static/signalr-get-started-finished.png)
