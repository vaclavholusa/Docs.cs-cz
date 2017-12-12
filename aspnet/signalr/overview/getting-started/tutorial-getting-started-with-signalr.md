---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: "Kurz: Začínáme s SignalR 2 | Microsoft Docs"
author: pfletcher
description: "Tento kurz ukazuje, jak vytvořit v reálném čase chatovací aplikace pomocí funkce SignalR. Přidáte SignalR prázdnou webovou aplikaci ASP.NET a vytvoření pa HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3bec32b9b21325cde461541d7a313f401a0cfce7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="b5338-104">Kurz: Začínáme s SignalR 2</span><span class="sxs-lookup"><span data-stu-id="b5338-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="b5338-105">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b5338-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="b5338-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="b5338-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="b5338-107">Tento kurz ukazuje, jak vytvořit v reálném čase chatovací aplikace pomocí funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="b5338-108">Přidáte SignalR prázdnou webovou aplikaci ASP.NET a vytvořit stránku HTML a odesílat zprávy o zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b5338-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5338-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="b5338-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b5338-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b5338-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b5338-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b5338-111">.NET 4.5</span></span>
> - <span data-ttu-id="b5338-112">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="b5338-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="b5338-113">Tento kurz pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b5338-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="b5338-114">Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b5338-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="b5338-115">Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="b5338-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="b5338-116">Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5338-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="b5338-117">Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="b5338-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="b5338-118">Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.</span><span class="sxs-lookup"><span data-stu-id="b5338-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="b5338-119">Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.</span><span class="sxs-lookup"><span data-stu-id="b5338-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="b5338-120">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="b5338-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="b5338-121">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b5338-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b5338-122">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="b5338-122">Questions and comments</span></span>
> 
> <span data-ttu-id="b5338-123">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="b5338-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b5338-124">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b5338-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="b5338-125">Přehled</span><span class="sxs-lookup"><span data-stu-id="b5338-125">Overview</span></span>

<span data-ttu-id="b5338-126">Tento kurz představuje SignalR vývoj ukazuje, jak sestavit aplikaci Jednoduchý chat založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b5338-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="b5338-127">Přidáte knihovně SignalR prázdnou webovou aplikaci ASP.NET, vytvořte třídu rozbočovače pro odesílání zpráv do klientů a vytvořit stránku HTML, která umožňuje uživatelům odesílat a přijímat zprávy v konverzaci.</span><span class="sxs-lookup"><span data-stu-id="b5338-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="b5338-128">Podobný kurz, který ukazuje vytvoření chatovací aplikace v MVC 5 pomocí zobrazení MVC, najdete v části [Začínáme s SignalR 2 a MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="b5338-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b5338-129">Tento kurz ukazuje, jak vytvářet aplikace SignalR ve verze 2.</span><span class="sxs-lookup"><span data-stu-id="b5338-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="b5338-130">Podrobnosti o změny mezi SignalR 1.x a 2, najdete v části [projekty 1.x upgrade SignalR](../releases/upgrading-signalr-1x-projects-to-20.md) a [k verzi Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="b5338-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="b5338-131">SignalR je knihovna .NET open source pro tvorbu webových aplikací, které vyžadují zásah uživatele za provozu nebo aktualizace dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="b5338-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="b5338-132">Mezi příklady patří sociálních aplikací, s více uživateli hry, obchodní spolupráce a zprávy, počasí nebo finančních aktualizaci aplikací.</span><span class="sxs-lookup"><span data-stu-id="b5338-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="b5338-133">Toto nastavení se často nazývá aplikací v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="b5338-133">These are often called real-time applications.</span></span>

<span data-ttu-id="b5338-134">SignalR zjednodušuje proces vytváření aplikace v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="b5338-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="b5338-135">Obsahuje knihovnu serveru server ASP.NET a knihovny JavaScript klienta, aby bylo snazší spravovat připojení klient server a předat obsahu aktualizace do klientů.</span><span class="sxs-lookup"><span data-stu-id="b5338-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="b5338-136">Knihovna SignalR můžete přidat do existující aplikace ASP.NET pro získání funkce v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="b5338-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="b5338-137">Tento kurz ukazuje následující úlohy vývoj SignalR:</span><span class="sxs-lookup"><span data-stu-id="b5338-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="b5338-138">Přidání knihovně SignalR k webové aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b5338-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="b5338-139">Vytvoření třídy rozbočovače pro uložení obsahu do klientů.</span><span class="sxs-lookup"><span data-stu-id="b5338-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="b5338-140">Vytvoření třídy pro spuštění OWIN při konfiguraci této aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5338-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="b5338-141">K odesílání zpráv a zobrazení aktualizací z centra pomocí knihovny jQuery SignalR na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="b5338-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="b5338-142">Následující snímek obrazovky ukazuje chatovací aplikace spuštěné v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b5338-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="b5338-143">Každý nový uživatel, můžete odeslat komentáře a najdete v části poznámky přidané po uživatel připojí chat.</span><span class="sxs-lookup"><span data-stu-id="b5338-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instance chatu](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="b5338-145">Části:</span><span class="sxs-lookup"><span data-stu-id="b5338-145">Sections:</span></span>

- [<span data-ttu-id="b5338-146">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="b5338-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="b5338-147">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="b5338-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="b5338-148">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="b5338-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="b5338-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5338-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="b5338-150">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="b5338-150">Set up the Project</span></span>

<span data-ttu-id="b5338-151">V této části ukazuje, jak pomocí sady Visual Studio 2013 a SignalR verze 2 můžete vytvořit prázdnou webovou aplikaci ASP.NET, přidejte SignalR a vytvoření chatovací aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5338-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="b5338-152">Předpoklady:</span><span class="sxs-lookup"><span data-stu-id="b5338-152">Prerequisites:</span></span>

- <span data-ttu-id="b5338-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b5338-153">Visual Studio 2013.</span></span> <span data-ttu-id="b5338-154">Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2013 Express vývoj nástroj.</span><span class="sxs-lookup"><span data-stu-id="b5338-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="b5338-155">Následující kroky použijte k vytvoření prázdné webové aplikace ASP.NET a přidání knihovny SignalR Visual Studio 2013:</span><span class="sxs-lookup"><span data-stu-id="b5338-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="b5338-156">V sadě Visual Studio vytvořte webovou aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b5338-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Vytvořit web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="b5338-158">V **nový projekt ASP.NET** okno, ponechejte **prázdný** vybraný a klikněte na tlačítko **vytvořit projekt**.</span><span class="sxs-lookup"><span data-stu-id="b5338-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Vytvořit prázdnou webovou](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="b5338-160">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Třídy rozbočovače SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="b5338-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="b5338-161">Název třídy **ChatHub.cs** a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="b5338-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="b5338-162">Tento krok vytvoří **ChatHub** třídy a přidá do projektu sadu souborů skriptů a odkazy na sestavení, které podporují funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5338-163">SignalR můžete také přidat do projektu otevřením **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="b5338-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="b5338-164">Používáte-li přidat SignalR konzole, můžete vytvořte třídy rozbočovače SignalR jako samostatný krok po přidání SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5338-165">Pokud používáte Visual Studio 2012, **třídy rozbočovače SignalR (v2)** šablony nebudete mít k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b5338-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="b5338-166">Můžete přidat prostý **třída** názvem `ChatHub` místo.</span><span class="sxs-lookup"><span data-stu-id="b5338-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="b5338-167">V **Průzkumníku**, rozbalte uzel skripty.</span><span class="sxs-lookup"><span data-stu-id="b5338-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="b5338-168">Jsou viditelné v projektu knihovny skriptů pro knihovny jQuery a SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="b5338-169">Nahraďte kód v novém **ChatHub** třídy následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b5338-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="b5338-170">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Třídy pro spuštění OWIN**.</span><span class="sxs-lookup"><span data-stu-id="b5338-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="b5338-171">Pojmenujte novou třídu `Startup` a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="b5338-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5338-172">Pokud používáte Visual Studio 2012, **třídy pro spuštění OWIN** šablony nebudete mít k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b5338-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="b5338-173">Můžete přidat prostý **třída** názvem `Startup` místo.</span><span class="sxs-lookup"><span data-stu-id="b5338-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="b5338-174">Změňte obsah novou třídu spuštění následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b5338-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="b5338-175">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Stránky HTML**.</span><span class="sxs-lookup"><span data-stu-id="b5338-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="b5338-176">Zadejte název nové stránky `index.html`.</span><span class="sxs-lookup"><span data-stu-id="b5338-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="b5338-177">Možná budete muset změnit čísla verzí pro odkazy na knihovny JQuery a SignalR</span><span class="sxs-lookup"><span data-stu-id="b5338-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="b5338-178">V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku HTML, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.</span><span class="sxs-lookup"><span data-stu-id="b5338-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="b5338-179">Ve výchozím kódu na stránce HTML nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="b5338-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5338-180">Novější verze skriptů SignalR mohou být nainstalovány službou Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="b5338-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="b5338-181">Ověřte, zda skript odkazy níže odpovídají verze souborů skriptu v projektu (bude jiné, pokud jste přidali pomocí nástroje NuGet a místo přidávání rozbočovači SignalR.)</span><span class="sxs-lookup"><span data-stu-id="b5338-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="b5338-182">**Uložte všechny** pro projekt.</span><span class="sxs-lookup"><span data-stu-id="b5338-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="b5338-183">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="b5338-183">Run the Sample</span></span>

1. <span data-ttu-id="b5338-184">Stisknutím klávesy F5 spusťte projekt v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="b5338-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="b5338-185">Načtení stránky HTML v instanci prohlížeče a vyzve k uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b5338-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="b5338-187">Zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b5338-187">Enter a user name.</span></span>
3. <span data-ttu-id="b5338-188">Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b5338-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="b5338-189">V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b5338-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="b5338-190">V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="b5338-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="b5338-191">Komentáře by měl zobrazit ve všech instancích prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b5338-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5338-192">Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru.</span><span class="sxs-lookup"><span data-stu-id="b5338-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="b5338-193">Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="b5338-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="b5338-194">Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.</span><span class="sxs-lookup"><span data-stu-id="b5338-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="b5338-195">Následující snímek obrazovky ukazuje chatovací aplikace běžící v tři instance prohlížeče, které aktualizují po jedné instance odešle zprávu:</span><span class="sxs-lookup"><span data-stu-id="b5338-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="b5338-197">V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b5338-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="b5338-198">Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="b5338-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="b5338-199">Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="b5338-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="b5338-200">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="b5338-200">Examine the Code</span></span>

<span data-ttu-id="b5338-201">Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="b5338-202">Rozbočovače SignalR</span><span class="sxs-lookup"><span data-stu-id="b5338-202">SignalR Hubs</span></span>

<span data-ttu-id="b5338-203">V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy.</span><span class="sxs-lookup"><span data-stu-id="b5338-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="b5338-204">Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="b5338-205">Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z skriptů na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="b5338-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="b5338-206">V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5338-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="b5338-207">Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="b5338-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="b5338-208">**Odeslat** metoda ukazuje několik konceptů rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="b5338-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="b5338-209">Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.</span><span class="sxs-lookup"><span data-stu-id="b5338-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="b5338-210">Použití **Microsoft.AspNet.SignalR.Hub.Clients** dynamických vlastností pro přístup k všechny klienty připojené k této rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b5338-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="b5338-211">Volání funkce na straně klienta (například `broadcastMessage` funkce) k aktualizaci klientů.</span><span class="sxs-lookup"><span data-stu-id="b5338-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="b5338-212">SignalR a jQuery</span><span class="sxs-lookup"><span data-stu-id="b5338-212">SignalR and jQuery</span></span>

<span data-ttu-id="b5338-213">Stránky HTML v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR.</span><span class="sxs-lookup"><span data-stu-id="b5338-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="b5338-214">Proxy server tak, aby odkazovaly rozbočovače, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a počáteční připojení k odeslání zprávy do centra jsou deklarace základních úloh v kódu.</span><span class="sxs-lookup"><span data-stu-id="b5338-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="b5338-215">Následující kód deklaruje odkaz na proxy server rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b5338-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="b5338-216">V jazyce JavaScript je odkaz na třídě serveru a její členy v camelCase.</span><span class="sxs-lookup"><span data-stu-id="b5338-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="b5338-217">Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jazyce JavaScript jako **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="b5338-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="b5338-218">Následující kód je, jak vytvořit funkci zpětného volání ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="b5338-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="b5338-219">Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů.</span><span class="sxs-lookup"><span data-stu-id="b5338-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="b5338-220">Dva řádky, HTML kódování obsahu před jejich zobrazením jsou volitelné a zobrazit jednoduchý způsob, jak zabránit vložení skriptu.</span><span class="sxs-lookup"><span data-stu-id="b5338-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="b5338-221">Následující kód ukazuje, jak k otevření připojení do centra.</span><span class="sxs-lookup"><span data-stu-id="b5338-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="b5338-222">Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce HTML.</span><span class="sxs-lookup"><span data-stu-id="b5338-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="b5338-223">Tento přístup zajišťuje, že připojení před provedením obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="b5338-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="b5338-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5338-224">Next Steps</span></span>

<span data-ttu-id="b5338-225">Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="b5338-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="b5338-226">Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="b5338-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="b5338-227">Podrobný postup nasazení ukázkové aplikace SignalR do Azure, najdete v části [pomocí SignalR s webovými aplikacemi ve službě Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="b5338-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="b5338-228">Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="b5338-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="b5338-229">Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:</span><span class="sxs-lookup"><span data-stu-id="b5338-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="b5338-230">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="b5338-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="b5338-231">SignalR Githubu a ukázky</span><span class="sxs-lookup"><span data-stu-id="b5338-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b5338-232">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="b5338-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
