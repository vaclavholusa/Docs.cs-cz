---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Použití Page Inspectoru pro Visual Studio 2012 v rozhraní ASP.NET Web Forms | Dokumentace Microsoftu
author: rick-anderson
description: Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů pomocí integrovaného prohlížeče. Vyberte jakýkoli element ve integrovaného prohlížeče a nástroj Page Inspector...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e914b87305fa729659822ec1166e9d1947e59cb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806271"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="9dafa-104">Použití Page Inspectoru pro Visual Studio 2012 ve webových formulářích ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9dafa-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="9dafa-105">podle Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="9dafa-105">by Tim Ammann</span></span>

> <span data-ttu-id="9dafa-106">Nástroj Page Inspector pro sadu Visual Studio 2012 je nástroj pro vývoj webů pomocí integrovaného prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9dafa-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="9dafa-107">Vybrat jakýkoli element ve integrovaného prohlížeče a nástroj Page Inspector okamžitě zvýrazní zdroje a šablon stylů CSS prvku.</span><span class="sxs-lookup"><span data-stu-id="9dafa-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="9dafa-108">Můžete procházet všechny stránky v aplikaci, rychle najít zdroje vykreslované značky a použít nástroje prohlížeče přímo v prostředí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9dafa-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="9dafa-109">Tento kurz shwos jak povolit režim kontroly a rychle najít a úprava pravidel šablon stylů CSS a text v rámci webového projektu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="9dafa-110">V tomto kurzu použijete projekt webových formulářů aplikace, ale můžete použít také nástroje Page Inspector pro webové projekty a [MVC](https://go.microsoft.com/?linkid=9802002) aplikací.</span><span class="sxs-lookup"><span data-stu-id="9dafa-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="9dafa-111">Tento kurz obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="9dafa-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="9dafa-112">Požadované součásti</span><span class="sxs-lookup"><span data-stu-id="9dafa-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="9dafa-113">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9dafa-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="9dafa-114">Zobrazit aplikaci pomocí nástroje Page Inspector</span><span class="sxs-lookup"><span data-stu-id="9dafa-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="9dafa-115">Povolit režim kontroly</span><span class="sxs-lookup"><span data-stu-id="9dafa-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="9dafa-116">Použití Page Inspectoru provádět změny kódu</span><span class="sxs-lookup"><span data-stu-id="9dafa-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="9dafa-117">Režim kontroly a v okně HTML</span><span class="sxs-lookup"><span data-stu-id="9dafa-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="9dafa-118">Náhled změn šablon stylů CSS v okně Styly</span><span class="sxs-lookup"><span data-stu-id="9dafa-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="9dafa-119">Automatická synchronizace šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="9dafa-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="9dafa-120">Výběr barvy šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="9dafa-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="9dafa-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9dafa-121">Prerequisites</span></span>

- <span data-ttu-id="9dafa-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) nebo [sady Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="9dafa-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="9dafa-123">Chcete-li získat nejnovější verzi nástroje Page Inspector, použijte [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=255386) k instalaci sady Azure SDK pro .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="9dafa-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="9dafa-124">Nástroj Page Inspector je součástí nástroje Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="9dafa-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="9dafa-125">Nejnovější verze je verze 1.3.</span><span class="sxs-lookup"><span data-stu-id="9dafa-125">The latest version is 1.3.</span></span> <span data-ttu-id="9dafa-126">Zjištění verze máte, spusťte Visual Studio a vyberte **o Microsoft Visual Studio** z **pomáhají** nabídky.</span><span class="sxs-lookup"><span data-stu-id="9dafa-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="9dafa-127">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9dafa-127">Create a Web Application</span></span>

<span data-ttu-id="9dafa-128">Nejprve vytvoříte webovou aplikaci, kterou budete používat nástroj Page Inspector se.</span><span class="sxs-lookup"><span data-stu-id="9dafa-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="9dafa-129">V sadě Visual Studio, zvolte **souboru** &gt; **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="9dafa-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="9dafa-130">Na levé straně rozbalte **Visual C#** vyberte **webové**a pak vyberte **aplikace webových formulářů ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="9dafa-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nová aplikace webových formulářů](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="9dafa-132">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dafa-132">Click **OK**.</span></span>

<span data-ttu-id="9dafa-133">Aplikace se otevře v **zdroj** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9dafa-133">The application opens in **Source** view.</span></span>

![Nová aplikace webových formulářů v zobrazení zdroje](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="9dafa-135">Teď, když máte aplikaci pro práci s, můžete použít nástroj Page Inspector sloužící ke zkoumání a upravte ho.</span><span class="sxs-lookup"><span data-stu-id="9dafa-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="9dafa-136">Zobrazit aplikaci pomocí nástroje Page Inspector</span><span class="sxs-lookup"><span data-stu-id="9dafa-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="9dafa-137">V dalším kroku se zobrazit aplikace s nástrojem Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="9dafa-138">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **zobrazení v nástroje Page Inspector**.</span><span class="sxs-lookup"><span data-stu-id="9dafa-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Zobrazení v nástroje Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="9dafa-140">Ve výchozím nastavení když poprvé, spustí nástroj Page Inspector je ukotven jako úzké okno na levé straně prostředí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9dafa-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="9dafa-141">Ponechte ukotvena na levé straně a nastavte ho na šířku, který je pro vás naučí, nebo jej v jedné oblasti nástroj ukotvit na nahoru, dolů nebo vpravo:</span><span class="sxs-lookup"><span data-stu-id="9dafa-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Nástroj Page Inspector pozice ukotvení](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="9dafa-143">Pokud uvolníte okna nástroje Page Inspector, je možné je umístit mimo sadu Visual Studio, nebo dokonce i na druhý monitor pokud nějakou máte.</span><span class="sxs-lookup"><span data-stu-id="9dafa-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="9dafa-144">Ale v pořadí ALT + TAB mezi nástroj Page Inspector a sady Visual Studio při okno nástroje Page Inspector je uvolněna, přejděte na **nástroje** &gt; **možnosti** &gt;  **Prostředí** &gt; **karty a Windows**a v části **kartu dobře**zrušte výběr zaškrtávacího políčka volá **plovoucí panely nástrojů trojů zůstávají nad hlavní okno**:</span><span class="sxs-lookup"><span data-stu-id="9dafa-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Zrušte zaškrtnutí políčka s plovoucí desetinnou čárkou windows nástroj ALT + TAB mezi Visual Studio a okno neukotvené panely nástroje Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="9dafa-146">Horní podokno okna nástroje Page Inspector aktuální stránky zobrazí v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9dafa-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="9dafa-147">V dolním podokně zobrazí na stránce v kódu HTML na levé straně a některé karty na pravé straně, která umožňují kontrolovat různé aspekty stránky.</span><span class="sxs-lookup"><span data-stu-id="9dafa-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="9dafa-148">Dolní podokno je podobný [vývojářské nástroje F12 pomáhají](https://msdn.microsoft.com/ie/aa740478) v aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="9dafa-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="9dafa-149">(Ale na rozdíl od vývojářských nástrojů, můžete nástroj Page Inspector přímo v sadě Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="9dafa-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspektor stránek](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="9dafa-151">V tomto kurzu použijete prohlížeč podokna nástroje Page Inspector a **HTML** a **styly** karty, které vám pomohou rychle přejít a provést změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="9dafa-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="9dafa-152">Povolit režim kontroly</span><span class="sxs-lookup"><span data-stu-id="9dafa-152">Enable Inspection Mode</span></span>

<span data-ttu-id="9dafa-153">Dále uvidíte, jak funguje režim kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="9dafa-154">V okně nástroje Page Inspector klikněte **zkontrolujte, jestli se** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9dafa-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Zkontrolovat Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="9dafa-156">Pokud chcete zobrazit režim kontroly v akci, najeďte myší různé části stránky v rámci okna prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="9dafa-157">Stejně jako, ukazatel myši se změní na velké znaménko plus a zvýrazní prvek pod:</span><span class="sxs-lookup"><span data-stu-id="9dafa-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Ukazatel myši div.content obálky](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="9dafa-159">Při přesunu ukazatele myši, Všimněte si, že</span><span class="sxs-lookup"><span data-stu-id="9dafa-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="9dafa-160">Obsah v **zdroj** zobrazení se změní a objeví značka odpovídající vybraný element na stránce.</span><span class="sxs-lookup"><span data-stu-id="9dafa-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="9dafa-161">Je zvýrazněn odpovídající značky.</span><span class="sxs-lookup"><span data-stu-id="9dafa-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="9dafa-162">Pokud je zdroj do jiného souboru, tento soubor otevřít v zobrazení zdroje se zvýrazněnou relevantní značkami.</span><span class="sxs-lookup"><span data-stu-id="9dafa-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="9dafa-163">Značky zobrazeny v **HTML** karta v nástroje Page Inspector se také změní tak, aby odpovídaly vybraný element na stránce.</span><span class="sxs-lookup"><span data-stu-id="9dafa-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="9dafa-164">V **HTML** kartu, jsou uvedeny odpovídající značky.</span><span class="sxs-lookup"><span data-stu-id="9dafa-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="9dafa-165">**Styly** karta zobrazuje relevantní pro aktuální výběr pravidel šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="9dafa-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="9dafa-166">Použití Page Inspectoru provádět změny kódu</span><span class="sxs-lookup"><span data-stu-id="9dafa-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="9dafa-167">Nyní uvidíte, jak vám pomůže nástroj Page Inspector najít a provést změny kódu nebo text, jehož umístění nemusí být hned zjevné.</span><span class="sxs-lookup"><span data-stu-id="9dafa-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="9dafa-168">Vložit nástroj Page Inspector v režimu kontroly a potom přejděte do dolní části domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="9dafa-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="9dafa-169">Jakmile zadáte zápatí, otevře se nástroj Page Inspector *Site.Master* soubor rozložení v **zdroje** zobrazení dočasný kartě napravo od druhé karty a zvýrazní části hlavní stránky, které jste Vybrali jste.</span><span class="sxs-lookup"><span data-stu-id="9dafa-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="9dafa-170">To se dozvíte, jak najít a zobrazit obsah na stránce, která ve skutečnosti může pocházet z různých souborů než ten, který původně otevřel nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Zápatí osvětlení na režim kontroly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="9dafa-172">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad řádek s copyrightu <a id="a"> </a>Všimněte si, že.</span><span class="sxs-lookup"><span data-stu-id="9dafa-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="9dafa-173">V *Site.Master* stránce odpovídajícím řádku se zvýrazní.</span><span class="sxs-lookup"><span data-stu-id="9dafa-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Zápatí o autorských právech řádek zvýrazněný](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="9dafa-175">Přidejte nějaký text do konce řádku v *Site.Master* souboru.</span><span class="sxs-lookup"><span data-stu-id="9dafa-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="9dafa-176">&lt;p&gt;&amp;kopírovat; &lt;%: DateTime.Now.Year %&gt; – My ASP.NET Application Rocks!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="9dafa-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="9dafa-177">Nyní stiskněte kombinaci kláves Ctrl + Alt + Enter nebo klikněte na panel aktualizace pro zobrazení výsledků v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="9dafa-179">Jste možná jste si mysleli, že byla v zápatí je uvedené na *Default.aspx* stránky, ale ukázalo být na stránce předlohy rozložení a nástroj Page Inspector zjistila, že pro vás.</span><span class="sxs-lookup"><span data-stu-id="9dafa-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="9dafa-180">Režim kontroly a v okně HTML</span><span class="sxs-lookup"><span data-stu-id="9dafa-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="9dafa-181">V dalším kroku bude mít rychlý pohled na okno HTML a jak mapuje prvky za vás.</span><span class="sxs-lookup"><span data-stu-id="9dafa-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="9dafa-182">Nástroj Page Inspector převeďte do režimu kontroly.</span><span class="sxs-lookup"><span data-stu-id="9dafa-182">Put Page Inspector in Inspection Mode.</span></span>

![Zkontrolovat Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="9dafa-184">Klepněte na horní část stránky, která říká "zde bude vaše logo".</span><span class="sxs-lookup"><span data-stu-id="9dafa-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="9dafa-185">Prohlížené konkrétní element podrobněji, tak zobrazení v okně prohlížeče už mění při přesunutí ukazatele myši.</span><span class="sxs-lookup"><span data-stu-id="9dafa-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="9dafa-186">Nyní přesunutí ukazatele myši **HTML** okna.</span><span class="sxs-lookup"><span data-stu-id="9dafa-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="9dafa-187">Při přesunu ukazatele myši, nástroj Page Inspector popisuje element v rámci **HTML** okno a zvýrazní odpovídající element v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9dafa-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Okno HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="9dafa-189">Jako předtím, otevře se nástroj Page Inspector *Site.Master* souboru pro vás dočasné kartě. Klikněte na kartu Site.Master a zvýrazní se odpovídající značky ve &lt;záhlaví&gt; části:</span><span class="sxs-lookup"><span data-stu-id="9dafa-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Zvýrazněná značka](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="9dafa-191">Náhled změn šablon stylů CSS v okně Styly</span><span class="sxs-lookup"><span data-stu-id="9dafa-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="9dafa-192">V dalším kroku se zobrazí, jak můžete použít nástroj Page Inspector **styly** okna Náhled změn do šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="9dafa-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="9dafa-193">Klikněte na tlačítko **zkontrolujte, jestli se** tlačítko Vložit nástroj Page Inspector režim kontroly.</span><span class="sxs-lookup"><span data-stu-id="9dafa-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="9dafa-194">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad oddíl "Home Page" až do **div.content obálky** popisek se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="9dafa-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Ukazatel myši přesunout elementy](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="9dafa-196">Klikněte v části div.content obálky jednou a poté přesuňte ukazatel myši na **styly** okna.</span><span class="sxs-lookup"><span data-stu-id="9dafa-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="9dafa-197">V části volič .featured .content obálkové třídy zrušte a zaškrtněte políčko pro vlastnost barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="9dafa-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Barva pozadí vymazat](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="9dafa-199">Všimněte si, jak tato změna přináší náhled okamžitě v okně prohlížeče nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="9dafa-200">Zaškrtněte toto políčko znovu, klikněte dvakrát na hodnotu vlastnosti a změňte ji na `red`.</span><span class="sxs-lookup"><span data-stu-id="9dafa-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="9dafa-201">Změna ukazuje hned:</span><span class="sxs-lookup"><span data-stu-id="9dafa-201">The change shows immediately:</span></span>

![Barva pozadí Red](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="9dafa-203">**Styly** okno umožňuje snadno test a náhled šablon stylů CSS změní před potvrzením změn na styl listu samotný.</span><span class="sxs-lookup"><span data-stu-id="9dafa-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="9dafa-204">Automatická synchronizace šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="9dafa-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="9dafa-205">Tato funkce vyžaduje verzi 1.3 nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="9dafa-206">Funkce Automatická synchronizace šablon stylů CSS lze upravit přímo soubor šablony stylů CSS a podívejte se změny okamžitě v prohlížeči nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="9dafa-207">Klikněte na tlačítko **zkontrolujte, jestli se** uvést do režimu kontroly nástroje Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="9dafa-208">V prohlížeči nástroj Page Inspector, přesuňte ukazatel myši nad oddíl "Home Page" až do **div.content obálky** popisek se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="9dafa-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="9dafa-209">Kliknutím vyberte tento element.</span><span class="sxs-lookup"><span data-stu-id="9dafa-209">Click once to select this element.</span></span>

<span data-ttu-id="9dafa-210">**– Styly** okno zobrazuje všechna pravidla šablon stylů CSS u tohoto elementu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="9dafa-211">Posuňte se dolů najít .featured .content – obálky třídy selektor.</span><span class="sxs-lookup"><span data-stu-id="9dafa-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="9dafa-212">Klikněte na ".featured .content obálku".</span><span class="sxs-lookup"><span data-stu-id="9dafa-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="9dafa-213">Nástroj Page Inspector otevře soubor šablony stylů CSS, která definuje tento styl (Site.css) a zvýrazní odpovídající stylu CSS.</span><span class="sxs-lookup"><span data-stu-id="9dafa-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Soubor CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="9dafa-215">Teď změňte hodnotu `background-color` na hodnotu "red".</span><span class="sxs-lookup"><span data-stu-id="9dafa-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="9dafa-216">Změny se okamžitě zobrazí v prohlížeči nástroj Page Inspector.</span><span class="sxs-lookup"><span data-stu-id="9dafa-216">The change appears immediately in the Page Inspector browser.</span></span>

![Nástroj Page Inspector prohlížeče](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="9dafa-218">Výběr barvy šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="9dafa-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="9dafa-219">V dalším kroku se dozvíte, jak používat nástroj Page Inspector rychle najít a změnit šablon stylů CSS pro zvýrazněného textu ve výchozím nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9dafa-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="9dafa-220">V tomto příkladu jste se rozhodli, že nemáte, jako jsou modře zvýrazněný a chcete ho změnit na jinou barvu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="9dafa-221">Klikněte na tlačítko **zkontrolujte, jestli se** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9dafa-221">Click the **Inspect** button.</span></span>

![Zkontrolovat Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="9dafa-223">V okně prohlížeče nástroj Page Inspector, přesuňte ukazatel myši nad zvýrazněnou "videa, kurzy a ukázky" tak, aby CSS "mark" Popisek zobrazí se text.</span><span class="sxs-lookup"><span data-stu-id="9dafa-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Najede myší element značky](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="9dafa-225">Klepněte na text a vyberte ji.</span><span class="sxs-lookup"><span data-stu-id="9dafa-225">Click the text to select it.</span></span> <span data-ttu-id="9dafa-226">Odpovídající značky selektor šablon stylů CSS se zobrazí v dolní části **styly** okna.</span><span class="sxs-lookup"><span data-stu-id="9dafa-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Označit selektoru v okně Styly](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="9dafa-228">Klepněte na volič značky.</span><span class="sxs-lookup"><span data-stu-id="9dafa-228">Click the mark selector.</span></span> <span data-ttu-id="9dafa-229">Tím se otevře *Site.css* souboru pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9dafa-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="9dafa-230">Klikněte na kartu Site.css a zvýrazní se odpovídající šablony stylů CSS pro selektor:</span><span class="sxs-lookup"><span data-stu-id="9dafa-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Označit selektoru v šabloně stylů](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="9dafa-232">Vyberte a odeberte řádku s vlastností barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="9dafa-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="9dafa-233">Teď použijete nový výběr barvy šablon stylů CSS 2012 Visual Studio zvolte novou barvu pro **označit** vlastnost barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="9dafa-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="9dafa-234">Pomocí volby barev služby Visual Studio 2012 šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="9dafa-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="9dafa-235">Editor šablon stylů CSS v sadě Visual Studio 2012 obsahuje barvu ovládacího prvku pro výběr, která umožňuje jednoduše vybrat a vložit barvy.</span><span class="sxs-lookup"><span data-stu-id="9dafa-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="9dafa-236">Obsahuje jednoduché pruhu barev a "pop dolů" ovládacího prvku pro výběr, který nabízí lepší kontrolu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="9dafa-237">Výběr barvy obsahuje standardní paletu barev, podporuje názvy standardních barev, hash kódy, barvy RGB, RGBA, HSL a HSLA a udržuje seznam barvy, které jste naposledy použili v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="9dafa-238">Na řádku, kde byla vlastnost background-color zadejte "bc" a jednou stisknutím šipky dolů.</span><span class="sxs-lookup"><span data-stu-id="9dafa-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="9dafa-239">Když zadáte první znak každého slova oddělená spojovníkem vlastnost jako "background-color", filtry IntelliSense v seznamu zobrazit pouze vlastnosti, které odpovídají:</span><span class="sxs-lookup"><span data-stu-id="9dafa-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Technologie IntelliSense filtrované hodnoty](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="9dafa-241">Teď zadejte dvojtečku.</span><span class="sxs-lookup"><span data-stu-id="9dafa-241">Now type a colon.</span></span> <span data-ttu-id="9dafa-242">Pokud ano, vloží se název vlastnosti plnou barvu pozadí.</span><span class="sxs-lookup"><span data-stu-id="9dafa-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="9dafa-243">Typ **#** nebo **rgb (**, a zobrazí se panel pro výběr barev:</span><span class="sxs-lookup"><span data-stu-id="9dafa-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Výběr pruhu barev šablon stylů CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="9dafa-245">Pokud chcete zobrazit, jak funguje pruhu barev pro výběr, klikněte na jeho barvy s ukazatelem myši nebo stisknutím klávesy se šipkou dolů a pak pomocí kláves šipka doleva a doprava procházení barvy.</span><span class="sxs-lookup"><span data-stu-id="9dafa-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="9dafa-246">Při návštěvě barvu, je zobrazen odpovídající hodnota pro vlastnost background-color:</span><span class="sxs-lookup"><span data-stu-id="9dafa-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![Hodnota vlastnosti barvu pozadí náhledu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="9dafa-248">V tomto okamžiku může stisknutím klávesy Enter vyberte hodnotu a potom středníkem (;) k dokončení položky šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="9dafa-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="9dafa-249">Teď přejděte k další části tak, abyste viděli, jak funguje v pop – seznamu pro výběr barvy.</span><span class="sxs-lookup"><span data-stu-id="9dafa-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="9dafa-250">Použití barev výběr Pop dolů</span><span class="sxs-lookup"><span data-stu-id="9dafa-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="9dafa-251">Pokud u panelu barev není přesná barva, kterou hledáte, můžete použít výběr barvy pop dolů.</span><span class="sxs-lookup"><span data-stu-id="9dafa-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="9dafa-252">Pokud chcete soubor otevřít, klikněte na dvojitou šipku na pravém konci pruhu barev nebo jednou nebo dvakrát klávesu šipka dolů na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="9dafa-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Výběr barvy šablon stylů CSS Pop dolů](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="9dafa-254">Klikněte na barvu z svislá čára na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="9dafa-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="9dafa-255">V hlavním okně zobrazí přechod pro tuto barvu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="9dafa-256">Zvolte barvu přímo z svislá čára stisknutím klávesy Enter nebo klikněte na tlačítko kdekoli v hlavním okně Vybrat s větší přesností.</span><span class="sxs-lookup"><span data-stu-id="9dafa-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="9dafa-257">Pokud je na obrazovce počítače, který chcete použít barvu (nemusí být uvnitř uživatelské rozhraní Visual Studia), můžete zachytit její hodnotu s použitím nástroje kapátko vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="9dafa-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="9dafa-258">Neprůhlednost barvu také můžete změnit přesunutím posuvníku v dolní části nástroje pro výběr barvy.</span><span class="sxs-lookup"><span data-stu-id="9dafa-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="9dafa-259">Provádí se tak změní se barva hodnot na hodnoty RGBA, protože formát RGBA může představovat neprůhlednosti.</span><span class="sxs-lookup"><span data-stu-id="9dafa-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="9dafa-260">Po výběru barvy, stiskněte klávesu Enter a pak zadejte středníkem k dokončení barvu pozadí položky v *Site.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="9dafa-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="9dafa-261">Kontrola aktualizace posuvník</span><span class="sxs-lookup"><span data-stu-id="9dafa-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="9dafa-262">Nástroj Page Inspector okamžitě zjistí změnu *Site.css* souboru (nebo na libovolný soubor v aplikaci) a zobrazí upozornění na aktualizace panelu.</span><span class="sxs-lookup"><span data-stu-id="9dafa-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Panel aktualizace](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="9dafa-264">Uložte všechny soubory a aktualizujte prohlížeč, nástroj Page Inspector stisknutím klávesy Ctrl + Alt + Enter nebo klikněte na panel aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9dafa-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="9dafa-265">Změna barvy zvýraznění se zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="9dafa-265">The change in the highlight color appears in the browser:</span></span>

![Změnit barvu zvýraznění](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="9dafa-267">Všimněte si, že jednoduše aktualizovat prohlížeč nástroj Page Inspector přímo z prostředí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9dafa-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="9dafa-268">Použití Page Inspectoru místo externího prohlížeče umožňuje zůstat v editoru při vývoji webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9dafa-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="9dafa-269">Viz také</span><span class="sxs-lookup"><span data-stu-id="9dafa-269">See Also</span></span>

<span data-ttu-id="9dafa-270">[Představení nástroje Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video Channel 9)</span><span class="sxs-lookup"><span data-stu-id="9dafa-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="9dafa-271">[Chybové zprávy nástroje Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="9dafa-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
