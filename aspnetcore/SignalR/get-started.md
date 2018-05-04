---
title: Začínáme s funkce SignalR technologie ASP.NET Core
author: rachelappel
description: V tomto kurzu vytvoříte aplikaci pomocí funkce SignalR pro ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 8b27e58a66c9030d92f9b9a6b8b2a7ed292e4ca9
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="e59a4-103">Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e59a4-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="e59a4-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e59a4-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="e59a4-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření v reálném čase aplikace pomocí funkce SignalR pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e59a4-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Řešení](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="e59a4-107">Tento kurz ukazuje vývoj SignalR následující:</span><span class="sxs-lookup"><span data-stu-id="e59a4-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e59a4-108">Vytvořte SignalR na webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e59a4-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="e59a4-109">Vytvořte rozbočovače SignalR tak, aby nabízel obsah pro klienty.</span><span class="sxs-lookup"><span data-stu-id="e59a4-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="e59a4-110">Upravit `Startup` třídy a konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="e59a4-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="e59a4-111">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e59a4-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="e59a4-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e59a4-112">Prerequisites</span></span>

<span data-ttu-id="e59a4-113">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="e59a4-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e59a4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e59a4-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e59a4-115">[.NET core 2.1.0 náhled 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) nebo novější</span><span class="sxs-lookup"><span data-stu-id="e59a4-115">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* <span data-ttu-id="e59a4-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="e59a4-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e59a4-117">npm</span><span class="sxs-lookup"><span data-stu-id="e59a4-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e59a4-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e59a4-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e59a4-119">[.NET core 2.1.0 náhled 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) nebo novější</span><span class="sxs-lookup"><span data-stu-id="e59a4-119">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* [<span data-ttu-id="e59a4-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e59a4-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e59a4-121">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e59a4-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="e59a4-122">npm</span><span class="sxs-lookup"><span data-stu-id="e59a4-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="e59a4-123">Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="e59a4-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e59a4-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e59a4-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="e59a4-125">Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e59a4-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e59a4-126">Název projektu *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="e59a4-126">Name the project *SignalRChat*.</span></span>

   ![Dialogové okno Nový projekt v sadě Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="e59a4-128">Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="e59a4-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="e59a4-129">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e59a4-129">Then select **OK**.</span></span> <span data-ttu-id="e59a4-130">Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="e59a4-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogové okno Nový projekt v sadě Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="e59a4-132">Visual Studio obsahuje `Microsoft.AspNetCore.SignalR` balíček obsahující jeho server knihoven jako součást jeho **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="e59a4-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="e59a4-133">Ale knihovny JavaScript klienta pro SignalR musí být nainstalovaný pomocí *npm*.</span><span class="sxs-lookup"><span data-stu-id="e59a4-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="e59a4-134">Spusťte následující příkazy **Konzola správce balíčků** okno z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="e59a4-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="e59a4-135">Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  k *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="e59a4-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e59a4-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e59a4-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="e59a4-137">Z **integrované Terminálové**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e59a4-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="e59a4-138">Instalace pomocí knihovny JavaScript klienta *npm*.</span><span class="sxs-lookup"><span data-stu-id="e59a4-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

3. <span data-ttu-id="e59a4-139">Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  k *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="e59a4-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="e59a4-140">Vytvoření centra SignalR</span><span class="sxs-lookup"><span data-stu-id="e59a4-140">Create the SignalR Hub</span></span>

<span data-ttu-id="e59a4-141">Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="e59a4-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e59a4-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e59a4-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="e59a4-143">Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**.</span><span class="sxs-lookup"><span data-stu-id="e59a4-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="e59a4-144">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="e59a4-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="e59a4-145">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.</span><span class="sxs-lookup"><span data-stu-id="e59a4-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="e59a4-146">Vytvořte `SendMessage` metoda, která odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="e59a4-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="e59a4-147">Všimněte si, vrátí hodnotu [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="e59a4-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="e59a4-148">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="e59a4-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e59a4-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e59a4-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="e59a4-150">Otevřete *SignalRChat* složky ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e59a4-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="e59a4-151">Do projektu přidejte třídu výběrem **soubor** > **nový soubor** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="e59a4-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="e59a4-152">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="e59a4-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="e59a4-153">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i příjem a odesílání dat do klientů.</span><span class="sxs-lookup"><span data-stu-id="e59a4-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="e59a4-154">Přidat `SendMessage` metody pro třídu.</span><span class="sxs-lookup"><span data-stu-id="e59a4-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="e59a4-155">`SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="e59a4-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="e59a4-156">Všimněte si, vrátí hodnotu [úloh](/dotnet/api/system.threading.tasks.task), protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="e59a4-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="e59a4-157">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="e59a4-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="e59a4-158">Konfigurace projektu pro použití funkce SignalR</span><span class="sxs-lookup"><span data-stu-id="e59a4-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="e59a4-159">SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.</span><span class="sxs-lookup"><span data-stu-id="e59a4-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="e59a4-160">Chcete-li konfigurovat projekt SignalR, změňte projektu `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="e59a4-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="e59a4-161">`services.AddSignalR` Přidá SignalR jako součást [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="e59a4-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="e59a4-162">Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="e59a4-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=36,56-59)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="e59a4-163">Vytvoření kódu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="e59a4-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="e59a4-164">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e59a4-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="e59a4-165">Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="e59a4-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="e59a4-166">Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="e59a4-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="e59a4-167">Přidejte soubor JavaScript s názvem *chat.js*do *wwwroot\js* složky.</span><span class="sxs-lookup"><span data-stu-id="e59a4-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="e59a4-168">Přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="e59a4-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="e59a4-169">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e59a4-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e59a4-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e59a4-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e59a4-171">Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně.</span><span class="sxs-lookup"><span data-stu-id="e59a4-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="e59a4-172">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="e59a4-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="e59a4-173">Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="e59a4-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="e59a4-174">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e59a4-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="e59a4-175">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="e59a4-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e59a4-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e59a4-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e59a4-177">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="e59a4-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="e59a4-178">Spuštění programu otevře okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e59a4-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="e59a4-179">Otevře další okno prohlížeče a webu místně v zatížení.</span><span class="sxs-lookup"><span data-stu-id="e59a4-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="e59a4-180">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e59a4-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="e59a4-181">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="e59a4-181">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Řešení](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="e59a4-183">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="e59a4-183">Related resources</span></span>

[<span data-ttu-id="e59a4-184">Úvod do základní ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="e59a4-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)