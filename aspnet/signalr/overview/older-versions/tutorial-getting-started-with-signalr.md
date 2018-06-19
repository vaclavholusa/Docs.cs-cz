---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Kurz: Začínáme s SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Sestavení aplikace chat v reálném čase na stránce HTML pomocí funkce SignalR technologie ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044731"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="7ccfd-103">Kurz: Začínáme s SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="7ccfd-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="7ccfd-104">podle [Patrik Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="7ccfd-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="7ccfd-105">V tomto kurzu se naučíte používat funkci SignalR k vytvoření aplikace pro chatování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="7ccfd-106">Přidáte SignalR prázdnou webovou aplikaci ASP.NET a vytvořit stránku HTML a odesílat zprávy o zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="7ccfd-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="7ccfd-107">Overview</span></span>

<span data-ttu-id="7ccfd-108">Tento kurz představuje SignalR vývoj ukazuje, jak sestavit aplikaci Jednoduchý chat založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="7ccfd-109">Přidáte knihovně SignalR prázdnou webovou aplikaci ASP.NET, vytvořte třídu rozbočovače pro odesílání zpráv do klientů a vytvořit stránku HTML, která umožňuje uživatelům odesílat a přijímat zprávy v konverzaci.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="7ccfd-110">Podobný kurz, který ukazuje vytvoření chatovací aplikace v MVC 4 pomocí zobrazení MVC, najdete v části [Začínáme s SignalR a MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="7ccfd-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7ccfd-111">Tento kurz používá (1.x) verzi SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="7ccfd-112">Podrobnosti o změny mezi SignalR 1.x a 2.0, najdete v části [upgrade SignalR 1.x projekty](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="7ccfd-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="7ccfd-113">SignalR je knihovna .NET open source pro tvorbu webových aplikací, které vyžadují zásah uživatele za provozu nebo aktualizace dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="7ccfd-114">Mezi příklady patří sociálních aplikací, s více uživateli hry, obchodní spolupráce a zprávy, počasí nebo finančních aktualizaci aplikací.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="7ccfd-115">Toto nastavení se často nazývá aplikací v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-115">These are often called real-time applications.</span></span>

<span data-ttu-id="7ccfd-116">SignalR zjednodušuje proces vytváření aplikace v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="7ccfd-117">Obsahuje knihovnu serveru server ASP.NET a knihovny JavaScript klienta, aby bylo snazší spravovat připojení klient server a předat obsahu aktualizace do klientů.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="7ccfd-118">Knihovna SignalR můžete přidat do existující aplikace ASP.NET pro získání funkce v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="7ccfd-119">Tento kurz ukazuje následující úlohy vývoj SignalR:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="7ccfd-120">Přidání knihovně SignalR k webové aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="7ccfd-121">Vytvoření třídy rozbočovače pro uložení obsahu do klientů.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="7ccfd-122">K odesílání zpráv a zobrazení aktualizací z centra pomocí knihovny jQuery SignalR na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="7ccfd-123">Následující snímek obrazovky ukazuje chatovací aplikace spuštěné v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="7ccfd-124">Každý nový uživatel, můžete odeslat komentáře a najdete v části poznámky přidané po uživatel připojí chat.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instance chatu](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="7ccfd-126">Části:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-126">Sections:</span></span>

- [<span data-ttu-id="7ccfd-127">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="7ccfd-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="7ccfd-128">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="7ccfd-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="7ccfd-129">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="7ccfd-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="7ccfd-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ccfd-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="7ccfd-131">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="7ccfd-131">Set up the Project</span></span>

<span data-ttu-id="7ccfd-132">V této části ukazuje, jak vytvořit prázdnou webovou aplikaci ASP.NET, přidejte SignalR a vytvoření chatovací aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="7ccfd-133">Předpoklady:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-133">Prerequisites:</span></span>

- <span data-ttu-id="7ccfd-134">Visual Studio 2010 SP1 nebo 2012.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="7ccfd-135">Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2012 Express vývoj nástroj.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="7ccfd-136">[Microsoft ASP.NET a webové nástroje 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="7ccfd-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="7ccfd-137">Pro sadu Visual Studio 2012 tento instalační program přidá nové funkce ASP.NET, včetně SignalR šablony sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="7ccfd-138">Pro Visual Studio 2010 SP1 instalační program není k dispozici, ale můžete dokončit kurz nainstalováním balíček SignalR NuGet, jak je popsáno v krocích instalační program.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="7ccfd-139">Následující postup použijte sadu Visual Studio 2012 vytvoření prázdné webové aplikace ASP.NET a přidání knihovně SignalR:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="7ccfd-140">V sadě Visual Studio vytvořte prázdný webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Vytvořit prázdnou webovou](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="7ccfd-142">Otevřete **Konzola správce balíčků** výběrem **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="7ccfd-143">Do okna konzoly, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="7ccfd-144">Tento příkaz nainstaluje nejnovější verzi SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="7ccfd-145">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Třída**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="7ccfd-146">Pojmenujte novou třídu **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="7ccfd-147">V **Průzkumníku řešení** rozbalte uzel skripty.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="7ccfd-148">Jsou viditelné v projektu knihovny skriptů pro knihovny jQuery a SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Knihovna odkazy](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="7ccfd-150">Nahraďte kód v **ChatHub** třídy následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="7ccfd-151">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="7ccfd-152">V **přidat novou položku** dialogovém okně, vyberte **globální třídy aplikace** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Přidejte globální](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="7ccfd-154">Přidejte následující `using` příkazy po poskytnutého `using` příkazy ve třídě Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="7ccfd-155">Přidejte následující řádek kódu `Application_Start` metoda globální třídy k registraci výchozí trasu pro rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="7ccfd-156">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="7ccfd-157">V **přidat novou položku** dialogové okno, vyberte stránku Html a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="7ccfd-158">V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku HTML, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="7ccfd-159">Ve výchozím kódu na stránce HTML nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="7ccfd-160">**Uložte všechny** pro projekt.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="7ccfd-161">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="7ccfd-161">Run the Sample</span></span>

1. <span data-ttu-id="7ccfd-162">Stisknutím klávesy F5 spusťte projekt v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="7ccfd-163">Načtení stránky HTML v instanci prohlížeče a vyzve k uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="7ccfd-165">Zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-165">Enter a user name.</span></span>
3. <span data-ttu-id="7ccfd-166">Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="7ccfd-167">V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="7ccfd-168">V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="7ccfd-169">Komentáře by měl zobrazit ve všech instancích prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ccfd-170">Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="7ccfd-171">Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="7ccfd-172">Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="7ccfd-173">Následující snímek obrazovky ukazuje chatovací aplikace běžící v tři instance prohlížeče, které aktualizují po jedné instance odešle zprávu:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="7ccfd-175">V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="7ccfd-176">Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="7ccfd-177">Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Vygenerovaný centra skriptů](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="7ccfd-179">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="7ccfd-179">Examine the Code</span></span>

<span data-ttu-id="7ccfd-180">Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="7ccfd-181">Rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="7ccfd-181">SignalR Hubs</span></span>

<span data-ttu-id="7ccfd-182">V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="7ccfd-183">Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="7ccfd-184">Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z jQuery skriptů na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="7ccfd-185">V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="7ccfd-186">Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="7ccfd-187">**Odeslat** metoda ukazuje několik konceptů rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="7ccfd-188">Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="7ccfd-189">Použití **Microsoft.AspNet.SignalR.Hub.Clients** dynamických vlastností pro přístup k všechny klienty připojené k této rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="7ccfd-190">Volání funkce jQuery na straně klienta (například `broadcastMessage` funkce) k aktualizaci klientů.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="7ccfd-191">SignalR a jQuery</span><span class="sxs-lookup"><span data-stu-id="7ccfd-191">SignalR and jQuery</span></span>

<span data-ttu-id="7ccfd-192">Stránky HTML v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="7ccfd-193">Proxy server tak, aby odkazovaly rozbočovače, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a počáteční připojení k odeslání zprávy do centra jsou deklarace základních úloh v kódu.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="7ccfd-194">Následující kód deklaruje proxy serveru pro rozbočovač.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="7ccfd-195">V jQuery je odkaz na třídě serveru a její členy v camelCase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="7ccfd-196">Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jQuery jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="7ccfd-197">Následující kód je, jak vytvořit funkci zpětného volání ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="7ccfd-198">Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="7ccfd-199">Dva řádky, HTML kódování obsahu před jejich zobrazením jsou volitelné a zobrazit jednoduchý způsob, jak zabránit vložení skriptu.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="7ccfd-200">Následující kód ukazuje, jak k otevření připojení do centra.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="7ccfd-201">Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce HTML.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="7ccfd-202">Tento přístup zajistí, že připojení před provedením obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="7ccfd-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ccfd-203">Next Steps</span></span>

<span data-ttu-id="7ccfd-204">Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="7ccfd-205">Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="7ccfd-206">Můžete zpřístupnit ukázkové aplikace v tomto kurzu nebo jinými aplikacemi SignalR přes Internet pomocí jejich nasazení do hostujícího zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="7ccfd-207">Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve bezplatný [zkušební účet systému Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="7ccfd-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="7ccfd-208">Podrobný postup nasazení ukázkové aplikace SignalR, najdete v části [SignalR získávání spustit ukázku jako webu systému Windows Azure publikování](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ccfd-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="7ccfd-209">Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [nasazení aplikace ASP.NET na webu systému Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="7ccfd-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="7ccfd-210">(Poznámka: přenos protokolu WebSocket aktuálně nepodporuje pro weby systému Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7ccfd-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="7ccfd-211">Přenos protokolu WebSocket když není k dispozici, SignalR používá k dispozici přenosy, jak je popsáno v části přenosy [Úvod k tématu SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="7ccfd-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="7ccfd-212">Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:</span><span class="sxs-lookup"><span data-stu-id="7ccfd-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="7ccfd-213">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="7ccfd-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="7ccfd-214">SignalR Githubu a ukázky</span><span class="sxs-lookup"><span data-stu-id="7ccfd-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="7ccfd-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7ccfd-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
