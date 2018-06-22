---
title: Začínáme s funkce SignalR technologie ASP.NET Core
author: rachelappel
description: V tomto kurzu vytvoříte aplikaci pomocí funkce SignalR pro ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 8762a4be1032d58014dd32dfdd3707197e14c6f9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291624"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="540a1-103">Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="540a1-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="540a1-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="540a1-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="540a1-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření v reálném čase aplikace pomocí funkce SignalR pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="540a1-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Řešení](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="540a1-107">Tento kurz ukazuje vývoj SignalR následující:</span><span class="sxs-lookup"><span data-stu-id="540a1-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="540a1-108">Vytvořte SignalR na webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="540a1-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="540a1-109">Vytvořte rozbočovače SignalR tak, aby nabízel obsah pro klienty.</span><span class="sxs-lookup"><span data-stu-id="540a1-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="540a1-110">Upravit `Startup` třídy a konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="540a1-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="540a1-111">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="540a1-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="540a1-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="540a1-112">Prerequisites</span></span>

<span data-ttu-id="540a1-113">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="540a1-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540a1-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540a1-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="540a1-115">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="540a1-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="540a1-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7 nebo novější s **ASP.NET a webové vývoj** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="540a1-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="540a1-117">npm</span><span class="sxs-lookup"><span data-stu-id="540a1-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540a1-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540a1-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="540a1-119">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="540a1-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="540a1-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540a1-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="540a1-121">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540a1-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="540a1-122">npm</span><span class="sxs-lookup"><span data-stu-id="540a1-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="540a1-123">Vytvoření projektu ASP.NET Core, který je hostitelem SignalR klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="540a1-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540a1-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540a1-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="540a1-125">Použití **soubor** > **nový projekt** nabídky možnost a zvolte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="540a1-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="540a1-126">Název projektu *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="540a1-126">Name the project *SignalRChat*.</span></span>

   ![Dialogové okno Nový projekt v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="540a1-128">Vyberte **webové aplikace** k vytvoření projektu pomocí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="540a1-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="540a1-129">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="540a1-129">Then select **OK**.</span></span> <span data-ttu-id="540a1-130">Ujistěte se, který **ASP.NET Core 2.1** se vybere z modulu pro výběr framework, i když SignalR běží ve starších verzích rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="540a1-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogové okno Nový projekt v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="540a1-132">Visual Studio obsahuje `Microsoft.AspNetCore.SignalR` balíček obsahující jeho server knihoven jako součást jeho **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="540a1-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="540a1-133">Ale knihovny JavaScript klienta pro SignalR musí být nainstalovaný pomocí *npm*.</span><span class="sxs-lookup"><span data-stu-id="540a1-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="540a1-134">Spusťte následující příkazy **Konzola správce balíčků** okno z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="540a1-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="540a1-135">Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="540a1-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="540a1-136">Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.</span><span class="sxs-lookup"><span data-stu-id="540a1-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540a1-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540a1-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="540a1-138">Z **integrované Terminálové**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="540a1-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="540a1-139">Instalace pomocí knihovny JavaScript klienta *npm*.</span><span class="sxs-lookup"><span data-stu-id="540a1-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="540a1-140">Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="540a1-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="540a1-141">Kopírování *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.</span><span class="sxs-lookup"><span data-stu-id="540a1-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="540a1-142">Vytvoření centra SignalR</span><span class="sxs-lookup"><span data-stu-id="540a1-142">Create the SignalR Hub</span></span>

<span data-ttu-id="540a1-143">Rozbočovač je třída, která slouží jako podrobný kanál, který umožňuje klientovi a serveru, volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="540a1-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540a1-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540a1-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="540a1-145">Do projektu přidejte třídu výběrem **soubor** > **nový** > **soubor** a výběrem **Visual C# – třída**.</span><span class="sxs-lookup"><span data-stu-id="540a1-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="540a1-146">Název souboru *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="540a1-146">Name the file *ChatHub*.</span></span>

2. <span data-ttu-id="540a1-147">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="540a1-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="540a1-148">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i přijímající a odesílající data.</span><span class="sxs-lookup"><span data-stu-id="540a1-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="540a1-149">Vytvořte `SendMessage` metoda, která odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="540a1-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="540a1-150">Všimněte si, vrátí hodnotu [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="540a1-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="540a1-151">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="540a1-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540a1-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540a1-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="540a1-153">Otevřete *SignalRChat* složky ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="540a1-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="540a1-154">Do projektu přidejte třídu výběrem **soubor** > **nový soubor** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="540a1-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="540a1-155">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="540a1-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="540a1-156">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupin, jakož i příjem a odesílání dat do klientů.</span><span class="sxs-lookup"><span data-stu-id="540a1-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="540a1-157">Přidat `SendMessage` metody pro třídu.</span><span class="sxs-lookup"><span data-stu-id="540a1-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="540a1-158">`SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="540a1-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="540a1-159">Všimněte si, vrátí hodnotu [úloh](/dotnet/api/system.threading.tasks.task), protože SignalR je asynchronní.</span><span class="sxs-lookup"><span data-stu-id="540a1-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="540a1-160">Asynchronní kódu poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="540a1-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="540a1-161">Konfigurace projektu pro použití funkce SignalR</span><span class="sxs-lookup"><span data-stu-id="540a1-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="540a1-162">SignalR server musí být konfigurován tak, aby věděl, že může předat požadavky SignalR.</span><span class="sxs-lookup"><span data-stu-id="540a1-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="540a1-163">Chcete-li konfigurovat projekt SignalR, změňte projektu `Startup.ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="540a1-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="540a1-164">`services.AddSignalR` Přidá SignalR jako součást [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="540a1-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="540a1-165">Konfigurace směrování, aby vaše centra pomocí `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="540a1-165">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="540a1-166">Vytvoření kódu klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="540a1-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="540a1-167">Přidejte soubor JavaScript s názvem *chat.js*do *wwwroot\js* složky.</span><span class="sxs-lookup"><span data-stu-id="540a1-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="540a1-168">Přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="540a1-168">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="540a1-169">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="540a1-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="540a1-170">Předchozí HTML zobrazí název a zpráva pole a tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="540a1-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="540a1-171">Všimněte si odkazům na skript v dolní části: odkaz na SignalR a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="540a1-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="540a1-172">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="540a1-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="540a1-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="540a1-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="540a1-174">Vyberte **ladění** > **spustit bez ladění** a spustí prohlížeč a nahrajte webu místně.</span><span class="sxs-lookup"><span data-stu-id="540a1-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="540a1-175">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="540a1-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="540a1-176">Otevřete jinou instanci prohlížeče (libovolného prohlížeče) a vložte adresu URL na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="540a1-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="540a1-177">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="540a1-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="540a1-178">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="540a1-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="540a1-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="540a1-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="540a1-180">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="540a1-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="540a1-181">Spuštění programu otevře okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="540a1-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="540a1-182">Otevře další okno prohlížeče a webu místně v zatížení.</span><span class="sxs-lookup"><span data-stu-id="540a1-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="540a1-183">Vyberte buď prohlížeče, zadejte název a zpráv a klikněte **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="540a1-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="540a1-184">Název a zpráva se zobrazí na obou stránkách okamžitě.</span><span class="sxs-lookup"><span data-stu-id="540a1-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Řešení](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="540a1-186">Související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="540a1-186">Related resources</span></span>

[<span data-ttu-id="540a1-187">Úvod do základní ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="540a1-187">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
