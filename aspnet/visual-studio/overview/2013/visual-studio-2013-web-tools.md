---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Praktické cvičení: Visual Studio 2013 nástroje pro Web | Dokumentace Microsoftu'
author: rick-anderson
description: Visual Studio je skvělé vývojové prostředí pro. Na základě NET Windows a webové projekty. Obsahuje výkonné textový editor, který můžete jednoduše použít na...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 8f0f19a184d50cdc6e8e5a2da0e55a0e8ca44dc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399397"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="74f86-104">Praktické cvičení: Visual Studio 2013 webové nástroje</span><span class="sxs-lookup"><span data-stu-id="74f86-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="74f86-105">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="74f86-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="74f86-106">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="74f86-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="74f86-107">Visual Studio je skvělé vývojové prostředí pro. Na základě NET Windows a webové projekty.</span><span class="sxs-lookup"><span data-stu-id="74f86-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="74f86-108">Obsahuje výkonné textový editor, který lze snadno použít k úpravě samostatné soubory bez projektu.</span><span class="sxs-lookup"><span data-stu-id="74f86-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="74f86-109">Visual Studio udržuje strom plně funkční analýzy při úpravě každého souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="74f86-110">Díky tomu Visual Studio a poskytují bezkonkurenční automatického dokončování a založenou na dokumentech akce při vytváření vývojové prostředí, příjemnější a mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="74f86-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="74f86-111">Tyto funkce jsou zvláště efektivní v dokumentech HTML a CSS.</span><span class="sxs-lookup"><span data-stu-id="74f86-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="74f86-112">Veškerý tento výkon je k dispozici pro rozšíření, usnadňuje rozšíření editorů výkonné nové funkce tak, aby odpovídala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="74f86-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="74f86-113">Web Essentials je kolekce (většinou) související s webem vylepšení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="74f86-114">Obsahuje i velké množství nového dokončování IntelliSense (hlavně u šablon stylů CSS), nové funkce Browser Link, automatické soubory JSHint pro jazyk JavaScript, nová upozornění pro HTML a CSS a mnoho dalších funkcí, které jsou nezbytné pro moderního webového vývoje.</span><span class="sxs-lookup"><span data-stu-id="74f86-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="74f86-115">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="74f86-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="74f86-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="74f86-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="74f86-117">Cíle</span><span class="sxs-lookup"><span data-stu-id="74f86-117">Objectives</span></span>

<span data-ttu-id="74f86-118">V této praktická cvičení se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="74f86-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="74f86-119">Použít nové funkce editoru HTML součástí Web Essentials, například bohaté fragmenty kódu HTML5 a Zen kódování</span><span class="sxs-lookup"><span data-stu-id="74f86-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="74f86-120">Použít nové funkce editoru šablon stylů CSS součástí Web Essentials, jako je například výběr barvy a popisu matice prohlížeče</span><span class="sxs-lookup"><span data-stu-id="74f86-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="74f86-121">Použít nové funkce editoru jazyka JavaScript součástí Web Essentials, jako je například extrahování do souboru a technologii IntelliSense pro všechny prvky jazyka HTML</span><span class="sxs-lookup"><span data-stu-id="74f86-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="74f86-122">Výměna dat mezi prohlížeče a použití funkce Browser Link sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74f86-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="74f86-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74f86-123">Prerequisites</span></span>

<span data-ttu-id="74f86-124">K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:</span><span class="sxs-lookup"><span data-stu-id="74f86-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="74f86-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="74f86-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="74f86-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="74f86-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="74f86-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="74f86-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="74f86-128">Instalace</span><span class="sxs-lookup"><span data-stu-id="74f86-128">Setup</span></span>

<span data-ttu-id="74f86-129">Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.</span><span class="sxs-lookup"><span data-stu-id="74f86-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="74f86-130">Otevřete okno Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.</span><span class="sxs-lookup"><span data-stu-id="74f86-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="74f86-131">Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a nainstalujte Visual Studio fragmenty kódu pro toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="74f86-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="74f86-132">Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="74f86-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="74f86-133">Ujistěte se, že jste zaškrtli všechny závislosti pro toto testovací prostředí před spuštěním instalace.</span><span class="sxs-lookup"><span data-stu-id="74f86-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="74f86-134">Používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="74f86-134">Using the Code Snippets</span></span>

<span data-ttu-id="74f86-135">V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="74f86-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="74f86-136">Pro usnadnění práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který se dá dostat z v rámci Visual Studio 2013, abyste ho nemuseli znovu přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="74f86-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="74f86-137">Každý cvičení se sadou počáteční řešení nachází v **začít** složky výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="74f86-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="74f86-138">Uvědomte si, že chybí z těchto řešení od fragmenty kódu, které se přidávají během cvičení a nemusí fungovat, dokud nedokončíte výkonu.</span><span class="sxs-lookup"><span data-stu-id="74f86-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="74f86-139">Uvnitř zdrojový kód pro cvičení, můžete také najdete **End** složku, která obsahuje řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení.</span><span class="sxs-lookup"><span data-stu-id="74f86-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="74f86-140">Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.</span><span class="sxs-lookup"><span data-stu-id="74f86-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="74f86-141">Cvičení</span><span class="sxs-lookup"><span data-stu-id="74f86-141">Exercises</span></span>

<span data-ttu-id="74f86-142">Toto praktické testovací prostředí obsahuje následující praktická cvičení:</span><span class="sxs-lookup"><span data-stu-id="74f86-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="74f86-143">Práce s Browser Link a Web Essentials</span><span class="sxs-lookup"><span data-stu-id="74f86-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="74f86-144">Využití výhod fragmenty kódu a technologii IntelliSense</span><span class="sxs-lookup"><span data-stu-id="74f86-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="74f86-145">Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce.</span><span class="sxs-lookup"><span data-stu-id="74f86-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="74f86-146">Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="74f86-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="74f86-147">Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce.</span><span class="sxs-lookup"><span data-stu-id="74f86-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="74f86-148">Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="74f86-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="74f86-149">Cvičení 1: Práce s Browser Link a Web Essentials</span><span class="sxs-lookup"><span data-stu-id="74f86-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="74f86-150">**Web Essentials** je rozšíření sady Visual Studio, která vytváří celou řadu užitečných funkcí pro vývoj moderních webových, především soustředí se na měřitelné webové vývojové prostředí příjemnější a mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="74f86-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="74f86-151">Web Essentials můžete nainstalovat z Galerie rozšíření v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="74f86-152">**Browser Link** je nová funkce zahrnuté v sadě Visual Studio 2013, která poskytuje kanál mezi integrovaném vývojovém prostředí sady Visual Studio a jakýmkoli spuštěným prohlížečem pro výměnu dat mezi vaší webové aplikace a sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="74f86-153">Web Essentials rozšiřuje Browser Link s nástroji pro manipulaci s objektový model DOM a CSS stylů webových stránek přímo z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74f86-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="74f86-154">V tomto cvičení bude prozkoumat některé z funkcí podporovaných **Web Essentials** a **Browser Link** k vylepšení stránku jednoduché kvíz.</span><span class="sxs-lookup"><span data-stu-id="74f86-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="74f86-155">Úloha 1 – spouštění projektu do více prohlížečů</span><span class="sxs-lookup"><span data-stu-id="74f86-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="74f86-156">V této úloze nakonfigurujete webové aplikace na spouštění v několika prohlížečích najednou, což se hodí při testování prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="74f86-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="74f86-157">Otevřít **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="74f86-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="74f86-158">V **souboru** příkaz **Open | Projekt nebo řešení...**  a přejděte do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** v **zdroj** složky testovacího prostředí (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="74f86-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="74f86-159">Vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="74f86-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="74f86-160">Na panelu nástrojů sady Visual Studio v nabídce prohlížeče rozbalte a vyberte **procházet s...** .</span><span class="sxs-lookup"><span data-stu-id="74f86-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="74f86-161">![Možnost nabídky Procházet se](visual-studio-2013-web-tools/_static/image1.png "procházet s... v nabídce prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="74f86-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="74f86-162">*Procházet pomocí nabídky*</span><span class="sxs-lookup"><span data-stu-id="74f86-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="74f86-163">V **procházet s** dialogovém okně vyberte **Google Chrome** a **aplikace Internet Explorer** současným **CTRL** key a klikněte na tlačítko  **Nastavit jako výchozí**.</span><span class="sxs-lookup"><span data-stu-id="74f86-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="74f86-164">![Procházet s dialogovým oknem](visual-studio-2013-web-tools/_static/image2.png "procházet s dialogovým oknem")</span><span class="sxs-lookup"><span data-stu-id="74f86-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="74f86-165">*Výběr více výchozích prohlížečů*</span><span class="sxs-lookup"><span data-stu-id="74f86-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="74f86-166">Google Chrome a prohlížeče Internet Explorer by měla nyní zobrazují jako nastavení výchozích prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="74f86-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="74f86-167">Klikněte na tlačítko **zrušit** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74f86-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="74f86-168">![Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče](visual-studio-2013-web-tools/_static/image3.png "výchozího prohlížeče Google Chrome a Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="74f86-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="74f86-169">*Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="74f86-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-170">Po konfiguraci nastavení výchozích prohlížečů **více prohlížečů** je vybraná možnost v nabídce prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74f86-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="74f86-171">![Více prohlížečů](visual-studio-2013-web-tools/_static/image4.png "více prohlížečů")</span><span class="sxs-lookup"><span data-stu-id="74f86-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="74f86-172">Stisknutím klávesy **CTRL** + **F5** ke spuštění aplikace bez ladění.</span><span class="sxs-lookup"><span data-stu-id="74f86-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="74f86-173">Při otevření obě okna prohlížeče, umístíte jeden z nich nad nich chcete-li zobrazit aktualizace v prohlížečích, oba současně.</span><span class="sxs-lookup"><span data-stu-id="74f86-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="74f86-174">Prohlížečů zobrazit webovou stránku pomocí světle modrý obdélník.</span><span class="sxs-lookup"><span data-stu-id="74f86-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="74f86-175">![Umístit jeden prohlížeč nad sebou](visual-studio-2013-web-tools/_static/image5.png "umístit jeden prohlížeč nad sebou")</span><span class="sxs-lookup"><span data-stu-id="74f86-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="74f86-176">*Umístit jeden prohlížeč nad sebou*</span><span class="sxs-lookup"><span data-stu-id="74f86-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="74f86-177">Nezavírejte prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="74f86-177">Do not close the browsers.</span></span> <span data-ttu-id="74f86-178">Je použijete v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="74f86-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="74f86-179">Úloha 2 – Zen pomocí kódování k vytváření prvků HTML</span><span class="sxs-lookup"><span data-stu-id="74f86-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="74f86-180">**Kódování Zen** je editor kódování modul plug-in pro vysokorychlostní HTML, XML, XSL (nebo jiný formát strukturovaný kód) a úpravy.</span><span class="sxs-lookup"><span data-stu-id="74f86-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="74f86-181">Základní tento modul plug-in je zkratka pro efektivní modul, který umožňuje rozšířit výrazů - podobný selektorů CSS - do kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="74f86-182">Kódování Zen je rychlý způsob zápisu selektor syntaxe stylu HTML pomocí šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="74f86-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="74f86-183">V tomto cvičení se použije k vygenerování tlačítka jazyka HTML představující možnosti dotaz Zen kódování funkcí poskytovaných Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="74f86-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="74f86-184">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="74f86-185">Otevřít **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="74f86-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="74f86-186">Nahraďte **&lt;!--TODO: přidejte možnosti--&gt;** komentář s následujícím kódem a stisknutím klávesy **kartu**.</span><span class="sxs-lookup"><span data-stu-id="74f86-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="74f86-187">Kód by měly být rozbalené do formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="74f86-188">![Rozbalit HTML](visual-studio-2013-web-tools/_static/image6.png "rozšířit HTML")</span><span class="sxs-lookup"><span data-stu-id="74f86-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="74f86-189">*Rozšířené HTML*</span><span class="sxs-lookup"><span data-stu-id="74f86-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-190">Další informace o psaní kódu Zen syntaxe, naleznete na následujícím [článku](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="74f86-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="74f86-191">Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74f86-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="74f86-192">![Aktualizovat propojené prohlížeče](visual-studio-2013-web-tools/_static/image7.png "aktualizovat propojené prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="74f86-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="74f86-193">*Aktualizovat propojené prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="74f86-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="74f86-194">![Internet Explorer - stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image8.png "aplikace Internet Explorer - aktualizaci stránky se čtyřmi tlačítky")</span><span class="sxs-lookup"><span data-stu-id="74f86-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="74f86-195">*Internet Explorer - aktualizaci stránky se čtyřmi tlačítky*</span><span class="sxs-lookup"><span data-stu-id="74f86-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="74f86-196">![Google Chrome – stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – stránka aktualizuje se čtyřmi tlačítky")</span><span class="sxs-lookup"><span data-stu-id="74f86-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="74f86-197">*Google Chrome – stránka aktualizuje se čtyřmi tlačítky*</span><span class="sxs-lookup"><span data-stu-id="74f86-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="74f86-198">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="74f86-199">Tlačítka jste přidali na stránku, ale je stále potřeba přidat otázku šablony.</span><span class="sxs-lookup"><span data-stu-id="74f86-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="74f86-200">K tomu použijete novou funkci v Web Essentials volá **Lorem Ipsum generátor**.</span><span class="sxs-lookup"><span data-stu-id="74f86-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="74f86-201">Vyhledejte **div** křížkem **třídy** atribut **front**.</span><span class="sxs-lookup"><span data-stu-id="74f86-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="74f86-202">Přidejte následující kód jako první podřízený element **div**a stiskněte klávesu **kartu**.</span><span class="sxs-lookup"><span data-stu-id="74f86-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="74f86-203">Kód by měly být rozbalené do formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="74f86-204">![Automaticky generované Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "automaticky generované Lorem Ipsum")</span><span class="sxs-lookup"><span data-stu-id="74f86-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="74f86-205">*Automaticky generované Lorem Ipsum*</span><span class="sxs-lookup"><span data-stu-id="74f86-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-206">Jako součást Zen kódování teď můžete vygenerovat Lorem Ipsum kódu přímo v editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="74f86-207">Jednoduše zadáte **lorem** a přístupů **kartu** a word Lorem Ipsum text bude vložen 30.</span><span class="sxs-lookup"><span data-stu-id="74f86-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="74f86-208">Například</span><span class="sxs-lookup"><span data-stu-id="74f86-208">E.g.</span></span> <span data-ttu-id="74f86-209">*lorem10* vloží 10 Lorem Ipsum slova.</span><span class="sxs-lookup"><span data-stu-id="74f86-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="74f86-210">Přidáte logo, které se v horní části na otázku pomocí další novou funkcí ve Web Essentials volá **Lorem Pixel generátor**.</span><span class="sxs-lookup"><span data-stu-id="74f86-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="74f86-211">Přidejte následující kód jako první podřízený element **div** element s **kontejneru** jako **třídy** hodnotu a stiskněte klávesu **kartu**.</span><span class="sxs-lookup"><span data-stu-id="74f86-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="74f86-212">Kód by měl rozšířit do formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="74f86-213">![Automaticky generované lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "automaticky generované Lorem pixelů")</span><span class="sxs-lookup"><span data-stu-id="74f86-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="74f86-214">*Automaticky generované lorem pixelů*</span><span class="sxs-lookup"><span data-stu-id="74f86-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-215">Jako součást Zen psaní kódu můžete vygenerovat také Lorem Pixel kódu přímo v editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="74f86-216">Jednoduše zadáte **pix. 200 x 200 zvířat** a přístupů **kartu** a **img** vloží značku s 200 x 200 obrázků zvířat.</span><span class="sxs-lookup"><span data-stu-id="74f86-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="74f86-217">Další informace najdete v [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="74f86-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="74f86-218">Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74f86-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="74f86-219">![Internet Explorer - automaticky generované obrázků a textu](visual-studio-2013-web-tools/_static/image12.png "aplikace Internet Explorer - automaticky generované obrázků a textu")</span><span class="sxs-lookup"><span data-stu-id="74f86-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="74f86-220">*Internet Explorer - automaticky generované obrázků a textu*</span><span class="sxs-lookup"><span data-stu-id="74f86-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="74f86-221">![Google Chrome – automaticky generované obrázků a textu](visual-studio-2013-web-tools/_static/image13.png "Google Chrome – automaticky generované obrázků a textu")</span><span class="sxs-lookup"><span data-stu-id="74f86-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="74f86-222">*Google Chrome – automaticky generované obrázků a textu*</span><span class="sxs-lookup"><span data-stu-id="74f86-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-223">Protože image je vybrán náhodně při přidání fragmentu kódu, se může lišit na obrázku je znázorněno v prohlížečích.</span><span class="sxs-lookup"><span data-stu-id="74f86-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="74f86-224">Nezavírejte prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="74f86-224">Do not close the browsers.</span></span> <span data-ttu-id="74f86-225">Je použijete v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="74f86-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="74f86-226">Úloha 3 - aktualizace vlastnosti stylu</span><span class="sxs-lookup"><span data-stu-id="74f86-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="74f86-227">V této úloze budete používat Browser Link **kontrolovat režimu** funkce ke zjišťování přesné umístění, ve kterém se vygeneruje konkrétní elementu DOM a pak aktualizujte vlastnost barvu tohoto elementu s použitím barvu ovládacího prvku pro výběr k dispozici na webu Základy.</span><span class="sxs-lookup"><span data-stu-id="74f86-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="74f86-228">V prohlížeči Internet Explorer, stiskněte klávesu **CTRL** + **ALT** + **můžu** umožňující kontrolovat režimu.</span><span class="sxs-lookup"><span data-stu-id="74f86-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="74f86-229">Přesuňte ukazatel nad světla modré ohraničení a klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74f86-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="74f86-230">![Ukazatele přes hranice světle modrý](visual-studio-2013-web-tools/_static/image14.png "ukazatele přes hranice světle modrá")</span><span class="sxs-lookup"><span data-stu-id="74f86-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="74f86-231">*Ukazatele přes hranice světle modrá*</span><span class="sxs-lookup"><span data-stu-id="74f86-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="74f86-232">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="74f86-233">Všimněte si, jak prvek HTML, který jste vybrali v prohlížeči také vybírá v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="74f86-234">![Element HTML zvoleným v editoru Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "zvoleným v editoru Visual Studio HTML elementu HTML")</span><span class="sxs-lookup"><span data-stu-id="74f86-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="74f86-235">*Element HTML zvoleným v editoru Visual Studio HTML*</span><span class="sxs-lookup"><span data-stu-id="74f86-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="74f86-236">Bude nyní aktualizovat **přední** třídu šablony stylů CSS, chcete-li změnit stylu vybraného prvku.</span><span class="sxs-lookup"><span data-stu-id="74f86-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="74f86-237">Chcete-li to provést, stiskněte **CTRL** + **,** otevřít **přejít na** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="74f86-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="74f86-238">Typ **site.css** a stiskněte klávesu **ENTER** k otevření souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="74f86-239">![Otevření souboru Site.css](visual-studio-2013-web-tools/_static/image16.png "otevírání souboru Site.css")</span><span class="sxs-lookup"><span data-stu-id="74f86-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="74f86-240">*Otevírání souboru Site.css*</span><span class="sxs-lookup"><span data-stu-id="74f86-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="74f86-241">Stisknutím klávesy **CTRL** + **F** a typ **.flip containter .front** najít selektor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="74f86-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="74f86-242">Klikněte na tlačítko světle modrý čtverec ve vlastnosti třídy pro otevření výběru barvy ohraničení.</span><span class="sxs-lookup"><span data-stu-id="74f86-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="74f86-243">![Otevření výběru barvy](visual-studio-2013-web-tools/_static/image17.png "otevření výběru barvy")</span><span class="sxs-lookup"><span data-stu-id="74f86-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="74f86-244">*Otevření dialogu pro výběr barvy*</span><span class="sxs-lookup"><span data-stu-id="74f86-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="74f86-245">Rozbalit výběr barvy kliknutím tlačítko s dvojitou šipkou a vyberte novou barvu.</span><span class="sxs-lookup"><span data-stu-id="74f86-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="74f86-246">![Rozšíření výběru barvy](visual-studio-2013-web-tools/_static/image18.png "rozšíření výběr barvy")</span><span class="sxs-lookup"><span data-stu-id="74f86-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="74f86-247">*Rozšíření výběr barvy*</span><span class="sxs-lookup"><span data-stu-id="74f86-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="74f86-248">Stisknutím klávesy **CTRL** + **ALT** + **ENTER** nutné aktualizovat propojené prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74f86-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="74f86-249">Přepněte do aplikace Internet Explorer a Všimněte si, jak se změní barva ohraničení.</span><span class="sxs-lookup"><span data-stu-id="74f86-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="74f86-250">![Internet Explorer – Barva ohraničení aktualizovat](visual-studio-2013-web-tools/_static/image19.png "aplikace Internet Explorer – Barva ohraničení aktualizovat")</span><span class="sxs-lookup"><span data-stu-id="74f86-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="74f86-251">*Internet Explorer – Barva ohraničení aktualizovat*</span><span class="sxs-lookup"><span data-stu-id="74f86-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="74f86-252">Přepněte do Google Chrome a Všimněte si, jak se změní barva ohraničení.</span><span class="sxs-lookup"><span data-stu-id="74f86-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="74f86-253">![Google Chrome – Barva ohraničení aktualizovat](visual-studio-2013-web-tools/_static/image20.png "Google Chrome – Barva ohraničení aktualizovat")</span><span class="sxs-lookup"><span data-stu-id="74f86-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="74f86-254">*Google Chrome – Barva ohraničení aktualizovat*</span><span class="sxs-lookup"><span data-stu-id="74f86-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="74f86-255">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="74f86-256">Přejděte na konec objektu **Site.css** souboru a stiskněte klávesu **CTRL** + **F** vyhledejte **.btn** selektor.</span><span class="sxs-lookup"><span data-stu-id="74f86-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="74f86-257">Všimněte si, že **vlastnost - webkit-border-radius** vlastnost je podtržený zeleně.</span><span class="sxs-lookup"><span data-stu-id="74f86-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="74f86-258">![Vlastnost - webkit-border-radius selektoru btn](visual-studio-2013-web-tools/_static/image21.png "vlastnost - webkit-border-radius selektoru btn")</span><span class="sxs-lookup"><span data-stu-id="74f86-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="74f86-259">*Vlastnost - webkit-border-radius selektoru btn*</span><span class="sxs-lookup"><span data-stu-id="74f86-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="74f86-260">Umístěte kurzor v **vlastnost - webkit-border-radius** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="74f86-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="74f86-261">Modrá čára by se zobrazit v rámci první písmeno prvního slova vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="74f86-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="74f86-262">Toto je **inteligentní značka**.</span><span class="sxs-lookup"><span data-stu-id="74f86-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="74f86-263">Stisknutím klávesy **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="74f86-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="74f86-264">Otevření nabídky návrhy a klepněte na tlačítko **přidat chybí vlastnost standardní (border-radius)**.</span><span class="sxs-lookup"><span data-stu-id="74f86-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="74f86-265">![Přidat chybějící standardní vlastnosti návrh](visual-studio-2013-web-tools/_static/image22.png "přidat chybějící návrh standardní vlastnosti")</span><span class="sxs-lookup"><span data-stu-id="74f86-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="74f86-266">*Přidat chybějící návrh standardní vlastnosti*</span><span class="sxs-lookup"><span data-stu-id="74f86-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="74f86-267">**Border-radius** automaticky přidána vlastnost do pravidla šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="74f86-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="74f86-268">![Chybí standard byla přidána vlastnost](visual-studio-2013-web-tools/_static/image23.png "chybí standard byla přidána vlastnost")</span><span class="sxs-lookup"><span data-stu-id="74f86-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="74f86-269">*Chybí standard byla přidána vlastnost*</span><span class="sxs-lookup"><span data-stu-id="74f86-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="74f86-270">Přesunutí ukazatele myši **border-radius** vlastnost pro zobrazení **prohlížeče matice popisek**.</span><span class="sxs-lookup"><span data-stu-id="74f86-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="74f86-271">**Prohlížeče matice popisek** zobrazuje dostupnost vlastnosti v každým prohlížečem.</span><span class="sxs-lookup"><span data-stu-id="74f86-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="74f86-272">![Popis matice prohlížeče](visual-studio-2013-web-tools/_static/image24.png "prohlížeče matice tooltip")</span><span class="sxs-lookup"><span data-stu-id="74f86-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="74f86-273">*Popis matice prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="74f86-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="74f86-274">Všimněte si, že hodnota **border-radius** vlastnost je stále podtržené.</span><span class="sxs-lookup"><span data-stu-id="74f86-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="74f86-275">Přesuňte ukazatel nad hodnotu, která se zobrazí zpráva upozorňující.</span><span class="sxs-lookup"><span data-stu-id="74f86-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="74f86-276">![Poloměr ohraničení vlastnost hodnotu upozornění](visual-studio-2013-web-tools/_static/image25.png "Border-radius vlastnost hodnotu upozornění")</span><span class="sxs-lookup"><span data-stu-id="74f86-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="74f86-277">*Poloměr ohraničení vlastnost hodnotu upozornění*</span><span class="sxs-lookup"><span data-stu-id="74f86-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="74f86-278">Odebrání jednotky **border-radius** hodnotu vlastnosti, jak je navrženo v popisu tlačítka.</span><span class="sxs-lookup"><span data-stu-id="74f86-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="74f86-279">Jako **border-radius** je vlastnost standard pro definování jak zaoblené rohy jsou, můžete odebrat ohraničení **vlastnost - webkit-border-radius** vlastnosti a hodnoty z pravidla šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="74f86-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="74f86-280">Umístěte kurzor v **zalamování slov** vlastnost a Všimněte si, že se inteligentní značky se zobrazí také níže.</span><span class="sxs-lookup"><span data-stu-id="74f86-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="74f86-281">Otevřete nabídku a klikněte na tlačítko **přidat chybějící specifika dodavatele**.</span><span class="sxs-lookup"><span data-stu-id="74f86-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="74f86-282">![Přidat chybějící dodavatele specifika návrhu](visual-studio-2013-web-tools/_static/image26.png "přidat chybějící dodavatele specifika návrhu")</span><span class="sxs-lookup"><span data-stu-id="74f86-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="74f86-283">*Přidat chybějící dodavatele specifika návrhu*</span><span class="sxs-lookup"><span data-stu-id="74f86-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="74f86-284">**-Ms-zalamování** automaticky přidána vlastnost do pravidla šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="74f86-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="74f86-285">![Byla přidána vlastnost konkrétního dodavatele](visual-studio-2013-web-tools/_static/image27.png "byla přidána vlastnost konkrétního dodavatele")</span><span class="sxs-lookup"><span data-stu-id="74f86-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="74f86-286">*Byla přidána vlastnost konkrétního dodavatele*</span><span class="sxs-lookup"><span data-stu-id="74f86-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="74f86-287">Úloha 4 – aktualizace kódu HTML z prohlížeče</span><span class="sxs-lookup"><span data-stu-id="74f86-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="74f86-288">V této úloze budete používat Browser Link **režimu návrhu** funkce Upravit objekt modelu DOM z prohlížeče a přenést změny do zdrojového souboru HTML v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74f86-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="74f86-289">V prohlížeči Google Chrome, stiskněte klávesu **CTRL** + **ALT** + **D** povolení režimu návrhu.</span><span class="sxs-lookup"><span data-stu-id="74f86-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="74f86-290">Přesunutí ukazatele myši **Lorem Ipsum dolor sit amet** označovat popisky a klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74f86-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="74f86-291">![Úpravy na otázku](visual-studio-2013-web-tools/_static/image28.png "úpravy na otázku")</span><span class="sxs-lookup"><span data-stu-id="74f86-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="74f86-292">*Úpravy na otázku*</span><span class="sxs-lookup"><span data-stu-id="74f86-292">*Editing the question*</span></span>
3. <span data-ttu-id="74f86-293">Kurzor by se zobrazit.</span><span class="sxs-lookup"><span data-stu-id="74f86-293">A cursor should appear.</span></span> <span data-ttu-id="74f86-294">Nahraďte původní text s *co to vypadá při psaní delší dotaz?* a potom stiskněte klávesu **ESC** pro ukončení režimu návrhu.</span><span class="sxs-lookup"><span data-stu-id="74f86-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="74f86-295">![Upravit dotaz](visual-studio-2013-web-tools/_static/image29.png "upravit dotaz")</span><span class="sxs-lookup"><span data-stu-id="74f86-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="74f86-296">*Upravit dotaz*</span><span class="sxs-lookup"><span data-stu-id="74f86-296">*Question edited*</span></span>
4. <span data-ttu-id="74f86-297">Přepněte zpět do sady Visual Studio a otevřete **Index.cshtml**, pokud ještě není otevřen.</span><span class="sxs-lookup"><span data-stu-id="74f86-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="74f86-298">Všimněte si, že vnitřní text **&lt;p&gt;** element byl aktualizován.</span><span class="sxs-lookup"><span data-stu-id="74f86-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="74f86-299">![Aktualizované dotaz na stránce HTML](visual-studio-2013-web-tools/_static/image30.png "aktualizované dotaz na stránce HTML")</span><span class="sxs-lookup"><span data-stu-id="74f86-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="74f86-300">*Aktualizované dotaz na stránce HTML*</span><span class="sxs-lookup"><span data-stu-id="74f86-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="74f86-301">Úloha 5: revize SEO související upozornění</span><span class="sxs-lookup"><span data-stu-id="74f86-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="74f86-302">**Optimalizace vyhledávacích** (vyhledávací weby SEO) je proces zpřístupnění vyšší webu pořadí na vyhledávací web ze seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="74f86-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="74f86-303">Vyšší webu řadí a čím více konzistentně je uveden, další návštěvníků webu získáte z tohoto vyhledávací web.</span><span class="sxs-lookup"><span data-stu-id="74f86-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="74f86-304">Web Essentials zahrnuje analytického nástroje, který prověří HTML, sestavy problémy najít a poskytuje pomoc a opravte je.</span><span class="sxs-lookup"><span data-stu-id="74f86-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="74f86-305">Přejděte **zobrazení** nabídky a klikněte na **seznam chyb** otevřete **seznam chyb** okno.</span><span class="sxs-lookup"><span data-stu-id="74f86-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="74f86-306">![Seznam chyb v zobrazení nabídky](visual-studio-2013-web-tools/_static/image31.png "seznam chyb v nabídce zobrazení")</span><span class="sxs-lookup"><span data-stu-id="74f86-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="74f86-307">*Seznam chyb v zobrazení nabídky*</span><span class="sxs-lookup"><span data-stu-id="74f86-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="74f86-308">Všimněte si, že dochází optimalizace pro vyhledávací weby upozornění s informací, které **&lt;meta&gt;** označit pro chybí popis stránky.</span><span class="sxs-lookup"><span data-stu-id="74f86-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="74f86-309">Dvakrát klikněte na položku upozornění optimalizace pro vyhledávací weby ho opravit.</span><span class="sxs-lookup"><span data-stu-id="74f86-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="74f86-310">![Okno Seznam chyb](visual-studio-2013-web-tools/_static/image32.png "okna Seznam chyb")</span><span class="sxs-lookup"><span data-stu-id="74f86-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="74f86-311">*Okno Seznam chyb*</span><span class="sxs-lookup"><span data-stu-id="74f86-311">*Error List window*</span></span>
3. <span data-ttu-id="74f86-312">V **Web Essentials** dialogové okno, klikněte na tlačítko **Ano** Vložit popis &lt;meta&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="74f86-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="74f86-313">![Dialogové okno Web Essentials](visual-studio-2013-web-tools/_static/image33.png "dialogové okno Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="74f86-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="74f86-314">*Dialogové okno Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="74f86-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="74f86-315">Editor pro  **\_Layout.cshtml** otevře a **&lt;meta&gt;** značky se automaticky přidá do **head** část Soubor HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="74f86-316">![Značka META automaticky přidá _rozložení stránce](visual-studio-2013-web-tools/_static/image34.png "metaznačku automaticky přidán do stránky _rozložení")</span><span class="sxs-lookup"><span data-stu-id="74f86-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="74f86-317">*Značka META automaticky přidá do \_stránku rozložení*</span><span class="sxs-lookup"><span data-stu-id="74f86-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="74f86-318">Změňte hodnotu **obsah** atribut *GeekQuiz* a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="74f86-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="74f86-319">Cvičení 2: Využití výhod fragmenty kódu a technologii IntelliSense</span><span class="sxs-lookup"><span data-stu-id="74f86-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="74f86-320">S Web Essentials bylo rozšířeno pomocí další funkce editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="74f86-321">V tomto cvičení zobrazí se několik nových funkcí, které jsou užitečné při vývoji webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="74f86-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="74f86-322">Úloha 1 – použití technologie IntelliSense do HTML dokumentů</span><span class="sxs-lookup"><span data-stu-id="74f86-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="74f86-323">První nové funkce, zobrazí se v této úloze se označuje jako **dynamické IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="74f86-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="74f86-324">Dynamické IntelliSense načte další značky a atributů, které mají odvodit možné ID, které bude používat.</span><span class="sxs-lookup"><span data-stu-id="74f86-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="74f86-325">V této úloze vytvoříte nový prvek formuláře HTML, který obsahuje popisek a vstupní pole.</span><span class="sxs-lookup"><span data-stu-id="74f86-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="74f86-326">Potom přidáte **pro** atribut label a vytvořte jeho vazbu na vstup a zobrazí se návrhy IntelliSense podle ID vstupů v oboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="74f86-327">Otevřít **Visual Studio Express 2013 for Web** a **Begin.sln** řešení nachází v **zdroj/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="74f86-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="74f86-328">Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="74f86-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="74f86-329">V **Průzkumníka řešení**, otevřete **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="74f86-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="74f86-330">Přidejte následující formulář uvnitř **&lt;části&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="74f86-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="74f86-331">(Fragment - kódu *VisualStudio2013WebTooling* - *Ex2* - *formuláře*)</span><span class="sxs-lookup"><span data-stu-id="74f86-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="74f86-332">Vstupní značky by měl předcházet popisek s přijde nějaký popis pole.</span><span class="sxs-lookup"><span data-stu-id="74f86-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="74f86-333">Přidejte následující popisek před vstupní značka.</span><span class="sxs-lookup"><span data-stu-id="74f86-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="74f86-334">**Pro** atribut **&lt;popisek&gt;** Určuje, který element formuláře a popisku je vázán na.</span><span class="sxs-lookup"><span data-stu-id="74f86-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="74f86-335">Hodnota atributu musí být rovna id elementu související.</span><span class="sxs-lookup"><span data-stu-id="74f86-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="74f86-336">Přidat **pro** atribut **&lt;popisek&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="74f86-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="74f86-337">Jak je znázorněno na následujícím obrázku &quot;název&quot; hodnota se zobrazí v dialogovém okně technologie IntelliSense na základě id elementů v rámci stejného oboru (nadřazený  **&lt;formuláře&gt;**).</span><span class="sxs-lookup"><span data-stu-id="74f86-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="74f86-338">![Zobrazuje id v technologii IntelliSense](visual-studio-2013-web-tools/_static/image35.png "zobrazuje id v technologii IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="74f86-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="74f86-339">*Zobrazuje id v technologii IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="74f86-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="74f86-340">Odstranit naposledy přidané **&lt;formuláře&gt;** elementu a jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="74f86-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="74f86-341">Úloha 2 – používání fragmentů kódu HTML</span><span class="sxs-lookup"><span data-stu-id="74f86-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="74f86-342">HTML5 zavedené více než 25 nové sémantické značky.</span><span class="sxs-lookup"><span data-stu-id="74f86-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="74f86-343">Visual Studio už měli podporu technologie IntelliSense pro tyto značky, ale Visual Studio 2013 umožňuje rychlejší a snazší psát kód tak, že přidáte nový fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="74f86-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="74f86-344">I když tyto značky nejsou složité, jsou součástí několik odlišností malé, jako je například přidávání náhrad správný kodek pro *zvuku* značky.</span><span class="sxs-lookup"><span data-stu-id="74f86-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="74f86-345">V této úloze zobrazí se fragmenty kódu HTML pro značku pro zvuk.</span><span class="sxs-lookup"><span data-stu-id="74f86-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="74f86-346">V **Index.cshtml** soubor, zadejte  **&lt;aud** uvnitř **&lt;části&gt;** elementu, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="74f86-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="74f86-347">![Vložení zvuku elementu](visual-studio-2013-web-tools/_static/image36.png "vložení zvuku elementu")</span><span class="sxs-lookup"><span data-stu-id="74f86-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="74f86-348">*Vložení zvuku elementu*</span><span class="sxs-lookup"><span data-stu-id="74f86-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="74f86-349">Stisknutím klávesy **kartu** dvakrát a Všimněte si, jak se na stránku přidá následující kód a je kurzor umístěn na **src** atribut první zdroj.</span><span class="sxs-lookup"><span data-stu-id="74f86-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="74f86-350">Stisknutím klávesy **kartu** klíče dvakrát vložení fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="74f86-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="74f86-351">Zvukový fragment kódu ukazuje standardní využití *zvuku* značky pomocí dvou zdrojových souborů pro lepší podporu.</span><span class="sxs-lookup"><span data-stu-id="74f86-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="74f86-352">Odstraňte druhý řádek a aktualizovat zdroj prvního řádku na následující odkaz k zobrazení WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="74f86-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="74f86-353">Výsledný kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="74f86-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="74f86-354">Zdrojový soubor *KatanaProject.mp3* slouží jako příklad.</span><span class="sxs-lookup"><span data-stu-id="74f86-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="74f86-355">Pokud dáváte přednost, můžete použít jiný zdroj.</span><span class="sxs-lookup"><span data-stu-id="74f86-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="74f86-356">Stisknutím klávesy **CTRL** + **S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="74f86-357">Stisknutím klávesy **CTRL** + **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="74f86-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="74f86-358">Všimněte si, že zvukový přehrávač přidal do aplikace.</span><span class="sxs-lookup"><span data-stu-id="74f86-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="74f86-359">![Zvukový přehrávač v aplikaci Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "zvukový přehrávač v aplikaci Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="74f86-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="74f86-360">*Zvukový přehrávač v aplikaci Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="74f86-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="74f86-361">![Zvukový přehrávač v prohlížeči Google Chrome](visual-studio-2013-web-tools/_static/image38.png "zvukový přehrávač v prohlížeči Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="74f86-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="74f86-362">*Zvukový přehrávač v prohlížeči Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="74f86-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="74f86-363">Nezavírejte prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="74f86-363">Do not close the browsers.</span></span> <span data-ttu-id="74f86-364">Je použijete v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="74f86-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="74f86-365">Úloha 3 – pomocí technologie IntelliSense jazyka JavaScript dokumentů</span><span class="sxs-lookup"><span data-stu-id="74f86-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="74f86-366">Web Essentials 2013 vytvoření seznamu ID tak názvy tříd šablon stylů a stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="74f86-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="74f86-367">V této úloze se dozvíte, jak jsou tyto seznamy vylepšit podporu technologie IntelliSense jazyka JavaScript v sadě Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="74f86-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="74f86-368">V **Index.cshtml** přidejte následující kód, který definuje **skript** značky pro kód jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="74f86-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="74f86-369">Přidejte následující kód **skript** značka definice funkce připravené zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="74f86-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="74f86-370">(Fragment - kódu *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="74f86-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="74f86-371">Umístěte kurzor v **skript** značky a stiskněte klávesu **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="74f86-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="74f86-372">Otevřete nabídku návrh.</span><span class="sxs-lookup"><span data-stu-id="74f86-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="74f86-373">Klikněte na tlačítko **extrahovat soubor**.</span><span class="sxs-lookup"><span data-stu-id="74f86-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="74f86-374">![Extrahovat jazyka JavaScript do souboru návrh](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrahovat soubor návrh")</span><span class="sxs-lookup"><span data-stu-id="74f86-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="74f86-375">*Extrahovat jazyka JavaScript do souboru návrh*</span><span class="sxs-lookup"><span data-stu-id="74f86-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="74f86-376">V **uložit jako** okna, vyberte **skripty** složku, název souboru **init.js** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="74f86-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="74f86-377">![Uložit jako okno](visual-studio-2013-web-tools/_static/image40.png "okně Uložit jako")</span><span class="sxs-lookup"><span data-stu-id="74f86-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="74f86-378">*Okno Uložit jako*</span><span class="sxs-lookup"><span data-stu-id="74f86-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-379">**Init.js** se vytvoří soubor a obsah souboru, který je přesunut do souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="74f86-380">![Vytvořené pomocí obsahu zahrnutého souboru Init.js](visual-studio-2013-web-tools/_static/image41.png "Init.js soubor vytvořený pomocí zahrnutého obsahu")</span><span class="sxs-lookup"><span data-stu-id="74f86-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="74f86-381">*Init.js soubor vytvořený pomocí zahrnutého obsahu*</span><span class="sxs-lookup"><span data-stu-id="74f86-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="74f86-382">Otevřít **Index.cshtml** soubor a zkontrolujte, že byl nahrazen značku skriptu s odkazem na **init.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="74f86-383">![Odkaz html Init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js odkaz html")</span><span class="sxs-lookup"><span data-stu-id="74f86-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="74f86-384">*Odkaz html Init.js*</span><span class="sxs-lookup"><span data-stu-id="74f86-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="74f86-385">Přejděte **Průzkumníku řešení** a Všimněte si, že **init.js** soubor byl automaticky součástí řešení.</span><span class="sxs-lookup"><span data-stu-id="74f86-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="74f86-386">![Soubor Init.js zahrnutý v řešení](visual-studio-2013-web-tools/_static/image43.png "Init.js soubor zahrnutý v řešení")</span><span class="sxs-lookup"><span data-stu-id="74f86-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="74f86-387">*Soubor Init.js zahrnutý v řešení*</span><span class="sxs-lookup"><span data-stu-id="74f86-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="74f86-388">Přepněte zpátky na **init.js** souboru se má aktualizovat **připravené** funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="74f86-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="74f86-389">Uvnitř definice funkce zpětného volání, která je předána *připravené*, přidejte následující kód se získat všechny elementy s konkrétní třídu atribut.</span><span class="sxs-lookup"><span data-stu-id="74f86-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="74f86-390">Stisknutím klávesy **CTRL** + **místo** mezi uvozovky uvnitř **getElementsByClassName** volání funkce.</span><span class="sxs-lookup"><span data-stu-id="74f86-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="74f86-391">![Zobrazení technologie IntelliSense pro funkci getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zobrazující technologie IntelliSense pro getElementsByClassName – funkce")</span><span class="sxs-lookup"><span data-stu-id="74f86-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="74f86-392">*Zobrazení technologie IntelliSense pro getElementsByClassName – funkce*</span><span class="sxs-lookup"><span data-stu-id="74f86-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-393">Všimněte si, že technologie IntelliSense zobrazuje tříd definovaných v šabloně stylů projektu.</span><span class="sxs-lookup"><span data-stu-id="74f86-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="74f86-394">Nahraďte řádek, který jste vytvořili s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="74f86-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="74f86-395">Umístěte kurzor po **au** v uvozovkách v **getElementsByTagName** funkce a stiskněte klávesu **CTRL** + **místo**.</span><span class="sxs-lookup"><span data-stu-id="74f86-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="74f86-396">![Zobrazení technologie IntelliSense pro metodu getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zobrazující technologie IntelliSense pro getElementByTagName – metoda")</span><span class="sxs-lookup"><span data-stu-id="74f86-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="74f86-397">*Zobrazení technologie IntelliSense pro getElementsByTagName – metoda*</span><span class="sxs-lookup"><span data-stu-id="74f86-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="74f86-398">Vyberte **&quot;zvuku&quot;** ze seznamu a stisknutím klávesy **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="74f86-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="74f86-399">Výsledek je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="74f86-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="74f86-400">![Načítání zvuku prvků](visual-studio-2013-web-tools/_static/image46.png "načítání zvuku prvků")</span><span class="sxs-lookup"><span data-stu-id="74f86-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="74f86-401">*Načítání zvuku prvků*</span><span class="sxs-lookup"><span data-stu-id="74f86-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="74f86-402">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **init.js** soubor **skripty** a pak zvolte položku **soubory JavaScript zrušíte Minifikaci** z **Web Essentials** nabídky.</span><span class="sxs-lookup"><span data-stu-id="74f86-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="74f86-403">![Soubory JavaScript zrušíte minifikaci](visual-studio-2013-web-tools/_static/image47.png "zrušíte Minifikaci Javascriptové soubory")</span><span class="sxs-lookup"><span data-stu-id="74f86-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="74f86-404">*Zrušíte minifikaci soubory jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="74f86-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="74f86-405">Po zobrazení výzvy k povolení automatického připravenost k minifikaci při změně zdrojových souborů, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="74f86-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="74f86-406">![Povolení automatického připravenost k minifikaci upozornění](visual-studio-2013-web-tools/_static/image48.png "povolení automatické připravenost k minifikaci upozornění")</span><span class="sxs-lookup"><span data-stu-id="74f86-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="74f86-407">*Povolení automatického připravenost k minifikaci upozornění*</span><span class="sxs-lookup"><span data-stu-id="74f86-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="74f86-408">**Init.min.js** je vytvořen a přidán jako závislost **init.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="74f86-409">![Vytvořený soubor Init.min.js](visual-studio-2013-web-tools/_static/image49.png "Init.min.js soubor vytvořený")</span><span class="sxs-lookup"><span data-stu-id="74f86-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="74f86-410">*Vytvořený soubor Init.min.js*</span><span class="sxs-lookup"><span data-stu-id="74f86-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="74f86-411">Otevřít **init.min.js** soubor a Všimněte si, že soubor je minifikovaný.</span><span class="sxs-lookup"><span data-stu-id="74f86-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="74f86-412">![Obsah souboru Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js obsah souboru")</span><span class="sxs-lookup"><span data-stu-id="74f86-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="74f86-413">*Obsah souboru Init.min.js*</span><span class="sxs-lookup"><span data-stu-id="74f86-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="74f86-414">V **init.js** přidejte následující kód uvedený níže **getElementsByTagName** volání přehrávání zvuku elementy funkce.</span><span class="sxs-lookup"><span data-stu-id="74f86-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="74f86-415">(Fragment - kódu *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="74f86-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="74f86-416">Klikněte na tlačítko **CTRL** + **S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="74f86-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="74f86-417">Protože minifikovaný soubor je už otevřený, zobrazí se dialogové okno s informacemi o tom, že soubor byl změněn mimo editoru zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="74f86-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="74f86-418">Klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="74f86-418">Click **Yes**.</span></span>

    <span data-ttu-id="74f86-419">![Microsoft Visual Studio upozornění](visual-studio-2013-web-tools/_static/image51.png "upozornění Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="74f86-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="74f86-420">*Microsoft Visual Studio upozornění*</span><span class="sxs-lookup"><span data-stu-id="74f86-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="74f86-421">Přepněte zpět **init.min.js** souboru můžete ověřit, že soubor byl aktualizován s novým kódem.</span><span class="sxs-lookup"><span data-stu-id="74f86-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="74f86-422">![Aktualizovat soubor Init.min.js](visual-studio-2013-web-tools/_static/image52.png "aktualizovat soubor Init.min.js")</span><span class="sxs-lookup"><span data-stu-id="74f86-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="74f86-423">*Aktualizovat soubor Init.min.js*</span><span class="sxs-lookup"><span data-stu-id="74f86-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="74f86-424">Klikněte na tlačítko **aktualizovat odkaz prohlížeče** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74f86-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="74f86-425">Jakmile se aktualizují i prohlížeče hudebních přehrávačů, které jste viděli v předchozí úloze začne přehrávat automaticky.</span><span class="sxs-lookup"><span data-stu-id="74f86-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="74f86-426">![Zvukový přehrávač zahrnuté v zobrazení](visual-studio-2013-web-tools/_static/image53.png "zvukový přehrávač zahrnuté v zobrazení")</span><span class="sxs-lookup"><span data-stu-id="74f86-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="74f86-427">*Zvukový přehrávač zahrnuté v zobrazení*</span><span class="sxs-lookup"><span data-stu-id="74f86-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="74f86-428">Souhrn</span><span class="sxs-lookup"><span data-stu-id="74f86-428">Summary</span></span>

<span data-ttu-id="74f86-429">Po dokončení tohoto praktických cvičení jste se naučili, jak:</span><span class="sxs-lookup"><span data-stu-id="74f86-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="74f86-430">Použít nové funkce editoru HTML součástí Web Essentials, například bohaté fragmenty kódu HTML5 a Zen kódování</span><span class="sxs-lookup"><span data-stu-id="74f86-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="74f86-431">Použít nové funkce editoru šablon stylů CSS součástí Web Essentials, jako je například výběr barvy a popisu matice prohlížeče</span><span class="sxs-lookup"><span data-stu-id="74f86-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="74f86-432">Použít nové funkce editoru jazyka JavaScript součástí Web Essentials, jako je například extrahování do souboru a technologii IntelliSense pro všechny prvky jazyka HTML</span><span class="sxs-lookup"><span data-stu-id="74f86-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="74f86-433">Výměna dat mezi prohlížeče a použití funkce Browser Link sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74f86-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
