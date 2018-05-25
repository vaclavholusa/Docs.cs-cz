---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Kurz: Začínáme s SignalR 1.x a MVC 4 | Microsoft Docs'
author: pfletcher
description: Sestavení aplikace chat v reálném čase pomocí funkce SignalR technologie ASP.NET a ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="9ecc0-103">Kurz: Začínáme s SignalR 1.x a MVC 4</span><span class="sxs-lookup"><span data-stu-id="9ecc0-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="9ecc0-104">podle [Patrik Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="9ecc0-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="9ecc0-105">Tento kurz ukazuje, jak vytvořit v reálném čase chatovací aplikace pomocí funkce SignalR technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="9ecc0-106">SignalR přidáte do aplikace MVC 4 a vytvořit zobrazení chatu a odesílat zprávy o zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="9ecc0-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="9ecc0-107">Overview</span></span>

<span data-ttu-id="9ecc0-108">Tento kurz vás seznámí s vývoj aplikací webu v reálném čase pomocí funkce SignalR technologie ASP.NET a ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="9ecc0-109">Tento kurz používá stejný kód aplikace chat jako [kurzu Začínáme SignalR](tutorial-getting-started-with-signalr.md), ale ukazuje, jak přidat do aplikace MVC 4, který je založený na šabloně Internetu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="9ecc0-110">V tomto tématu se dozvíte následující úlohy vývoj SignalR:</span><span class="sxs-lookup"><span data-stu-id="9ecc0-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="9ecc0-111">Přidání knihovně SignalR do aplikace MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="9ecc0-112">Vytvoření třídy rozbočovače pro uložení obsahu do klientů.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="9ecc0-113">K odesílání zpráv a zobrazení aktualizací z centra pomocí knihovny jQuery SignalR na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="9ecc0-114">Následující snímek obrazovky ukazuje dokončené chatovací aplikace spuštěné v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instance chatu](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="9ecc0-116">Části:</span><span class="sxs-lookup"><span data-stu-id="9ecc0-116">Sections:</span></span>

- [<span data-ttu-id="9ecc0-117">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="9ecc0-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="9ecc0-118">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="9ecc0-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="9ecc0-119">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="9ecc0-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="9ecc0-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ecc0-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="9ecc0-121">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="9ecc0-121">Set up the Project</span></span>

<span data-ttu-id="9ecc0-122">Předpoklady:</span><span class="sxs-lookup"><span data-stu-id="9ecc0-122">Prerequisites:</span></span>

- <span data-ttu-id="9ecc0-123">Visual Studio 2010 SP1, Visual Studio 2012 nebo Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="9ecc0-124">Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2012 Express vývoj nástroj.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="9ecc0-125">Pro Visual Studio 2010, nainstalujte [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="9ecc0-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="9ecc0-126">V této části ukazuje, jak vytvořit aplikaci ASP.NET MVC 4, přidejte knihovně SignalR a vytvoření chatovací aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="9ecc0-127">V sadě Visual Studio vytvořit aplikaci ASP.NET MVC 4, název SignalRChat a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="9ecc0-128">V VS 2010, vyberte **rozhraní .NET Framework 4** v rozevíracím seznamu Framework verze.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="9ecc0-129">Spustí kód SignalR na rozhraní .NET Framework verze 4 a 4.5.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Vytvoření webové mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="9ecc0-131">Vyberte šablonu, internetovou aplikaci, zrušte zaškrtnutí políčka pro **vytvoření projektu testování částí**a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Vytvoření webu na Internetu mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="9ecc0-133">Otevřete **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="9ecc0-134">Tento krok přidává do projektu sadu souborů skriptů a odkazy na sestavení, které umožňují funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="9ecc0-135">V **Průzkumníku řešení** rozbalte složku skripty.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="9ecc0-136">Všimněte si, že byly přidány do projektu knihovny skriptů pro SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Knihovna odkazy](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="9ecc0-138">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Nová složka**, a přidejte novou složku s názvem **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="9ecc0-139">Klikněte pravým tlačítkem myši **Hubs** složku, klikněte na tlačítko **přidat | Třída**a vytvořte novou třídu C#, s názvem **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="9ecc0-140">Tato třída použije jako server rozbočovače SignalR zasílání zpráv na všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="9ecc0-141">Pokud používáte Visual Studio 2012 a nainstalovaný [ASP.NET a webové nástroje 2012.2 aktualizace](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), můžete použít novou šablonu SignalR položku pro vytvoření třídy rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="9ecc0-142">To lze provést, klikněte pravým tlačítkem myši **centra** složku, klikněte na tlačítko **přidat | Nová položka**, vyberte **třídy rozbočovače SignalR (v1)** a název třídy **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="9ecc0-143">Nahraďte kód v **ChatHub** třídy následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="9ecc0-144">Otevřete **Global.asax** souboru pro projekt a přidejte volání do metody `RouteTable.Routes.MapHubs();` jako první řádek kódu `Application_Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="9ecc0-145">Tento kód registruje výchozí trasu pro rozbočovače SignalR a musí být voláno předtím, než zaregistrujete jiným trasám.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="9ecc0-146">Dokončené `Application_Start` metoda vypadá jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="9ecc0-147">Upravit `HomeController` nalezena třída v **Controllers/HomeController.cs** a přidejte následující metodu do třídy.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="9ecc0-148">Tato metoda vrátí hodnotu **Chat** zobrazení, který chcete vytvořit později.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="9ecc0-149">Klikněte pravým tlačítkem myši v rámci `Chat` metoda jste právě vytvořili a klikněte na tlačítko **přidat zobrazení** k vytvoření nového souboru zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="9ecc0-150">V **přidat zobrazení** dialogové okno, ujistěte se, že je zaškrtnuté políčko **použít rozložení stránky předlohy** (zrušte zaškrtnutí políček jiných) a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Přidání zobrazení](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="9ecc0-152">Upravit nové zobrazení soubor s názvem **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="9ecc0-153">Po &lt;h2&gt; značky, vložte následující &lt;div&gt; části a `@section scripts` blok kódu na stránku.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="9ecc0-154">Tento skript umožňuje stránku k odeslání chatovací zprávy a zobrazení zpráv ze serveru.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="9ecc0-155">Kód dokončení chat zobrazení se zobrazí v následující blok kódu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9ecc0-156">Když přidáte do projektu sady Visual Studio SignalR a další skript knihovny, může správce balíčků nainstalujte verze skriptů, které jsou novější než verze uvedené v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="9ecc0-157">Ujistěte se, že odkazům na skript v kódu odpovídají verze knihoven skript, který je nainstalován ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="9ecc0-158">**Uložte všechny** pro projekt.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="9ecc0-159">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="9ecc0-159">Run the Sample</span></span>

1. <span data-ttu-id="9ecc0-160">Stisknutím klávesy F5 spusťte projekt v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="9ecc0-161">V adresním řádku prohlížeče, připojit **/home/chat** na adresu URL výchozí stránky pro projekt.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="9ecc0-162">Načtení stránky Chat v instanci prohlížeče a vyzve k uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="9ecc0-164">Zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-164">Enter a user name.</span></span>
4. <span data-ttu-id="9ecc0-165">Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="9ecc0-166">V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="9ecc0-167">V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="9ecc0-168">Komentáře by měl zobrazit ve všech instancích prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9ecc0-169">Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="9ecc0-170">Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="9ecc0-171">Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="9ecc0-172">Následující snímek obrazovky ukazuje chatovací aplikace spuštěné v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="9ecc0-174">V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="9ecc0-175">Tento uzel je viditelný v režimu ladění, pokud používáte Internet Explorer jako prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="9ecc0-176">Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="9ecc0-177">Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="9ecc0-178">Pokud používáte jiný prohlížeč než Internet Explorer, můžete taky přejít dynamická **hubs** souboru tak, že přejde k němu přímo, například http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Vygenerovaný centra skriptů](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="9ecc0-180">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="9ecc0-180">Examine the Code</span></span>

<span data-ttu-id="9ecc0-181">Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="9ecc0-182">Rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="9ecc0-182">SignalR Hubs</span></span>

<span data-ttu-id="9ecc0-183">V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="9ecc0-184">Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="9ecc0-185">Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z jQuery skriptů na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="9ecc0-186">V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="9ecc0-187">Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="9ecc0-188">**Odeslat** metoda ukazuje několik konceptů rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="9ecc0-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="9ecc0-189">Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="9ecc0-190">Použití **Microsoft.AspNet.SignalR.Hub.Clients** vlastnost, která má přístup všichni klienti připojení k této rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="9ecc0-191">Volání funkce jQuery na straně klienta (například `addNewMessageToPage` funkce) k aktualizaci klientů.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="9ecc0-192">SignalR a jQuery</span><span class="sxs-lookup"><span data-stu-id="9ecc0-192">SignalR and jQuery</span></span>

<span data-ttu-id="9ecc0-193">**Chat.cshtml** zobrazení souboru v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="9ecc0-194">Základních úloh v kódu vytváříte odkaz na automaticky generovaný proxy serveru pro rozbočovač, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a spuštění připojení k odesílání zpráv do rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="9ecc0-195">Následující kód deklaruje proxy serveru pro rozbočovač.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="9ecc0-196">V jQuery je odkaz na třídě serveru a její členy v camelCase.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="9ecc0-197">Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jQuery jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="9ecc0-198">Pokud chcete odkazovat `ChatHub` – třída v jQuery s konvenční Pascal velká a malá písmena stejně jako v jazyce C#, upravte soubor ChatHub.cs třídy.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="9ecc0-199">Přidat `using` příkaz tak, aby odkazovaly `Microsoft.AspNet.SignalR.Hubs` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="9ecc0-200">Pak přidejte `HubName` atribut `ChatHub` třídy, například `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="9ecc0-201">Nakonec aktualizujte vaši jQuery informaci, abyste `ChatHub` třídy.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="9ecc0-202">Následující kód ukazuje, jak vytvořit funkci zpětného volání ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="9ecc0-203">Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="9ecc0-204">Volitelné volání `htmlEncode` funkce zobrazí způsob, jak HTML kódování obsahu zprávy, než se zobrazí na stránce jako způsob, jak zabránit vložení skriptu.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="9ecc0-205">Následující kód ukazuje, jak k otevření připojení do centra.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="9ecc0-206">Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce konverzace.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="9ecc0-207">Tento přístup zajišťuje, že připojení před provedením obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="9ecc0-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ecc0-208">Next Steps</span></span>

<span data-ttu-id="9ecc0-209">Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="9ecc0-210">Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9ecc0-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="9ecc0-211">Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:</span><span class="sxs-lookup"><span data-stu-id="9ecc0-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="9ecc0-212">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="9ecc0-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="9ecc0-213">SignalR Githubu a ukázky</span><span class="sxs-lookup"><span data-stu-id="9ecc0-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="9ecc0-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="9ecc0-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
