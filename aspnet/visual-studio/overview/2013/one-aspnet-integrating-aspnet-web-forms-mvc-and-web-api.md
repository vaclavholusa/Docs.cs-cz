---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Rukou na testovacím: Jeden ASP.NET: integrace webové formuláře ASP.NET, MVC a webových rozhraní API | Microsoft Docs'
author: rick-anderson
description: ASP.NET je architektura pro vytváření webů, aplikacím a službám pomocí speciální technologie jako MVC, Web API a dalších. Rozšiřující ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566440"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="3004c-104">Rukou na testovacím: Jeden ASP.NET: integrace webové formuláře ASP.NET, MVC a webových rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3004c-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="3004c-105">Podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3004c-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="3004c-106">Stažení webové táborech cvičení Kit</span><span class="sxs-lookup"><span data-stu-id="3004c-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="3004c-107">ASP.NET je architektura pro vytváření webů, aplikacím a službám pomocí speciální technologie jako MVC, Web API a dalších.</span><span class="sxs-lookup"><span data-stu-id="3004c-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="3004c-108">Rozšiřující ASP.NET zaznamenal od svého vytvoření a uvedeného musí mít tyto technologie integrované, došlo ke poslední úsilí naplňování **jeden ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="3004c-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="3004c-109">Visual Studio 2013 zavádí nový systém jednotná projektu, která vám umožní sestavit aplikaci a použít všechny technologie ASP.NET v jednom projektu.</span><span class="sxs-lookup"><span data-stu-id="3004c-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="3004c-110">Tato funkce eliminuje potřebu vyberte jeden technologie na začátku projektu a Flash disk s ním a místo toho umožňuje použití více rozhraní ASP.NET v rámci jednoho projektu.</span><span class="sxs-lookup"><span data-stu-id="3004c-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="3004c-111">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="3004c-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="3004c-112">Přehled</span><span class="sxs-lookup"><span data-stu-id="3004c-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="3004c-113">Cíle</span><span class="sxs-lookup"><span data-stu-id="3004c-113">Objectives</span></span>

<span data-ttu-id="3004c-114">V tomto testovacím prostředí praktických se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="3004c-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="3004c-115">Vytvoření webu na základě **jeden ASP.NET** typ projektu</span><span class="sxs-lookup"><span data-stu-id="3004c-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="3004c-116">Použití různých **ASP.NET** rozhraní jako **MVC** a **webového rozhraní API** ve stejném projektu</span><span class="sxs-lookup"><span data-stu-id="3004c-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="3004c-117">Určení hlavních komponent **ASP.NET** aplikace</span><span class="sxs-lookup"><span data-stu-id="3004c-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="3004c-118">Využít **ASP.NET generování uživatelského rozhraní** framework automaticky vytvořit Kontrolery a zobrazení, které slouží k provádění operací CRUD podle tříd modelu</span><span class="sxs-lookup"><span data-stu-id="3004c-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="3004c-119">Vystavení stejnou sadu informace v počítači - a čitelná pro člověka formátech pomocí ten nejvhodnější nástroj pro každou úlohu</span><span class="sxs-lookup"><span data-stu-id="3004c-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3004c-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3004c-120">Prerequisites</span></span>

<span data-ttu-id="3004c-121">Pro dokončení této praktické cvičení je vyžadován následující text:</span><span class="sxs-lookup"><span data-stu-id="3004c-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="3004c-122">[Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/) nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="3004c-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="3004c-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="3004c-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3004c-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="3004c-124">Setup</span></span>

<span data-ttu-id="3004c-125">Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.</span><span class="sxs-lookup"><span data-stu-id="3004c-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="3004c-126">Otevřete Průzkumníka Windows a přejděte do testovacího prostředí na **zdroj** složky.</span><span class="sxs-lookup"><span data-stu-id="3004c-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="3004c-127">Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="3004c-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="3004c-128">Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.</span><span class="sxs-lookup"><span data-stu-id="3004c-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="3004c-129">Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="3004c-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="3004c-130">Používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="3004c-130">Using the Code Snippets</span></span>

<span data-ttu-id="3004c-131">V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="3004c-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="3004c-132">Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="3004c-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="3004c-133">Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="3004c-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="3004c-134">Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu.</span><span class="sxs-lookup"><span data-stu-id="3004c-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="3004c-135">Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení.</span><span class="sxs-lookup"><span data-stu-id="3004c-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="3004c-136">Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.</span><span class="sxs-lookup"><span data-stu-id="3004c-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3004c-137">Cvičení</span><span class="sxs-lookup"><span data-stu-id="3004c-137">Exercises</span></span>

<span data-ttu-id="3004c-138">Toto praktické cvičení zahrnuje následující cvičení:</span><span class="sxs-lookup"><span data-stu-id="3004c-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="3004c-139">Vytvoření nového projektu webové formuláře</span><span class="sxs-lookup"><span data-stu-id="3004c-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="3004c-140">Vytvoření řadiče MVC pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3004c-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="3004c-141">Vytvoření řadiče webové rozhraní API pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3004c-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="3004c-142">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="3004c-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="3004c-143">Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce.</span><span class="sxs-lookup"><span data-stu-id="3004c-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="3004c-144">Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3004c-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="3004c-145">Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce.</span><span class="sxs-lookup"><span data-stu-id="3004c-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="3004c-146">Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="3004c-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="3004c-147">Cvičení 1: Vytvoření nového projektu webové formuláře</span><span class="sxs-lookup"><span data-stu-id="3004c-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="3004c-148">V tomto cvičení vytvoříte novou lokalitu webové formuláře v sadě Visual Studio 2013 pomocí **jeden ASP.NET** jednotné rozhraní projektu, který vám umožní snadno integrovat webových formulářů, MVC a webového rozhraní API součásti ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3004c-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="3004c-149">Potom zkoumat generovaného řešení a identifikovat jeho části, a nakonec bude naleznete na webu v akci.</span><span class="sxs-lookup"><span data-stu-id="3004c-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="3004c-150">Úloha 1 – Vytvoření nové lokality pomocí jedné prostředí ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3004c-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="3004c-151">V této úloze se spustí vytváření nového webu v sadě Visual Studio na základě **jeden ASP.NET** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="3004c-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="3004c-152">**Jeden ASP.NET** kombinuje všechny technologie ASP.NET a vám dává možnost kombinovat a párovat je podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3004c-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="3004c-153">Různé komponenty webových formulářů, MVC a webového rozhraní API, které za provozu se pak rozpoznat vedle sebe v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3004c-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="3004c-154">Otevřete **Visual Studio Express 2013 pro Web** a vyberte **souboru | Nový projekt...**  zahájíte nové řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Vytvoření nového projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="3004c-156">*Vytvoření nového projektu*</span><span class="sxs-lookup"><span data-stu-id="3004c-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="3004c-157">V **nový projekt** dialogové okno, vyberte **webové aplikace ASP.NET** pod **Visual C# | Webové** kartě a zajistěte, aby **rozhraní .NET Framework 4.5** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="3004c-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="3004c-158">Název projektu *MyHybridSite*, vyberte **umístění** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3004c-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nový projekt webové aplikace ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="3004c-160">*Vytvoření nového projektu webové aplikace ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="3004c-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="3004c-161">V **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony a vyberte **MVC** a **webového rozhraní API** možnosti.</span><span class="sxs-lookup"><span data-stu-id="3004c-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="3004c-162">Také zkontrolujte, zda **ověřování** je možnost nastavena na **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="3004c-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="3004c-163">Klikněte na tlačítko **OK** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="3004c-163">Click **OK** to continue.</span></span>

    ![Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="3004c-165">*Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC*</span><span class="sxs-lookup"><span data-stu-id="3004c-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="3004c-166">Nyní můžete prozkoumat strukturu generovaného řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-166">You can now explore the structure of the generated solution.</span></span>

    ![Zkoumání generovaného řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="3004c-168">*Zkoumání generovaného řešení*</span><span class="sxs-lookup"><span data-stu-id="3004c-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="3004c-169">**Účet:** tato složka obsahuje stránky webového formuláře, které chcete zaregistrovat, přihlaste se k a spravovat aplikace uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="3004c-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="3004c-170">Tato složka je vytvořena při **jednotlivé uživatelské účty** během konfigurace webové formuláře v šabloně projektů je vybraná možnost ověřování.</span><span class="sxs-lookup"><span data-stu-id="3004c-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="3004c-171">**Modely:** tato složka bude obsahovat třídy, které představují data aplikací.</span><span class="sxs-lookup"><span data-stu-id="3004c-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="3004c-172">**Řadiče** a **zobrazení**: jsou požadovány pro tyto složky **ASP.NET MVC** a **rozhraní ASP.NET Web API** součásti.</span><span class="sxs-lookup"><span data-stu-id="3004c-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="3004c-173">Zaměříte MVC a webového rozhraní API technologie v dalším cvičení.</span><span class="sxs-lookup"><span data-stu-id="3004c-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="3004c-174">**Default.aspx**, **Contact.aspx** a **About.aspx** soubory jsou předem definované stránky webového formuláře, které můžete použít jako výchozí body k vytvoření stránky, které jsou specifické pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="3004c-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="3004c-175">Programovací logiku těchto souborů se nachází v samostatném souboru označuje jako &quot;kódu&quot; souboru, který má &quot;. aspx&quot; nebo &quot;. aspx.cs&quot; rozšíření (v závislosti na jazyk použitý).</span><span class="sxs-lookup"><span data-stu-id="3004c-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="3004c-176">Logika kódu na pozadí běží na serveru a dynamicky vytvoří výstupu protokolu HTML pro stránku.</span><span class="sxs-lookup"><span data-stu-id="3004c-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="3004c-177">**Site.Master** a **Site.Mobile.Master** stránky definujte vzhled a chování standardní všechny stránky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3004c-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="3004c-178">Dvakrát klikněte **Default.aspx** souboru a prozkoumejte obsah stránky.</span><span class="sxs-lookup"><span data-stu-id="3004c-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Prohlížení stránky Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="3004c-180">*Prohlížení stránky Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="3004c-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-181">**Stránky** – direktiva v horní části souboru definuje atributy stránky webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="3004c-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="3004c-182">Například **MasterPageFile** atribut určuje cestu k hlavní stránce – v takovém případě *Site.Master* stránky – a **Inherits** atribut definuje třída kódu pro stránku dědění.</span><span class="sxs-lookup"><span data-stu-id="3004c-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="3004c-183">Tato třída se nachází v souboru dáno **CodeBehind** atribut.</span><span class="sxs-lookup"><span data-stu-id="3004c-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="3004c-184">**Asp: obsah** řízení obsahuje skutečný obsah stránky (text, značky a ovládací prvky) a je namapovaná na **asp: ContentPlaceHolder** ovládacího prvku na hlavní stránku.</span><span class="sxs-lookup"><span data-stu-id="3004c-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="3004c-185">V takovém případě bude možné vykreslit obsah stránky v rámci *MainContent* prvek definovaný v *Site.Master* stránky.</span><span class="sxs-lookup"><span data-stu-id="3004c-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="3004c-186">Rozbalte **aplikace\_spustit** složky a Všimněte si **WebApiConfig.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="3004c-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="3004c-187">Visual Studio součástí souboru generovaného řešení jste do webového rozhraní API, při konfiguraci projektu pomocí šablony jeden ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3004c-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="3004c-188">Otevřete **WebApiConfig.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="3004c-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="3004c-189">V *WebApiConfig* třída zjistíte konfiguraci přidružené webové rozhraní API, který mapuje HTTP směruje na **řadiče webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="3004c-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="3004c-190">Otevřete **soubor RouteConfig.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="3004c-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="3004c-191">Uvnitř *RegisterRoutes* metoda zjistíte konfigurace přidružené MVC, která se mapuje trasy HTTP k **řadiče MVC**.</span><span class="sxs-lookup"><span data-stu-id="3004c-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="3004c-192">Úloha 2 – spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="3004c-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="3004c-193">V této úloze se spustit generovaného řešení, prozkoumat aplikace a některé jeho funkce, jako je přepisování adres URL a integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="3004c-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="3004c-194">Chcete-li spustit řešení, stiskněte **F5** nebo klikněte na tlačítko **spustit** tlačítko nachází na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="3004c-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="3004c-195">Domovskou stránku aplikace by měla otevřít v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3004c-195">The application home page should open in the browser.</span></span>

    ![Spuštění řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="3004c-197">Ověřte, že jsou volané stránky webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="3004c-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="3004c-198">Chcete-li to provést, připojte **/contact.aspx** adresy URL v adresním řádku a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3004c-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Přátelské adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="3004c-200">*Přátelské adresy URL*</span><span class="sxs-lookup"><span data-stu-id="3004c-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-201">Jak vidíte, se změní adresa URL pro **/obraťte se na**.</span><span class="sxs-lookup"><span data-stu-id="3004c-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="3004c-202">Od **ASP.NET 4**, možnosti směrování URL byly přidány do webových formulářů, takže můžete napsat adresy URL jako *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* místo  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="3004c-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="3004c-203">Další informace najdete v části [směrování adres URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="3004c-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="3004c-204">Nyní zaměříte tok ověřování integrovaná do aplikace.</span><span class="sxs-lookup"><span data-stu-id="3004c-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="3004c-205">Chcete-li to provést, klikněte na tlačítko **zaregistrovat** v pravém horním rohu stránky.</span><span class="sxs-lookup"><span data-stu-id="3004c-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrace nového uživatele](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="3004c-207">*Registrace nového uživatele*</span><span class="sxs-lookup"><span data-stu-id="3004c-207">*Registering a new user*</span></span>
4. <span data-ttu-id="3004c-208">V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="3004c-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Zaregistrovat stránky](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="3004c-210">*Zaregistrovat stránky*</span><span class="sxs-lookup"><span data-stu-id="3004c-210">*Register page*</span></span>
5. <span data-ttu-id="3004c-211">Aplikace zaregistruje nový účet a ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="3004c-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Uživatel ověřený](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="3004c-213">*Uživatel ověřený*</span><span class="sxs-lookup"><span data-stu-id="3004c-213">*User authenticated*</span></span>
6. <span data-ttu-id="3004c-214">Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="3004c-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="3004c-215">Cvičení 2: Vytvoření řadiče MVC pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3004c-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="3004c-216">V tomto cvičení bude využít výhod rozhraní ASP.NET generování uživatelského rozhraní poskytované sadě Visual Studio k vytvoření řadiče ASP.NET MVC 5 s akcemi a zobrazeními Razor k provádění operací CRUD, bez nutnosti napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="3004c-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="3004c-217">Proces generování uživatelského rozhraní použije Entity Framework Code First pro generování kontextu dat a schéma databáze v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="3004c-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="3004c-218">**O rozhraní Entity Framework Code nejprve**</span><span class="sxs-lookup"><span data-stu-id="3004c-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="3004c-219">Entity Framework (EF) je objekt relační mapper (ORM), která umožňuje vytvořit programování s modelem koncepční aplikace místo programování přímo pomocí schématu relační úložiště dat přístup k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="3004c-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="3004c-220">Entity Framework Code First pracovního postupu modelování vám umožní používat vlastní třídy domény představující model, který využívá EF při provádění dotazu, funkce sledování změn a aktualizací.</span><span class="sxs-lookup"><span data-stu-id="3004c-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="3004c-221">Pomocí Code First pracovní postup vývoje, není nutné zahájíte aplikace vytvoření databáze nebo zadáním schéma.</span><span class="sxs-lookup"><span data-stu-id="3004c-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="3004c-222">Místo toho můžete napsat standardní tříd rozhraní .NET, které definují nejvhodnější objekty modelu domény pro vaši aplikaci a Entity Framework pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="3004c-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="3004c-223">Další informace o rozhraní Entity Framework [zde](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="3004c-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="3004c-224">Úloha 1 – Vytvoření nového modelu</span><span class="sxs-lookup"><span data-stu-id="3004c-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="3004c-225">Nyní nadefinujete **osoba** třídy, který bude používán procesem generování uživatelského rozhraní pro vytvoření řadiče MVC a zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="3004c-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="3004c-226">Začněte vytvořením **osoba** třídy modelu a operace CRUD v kontroleru se automaticky vytvoří pomocí funkce generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3004c-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="3004c-227">Otevřete **Visual Studio Express 2013 pro Web** a **MyHybridSite.sln** řešení umístěný v **zdroj/Ex2-MvcScaffolding/Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="3004c-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="3004c-228">Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="3004c-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="3004c-229">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **modely** složky **MyHybridSite** projektu a vyberte **přidat | Třída...** .</span><span class="sxs-lookup"><span data-stu-id="3004c-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Přidání třídy modelu osoba](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="3004c-231">*Přidání třídy modelu osoba*</span><span class="sxs-lookup"><span data-stu-id="3004c-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="3004c-232">V **přidat novou položku** dialogové okno, název souboru *Person.cs* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3004c-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Vytvoření třídy modelu osoba](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="3004c-234">*Vytvoření třídy modelu osoba*</span><span class="sxs-lookup"><span data-stu-id="3004c-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="3004c-235">Nahradí obsah **Person.cs** soubor s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="3004c-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="3004c-236">Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="3004c-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="3004c-237">(Code fragment kódu - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="3004c-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="3004c-238">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **MyHybridSite** projektu a vyberte **sestavení**, nebo stiskněte klávesu **CTRL + SHIFT + B** a tím projekt sestavit.</span><span class="sxs-lookup"><span data-stu-id="3004c-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="3004c-239">Úloha 2 – Vytvoření řadič MVC</span><span class="sxs-lookup"><span data-stu-id="3004c-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="3004c-240">Teď, když **osoba** model je vytvořený, použijete generování uživatelského rozhraní ASP.NET MVC rozhraní Entity Framework pro vytvoření řadiče akcemi CRUD a zobrazení pro **osoba**.</span><span class="sxs-lookup"><span data-stu-id="3004c-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="3004c-241">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nové vygenerované položky...** .</span><span class="sxs-lookup"><span data-stu-id="3004c-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Vytvoření nového vygenerované řadiče](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="3004c-243">*Vytvoření nového řadiče generované uživatelské rozhraní*</span><span class="sxs-lookup"><span data-stu-id="3004c-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="3004c-244">V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** a pak klikněte na tlačítko **přidat.**</span><span class="sxs-lookup"><span data-stu-id="3004c-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Výběr kontroler MVC 5 s zobrazení a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="3004c-246">*Výběr kontroler MVC 5 s zobrazení a Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="3004c-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="3004c-247">Nastavit *MvcPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  jako **třída modelu**.</span><span class="sxs-lookup"><span data-stu-id="3004c-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Přidání kontroler MVC pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="3004c-249">*Přidání kontroler MVC pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="3004c-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="3004c-250">V části **třída kontextu dat**, klikněte na tlačítko **nový kontext data...** .</span><span class="sxs-lookup"><span data-stu-id="3004c-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Vytvoření nové kontextu dat](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="3004c-252">*Vytvoření nové kontextu dat*</span><span class="sxs-lookup"><span data-stu-id="3004c-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="3004c-253">V **nový kontext dat** dialogové okno, název nový kontext dat *PersonContext* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3004c-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Vytvoření nové PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="3004c-255">*Vytvoření nového typu PersonContext*</span><span class="sxs-lookup"><span data-stu-id="3004c-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="3004c-256">Klikněte na tlačítko **přidat** k vytvoření nového řadiče pro **osoba** pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3004c-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="3004c-257">Visual Studio pak vygeneruje akce kontroleru, kontextu dat osoby a zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3004c-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Po vytvoření řadiče MVC pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="3004c-259">*Po vytvoření řadiče MVC pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="3004c-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="3004c-260">Otevřete **MvcPersonController.cs** v soubor **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="3004c-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="3004c-261">Všimněte si, že byly automaticky generovány metody akce CRUD.</span><span class="sxs-lookup"><span data-stu-id="3004c-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="3004c-262">Výběrem **použít asynchronní akce kontroleru** možnosti zaškrtnutí políčka generování uživatelského rozhraní v předchozích krocích, Visual Studio vygeneruje metody asynchronní akce pro všechny akce, které se týkají přístupu k kontextu dat osoby.</span><span class="sxs-lookup"><span data-stu-id="3004c-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="3004c-263">Doporučujeme, použít asynchronní akce metody pro dlouhodobé, bez procesoru vázaný požadavky k zabránění blokování webový server z provede práci během zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="3004c-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="3004c-264">Úloha 3 – spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="3004c-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="3004c-265">V této úloze budete spouštět řešení znovu a ověřte, zda zobrazení pro **osoba** fungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="3004c-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="3004c-266">Přidejte nové osobě, chcete-li ověřit, že je úspěšně uložen do databáze.</span><span class="sxs-lookup"><span data-stu-id="3004c-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="3004c-267">Stiskněte klávesu **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="3004c-268">Přejděte na **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="3004c-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="3004c-269">Vygenerované zobrazení, který zobrazuje seznam lidí, kteří by se zobrazit.</span><span class="sxs-lookup"><span data-stu-id="3004c-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="3004c-270">Klikněte na tlačítko **vytvořit nový** chcete přidat nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="3004c-270">Click **Create New** to add a new person.</span></span>

    ![Přejdete na vygenerované zobrazení MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="3004c-272">*Přejdete na vygenerované zobrazení MVC*</span><span class="sxs-lookup"><span data-stu-id="3004c-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="3004c-273">V **vytvořit** zobrazit, zadejte **název** a **stáří** pro osoby a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3004c-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Přidání nové osobě](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="3004c-275">*Přidání nové osobě*</span><span class="sxs-lookup"><span data-stu-id="3004c-275">*Adding a new person*</span></span>
5. <span data-ttu-id="3004c-276">Nový uživatel se přidá do seznamu.</span><span class="sxs-lookup"><span data-stu-id="3004c-276">The new person is added to the list.</span></span> <span data-ttu-id="3004c-277">Element seznamu, klikněte na **podrobnosti** zobrazit podrobnosti, osoby.</span><span class="sxs-lookup"><span data-stu-id="3004c-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="3004c-278">Potom v **podrobnosti** zobrazit, klikněte na tlačítko **zpět do seznamu** se vrátíte k zobrazení seznamu.</span><span class="sxs-lookup"><span data-stu-id="3004c-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Zobrazení podrobností osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="3004c-280">*Zobrazení podrobností osoby*</span><span class="sxs-lookup"><span data-stu-id="3004c-280">*Person's details view*</span></span>
6. <span data-ttu-id="3004c-281">Klikněte **odstranit** odkaz odstraníte osoby.</span><span class="sxs-lookup"><span data-stu-id="3004c-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="3004c-282">V **odstranit** zobrazit, klikněte na tlačítko **odstranit** k potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="3004c-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Odstraňování osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="3004c-284">*Odstraňování osoby*</span><span class="sxs-lookup"><span data-stu-id="3004c-284">*Deleting a person*</span></span>
7. <span data-ttu-id="3004c-285">Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="3004c-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="3004c-286">Cvičení 3: Vytvoření řadiče webové rozhraní API pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3004c-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="3004c-287">Rozhraní Web API je součástí zásobníku technologie ASP.NET a navržený tak, aby implementující služeb HTTP jednodušší, obecně odesílání a příjmu dat prostřednictvím rozhraní RESTful API ve formátu JSON nebo XML.</span><span class="sxs-lookup"><span data-stu-id="3004c-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="3004c-288">V tomto cvičení použijete ASP.NET generování uživatelského rozhraní znovu vygenerovat kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3004c-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="3004c-289">Budete používat stejný **osoba** a **PersonContext** třídy z předchozím cvičení zajistit stejná osoba data ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="3004c-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="3004c-290">Zobrazí se, jak můžou stejné prostředky zpřístupnit různými způsoby v rámci stejné aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3004c-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="3004c-291">Úloha 1 – Vytvoření Kontroleru webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3004c-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="3004c-292">V této úloze se vytvoří nové **kontroler Web API** který zveřejní osoba data ve formátu počítač consumable jako JSON.</span><span class="sxs-lookup"><span data-stu-id="3004c-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="3004c-293">Pokud ještě není otevřené, otevřete **Visual Studio Express 2013 pro Web** a otevřete **MyHybridSite.sln** řešení umístěný v **zdroj/EX3.-WebAPI/Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="3004c-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="3004c-294">Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="3004c-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-295">Pokud spustíte s řešením Begin z cvičení 3, stiskněte klávesu **CTRL + SHIFT + B** sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="3004c-296">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nové vygenerované položky...** .</span><span class="sxs-lookup"><span data-stu-id="3004c-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Vytvoření nového vygenerované řadiče](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="3004c-298">*Vytvoření nového vygenerované řadiče*</span><span class="sxs-lookup"><span data-stu-id="3004c-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="3004c-299">V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **webového rozhraní API** v levém podokně, pak **webové 2 kontroler API s akcemi používající rozhraní Entity Framework** v prostředním podokně a pak klikněte na tlačítko  **Přidáte.**</span><span class="sxs-lookup"><span data-stu-id="3004c-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="3004c-300">![Výběr kontroler Web API 2 s akcemi a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "výběr kontroler Web API s akcemi a Entity Framework 2")</span><span class="sxs-lookup"><span data-stu-id="3004c-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="3004c-301">*Výběr kontroler Web API 2 s akcemi a Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="3004c-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="3004c-302">Nastavit *ApiPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  a **PersonContext (MyHybridSite.Models)** jako **modelu** a **kontextu dat** třídy v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3004c-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="3004c-303">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3004c-303">Then click **Add**.</span></span>

    <span data-ttu-id="3004c-304">![Přidání kontroler Web API s generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="3004c-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="3004c-305">*Přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="3004c-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="3004c-306">Visual Studio pak vygeneruje **ApiPersonController** třídy s čtyři akcemi CRUD pro práci s daty.</span><span class="sxs-lookup"><span data-stu-id="3004c-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="3004c-307">![Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="3004c-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="3004c-308">*Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="3004c-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="3004c-309">Otevřete **ApiPersonController.cs** souboru a zkontrolovat *GetPeople* metody akce.</span><span class="sxs-lookup"><span data-stu-id="3004c-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="3004c-310">Tato metoda dotazuje pole db **PersonContext** typu, aby bylo možné získat data uživatele.</span><span class="sxs-lookup"><span data-stu-id="3004c-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="3004c-311">Nyní Všimněte si komentář výše definici metody.</span><span class="sxs-lookup"><span data-stu-id="3004c-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="3004c-312">Poskytuje identifikátor URI, který zveřejňuje tuto akci, kterou chcete používat v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="3004c-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="3004c-313">Ve výchozím nastavení, je nakonfigurované catch dotazů do rozhraní API webové */api* cestu k zabrání se tím kolizím s kontrolery MVC.</span><span class="sxs-lookup"><span data-stu-id="3004c-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="3004c-314">Pokud potřebujete změnit toto nastavení, podívejte se na [směrování v rozhraní ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3004c-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="3004c-315">Úloha 2 – spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="3004c-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="3004c-316">V této úloze budete používat v Internet Exploreru **nástroje pro vývojáře F12** kontrola úplnou odpověď z kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3004c-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="3004c-317">Zobrazí se, jak můžete zaznamenávat provoz sítě, chcete-li získat další vhled do vašich dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="3004c-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="3004c-318">Ujistěte se, že **Internet Explorer** vybrán **spustit** tlačítko nachází na panelu nástrojů Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3004c-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Možnost aplikace Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="3004c-320">**Nástroje pro vývojáře F12** mají celou sadu funkcí, které nejsou zahrnuty v tomto hands-na-testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3004c-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="3004c-321">Pokud chcete další informace o tom, podívejte se na [používání nástrojů pro vývojáře F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="3004c-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="3004c-322">Stiskněte klávesu **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-323">Chcete-li provést tuto úlohu správně, musí mít data aplikace.</span><span class="sxs-lookup"><span data-stu-id="3004c-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="3004c-324">Pokud vaše databáze je prázdná, můžete přejít zpět k úloze 3 v cvičení 2 a postupujte podle kroků pro vytvoření nového uživatele, kteří používají zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="3004c-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="3004c-325">V prohlížeči stiskněte **F12** otevřete **Developer Tools** panelu.</span><span class="sxs-lookup"><span data-stu-id="3004c-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="3004c-326">Stiskněte klávesu **CTRL** + **4** nebo klikněte na tlačítko **sítě** ikonu a pak klikněte na tlačítko se zelenou šipkou zahájíte zachytávání síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="3004c-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="3004c-327">![Probíhá inicializace zachycení dat ze sítě webového rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "zachycení dat ze sítě inicializaci webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="3004c-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="3004c-328">*Inicializace webového rozhraní API zachycení dat ze sítě*</span><span class="sxs-lookup"><span data-stu-id="3004c-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="3004c-329">Připojit **rozhraní api nebo ApiPerson** na adresu URL v adresním řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3004c-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="3004c-330">Nyní bude kontrolovat podrobnosti o odpověď **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="3004c-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="3004c-331">![Načítání dat osoba prostřednictvím webového rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "načítání dat osoba prostřednictvím webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="3004c-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="3004c-332">*Načítání dat osoba prostřednictvím webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="3004c-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-333">Po dokončení stahování se výzva, aby akce s stažený soubor.</span><span class="sxs-lookup"><span data-stu-id="3004c-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="3004c-334">Ponechte dialogové okno otevřené, aby bylo možné sledovat obsahu odpovědi prostřednictvím okno nástroje pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="3004c-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="3004c-335">Nyní bude kontrolovat text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3004c-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="3004c-336">Chcete-li to provést, klikněte na tlačítko **podrobnosti** a pak klikněte **odpovědi**.</span><span class="sxs-lookup"><span data-stu-id="3004c-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="3004c-337">Můžete zkontrolovat, že stažená data jsou seznam objektů s vlastnostmi **Id**, **název** a **stáří** , odpovídají **osoba** Třída.</span><span class="sxs-lookup"><span data-stu-id="3004c-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="3004c-338">![Zobrazení webové rozhraní API odpovědi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "zobrazení webové rozhraní API odpovědi")</span><span class="sxs-lookup"><span data-stu-id="3004c-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="3004c-339">*Zobrazení webové rozhraní API odpovědi*</span><span class="sxs-lookup"><span data-stu-id="3004c-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="3004c-340">Úloha 3 – Přidání webové stránky nápovědy rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3004c-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="3004c-341">Při vytváření webového rozhraní API, je vhodné vytvořit stránku nápovědy, aby ostatní vývojáři věděli, jak volat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3004c-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="3004c-342">Můžete vytvářet a aktualizovat stránky dokumentace k ručně, ale je lepší pro automatické generování je, abyste nemuseli dělat údržbu práci.</span><span class="sxs-lookup"><span data-stu-id="3004c-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="3004c-343">V této úloze budete používat balíčku Nuget k automatickému generování stránky nápovědy webového rozhraní API k řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="3004c-344">Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="3004c-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="3004c-345">V **Konzola správce balíčků** okno, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3004c-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="3004c-346">**Microsoft.AspNet.WebApi.HelpPage** balíček nainstaluje potřebné sestavení a přidá zobrazení MVC pro stránky nápovědy v části **oblasti nebo HelpPage** složky.</span><span class="sxs-lookup"><span data-stu-id="3004c-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="3004c-347">![Oblasti HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage oblasti")</span><span class="sxs-lookup"><span data-stu-id="3004c-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="3004c-348">*HelpPage oblasti*</span><span class="sxs-lookup"><span data-stu-id="3004c-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="3004c-349">Ve výchozím nastavení v nápovědě stránky máte řetězce zástupný symbol pro dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3004c-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="3004c-350">Dokumentační komentáře XML slouží k vytvoření v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3004c-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="3004c-351">Chcete-li povolit tuto funkci, otevřete **HelpPageConfig.cs** soubor umístěný ve **oblasti nebo HelpPage nebo aplikace\_spustit** složky a zrušte komentář u následujícího řádku:</span><span class="sxs-lookup"><span data-stu-id="3004c-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="3004c-352">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt **MyHybridSite**, vyberte **vlastnosti** a klikněte na tlačítko **sestavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="3004c-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="3004c-353">![Sestavení karta](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "sestavení oddílu")</span><span class="sxs-lookup"><span data-stu-id="3004c-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="3004c-354">*Karta sestavení*</span><span class="sxs-lookup"><span data-stu-id="3004c-354">*Build tab*</span></span>
5. <span data-ttu-id="3004c-355">V části **výstup**, vyberte **souborů dokumentace XML**.</span><span class="sxs-lookup"><span data-stu-id="3004c-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="3004c-356">V dialogovém okně Upravit typ **aplikace\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="3004c-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="3004c-357">![Výstup části na kartě sestavení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "výstup části na kartě sestavení")</span><span class="sxs-lookup"><span data-stu-id="3004c-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="3004c-358">*Výstup části na kartě sestavení*</span><span class="sxs-lookup"><span data-stu-id="3004c-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="3004c-359">Stiskněte klávesu **CTRL** + **S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="3004c-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="3004c-360">Otevřete **ApiPersonController.cs** souboru z **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="3004c-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="3004c-361">Zadejte nový řádek mezi *GetPeople* podpis metody a */ / GET rozhraní api nebo ApiPerson* komentář a potom zadejte tři lomítka.</span><span class="sxs-lookup"><span data-stu-id="3004c-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-362">Visual Studio automaticky vloží elementy XML, které definují metoda dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="3004c-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="3004c-363">Přidat text shrnutí a návratovou hodnotu pro *GetPeople* metoda.</span><span class="sxs-lookup"><span data-stu-id="3004c-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="3004c-364">By měl vypadat jako následující.</span><span class="sxs-lookup"><span data-stu-id="3004c-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="3004c-365">Stiskněte klávesu **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="3004c-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="3004c-366">Připojit **/help** na adresu URL v adresním řádku a přejděte na stránku nápovědy.</span><span class="sxs-lookup"><span data-stu-id="3004c-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="3004c-367">![Stránka nápovědy rozhraní ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "stránka nápovědy rozhraní ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="3004c-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="3004c-368">*Stránku nápovědy rozhraní ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="3004c-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3004c-369">Hlavní obsah stránky je tabulka rozhraní API, seskupené podle řadiče.</span><span class="sxs-lookup"><span data-stu-id="3004c-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="3004c-370">Položky tabulky jsou generována dynamicky, pomocí **IApiExplorer** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3004c-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="3004c-371">Pokud přidáte nebo aktualizujete kontroler API, v tabulce budou automaticky aktualizovány při příštím sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3004c-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="3004c-372">**Rozhraní API** sloupec obsahuje metodu HTTP a relativní identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="3004c-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="3004c-373">**Popis** sloupec obsahuje informace, které jste extrahovali z dokumentace metody.</span><span class="sxs-lookup"><span data-stu-id="3004c-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="3004c-374">Všimněte si, že popis, který jste přidali výše definici metody se zobrazí ve sloupci Popis.</span><span class="sxs-lookup"><span data-stu-id="3004c-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="3004c-375">![Popis rozhraní API metoda](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "popis metody rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="3004c-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="3004c-376">*Popis rozhraní API – metoda*</span><span class="sxs-lookup"><span data-stu-id="3004c-376">*API method description*</span></span>
13. <span data-ttu-id="3004c-377">Klikněte na jednu z metod rozhraní API přejít na stránku s podrobnější informace, včetně ukázkové těla odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3004c-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="3004c-378">![Stránka informace o podrobností](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "podrobné informace o stránky")</span><span class="sxs-lookup"><span data-stu-id="3004c-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="3004c-379">*Podrobné informace stránky*</span><span class="sxs-lookup"><span data-stu-id="3004c-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3004c-380">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3004c-380">Summary</span></span>

<span data-ttu-id="3004c-381">Pomocí této praktické cvičení jste se naučili, jak:</span><span class="sxs-lookup"><span data-stu-id="3004c-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="3004c-382">Vytvoření nové webové aplikace pomocí jednoho prostředí ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3004c-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="3004c-383">Integrace více technologie ASP.NET do jednoho jednoho projektu</span><span class="sxs-lookup"><span data-stu-id="3004c-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="3004c-384">Generování řadiče MVC a zobrazení z tříd modelu pomocí ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3004c-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="3004c-385">Generovat řadičů webového rozhraní API, která používá funkce, jako jsou asynchronní programování a přístup k datům prostřednictvím rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3004c-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="3004c-386">Automatické generování stránky nápovědy webové rozhraní API pro vaše řadiče</span><span class="sxs-lookup"><span data-stu-id="3004c-386">Automatically generate Web API Help Pages for your controllers</span></span>
