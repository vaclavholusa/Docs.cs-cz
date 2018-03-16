---
title: "Začínáme s funkce SignalR technologie ASP.NET Core"
author: rachelappel
description: "V tomto kurzu vytvoříte aplikaci pomocí funkce SignalR pro ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="1e834-103">Kurz: Začínáme s SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e834-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="1e834-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="1e834-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="1e834-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření v reálném čase aplikace pomocí funkce SignalR pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e834-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Řešení](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="1e834-107">Tento kurz ukazuje vývoj SignalR následující:</span><span class="sxs-lookup"><span data-stu-id="1e834-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1e834-108">Vytvořte SignalR na webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e834-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="1e834-109">Vytvořte rozbočovače SignalR tak, aby nabízel obsah pro klienty.</span><span class="sxs-lookup"><span data-stu-id="1e834-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="1e834-110">Upravit `Startup` třídy a konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e834-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="1e834-111">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e834-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="1e834-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e834-112">Prerequisites</span></span>

<span data-ttu-id="1e834-113">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="1e834-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e834-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e834-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1e834-115">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) nebo novější</span><span class="sxs-lookup"><span data-stu-id="1e834-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="1e834-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.6 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="1e834-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="1e834-117">npm</span><span class="sxs-lookup"><span data-stu-id="1e834-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e834-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e834-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1e834-119">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) nebo novější</span><span class="sxs-lookup"><span data-stu-id="1e834-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="1e834-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e834-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="1e834-121">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e834-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="1e834-122">npm</span><span class="sxs-lookup"><span data-stu-id="1e834-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="1e834-123">Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="1e834-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e834-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e834-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1e834-125">Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1e834-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="1e834-126">Název projektu *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="1e834-126">Name the project *SignalRChat*.</span></span>

  ![Dialogové okno Nový projekt v sadě Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="1e834-128">Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="1e834-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="1e834-129">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e834-129">Then select **OK**.</span></span> <span data-ttu-id="1e834-130">Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="1e834-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Dialogové okno Nový projekt v sadě Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="1e834-132">Klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** > **přidat** > **nová položka** > **konfigurační soubor npm** .</span><span class="sxs-lookup"><span data-stu-id="1e834-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="1e834-133">Název souboru *package.json*.</span><span class="sxs-lookup"><span data-stu-id="1e834-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="1e834-134">Spusťte následující příkaz **Konzola správce balíčků** okno z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="1e834-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="1e834-135">Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  k *wwwroot\lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="1e834-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e834-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e834-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1e834-137">Z **integrované Terminálové**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1e834-137">From the **Integrated Terminal**, run the following command:</span></span>
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="1e834-138">Instalace pomocí knihovny JavaScript klienta *npm*.</span><span class="sxs-lookup"><span data-stu-id="1e834-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="1e834-139">Vytvoření centra SignalR</span><span class="sxs-lookup"><span data-stu-id="1e834-139">Create the SignalR Hub</span></span>

<span data-ttu-id="1e834-140">Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="1e834-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e834-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e834-141">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1e834-142">Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**.</span><span class="sxs-lookup"><span data-stu-id="1e834-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

1. <span data-ttu-id="1e834-143">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="1e834-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="1e834-144">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.</span><span class="sxs-lookup"><span data-stu-id="1e834-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="1e834-145">Vytvořte `SendMessage` metoda, která odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="1e834-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="1e834-146">Všimněte si, vrátí hodnotu [úloh](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="1e834-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="1e834-147">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="1e834-147">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e834-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e834-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1e834-149">Otevřete *SignalRChat* složky ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1e834-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="1e834-150">Do projektu přidejte třídu výběrem **soubor** > **nový soubor** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="1e834-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

1. <span data-ttu-id="1e834-151">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="1e834-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="1e834-152">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i příjem a odesílání dat do klientů.</span><span class="sxs-lookup"><span data-stu-id="1e834-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

1. <span data-ttu-id="1e834-153">Přidat `SendMessage` metody pro třídu.</span><span class="sxs-lookup"><span data-stu-id="1e834-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="1e834-154">`SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="1e834-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="1e834-155">Všimněte si, vrátí hodnotu [úloh](/dotnet/api/system.threading.tasks.task), protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="1e834-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="1e834-156">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="1e834-156">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="1e834-157">Konfigurace projektu pro použití funkce SignalR</span><span class="sxs-lookup"><span data-stu-id="1e834-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="1e834-158">SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e834-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="1e834-159">Chcete-li konfigurovat projekt SignalR, změňte projektu `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="1e834-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="1e834-160">`services.AddSignalR` Přidá SignalR jako součást [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="1e834-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="1e834-161">Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="1e834-161">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="1e834-162">Vytvoření kódu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="1e834-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="1e834-163">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1e834-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="1e834-164">Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="1e834-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="1e834-165">Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="1e834-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="1e834-166">Přidejte soubor JavaScript s názvem *chat.js*do *wwwroot\js* složky.</span><span class="sxs-lookup"><span data-stu-id="1e834-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="1e834-167">Přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="1e834-167">Add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="1e834-168">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1e834-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e834-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e834-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1e834-170">Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně.</span><span class="sxs-lookup"><span data-stu-id="1e834-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="1e834-171">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="1e834-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="1e834-172">Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="1e834-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="1e834-173">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e834-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="1e834-174">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1e834-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e834-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e834-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1e834-176">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="1e834-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="1e834-177">Spuštění programu otevře okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1e834-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="1e834-178">Otevře další okno prohlížeče a webu místně v zatížení.</span><span class="sxs-lookup"><span data-stu-id="1e834-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="1e834-179">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e834-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="1e834-180">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1e834-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Řešení](get-started-signalr-core/_static/signalr-get-started-finished.png)