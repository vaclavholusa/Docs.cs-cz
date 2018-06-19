---
uid: visual-studio/overview/2013/using-browser-link
title: Pomocí odkazů prohlížeče v sadě Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044107"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="6a778-102">Pomocí odkazů prohlížeče v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6a778-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="6a778-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6a778-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6a778-104">Browser Link je nová funkce v sadě Visual Studio 2013 vytvářející komunikační kanál mezi vývojového prostředí a jeden nebo více webových prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="6a778-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="6a778-105">Můžete použít Browser Link aktualizujte svoji webovou aplikaci v několika prohlížeče najednou, což je užitečné pro testování různých prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="6a778-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="6a778-106">Obnovit v prohlížeči</span><span class="sxs-lookup"><span data-stu-id="6a778-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="6a778-107">Zobrazení řídicím panelu odkazů prohlížeče</span><span class="sxs-lookup"><span data-stu-id="6a778-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="6a778-108">Povolení odkazů prohlížeče pro soubory statické HTML</span><span class="sxs-lookup"><span data-stu-id="6a778-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="6a778-109">Zakázání Browser Link</span><span class="sxs-lookup"><span data-stu-id="6a778-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="6a778-110">Jak to funguje?</span><span class="sxs-lookup"><span data-stu-id="6a778-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="6a778-111">Obnovit v prohlížeči</span><span class="sxs-lookup"><span data-stu-id="6a778-111">Browser Refresh</span></span>

<span data-ttu-id="6a778-112">Pomocí prohlížeče aktualizovat můžete obnovit více prohlížečů, které jsou připojené k sadě Visual Studio prostřednictvím odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6a778-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="6a778-113">Pokud chcete použít, aktualizujte prohlížeč, nejprve vytvořte aplikace ASP.NET pomocí kteréhokoli z šablon projektu.</span><span class="sxs-lookup"><span data-stu-id="6a778-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="6a778-114">Ladění aplikace stisknutím klávesy F5 nebo kliknutím na ikonu šipky v panelu nástrojů:</span><span class="sxs-lookup"><span data-stu-id="6a778-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="6a778-115">Můžete taky rozevíracího seznamu vyberte konkrétní prohlížeč pro ladění.</span><span class="sxs-lookup"><span data-stu-id="6a778-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="6a778-116">Chcete-li ladit s více prohlížečů, vyberte **procházet s**.</span><span class="sxs-lookup"><span data-stu-id="6a778-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="6a778-117">V **procházet s** dialogové okno, podržíte stisknutou klávesu CTRL k výběru více než jeden prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6a778-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="6a778-118">Klikněte na tlačítko **Procházet** k ladění u vybrané prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="6a778-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="6a778-119">Browser Link lze použít také v případě spusťte prohlížeč mimo aplikaci Visual Studio a přejděte na adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a778-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="6a778-120">Ovládací prvky Browser Link jsou umístěny v rozevírací nabídce s ikonou cyklické šipku.</span><span class="sxs-lookup"><span data-stu-id="6a778-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="6a778-121">Je na ikonu šipky **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a778-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="6a778-122">Pokud chcete zjistit, které prohlížeče jsou připojeny, najeďte myší **aktualizovat** tlačítko při ladění.</span><span class="sxs-lookup"><span data-stu-id="6a778-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="6a778-123">Připojené prohlížeče se zobrazí v okně popisu.</span><span class="sxs-lookup"><span data-stu-id="6a778-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="6a778-124">Chcete-li aktualizovat připojené prohlížeče, klikněte na tlačítko **aktualizovat** tlačítko nebo stiskněte klávesu CTRL + ALT + ENTER.</span><span class="sxs-lookup"><span data-stu-id="6a778-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="6a778-125">Například následující snímek obrazovky ukazuje projekt ASP.NET, který byl vytvořen pomocí šablony projektu MVC 5.</span><span class="sxs-lookup"><span data-stu-id="6a778-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="6a778-126">Zobrazí se aplikace spuštěné v prohlížečích dvě v horní části.</span><span class="sxs-lookup"><span data-stu-id="6a778-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="6a778-127">V dolní části je projekt otevřete v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a778-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="6a778-128">V sadě Visual Studio, bylo změněno &lt;h1&gt; záhlaví pro domovskou stránku:</span><span class="sxs-lookup"><span data-stu-id="6a778-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="6a778-129">Když po klepnutí **aktualizovat** tlačítko, změna se objevil v systémech windows prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="6a778-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="6a778-130">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="6a778-130">**Notes**</span></span>

- <span data-ttu-id="6a778-131">Chcete-li povolit Browser Link, nastavte `debug=true` v [ &lt;kompilace&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element v souboru Web.config projektu.</span><span class="sxs-lookup"><span data-stu-id="6a778-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="6a778-132">Aplikace musí být spuštěn na místním hostiteli.</span><span class="sxs-lookup"><span data-stu-id="6a778-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="6a778-133">Aplikace musí mít jako cíl rozhraní .NET 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6a778-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="6a778-134">Zobrazení řídicím panelu odkazů prohlížeče</span><span class="sxs-lookup"><span data-stu-id="6a778-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="6a778-135">Řídicím panelu odkazů prohlížeče zobrazují informace o připojení odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6a778-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="6a778-136">Chcete-li zobrazit řídicí panel, vyberte v rozevírací nabídce Browser Link (na malou šipku vedle položky **aktualizovat** tlačítko).</span><span class="sxs-lookup"><span data-stu-id="6a778-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="6a778-137">Pak klikněte na tlačítko **řídicím odkazů prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="6a778-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="6a778-138">Řídicí panel uvádí připojené prohlížeči a adresu URL, na kterou má přešli každým prohlížečem.</span><span class="sxs-lookup"><span data-stu-id="6a778-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="6a778-139">**Požadavky** části se zobrazují všechny kroky potřebnými k povolení Browser Link pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="6a778-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="6a778-140">Například následující snímek obrazovky ukazuje projektu, kde "ladění" je nastaven na hodnotu false v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="6a778-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="6a778-141">Povolení odkazů prohlížeče pro soubory statické HTML</span><span class="sxs-lookup"><span data-stu-id="6a778-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="6a778-142">Chcete-li povolit Browser Link pro statické soubory HTML, přidejte následující do souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="6a778-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="6a778-143">Z důvodů výkonu odeberte toto nastavení při publikování projektu.</span><span class="sxs-lookup"><span data-stu-id="6a778-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="6a778-144">Zakázání Browser Link</span><span class="sxs-lookup"><span data-stu-id="6a778-144">Disabling Browser Link</span></span>

<span data-ttu-id="6a778-145">Browser Link je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="6a778-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="6a778-146">Zakázat několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="6a778-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="6a778-147">V rozevírací nabídce Browser Link, zrušte zaškrtnutí políčka **povolit Browser Link**.</span><span class="sxs-lookup"><span data-stu-id="6a778-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="6a778-148">V souboru Web.config přidejte klíč s názvem "vs: EnableBrowserLink" s hodnotou "false" v části appSettings.</span><span class="sxs-lookup"><span data-stu-id="6a778-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="6a778-149">V souboru Web.config nastaven na hodnotu false ladění.</span><span class="sxs-lookup"><span data-stu-id="6a778-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="6a778-150">Jak to funguje?</span><span class="sxs-lookup"><span data-stu-id="6a778-150">How Does It Work?</span></span>

<span data-ttu-id="6a778-151">Browser Link používá [SignalR](../../../signalr/index.md) vytvořit komunikační kanál mezi Visual Studio a prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6a778-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="6a778-152">Pokud je povoleno Browser Link, Visual Studio funguje jako server SignalR, více klientů (prohlížeče) mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="6a778-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="6a778-153">Browser Link také zaregistruje modul HTTP s technologií ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6a778-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="6a778-154">Tento modul Vloží speciální &lt;skriptu&gt; odkazy na každý požadavek na stránku ze serveru.</span><span class="sxs-lookup"><span data-stu-id="6a778-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="6a778-155">Uvidíte odkazům na skript tak, že vyberete "Zdroj zobrazení" v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6a778-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="6a778-156">Zdrojové soubory vašeho se nemění.</span><span class="sxs-lookup"><span data-stu-id="6a778-156">Your source files are not modified.</span></span> <span data-ttu-id="6a778-157">Modul HTTP vloží odkazům na skript dynamicky.</span><span class="sxs-lookup"><span data-stu-id="6a778-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="6a778-158">Protože kód prohlížeče na straně je všechny JavaScript, funguje u všech prohlížečů, [SignalR podporuje](../../../signalr/overview/getting-started/supported-platforms.md), bez nutnosti jakékoli modul plug-in prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6a778-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
