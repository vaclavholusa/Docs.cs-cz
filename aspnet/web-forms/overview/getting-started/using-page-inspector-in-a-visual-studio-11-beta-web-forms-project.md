---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Díky nástroji Page Inspector pro sadu Visual Studio 2012 v rozhraní ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče. Vyberte libovolný element v integrovaném prohlížeči a nástroj Page Inspector...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28040324"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="7611f-104">Díky nástroji Page Inspector pro sadu Visual Studio 2012 v webové formuláře ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7611f-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="7611f-105">podle Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="7611f-105">by Tim Ammann</span></span>

> <span data-ttu-id="7611f-106">Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7611f-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="7611f-107">Vyberte libovolný element, v prohlížeči integrované a nástroj Page Inspector okamžitě klade důraz elementu zdroje a šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="7611f-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="7611f-108">Můžete procházet všechny stránky v aplikaci, rychle najít zdroje vykreslované značky a pomocí nástroje prohlížeče přímo v prostředí Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7611f-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="7611f-109">Tento kurz shwos povolení režimu kontroly a rychle najít a upravit pravidla šablon stylů CSS a text v rámci svého webového projektu.</span><span class="sxs-lookup"><span data-stu-id="7611f-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="7611f-110">Tento kurz používá projekt webové formuláře aplikace, ale můžete použít také nástroje Page Inspector pro webové projekty a [MVC](https://go.microsoft.com/?linkid=9802002) aplikace.</span><span class="sxs-lookup"><span data-stu-id="7611f-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="7611f-111">Tento kurz obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="7611f-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="7611f-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7611f-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="7611f-113">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7611f-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="7611f-114">Chcete-li zobrazit aplikaci použít nástroj Page Inspector</span><span class="sxs-lookup"><span data-stu-id="7611f-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="7611f-115">Povolit režim kontroly</span><span class="sxs-lookup"><span data-stu-id="7611f-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="7611f-116">Použijte nástroj Page Inspector provádět změny kódu</span><span class="sxs-lookup"><span data-stu-id="7611f-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="7611f-117">Režim kontroly a okno HTML</span><span class="sxs-lookup"><span data-stu-id="7611f-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="7611f-118">Zobrazení náhledu změn šablon stylů CSS v okně Styly</span><span class="sxs-lookup"><span data-stu-id="7611f-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="7611f-119">Automatická synchronizace šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="7611f-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="7611f-120">Výběr barvy šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="7611f-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="7611f-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7611f-121">Prerequisites</span></span>

- <span data-ttu-id="7611f-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) nebo [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="7611f-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="7611f-123">Chcete-li získat nejnovější verzi nástroje Page Inspector, použijte [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=255386) nainstalovat sadu Azure SDK pro rozhraní .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="7611f-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="7611f-124">Nástroj Page Inspector je instalován s Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="7611f-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="7611f-125">Nejnovější verze je 1.3.</span><span class="sxs-lookup"><span data-stu-id="7611f-125">The latest version is 1.3.</span></span> <span data-ttu-id="7611f-126">Zjištění verze máte, spusťte Visual Studio a vyberte **o sadě Microsoft Visual Studio** z **pomoci** nabídky.</span><span class="sxs-lookup"><span data-stu-id="7611f-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="7611f-127">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="7611f-127">Create a Web Application</span></span>

<span data-ttu-id="7611f-128">Nejdřív vytvoříte webovou aplikaci, která nástroj Page Inspector se bude používat.</span><span class="sxs-lookup"><span data-stu-id="7611f-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="7611f-129">V sadě Visual Studio, vyberte **soubor** &gt; **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="7611f-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="7611f-130">Na levé straně, rozbalte položku **Visual C#**, vyberte **webové**a potom vyberte **aplikaci webových formulářů ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="7611f-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Novou aplikaci Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="7611f-132">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="7611f-132">Click **OK**.</span></span>

<span data-ttu-id="7611f-133">Aplikace se otevře v **zdroj** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7611f-133">The application opens in **Source** view.</span></span>

![Novou aplikaci Web Forms v zobrazení zdroje](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="7611f-135">Teď, když máte aplikaci pro práci s, můžete použít nástroj Page Inspector prozkoumat a upravit ho.</span><span class="sxs-lookup"><span data-stu-id="7611f-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="7611f-136">Chcete-li zobrazit aplikaci použít nástroj Page Inspector</span><span class="sxs-lookup"><span data-stu-id="7611f-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="7611f-137">V dalším kroku se zobrazí aplikace s nástrojem Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="7611f-138">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a potom zvolte **zobrazení v nástroj Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="7611f-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Zobrazení v nástroj Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="7611f-140">Ve výchozím nastavení kdy nástroj Page Inspector spouští poprvé, je ukotven jako úzké okno na levé straně prostředí Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7611f-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="7611f-141">Ponechte ukotven na levé straně a nastavte ji na šířku, který je možnost pro vás, nebo ho v jednom z oblastí, které nástroj ukotvení na nahoru, dolů nebo vpravo:</span><span class="sxs-lookup"><span data-stu-id="7611f-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Nástroj Page Inspector pozice ukotvení](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="7611f-143">Pokud uvolníte okno nástroj Page Inspector, můžete umístit ji mimo Visual Studio, nebo i na druhém monitoru Pokud nemáte.</span><span class="sxs-lookup"><span data-stu-id="7611f-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="7611f-144">Však v pořadí, v jakém ALT + TAB mezi nástroj Page Inspector a Visual Studio při okno nástroj Page Inspector není ukotvený, přejděte do **nástroje** &gt; **možnosti** &gt;  **Prostředí** &gt; **karty a okna**a v části **kartě dobře**, zrušte zaškrtnutí políčka názvem **plovoucí nástroj windows vždy zůstat na hlavní okno**:</span><span class="sxs-lookup"><span data-stu-id="7611f-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Zrušením zaškrtnutí plovoucí windows nástroj ALT + TAB mezi odpojenou okno nástroj Page Inspector a Visual Studio](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="7611f-146">Horní podokno okna nástroje Page Inspector ukazuje aktuální stránky v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7611f-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="7611f-147">V dolním podokně zobrazí stránku v značka jazyka HTML na levé straně a některé karty na pravé straně, která umožňují kontrolovat různé aspekty stránky.</span><span class="sxs-lookup"><span data-stu-id="7611f-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="7611f-148">Dolní podokno je podobná [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) v aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="7611f-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="7611f-149">(Ale na rozdíl od nástrojů pro vývojáře, můžete nástroj Page Inspector přímo v sadě Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="7611f-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspektor stránek](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="7611f-151">V tomto kurzu budete používat v podokně Prohlížeč nástroje Page Inspector a **HTML** a **styly** karty vám pomohou rychle přejděte a provést změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="7611f-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="7611f-152">Povolit režim kontroly</span><span class="sxs-lookup"><span data-stu-id="7611f-152">Enable Inspection Mode</span></span>

<span data-ttu-id="7611f-153">V dalším kroku uvidíte, jak funguje nástroj Page Inspector kontroly režimu.</span><span class="sxs-lookup"><span data-stu-id="7611f-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="7611f-154">V okně nástroje Page Inspector klikněte na **kontroly** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7611f-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Kontrola – Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="7611f-156">Informace o režimu kontroly v akci, přesuňte ukazatel myši přes různé části stránky v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="7611f-157">Jak to uděláte, nezmění na velké znaménko plus a zvýrazní se element pod:</span><span class="sxs-lookup"><span data-stu-id="7611f-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Ukazatele myši div.content obálky](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="7611f-159">Jako ukazatel myši přesunete, Všimněte si, že</span><span class="sxs-lookup"><span data-stu-id="7611f-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="7611f-160">Obsah **zdroj** zobrazit změny zobrazit značku odpovídající vybraný element na stránce.</span><span class="sxs-lookup"><span data-stu-id="7611f-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="7611f-161">Je označený relevantní značek.</span><span class="sxs-lookup"><span data-stu-id="7611f-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="7611f-162">Pokud zdroj je v jiném souboru, je tento soubor otevřít v zobrazení zdroje se zvýrazněnou relevantní značkami.</span><span class="sxs-lookup"><span data-stu-id="7611f-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="7611f-163">Kód zobrazí v **HTML** ve nástroj Page Inspector také změní tak, aby odpovídaly vybraný element na stránce.</span><span class="sxs-lookup"><span data-stu-id="7611f-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="7611f-164">V **HTML** kartě popsané relevantní značek.</span><span class="sxs-lookup"><span data-stu-id="7611f-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="7611f-165">**Styly** kartě se zobrazují pravidla šablon stylů CSS relevantní pro aktuální výběr.</span><span class="sxs-lookup"><span data-stu-id="7611f-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="7611f-166">Použijte nástroj Page Inspector provádět změny kódu</span><span class="sxs-lookup"><span data-stu-id="7611f-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="7611f-167">Nyní uvidíte, jak můžete použít nástroj Page Inspector k vyhledání a provést změny kódu nebo text, jehož umístění nemusí být hned zjevné.</span><span class="sxs-lookup"><span data-stu-id="7611f-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="7611f-168">Nástroj Page Inspector uvést do režimu kontroly a potom přejděte k dolnímu okraji domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="7611f-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="7611f-169">Jakmile zadáte oblasti zápatí, otevře se nástroj Page Inspector *Site.Master* rozložení souboru v **zdroje** zobrazení na dočasné kartě napravo od dalších karty a zvýrazňuje části hlavní stránky, které Vybrali jste.</span><span class="sxs-lookup"><span data-stu-id="7611f-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="7611f-170">To ukazuje, jak nástroj Page Inspector můžete najít a zobrazit obsah na stránku, která ve skutečnosti může pocházet z jiného souboru než ten, který původně otevřel.</span><span class="sxs-lookup"><span data-stu-id="7611f-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Označuje zápatí v režimu kontroly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="7611f-172">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad řádek s copyrightu <a id="a"> </a>Všimněte si.</span><span class="sxs-lookup"><span data-stu-id="7611f-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="7611f-173">V *Site.Master* stránky, odpovídající řádek zvýrazněna.</span><span class="sxs-lookup"><span data-stu-id="7611f-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Zápatí autorským řádek zvýrazněna](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="7611f-175">Přidejte na konec řádku v nějaký text *Site.Master* souboru.</span><span class="sxs-lookup"><span data-stu-id="7611f-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="7611f-176">&lt;p&gt;&amp;kopírovat; &lt;%: DateTime.Now.Year %&gt; -Moje aplikace skály ASP.NET!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="7611f-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="7611f-177">Nyní stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace a zobrazte výsledky v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moje aplikace skály ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="7611f-179">Vám může mít chápat, že zápatí byla na *Default.aspx* stránky, ale ukázalo v hlavní rozložení stránky a nástroj Page Inspector zjistila, že pro vás.</span><span class="sxs-lookup"><span data-stu-id="7611f-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="7611f-180">Režim kontroly a okno HTML</span><span class="sxs-lookup"><span data-stu-id="7611f-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="7611f-181">V dalším kroku bude mít rychlý pohled na okno HTML a jak mapuje prvky pro vás.</span><span class="sxs-lookup"><span data-stu-id="7611f-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="7611f-182">Nástroj Page Inspector uvést do režimu kontroly.</span><span class="sxs-lookup"><span data-stu-id="7611f-182">Put Page Inspector in Inspection Mode.</span></span>

![Kontrola – Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="7611f-184">Klikněte na horní část stránky, která říká "zde bude vaše logo".</span><span class="sxs-lookup"><span data-stu-id="7611f-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="7611f-185">Jsou zkoumání určitý element podrobněji tak zobrazení v okně prohlížeče už změní, když ukazatel myši přesunete.</span><span class="sxs-lookup"><span data-stu-id="7611f-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="7611f-186">Nyní přesunutí ukazatele myši **HTML** okno.</span><span class="sxs-lookup"><span data-stu-id="7611f-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="7611f-187">Jako ukazatel myši přesunete, nástroj Page Inspector popisuje element v rámci **HTML** okno a zvýrazňuje odpovídající element v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7611f-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML Window](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="7611f-189">Jak před, otevře se nástroj Page Inspector *Site.Master* souboru můžete na kartě dočasné. Klikněte na kartu Site.Master a zvýrazní se odpovídající kód v &lt;záhlaví&gt; části:</span><span class="sxs-lookup"><span data-stu-id="7611f-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Zvýrazněná značka](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="7611f-191">Zobrazení náhledu změn šablon stylů CSS v okně Styly</span><span class="sxs-lookup"><span data-stu-id="7611f-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="7611f-192">V dalším kroku uvidíte, jak můžete použít nástroj Page Inspector **styly** okna zobrazení náhledu změn do šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="7611f-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="7611f-193">Klikněte **kontroly** tlačítko uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="7611f-194">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek.</span><span class="sxs-lookup"><span data-stu-id="7611f-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Ukazatele myši na elementy](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="7611f-196">Klikněte v části div.content obálku jednou a poté přesuňte ukazatel myši na **styly** okno.</span><span class="sxs-lookup"><span data-stu-id="7611f-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="7611f-197">V části volič třídy .featured .content obálky zrušte a zaškrtněte políčko pro vlastnost barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="7611f-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Barva pozadí vymazat](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="7611f-199">Všimněte si, jak tato změna přináší náhled okamžitě v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="7611f-200">Zaškrtněte políčko znovu, klikněte dvakrát na hodnotu vlastnosti a změňte ji na `red`.</span><span class="sxs-lookup"><span data-stu-id="7611f-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="7611f-201">Změna ukazuje okamžitě:</span><span class="sxs-lookup"><span data-stu-id="7611f-201">The change shows immediately:</span></span>

![Barva pozadí Red](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="7611f-203">**Styly** okno díky usnadňuje testování a náhled šablon stylů CSS změní před potvrzením změny styl list sám sebe.</span><span class="sxs-lookup"><span data-stu-id="7611f-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="7611f-204">Automatická synchronizace šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="7611f-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="7611f-205">Tato funkce vyžaduje 1.3 verzi nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="7611f-206">Automatická synchronizace šablon stylů CSS funkce umožňuje přímo upravit soubor CSS a podívejte se změny okamžitě v prohlížeči nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="7611f-207">Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="7611f-208">V prohlížeči nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek.</span><span class="sxs-lookup"><span data-stu-id="7611f-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="7611f-209">Klikněte na tlačítko jednou tento element vyberte.</span><span class="sxs-lookup"><span data-stu-id="7611f-209">Click once to select this element.</span></span>

<span data-ttu-id="7611f-210">**Syles** v okně se zobrazí všechna pravidla šablon stylů CSS pro tento element.</span><span class="sxs-lookup"><span data-stu-id="7611f-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="7611f-211">Posuňte se dolů a najít .featured .content obálku třída selektor.</span><span class="sxs-lookup"><span data-stu-id="7611f-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="7611f-212">Klikněte na ".featured .content obálku".</span><span class="sxs-lookup"><span data-stu-id="7611f-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="7611f-213">Nástroj Page Inspector otevře soubor CSS, který definuje tento styl (Site.css) a zvýrazňuje odpovídající stylu CSS.</span><span class="sxs-lookup"><span data-stu-id="7611f-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Soubor CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="7611f-215">Teď změňte hodnotu `background-color` na "red".</span><span class="sxs-lookup"><span data-stu-id="7611f-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="7611f-216">Změny se okamžitě zobrazí v prohlížeči nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="7611f-216">The change appears immediately in the Page Inspector browser.</span></span>

![Nástroj Page Inspector prohlížeče](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="7611f-218">Výběr barvy šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="7611f-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="7611f-219">Dále budete Další informace o použití nástroje Page Inspector rychle najít a změnit šablon stylů CSS v výchozí aplikace zvýrazněný text.</span><span class="sxs-lookup"><span data-stu-id="7611f-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="7611f-220">V tomto příkladu jste se rozhodli, že nemáte jako modře zvýrazněný příklad a chcete ho změnit na jinou barvu.</span><span class="sxs-lookup"><span data-stu-id="7611f-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="7611f-221">Klikněte **kontroly** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7611f-221">Click the **Inspect** button.</span></span>

![Kontrola – Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="7611f-223">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad zvýrazněnou "videa, kurzy a ukázky" text tak, aby CSS "označením" Popisek se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="7611f-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Ukazatele myši značky elementu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="7611f-225">Kliknutím na text vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="7611f-225">Click the text to select it.</span></span> <span data-ttu-id="7611f-226">V dolní části se zobrazí odpovídající značky selektor šablon stylů CSS **styly** okno.</span><span class="sxs-lookup"><span data-stu-id="7611f-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Označit pro výběr v okně Styly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="7611f-228">Klikněte na označit selektor.</span><span class="sxs-lookup"><span data-stu-id="7611f-228">Click the mark selector.</span></span> <span data-ttu-id="7611f-229">Tím se otevře *Site.css* souboru pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7611f-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="7611f-230">Klikněte na kartu Site.css a zvýrazní se odpovídající šablon stylů CSS pro selektor:</span><span class="sxs-lookup"><span data-stu-id="7611f-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Označit selektor v šabloně stylů.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="7611f-232">Vyberte a odeberte řádku s vlastností barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="7611f-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="7611f-233">Teď použijete nové volby barev Visual Studio 2012 CSS vybrat nový barvu pro **označit** vlastnost barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="7611f-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="7611f-234">Pomocí volby barev služby Visual Studio 2012 šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="7611f-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="7611f-235">Editor šablon stylů CSS v sadě Visual Studio 2012 má výběr barvy, která usnadňuje zvolte a vložit barvy.</span><span class="sxs-lookup"><span data-stu-id="7611f-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="7611f-236">Obsahuje výběr "pop rozbalovací", která nabízí jemnějšího ovládání a jednoduchý pruhu barev.</span><span class="sxs-lookup"><span data-stu-id="7611f-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="7611f-237">Výběr barvy obsahuje standardní paletu barev, podporuje názvy standardní barev, kódů hash, barvy RGB, RGBA, HSL a HSLA a udržuje seznam barev, které byly naposledy použity v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7611f-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="7611f-238">V řádku, kde byla vlastnost Barva pozadí zadejte "bc" a stiskněte jednou na šipku dolů.</span><span class="sxs-lookup"><span data-stu-id="7611f-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="7611f-239">Když zadáte první znak každého slova ve vlastnosti oddělené pomlčkou jako "background-color", filtry IntelliSense v seznamu zobrazit pouze vlastnosti, které odpovídají:</span><span class="sxs-lookup"><span data-stu-id="7611f-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense filtrované hodnoty](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="7611f-241">Nyní zadejte středník.</span><span class="sxs-lookup"><span data-stu-id="7611f-241">Now type a colon.</span></span> <span data-ttu-id="7611f-242">Když to uděláte, vloží se název vlastnosti úplné barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="7611f-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="7611f-243">Typ **#** nebo **rgb (**, a zobrazí se na panelu Výběr barev:</span><span class="sxs-lookup"><span data-stu-id="7611f-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Na panelu Výběr barev šablon stylů CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="7611f-245">Pokud chcete zjistit, jak funguje pruhu barev výběr, klikněte na jeho barvy s ukazatelem myši nebo stiskněte klávesu šipka dolů a pak použijte klávesy se šipkami vlevo a vpravo procházení barvy.</span><span class="sxs-lookup"><span data-stu-id="7611f-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="7611f-246">Při návštěvě barvu, která je zobrazen náhled s odpovídající hodnotou pro vlastnost background-color:</span><span class="sxs-lookup"><span data-stu-id="7611f-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![Hodnota vlastnosti Barva pozadí zobrazení náhledu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="7611f-248">V tomto okamžiku by mohla stisknutím klávesy Enter vyberte hodnotu a pak středníkem (;) k dokončení položka šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="7611f-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="7611f-249">Teď přejděte k další části, aby mohli zobrazit, jak funguje barva výběru pop nižší.</span><span class="sxs-lookup"><span data-stu-id="7611f-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="7611f-250">Pomocí Pop nižší výběr barev</span><span class="sxs-lookup"><span data-stu-id="7611f-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="7611f-251">Když pruhu barev nemá přesný barvu, která hledáte, můžete pro výběr barvy rozbalovací pop.</span><span class="sxs-lookup"><span data-stu-id="7611f-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="7611f-252">Otevřete ho kliknutím na dvojitou šipku na pravém konci pruhu barev nebo stiskněte klávesu šipka dolů jednou nebo dvakrát na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="7611f-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Výběr rozbalovací Pop barva šablon stylů CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="7611f-254">Klikněte na barvu ze svislá čára na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="7611f-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="7611f-255">Ukazuje to přechodu pro tuto barvu v hlavním okně.</span><span class="sxs-lookup"><span data-stu-id="7611f-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="7611f-256">Vybrat barvu přímo z panelu svislé stisknutím klávesy Enter nebo klikněte na libovolný bod v hlavním okně zvolit s vyšší přesností.</span><span class="sxs-lookup"><span data-stu-id="7611f-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="7611f-257">Pokud je na obrazovce počítače, který chcete použít barvu (nemusí to být v uživatelském rozhraní sady Visual Studio), jeho hodnota můžete zaznamenat pomocí nástroje kapátko vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="7611f-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="7611f-258">Také můžete změnit krytí barvu přesunutím posuvníku v dolní části volby barev.</span><span class="sxs-lookup"><span data-stu-id="7611f-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="7611f-259">V tom změny barvu hodnoty RGBA hodnoty, protože formát RGBA může představovat neprůhlednost.</span><span class="sxs-lookup"><span data-stu-id="7611f-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="7611f-260">Po výběru barvy, stiskněte klávesu Enter a pak zadejte středníkem k dokončení barvu pozadí položky v *Site.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="7611f-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="7611f-261">Na stránce Inspector aktualizace panelu</span><span class="sxs-lookup"><span data-stu-id="7611f-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="7611f-262">Nástroj Page Inspector okamžitě zjistí změnu *Site.css* souboru (nebo všechny soubory v aplikaci) a zobrazí upozornění na aktualizace panelu.</span><span class="sxs-lookup"><span data-stu-id="7611f-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Aktualizace panelu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="7611f-264">Pokud chcete uložit všechny soubory a aktualizujte stránku prohlížeče nástroj Page Inspector, stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7611f-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="7611f-265">Změna v barva zvýraznění se zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="7611f-265">The change in the highlight color appears in the browser:</span></span>

![Barva zvýraznění změnit](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="7611f-267">Všimněte si, nemusí aktualizovat prohlížeč nástroje Page Inspector přímo na v prostředí Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7611f-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="7611f-268">Díky nástroji Page Inspector místo externího prohlížeče umožňuje zůstat v editoru při vývoji webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7611f-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="7611f-269">Viz také</span><span class="sxs-lookup"><span data-stu-id="7611f-269">See Also</span></span>

<span data-ttu-id="7611f-270">[Představení nástroje Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="7611f-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="7611f-271">[Nástroj Page Inspector chybové zprávy](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="7611f-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
