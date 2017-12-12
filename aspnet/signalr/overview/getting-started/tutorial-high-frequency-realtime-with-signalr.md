---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "Kurz: Vysoká frekvence v reálném čase s SignalR 2 | Microsoft Docs"
author: pfletcher
description: "Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá funkce SignalR technologie ASP.NET poskytuje funkce zasílání zpráv vysoká frekvence. Vysoká frekvence zasílání zpráv v..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5af7289392c18d58de11249c75e539c9e08954be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a><span data-ttu-id="1ec41-104">Kurz: Vysoká frekvence v reálném čase s SignalR 2</span><span class="sxs-lookup"><span data-stu-id="1ec41-104">Tutorial: High-Frequency Realtime with SignalR 2</span></span>
====================
<span data-ttu-id="1ec41-105">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1ec41-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="1ec41-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="1ec41-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> <span data-ttu-id="1ec41-107">Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá ASP.NET SignalR 2 vysoká frekvence funkce zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="1ec41-107">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="1ec41-108">Vysoká frekvence zasílání zpráv v tomto případě znamená aktualizace, které se odesílají s pevnou sazbou; v případě této aplikace zpráv až 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="1ec41-108">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="1ec41-109">Aplikace, kterou budete v tomto kurzu vytvoříte zobrazí tvar, který můžete přetáhnout uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ec41-109">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="1ec41-110">Pozice tvaru ve všech ostatních připojených prohlížečích aktualizovány tak, aby odpovídaly místo taženou tvaru pomocí aktualizací vypršel.</span><span class="sxs-lookup"><span data-stu-id="1ec41-110">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="1ec41-111">Koncepty představené v tomto kurzu jste aplikací v reálném čase herní nebo jiných aplikací simulace.</span><span class="sxs-lookup"><span data-stu-id="1ec41-111">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1ec41-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="1ec41-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="1ec41-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1ec41-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="1ec41-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1ec41-114">.NET 4.5</span></span>
> - <span data-ttu-id="1ec41-115">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="1ec41-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="1ec41-116">Tento kurz pomocí sady Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1ec41-116">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="1ec41-117">Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1ec41-117">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="1ec41-118">Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="1ec41-118">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="1ec41-119">Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ec41-119">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="1ec41-120">Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-120">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="1ec41-121">Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-121">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="1ec41-122">Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.</span><span class="sxs-lookup"><span data-stu-id="1ec41-122">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="1ec41-123">Kurz verze</span><span class="sxs-lookup"><span data-stu-id="1ec41-123">Tutorial Versions</span></span>
> 
> <span data-ttu-id="1ec41-124">Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="1ec41-124">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="1ec41-125">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="1ec41-125">Questions and comments</span></span>
> 
> <span data-ttu-id="1ec41-126">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="1ec41-126">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="1ec41-127">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="1ec41-127">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="1ec41-128">Přehled</span><span class="sxs-lookup"><span data-stu-id="1ec41-128">Overview</span></span>

<span data-ttu-id="1ec41-129">Tento kurz ukazuje, jak vytvořit aplikaci, který sdílí stav objektu jiných prohlížečů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="1ec41-129">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="1ec41-130">Aplikace, kterou vytvoříme nazývá MoveShape.</span><span class="sxs-lookup"><span data-stu-id="1ec41-130">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="1ec41-131">Na stránce MoveShape se zobrazí element Div jazyka HTML, který uživatel můžete přetáhnout; Když uživatel nastavuje tažením Div, nové místo odešle na server, který bude potom informovat, všechny ostatní připojené klienty aktualizovat umístění tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="1ec41-131">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="1ec41-133">Aplikace vytvořené v tomto kurzu je založena na ukázku Damianu Edwards.</span><span class="sxs-lookup"><span data-stu-id="1ec41-133">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="1ec41-134">Video obsahující tuto ukázku si můžete prohlédnout [zde](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span><span class="sxs-lookup"><span data-stu-id="1ec41-134">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="1ec41-135">Kurz se spustí ve který ukazuje, jak k odeslání zprávy SignalR z každé události, které se zahájí, jako je přetáhnout tvaru.</span><span class="sxs-lookup"><span data-stu-id="1ec41-135">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="1ec41-136">Každý připojený klient pak bude aktualizovat umístění v místní verzi tvaru pokaždé, když je přijata zpráva.</span><span class="sxs-lookup"><span data-stu-id="1ec41-136">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="1ec41-137">Když aplikace bude fungovat touto metodou, to není doporučené programovací model, vzhledem k tomu, že by existovat žádné horní limit pro počet zpráv, získávání odeslat, aby klienti a server může získat zahlcen zprávy a by snížení výkonu .</span><span class="sxs-lookup"><span data-stu-id="1ec41-137">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="1ec41-138">Zobrazené animace v klientovi by také odděleném pohybu tvar, který by být okamžitě každý metodou spíše než přesunutí plynule do každého nového umístění.</span><span class="sxs-lookup"><span data-stu-id="1ec41-138">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="1ec41-139">V dalších částech tohoto kurzu bude ukazují, jak vytvořit časovač funkci, která omezuje maximální rychlost, jakou jsou odesílány zprávy klienta nebo serveru a postup přesunutí tvaru plynule mezi umístěními.</span><span class="sxs-lookup"><span data-stu-id="1ec41-139">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="1ec41-140">Finální verzi aplikace vytvořené v tomto kurzu lze stáhnout z [galerie kódů](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span><span class="sxs-lookup"><span data-stu-id="1ec41-140">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="1ec41-141">Tento kurz obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="1ec41-141">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="1ec41-142">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1ec41-142">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="1ec41-143">Vytvořte projekt a přidejte balíček SignalR a JQuery.UI NuGet</span><span class="sxs-lookup"><span data-stu-id="1ec41-143">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>](#createtheproject2013)
- [<span data-ttu-id="1ec41-144">Vytvoření základní aplikace</span><span class="sxs-lookup"><span data-stu-id="1ec41-144">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="1ec41-145">Spouštění rozbočovače při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1ec41-145">Starting the hub when the application starts</span></span>](#startup2013)
- [<span data-ttu-id="1ec41-146">Přidat klienta smyčky</span><span class="sxs-lookup"><span data-stu-id="1ec41-146">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="1ec41-147">Přidat server smyčky</span><span class="sxs-lookup"><span data-stu-id="1ec41-147">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="1ec41-148">Přidat smooth animace na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1ec41-148">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="1ec41-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ec41-149">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="1ec41-150">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1ec41-150">Prerequisites</span></span>

<span data-ttu-id="1ec41-151">Tento kurz vyžaduje Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1ec41-151">This tutorial requires Visual Studio 2013.</span></span>

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a><span data-ttu-id="1ec41-152">Vytvořte projekt a přidejte balíček SignalR a JQuery.UI NuGet</span><span class="sxs-lookup"><span data-stu-id="1ec41-152">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>

<span data-ttu-id="1ec41-153">V této části vytvoříme projektu v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1ec41-153">In this section, we'll create the project in Visual Studio 2013.</span></span>

<span data-ttu-id="1ec41-154">Následující kroky použijte k vytvoření prázdné webové aplikace ASP.NET a přidání knihoven SignalR a jQuery.UI Visual Studio 2013:</span><span class="sxs-lookup"><span data-stu-id="1ec41-154">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR and jQuery.UI libraries:</span></span>

1. <span data-ttu-id="1ec41-155">V sadě Visual Studio vytvořte webovou aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ec41-155">In Visual Studio create an ASP.NET Web Application.</span></span>

    ![Vytvořit web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. <span data-ttu-id="1ec41-157">V **nový projekt ASP.NET** okno, ponechejte **prázdný** vybraný a klikněte na tlačítko **vytvořit projekt**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-157">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Vytvořit prázdnou webovou](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. <span data-ttu-id="1ec41-159">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Třídy rozbočovače SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-159">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="1ec41-160">Název třídy **MoveShapeHub.cs** a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="1ec41-160">Name the class **MoveShapeHub.cs** and add it to the project.</span></span> <span data-ttu-id="1ec41-161">Tento krok vytvoří **MoveShapeHub** třídy a přidá do projektu sadu souborů skriptů a odkazy na sestavení, které podporují funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="1ec41-161">This step creates the **MoveShapeHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ec41-162">SignalR můžete také přidat do projektu kliknutím **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="1ec41-162">You can also add SignalR to a project by clicking **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    <span data-ttu-id="1ec41-163">`install-package Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="1ec41-163">`install-package Microsoft.AspNet.SignalR`.</span></span> 

    <span data-ttu-id="1ec41-164">Používáte-li přidat SignalR konzole, můžete vytvořte třídy rozbočovače SignalR jako samostatný krok po přidání SignalR.</span><span class="sxs-lookup"><span data-stu-id="1ec41-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>
4. <span data-ttu-id="1ec41-165">Klikněte na tlačítko **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-165">Click **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="1ec41-166">V okně Správce balíčku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1ec41-166">In the package manager window, run the following command:</span></span>

    `Install-Package jQuery.UI.Combined`

    <span data-ttu-id="1ec41-167">To nainstaluje knihovny uživatelského rozhraní jQuery, který budete používat pro animaci tvaru.</span><span class="sxs-lookup"><span data-stu-id="1ec41-167">This installs the jQuery UI library, which you'll use to animate the shape.</span></span>
5. <span data-ttu-id="1ec41-168">V **Průzkumníku řešení** rozbalte uzel skripty.</span><span class="sxs-lookup"><span data-stu-id="1ec41-168">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="1ec41-169">Skript knihovny jQuery, jQueryUI a SignalR jsou viditelné v projektu.</span><span class="sxs-lookup"><span data-stu-id="1ec41-169">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

    ![Reference knihovny skriptu](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="1ec41-171">Vytvoření základní aplikace</span><span class="sxs-lookup"><span data-stu-id="1ec41-171">Create the base application</span></span>

<span data-ttu-id="1ec41-172">V této části vytvoříme prohlížeče aplikace, která odesílá umístění tvaru na server během každou událost pohybu myší.</span><span class="sxs-lookup"><span data-stu-id="1ec41-172">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="1ec41-173">Přijetí, všesměrově server pak tyto informace do všech ostatních připojených klientů.</span><span class="sxs-lookup"><span data-stu-id="1ec41-173">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="1ec41-174">Jsme budete rozšířit do této aplikace v pozdějších částech.</span><span class="sxs-lookup"><span data-stu-id="1ec41-174">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="1ec41-175">Pokud jste ještě nevytvořili třídě MoveShapeHub.cs v **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat**, **třídy...** . Název třídy **MoveShapeHub** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-175">If you haven't already created the MoveShapeHub.cs class, in **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="1ec41-176">Nahraďte kód v novém **MoveShapeHub** třídy následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="1ec41-176">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="1ec41-177">`MoveShapeHub` Výše třída je implementace rozbočovače SignalR.</span><span class="sxs-lookup"><span data-stu-id="1ec41-177">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="1ec41-178">Jako v [Začínáme s SignalR](tutorial-getting-started-with-signalr.md) kurzu rozbočovače má metodu, která klienti bude volat přímo.</span><span class="sxs-lookup"><span data-stu-id="1ec41-178">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="1ec41-179">V tomto případě klient odešle objekt, který obsahuje nový tvar, který má na server, který pak získá vysílali pro všechny ostatní připojené klienty souřadnice X a Y.</span><span class="sxs-lookup"><span data-stu-id="1ec41-179">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="1ec41-180">SignalR bude automaticky serializovat tento objekt, který používá JSON.</span><span class="sxs-lookup"><span data-stu-id="1ec41-180">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="1ec41-181">Objekt, který odešle klientovi (`ShapeModel`) obsahuje členy k uložení pozice tvaru.</span><span class="sxs-lookup"><span data-stu-id="1ec41-181">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="1ec41-182">Verze objektu na serveru také obsahuje členem, abyste mohli sledovat data klientů, které ukládají, tak, aby daného klienta nebude odeslána svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="1ec41-182">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="1ec41-183">Tento člen používá `JsonIgnore` atribut zabránit jeho se odešle do klienta a serializovat.</span><span class="sxs-lookup"><span data-stu-id="1ec41-183">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a><span data-ttu-id="1ec41-184">Spouštění rozbočovače při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1ec41-184">Starting the hub when the application starts</span></span>

1. <span data-ttu-id="1ec41-185">V dalším kroku nastavíme mapování k rozbočovači při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ec41-185">Next, we'll set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="1ec41-186">V systému SignalR 2, to se provádí přidáním třídy pro spuštění OWIN, který bude volat `MapSignalR` při – spuštění třída `Configuration` metoda spuštěna při spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="1ec41-186">In SignalR 2, this is done by adding an OWIN startup class, which will call `MapSignalR` when the startup class' `Configuration` method is executed when OWIN starts.</span></span> <span data-ttu-id="1ec41-187">Třída je přidán do na OWIN při spuštění zpracování pomocí `OwinStartup` atributu sestavení.</span><span class="sxs-lookup"><span data-stu-id="1ec41-187">The class is added to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

    <span data-ttu-id="1ec41-188">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Třídy pro spuštění OWIN**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-188">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="1ec41-189">Název třídy *spuštění* a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-189">Name the class *Startup* and click **OK**.</span></span>
2. <span data-ttu-id="1ec41-190">Obsah souboru Startup.cs změňte na následující:</span><span class="sxs-lookup"><span data-stu-id="1ec41-190">Change the contents of Startup.cs to the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a><span data-ttu-id="1ec41-191">Přidání klienta</span><span class="sxs-lookup"><span data-stu-id="1ec41-191">Adding the client</span></span>

1. <span data-ttu-id="1ec41-192">Potom přidáme klienta.</span><span class="sxs-lookup"><span data-stu-id="1ec41-192">Next, we'll add the client.</span></span> <span data-ttu-id="1ec41-193">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-193">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="1ec41-194">V **přidat novou položku** dialogovém okně, vyberte **stránku Html**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-194">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="1ec41-195">Název stránky **Default.html** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-195">Name the page **Default.html** and click **Add**.</span></span>
2. <span data-ttu-id="1ec41-196">V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.</span><span class="sxs-lookup"><span data-stu-id="1ec41-196">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
3. <span data-ttu-id="1ec41-197">Nahraďte kód výchozí stránku HTML následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="1ec41-197">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ec41-198">Ověřte, zda skript odkazuje níže shodu balíčky přidat do projektu ve složce skripty.</span><span class="sxs-lookup"><span data-stu-id="1ec41-198">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    <span data-ttu-id="1ec41-199">Ve výše uvedeném kódu HTML a JavaScript vytvoří red Div názvem tvaru, umožňuje tvaru přetahování chování pomocí knihovny jQuery a používá tvaru `drag` událost, která má odeslat umístění na server.</span><span class="sxs-lookup"><span data-stu-id="1ec41-199">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
4. <span data-ttu-id="1ec41-200">Spusťte aplikaci stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="1ec41-200">Start the application by pressing F5.</span></span> <span data-ttu-id="1ec41-201">Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-201">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="1ec41-202">Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-202">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="1ec41-204">Přidat klienta smyčky</span><span class="sxs-lookup"><span data-stu-id="1ec41-204">Add the client loop</span></span>

<span data-ttu-id="1ec41-205">Vzhledem k tomu, že odesílání umístění obrazce na každou událost pohybu myší vytvoří nepotřebných množství síťový provoz, je potřeba omezeny zprávy z klienta.</span><span class="sxs-lookup"><span data-stu-id="1ec41-205">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="1ec41-206">Použijeme javascript `setInterval` funkce nastavit smyčku, která odesílá informace o novém umístění na server s pevnou sazbou.</span><span class="sxs-lookup"><span data-stu-id="1ec41-206">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="1ec41-207">Tato smyčka je velmi základní reprezentace "herní smyčka", opakovaně zavolat funkci, která jednotky všechny funkce hry nebo jiné simulace.</span><span class="sxs-lookup"><span data-stu-id="1ec41-207">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="1ec41-208">Aktualizujte kód klienta na stránce HTML tak, aby odpovídala následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="1ec41-208">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    <span data-ttu-id="1ec41-209">Výše uvedené aktualizace přidá `updateServerModel` funkce, který je volán na pevnou frekvencí.</span><span class="sxs-lookup"><span data-stu-id="1ec41-209">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="1ec41-210">Tato funkce odesílá umístění dat na server vždy, když `moved` příznak znamená, že existuje nové umístění dat k odeslání.</span><span class="sxs-lookup"><span data-stu-id="1ec41-210">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="1ec41-211">Spusťte aplikaci stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="1ec41-211">Start the application by pressing F5.</span></span> <span data-ttu-id="1ec41-212">Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="1ec41-213">Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="1ec41-214">Vzhledem k tomu, že počet zpráv, které získat odeslány na server budou omezeny, nezobrazí se jako plynulé jako v předchozí části animace.</span><span class="sxs-lookup"><span data-stu-id="1ec41-214">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="1ec41-216">Přidat server smyčky</span><span class="sxs-lookup"><span data-stu-id="1ec41-216">Add the server loop</span></span>

<span data-ttu-id="1ec41-217">V aktuální aplikaci zprávy odeslané ze serveru klientovi přejděte často příjmu.</span><span class="sxs-lookup"><span data-stu-id="1ec41-217">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="1ec41-218">To představuje podobný problém, jak je vidět na straně klienta; častěji, než jsou potřeba, a připojení může být přenášeny proto nelze odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="1ec41-218">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="1ec41-219">Tato část popisuje postup aktualizace serveru pro implementaci časovač, který omezí generovaný počet odchozích zpráv.</span><span class="sxs-lookup"><span data-stu-id="1ec41-219">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="1ec41-220">Nahraďte obsah `MoveShapeHub.cs` následujícím fragmentem kódu.</span><span class="sxs-lookup"><span data-stu-id="1ec41-220">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="1ec41-221">Výše uvedený kód rozšiřuje klienta přidat `Broadcaster` třídy, která omezí generovaný odchozích zpráv pomocí `Timer` třídy z rozhraní .NET framework.</span><span class="sxs-lookup"><span data-stu-id="1ec41-221">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="1ec41-222">Vzhledem k tomu, že je přechodné rozbočovače, samotné (vytváří se pokaždé, když je to potřeba), `Broadcaster` bude vytvořen jako typ singleton.</span><span class="sxs-lookup"><span data-stu-id="1ec41-222">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="1ec41-223">Opožděná inicializace (dostupné v rozhraní .NET 4) se používá k odložení svého vytvoření, dokud nebude potřeba, zajistíte, aby první instanci rozbočovače úplně vytvořený před zahájením časovač.</span><span class="sxs-lookup"><span data-stu-id="1ec41-223">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="1ec41-224">Volání klientů `UpdateShape` funkce je pak přesunout mimo rozbočovače na `UpdateModel` metody, které se nazývá už okamžitě vždy, když jsou přijaty příchozí zprávy.</span><span class="sxs-lookup"><span data-stu-id="1ec41-224">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="1ec41-225">Místo toho budou odeslány zprávy, které mají klienty s rychlostí 25 volání za sekundu, spravuje `_broadcastLoop` časovače uvnitř `Broadcaster` třídy.</span><span class="sxs-lookup"><span data-stu-id="1ec41-225">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="1ec41-226">Nakonec namísto volání metody klienta od rozbočovače přímo, `Broadcaster` třída musí získat odkaz na aktuálně operačního centra (`_hubContext`) pomocí `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="1ec41-226">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="1ec41-227">Spusťte aplikaci stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="1ec41-227">Start the application by pressing F5.</span></span> <span data-ttu-id="1ec41-228">Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-228">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="1ec41-229">Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-229">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="1ec41-230">Nebudou viditelné rozdíl v prohlížeči z předchozí části, ale počet zpráv, které získat odeslat klientovi, budou omezeny.</span><span class="sxs-lookup"><span data-stu-id="1ec41-230">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="1ec41-232">Přidat smooth animace na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1ec41-232">Add smooth animation on the client</span></span>

<span data-ttu-id="1ec41-233">Aplikace je téměř dokončena, ale může uděláme jeden další vylepšení v pohybu tvaru v klientovi pohybu v odpovědi na zprávy serveru.</span><span class="sxs-lookup"><span data-stu-id="1ec41-233">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="1ec41-234">Místo nastavení polohy tvaru do nového umístění zadané serverem, použijeme knihovna uživatelského rozhraní JQuery `animate` funkce přesunutí obrazce plynule mezi aktuálních a nových pozici.</span><span class="sxs-lookup"><span data-stu-id="1ec41-234">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="1ec41-235">Aktualizace klienta `updateShape` metodu vypadat jako následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="1ec41-235">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    <span data-ttu-id="1ec41-236">Ve výše uvedeném kódu přesune tvaru z původního umístění do nového zadaný server v průběhu intervalu animace (v tomto případě 100 milisekund).</span><span class="sxs-lookup"><span data-stu-id="1ec41-236">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="1ec41-237">Všechny předchozí animace spuštěna na tvar, který není zaškrtnuto, před zahájením nové animace.</span><span class="sxs-lookup"><span data-stu-id="1ec41-237">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="1ec41-238">Spusťte aplikaci stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="1ec41-238">Start the application by pressing F5.</span></span> <span data-ttu-id="1ec41-239">Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-239">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="1ec41-240">Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1ec41-240">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="1ec41-241">Přesun tvaru v dalším okně by se zobrazit méně trhané, jako je jeho přesunu interpolované přes čas a nikoli Probíhá nastavení jednou za příchozí zprávy.</span><span class="sxs-lookup"><span data-stu-id="1ec41-241">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="1ec41-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ec41-243">Further Steps</span></span>

<span data-ttu-id="1ec41-244">V tomto kurzu když jste se naučili programování aplikace SignalR, která odesílá zprávy vysoká frekvence mezi klienty a servery.</span><span class="sxs-lookup"><span data-stu-id="1ec41-244">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="1ec41-245">Tato komunikace zlepší je užitečné pro vývoj online her a dalších simulace, například [herní ShootR vytvořili pomocí nástroje SignalR](http://shootr.signalr.net).</span><span class="sxs-lookup"><span data-stu-id="1ec41-245">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](http://shootr.signalr.net).</span></span>

<span data-ttu-id="1ec41-246">Kompletní aplikace vytvořené v tomto kurzu lze stáhnout z [galerie kódů](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span><span class="sxs-lookup"><span data-stu-id="1ec41-246">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="1ec41-247">Další informace o konceptech vývoj SignalR, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:</span><span class="sxs-lookup"><span data-stu-id="1ec41-247">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="1ec41-248">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="1ec41-248">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="1ec41-249">SignalR Githubu a ukázky</span><span class="sxs-lookup"><span data-stu-id="1ec41-249">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="1ec41-250">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="1ec41-250">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="1ec41-251">Podrobný postup nasazení aplikace SignalR do Azure, najdete v části [pomocí SignalR s webovými aplikacemi ve službě Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="1ec41-251">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="1ec41-252">Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="1ec41-252">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span></span>
