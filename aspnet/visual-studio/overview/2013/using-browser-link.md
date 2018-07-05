---
uid: visual-studio/overview/2013/using-browser-link
title: Použití funkce Browser Link v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 96add0de1c1e4366353137898f1ba102aec7f754
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399318"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="e8372-102">Použití funkce Browser Link v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e8372-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="e8372-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e8372-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e8372-104">Browser Link je nová funkce v sadě Visual Studio 2013, která vytvoří komunikační kanál mezi vývojové prostředí a jeden nebo více webových prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="e8372-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="e8372-105">Můžete použít odkaz prohlížeče pro aktualizaci webové aplikace v několika prohlížečích najednou, což je užitečné pro testování prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="e8372-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="e8372-106">Prohlížeč-obnovit</span><span class="sxs-lookup"><span data-stu-id="e8372-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="e8372-107">Zobrazení řídicím panelu odkazů prohlížeče</span><span class="sxs-lookup"><span data-stu-id="e8372-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="e8372-108">Povolení Browser Link pro soubory statický kód HTML</span><span class="sxs-lookup"><span data-stu-id="e8372-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="e8372-109">Zakázání Browser Link</span><span class="sxs-lookup"><span data-stu-id="e8372-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="e8372-110">Jak to funguje?</span><span class="sxs-lookup"><span data-stu-id="e8372-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="e8372-111">Prohlížeč-obnovit</span><span class="sxs-lookup"><span data-stu-id="e8372-111">Browser Refresh</span></span>

<span data-ttu-id="e8372-112">S aktualizovat prohlížeč můžete aktualizovat více prohlížečů, které jsou připojené ke službě Visual Studio prostřednictvím odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e8372-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="e8372-113">Pokud chcete použít, aktualizujte prohlížeč, nejprve vytvořte aplikaci ASP.NET pomocí kteréhokoli z šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="e8372-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="e8372-114">Ladění aplikace pomocí klávesy F5 nebo kliknutím na ikonu šipky v panelu nástrojů:</span><span class="sxs-lookup"><span data-stu-id="e8372-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="e8372-115">Můžete také použijete rozevírací seznam pro výběr určitého webového prohlížeče pro ladění.</span><span class="sxs-lookup"><span data-stu-id="e8372-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="e8372-116">Chcete-li ladit s více prohlížečů, vyberte **procházet s**.</span><span class="sxs-lookup"><span data-stu-id="e8372-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="e8372-117">V **procházet s** dialogového okna, podržte stisknutou klávesu CTRL k výběru více než jeden prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="e8372-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="e8372-118">Klikněte na tlačítko **Procházet** ladění vybrané prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e8372-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="e8372-119">Browser Link funguje také v případě spusťte prohlížeč mimo aplikaci Visual Studio a přejděte na adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="e8372-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="e8372-120">Browser Link ovládací prvky jsou umístěny v rozevírací nabídce se na ikonu Kruhové šipky.</span><span class="sxs-lookup"><span data-stu-id="e8372-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="e8372-121">Ikona šipky je **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8372-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="e8372-122">Pokud chcete zobrazit, které prohlížeče jsou připojené, najeďte myší **aktualizovat** tlačítko během ladění.</span><span class="sxs-lookup"><span data-stu-id="e8372-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="e8372-123">Propojených prohlížečů jsou zobrazeny v okně popisek.</span><span class="sxs-lookup"><span data-stu-id="e8372-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="e8372-124">Chcete-li aktualizovat propojené prohlížeče, klikněte na tlačítko **aktualizovat** tlačítko nebo stiskněte kombinaci kláves CTRL + ALT + ENTER.</span><span class="sxs-lookup"><span data-stu-id="e8372-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="e8372-125">Například následující snímek obrazovky ukazuje projekt ASP.NET, který jsem vytvořil pomocí projektu šablony MVC 5.</span><span class="sxs-lookup"><span data-stu-id="e8372-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="e8372-126">Můžete zobrazit aplikace spuštěná v prohlížečů v horní části.</span><span class="sxs-lookup"><span data-stu-id="e8372-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="e8372-127">V dolní části je projekt otevřít v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8372-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="e8372-128">V sadě Visual Studio po změně &lt;h1&gt; nadpisu na domovské stránce:</span><span class="sxs-lookup"><span data-stu-id="e8372-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="e8372-129">Když jsem kliknul / a **aktualizovat** tlačítko, změny se objevil v obě okna prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="e8372-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="e8372-130">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="e8372-130">**Notes**</span></span>

- <span data-ttu-id="e8372-131">Chcete-li povolit Browser Link, nastavte `debug=true` v [ &lt;kompilace&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element v souboru Web.config v projektu.</span><span class="sxs-lookup"><span data-stu-id="e8372-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="e8372-132">Aplikace musí být spuštěná v místním hostiteli.</span><span class="sxs-lookup"><span data-stu-id="e8372-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="e8372-133">Aplikace musí cílit na .NET 4.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e8372-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="e8372-134">Zobrazení řídicím panelu odkazů prohlížeče</span><span class="sxs-lookup"><span data-stu-id="e8372-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="e8372-135">Browser Link řídicí panel s informacemi o připojení odkazů prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e8372-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="e8372-136">Chcete-li zobrazit řídicí panel, vyberte rozevírací nabídce Browser Link (na malou šipku vedle položky **aktualizovat** tlačítko).</span><span class="sxs-lookup"><span data-stu-id="e8372-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="e8372-137">Pak klikněte na tlačítko **řídicím odkazů prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="e8372-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="e8372-138">Řídicí panel obsahuje seznam propojených prohlížečů a adresu URL, na kterou má přešli každým prohlížečem.</span><span class="sxs-lookup"><span data-stu-id="e8372-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="e8372-139">**Požadavky** část ukazuje všechny kroky potřebnými k povolení Browser Link pro daný projekt.</span><span class="sxs-lookup"><span data-stu-id="e8372-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="e8372-140">Například následující snímek obrazovky ukazuje projektu, kde "debug" je nastavena na hodnotu false v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="e8372-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="e8372-141">Povolení Browser Link pro soubory statický kód HTML</span><span class="sxs-lookup"><span data-stu-id="e8372-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="e8372-142">Chcete-li povolit Browser Link pro statické soubory HTML, přidejte do souboru Web.config následující.</span><span class="sxs-lookup"><span data-stu-id="e8372-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="e8372-143">Z důvodů výkonu odeberte toto nastavení při publikování projektu.</span><span class="sxs-lookup"><span data-stu-id="e8372-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="e8372-144">Zakázání Browser Link</span><span class="sxs-lookup"><span data-stu-id="e8372-144">Disabling Browser Link</span></span>

<span data-ttu-id="e8372-145">Browser Link je standardně povolená.</span><span class="sxs-lookup"><span data-stu-id="e8372-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="e8372-146">Existuje několik způsobů, jak zakázat:</span><span class="sxs-lookup"><span data-stu-id="e8372-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="e8372-147">V rozevírací nabídce Browser Link, zrušte zaškrtnutí políčka **Povolit odkaz prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="e8372-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="e8372-148">V souboru Web.config přidejte klíč s názvem "vs: EnableBrowserLink" s hodnotou "false" v sekci appSettings.</span><span class="sxs-lookup"><span data-stu-id="e8372-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="e8372-149">V souboru Web.config nastavte ladění na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="e8372-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="e8372-150">Jak to funguje?</span><span class="sxs-lookup"><span data-stu-id="e8372-150">How Does It Work?</span></span>

<span data-ttu-id="e8372-151">Browser Link používá [SignalR](../../../signalr/index.md) vytvořit komunikační kanál mezi Visual Studio a prohlížečem.</span><span class="sxs-lookup"><span data-stu-id="e8372-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="e8372-152">Když je povolené Browser Link, Visual Studio funguje jako více klientů (prohlížečů) můžete připojit k serveru funkce SignalR.</span><span class="sxs-lookup"><span data-stu-id="e8372-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="e8372-153">Browser Link také zaregistruje modul HTTP pomocí technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e8372-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="e8372-154">Tento modul Vloží speciální &lt;skript&gt; odkazy do každého požadavku stránky ze serveru.</span><span class="sxs-lookup"><span data-stu-id="e8372-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="e8372-155">Zobrazí se odkazy na skript tak, že vyberete "Zdroj zobrazení" v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="e8372-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="e8372-156">Zdrojové soubory se nemění.</span><span class="sxs-lookup"><span data-stu-id="e8372-156">Your source files are not modified.</span></span> <span data-ttu-id="e8372-157">Modul HTTP dynamicky vkládá skriptových odkazů.</span><span class="sxs-lookup"><span data-stu-id="e8372-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="e8372-158">Protože kód prohlížeče straně je všechny JavaScript, funguje ve všech prohlížečích, které [podporuje SignalR](../../../signalr/overview/getting-started/supported-platforms.md), bez nutnosti jakékoli modul plug-in prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e8372-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
