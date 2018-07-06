---
title: Začínáme s funkce SignalR technologie ASP.NET Core
author: rachelappel
description: V tomto kurzu vytvoříte aplikaci pomocí nástroje SignalR pro ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830552"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="4755f-103">Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4755f-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="4755f-104">Podle [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="4755f-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="4755f-105">V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4755f-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Řešení](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="4755f-107">Tento kurz demonstruje následující úkoly vývoje SignalR:</span><span class="sxs-lookup"><span data-stu-id="4755f-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4755f-108">Vytvoření knihovnou SignalR ve webové aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4755f-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="4755f-109">Vytvoření rozbočovače SignalR tak, aby nabízel obsah pro klienty.</span><span class="sxs-lookup"><span data-stu-id="4755f-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="4755f-110">Upravit `Startup` třídy a konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="4755f-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="4755f-111">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4755f-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4755f-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4755f-112">Prerequisites</span></span>

<span data-ttu-id="4755f-113">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="4755f-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4755f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4755f-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="4755f-115">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4755f-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="4755f-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15.7.3 nebo novější s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="4755f-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="4755f-117">npm</span><span class="sxs-lookup"><span data-stu-id="4755f-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4755f-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4755f-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="4755f-119">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4755f-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="4755f-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4755f-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="4755f-121">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4755f-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="4755f-122">npm</span><span class="sxs-lookup"><span data-stu-id="4755f-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="4755f-123">Vytvoření projektu aplikace ASP.NET Core, který je hostitelem SignalR klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="4755f-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4755f-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4755f-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="4755f-125">Použití **souboru** > **nový projekt** nabídku a vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4755f-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4755f-126">Pojmenujte projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="4755f-126">Name the project *SignalRChat*.</span></span>

   ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="4755f-128">Vyberte **webovou aplikaci** vytvoření projektu pomocí stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="4755f-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="4755f-129">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="4755f-129">Then select **OK**.</span></span> <span data-ttu-id="4755f-130">Ujistěte se, že **ASP.NET Core 2.1** vyberete ze selektoru rozhraní framework, i když SignalR běží na starší verze rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="4755f-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="4755f-132">Visual Studio obsahuje `Microsoft.AspNetCore.SignalR` balíček, který obsahuje jeho server knihoven jako součást jeho **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="4755f-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="4755f-133">Nicméně knihovny JavaScript klienta pro funkci SignalR musí být nainstalovaný pomocí *npm*.</span><span class="sxs-lookup"><span data-stu-id="4755f-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="4755f-134">Spuštěním následujících příkazů **Konzola správce balíčků** okna z kořenového adresáře projektu:</span><span class="sxs-lookup"><span data-stu-id="4755f-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="4755f-135">Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="4755f-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="4755f-136">Kopírovat *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.</span><span class="sxs-lookup"><span data-stu-id="4755f-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4755f-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4755f-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="4755f-138">Z **integrovaný terminál**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4755f-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="4755f-139">Nainstalovat s použitím knihovny JavaScript klienta *npm*.</span><span class="sxs-lookup"><span data-stu-id="4755f-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="4755f-140">Vytvořte novou složku s názvem "signalr" uvnitř *lib* složku ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="4755f-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="4755f-141">Kopírovat *signalr.js* souboru z *node_modules\\ @aspnet\signalr\dist\browser*  do této složky.</span><span class="sxs-lookup"><span data-stu-id="4755f-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="4755f-142">Vytvoření rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="4755f-142">Create the SignalR Hub</span></span>

<span data-ttu-id="4755f-143">Centrum je třída, která slouží jako základní kanál, který umožňuje klientovi i serveru, volání metod na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="4755f-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4755f-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4755f-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="4755f-145">Přidání třídy do projektu výběrem **souboru** > **nový** > **souboru** a vyberete **třídy Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="4755f-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="4755f-146">Název třídy `ChatHub` a soubor *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="4755f-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="4755f-147">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="4755f-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="4755f-148">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupiny, jakož i příjem a odesílání data.</span><span class="sxs-lookup"><span data-stu-id="4755f-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="4755f-149">Vytvořte `SendMessage` metodu, která odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="4755f-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="4755f-150">Všimněte si, že ji vrací [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), protože je asynchronní funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="4755f-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="4755f-151">Asynchronní kód poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="4755f-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4755f-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4755f-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="4755f-153">Otevřít *SignalRChat* složky ve Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4755f-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="4755f-154">Přidání třídy do projektu tak, že vyberete **souboru** > **nový soubor** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="4755f-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="4755f-155">Název třídy `ChatHub` a soubor *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="4755f-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="4755f-156">Dědit z `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="4755f-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="4755f-157">`Hub` Třída obsahuje vlastnosti a události pro správu připojení a skupiny, jakož i příjem a odesílání dat do klientů.</span><span class="sxs-lookup"><span data-stu-id="4755f-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="4755f-158">Přidat `SendMessage` metodu do třídy.</span><span class="sxs-lookup"><span data-stu-id="4755f-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="4755f-159">`SendMessage` Metoda odešle zprávu do všech klientů připojených konverzace.</span><span class="sxs-lookup"><span data-stu-id="4755f-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="4755f-160">Všimněte si, že ji vrací [úloh](/dotnet/api/system.threading.tasks.task), protože je asynchronní funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="4755f-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="4755f-161">Asynchronní kód poskytuje lepší škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="4755f-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="4755f-162">Nakonfigurujte projekt tak, aby používaly SignalR</span><span class="sxs-lookup"><span data-stu-id="4755f-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="4755f-163">SignalR server musí nakonfigurovat tak, aby věděl, že může předat požadavky SignalR.</span><span class="sxs-lookup"><span data-stu-id="4755f-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="4755f-164">Chcete-li nakonfigurovat projekt SignalR, upravte projektu `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="4755f-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="4755f-165">`services.AddSignalR` je k dispozici pro služby SignalR [injektáž závislostí](xref:fundamentals/dependency-injection) systému.</span><span class="sxs-lookup"><span data-stu-id="4755f-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="4755f-166">Konfigurují trasy pro vaše uzlům `UseSignalR` v `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="4755f-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="4755f-167">`app.UseSignalR` Přidá připojení SignalR pro [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="4755f-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="4755f-168">Vytvořit kód klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="4755f-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="4755f-169">Přidejte soubor JavaScriptu s názvem *chat.js*, *wwwroot\js* složky.</span><span class="sxs-lookup"><span data-stu-id="4755f-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="4755f-170">Přidejte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="4755f-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="4755f-171">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4755f-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="4755f-172">Předchozí kód HTML zobrazí název a zprávu pole a tlačítka Odeslat.</span><span class="sxs-lookup"><span data-stu-id="4755f-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="4755f-173">Všimněte si, že odkazy na skript v dolní části: referenční dokumentace ke knihovně SignalR a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="4755f-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4755f-174">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="4755f-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4755f-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4755f-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4755f-176">Vyberte **ladění** > **spustit bez ladění** spusťte prohlížeč a zatížení webu místně.</span><span class="sxs-lookup"><span data-stu-id="4755f-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="4755f-177">Zkopírujte adresu URL z panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="4755f-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="4755f-178">Otevřete jiná instance prohlížeče (libovolného prohlížeče) a vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="4755f-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="4755f-179">Zvolte buď prohlížeče, zadejte název a zprávu a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4755f-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="4755f-180">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4755f-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4755f-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4755f-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="4755f-182">Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program.</span><span class="sxs-lookup"><span data-stu-id="4755f-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="4755f-183">Spuštění programu se otevře okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4755f-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="4755f-184">Otevřete další okno prohlížeče a zatížení webu místně v ní.</span><span class="sxs-lookup"><span data-stu-id="4755f-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="4755f-185">Zvolte buď prohlížeče, zadejte název a zprávu a klikněte na tlačítko **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4755f-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="4755f-186">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4755f-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![Řešení](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="4755f-188">Související prostředky</span><span class="sxs-lookup"><span data-stu-id="4755f-188">Related resources</span></span>

[<span data-ttu-id="4755f-189">Úvod do ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4755f-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
