---
title: 'Kurz: Začínáme s funkce SignalR technologie ASP.NET Core'
author: tdykstra
description: V tomto kurzu vytvoříte aplikaci chatu, který používá funkce SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6d96331a4630f766ca11edb056fd3e13b52b6ae4
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893162"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="8e59d-103">Kurz: Začínáme s funkce SignalR technologie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e59d-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="8e59d-104">V tomto kurzu se naučíte se základy vytváření aplikace v reálném čase pomocí nástroje SignalR.</span><span class="sxs-lookup"><span data-stu-id="8e59d-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="8e59d-105">Získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="8e59d-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8e59d-106">Vytvoření webové aplikace, která používá SignalR v ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e59d-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="8e59d-107">Vytvoření rozbočovače SignalR na serveru.</span><span class="sxs-lookup"><span data-stu-id="8e59d-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="8e59d-108">Připojte se k rozbočovači SignalR z klientů JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8e59d-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="8e59d-109">Pomocí centra pro odesílání zpráv z libovolného klienta na všechny připojené klienty.</span><span class="sxs-lookup"><span data-stu-id="8e59d-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="8e59d-110">Na konci budete mít funkční aplikaci chatu:</span><span class="sxs-lookup"><span data-stu-id="8e59d-110">At the end, you'll have a working chat app:</span></span>

![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="8e59d-112">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8e59d-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e59d-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8e59d-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e59d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e59d-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e59d-115">[Visual Studio 2017 verze 15,8 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro ASP.NET a web** pracovního vytížení</span><span class="sxs-lookup"><span data-stu-id="8e59d-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8e59d-116">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8e59d-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e59d-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e59d-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8e59d-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e59d-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8e59d-119">.NET core SDK 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8e59d-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8e59d-120">C# pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e59d-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e59d-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8e59d-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="8e59d-122">Visual Studio pro Mac verze 7.5.4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8e59d-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="8e59d-123">[.NET core SDK 2.1 nebo novější](https://www.microsoft.com/net/download/all) (zahrnuty v instalaci sady Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="8e59d-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="8e59d-124">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="8e59d-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e59d-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e59d-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="8e59d-126">V nabídce vyberte **soubor > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="8e59d-127">V **nový projekt** dialogového okna, vyberte **nainstalováno > Visual C# > Web > Webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8e59d-128">Pojmenujte projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="8e59d-128">Name the project *SignalRChat*.</span></span>

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="8e59d-130">Vyberte **webovou aplikaci** vytvořit projekt, který se používá pro stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="8e59d-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="8e59d-131">Vyberte cílovou architekturu **.NET Core**vyberte **ASP.NET Core 2.1**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Dialogové okno nového projektu v sadě Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e59d-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e59d-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="8e59d-134">Otevřete složku, která můžete použít pro nový projekt.</span><span class="sxs-lookup"><span data-stu-id="8e59d-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="8e59d-135">V **integrovaný terminál**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8e59d-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e59d-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8e59d-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e59d-137">V nabídce vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="8e59d-138">Vyberte **.NET Core > aplikace > webové aplikace ASP.NET Core** (nevybírejte **ASP.NET Core webové aplikace (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="8e59d-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="8e59d-139">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-139">Select **Next**.</span></span>

* <span data-ttu-id="8e59d-140">Pojmenujte projekt *SignalRChat*a pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="8e59d-141">Přidat klientské knihovně SignalR</span><span class="sxs-lookup"><span data-stu-id="8e59d-141">Add the SignalR client library</span></span>

<span data-ttu-id="8e59d-142">Je součástí serveru knihovny SignalR [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8e59d-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="8e59d-143">Knihovny JavaScript klienta není automaticky zahrnut v projektu.</span><span class="sxs-lookup"><span data-stu-id="8e59d-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="8e59d-144">V tomto kurzu použijete [Správce knihovny (LibMan)](xref:client-side/libman/index) získat klientské knihovny z *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="8e59d-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="8e59d-145">[unpkg](https://unpkg.com/#/) je [síť pro doručování obsahu](https://wikipedia.org/wiki/Content_delivery_network) , která může doručovat cokoli, co je součástí [npm, Správce balíčků Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="8e59d-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e59d-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e59d-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="8e59d-147">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **knihoven na straně klienta**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="8e59d-148">V **přidat knihovnu na straně klienta** dialogovém okně pro **poskytovatele** vyberte **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="8e59d-149">Pro **knihovny**, zadejte _@aspnet/signalr @1_a vyberte nejnovější verzi, která není ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="8e59d-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![Přidat dialog knihoven na straně klienta - knihovně](signalr/_static/libman1.png)

* <span data-ttu-id="8e59d-151">Vyberte **zvolte konkrétní soubory**, rozbalte *dist a prohlížeče* a pak zvolte položku *signalr.js* a *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="8e59d-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="8e59d-152">Nastavte **cílové umístění** k *wwwroot/lib/signalr/* a vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Přidat dialog knihoven na straně klienta – vyberte soubory a cíl](signalr/_static/libman2.png)

  <span data-ttu-id="8e59d-154">[LibMan](xref:client-side/libman/index) vytvoří *wwwroot/lib/signalr* složky a kopie vybrané soubory.</span><span class="sxs-lookup"><span data-stu-id="8e59d-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e59d-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e59d-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="8e59d-156">V **integrovaný terminál**, spusťte následující příkaz k instalaci LibMan.</span><span class="sxs-lookup"><span data-stu-id="8e59d-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="8e59d-157">Přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="8e59d-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="8e59d-158">Spusťte následující příkaz s použitím LibMan získat klientské knihovně SignalR.</span><span class="sxs-lookup"><span data-stu-id="8e59d-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="8e59d-159">Budete muset počkat několik sekund, než výstup.</span><span class="sxs-lookup"><span data-stu-id="8e59d-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="8e59d-160">Parametry určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="8e59d-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="8e59d-161">Použití zprostředkovatele unpkg.</span><span class="sxs-lookup"><span data-stu-id="8e59d-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="8e59d-162">Kopírovat soubory *wwwroot/lib/signalr* cíl.</span><span class="sxs-lookup"><span data-stu-id="8e59d-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="8e59d-163">Zkopírujte pouze zadané soubory.</span><span class="sxs-lookup"><span data-stu-id="8e59d-163">Copy only the specified files.</span></span>

  <span data-ttu-id="8e59d-164">Výstup bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="8e59d-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e59d-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8e59d-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e59d-166">V **terminálu**, spusťte následující příkaz k instalaci LibMan.</span><span class="sxs-lookup"><span data-stu-id="8e59d-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="8e59d-167">Přejděte do složky projektu (ten, který obsahuje *SignalRChat.csproj* souboru).</span><span class="sxs-lookup"><span data-stu-id="8e59d-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="8e59d-168">Spusťte následující příkaz s použitím LibMan získat klientské knihovně SignalR.</span><span class="sxs-lookup"><span data-stu-id="8e59d-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="8e59d-169">Parametry určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="8e59d-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="8e59d-170">Použití zprostředkovatele unpkg.</span><span class="sxs-lookup"><span data-stu-id="8e59d-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="8e59d-171">Kopírovat soubory *wwwroot/lib/signalr* cíl.</span><span class="sxs-lookup"><span data-stu-id="8e59d-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="8e59d-172">Zkopírujte pouze zadané soubory.</span><span class="sxs-lookup"><span data-stu-id="8e59d-172">Copy only the specified files.</span></span>

  <span data-ttu-id="8e59d-173">Výstup bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="8e59d-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="8e59d-174">Vytvoření rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="8e59d-174">Create the SignalR hub</span></span>

<span data-ttu-id="8e59d-175">A [centra](xref:signalr/hubs) je třída, která slouží jako základní kanál, který zpracovává vnitřní komunikaci klient server.</span><span class="sxs-lookup"><span data-stu-id="8e59d-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="8e59d-176">Ve složce projektu SignalRChat vytvořit *rozbočovače* složky.</span><span class="sxs-lookup"><span data-stu-id="8e59d-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="8e59d-177">V *rozbočovače* složku, vytvořte *ChatHub.cs* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8e59d-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="8e59d-178">`ChatHub` Třída dědí z funkce SignalR [centra](/dotnet/api/microsoft.aspnetcore.signalr.hub) třídy.</span><span class="sxs-lookup"><span data-stu-id="8e59d-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="8e59d-179">`Hub` Třída spravuje připojení, skupiny a zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="8e59d-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="8e59d-180">`SendMessage` Metoda může být volán všech připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="8e59d-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="8e59d-181">Přijaté zprávy odešle všem klientům.</span><span class="sxs-lookup"><span data-stu-id="8e59d-181">It sends the received message to all clients.</span></span> <span data-ttu-id="8e59d-182">Kód SignalR je asynchronní zajistit maximální škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="8e59d-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="8e59d-183">Nakonfigurujte projekt tak, aby používaly SignalR</span><span class="sxs-lookup"><span data-stu-id="8e59d-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="8e59d-184">Na serveru funkce SignalR nastavené předat požadavky SignalR SignalR.</span><span class="sxs-lookup"><span data-stu-id="8e59d-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="8e59d-185">Přidejte následující zvýrazněný kód do *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="8e59d-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="8e59d-186">SignalR pro přidání těchto změn [injektáž závislostí](xref:fundamentals/dependency-injection) systému a [middleware](xref:fundamentals/middleware/index) kanálu.</span><span class="sxs-lookup"><span data-stu-id="8e59d-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="8e59d-187">Vytvořit kód klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="8e59d-187">Create the SignalR client code</span></span>

* <span data-ttu-id="8e59d-188">Nahraďte obsah *Pages\Index.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8e59d-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="8e59d-189">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="8e59d-189">The preceding code:</span></span>

  * <span data-ttu-id="8e59d-190">Vytvoří textová pole pro název a text zprávy a tlačítka Odeslat.</span><span class="sxs-lookup"><span data-stu-id="8e59d-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="8e59d-191">Vytvoří seznam s `id="messagesList"` pro zobrazení zprávy, které byly přijaty z rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="8e59d-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="8e59d-192">Obsahuje odkazy na skript ke knihovně SignalR a *chat.js* aplikační kód, který vytvoříte v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="8e59d-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="8e59d-193">V *wwwroot/js* složku, vytvořte *chat.js* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8e59d-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="8e59d-194">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="8e59d-194">The preceding code:</span></span>

  * <span data-ttu-id="8e59d-195">Vytvoří a spustí připojení.</span><span class="sxs-lookup"><span data-stu-id="8e59d-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="8e59d-196">Přidá tlačítko Odeslat obslužnou rutinu, která odesílá zprávy do centra.</span><span class="sxs-lookup"><span data-stu-id="8e59d-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="8e59d-197">Přidá objekt připojení obslužnou rutinu, která přijímá zprávy z centra a přidá je do seznamu.</span><span class="sxs-lookup"><span data-stu-id="8e59d-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8e59d-198">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="8e59d-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e59d-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e59d-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e59d-200">Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="8e59d-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e59d-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e59d-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8e59d-202">Stisknutím klávesy **CTRL + F5** a spusťte tak aplikaci bez ladění.</span><span class="sxs-lookup"><span data-stu-id="8e59d-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e59d-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="8e59d-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e59d-204">V nabídce vyberte **spuštění > Spustit bez ladění**.</span><span class="sxs-lookup"><span data-stu-id="8e59d-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="8e59d-205">Zkopírujte adresu URL z adresního řádku, otevřete jinou instanci prohlížeče nebo karty a vložte adresu URL do adresního řádku.</span><span class="sxs-lookup"><span data-stu-id="8e59d-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="8e59d-206">Zvolte buď prohlížeče, zadejte název a zprávu a vybrat **odeslat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e59d-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="8e59d-207">Název a zpráva se zobrazí na obě stránky okamžitě.</span><span class="sxs-lookup"><span data-stu-id="8e59d-207">The name and message are displayed on both pages instantly.</span></span>

  ![Ukázková aplikace SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="8e59d-209">Pokud aplikace nebude fungovat, otevřete prohlížeč vývojářské nástroje (F12) a přejděte do konzoly.</span><span class="sxs-lookup"><span data-stu-id="8e59d-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="8e59d-210">Můžou se zobrazovat chyby související s kódem HTML a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8e59d-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="8e59d-211">Předpokládejme například, kam si ukládáte *signalr.js* v jiné složce než řízené.</span><span class="sxs-lookup"><span data-stu-id="8e59d-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="8e59d-212">V takovém případě nebude fungovat odkaz na tento soubor a uvidíte v konzole chybu 404.</span><span class="sxs-lookup"><span data-stu-id="8e59d-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="8e59d-213">![Chyba nenalezení signalr.js](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="8e59d-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e59d-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e59d-214">Next steps</span></span>

<span data-ttu-id="8e59d-215">Pokud chcete, aby klienti mohli připojovat k aplikaci s knihovnou SignalR z jiných domén, budete muset povolit sdílení prostředků mezi zdroji (CORS).</span><span class="sxs-lookup"><span data-stu-id="8e59d-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="8e59d-216">Další informace najdete v tématu [sdílení prostředků různého původu](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="8e59d-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="8e59d-217">Další informace o funkci SignalR, rozbočovačů a klientů JavaScript naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="8e59d-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="8e59d-218">Úvod ke knihovně SignalR pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e59d-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="8e59d-219">Použití rozbočovače signalr pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e59d-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8e59d-220">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="8e59d-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
