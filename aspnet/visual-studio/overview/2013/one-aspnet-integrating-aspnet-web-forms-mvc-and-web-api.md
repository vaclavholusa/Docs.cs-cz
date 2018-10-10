---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Praktické cvičení: Jeden ASP.NET: integrace webových formulářů ASP.NET, MVC a webového rozhraní API | Dokumentace Microsoftu'
author: rick-anderson
description: ASP.NET je architektura určená k vytváření webů, aplikací a služeb pomocí specializované technologie, jako je MVC, webové rozhraní API a další. S rozšiřující se ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: c18911680b59448cd67190f71e951a3fcf3d0478
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912732"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="1bee1-104">Praktické cvičení: Jeden ASP.NET: integrace webových formulářů ASP.NET, MVC a webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1bee1-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="1bee1-105">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1bee1-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="1bee1-106">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="1bee1-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="1bee1-107">ASP.NET je architektura určená k vytváření webů, aplikací a služeb pomocí specializované technologie, jako je MVC, webové rozhraní API a další.</span><span class="sxs-lookup"><span data-stu-id="1bee1-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="1bee1-108">Pomocí rozšíření ASP.NET zaznamenal od jejího vytvoření a uvedeného musí mít tyto technologie integrované, aby byla poslední úsilí o naplňování **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="1bee1-109">Visual Studio 2013 přináší nový sjednocený projektový systém, který umožňuje vytvářet aplikace a použít všechny technologie ASP.NET v jednom projektu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="1bee1-110">Tato funkce eliminuje potřebu vyberte jeden technologii na začátku projektu a stonek s ním a místo toho doporučuje použití více rozhraní ASP.NET v rámci jednoho projektu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="1bee1-111">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="1bee1-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="1bee1-112">Přehled</span><span class="sxs-lookup"><span data-stu-id="1bee1-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1bee1-113">Cíle</span><span class="sxs-lookup"><span data-stu-id="1bee1-113">Objectives</span></span>

<span data-ttu-id="1bee1-114">V této praktická cvičení se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="1bee1-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="1bee1-115">Vytvoření webu na základě **One ASP.NET** typ projektu</span><span class="sxs-lookup"><span data-stu-id="1bee1-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="1bee1-116">Použití různých **ASP.NET** architektur, jako jsou **MVC** a **webového rozhraní API** ve stejném projektu</span><span class="sxs-lookup"><span data-stu-id="1bee1-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="1bee1-117">Identifikovat hlavní součásti **ASP.NET** aplikace</span><span class="sxs-lookup"><span data-stu-id="1bee1-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="1bee1-118">Využijte výhod **ASP.NET generování uživatelského rozhraní** framework pro automatické vytvoření Kontrolerů a zobrazení k provádění operací CRUD založen na třídách modelu</span><span class="sxs-lookup"><span data-stu-id="1bee1-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="1bee1-119">Vystavení stejnou sadu informace v počítači – a lidsky čitelném formátu nejvhodnější nástroj pro každou úlohu</span><span class="sxs-lookup"><span data-stu-id="1bee1-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1bee1-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1bee1-120">Prerequisites</span></span>

<span data-ttu-id="1bee1-121">K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:</span><span class="sxs-lookup"><span data-stu-id="1bee1-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="1bee1-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="1bee1-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="1bee1-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="1bee1-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1bee1-124">Instalace</span><span class="sxs-lookup"><span data-stu-id="1bee1-124">Setup</span></span>

<span data-ttu-id="1bee1-125">Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.</span><span class="sxs-lookup"><span data-stu-id="1bee1-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="1bee1-126">Otevřete Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="1bee1-127">Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a nainstalujte Visual Studio fragmenty kódu pro toto testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="1bee1-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="1bee1-128">Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="1bee1-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="1bee1-129">Ujistěte se, že jste zaškrtli všechny závislosti pro toto testovací prostředí před spuštěním instalace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="1bee1-130">Používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="1bee1-130">Using the Code Snippets</span></span>

<span data-ttu-id="1bee1-131">V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="1bee1-132">Pro usnadnění práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který se dá dostat z v rámci Visual Studio 2013, abyste ho nemuseli znovu přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="1bee1-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="1bee1-133">Každý cvičení se sadou počáteční řešení nachází v **začít** složky výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="1bee1-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="1bee1-134">Uvědomte si, že chybí z těchto řešení od fragmenty kódu, které se přidávají během cvičení a nemusí fungovat, dokud nedokončíte výkonu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="1bee1-135">Uvnitř zdrojový kód pro cvičení, můžete také najdete **End** složku, která obsahuje řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="1bee1-136">Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1bee1-137">Cvičení</span><span class="sxs-lookup"><span data-stu-id="1bee1-137">Exercises</span></span>

<span data-ttu-id="1bee1-138">Toto praktické testovací prostředí obsahuje následující praktická cvičení:</span><span class="sxs-lookup"><span data-stu-id="1bee1-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="1bee1-139">Vytváří se nový projekt webových formulářů</span><span class="sxs-lookup"><span data-stu-id="1bee1-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="1bee1-140">Vytvoření Kontroleru MVC pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1bee1-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="1bee1-141">Vytvoření Kontroleru webového rozhraní API používat generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1bee1-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="1bee1-142">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="1bee1-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="1bee1-143">Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce.</span><span class="sxs-lookup"><span data-stu-id="1bee1-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="1bee1-144">Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1bee1-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="1bee1-145">Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce.</span><span class="sxs-lookup"><span data-stu-id="1bee1-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="1bee1-146">Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="1bee1-147">Cvičení 1: Vytvoření nového projektu webových formulářů</span><span class="sxs-lookup"><span data-stu-id="1bee1-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="1bee1-148">V tomto cvičení vytvoříte novou lokalitu webové formuláře v sadě Visual Studio 2013 pomocí **One ASP.NET** jednotné rozhraní projektu, který vám umožní snadno integrovat komponenty webové formuláře, MVC a webového rozhraní API ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="1bee1-149">Potom seznámení s řešením generované a určit její části, a nakonec se zobrazí na webu v akci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="1bee1-150">Úloha 1 – vytváření nového webu pomocí jedné prostředí ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1bee1-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="1bee1-151">V této úloze se spustí vytváření nového webu v sadě Visual Studio na základě **One ASP.NET** typ projektu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="1bee1-152">**One ASP.NET** sjednocuje všechny technologie ASP.NET a dává vám možnost kombinovat a párovat podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1bee1-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="1bee1-153">Pak poznáte různých komponent nástroje webové formuláře, MVC a webového rozhraní API, kteří žijí vedle sebe v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="1bee1-154">Otevřít **Visual Studio Express 2013 for Web** a vyberte **soubor | Nový projekt...**  spusťte nové řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Vytvoření nového projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="1bee1-156">*Vytvoření nového projektu*</span><span class="sxs-lookup"><span data-stu-id="1bee1-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="1bee1-157">V **nový projekt** dialogu **webová aplikace ASP.NET** pod **Visual C# | Web** kartu a ujistěte se, že **rozhraní .NET Framework 4.5** zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="1bee1-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="1bee1-158">Pojmenujte projekt *MyHybridSite*, zvolte **umístění** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nový projekt webové aplikace ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="1bee1-160">*Vytvoření nového projektu webové aplikace ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="1bee1-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="1bee1-161">V **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony a vyberte **MVC** a **webového rozhraní API** možnosti.</span><span class="sxs-lookup"><span data-stu-id="1bee1-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="1bee1-162">Také, ujistěte se, že **ověřování** je možnost nastavená na **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="1bee1-163">Klikněte na tlačítko **OK** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="1bee1-163">Click **OK** to continue.</span></span>

    ![Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="1bee1-165">*Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC*</span><span class="sxs-lookup"><span data-stu-id="1bee1-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="1bee1-166">Teď si můžete projít strukturu generované řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-166">You can now explore the structure of the generated solution.</span></span>

    ![Prozkoumání vygenerované řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="1bee1-168">*Prozkoumání vygenerované řešení*</span><span class="sxs-lookup"><span data-stu-id="1bee1-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="1bee1-169">**Účet:** tato složka obsahuje webový formulář stránky, které chcete zaregistrovat, přihlaste se k a spravovat účty pro uživatele vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="1bee1-170">Tato složka je vytvořena při **jednotlivé uživatelské účty** během konfigurace šablony projektu webových formulářů je vybraná možnost ověřování.</span><span class="sxs-lookup"><span data-stu-id="1bee1-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="1bee1-171">**Modely:** tato složka bude obsahovat třídy, které představují data vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="1bee1-172">**Řadiče** a **zobrazení**: tyto složky jsou vyžadována pro **ASP.NET MVC** a **rozhraní ASP.NET Web API** komponenty.</span><span class="sxs-lookup"><span data-stu-id="1bee1-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="1bee1-173">Prozkoumáte technologie MVC a webového rozhraní API v dalším cvičení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="1bee1-174">**Default.aspx**, **Contact.aspx** a **About.aspx** soubory jsou předem definovaných stránky webového formuláře, které můžete použít jako odrazový můstek k vytváření stránek, které jsou specifické pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="1bee1-175">Programovou logiku těchto souborů se nachází v samostatném souboru, který se označuje jako &quot;použití modelu code-behind&quot; souboru, který má &quot;. aspx&quot; nebo &quot;. aspx.cs&quot; rozšíření (v závislosti na tom jazyk používaný).</span><span class="sxs-lookup"><span data-stu-id="1bee1-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="1bee1-176">Použití modelu code-behind logiku běží na serveru a dynamicky vytvoří výstup ve formátu HTML stránky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="1bee1-177">**Site.Master** a **Site.Mobile.Master** stránky definují vzhled a chování a standardní chování všechny stránky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="1bee1-178">Dvakrát klikněte **Default.aspx** souboru zkoumání obsahu stránky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Zkoumání stránku Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="1bee1-180">*Zkoumání stránku Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="1bee1-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-181">**Stránky** – direktiva v horní části souboru definuje atributy stránky webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="1bee1-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="1bee1-182">Například **MasterPageFile** atribut určuje cestu k hlavní stránce – v takovém případě *Site.Master* stránky – a **Inherits** definuje atribut použití modelu Code-behind třída stránky zdědí.</span><span class="sxs-lookup"><span data-stu-id="1bee1-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="1bee1-183">Tato třída je umístěn v souboru dáno **CodeBehind** atribut.</span><span class="sxs-lookup"><span data-stu-id="1bee1-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="1bee1-184">**Asp: Content** ovládací prvek obsahuje skutečný obsah na stránce (text, značky a ovládací prvky) a je namapovaná na **asp: ContentPlaceHolder** ovládacího prvku na hlavní stránku.</span><span class="sxs-lookup"><span data-stu-id="1bee1-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="1bee1-185">V takovém případě obsah stránky bude možné vykreslit v rámci *MainContent* ovládací prvek definovaný v *Site.Master* stránky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="1bee1-186">Rozbalte **aplikace\_Start** složky a Všimněte si, že **WebApiConfig.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="1bee1-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="1bee1-187">Visual Studio zahrnout generované řešení tento soubor, protože jste zahrnuli webového rozhraní API, pokud konfigurace vašeho projektu pomocí šablony One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1bee1-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="1bee1-188">Otevřít **WebApiConfig.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="1bee1-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="1bee1-189">V *WebApiConfig* třídy zjistíte konfigurace související s webovým rozhraním API, který mapuje HTTP směruje do **kontrolerů rozhraní Web API**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="1bee1-190">Otevřít **RouteConfig.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="1bee1-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="1bee1-191">Uvnitř *RegisterRoutes* metoda najdete konfigurace související s MVC, který mapuje trasy protokolu HTTP a **kontrolery MVC**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="1bee1-192">Úloha 2 – spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="1bee1-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="1bee1-193">V této úloze se spustit generované řešení, prozkoumejte aplikace a některé z jejích funkcí, jako jsou přepisování adres URL a integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="1bee1-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="1bee1-194">Chcete-li spustit řešení, stiskněte **F5** nebo klikněte na tlačítko **Start** tlačítka umístěny na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="1bee1-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="1bee1-195">Domovská stránka aplikace by měla otevřít v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1bee1-195">The application home page should open in the browser.</span></span>

    ![Spuštění řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="1bee1-197">Ověřte, že jsou vyvolání stránky webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="1bee1-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="1bee1-198">Chcete-li to provést, přidejte **/contact.aspx** adresy URL v adresním řádku a stisknutím klávesy **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Přátelské adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="1bee1-200">*Přátelské adresy URL*</span><span class="sxs-lookup"><span data-stu-id="1bee1-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-201">Jak je vidět, se změní adresa URL k **/kontaktovat**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="1bee1-202">Počínaje **ASP.NET 4**, adresy URL směrování funkce byly přidány do webových formulářů, takže můžete psát, jako jsou adresy URL *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* místo  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="1bee1-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="1bee1-203">Další informace najdete v [směrování adres URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="1bee1-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="1bee1-204">Tok ověření integrované do aplikace se teď si projděte.</span><span class="sxs-lookup"><span data-stu-id="1bee1-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="1bee1-205">Chcete-li to provést, klikněte na tlačítko **zaregistrovat** v pravém horním rohu stránky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrace nového uživatele](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="1bee1-207">*Registrace nového uživatele*</span><span class="sxs-lookup"><span data-stu-id="1bee1-207">*Registering a new user*</span></span>
4. <span data-ttu-id="1bee1-208">V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Stránka pro registraci](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="1bee1-210">*Stránka pro registraci*</span><span class="sxs-lookup"><span data-stu-id="1bee1-210">*Register page*</span></span>
5. <span data-ttu-id="1bee1-211">Aplikace registruje nový účet a uživatel je ověřen.</span><span class="sxs-lookup"><span data-stu-id="1bee1-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Uživatel byl ověřen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="1bee1-213">*Uživatel byl ověřen*</span><span class="sxs-lookup"><span data-stu-id="1bee1-213">*User authenticated*</span></span>
6. <span data-ttu-id="1bee1-214">Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="1bee1-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="1bee1-215">Cvičení 2: Vytvoření Kontroleru MVC pomocí generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1bee1-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="1bee1-216">V tomto cvičení bude využívat technologie ASP.NET generování uživatelského rozhraní framework poskytovaný sadou Visual Studio k vytvoření kontroler ASP.NET MVC 5 s akcemi a zobrazeními Razor k provádění operací CRUD, aniž byste museli napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="1bee1-217">Proces generování uživatelského rozhraní pomocí platformy Entity Framework Code First vygenerovat kontext dat a schéma databáze ve službě SQL database.</span><span class="sxs-lookup"><span data-stu-id="1bee1-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="1bee1-218">**O rozhraní Entity Framework Code nejprve**</span><span class="sxs-lookup"><span data-stu-id="1bee1-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="1bee1-219">Entity Framework (EF) je objektově relační Mapovač (ORM), který vám umožní vytvářet aplikace přistupující k datům v programování s modelem koncepční aplikace namísto programování přímo pomocí schématu relační úložiště.</span><span class="sxs-lookup"><span data-stu-id="1bee1-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="1bee1-220">Entity Framework Code First pracovního postupu modelování vám umožní používat vlastní třídy domény k vyjádření modelu EF spoléhá na při provádění dotazu, funkce sledování změn a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="1bee1-221">Pomocí pracovního postupu vývoje Code First, není potřeba začněte tím, že vytvoříte databázi nebo zadání schéma vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="1bee1-222">Místo toho můžete psát standardní třídy .NET, které definují nejvhodnější objekty modelu domény pro vaši aplikaci a Entity Framework pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="1bee1-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="1bee1-223">Další informace o rozhraní Entity Framework [tady](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="1bee1-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="1bee1-224">Úloha 1 – Vytvoření nového modelu</span><span class="sxs-lookup"><span data-stu-id="1bee1-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="1bee1-225">Nyní budou definovat **osoba** třídu, která se používá procesem generování uživatelského rozhraní pro vytvoření kontroleru MVC a zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="1bee1-226">Začne tím, že vytvoříte **osoba** třídy modelu a operace CRUD v kontroleru se automaticky vytvoří pomocí funkce generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1bee1-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="1bee1-227">Otevřít **Visual Studio Express 2013 for Web** a **MyHybridSite.sln** řešení nachází v **zdroj/Ex2-MvcScaffolding/Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="1bee1-228">Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="1bee1-229">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **modely** složky **MyHybridSite** projektu a vyberte **přidat | Třída...** .</span><span class="sxs-lookup"><span data-stu-id="1bee1-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Přidání třídy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="1bee1-231">*Přidání třídy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="1bee1-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="1bee1-232">V **přidat novou položku** dialogové okno, pojmenujte soubor *Person.cs* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Vytvoření třídy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="1bee1-234">*Vytvoření třídy modelu osoby*</span><span class="sxs-lookup"><span data-stu-id="1bee1-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="1bee1-235">Nahradí obsah **Person.cs** souboru následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="1bee1-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="1bee1-236">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="1bee1-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="1bee1-237">(Fragment - kódu *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="1bee1-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="1bee1-238">V **Průzkumníka řešení**, klikněte pravým tlačítkem na **MyHybridSite** projektu a vyberte **sestavení**, nebo stiskněte klávesu **CTRL + SHIFT + B** k sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="1bee1-239">Úloha 2 – Vytvoření Kontroleru MVC</span><span class="sxs-lookup"><span data-stu-id="1bee1-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="1bee1-240">Teď, když **osoba** model je vytvořený, budete používat generování uživatelského rozhraní ASP.NET MVC s Entity Framework pro vytvoření akce kontroleru CRUD a zobrazení pro **osoba**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="1bee1-241">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nová vygenerovaná položka...** .</span><span class="sxs-lookup"><span data-stu-id="1bee1-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Vytvoření nové vygenerované Kontroleru](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="1bee1-243">*Vytvoření nové automaticky generovaný Kontroleru*</span><span class="sxs-lookup"><span data-stu-id="1bee1-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="1bee1-244">V **přidat vygenerované uživatelské rozhraní** dialogu **kontroler MVC 5 se zobrazeními, používá nástroj Entity Framework** a potom klikněte na tlačítko **přidat.**</span><span class="sxs-lookup"><span data-stu-id="1bee1-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Vyberte kontroler MVC 5 se zobrazeními a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="1bee1-246">*Vyberte kontroler MVC 5 se zobrazeními a Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="1bee1-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="1bee1-247">Nastavte *MvcPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  jako **třída modelu**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Přidat kontroler MVC s generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="1bee1-249">*Přidat kontroler MVC s generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="1bee1-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="1bee1-250">V části **třída kontextu dat**, klikněte na tlačítko **nový kontext dat...** .</span><span class="sxs-lookup"><span data-stu-id="1bee1-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Vytváří se nový kontext dat.](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="1bee1-252">*Vytváří se nový kontext dat.*</span><span class="sxs-lookup"><span data-stu-id="1bee1-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="1bee1-253">V **nový kontext dat.** dialogové okno, název nový kontext dat *PersonContext* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Vytváří se nový PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="1bee1-255">*Vytváří se nový typ PersonContext*</span><span class="sxs-lookup"><span data-stu-id="1bee1-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="1bee1-256">Klikněte na tlačítko **přidat** k vytvoření nového řadiče pro **osoba** pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1bee1-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="1bee1-257">Visual Studio pak vygeneruje akce kontroleru, kontext dat osoby a zobrazeními Razor.</span><span class="sxs-lookup"><span data-stu-id="1bee1-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Po vytvoření kontroleru MVC pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="1bee1-259">*Po vytvoření kontroleru MVC pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="1bee1-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="1bee1-260">Otevřít **MvcPersonController.cs** soubor **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="1bee1-261">Všimněte si, že metody akcí CRUD byly vytvořeny automaticky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="1bee1-262">Výběrem **použít asynchronní akce kontroleru** zaškrtnutí políčka základní kostry aplikace možnosti v předchozích krocích, sada Visual Studio generuje asynchronní akce metody pro všechny akce, které zahrnují přístup ke kontextu dat osoby.</span><span class="sxs-lookup"><span data-stu-id="1bee1-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="1bee1-263">Doporučuje se, že použití metod asynchronní akce pro dlouho běžící, procesor vázán požadavky k zabránění blokování webový server z provádění práce během zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="1bee1-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="1bee1-264">Úloha 3 – spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="1bee1-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="1bee1-265">V této úloze budete spouštět řešení znovu ověřte, že zobrazení pro **osoba** fungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="1bee1-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="1bee1-266">Přidejte nové osobě, chcete-li ověřit, že je úspěšně uložen do databáze.</span><span class="sxs-lookup"><span data-stu-id="1bee1-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="1bee1-267">Stisknutím klávesy **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="1bee1-268">Přejděte do **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="1bee1-269">Vygenerovaná zobrazení, který zobrazuje seznam lidí, kteří by se zobrazit.</span><span class="sxs-lookup"><span data-stu-id="1bee1-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="1bee1-270">Klikněte na tlačítko **vytvořit nový** chcete přidat nové uživatele.</span><span class="sxs-lookup"><span data-stu-id="1bee1-270">Click **Create New** to add a new person.</span></span>

    ![Přejděte na automaticky generovaný zobrazení MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="1bee1-272">*Přejděte na automaticky generovaný zobrazení MVC*</span><span class="sxs-lookup"><span data-stu-id="1bee1-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="1bee1-273">V **vytvořit** zobrazení, zadejte **název** a **stáří** osoba, a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Přidání nové osobě](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="1bee1-275">*Přidání nové osobě*</span><span class="sxs-lookup"><span data-stu-id="1bee1-275">*Adding a new person*</span></span>
5. <span data-ttu-id="1bee1-276">Nový uživatel se přidá do seznamu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-276">The new person is added to the list.</span></span> <span data-ttu-id="1bee1-277">V seznamu prvků, klikněte na tlačítko **podrobnosti** k zobrazení podrobností osoby.</span><span class="sxs-lookup"><span data-stu-id="1bee1-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="1bee1-278">Potom v **podrobnosti** zobrazení, klikněte na tlačítko **zpět do seznamu** chcete přejít zpátky k zobrazení seznamu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Zobrazení podrobností o osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="1bee1-280">*Zobrazení podrobností o osoby*</span><span class="sxs-lookup"><span data-stu-id="1bee1-280">*Person's details view*</span></span>
6. <span data-ttu-id="1bee1-281">Klikněte na tlačítko **odstranit** odkaz se odstranit uživatele.</span><span class="sxs-lookup"><span data-stu-id="1bee1-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="1bee1-282">V **odstranit** zobrazení, klikněte na tlačítko **odstranit** potvrďte operaci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Odstraňuje se uživatel](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="1bee1-284">*Odstraňuje se uživatel*</span><span class="sxs-lookup"><span data-stu-id="1bee1-284">*Deleting a person*</span></span>
7. <span data-ttu-id="1bee1-285">Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="1bee1-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="1bee1-286">Cvičení 3: Vytvoření Kontroleru webového rozhraní API používat generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1bee1-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="1bee1-287">Rozhraní Web API je součástí technologie ASP.NET zásobníku a navržená tak, aby implementace služby HTTP snadnější, obecná odesílání a přijímání dat ve formátu JSON nebo XML pomocí rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="1bee1-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="1bee1-288">V tomto cvičení použijete ASP.NET generování uživatelského rozhraní znovu generovat kontroler Web API.</span><span class="sxs-lookup"><span data-stu-id="1bee1-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="1bee1-289">Budete používat stejný **osoba** a **PersonContext** třídy z předchozí cvičení poskytovat stejné osobě data ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee1-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="1bee1-290">Zobrazí se, jak můžete stejné prostředky zpřístupnit různými způsoby v rámci stejné aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1bee1-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="1bee1-291">Úloha 1 – Vytvoření Kontroleru webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1bee1-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="1bee1-292">V této úloze vytvoříte nový **Kontroleru webového rozhraní API** , který bude vystavovat osoba data ve formátu použitelnost počítače jako JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee1-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="1bee1-293">Pokud ještě není otevřen, otevřete **Visual Studio Express 2013 for Web** a otevřete **MyHybridSite.sln** řešení nachází v **zdroj/EX3.-WebAPI/Begin** složky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="1bee1-294">Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-295">Pokud byste začali s počáteční řešení od cvičení 3, stiskněte klávesu **CTRL + SHIFT + B** k sestavení řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="1bee1-296">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nová vygenerovaná položka...** .</span><span class="sxs-lookup"><span data-stu-id="1bee1-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Vytvoření nové vygenerované Kontroleru](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="1bee1-298">*Vytvoření nové vygenerované Kontroleru*</span><span class="sxs-lookup"><span data-stu-id="1bee1-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="1bee1-299">V **přidat vygenerované uživatelské rozhraní** dialogu **webového rozhraní API** v levém podokně, pak **Kontroleru webového rozhraní API 2 s akcemi používající nástroj Entity Framework** v prostředním podokně a pak klikněte na tlačítko  **Přidáte.**</span><span class="sxs-lookup"><span data-stu-id="1bee1-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="1bee1-300">![Vyberte kontroler rozhraní Web API 2 s akcemi a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "výběrem Kontroleru webového rozhraní API s akcemi a Entity Framework 2")</span><span class="sxs-lookup"><span data-stu-id="1bee1-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="1bee1-301">*Vyberte kontroler rozhraní Web API 2 s akcemi a Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="1bee1-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="1bee1-302">Nastavte *ApiPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  a **PersonContext (MyHybridSite.Models)** jako **modelu** a **kontext dat** třídy v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="1bee1-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="1bee1-303">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-303">Then click **Add**.</span></span>

    <span data-ttu-id="1bee1-304">![Přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="1bee1-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="1bee1-305">*Přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="1bee1-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="1bee1-306">Visual Studio pak vygeneruje **ApiPersonController** třída s atributem čtyři akcemi CRUD pro práci s vašimi daty.</span><span class="sxs-lookup"><span data-stu-id="1bee1-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="1bee1-307">![Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")</span><span class="sxs-lookup"><span data-stu-id="1bee1-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="1bee1-308">*Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*</span><span class="sxs-lookup"><span data-stu-id="1bee1-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="1bee1-309">Otevřít **ApiPersonController.cs** soubor a zkontrolujte *GetPeople* metody akce.</span><span class="sxs-lookup"><span data-stu-id="1bee1-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="1bee1-310">Tato metoda dotazuje pole db **PersonContext** aby bylo možné získat data osob.</span><span class="sxs-lookup"><span data-stu-id="1bee1-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="1bee1-311">Nyní Všimněte si, že komentář nad definice metody.</span><span class="sxs-lookup"><span data-stu-id="1bee1-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="1bee1-312">Poskytuje identifikátor URI, který zpřístupňuje tuto akci, kterou použijete v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="1bee1-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="1bee1-313">Ve výchozím nastavení, webové rozhraní API umožňují odchytit dotazy na */webové rozhraní API* cestu pro zabránění kolizím s kontrolery MVC.</span><span class="sxs-lookup"><span data-stu-id="1bee1-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="1bee1-314">Pokud potřebujete změnit toto nastavení, podívejte se na [směrování v rozhraní ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1bee1-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="1bee1-315">Úloha 2 – spuštění řešení</span><span class="sxs-lookup"><span data-stu-id="1bee1-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="1bee1-316">V této úloze bude používat Internet Explorer **vývojářské nástroje F12 pomáhají** ke kontrole úplnou odpověď z kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1bee1-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="1bee1-317">Zobrazí se, jak můžete zaznamenávat provoz sítě získat lepší přehled o vašich aplikačních dat.</span><span class="sxs-lookup"><span data-stu-id="1bee1-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="1bee1-318">Ujistěte se, že **aplikace Internet Explorer** výběru v **Start** tlačítka umístěny na panelu nástrojů sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bee1-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Možnosti aplikace Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="1bee1-320">**Vývojářské nástroje F12 pomáhají** mají celou sadu funkcí, které nejsou uvedeny v tomto praktická-testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="1bee1-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="1bee1-321">Pokud chcete další informace o tom, podívejte se na [pomocí vývojářských nástrojů F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="1bee1-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="1bee1-322">Stisknutím klávesy **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-323">Aby bylo možné provést tento úkol správně, musí mít data vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="1bee1-324">Pokud vaše databáze je prázdná, můžete přejít zpět k úloze 3 v cvičení 2 a postupujte podle kroků pro vytvoření nové osobě pomocí zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="1bee1-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="1bee1-325">V prohlížeči stiskněte **F12** otevřít **vývojářské nástroje** panelu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="1bee1-326">Stisknutím klávesy **CTRL** + **4** nebo klikněte na tlačítko **sítě** ikony a pak klikněte na tlačítko začít zachytávání síťového provozu na zelenou šipku.</span><span class="sxs-lookup"><span data-stu-id="1bee1-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="1bee1-327">![Inicializace webového rozhraní API zachytávání sítě](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "zachytávání sítě inicializaci webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="1bee1-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="1bee1-328">*Spouštění Zachytávání síťových rozhraní Web API*</span><span class="sxs-lookup"><span data-stu-id="1bee1-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="1bee1-329">Připojit **rozhraní api/ApiPerson** na adresu URL v adresním řádku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1bee1-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="1bee1-330">Nyní zkontrolujete podrobnosti odpovědi **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="1bee1-331">![Načítají se data osob prostřednictvím webového rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "načítání dat osoby prostřednictvím webového rozhraní API")</span><span class="sxs-lookup"><span data-stu-id="1bee1-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="1bee1-332">*Načítají se data osob prostřednictvím webového rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="1bee1-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-333">Jakmile se stahování dokončí, vyzve k akci udělat pomocí staženého souboru.</span><span class="sxs-lookup"><span data-stu-id="1bee1-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="1bee1-334">Ponechte dialogové okno otevřené, aby bylo možné sledovat obsah odpovědi prostřednictvím panel nástrojů pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="1bee1-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="1bee1-335">Nyní bude kontrolovat textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1bee1-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="1bee1-336">Chcete-li to provést, klikněte na tlačítko **podrobnosti** kartu a potom klikněte na tlačítko **tělo odpovědi**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="1bee1-337">Můžete zkontrolovat, že stažených dat je seznam objektů s vlastnostmi **Id**, **název** a **stáří** , které odpovídají **osoba** Třída.</span><span class="sxs-lookup"><span data-stu-id="1bee1-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="1bee1-338">![Text odpovědi rozhraní API webové zobrazení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "text odpovědi rozhraní API webové zobrazení")</span><span class="sxs-lookup"><span data-stu-id="1bee1-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="1bee1-339">*Text odpovědi rozhraní API webové zobrazení*</span><span class="sxs-lookup"><span data-stu-id="1bee1-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="1bee1-340">Úloha 3 – Přidání webové stránky nápovědy k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1bee1-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="1bee1-341">Při vytváření webové rozhraní API, je užitečné vytvořit stránku nápovědy tak, aby ostatní vývojáři vědět, jak volat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1bee1-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="1bee1-342">Můžete vytvořit a aktualizovat stránky dokumentace ručně, ale je vhodnější pro automatické generování je vyhnout se nutnosti údržby práci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="1bee1-343">V této úloze používáte balíček Nuget k automatickému generování stránek nápovědy webového rozhraní API do řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="1bee1-344">Z **nástroje** v aplikaci Visual Studio, vyberte v nabídce **Správce balíčků NuGet**a potom klepněte na **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-344">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="1bee1-345">V **Konzola správce balíčků** okno, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1bee1-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="1bee1-346">**Microsoft.AspNet.WebApi.HelpPage** balíček nainstaluje potřebná sestavení a přidá zobrazení MVC pro stránky nápovědy v rámci **oblasti/HelpPage** složky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="1bee1-347">![Oblast HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage oblasti")</span><span class="sxs-lookup"><span data-stu-id="1bee1-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="1bee1-348">*HelpPage oblasti*</span><span class="sxs-lookup"><span data-stu-id="1bee1-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="1bee1-349">Ve výchozím nastavení, v nápovědě stránky obsahují zástupný text řetězce pro dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="1bee1-350">Dokumentační komentáře XML slouží k vytvoření v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1bee1-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="1bee1-351">Chcete-li tuto funkci povolit, otevřete **HelpPageConfig.cs** soubor umístěný ve **oblasti/HelpPage/aplikace\_Start** složky a zrušte komentář u následujícího řádku:</span><span class="sxs-lookup"><span data-stu-id="1bee1-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="1bee1-352">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt **MyHybridSite**vyberte **vlastnosti** a klikněte na tlačítko **sestavení** kartu.</span><span class="sxs-lookup"><span data-stu-id="1bee1-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="1bee1-353">![Vytvořit kartu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "sestavení oddílu")</span><span class="sxs-lookup"><span data-stu-id="1bee1-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="1bee1-354">*Vytvořit kartu*</span><span class="sxs-lookup"><span data-stu-id="1bee1-354">*Build tab*</span></span>
5. <span data-ttu-id="1bee1-355">V části **výstup**vyberte **soubor dokumentace XML**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="1bee1-356">Do textového pole zadejte **aplikace\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="1bee1-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="1bee1-357">![Výstup v kartě sestavení části](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "výstupní sekce kartě sestavení")</span><span class="sxs-lookup"><span data-stu-id="1bee1-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="1bee1-358">*Výstupní sekce kartě sestavení*</span><span class="sxs-lookup"><span data-stu-id="1bee1-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="1bee1-359">Stisknutím klávesy **CTRL** + **S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="1bee1-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="1bee1-360">Otevřít **ApiPersonController.cs** soubor **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="1bee1-361">Zadejte nový řádek mezi *GetPeople* podpisu metody a */ / api/ApiPerson GET* komentář a potom zadejte třemi lomítky.</span><span class="sxs-lookup"><span data-stu-id="1bee1-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-362">Sada Visual Studio automaticky vloží elementy XML, které definují v dokumentaci k metodě.</span><span class="sxs-lookup"><span data-stu-id="1bee1-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="1bee1-363">Přidejte souhrnný text a návratová hodnota pro *GetPeople* metody.</span><span class="sxs-lookup"><span data-stu-id="1bee1-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="1bee1-364">By měl vypadat nějak takto.</span><span class="sxs-lookup"><span data-stu-id="1bee1-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="1bee1-365">Stisknutím klávesy **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="1bee1-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="1bee1-366">Připojit **/help** na adresu URL do adresního řádku a přejděte na stránku nápovědy.</span><span class="sxs-lookup"><span data-stu-id="1bee1-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="1bee1-367">![Stránka nápovědy pro rozhraní ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "stránku s nápovědou rozhraní ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="1bee1-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="1bee1-368">*Stránka nápovědy webové rozhraní API technologie ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="1bee1-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1bee1-369">Hlavní obsah stránky je tabulka rozhraní API, seskupené podle kontroleru.</span><span class="sxs-lookup"><span data-stu-id="1bee1-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="1bee1-370">Položky tabulky generuje dynamicky pomocí **IApiExplorer** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1bee1-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="1bee1-371">Pokud přidáte nebo aktualizujete kontroler API, v tabulce budou automaticky aktualizovány při příštím sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="1bee1-372">**API** sloupec uvádí metodu HTTP a relativní identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="1bee1-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="1bee1-373">**Popis** sloupec obsahuje informace, které byly extrahovány z metody dokumentace.</span><span class="sxs-lookup"><span data-stu-id="1bee1-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="1bee1-374">Všimněte si, že popis, který jste přidali výše definici metody se zobrazí ve sloupci Popis.</span><span class="sxs-lookup"><span data-stu-id="1bee1-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="1bee1-375">![Popis metody rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "popis rozhraní API – metoda")</span><span class="sxs-lookup"><span data-stu-id="1bee1-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="1bee1-376">*Popis rozhraní API – metoda*</span><span class="sxs-lookup"><span data-stu-id="1bee1-376">*API method description*</span></span>
13. <span data-ttu-id="1bee1-377">Klikněte na jednu z metod rozhraní API přejít na stránku podrobné informace, včetně těla odpovědi vzorku.</span><span class="sxs-lookup"><span data-stu-id="1bee1-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="1bee1-378">![Stránka informace o podrobnosti](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "podrobné informace o stránce")</span><span class="sxs-lookup"><span data-stu-id="1bee1-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="1bee1-379">*Podrobné informace o stránce*</span><span class="sxs-lookup"><span data-stu-id="1bee1-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1bee1-380">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1bee1-380">Summary</span></span>

<span data-ttu-id="1bee1-381">Po dokončení tohoto praktických cvičení jste se naučili, jak:</span><span class="sxs-lookup"><span data-stu-id="1bee1-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="1bee1-382">Vytvořit novou webovou aplikaci pomocí jednoho rozhraní ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1bee1-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="1bee1-383">Integrace více technologie ASP.NET do jednoho jediného projektu</span><span class="sxs-lookup"><span data-stu-id="1bee1-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="1bee1-384">Generování kontrolerů a zobrazení MVC z tříd modelu pomocí technologie ASP.NET generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1bee1-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="1bee1-385">Generování kontrolerů rozhraní Web API, které používají funkce, jako je asynchronní programování a přístup k datům pomocí Entity Frameworku</span><span class="sxs-lookup"><span data-stu-id="1bee1-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="1bee1-386">Automatické generování stránky nápovědy webového rozhraní API pro vaše řadiče</span><span class="sxs-lookup"><span data-stu-id="1bee1-386">Automatically generate Web API Help Pages for your controllers</span></span>
