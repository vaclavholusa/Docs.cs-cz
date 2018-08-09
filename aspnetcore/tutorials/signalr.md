---
title: 'Kurz: Začínáme s funkce SignalR technologie ASP.NET Core'
author: tdykstra
description: V tomto kurzu vytvoříte aplikaci chatu, který používá funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/08/2018
uid: tutorials/signalr
ms.openlocfilehash: 2c1c46b4a608eb0d39287a5261ed7c1f847a644e
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722474"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="d139c-103">Kurz: Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d139c-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="d139c-104">V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR.</span><span class="sxs-lookup"><span data-stu-id="d139c-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="d139c-105">Získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="d139c-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d139c-106">Vytvoření webové aplikace, která používá SignalR v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d139c-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="d139c-107">Vytvoření rozbočovače SignalR na serveru.</span><span class="sxs-lookup"><span data-stu-id="d139c-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="d139c-108">Připojte se k rozbočovači SignalR z klientů JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d139c-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="d139c-109">Pomocí centra pro odesílání zpráv z libovolného klienta na všechny připojené klienty.</span><span class="sxs-lookup"><span data-stu-id="d139c-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="d139c-110">Na konci budete mít funkční aplikaci chatu:</span><span class="sxs-lookup"><span data-stu-id="d139c-110">At the end you'll have a working chat app:</span></span>

![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="d139c-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d139c-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d139c-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d139c-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d139c-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d139c-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d139c-115">[Visual Studio 2017 verze 15.7.3 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="d139c-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="d139c-116">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d139c-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d139c-117">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)</span><span class="sxs-lookup"><span data-stu-id="d139c-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d139c-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d139c-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="d139c-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d139c-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="d139c-120">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="d139c-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="d139c-121">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d139c-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="d139c-122">[npm](https://www.npmjs.com/get-npm) (Správce balíčků pro Node.js, použít klientskou knihovnou SignalR JavaScript)</span><span class="sxs-lookup"><span data-stu-id="d139c-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="d139c-123">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="d139c-123">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d139c-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d139c-124">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d139c-125">V nabídce vyberte **soubor > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="d139c-125">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="d139c-126">V **nový projekt** dialogového okna, vyberte **nainstalováno > Visual C# > Web > Webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d139c-126">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d139c-127">Pojmenujte projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="d139c-127">Name the project *SignalRChat*.</span></span>

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="d139c-129">Vyberte **webovou aplikaci** vytvořit projekt, který se používá pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="d139c-129">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="d139c-130">Ujistěte se, že Cílová architektura, která je **ASP.NET Core 2.1**a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="d139c-130">Make sure that the target framework is **ASP.NET Core 2.1**, and then select **OK**.</span></span> 

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d139c-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d139c-132">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d139c-133">Otevřete složku, která můžete použít pro nový projekt.</span><span class="sxs-lookup"><span data-stu-id="d139c-133">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="d139c-134">V **integrovaný terminál**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d139c-134">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="d139c-135">Přidat klientské knihovně SignalR</span><span class="sxs-lookup"><span data-stu-id="d139c-135">Add the SignalR client library</span></span>

<span data-ttu-id="d139c-136">Je součástí serveru knihovny SignalR [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d139c-136">The SignalR server library is included in the [Microsoft.AspnetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="d139c-137">Ale budete muset získat Javascriptovou klientskou knihovnu z npm, Správce balíčků Node.js.</span><span class="sxs-lookup"><span data-stu-id="d139c-137">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d139c-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d139c-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d139c-139">V **Konzola správce balíčků** (PMC), přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="d139c-139">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d139c-140">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d139c-140">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="d139c-141">Přejděte do nové složky projektu.</span><span class="sxs-lookup"><span data-stu-id="d139c-141">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

---

* <span data-ttu-id="d139c-142">Spustit inicializátor npm. Chcete-li vytvořit *package.json* souboru:</span><span class="sxs-lookup"><span data-stu-id="d139c-142">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="d139c-143">Příkaz vytvoří výstup podobný následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="d139c-143">The command creates output similar to the following example:</span></span>

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

* <span data-ttu-id="d139c-144">Nainstalujte balíček knihovny klienta:</span><span class="sxs-lookup"><span data-stu-id="d139c-144">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="d139c-145">Příkaz vytvoří výstup podobný následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="d139c-145">The command creates output similar to the following example:</span></span>

  ```
  npm : npm notice created a lockfile as package-lock.json. You should commit this file.
  At line:1 char:1
  + npm install @aspnet/signalr
  + ~~~~~~~~~~~~~~~~~~~~~~~~~~~
      + CategoryInfo          : NotSpecified: (npm notice crea...mmit this file.:String) [], RemoteException
      + FullyQualifiedErrorId : NativeCommandError
  WARN
   SignalRChat@1.0.0 No description
  WARN
   SignalRChat@1.0.0 No repository field.
  + @aspnet/signalr@1.0.2
  added 1 package in 1.398s
  ```

<span data-ttu-id="d139c-146">`npm install` Příkaz stáhli Javascriptovou klientskou knihovnu pro podsložku *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="d139c-146">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="d139c-147">Zkopírujte z něj do složky v části *wwwroot* , kterou můžete odkazovat z webové stránky chatovací aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d139c-147">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="d139c-148">Vytvoření *signalr* složky *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="d139c-148">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="d139c-149">Kopírovat *signalr.js* souboru z *node_modules/@aspnet/signalr/dist/browser* k novému *wwwroot/lib/signalr* složky.</span><span class="sxs-lookup"><span data-stu-id="d139c-149">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="d139c-150">Vytvoření rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="d139c-150">Create the SignalR hub</span></span>

<span data-ttu-id="d139c-151">A [centra](xref:signalr/hubs) je třída, která slouží jako základní kanál, který zpracovává vnitřní komunikaci klient server.</span><span class="sxs-lookup"><span data-stu-id="d139c-151">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="d139c-152">Ve složce projektu SignalRChat vytvořit *rozbočovače* složky.</span><span class="sxs-lookup"><span data-stu-id="d139c-152">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="d139c-153">V *rozbočovače* složku, vytvořte *ChatHub.cs* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d139c-153">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="d139c-154">`ChatHub` Třída dědí z funkce SignalR [centra](/dotnet/api/microsoft.aspnetcore.signalr.hub) třídy.</span><span class="sxs-lookup"><span data-stu-id="d139c-154">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="d139c-155">`Hub` Třída spravuje připojení, skupiny a zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="d139c-155">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="d139c-156">`SendMessage` Metoda může být volán všech připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="d139c-156">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="d139c-157">Přijaté zprávy odešle všem klientům.</span><span class="sxs-lookup"><span data-stu-id="d139c-157">It sends the received message to all clients.</span></span> <span data-ttu-id="d139c-158">Kód SignalR je asynchronní zajistit maximální škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="d139c-158">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="d139c-159">Nakonfigurujte projekt tak, aby používaly SignalR</span><span class="sxs-lookup"><span data-stu-id="d139c-159">Configure the project to use SignalR</span></span>

<span data-ttu-id="d139c-160">Na serveru funkce SignalR nastavené předat požadavky SignalR SignalR.</span><span class="sxs-lookup"><span data-stu-id="d139c-160">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="d139c-161">Přidejte následující zvýrazněný kód do *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="d139c-161">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="d139c-162">SignalR pro přidání těchto změn [injektáž závislostí](xref:fundamentals/dependency-injection) systému a [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="d139c-162">These changes add SignalR to the the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="d139c-163">Vytvořit kód klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="d139c-163">Create the SignalR client code</span></span>

* <span data-ttu-id="d139c-164">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d139c-164">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="d139c-165">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="d139c-165">The preceding code:</span></span>

  * <span data-ttu-id="d139c-166">Vytvoří textová pole pro název a text zprávy a tlačítka Odeslat.</span><span class="sxs-lookup"><span data-stu-id="d139c-166">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="d139c-167">Vytvoří seznam s `id="messagesList"` pro zobrazení zprávy, které byly přijaty z rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="d139c-167">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="d139c-168">Obsahuje odkazy na skript ke knihovně SignalR a *chat.js* aplikační kód, který vytvoříte v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d139c-168">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="d139c-169">V *wwwroot/js* složku, vytvořte *chat.js* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d139c-169">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="d139c-170">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="d139c-170">The preceding code:</span></span>

  * <span data-ttu-id="d139c-171">Vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="d139c-171">Creates and starts a connection.</span></span>
  * <span data-ttu-id="d139c-172">Přidá tlačítko Odeslat obslužnou rutinu, která odesílá zprávy do centra.</span><span class="sxs-lookup"><span data-stu-id="d139c-172">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="d139c-173">Přidá objekt připojení obslužnou rutinu, která přijímá zprávy z centra a přidá je do seznamu.</span><span class="sxs-lookup"><span data-stu-id="d139c-173">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d139c-174">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d139c-174">Run the app</span></span>

* <span data-ttu-id="d139c-175">Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="d139c-175">Press **CTRL+F5** to run the app without debugging.</span></span>

* <span data-ttu-id="d139c-176">Zkopírujte adresu URL z adresního řádku, otevřete jinou instanci prohlížeče nebo karty a vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="d139c-176">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="d139c-177">Zvolte buď prohlížeče, zadejte název a zprávu a vybrat **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d139c-177">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="d139c-178">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d139c-178">The name and message are displayed on both pages instantly.</span></span>

  ![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="d139c-180">Pokud aplikace nebude fungovat, otevřete prohlížeč vývojářské nástroje (F12) a přejděte do konzoly.</span><span class="sxs-lookup"><span data-stu-id="d139c-180">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="d139c-181">Můžou se zobrazovat chyby související s kódem HTML a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d139c-181">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="d139c-182">Předpokládejme například, kam si ukládáte *signalr.js* v jiné složce než řízené.</span><span class="sxs-lookup"><span data-stu-id="d139c-182">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="d139c-183">V takovém případě nebude fungovat odkaz na tento soubor a uvidíte v konzole chybu 404.</span><span class="sxs-lookup"><span data-stu-id="d139c-183">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="d139c-184">![Chyba nenalezení signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="d139c-184">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d139c-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d139c-185">Next steps</span></span>

<span data-ttu-id="d139c-186">Pokud chcete, aby klienti mohli připojovat k aplikaci s knihovnou SignalR z jiných domén, budete muset povolit sdílení prostředků mezi zdroji (CORS).</span><span class="sxs-lookup"><span data-stu-id="d139c-186">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="d139c-187">Další informace najdete v tématu [sdílení prostředků různého původu](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="d139c-187">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="d139c-188">Další informace o funkci SignalR, rozbočovačů a klientů JavaScript naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="d139c-188">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="d139c-189">Úvod ke knihovně SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d139c-189">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="d139c-190">Použití rozbočovače signalr pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d139c-190">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d139c-191">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="d139c-191">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
