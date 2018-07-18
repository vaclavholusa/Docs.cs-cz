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
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="4beea-103">Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4beea-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="4beea-104">V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4beea-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Řešení](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="4beea-106">Tento kurz demonstruje následující úkoly vývoje SignalR:</span><span class="sxs-lookup"><span data-stu-id="4beea-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4beea-107">Vytvoření knihovnou SignalR ve webové aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4beea-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="4beea-108">Vytvoření rozbočovače SignalR tak, aby nabízel obsah pro klienty.</span><span class="sxs-lookup"><span data-stu-id="4beea-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="4beea-109">Upravit `Startup` třídy a konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="4beea-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="4beea-110">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4beea-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4beea-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4beea-111">Prerequisites</span></span>

<span data-ttu-id="4beea-112">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="4beea-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4beea-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4beea-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="4beea-114">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4beea-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="4beea-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7.3 nebo novější s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="4beea-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="4beea-116">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js)</span><span class="sxs-lookup"><span data-stu-id="4beea-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4beea-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4beea-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="4beea-118">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4beea-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="4beea-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4beea-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="4beea-120">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4beea-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="4beea-121">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js)</span><span class="sxs-lookup"><span data-stu-id="4beea-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="4beea-122">Vytvoření projektu aplikace ASP.NET Core, který je hostitelem SignalR klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="4beea-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4beea-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4beea-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="4beea-124">Použití **souboru** > **nový projekt** nabídku a vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4beea-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4beea-125">Pojmenujte projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="4beea-125">Name the project *SignalRChat*.</span></span>

   ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="4beea-127">Vyberte **webovou aplikaci** vytvoření projektu pomocí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="4beea-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="4beea-128">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4beea-128">Then select **OK**.</span></span> <span data-ttu-id="4beea-129">Ujistěte se, že **ASP.NET Core 2.1** vyberete ze selektoru rozhraní framework, i když SignalR běží na starší verze rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="4beea-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="4beea-131">Visual Studio obsahuje `Microsoft.AspNetCore.SignalR` balíček, který obsahuje jeho server knihoven jako součást jeho **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="4beea-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="4beea-132">Nicméně knihovny JavaScript klienta pro funkci SignalR musí být nainstalovaný pomocí *npm*.</span><span class="sxs-lookup"><span data-stu-id="4beea-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="4beea-133">Spuštěním následujících příkazů **Konzola správce balíčků** okna z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="4beea-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="4beea-134">Vytvořte novou složku s názvem "signalr" uvnitř *wwwroot/lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="4beea-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="4beea-135">Kopírovat *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.</span><span class="sxs-lookup"><span data-stu-id="4beea-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4beea-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4beea-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="4beea-137">Z **integrovaný terminál**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4beea-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="4beea-138">Nainstalovat s použitím knihovny JavaScript klienta *npm*.</span><span class="sxs-lookup"><span data-stu-id="4beea-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="4beea-139">Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="4beea-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="4beea-140">Kopírovat *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.</span><span class="sxs-lookup"><span data-stu-id="4beea-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="4beea-141">Vytvoření rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="4beea-141">Create the SignalR Hub</span></span>

<span data-ttu-id="4beea-142">Centrum je třída, která slouží jako základní kanál, který umožňuje klientovi i serveru, volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="4beea-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4beea-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4beea-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="4beea-144">Přidání třídy do projektu výběrem **souboru** > **nový** > **souboru** a vyberete **třídy Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="4beea-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="4beea-145">Název třídy `ChatHub` a soubor *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="4beea-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="4beea-146">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="4beea-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="4beea-147">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupiny, jakož i příjem a odesílání data.</span><span class="sxs-lookup"><span data-stu-id="4beea-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="4beea-148">Vytvořte `SendMessage` metodu, která odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="4beea-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="4beea-149">Všimněte si, že ji vrací [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), protože je asynchronní funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="4beea-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="4beea-150">Asynchronní kód poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="4beea-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4beea-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4beea-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="4beea-152">Otevřít *SignalRChat* složky ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4beea-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="4beea-153">Přidání třídy do projektu tak, že vyberete **souboru** > **nový soubor** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="4beea-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="4beea-154">Název třídy `ChatHub` a soubor *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="4beea-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="4beea-155">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="4beea-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="4beea-156">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupiny, jakož i příjem a odesílání dat do klientů.</span><span class="sxs-lookup"><span data-stu-id="4beea-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="4beea-157">Přidat `SendMessage` metodu do třídy.</span><span class="sxs-lookup"><span data-stu-id="4beea-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="4beea-158">`SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="4beea-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="4beea-159">Všimněte si, že ji vrací [úloh](/dotnet/api/system.threading.tasks.task), protože je asynchronní funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="4beea-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="4beea-160">Asynchronní kód poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="4beea-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="4beea-161">Nakonfigurujte projekt tak, aby používaly SignalR</span><span class="sxs-lookup"><span data-stu-id="4beea-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="4beea-162">SignalR server musí nakonfigurovat tak, aby věděl, že může předat požadavky SignalR.</span><span class="sxs-lookup"><span data-stu-id="4beea-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="4beea-163">Chcete-li nakonfigurovat projekt SignalR, upravte projektu `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="4beea-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="4beea-164">`services.AddSignalR` je k dispozici pro služby SignalR [injektáž závislostí](xref:fundamentals/dependency-injection) systému.</span><span class="sxs-lookup"><span data-stu-id="4beea-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="4beea-165">Konfigurují trasy pro vaše uzlům `UseSignalR` v `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="4beea-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="4beea-166">`app.UseSignalR` Přidá připojení SignalR pro [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="4beea-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="4beea-167">Vytvořit kód klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="4beea-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="4beea-168">Přidejte soubor JavaScriptu s názvem *chat.js*, *wwwroot\js* složky.</span><span class="sxs-lookup"><span data-stu-id="4beea-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="4beea-169">Přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="4beea-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="4beea-170">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4beea-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="4beea-171">Předchozí kód HTML zobrazí název a zprávu pole a tlačítka Odeslat.</span><span class="sxs-lookup"><span data-stu-id="4beea-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="4beea-172">Všimněte si, že odkazy na skript v dolní části: referenční dokumentace ke knihovně SignalR a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="4beea-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4beea-173">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="4beea-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4beea-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4beea-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4beea-175">Vyberte **ladění** > **spustit bez ladění** spusťte prohlížeč a zatížení webu místně.</span><span class="sxs-lookup"><span data-stu-id="4beea-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="4beea-176">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="4beea-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="4beea-177">Otevřete jiná instance prohlížeče (libovolného prohlížeče) a vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="4beea-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="4beea-178">Zvolte buď prohlížeče, zadejte název a zprávu a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4beea-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="4beea-179">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4beea-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4beea-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4beea-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="4beea-181">Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program.</span><span class="sxs-lookup"><span data-stu-id="4beea-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="4beea-182">Spuštění programu se otevře okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4beea-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="4beea-183">Otevřete další okno prohlížeče a zatížení webu místně v ní.</span><span class="sxs-lookup"><span data-stu-id="4beea-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="4beea-184">Zvolte buď prohlížeče, zadejte název a zprávu a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4beea-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="4beea-185">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4beea-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Řešení](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="4beea-187">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="4beea-187">Related resources</span></span>

[<span data-ttu-id="4beea-188">Úvod do ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4beea-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
