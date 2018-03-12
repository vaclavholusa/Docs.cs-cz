---
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
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="18428-103">Kurz: Začínáme s SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18428-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="18428-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="18428-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="18428-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření v reálném čase aplikace pomocí funkce SignalR pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="18428-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Řešení](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="18428-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18428-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="18428-108">Tento kurz ukazuje vývoj SignalR následující:</span><span class="sxs-lookup"><span data-stu-id="18428-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="18428-109">Vytvoření webové aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="18428-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="18428-110">Vytvořte rozbočovače SignalR tak, aby nabízel obsah pro klienty.</span><span class="sxs-lookup"><span data-stu-id="18428-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="18428-111">V knihovně SignalR JavaScript k odesílání zpráv a zobrazení aktualizací z rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="18428-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18428-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="18428-112">Prerequisites</span></span>

<span data-ttu-id="18428-113">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="18428-113">Install the following software:</span></span>

* <span data-ttu-id="18428-114">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) nebo novější</span><span class="sxs-lookup"><span data-stu-id="18428-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="18428-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.6 nebo novější s ASP.NET a webové úlohy vývoj</span><span class="sxs-lookup"><span data-stu-id="18428-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="18428-116">npm</span><span class="sxs-lookup"><span data-stu-id="18428-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="18428-117">Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="18428-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="18428-118">Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="18428-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="18428-119">Název projektu `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="18428-119">Name the project `SignalRChat`.</span></span>

  ![Dialogové okno Nový projekt v sadě Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="18428-121">Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="18428-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="18428-122">Potom vyberte **Ok**.</span><span class="sxs-lookup"><span data-stu-id="18428-122">Then select **Ok**.</span></span> <span data-ttu-id="18428-123">Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="18428-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Dialogové okno Nový projekt v sadě Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="18428-125">V šabloně projektů jsou součástí knihovny, které jsou hostiteli kódu na straně serveru SignalR.</span><span class="sxs-lookup"><span data-stu-id="18428-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="18428-126">Nainstalujte klienta JavaScript samostatně s [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="18428-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="18428-127">Kopírování *signalr.js* z *node_modules\\ @aspnet\signalr\dist\browser*  k *wwwroot\lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="18428-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="18428-128">Vytvoření centra SignalR</span><span class="sxs-lookup"><span data-stu-id="18428-128">Create the SignalR Hub</span></span>

<span data-ttu-id="18428-129">Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="18428-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="18428-130">Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**.</span><span class="sxs-lookup"><span data-stu-id="18428-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="18428-131">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="18428-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="18428-132">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.</span><span class="sxs-lookup"><span data-stu-id="18428-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="18428-133">Vytvořte `Send` metoda, která odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="18428-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="18428-134">Všimněte si, vrátí hodnotu `Task`, protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="18428-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="18428-135">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="18428-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="18428-136">Konfigurace projektu pro použití funkce SignalR</span><span class="sxs-lookup"><span data-stu-id="18428-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="18428-137">SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.</span><span class="sxs-lookup"><span data-stu-id="18428-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="18428-138">Chcete-li konfigurovat projekt SignalR, změňte `ConfigureServices` metoda aplikace `Startup` třída vložením volání `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="18428-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="18428-139">`services.AddSignalR` Přidá SignalR jako součást [ASP.NET Core middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="18428-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="18428-140">Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="18428-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="18428-141">Vytvoření kódu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="18428-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="18428-142">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="18428-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="18428-143">Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="18428-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="18428-144">Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="18428-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="18428-145">Přidejte soubor jazyka JavaScript *wwwroot\js* složku s názvem *chat.js* a přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="18428-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="18428-146">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="18428-146">Run the app</span></span>

1. <span data-ttu-id="18428-147">Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně.</span><span class="sxs-lookup"><span data-stu-id="18428-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="18428-148">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="18428-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="18428-149">Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="18428-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="18428-150">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="18428-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="18428-151">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="18428-151">The name and message are displayed on both pages instantly.</span></span>

  ![Řešení](get-started-signalr-core/_static/signalr-get-started-finished.png)
