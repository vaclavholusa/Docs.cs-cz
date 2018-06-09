---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Díky nástroji Page Inspector v architektuře ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Nástroj Page Inspector v sadě Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče. Vyberte libovolný element v integrovaném prohlížeči a nástroj Page Inspector i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034513"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="acc31-104">Díky nástroji Page Inspector v architektuře ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="acc31-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="acc31-105">podle Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="acc31-105">by Tim Ammann</span></span>

> <span data-ttu-id="acc31-106">Nástroj Page Inspector v sadě Visual Studio 2012 je nástroj pro vývoj webů s integrované prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="acc31-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="acc31-107">Vyberte libovolný element, v prohlížeči integrované a nástroj Page Inspector okamžitě klade důraz elementu zdroje a šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="acc31-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="acc31-108">Můžete procházet všechny zobrazení MVC, rychle najít zdroje vykreslované značky a pomocí nástroje prohlížeče přímo v prostředí Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acc31-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="acc31-109">Podívejte se Video</span><span class="sxs-lookup"><span data-stu-id="acc31-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="acc31-110">Tento kurz ukazuje, jak povolit režim kontroly a rychle najít a upravit kód a šablon stylů CSS v rámci svého webového projektu.</span><span class="sxs-lookup"><span data-stu-id="acc31-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="acc31-111">Tento kurz používá projektu aplikace MVC, ale můžete použít také nástroje Page Inspector pro [webových formulářů](https://go.microsoft.com/?linkid=9802001) a dalších aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="acc31-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="acc31-112">Tento kurz obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="acc31-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="acc31-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="acc31-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="acc31-114">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="acc31-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="acc31-115">Použijte nástroj Page Inspector na Procházet a zobrazení</span><span class="sxs-lookup"><span data-stu-id="acc31-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="acc31-116">Povolit režim kontroly</span><span class="sxs-lookup"><span data-stu-id="acc31-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="acc31-117">Použijte nástroj Page Inspector provádět změny kódu</span><span class="sxs-lookup"><span data-stu-id="acc31-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="acc31-118">Režim kontroly a okno HTML</span><span class="sxs-lookup"><span data-stu-id="acc31-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="acc31-119">Zobrazení náhledu změn šablon stylů CSS v okně Styly</span><span class="sxs-lookup"><span data-stu-id="acc31-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="acc31-120">Automatická synchronizace šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="acc31-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="acc31-121">Výběr barvy šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="acc31-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="acc31-122">Mapování elementů dynamické stránky JavaScript</span><span class="sxs-lookup"><span data-stu-id="acc31-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="acc31-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="acc31-123">Prerequisites</span></span>

- <span data-ttu-id="acc31-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) nebo [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="acc31-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="acc31-125">Chcete-li získat nejnovější verzi nástroje Page Inspector, použijte [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=255386) nainstalovat sadu Windows Azure SDK pro rozhraní .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="acc31-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="acc31-126">Nástroj Page Inspector je instalován s Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="acc31-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="acc31-127">Nejnovější verze je 1.3.</span><span class="sxs-lookup"><span data-stu-id="acc31-127">The latest version is 1.3.</span></span> <span data-ttu-id="acc31-128">Zjištění verze máte, spusťte Visual Studio a vyberte **o sadě Microsoft Visual Studio** z **pomoci** nabídky.</span><span class="sxs-lookup"><span data-stu-id="acc31-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="acc31-129">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="acc31-129">Create a Web Application</span></span>

<span data-ttu-id="acc31-130">Nejprve vytvořte webovou aplikaci, která nástroj Page Inspector se bude používat.</span><span class="sxs-lookup"><span data-stu-id="acc31-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="acc31-131">V sadě Visual Studio, vyberte **soubor** &gt; **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="acc31-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="acc31-132">Na levé straně, rozbalte položku **Visual C#**, vyberte **webové**a potom vyberte **webové aplikace ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="acc31-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nové aplikace ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="acc31-134">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="acc31-134">Click **OK**.</span></span>

<span data-ttu-id="acc31-135">V **nový ASP.NET MVC 4 projekt** dialogové okno, vyberte **Internetové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="acc31-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="acc31-136">Nechte **Razor** jako výchozí modul zobrazení.</span><span class="sxs-lookup"><span data-stu-id="acc31-136">Leave **Razor** as the default view engine.</span></span>

![Nový projekt ASP.NET MVC – Internetové aplikace.](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="acc31-138">Aplikace se otevře v **zdroj** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="acc31-138">The application opens in **Source** view.</span></span>

![Nové aplikace ASP.NET MVC v zobrazení zdroje](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="acc31-140">Teď, když máte aplikaci pro práci s, můžete použít nástroj Page Inspector prozkoumat a upravit ho.</span><span class="sxs-lookup"><span data-stu-id="acc31-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="acc31-141">Použijte nástroj Page Inspector na Procházet a zobrazení</span><span class="sxs-lookup"><span data-stu-id="acc31-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="acc31-142">V sadě Visual Studio 2012, kliknete pravým tlačítkem všechna zobrazení v projektu, vyberte **zobrazení v nástroj Page Inspector**, a nástroj Page Inspector se zjistit trasy a zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="acc31-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="acc31-143">V **Průzkumníku řešení**, rozbalte **zobrazení** složku a potom **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="acc31-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="acc31-144">Klikněte pravým tlačítkem na soubor Index.cshtml a zvolte **zobrazení v nástroj Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="acc31-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Zobrazení Index.cshtml v nástroj Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="acc31-146">Ve výchozím nastavení nástroj Page Inspector ukotven jako okno na levé straně prostředí Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acc31-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="acc31-147">Pokud dáváte přednost, můžete ho jinde ukotvení nebo zrušení okna ukotvení.</span><span class="sxs-lookup"><span data-stu-id="acc31-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="acc31-148">V tématu [postupy: rozvržení a dokování oken](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="acc31-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="acc31-149">Horní podokno okna nástroje Page Inspector ukazuje aktuální stránky v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="acc31-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="acc31-150">V dolním podokně zobrazí stránku v značka jazyka HTML, společně s některé karty, která umožňují kontrolovat různé aspekty stránky.</span><span class="sxs-lookup"><span data-stu-id="acc31-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="acc31-151">Dolní podokno je podobná [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) v aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="acc31-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplikace ASP.NET MVC v nástroj Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="acc31-153">V tomto kurzu budete používat **HTML** a **styly** karet procházejte rychle a provést změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="acc31-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="acc31-154">Režim EnableInspection</span><span class="sxs-lookup"><span data-stu-id="acc31-154">EnableInspection Mode</span></span>

<span data-ttu-id="acc31-155">Chcete-li nástroj Page Inspector uvést do režimu kontroly, klikněte na tlačítko **kontroly** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="acc31-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="acc31-156">V režimu kontroly když podržíte ukazatel myši nad libovolná součást na vykreslené stránce odpovídající zdrojový kód nebo kód zvýrazní.</span><span class="sxs-lookup"><span data-stu-id="acc31-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Přepnout režim kontroly](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="acc31-158">Nyní přesuňte ukazatel myši nad různé části stránky v rámci nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="acc31-159">Jak to uděláte, nezmění na velké znaménko plus a zvýrazní se element pod:</span><span class="sxs-lookup"><span data-stu-id="acc31-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Ukazatele myši div.content obálky](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="acc31-161">Jako ukazatel myši přesunete, Visual Studio označuje odpovídající syntaxi Razor ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="acc31-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="acc31-162">Pokud HTML element pochází z jiného zdrojový soubor, Visual Studio automaticky otevře soubor.</span><span class="sxs-lookup"><span data-stu-id="acc31-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="acc31-163">V nástroj Page Inspector **HTML** kartě se zobrazují HTML, který byl vytvořen z syntaxi Razor.</span><span class="sxs-lookup"><span data-stu-id="acc31-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="acc31-164">Když přesouváte ukazatel myši, jsou vyznačené elementů HTML.</span><span class="sxs-lookup"><span data-stu-id="acc31-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="acc31-165">**Styly** kartě se zobrazují pravidla šablon stylů CSS pro daný element.</span><span class="sxs-lookup"><span data-stu-id="acc31-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="acc31-166">Použijte nástroj Page Inspector provádět změny kódu</span><span class="sxs-lookup"><span data-stu-id="acc31-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="acc31-167">Nástroj Page Inspector umožňuje najít značek, jejichž umístění nemusí být zřejmé.</span><span class="sxs-lookup"><span data-stu-id="acc31-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="acc31-168">Potom můžete upravit kód a zobrazit výsledné změny.</span><span class="sxs-lookup"><span data-stu-id="acc31-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="acc31-169">Chcete-li toto zobrazit, klikněte na tlačítko **kontroly** a posuňte se k dolnímu okraji stránky v okně nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="acc31-170">Při přesunutí ukazatele myši do oblasti zápatí, otevře se nástroj Page Inspector \_Layout.cshtml souboru a zvýrazňuje části ke stránce rozložení, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="acc31-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="acc31-171">Jak vidíte, jsou zápatí je definována v souboru rozložení a není zobrazení.</span><span class="sxs-lookup"><span data-stu-id="acc31-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Zápatí stránky](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="acc31-173">Nyní, přesuňte ukazatel myši na řádek s copyrightu <a id="a"> </a>Všimněte si.</span><span class="sxs-lookup"><span data-stu-id="acc31-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="acc31-174">V \_stránku Layout.cshtml, odpovídající řádek zvýrazněna.</span><span class="sxs-lookup"><span data-stu-id="acc31-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Zápatí autorským řádek zvýrazněna](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="acc31-176">Přidejte na konec řádku v nějaký text \_Layout.cshtml souboru.</span><span class="sxs-lookup"><span data-stu-id="acc31-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="acc31-177">&lt;p&gt;&amp;kopírovat; @DateTime.Now.Year – Moje aplikace ASP.NET MVC skály! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="acc31-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="acc31-178">Nyní stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace a zobrazte výsledky v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Moje aplikace skály ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="acc31-180">Může mít představit zápatí definované v Index.cshtml, že v ukázalo \_Layout.cshtml a nástroj Page Inspector zjistila, že pro vás.</span><span class="sxs-lookup"><span data-stu-id="acc31-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="acc31-181">Režim kontroly a okno HTML</span><span class="sxs-lookup"><span data-stu-id="acc31-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="acc31-182">V dalším kroku bude mít rychlý pohled na okno HTML a jak mapuje prvky pro vás.</span><span class="sxs-lookup"><span data-stu-id="acc31-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="acc31-183">Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="acc31-184">Klikněte na horní část stránky, která říká "logohere".</span><span class="sxs-lookup"><span data-stu-id="acc31-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="acc31-185">Jsou zkoumání určitý element podrobněji tak zobrazení v okně prohlížeče už změní, když ukazatel myši přesunete.</span><span class="sxs-lookup"><span data-stu-id="acc31-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="acc31-186">Nyní přesunutí ukazatele myši **HTML** okno.</span><span class="sxs-lookup"><span data-stu-id="acc31-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="acc31-187">Jako ukazatel myši přesunete, nástroj Page Inspector popisuje element v rámci **HTML** okno a zvýrazňuje odpovídající element v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="acc31-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="acc31-189">Jak před, otevře se nástroj Page Inspector \_Layout.cshtml souboru můžete na kartě dočasné. Klikněte na tlačítko \_Layout.cshtml dočasné kartě a odpovídající kód bude mít zvýrazněná v &lt;záhlaví&gt; části pro vás:</span><span class="sxs-lookup"><span data-stu-id="acc31-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Zvýrazněná značka](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="acc31-191">Zobrazení náhledu změn šablon stylů CSS v okně Styly</span><span class="sxs-lookup"><span data-stu-id="acc31-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="acc31-192">V dalším kroku použijete nástroj Page Inspector **styly** okna zobrazení náhledu změn do šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="acc31-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="acc31-193">Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="acc31-194">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek.</span><span class="sxs-lookup"><span data-stu-id="acc31-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Ukazatele myši div.content obálky](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="acc31-196">Klikněte v části div.content obálku jednou a poté přesuňte ukazatel myši na **styly** okno.</span><span class="sxs-lookup"><span data-stu-id="acc31-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="acc31-197">**Syles** v okně se zobrazí všechna pravidla šablon stylů CSS pro tento element.</span><span class="sxs-lookup"><span data-stu-id="acc31-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="acc31-198">Posuňte se dolů a najít .featured .content obálku třída selektor.</span><span class="sxs-lookup"><span data-stu-id="acc31-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="acc31-199">Nyní zrušte zaškrtnutí políčka pro vlastnost barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="acc31-199">Now clear the checkbox for the background-color property.</span></span>

![Barva pozadí vymazat](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="acc31-201">Všimněte si, jak tato změna přináší náhled okamžitě v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="acc31-202">Zaškrtněte políčko znovu, klikněte dvakrát na hodnotu vlastnosti a změňte ji na červený.</span><span class="sxs-lookup"><span data-stu-id="acc31-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="acc31-203">Změna ukazuje okamžitě:</span><span class="sxs-lookup"><span data-stu-id="acc31-203">The change shows immediately:</span></span>

![Barva pozadí Red](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="acc31-205">**Styly** okno díky usnadňuje testování a náhled šablon stylů CSS změní před potvrzením změny styl list sám sebe.</span><span class="sxs-lookup"><span data-stu-id="acc31-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="acc31-206">Automatická synchronizace šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="acc31-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="acc31-207">Tato funkce vyžaduje 1.3 verzi nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="acc31-208">Automatická synchronizace šablon stylů CSS funkce umožňuje přímo upravit soubor CSS a podívejte se změny okamžitě v prohlížeči nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="acc31-209">Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="acc31-210">V prohlížeči nástroj Page Inspector, přesuňte ukazatel myši nad v části "Home Page", dokud **div.content obálku** se zobrazí popisek.</span><span class="sxs-lookup"><span data-stu-id="acc31-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="acc31-211">Klikněte na tlačítko jednou tento element vyberte.</span><span class="sxs-lookup"><span data-stu-id="acc31-211">Click once to select this element.</span></span>

<span data-ttu-id="acc31-212">**Syles** v okně se zobrazí všechna pravidla šablon stylů CSS pro tento element.</span><span class="sxs-lookup"><span data-stu-id="acc31-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="acc31-213">Posuňte se dolů a najít .featured .content obálku třída selektor.</span><span class="sxs-lookup"><span data-stu-id="acc31-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="acc31-214">Klikněte na ".featured .content obálku".</span><span class="sxs-lookup"><span data-stu-id="acc31-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="acc31-215">Nástroj Page Inspector otevře soubor CSS, který definuje tento styl (Site.css) a zvýrazňuje odpovídající stylu CSS.</span><span class="sxs-lookup"><span data-stu-id="acc31-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="acc31-216">Teď změňte hodnotu `background-color` na "red".</span><span class="sxs-lookup"><span data-stu-id="acc31-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="acc31-217">Změny se okamžitě zobrazí v prohlížeči nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="acc31-218">Výběr barvy šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="acc31-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="acc31-219">Editor šablon stylů CSS v sadě Visual Studio 2012 má výběr barvy, která usnadňuje zvolte a vložit barvy.</span><span class="sxs-lookup"><span data-stu-id="acc31-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="acc31-220">Výběr barvy obsahuje standardní paletu barev, podporuje názvy standardní barev, kódů hash, barvy RGB, RGBA, HSL a HSLA a udržuje seznam barev, které byly naposledy použity v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="acc31-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="acc31-221">V předchozí části, můžete změnit hodnotu `background-color` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="acc31-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="acc31-222">K vyvolání volby barev, umístěte kurzor po název vlastnosti a typ **#** nebo **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="acc31-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Na panelu Výběr barev šablon stylů CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="acc31-224">Klikněte na barvu, která má vyberte ho a stiskněte klávesu šipka dolů a pak použijte klávesy se šipkami vlevo a vpravo procházení barvy.</span><span class="sxs-lookup"><span data-stu-id="acc31-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="acc31-225">Při návštěvě barvy s odpovídající hodnotou šestnáctkových zobrazení náhledu:</span><span class="sxs-lookup"><span data-stu-id="acc31-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![Hodnota vlastnosti Barva pozadí zobrazení náhledu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="acc31-227">Pokud pruhu barev nemá požadovanou barvu přesný, můžete na pop nabídku pro výběr barev.</span><span class="sxs-lookup"><span data-stu-id="acc31-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="acc31-228">Otevřete ho kliknutím na dvojitou šipku na pravém konci pruhu barev nebo stiskněte klávesu šipka dolů jednou nebo dvakrát na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="acc31-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Výběr rozbalovací Pop barva šablon stylů CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="acc31-230">Klikněte na barvu ze svislá čára na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="acc31-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="acc31-231">Ukazuje to přechodu pro tuto barvu v hlavním okně.</span><span class="sxs-lookup"><span data-stu-id="acc31-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="acc31-232">Vybrat barvu přímo z panelu svislé stisknutím klávesy Enter nebo klikněte na libovolný bod v hlavním okně zvolit s vyšší přesností.</span><span class="sxs-lookup"><span data-stu-id="acc31-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="acc31-233">Pokud je na obrazovce počítače, který chcete použít barvu (nemusí to být v uživatelském rozhraní sady Visual Studio), jeho hodnota můžete zaznamenat pomocí nástroje kapátko vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="acc31-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="acc31-234">Také můžete změnit krytí barvu přesunutím posuvníku v dolní části volby barev.</span><span class="sxs-lookup"><span data-stu-id="acc31-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="acc31-235">V tom změny barvu hodnoty RGBA hodnoty, protože formát RGBA může představovat neprůhlednost.</span><span class="sxs-lookup"><span data-stu-id="acc31-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="acc31-236">Po výběru barvy, stiskněte klávesu Enter a pak zadejte středníkem k dokončení barvu pozadí položky v *Site.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="acc31-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="acc31-237">Na stránce Inspector aktualizace panelu</span><span class="sxs-lookup"><span data-stu-id="acc31-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="acc31-238">Nástroj Page Inspector okamžitě zjistí změnu *Site.css* souboru a zobrazí výstrahu panelu aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="acc31-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Aktualizace panelu](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="acc31-240">Pokud chcete uložit všechny soubory a aktualizujte stránku prohlížeče nástroj Page Inspector, stiskněte Ctrl + Alt + Enter nebo klikněte na panelu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="acc31-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="acc31-241">Změna v barva zvýraznění se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="acc31-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="acc31-242">Mapování elementů dynamické stránky JavaScript</span><span class="sxs-lookup"><span data-stu-id="acc31-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="acc31-243">V moderních webových aplikací elementy na stránce jsou často generována dynamicky v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="acc31-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="acc31-244">To znamená, že neexistuje žádná statické značek (HTML nebo Razor), která odpovídá tyto prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="acc31-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="acc31-245">S verzí 1.3 můžete nástroj Page Inspector teď namapovat položky, které byly dynamicky přidat na stránku zpět na odpovídající kód jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="acc31-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="acc31-246">K předvedení tuto funkci, použijeme [jedné stránky aplikace (SPA) šablony](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="acc31-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="acc31-247">Šablona SPA vyžaduje [ASP.NET a webové nástroje 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="acc31-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="acc31-248">V sadě Visual Studio, vyberte **soubor** &gt; **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="acc31-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="acc31-249">Na levé straně, rozbalte položku **Visual C#**, vyberte **webové**a potom vyberte **webové aplikace ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="acc31-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="acc31-250">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="acc31-250">Click **OK**.</span></span>

<span data-ttu-id="acc31-251">V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **jedné stránky aplikace**.</span><span class="sxs-lookup"><span data-stu-id="acc31-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="acc31-252">V Průzkumníku řešení rozbalte **zobrazení** složku a potom **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="acc31-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="acc31-253">Klikněte pravým tlačítkem na soubor Index.cshtml a zvolte **zobrazení v nástroj Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="acc31-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="acc31-254">Nejprve thing tedy zobrazené v prohlížeči nástroj Page Inspector je přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="acc31-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="acc31-255">Klikněte na tlačítko "Zaregistrovat" a vytvořte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="acc31-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="acc31-256">Po registraci aplikace můžete přihlásí a vytvoří seznam úkolů s některé ukázkové položky.</span><span class="sxs-lookup"><span data-stu-id="acc31-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="acc31-257">Klikněte na tlačítko **kontroly** uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="acc31-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="acc31-258">V prohlížeči nástroj Page Inspector klikněte na jedné z položek seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="acc31-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="acc31-259">Všimněte si, že místo se modře zvýrazněný, element je označený na oranžová s "JS" vedle názvu elementu.</span><span class="sxs-lookup"><span data-stu-id="acc31-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="acc31-260">To znamená, že element vytvořil dynamicky prostřednictvím skriptu.</span><span class="sxs-lookup"><span data-stu-id="acc31-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="acc31-261">Kromě toho se zobrazí oranžové podtržení na **zásobníkem volání** kartě. Znamená to, že **zásobníkem volání** podokno má další informace o elementu.</span><span class="sxs-lookup"><span data-stu-id="acc31-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="acc31-262">Klikněte na **zásobníkem volání** kartě. **Zásobníkem volání** podokně se zobrazují zásobníku volání pro volání jazyka JavaScript, který vytvořili elementu.</span><span class="sxs-lookup"><span data-stu-id="acc31-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="acc31-263">Volá, aby externí knihovny, jako jsou sbaleny jQuery, takže můžete snadno vidět, volání do vaší aplikace skriptu.</span><span class="sxs-lookup"><span data-stu-id="acc31-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="acc31-264">Chcete-li zobrazit úplnou zásobníku, včetně volání externí knihovny, můžete rozbalte uzly s označením "Externí knihovny":</span><span class="sxs-lookup"><span data-stu-id="acc31-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="acc31-265">Pokud kliknete na položku v zásobníku volání, Visual Studio otevře soubor kódu a označuje odpovídající skript.</span><span class="sxs-lookup"><span data-stu-id="acc31-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="acc31-266">Viz také</span><span class="sxs-lookup"><span data-stu-id="acc31-266">See Also</span></span>

<span data-ttu-id="acc31-267">[Úvod do architektury ASP.NET MVC 4 pomocí sady Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (webu ASP.net)</span><span class="sxs-lookup"><span data-stu-id="acc31-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="acc31-268">[Představení nástroje Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="acc31-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="acc31-269">[Nástroj Page Inspector chybové zprávy](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="acc31-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
