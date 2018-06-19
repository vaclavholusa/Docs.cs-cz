---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Rukou na testovacím: 2013 webové nástroje sady Visual Studio | Microsoft Docs'
author: rick-anderson
description: Visual Studio je vynikající vývoj prostředí pro. Na základě NET Windows a webové projekty. Zahrnuje výkonné textový editor, který lze snadno použít k...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "26566443"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="ee2e4-104">Rukou na testovacího prostředí: Visual Studio 2013 nástroje pro Web</span><span class="sxs-lookup"><span data-stu-id="ee2e4-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="ee2e4-105">Podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ee2e4-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="ee2e4-106">Stažení webové táborech cvičení Kit</span><span class="sxs-lookup"><span data-stu-id="ee2e4-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="ee2e4-107">Visual Studio je vynikající vývoj prostředí pro. Na základě NET Windows a webové projekty.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="ee2e4-108">Zahrnuje výkonné textový editor, který lze snadno upravit samostatné soubory bez projektu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="ee2e4-109">Visual Studio udržuje strom analýzy plně vybavená jako upravit každý soubor.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="ee2e4-110">To umožňuje poskytovat bezkonkurenční automatické dokončování a na základě dokumentu akce při vytváření vývojového prostředí mnohem rychlejší i zpříjemnit Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="ee2e4-111">Tyto funkce jsou zvláště efektivní v dokumentech HTML a CSS.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="ee2e4-112">Všechna tato napájení je k dispozici pro rozšíření, což jednoduše rozšiřitelný editory nové funkce, aby vyhovovaly potřebám vaší.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="ee2e4-113">Web Essentials je kolekce (většinou) týkající se webu vylepšení pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="ee2e4-114">Obsahuje velké množství nové dokončování IntelliSense (hlavně u šablon stylů CSS), nové funkce Browser Link, automatické soubory JSHint pro jazyk JavaScript, nové upozornění pro HTML a CSS a řadu dalších funkcí, které jsou pro vývoj moderních webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="ee2e4-115">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="ee2e4-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="ee2e4-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="ee2e4-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="ee2e4-117">Cíle</span><span class="sxs-lookup"><span data-stu-id="ee2e4-117">Objectives</span></span>

<span data-ttu-id="ee2e4-118">V tomto testovacím prostředí praktických se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="ee2e4-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="ee2e4-119">Používat nové funkce editor HTML součástí Web Essentials, například bohaté fragmenty kódu jazyka HTML5 a Zen kódování</span><span class="sxs-lookup"><span data-stu-id="ee2e4-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="ee2e4-120">Používat nové funkce editor šablon stylů CSS součástí Web Essentials, například výběr barvy a prohlížeče matice popisku</span><span class="sxs-lookup"><span data-stu-id="ee2e4-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="ee2e4-121">Používat nové funkce JavaScript editor součástí Web Essentials, jako je extrakce souboru a IntelliSense pro všechny elementy HTML</span><span class="sxs-lookup"><span data-stu-id="ee2e4-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="ee2e4-122">Výměna dat mezi prohlížečem a sadě Visual Studio pomocí Browser Link</span><span class="sxs-lookup"><span data-stu-id="ee2e4-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ee2e4-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ee2e4-123">Prerequisites</span></span>

<span data-ttu-id="ee2e4-124">Pro dokončení této praktické cvičení je vyžadován následující text:</span><span class="sxs-lookup"><span data-stu-id="ee2e4-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="ee2e4-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span><span class="sxs-lookup"><span data-stu-id="ee2e4-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="ee2e4-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="ee2e4-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="ee2e4-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="ee2e4-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ee2e4-128">Instalace</span><span class="sxs-lookup"><span data-stu-id="ee2e4-128">Setup</span></span>

<span data-ttu-id="ee2e4-129">Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="ee2e4-130">Otevřete okno Průzkumníka Windows a přejděte do tohoto prostředí **zdroj** složky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="ee2e4-131">Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="ee2e4-132">Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="ee2e4-133">Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="ee2e4-134">Používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="ee2e4-134">Using the Code Snippets</span></span>

<span data-ttu-id="ee2e4-135">V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="ee2e4-136">Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="ee2e4-137">Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="ee2e4-138">Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="ee2e4-139">Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="ee2e4-140">Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ee2e4-141">Cvičení</span><span class="sxs-lookup"><span data-stu-id="ee2e4-141">Exercises</span></span>

<span data-ttu-id="ee2e4-142">Toto praktické cvičení zahrnuje následující cvičení:</span><span class="sxs-lookup"><span data-stu-id="ee2e4-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="ee2e4-143">Práce s Browser Link a webové Essentials</span><span class="sxs-lookup"><span data-stu-id="ee2e4-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="ee2e4-144">Využití výhod fragmenty kódu a IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ee2e4-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="ee2e4-145">Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="ee2e4-146">Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="ee2e4-147">Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="ee2e4-148">Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="ee2e4-149">Cvičení 1: Práce s Browser Link a webové Essentials</span><span class="sxs-lookup"><span data-stu-id="ee2e4-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="ee2e4-150">**Web Essentials** je rozšíření sady Visual Studio, který přidává celou řadu užitečných funkcí pro vývoj moderních webových, většinou zaměřuje na provedení vývojového prostředí webové mnohem rychlejší i zpříjemnit.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="ee2e4-151">Web Essentials můžete nainstalovat z Galerie rozšíření v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="ee2e4-152">**Browser Link** je nová funkce zahrnuté ve Visual Studiu 2013, který poskytuje kanál mezi Visual Studio IDE a libovolného prohlížeče otevřete pro výměnu dat mezi vaší webové aplikace a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="ee2e4-153">Web Essentials rozšiřuje Browser Link pomocí nástrojů k manipulaci s objektový model DOM a stylů CSS webových stránek přímo z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="ee2e4-154">V tomto cvičení bude prozkoumávat některé z funkcí podporovaných **Web Essentials** a **Browser Link** k vylepšení jednoduché kvízu stránky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="ee2e4-155">Úloha 1 – spuštění projektu v více prohlížečů</span><span class="sxs-lookup"><span data-stu-id="ee2e4-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="ee2e4-156">V této úloze nakonfigurujete webové aplikace na spouštění v několika prohlížeče najednou, což je užitečné pro testování různých prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="ee2e4-157">Open **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="ee2e4-158">V **soubor** nabídce vyberte možnost **otevřete | Projekt nebo řešení...**  a přejděte do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** v **zdroj** složky laboratoře (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="ee2e4-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="ee2e4-159">Vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="ee2e4-160">Na panelu nástrojů Visual Studio rozbalte nabídku prohlížeče a vyberte **procházet s...** .</span><span class="sxs-lookup"><span data-stu-id="ee2e4-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="ee2e4-161">![Procházet s možností nabídky](visual-studio-2013-web-tools/_static/image1.png "procházet s... v nabídce prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="ee2e4-162">*Procházet s možností nabídky*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="ee2e4-163">V **procházet s** dialogové okno, vyberte **Google Chrome** a **Internet Explorer** podržením **CTRL** klíče a klikněte na tlačítko  **Nastavit jako výchozí**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="ee2e4-164">![Procházet s dialogové okno](visual-studio-2013-web-tools/_static/image2.png "procházet s dialogové okno")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="ee2e4-165">*Výběr více nastavení výchozích prohlížečů*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="ee2e4-166">Google Chrome a prohlížeče Internet Explorer by se měla zobrazit jako nastavení výchozích prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="ee2e4-167">Klikněte na tlačítko **zrušit** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="ee2e4-168">![Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče](visual-studio-2013-web-tools/_static/image3.png "Google Chrome a prohlížeče Internet Explorer nastavení výchozích prohlížečů")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="ee2e4-169">*Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-170">Po konfiguraci nastavení výchozích prohlížečů **více prohlížečů** je vybraná možnost v nabídce prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="ee2e4-171">![Více prohlížečů](visual-studio-2013-web-tools/_static/image4.png "více prohlížečů")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="ee2e4-172">Stiskněte klávesu **CTRL** + **F5** ke spuštění aplikace bez ladění.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="ee2e4-173">Při otevření obě okna prohlížeče, umístěte jeden z nich nad sebou chcete-li zobrazit aktualizace v obou prohlížečích současně.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="ee2e4-174">Prohlížečů měli zobrazení webové stránky s obdélníku světle modrou.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="ee2e4-175">![Umístění jeden prohlížeč nad sebou](visual-studio-2013-web-tools/_static/image5.png "umístění jeden prohlížeč nad sebou")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="ee2e4-176">*Umístění jeden prohlížeč nad sebou*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="ee2e4-177">Nezavírejte okno prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-177">Do not close the browsers.</span></span> <span data-ttu-id="ee2e4-178">Budete je používat v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="ee2e4-179">Úloha 2 – Zen pomocí kódování k vytváření prvků HTML</span><span class="sxs-lookup"><span data-stu-id="ee2e4-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="ee2e4-180">**Kódování Zen** je editor kódování modul plug-in pro vysokorychlostní HTML, XML, XSL (nebo jiného formátu strukturovaný kód) a úpravy.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="ee2e4-181">Základní tento modul plug-in je výkonný zkratka modul, který umožňuje rozšířit výrazy - podobná Selektory šablon stylů CSS – do kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="ee2e4-182">Kódování Zen je rychlý způsob, jak zapsat syntaxe selektor formátování HTML pomocí šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="ee2e4-183">V tomto cvičení použije k vygenerování tlačítka HTML, která představují možnosti otázka Zen kódování funkce poskytované Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="ee2e4-184">Přepněte zpátky na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="ee2e4-185">Otevřete **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="ee2e4-186">Nahraďte **&lt;!--TODO: Přidat zde – možnosti&gt;** komentář s následující kód a stiskněte klávesu **KARTĚ**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="ee2e4-187">Kód by měl rozšířit, aby HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="ee2e4-188">![Rozšířit HTML](visual-studio-2013-web-tools/_static/image6.png "rozšířit HTML")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="ee2e4-189">*Rozšířené HTML*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-190">Další informace o kódování Zen syntaxi najdete v tématu následující [článku](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="ee2e4-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="ee2e4-191">Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="ee2e4-192">![Aktualizujte propojené prohlížeče](visual-studio-2013-web-tools/_static/image7.png "aktualizovat propojené prohlížečů")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="ee2e4-193">*Aktualizujte propojené prohlížečů*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="ee2e4-194">![Internet Explorer - stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - stránka aktualizována čtyři tlačítka")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="ee2e4-195">*Internet Explorer - stránka aktualizována čtyři tlačítka*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="ee2e4-196">![Google Chrome – stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – stránka aktualizována čtyři tlačítka")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="ee2e4-197">*Google Chrome – stránka aktualizována čtyři tlačítka*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="ee2e4-198">Přepněte zpátky na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="ee2e4-199">Jste přidali tlačítka na stránku, ale stále budete muset přidat šablonu otázku.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="ee2e4-200">Uděláte to tak, budete používat nové funkce ve webové Essentials názvem **Lorem Ipsum generátor**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="ee2e4-201">Vyhledejte **div** element s **třída** atribut **přední**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="ee2e4-202">Přidejte následující kód jako první podřízený prvek **div**a stiskněte klávesu **KARTĚ**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="ee2e4-203">Kód by měl rozšířit, aby HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="ee2e4-204">![Automaticky generovaný Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "automaticky generovaný Lorem Ipsum")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="ee2e4-205">*Automaticky generovaný Lorem Ipsum*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-206">Jako součást Zen kódování můžete nyní vygenerovat kód Lorem Ipsum přímo v editoru HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="ee2e4-207">Jednoduše zadejte **lorem** a počtu **karta** a wordový Lorem Ipsum text se vloží 30.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="ee2e4-208">Například</span><span class="sxs-lookup"><span data-stu-id="ee2e4-208">E.g.</span></span> <span data-ttu-id="ee2e4-209">*lorem10* vloží 10 Lorem Ipsum slova.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="ee2e4-210">Přidáte logo, které se v horní části na otázku pomocí další novou funkcí v Web Essentials názvem **Lorem pixelů generátor**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="ee2e4-211">Přidejte následující kód jako první podřízený prvek **div** element s **kontejneru** jako **třída** hodnotu a stiskněte klávesu **KARTĚ**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="ee2e4-212">Kód by měl rozšířit do HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="ee2e4-213">![Automaticky generovaný lorem pixelů](visual-studio-2013-web-tools/_static/image11.png "automaticky generovaný Lorem pixelů")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="ee2e4-214">*Automaticky generovaný lorem pixelů*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-215">Jako součást Zen kódování můžete také vygenerovat kód Lorem pixelů přímo v editoru HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="ee2e4-216">Jednoduše zadejte **obrázky. 200 x 200 zvířat** a počtu **KARTĚ** a **img** značky se 200 x 200 bitovou kopií zvíře bude vložen.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="ee2e4-217">Další informace najdete v části [Lorem pixelů](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="ee2e4-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="ee2e4-218">Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="ee2e4-219">![Internet Explorer - automaticky vygenerovanou image a text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - automaticky vygenerovanou image a text.")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="ee2e4-220">*Internet Explorer - automaticky vygenerovanou image a text.*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="ee2e4-221">![Google Chrome – generován automaticky image a text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome – generován automaticky image a text.")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="ee2e4-222">*Google Chrome – generován automaticky image a text.*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-223">Vzhledem k tomu, že při přidání fragmentu kódu, vybraná náhodně bitovou kopii, bitovou kopii vidět v prohlížečích se můžou lišit.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="ee2e4-224">Nezavírejte okno prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-224">Do not close the browsers.</span></span> <span data-ttu-id="ee2e4-225">Budete je používat v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="ee2e4-226">Úloha 3 - aktualizace vlastnost stylu</span><span class="sxs-lookup"><span data-stu-id="ee2e4-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="ee2e4-227">V této úloze budete používat Browser Link **zkontrolovat režimu** funkci tak, aby zjistit přesné umístění, kde se vygeneruje konkrétní elementu DOM a aktualizujte vlastnost barvu tohoto elementu s použitím výběr barvy, a poskytuje webové Essentials.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="ee2e4-228">V prohlížeči Internet Explorer, stiskněte klávesu **CTRL** + **ALT** + **I** aby kontrolovat režimu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="ee2e4-229">Přesuňte ukazatel myši světla modré ohraničení a klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="ee2e4-230">![Ukazatele přes světla modré ohraničení](visual-studio-2013-web-tools/_static/image14.png "ukazatele přes světla modré ohraničení")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="ee2e4-231">*Ukazatele přes světla modré ohraničení*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="ee2e4-232">Přepněte zpátky na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="ee2e4-233">Všimněte si, jak prvek HTML, který jste vybrali v prohlížeči, vybere se také v editoru Visual Studio HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="ee2e4-234">![Element HTML v editoru Visual Studio HTML vybrána](visual-studio-2013-web-tools/_static/image15.png "prvek HTML vybraného v editoru Visual Studio HTML")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="ee2e4-235">*Element HTML vybraného v editoru Visual Studio HTML*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="ee2e4-236">Bude nyní aktualizovat **přední** třídy CSS, aby bylo možné změnit stylů vybraného prvku.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="ee2e4-237">Chcete-li to provést, stiskněte **CTRL** + **,** otevřete **přejít na** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="ee2e4-238">Typ **site.css** a stiskněte klávesu **ENTER** k otevření souboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="ee2e4-239">![Otevření souboru Site.css](visual-studio-2013-web-tools/_static/image16.png "při otevírání souboru Site.css")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="ee2e4-240">*Při otevírání souboru Site.css*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="ee2e4-241">Stiskněte klávesu **CTRL** + **F** a typ **.flip kontejneru .front** k vyhledání modulu pro výběr šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="ee2e4-242">Klikněte na tlačítko světla blue hranaté ve vlastnosti ohraničení třídy otevřete volby barev.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="ee2e4-243">![Otevření dialogového okna Výběr barvy](visual-studio-2013-web-tools/_static/image17.png "otevírání volby barev")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="ee2e4-244">*Otevření dialogového okna Výběr barvy*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="ee2e4-245">Kliknutím tlačítko s dvojitou šipkou rozbalte výběr barvy a vyberte novou barvu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="ee2e4-246">![Rozšiřování volby barev](visual-studio-2013-web-tools/_static/image18.png "rozšiřování volby barev")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="ee2e4-247">*Rozšiřování volby barev*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="ee2e4-248">Stiskněte klávesu **CTRL** + **ALT** + **ENTER** aktualizovat propojené prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="ee2e4-249">Přepněte do Internet Exploreru a Všimněte si, jak má změnit barvu ohraničení.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="ee2e4-250">![Internet Explorer - aktualizovat barvu ohraničení](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - aktualizovat barvu ohraničení")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="ee2e4-251">*Internet Explorer - aktualizovat barvu ohraničení*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="ee2e4-252">Přepněte do Google Chrome a Všimněte si, jak má změnit barvu ohraničení.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="ee2e4-253">![Google Chrome – aktualizovat barvu ohraničení](visual-studio-2013-web-tools/_static/image20.png "Google Chrome – aktualizovat barvu ohraničení")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="ee2e4-254">*Google Chrome – aktualizovat barvu ohraničení*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="ee2e4-255">Přepněte zpátky na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="ee2e4-256">Přejděte na konec **Site.css** souboru a stiskněte klávesu **CTRL** + **F** najít **.btn** selektor.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="ee2e4-257">Všimněte si, že **- webkit-border-radius** vlastnost je podtržený zeleně.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="ee2e4-258">![Vlastnost - webkit-border-radius selektoru btn](visual-studio-2013-web-tools/_static/image21.png "vlastnost - webkit-border-radius selektoru btn")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="ee2e4-259">*Vlastnost - webkit-border-radius selektoru btn*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="ee2e4-260">Umístěte pomocí kurzoru v **- webkit-border-radius** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="ee2e4-261">Modré řádku by se zobrazit v části první písmeno prvního slova vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="ee2e4-262">Toto je **inteligentní značky**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="ee2e4-263">Stiskněte klávesu **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="ee2e4-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="ee2e4-264">Otevřete nabídku návrhy a klikněte na tlačítko **přidat chybí vlastnost standardní (border-radius)**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="ee2e4-265">![Přidat chybí vlastnost standardní návrhu](visual-studio-2013-web-tools/_static/image22.png "přidat chybí vlastnost standardní návrhu")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="ee2e4-266">*Přidejte chybějící standardní vlastnost návrhu*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="ee2e4-267">**Border-radius** vlastnost je automaticky přidán do pravidla šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="ee2e4-268">![Chybí standardní vlastnost přidat](visual-studio-2013-web-tools/_static/image23.png "chybí standardní vlastnost přidána")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="ee2e4-269">*Chybí standardní vlastnost přidána*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="ee2e4-270">Přesuňte ukazatel myši **border-radius** zobrazovanou vlastnost **prohlížeče matice popisek**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="ee2e4-271">**Prohlížeče matice popisek** uvádí dostupnost vlastnosti v každé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="ee2e4-272">![Popisek matice prohlížeče](visual-studio-2013-web-tools/_static/image24.png "popisek matice prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="ee2e4-273">*Popisek matice prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="ee2e4-274">Všimněte si, že hodnota **border-radius** vlastnost je stále podtržené.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="ee2e4-275">Přesuňte ukazatel myši hodnota zobrazíte upozornění.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="ee2e4-276">![Upozornění hodnota border-radius vlastnost](visual-studio-2013-web-tools/_static/image25.png "Border-radius vlastnost hodnota upozornění")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="ee2e4-277">*Upozornění hodnotu vlastnosti border-radius*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="ee2e4-278">Odeberte na jednotku **border-radius** hodnotu vlastnosti na návrh popisek.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="ee2e4-279">Jako **border-radius** je standardní vlastnost pro definování způsobu zaokrouhlené ohraničení rozích jsou, můžete odebrat **- webkit-border-radius** vlastností a hodnotou z pravidla šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="ee2e4-280">Umístěte pomocí kurzoru v **word-wrap** vlastnost a Všimněte si, že inteligentní značky se zobrazí také níže.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="ee2e4-281">Otevřete nabídku a klikněte na tlačítko **přidejte chybějící specifika dodavatele**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="ee2e4-282">![Přidejte chybějící dodavatele specifika návrhu](visual-studio-2013-web-tools/_static/image26.png "přidejte chybějící dodavatele specifika návrhu")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="ee2e4-283">*Přidejte chybějící dodavatele specifika návrhu*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="ee2e4-284">**-Ms-zalamování** vlastnost je automaticky přidán do pravidla šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="ee2e4-285">![Určité vlastnosti dodavatele přidat](visual-studio-2013-web-tools/_static/image27.png "dodavatele určitou vlastnost přidána")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="ee2e4-286">*Přidat určitou vlastnost dodavatele*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="ee2e4-287">Úloha 4 – aktualizace kód HTML z prohlížeče</span><span class="sxs-lookup"><span data-stu-id="ee2e4-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="ee2e4-288">V této úloze budete používat Browser Link **režimu návrhu** funkce Upravit objekt DOM z prohlížeče a přenést změny ke zdrojovému souboru HTML v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="ee2e4-289">V Google Chrome, stiskněte klávesu **CTRL** + **ALT** + **D** povolení režimu návrhu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="ee2e4-290">Přesuňte ukazatel myši **Lorem Ipsum dolor lokality amet** popisku a klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="ee2e4-291">![Úpravy na otázku](visual-studio-2013-web-tools/_static/image28.png "úpravy na otázku")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="ee2e4-292">*Úpravy na otázku*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-292">*Editing the question*</span></span>
3. <span data-ttu-id="ee2e4-293">Kurzor by se zobrazit.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-293">A cursor should appear.</span></span> <span data-ttu-id="ee2e4-294">Nahradí původní text s *jak ho vypadá při psaní delší otázku?* a potom stiskněte klávesu **ESC** ukončíte režimu návrhu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="ee2e4-295">![Otázka upravit](visual-studio-2013-web-tools/_static/image29.png "otázku upravit")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="ee2e4-296">*Upravit dotaz*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-296">*Question edited*</span></span>
4. <span data-ttu-id="ee2e4-297">Přepněte zpět do Visual Studio a otevřete **Index.cshtml**, pokud ještě není otevřené.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="ee2e4-298">Všimněte si, že vnitřní text **&lt;p&gt;** element se aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="ee2e4-299">![Aktualizované otázku na stránce HTML](visual-studio-2013-web-tools/_static/image30.png "aktualizované otázku na stránce HTML")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="ee2e4-300">*Aktualizované otázku na stránce HTML*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="ee2e4-301">Úloha 5: prostudovali SEO související upozornění</span><span class="sxs-lookup"><span data-stu-id="ee2e4-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="ee2e4-302">**Hledání modul optimalizace** (vyhledávací weby SEO) je proces převedení vyšší pořadí webu na vyhledávací web seznam výsledků.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="ee2e4-303">Tím vyšší určuje pořadí webu a více konzistentně je uvedena, další návštěvníkům webu získají z této vyhledávací web.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="ee2e4-304">Web Essentials zahrnuje analytické nástroje, která prověřuje HTML, sestavy problémy najít a nabízí pomoc při jejich odstranění.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="ee2e4-305">Přejděte na **zobrazení** nabídky a klikněte na tlačítko **seznam chyb** otevřete **seznam chyb** okno.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="ee2e4-306">![Chyba v zobrazení seznamu nabídky](visual-studio-2013-web-tools/_static/image31.png "seznam chyb v nabídce zobrazení")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="ee2e4-307">*Chyba v zobrazení seznamu nabídky*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="ee2e4-308">Všimněte si, že je SEO upozornění s informací, které **&lt;meta&gt;** značky pro popis stránky chybí.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="ee2e4-309">Dvakrát klikněte na položku upozornění optimalizace pro vyhledávací weby a opravte ji.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="ee2e4-310">![Okno Seznam chyb](visual-studio-2013-web-tools/_static/image32.png "v okně Seznam chyb")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="ee2e4-311">*Okno Seznam chyb*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-311">*Error List window*</span></span>
3. <span data-ttu-id="ee2e4-312">V **Web Essentials** dialogové okno, klikněte na tlačítko **Ano** Vložit popis &lt;meta&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="ee2e4-313">![Dialogové okno Essentials web](visual-studio-2013-web-tools/_static/image33.png "dialogové okno Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="ee2e4-314">*Dialogové okno Essentials Web*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="ee2e4-315">Editor pro  **\_Layout.cshtml** otevře a **&lt;meta&gt;** značky je automaticky přidán do **head** části Soubor HTML.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="ee2e4-316">![Značka META automaticky přidá stránce _Layout](visual-studio-2013-web-tools/_static/image34.png "metaznačku automaticky přidány _Layout stránku")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="ee2e4-317">*Značka META automaticky přidá do \_rozložení stránky*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="ee2e4-318">Změňte hodnotu **obsah** atribut *GeekQuiz* a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="ee2e4-319">Cvičení 2: Využívat výhod fragmenty kódu a IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ee2e4-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="ee2e4-320">S Web Essentials bylo rozšířeno HTML editor s další funkce.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="ee2e4-321">V tomto cvičení zobrazí se některé nové funkce, které jsou užitečné při vývoji webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="ee2e4-322">Úloha 1 – pomocí IntelliSense v dokumentech HTML</span><span class="sxs-lookup"><span data-stu-id="ee2e4-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="ee2e4-323">První nové funkce, zobrazí se v této úloze se nazývá **dynamické IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="ee2e4-324">Dynamické IntelliSense přečte další značky a atributy, které mají odvození možné ID, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="ee2e4-325">V této úloze vytvoříte nový element formuláře HTML obsahující štítky a vstupního pole.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="ee2e4-326">Pak přidáte **pro** atribut štítek, který chcete navázat jej na vstup a zobrazí se vám IntelliSense návrhy podle ID vstupních hodnot v oboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="ee2e4-327">Otevřete **Visual Studio Express 2013 pro Web** a **Begin.sln** řešení umístěný v **zdroj/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="ee2e4-328">Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="ee2e4-329">V **Průzkumníku řešení**, otevřete **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="ee2e4-330">Přidejte následující formulář uvnitř **&lt;části&gt;** element.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="ee2e4-331">(Code fragment kódu - *VisualStudio2013WebTooling* - *Ex2* - *formuláře*)</span><span class="sxs-lookup"><span data-stu-id="ee2e4-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="ee2e4-332">Vstupní značky musí předcházet popisek nějaké popis pole.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="ee2e4-333">Přidejte následující popisku před vstupní značka.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="ee2e4-334">**Pro** atribut **&lt;popisek&gt;** Určuje, který element formuláře a popisku je vázán k.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="ee2e4-335">Hodnota atributu musí být roven id související elementu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="ee2e4-336">Přidat **pro** atribut **&lt;popisek&gt;** element.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="ee2e4-337">Jak je znázorněno na následujícím obrázku &quot;název&quot; hodnotu objeví v dialogovém okně IntelliSense na základě id elementů v rámci stejného oboru (uzavření  **&lt;formuláře&gt;**).</span><span class="sxs-lookup"><span data-stu-id="ee2e4-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="ee2e4-338">![Zobrazení v IntelliSense id](visual-studio-2013-web-tools/_static/image35.png "id zobrazení v IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="ee2e4-339">*Id zobrazení v IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="ee2e4-340">Odstranit nedávno přidané **&lt;formuláře&gt;** elementu a jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="ee2e4-341">Úloha 2 – pomocí fragmenty kódu HTML</span><span class="sxs-lookup"><span data-stu-id="ee2e4-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="ee2e4-342">HTML5 zavedl víc než 25 nové sémantického značky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="ee2e4-343">Visual Studio již obsahuje podporu technologie IntelliSense pro tyto značky, ale Visual Studio 2013 je rychlejší a snazší psát kód přidáním nové fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="ee2e4-344">I když tyto značky nejsou složitá, jsou součástí pár malých odlišnosti, jako je například přidávání případech přejít správný kodek pro *zvuk* značky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="ee2e4-345">V této úloze uvidíte fragmenty kódu HTML pro zvuk značku.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="ee2e4-346">V **Index.cshtml** souboru, zadejte  **&lt;oblast** uvnitř **&lt;části&gt;** element, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="ee2e4-347">![Vkládání audio element](visual-studio-2013-web-tools/_static/image36.png "vkládání audio element")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="ee2e4-348">*Vkládání audio element*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="ee2e4-349">Stiskněte klávesu **KARTĚ** dvakrát a Všimněte si, jak se přidá následující kód na stránce a je umístěn kurzor na **src** atribut první zdroje.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="ee2e4-350">Stisknutím kombinace kláves **KARTĚ** klíče dvakrát fragmentu kódu je vložen.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="ee2e4-351">Zvuk fragment kódu ukazuje standardní využití *zvuk* značky s dvěma zdrojové soubory pro lepší podporu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="ee2e4-352">Odstraňte na druhém řádku a aktualizace zdroje prvního řádku s použitím na následující odkaz k zobrazení WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="ee2e4-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="ee2e4-353">Výsledný kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="ee2e4-354">Zdrojový soubor *KatanaProject.mp3* slouží jako příklad.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="ee2e4-355">Pokud dáváte přednost, můžete použít jiný zdroj.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="ee2e4-356">Stiskněte klávesu **CTRL** + **S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="ee2e4-357">Stiskněte klávesu **CTRL** + **F5** a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="ee2e4-358">Všimněte si, že audio player byl přidán do aplikace.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="ee2e4-359">![Přehrávač v aplikaci Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "zvuk player v aplikaci Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="ee2e4-360">*Přehrávač v Internet Exploreru*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="ee2e4-361">![Přehrávač v Google Chrome](visual-studio-2013-web-tools/_static/image38.png "player zvukovém souboru v Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="ee2e4-362">*Přehrávač v Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="ee2e4-363">Nezavírejte okno prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-363">Do not close the browsers.</span></span> <span data-ttu-id="ee2e4-364">Budete je používat v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="ee2e4-365">Úloha 3 – pomocí IntelliSense v dokumentech jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ee2e4-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="ee2e4-366">S Web Essentials 2013 šablony stylů a stránky HTML vytvořit seznam ID a třídy názvů.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="ee2e4-367">V této úloze se dozvíte, jak tyto seznamy vylepšit podporu JavaScript IntelliSense v webové Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="ee2e4-368">V **Index.cshtml** soubor, přidejte následující kód k definování **skriptu** značky pro kód jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="ee2e4-369">Přidejte následující kód do **skriptu** značky definice funkce připravené zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="ee2e4-370">(Code fragment kódu - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="ee2e4-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="ee2e4-371">Umístěte pomocí kurzoru v **skriptu** značky a stiskněte klávesu **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="ee2e4-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="ee2e4-372">Otevřete nabídku návrhu.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="ee2e4-373">Klikněte na tlačítko **extrahovat do souboru**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="ee2e4-374">![JavaScript extrakci souborů návrhu](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrakci souborů návrhu")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="ee2e4-375">*JavaScript extrakci souborů návrhu*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="ee2e4-376">V **uložit jako** vyberte **skripty** složku, název souboru **init.js** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="ee2e4-377">![Okno Uložit jako](visual-studio-2013-web-tools/_static/image40.png "okno Uložit jako")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="ee2e4-378">*Okno Uložit jako*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-379">**Init.js** soubor je vytvořen a obsah skriptu je přesunut do souboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="ee2e4-380">![Init.js souboru vytvořeného v obsah zahrnutý](visual-studio-2013-web-tools/_static/image41.png "Init.js souboru vytvořeného v obsah zahrnutý")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="ee2e4-381">*Init.js souboru vytvořeného v obsah zahrnutý*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="ee2e4-382">Otevřete **Index.cshtml** soubor a zkontrolujte, zda byl nahrazen značky script s odkazem na **init.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="ee2e4-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="ee2e4-384">*Init.js html reference*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="ee2e4-385">Přejděte na **Průzkumníku řešení** a Všimněte si, že **init.js** souboru je automaticky zahrnutý v řešení.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="ee2e4-386">![Soubor Init.js součástí řešení](visual-studio-2013-web-tools/_static/image43.png "Init.js soubor zahrnutý v řešení")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="ee2e4-387">*Soubor Init.js součástí řešení*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="ee2e4-388">Přepněte zpět na **init.js** soubor aktualizovat **připraven** funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="ee2e4-389">V definici funkce zpětného volání, který je předán do *připraven*, přidejte následující kód slouží k získání všechny elementy atributem určité třídy.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="ee2e4-390">Stiskněte klávesu **CTRL** + **místo** mezi uvozovky uvnitř **getElementsByClassName** volání funkce.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="ee2e4-391">![Zobrazení technologie IntelliSense pro funkci getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zobrazující IntelliSense pro funkci getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="ee2e4-392">*Zobrazení technologie IntelliSense pro funkci getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-393">Všimněte si, že IntelliSense ukazuje třídy definované v projektu šablony stylů.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="ee2e4-394">Nahraďte řádek, který jste vytvořili následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="ee2e4-395">Umístěte kurzor po **au** uvnitř uvozovek v **getElementsByTagName** funkce a stiskněte klávesu **CTRL** + **místo**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="ee2e4-396">![Zobrazení technologie IntelliSense pro metodu getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zobrazující IntelliSense pro metodu getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="ee2e4-397">*Zobrazení technologie IntelliSense pro metodu getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="ee2e4-398">Vyberte **&quot;zvuk&quot;** ze seznamu a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="ee2e4-399">Výsledkem je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="ee2e4-400">![Načítání zvuku elementy](visual-studio-2013-web-tools/_static/image46.png "načítání zvuku elementy")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="ee2e4-401">*Načítání zvuku elementy*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="ee2e4-402">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **init.js** souboru v **skripty** složky a vyberte **Minifikaci JavaScript soubory** z **Web Essentials** nabídky.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="ee2e4-403">![Minifikaci soubory JavaScript](visual-studio-2013-web-tools/_static/image47.png "soubory Minifikaci JavaScript")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="ee2e4-404">*Minifikaci soubory jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="ee2e4-405">Po zobrazení výzvy k povolení automatického minimalizace při změní zdrojový soubor, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="ee2e4-406">![Povolení automatického minimalizace upozornění](visual-studio-2013-web-tools/_static/image48.png "povolení automatického minimalizace upozornění")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="ee2e4-407">*Povolení automatického minimalizace upozornění*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee2e4-408">**Init.min.js** je vytvořen a se přidal jako závislost **init.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="ee2e4-409">![Soubor Init.min.js vytvořený](visual-studio-2013-web-tools/_static/image49.png "Init.min.js soubor vytvořený")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="ee2e4-410">*Soubor Init.min.js vytvořený*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="ee2e4-411">Otevřete **init.min.js** souboru a Všimněte si, že soubor je minifikovaný.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="ee2e4-412">![Obsah souboru Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js obsah souboru")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="ee2e4-413">*Obsah souboru Init.min.js*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="ee2e4-414">V **init.js** soubor, přidejte následující kód níže **getElementsByTagName** volání přehrávání zvuku elementy funkce.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="ee2e4-415">(Code fragment kódu - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="ee2e4-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="ee2e4-416">Klikněte na tlačítko **CTRL** + **S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="ee2e4-417">Vzhledem k tomu, že minifikovaný soubor je již otevřen, zobrazí se dialogové okno s informacemi o tom, že soubor byl změněn mimo editoru zdroje.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="ee2e4-418">Klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-418">Click **Yes**.</span></span>

    <span data-ttu-id="ee2e4-419">![Microsoft Visual Studio upozornění](visual-studio-2013-web-tools/_static/image51.png "upozornění Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="ee2e4-420">*Microsoft Visual Studio warning*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="ee2e4-421">Přepnout zpět **init.min.js** souboru a ověřte, že soubor byl aktualizován s novým kódem.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="ee2e4-422">![Aktualizovat soubor Init.min.js](visual-studio-2013-web-tools/_static/image52.png "Init.min.js soubor aktualizovat")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="ee2e4-423">*Aktualizovat soubor Init.min.js*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="ee2e4-424">Klikněte **aktualizovat odkaz prohlížeče** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="ee2e4-425">Po aktualizaci obou prohlížečů zvuk hráči, které jste viděli v předchozí úloze se spustí automaticky přehrávání.</span><span class="sxs-lookup"><span data-stu-id="ee2e4-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="ee2e4-426">![Přehrávač zahrnuté v zobrazení](visual-studio-2013-web-tools/_static/image53.png "player zvuk zahrnuté v zobrazení")</span><span class="sxs-lookup"><span data-stu-id="ee2e4-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="ee2e4-427">*Přehrávač zahrnuté v zobrazení*</span><span class="sxs-lookup"><span data-stu-id="ee2e4-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ee2e4-428">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ee2e4-428">Summary</span></span>

<span data-ttu-id="ee2e4-429">Pomocí této praktické cvičení jste se naučili, jak:</span><span class="sxs-lookup"><span data-stu-id="ee2e4-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="ee2e4-430">Používat nové funkce editor HTML součástí Web Essentials, například bohaté fragmenty kódu jazyka HTML5 a Zen kódování</span><span class="sxs-lookup"><span data-stu-id="ee2e4-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="ee2e4-431">Používat nové funkce editor šablon stylů CSS součástí Web Essentials, například výběr barvy a prohlížeče matice popisku</span><span class="sxs-lookup"><span data-stu-id="ee2e4-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="ee2e4-432">Používat nové funkce JavaScript editor součástí Web Essentials, jako je extrakce souboru a IntelliSense pro všechny elementy HTML</span><span class="sxs-lookup"><span data-stu-id="ee2e4-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="ee2e4-433">Výměna dat mezi prohlížečem a sadě Visual Studio pomocí Browser Link</span><span class="sxs-lookup"><span data-stu-id="ee2e4-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
