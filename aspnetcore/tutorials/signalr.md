---
title: 'Kurz: Začínáme s funkce SignalR technologie ASP.NET Core'
author: tdykstra
description: V tomto kurzu vytvoříte aplikaci chatu, který používá funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/20/2018
ms.locfileid: "41757198"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="4f5fe-103">Kurz: Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f5fe-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="4f5fe-104">V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="4f5fe-105">Získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4f5fe-106">Vytvoření webové aplikace, která používá SignalR v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="4f5fe-107">Vytvoření rozbočovače SignalR na serveru.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="4f5fe-108">Připojte se k rozbočovači SignalR z klientů JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="4f5fe-109">Pomocí centra pro odesílání zpráv z libovolného klienta na všechny připojené klienty.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="4f5fe-110">Na konci budete mít funkční aplikaci chatu:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-110">At the end, you'll have a working chat app:</span></span>

![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="4f5fe-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f5fe-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f5fe-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5fe-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5fe-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f5fe-115">[Visual Studio 2017 verze 15.7.3 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="4f5fe-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="4f5fe-116">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4f5fe-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="4f5fe-117">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)</span><span class="sxs-lookup"><span data-stu-id="4f5fe-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5fe-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f5fe-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="4f5fe-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f5fe-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="4f5fe-120">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4f5fe-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="4f5fe-121">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f5fe-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="4f5fe-122">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)</span><span class="sxs-lookup"><span data-stu-id="4f5fe-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5fe-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f5fe-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="4f5fe-124">Visual Studio pro Mac verze 7.5.4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="4f5fe-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="4f5fe-125">[.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all) (zahrnuty v instalaci sady Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="4f5fe-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="4f5fe-126">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)</span><span class="sxs-lookup"><span data-stu-id="4f5fe-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="4f5fe-127">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="4f5fe-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5fe-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5fe-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="4f5fe-129">V nabídce vyberte **soubor > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="4f5fe-130">V **nový projekt** dialogového okna, vyberte **nainstalováno > Visual C# > Web > Webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4f5fe-131">Pojmenujte projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-131">Name the project *SignalRChat*.</span></span>

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="4f5fe-133">Vyberte **webovou aplikaci** vytvořit projekt, který se používá pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="4f5fe-134">Vyberte cílovou architekturu **.NET Core**vyberte **ASP.NET Core 2.1**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5fe-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f5fe-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="4f5fe-137">Otevřete složku, která můžete použít pro nový projekt.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="4f5fe-138">V **integrovaný terminál**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5fe-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f5fe-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f5fe-140">V nabídce vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="4f5fe-141">Vyberte **.NET Core > aplikace > webové aplikace ASP.NET Core** (nevybírejte **ASP.NET Core webové aplikace (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="4f5fe-142">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-142">Select **Next**.</span></span>

* <span data-ttu-id="4f5fe-143">Pojmenujte projekt *SignalRChat*a pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="4f5fe-144">Přidat klientské knihovně SignalR</span><span class="sxs-lookup"><span data-stu-id="4f5fe-144">Add the SignalR client library</span></span>

<span data-ttu-id="4f5fe-145">Je součástí serveru knihovny SignalR [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4f5fe-146">Ale budete muset získat Javascriptovou klientskou knihovnu z npm, Správce balíčků Node.js.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-146">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5fe-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5fe-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="4f5fe-148">V **Konzola správce balíčků** (PMC), přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5fe-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f5fe-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="4f5fe-150">Přejděte do nové složky projektu.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5fe-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f5fe-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f5fe-152">V **terminálu**, přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="4f5fe-153">Spustit inicializátor npm. Chcete-li vytvořit *package.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="4f5fe-154">Příkaz vytvoří výstup podobný následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="4f5fe-155">Nainstalujte balíček knihovny klienta:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="4f5fe-156">Příkaz vytvoří výstup podobný následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="4f5fe-157">`npm install` Příkaz stáhli Javascriptovou klientskou knihovnu pro podsložku *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="4f5fe-158">Zkopírujte z něj do složky v části *wwwroot* , kterou můžete odkazovat z webové stránky chatovací aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="4f5fe-159">Vytvoření *signalr* složky *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="4f5fe-160">Kopírovat *signalr.js* souboru z *node_modules/@aspnet/signalr/dist/browser* k novému *wwwroot/lib/signalr* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="4f5fe-161">Vytvoření rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="4f5fe-161">Create the SignalR hub</span></span>

<span data-ttu-id="4f5fe-162">A [centra](xref:signalr/hubs) je třída, která slouží jako základní kanál, který zpracovává vnitřní komunikaci klient server.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="4f5fe-163">Ve složce projektu SignalRChat vytvořit *rozbočovače* složky.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="4f5fe-164">V *rozbočovače* složku, vytvořte *ChatHub.cs* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="4f5fe-165">`ChatHub` Třída dědí z funkce SignalR [centra](/dotnet/api/microsoft.aspnetcore.signalr.hub) třídy.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="4f5fe-166">`Hub` Třída spravuje připojení, skupiny a zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="4f5fe-167">`SendMessage` Metoda může být volán všech připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="4f5fe-168">Přijaté zprávy odešle všem klientům.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-168">It sends the received message to all clients.</span></span> <span data-ttu-id="4f5fe-169">Kód SignalR je asynchronní zajistit maximální škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="4f5fe-170">Nakonfigurujte projekt tak, aby používaly SignalR</span><span class="sxs-lookup"><span data-stu-id="4f5fe-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="4f5fe-171">Na serveru funkce SignalR nastavené předat požadavky SignalR SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="4f5fe-172">Přidejte následující zvýrazněný kód do *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="4f5fe-173">SignalR pro přidání těchto změn [injektáž závislostí](xref:fundamentals/dependency-injection) systému a [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="4f5fe-174">Vytvořit kód klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="4f5fe-174">Create the SignalR client code</span></span>

* <span data-ttu-id="4f5fe-175">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="4f5fe-176">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-176">The preceding code:</span></span>

  * <span data-ttu-id="4f5fe-177">Vytvoří textová pole pro název a text zprávy a tlačítka Odeslat.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="4f5fe-178">Vytvoří seznam s `id="messagesList"` pro zobrazení zprávy, které byly přijaty z rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="4f5fe-179">Obsahuje odkazy na skript ke knihovně SignalR a *chat.js* aplikační kód, který vytvoříte v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="4f5fe-180">V *wwwroot/js* složku, vytvořte *chat.js* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="4f5fe-181">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-181">The preceding code:</span></span>

  * <span data-ttu-id="4f5fe-182">Vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="4f5fe-183">Přidá tlačítko Odeslat obslužnou rutinu, která odesílá zprávy do centra.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="4f5fe-184">Přidá objekt připojení obslužnou rutinu, která přijímá zprávy z centra a přidá je do seznamu.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4f5fe-185">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="4f5fe-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f5fe-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f5fe-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f5fe-187">Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f5fe-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4f5fe-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4f5fe-189">Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f5fe-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4f5fe-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="4f5fe-191">V nabídce vyberte **spuštění > Spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="4f5fe-192">Zkopírujte adresu URL z adresního řádku, otevřete jinou instanci prohlížeče nebo karty a vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="4f5fe-193">Zvolte buď prohlížeče, zadejte název a zprávu a vybrat **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="4f5fe-194">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-194">The name and message are displayed on both pages instantly.</span></span>

  ![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="4f5fe-196">Pokud aplikace nebude fungovat, otevřete prohlížeč vývojářské nástroje (F12) a přejděte do konzoly.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="4f5fe-197">Můžou se zobrazovat chyby související s kódem HTML a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="4f5fe-198">Předpokládejme například, kam si ukládáte *signalr.js* v jiné složce než řízené.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="4f5fe-199">V takovém případě nebude fungovat odkaz na tento soubor a uvidíte v konzole chybu 404.</span><span class="sxs-lookup"><span data-stu-id="4f5fe-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="4f5fe-200">![Chyba nenalezení signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="4f5fe-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f5fe-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f5fe-201">Next steps</span></span>

<span data-ttu-id="4f5fe-202">Pokud chcete, aby klienti mohli připojovat k aplikaci s knihovnou SignalR z jiných domén, budete muset povolit sdílení prostředků mezi zdroji (CORS).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="4f5fe-203">Další informace najdete v tématu [sdílení prostředků různého původu](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="4f5fe-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="4f5fe-204">Další informace o funkci SignalR, rozbočovačů a klientů JavaScript naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="4f5fe-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="4f5fe-205">Úvod ke knihovně SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f5fe-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="4f5fe-206">Použití rozbočovače signalr pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f5fe-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4f5fe-207">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="4f5fe-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
