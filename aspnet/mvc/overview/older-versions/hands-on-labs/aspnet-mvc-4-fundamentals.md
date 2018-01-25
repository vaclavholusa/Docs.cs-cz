---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "Základy architektury ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Toto testovací prostředí Hands-On vychází z úložiště Hudba MVC (Model View Controller), kurz aplikace, která představuje a vysvětluje podrobný postup používání ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 468f6d5dabb645b1c005680dc5a1ffc4debd63b6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="6165a-103">Základy architektury ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6165a-103">ASP.NET MVC 4 Fundamentals</span></span>
====================
<span data-ttu-id="6165a-104">podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6165a-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="6165a-105">Toto testovací prostředí Hands-On je založena na rozhraní MVC (Model View Controller) Hudba úložiště, kurz aplikace, která uvádí a popisuje podrobný aplikace ASP.NET MVC a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-105">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="6165a-106">V testovacím prostředí se dozvíte, jednoduchost, ještě power společně používání těchto technologií.</span><span class="sxs-lookup"><span data-stu-id="6165a-106">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="6165a-107">Se spustí s jednoduchou aplikaci a bude sestavte jej, dokud nebudete mít plně funkční ASP.NET MVC 4 webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6165a-107">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>
> 
> <span data-ttu-id="6165a-108">Toto testovací prostředí pracuje s ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6165a-108">This Lab works with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="6165a-109">Pokud chcete prozkoumat verze ASP.NET MVC 3 kurz aplikace, najdete ji v [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="6165a-109">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="6165a-110">Toto testovací prostředí Hands-On předpokládá, že vývojář má prostředí do webové vývoj technologií, jako je například HTML a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6165a-110">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>
> 
> 
> <span data-ttu-id="6165a-111">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="6165a-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="6165a-112">Aplikaci Store Hudba</span><span class="sxs-lookup"><span data-stu-id="6165a-112">The Music Store application</span></span>

<span data-ttu-id="6165a-113">Hudba úložiště webové aplikace, které budou vytvořeny v celém tomto testovacím prostředí zahrnuje tři hlavní části: nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="6165a-113">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="6165a-114">Bude moct procházet alb podle genre, přidejte do své košíku alb, zkontrolujte jejich výběr a nakonec pokračujte najdete v článku věnovaném přihlášení a dokončení pořadí návštěvníky.</span><span class="sxs-lookup"><span data-stu-id="6165a-114">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="6165a-115">Kromě toho Správci úložiště bude možné spravovat dostupné alb, jakož i jejich hlavní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6165a-115">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="6165a-116">![Hudba úložiště obrazovky](aspnet-mvc-4-fundamentals/_static/image1.png "obrazovky Hudba úložiště")</span><span class="sxs-lookup"><span data-stu-id="6165a-116">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="6165a-117">*Hudba úložiště obrazovky*</span><span class="sxs-lookup"><span data-stu-id="6165a-117">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="6165a-118">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="6165a-118">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="6165a-119">Aplikaci Store Hudba budou vytvořeny pomocí **řadiče MVC (Model View)**, architekturní vzor, který rozděluje aplikace do tří hlavních součástí:</span><span class="sxs-lookup"><span data-stu-id="6165a-119">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="6165a-120">**Modely**: objekty modelů jsou části aplikace, které implementují logiku domény.</span><span class="sxs-lookup"><span data-stu-id="6165a-120">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="6165a-121">Objekty modelů často také načíst a ukládají stav modelu v databázi.</span><span class="sxs-lookup"><span data-stu-id="6165a-121">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="6165a-122">**Zobrazení:** zobrazení jsou komponenty, které zobrazují aplikace uživatelské rozhraní (UI).</span><span class="sxs-lookup"><span data-stu-id="6165a-122">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="6165a-123">Toto uživatelské rozhraní se obvykle je vytvořena z dat modelu.</span><span class="sxs-lookup"><span data-stu-id="6165a-123">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="6165a-124">Příkladem může být zobrazení upravit alb, které zobrazuje textová pole a rozevírací seznam na základě aktuálního stavu objektu alb.</span><span class="sxs-lookup"><span data-stu-id="6165a-124">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="6165a-125">**Řadiče:** Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracovat s modelem a konečně vybírají zobrazení k vykreslení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6165a-125">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="6165a-126">V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.</span><span class="sxs-lookup"><span data-stu-id="6165a-126">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="6165a-127">Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy.</span><span class="sxs-lookup"><span data-stu-id="6165a-127">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="6165a-128">Tato separace pomáhá spravovat složitost, když vytvoříte aplikaci, jako umožňuje zaměřit se na jeden aspekt jejich implementace najednou.</span><span class="sxs-lookup"><span data-stu-id="6165a-128">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="6165a-129">Kromě toho vzor MVC usnadňuje testování aplikací, také podpora pro vytváření aplikací pro používání testy řízený vývoj (TDD).</span><span class="sxs-lookup"><span data-stu-id="6165a-129">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="6165a-130">**ASP.NET MVC** framework představuje alternativu ke vzoru webových formulářů ASP.NET pro vytváření založené na ASP.NET MVC webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6165a-130">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="6165a-131">**ASP.NET MVC** framework je odlehčený, intenzivního prezentační architektura která (stejně jako u aplikace využívající webové formuláře) je integrována stávajících funkcí technologie ASP.NET, jako je například hlavní stránky a na základě členství ověřování tak, abyste dosáhli všech power základní rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6165a-131">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="6165a-132">To je užitečné, pokud jste již obeznámeni s webovými formuláři ASP.NET protože všech knihoven, které už používáte jsou také k dispozici v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6165a-132">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="6165a-133">Kromě toho volné párování mezi třemi hlavními komponentami architektury MVC rovněž podporuje paralelní vývoj.</span><span class="sxs-lookup"><span data-stu-id="6165a-133">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="6165a-134">Například jeden vývojář může pracovat na zobrazení, druhý může pracovat na logice kontroleru a třetí můžete soustředit na obchodní logiku v modelu.</span><span class="sxs-lookup"><span data-stu-id="6165a-134">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="6165a-135">Cíle</span><span class="sxs-lookup"><span data-stu-id="6165a-135">Objectives</span></span>

<span data-ttu-id="6165a-136">V tomto testovacím prostředí Hands-On se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="6165a-136">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="6165a-137">Vytvoření aplikace ASP.NET MVC od začátku podle kurzu aplikaci Store Hudba</span><span class="sxs-lookup"><span data-stu-id="6165a-137">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="6165a-138">Přidání řadičů pro zpracování adresy URL na domovské stránce webu a jeho hlavní funkce procházení</span><span class="sxs-lookup"><span data-stu-id="6165a-138">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="6165a-139">Přidání zobrazení upravit obsah zobrazený spolu s jeho styl</span><span class="sxs-lookup"><span data-stu-id="6165a-139">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="6165a-140">Přidání třídy modelu obsahovat a spravovat data a domény logiku</span><span class="sxs-lookup"><span data-stu-id="6165a-140">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="6165a-141">Použití vzoru zobrazení modelu k předávání informací z akce kontroleru zobrazení šablon</span><span class="sxs-lookup"><span data-stu-id="6165a-141">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="6165a-142">Prozkoumat nové šablony ASP.NET MVC 4 pro internetové aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-142">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6165a-143">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6165a-143">Prerequisites</span></span>

<span data-ttu-id="6165a-144">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="6165a-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="6165a-145">[Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat)</span><span class="sxs-lookup"><span data-stu-id="6165a-145">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="6165a-146">Instalace</span><span class="sxs-lookup"><span data-stu-id="6165a-146">Setup</span></span>

<span data-ttu-id="6165a-147">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="6165a-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="6165a-148">Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="6165a-149">K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="6165a-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="6165a-150">Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="6165a-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6165a-151">Cvičení</span><span class="sxs-lookup"><span data-stu-id="6165a-151">Exercises</span></span>

<span data-ttu-id="6165a-152">Toto testovací prostředí Hands-On se skládá ve cvičeních následující:</span><span class="sxs-lookup"><span data-stu-id="6165a-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="6165a-153">Cvičení 1: Vytvoření projektu webové aplikace pomocí MusicStore rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6165a-153">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="6165a-154">Cvičení 2: Vytvoření řadiče</span><span class="sxs-lookup"><span data-stu-id="6165a-154">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="6165a-155">Cvičení 3: Předávání na řadič parametrů</span><span class="sxs-lookup"><span data-stu-id="6165a-155">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="6165a-156">Cvičení 4: Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-156">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="6165a-157">Cvičení 5: Vytvoření modelu zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-157">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="6165a-158">Cvičení 6: Pomocí parametrů v zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-158">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="6165a-159">Cvičení 7: Okruhu kolem ASP.NET MVC 4 nové šablony</span><span class="sxs-lookup"><span data-stu-id="6165a-159">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="6165a-160">Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="6165a-160">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="6165a-161">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.</span><span class="sxs-lookup"><span data-stu-id="6165a-161">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="6165a-162">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="6165a-162">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="6165a-163">Cvičení 1: Vytvoření projektu webové aplikace pomocí MusicStore rozhraní ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6165a-163">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="6165a-164">V tomto cvičení se dozvíte, jak vytvořit aplikaci ASP.NET MVC v aplikaci Visual Studio 2012 Express pro Web, jakož i jeho hlavní složky organizaci.</span><span class="sxs-lookup"><span data-stu-id="6165a-164">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="6165a-165">Kromě toho se dozvíte, jak přidat nový řadič a změňte ji zobrazit jednoduchým řetězcem aplikace domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="6165a-165">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="6165a-166">Úloha 1 – Vytvoření projektu webové aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6165a-166">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="6165a-167">V této úloze vytvoříte pomocí šablony sady Visual Studio MVC prázdný projekt aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6165a-167">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="6165a-168">Spustit **VS Express pro Web**.</span><span class="sxs-lookup"><span data-stu-id="6165a-168">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6165a-169">Na **soubor** nabídky, klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="6165a-169">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="6165a-170">V **nový projekt** dialogovém okně vyberte pole **webové aplikace ASP.NET MVC 4** projektu typu, najdete v **Visual C#,** **webové** šablony seznam.</span><span class="sxs-lookup"><span data-stu-id="6165a-170">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="6165a-171">Změna **název** k *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="6165a-171">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="6165a-172">Nastavte umístění, řešení uvnitř novou **začít** složky v tomto cvičení zdrojové složky, například **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="6165a-172">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="6165a-173">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="6165a-173">Click **OK**.</span></span>

    <span data-ttu-id="6165a-174">![Vytvořit dialogové okno Nový projekt](aspnet-mvc-4-fundamentals/_static/image2.png "vytvořit dialogové okno Nový projekt")</span><span class="sxs-lookup"><span data-stu-id="6165a-174">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="6165a-175">*Vytvořit dialogové okno Nový projekt*</span><span class="sxs-lookup"><span data-stu-id="6165a-175">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="6165a-176">V **nový ASP.NET MVC 4 projekt** dialogovém okně vyberte pole **základní** šablony a ujistěte se, že **zobrazovací modul** vybraná **Razor**.</span><span class="sxs-lookup"><span data-stu-id="6165a-176">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="6165a-177">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="6165a-177">Click **OK**.</span></span>

    <span data-ttu-id="6165a-178">![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-fundamentals/_static/image3.png "architektury ASP.NET MVC 4 projektu dialogové okno Nový")</span><span class="sxs-lookup"><span data-stu-id="6165a-178">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="6165a-179">*ASP.NET MVC 4 projektu dialogové okno Nový*</span><span class="sxs-lookup"><span data-stu-id="6165a-179">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="6165a-180">Úloha 2 – prohlížení struktury řešení</span><span class="sxs-lookup"><span data-stu-id="6165a-180">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="6165a-181">Rozhraní ASP.NET MVC zahrnuje šablona projektu sady Visual Studio, která vám pomůže vytvořit webové aplikace podporující vzor MVC.</span><span class="sxs-lookup"><span data-stu-id="6165a-181">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="6165a-182">Tato šablona vytvoří novou aplikaci ASP.NET MVC Web s potřebné složky, šablony položek a položek konfigurace souboru.</span><span class="sxs-lookup"><span data-stu-id="6165a-182">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="6165a-183">V této úloze prozkoumáte strukturu řešení pochopit prvky, které se podílejí a jejich vztahů.</span><span class="sxs-lookup"><span data-stu-id="6165a-183">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="6165a-184">Následující složky jsou součástí všechny aplikace ASP.NET MVC, protože rozhraní ASP.NET MVC ve výchozím nastavení používá &quot;konvence přes konfigurace&quot; přístup a díky některé předpoklady výchozí podle složky pojmenování konvence.</span><span class="sxs-lookup"><span data-stu-id="6165a-184">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="6165a-185">Po vytvoření projektu, projděte si strukturu složek, který byl vytvořen v Průzkumníku řešení na pravé straně:</span><span class="sxs-lookup"><span data-stu-id="6165a-185">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="6165a-186">![Struktura složek ASP.NET MVC v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image4.png "struktura složek ASP.NET MVC v Průzkumníku řešení")</span><span class="sxs-lookup"><span data-stu-id="6165a-186">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="6165a-187">*Struktura složek ASP.NET MVC v Průzkumníku řešení*</span><span class="sxs-lookup"><span data-stu-id="6165a-187">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

    1. <span data-ttu-id="6165a-188">**Řadiče**.</span><span class="sxs-lookup"><span data-stu-id="6165a-188">**Controllers**.</span></span> <span data-ttu-id="6165a-189">Tato složka bude obsahovat třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6165a-189">This folder will contain the controller classes.</span></span> <span data-ttu-id="6165a-190">V aplikaci MVC na základě řadiče jsou zodpovědná za zpracování interakce s koncovým uživatelem, manipulace s modelem a nakonec výběr zobrazení k vykreslení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6165a-190">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

        > [!NOTE]
        > <span data-ttu-id="6165a-191">Rozhraní MVC požaduje názvy všech řadičů tak, aby končit &quot;řadič&quot;– například HomeController, LoginController nebo ProductController.</span><span class="sxs-lookup"><span data-stu-id="6165a-191">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
    2. <span data-ttu-id="6165a-192">**Modely**.</span><span class="sxs-lookup"><span data-stu-id="6165a-192">**Models**.</span></span> <span data-ttu-id="6165a-193">Tato složka se poskytuje třídy, které představují aplikačního modelu pro MVC webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-193">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="6165a-194">To obvykle zahrnuje kód, který definuje objekty a logiku pro interakci s úložištěm dat.</span><span class="sxs-lookup"><span data-stu-id="6165a-194">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="6165a-195">Obvykle objekty skutečné modelu bude v samostatné třídy knihovny.</span><span class="sxs-lookup"><span data-stu-id="6165a-195">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="6165a-196">Ale když vytvoříte novou aplikaci, můžete zahrnout třídy a přesuňte je do knihovny tříd samostatné později v cyklu vývoje.</span><span class="sxs-lookup"><span data-stu-id="6165a-196">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
    3. <span data-ttu-id="6165a-197">**Zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-197">**Views**.</span></span> <span data-ttu-id="6165a-198">Tato složka je doporučené umístění pro zobrazení, komponenty zodpovědná za zobrazení uživatelského rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-198">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="6165a-199">Zobrazení souborů .aspx, .ascx, .cshtml a .master pomocí všech ostatních souborů, které se vztahují k vykreslení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-199">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="6165a-200">Složka zobrazení obsahuje složku pro každý řadič; Složka služby se nazývá s předponou názvu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6165a-200">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="6165a-201">Například, pokud se jedná o zařízení s názvem **HomeController**, složce zobrazení bude obsahovat složku s názvem Domů.</span><span class="sxs-lookup"><span data-stu-id="6165a-201">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="6165a-202">Ve výchozím nastavení, pokud rozhraní ASP.NET MVC načte zobrazení, hledá soubor .aspx s názvem požadované zobrazení ve složce Views\controllerName (**zobrazení [ControllerName] [akce] .aspx**) nebo (**zobrazení [ControllerName] [Akce] .cshtml**) pro zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="6165a-202">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-203">Kromě složky uvedených výše, používá MVC webovou aplikaci **Global.asax** výchozí soubor, aby globální směrování adres URL a používá **Web.config** soubor konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-203">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="6165a-204">Úloha 3 – Přidání HomeController</span><span class="sxs-lookup"><span data-stu-id="6165a-204">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="6165a-205">V aplikacích ASP.NET, které nepoužívají rozhraní MVC je organizovaná interakci s uživatelem okolo stránek a kolem vyvolání a zpracování události z tyto stránek.</span><span class="sxs-lookup"><span data-stu-id="6165a-205">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="6165a-206">Interakce uživatele s aplikací ASP.NET MVC je naopak věnuje řadiče a jejich metody akce.</span><span class="sxs-lookup"><span data-stu-id="6165a-206">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="6165a-207">Na druhé straně rozhraní ASP.NET MVC mapuje adresy URL do tříd, které se označují jako řadiče.</span><span class="sxs-lookup"><span data-stu-id="6165a-207">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="6165a-208">Řadiče zpracovat příchozí žádosti, zpracování uživatelského vstupu a interakce, spouštět logiku příslušné aplikace a určení odpověď k odeslání zpět do klienta (zobrazení HTML, stažení souboru, přesměrovat na jinou adresu URL, atd.).</span><span class="sxs-lookup"><span data-stu-id="6165a-208">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="6165a-209">V případě zobrazení HTML, třídy kontroleru obvykle volá komponentu oddělená zobrazení pro generování kód HTML pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="6165a-209">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="6165a-210">V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.</span><span class="sxs-lookup"><span data-stu-id="6165a-210">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="6165a-211">V této úloze přidáte řadič třídu, která bude zpracovávat adresy URL na domovské stránce webu Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="6165a-211">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="6165a-212">Klikněte pravým tlačítkem na **řadiče** složky v Průzkumníku řešení, vyberte **přidat** a potom **řadič** příkaz:</span><span class="sxs-lookup"><span data-stu-id="6165a-212">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="6165a-213">![Přidat řadič příkaz](aspnet-mvc-4-fundamentals/_static/image5.png "přidat příkaz řadiče")</span><span class="sxs-lookup"><span data-stu-id="6165a-213">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="6165a-214">*Přidat řadič – příkaz*</span><span class="sxs-lookup"><span data-stu-id="6165a-214">*Add Controller Command*</span></span>
2. <span data-ttu-id="6165a-215">**Přidat kontroler** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6165a-215">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="6165a-216">Název kontroleru *HomeController* a stiskněte klávesu **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-216">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="6165a-217">![Dialogové okno řadiče přidání](aspnet-mvc-4-fundamentals/_static/image6.png "řadiče dialogové okno Přidání")</span><span class="sxs-lookup"><span data-stu-id="6165a-217">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="6165a-218">*Dialogové okno řadiče přidání*</span><span class="sxs-lookup"><span data-stu-id="6165a-218">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="6165a-219">Soubor **HomeController.cs** je vytvořen v **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="6165a-219">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="6165a-220">Aby bylo možné používat **HomeController** vrátí řetězec na jeho akce indexu, nahraďte **Index** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-220">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="6165a-221">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="6165a-221">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="6165a-222">Úloha 4 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-222">Task 4 - Running the Application</span></span>

<span data-ttu-id="6165a-223">V této úloze můžete vyzkoušet na aplikaci ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6165a-223">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="6165a-224">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-224">Press **F5** to run the Application.</span></span> <span data-ttu-id="6165a-225">Kompilace projektu a spustí místní webový Server IIS.</span><span class="sxs-lookup"><span data-stu-id="6165a-225">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="6165a-226">Místní webový Server IIS se automaticky otevře webový prohlížeč, přejdete na adresu URL webového serveru.</span><span class="sxs-lookup"><span data-stu-id="6165a-226">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="6165a-227">![Aplikace běžící ve webovém prohlížeči](aspnet-mvc-4-fundamentals/_static/image7.png "aplikaci spuštěnou ve webovém prohlížeči")</span><span class="sxs-lookup"><span data-stu-id="6165a-227">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="6165a-228">*Aplikace běžící ve webovém prohlížeči*</span><span class="sxs-lookup"><span data-stu-id="6165a-228">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-229">Místního webového serveru IIS se spustí na webu na náhodných volné portu s číslem.</span><span class="sxs-lookup"><span data-stu-id="6165a-229">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="6165a-230">Ve výše uvedeném obrázku webu běží v `http://localhost:50103/`, takže používá port 50103.</span><span class="sxs-lookup"><span data-stu-id="6165a-230">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="6165a-231">Vaše číslo portu se může lišit.</span><span class="sxs-lookup"><span data-stu-id="6165a-231">Your port number may vary.</span></span>
2. <span data-ttu-id="6165a-232">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6165a-232">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="6165a-233">Cvičení 2: Vytvoření řadiče</span><span class="sxs-lookup"><span data-stu-id="6165a-233">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="6165a-234">V tomto cvičení se dozvíte, jak k aktualizaci řadiče implementovat jednoduché funkce aplikace Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="6165a-234">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="6165a-235">Tomuto řadiči definují akce metody pro zpracování každé z následujících konkrétní požadavky:</span><span class="sxs-lookup"><span data-stu-id="6165a-235">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="6165a-236">Stránka výpis žánry Hudba v úložišti Hudba</span><span class="sxs-lookup"><span data-stu-id="6165a-236">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="6165a-237">Procházet stránky, který se zobrazí seznam všech hudebních alb pro konkrétní genre</span><span class="sxs-lookup"><span data-stu-id="6165a-237">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="6165a-238">Stránka Podrobnosti, která se zobrazují informace o konkrétní Hudba album</span><span class="sxs-lookup"><span data-stu-id="6165a-238">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="6165a-239">Pro obor tohoto cvičení tyto akce jednoduše vrátí řetězec nyní.</span><span class="sxs-lookup"><span data-stu-id="6165a-239">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="6165a-240">Úloha 1 – přidání nové třídy StoreController</span><span class="sxs-lookup"><span data-stu-id="6165a-240">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="6165a-241">V této úloze přidáte nový řadič.</span><span class="sxs-lookup"><span data-stu-id="6165a-241">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="6165a-242">Pokud už otevřený, spusťte **VS Express pro Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="6165a-242">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="6165a-243">V **soubor** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="6165a-243">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6165a-244">V dialogovém okně Otevřít projekt přejděte do **Source\Ex02 CreatingAController\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="6165a-244">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6165a-245">Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="6165a-245">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="6165a-246">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6165a-246">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6165a-247">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6165a-247">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6165a-248">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="6165a-248">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6165a-249">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-249">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-250">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-250">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6165a-251">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-251">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6165a-252">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-252">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6165a-253">Přidáte nový řadič.</span><span class="sxs-lookup"><span data-stu-id="6165a-253">Add the new controller.</span></span> <span data-ttu-id="6165a-254">Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složky v Průzkumníku řešení, vyberte **přidat** a potom **řadič** příkaz.</span><span class="sxs-lookup"><span data-stu-id="6165a-254">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="6165a-255">Změna **názvu Kontroleru** k *StoreController*a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-255">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="6165a-256">![Dialogové okno řadiče přidání](aspnet-mvc-4-fundamentals/_static/image8.png "řadiče dialogové okno Přidání")</span><span class="sxs-lookup"><span data-stu-id="6165a-256">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="6165a-257">*Dialogové okno řadiče přidání*</span><span class="sxs-lookup"><span data-stu-id="6165a-257">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="6165a-258">Úloha 2 – úpravy StoreController akce</span><span class="sxs-lookup"><span data-stu-id="6165a-258">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="6165a-259">Tato úloha slouží k úpravě řadiče metody, které se nazývají **akce**.</span><span class="sxs-lookup"><span data-stu-id="6165a-259">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="6165a-260">Akce jsou zodpovědná za zpracování žádostí adresy URL a určení, obsah, který by měly být odeslány zpět do prohlížeče nebo uživatele, který volá adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-260">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="6165a-261">**StoreController** třídy již **Index** metoda.</span><span class="sxs-lookup"><span data-stu-id="6165a-261">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="6165a-262">Použijete ho později v tomto testovacím prostředí implementovat stránka, která obsahuje seznam všech žánry Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="6165a-262">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="6165a-263">Prozatím se právě nahradit **Index** metoda s následující kód, který vrací řetězec &quot;Hello z Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="6165a-263">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="6165a-264">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex2 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="6165a-264">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="6165a-265">Přidat **Procházet** a **podrobnosti** metody.</span><span class="sxs-lookup"><span data-stu-id="6165a-265">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="6165a-266">K tomu, přidejte následující kód, který **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="6165a-266">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="6165a-267">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="6165a-267">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="6165a-268">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-268">Task 3 - Running the Application</span></span>

<span data-ttu-id="6165a-269">V této úloze můžete vyzkoušet na aplikaci ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6165a-269">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="6165a-270">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-270">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-271">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-271">The project starts in the **Home** page.</span></span> <span data-ttu-id="6165a-272">Změňte adresu URL k ověření implementace každá akce.</span><span class="sxs-lookup"><span data-stu-id="6165a-272">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="6165a-273">**/ Ukládání**.</span><span class="sxs-lookup"><span data-stu-id="6165a-273">**/Store**.</span></span> <span data-ttu-id="6165a-274">Zobrazí se  **&quot;Hello z Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="6165a-274">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="6165a-275">**/ Úložiště nebo procházení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-275">**/Store/Browse**.</span></span> <span data-ttu-id="6165a-276">Zobrazí se  **&quot;Hello z Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="6165a-276">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="6165a-277">**/ / Podrobnosti úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6165a-277">**/Store/Details**.</span></span> <span data-ttu-id="6165a-278">Zobrazí se  **&quot;Hello z Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="6165a-278">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="6165a-279">![Procházení StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "procházení StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="6165a-279">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="6165a-280">*Procházení /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="6165a-280">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="6165a-281">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6165a-281">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="6165a-282">Cvičení 3: Předávání na řadič parametrů</span><span class="sxs-lookup"><span data-stu-id="6165a-282">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="6165a-283">Dosud jste byla vrací konstantní řetězce z řadičů.</span><span class="sxs-lookup"><span data-stu-id="6165a-283">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="6165a-284">V tomto cvičení se dozvíte, jak předat parametry do řadiče pomocí adresy URL a řetězce dotazu a pak provedením metoda akce odpovídat text do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6165a-284">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="6165a-285">Úloha 1 – Přidání parametru Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="6165a-285">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="6165a-286">V této úloze budete používat **řetězce dotazu** odeslat parametry, které **Procházet** metodu akce v **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="6165a-286">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="6165a-287">Pokud už otevřený, spusťte **VS Express pro Web**.</span><span class="sxs-lookup"><span data-stu-id="6165a-287">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6165a-288">V **soubor** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="6165a-288">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6165a-289">V dialogovém okně Otevřít projekt přejděte do **Source\Ex03 PassingParametersToAController\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="6165a-289">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6165a-290">Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="6165a-290">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="6165a-291">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6165a-291">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6165a-292">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6165a-292">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6165a-293">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="6165a-293">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6165a-294">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-294">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-295">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-295">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6165a-296">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-296">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6165a-297">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-297">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6165a-298">Otevřete **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-298">Open **StoreController** class.</span></span> <span data-ttu-id="6165a-299">Chcete-li to provést, v **Průzkumníku řešení**, rozbalte **řadiče** složku a dvojím kliknutím **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="6165a-299">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="6165a-300">Změna **Procházet** metoda, přidání k vyžádání pro konkrétní genre parametr řetězce.</span><span class="sxs-lookup"><span data-stu-id="6165a-300">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="6165a-301">ASP.NET MVC automaticky předat žádné řetězce dotazu nebo zpracování odeslaného formuláře Parametry s názvem **genre** k této metodě akce při vyvolání.</span><span class="sxs-lookup"><span data-stu-id="6165a-301">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="6165a-302">Chcete-li to provést, nahraďte **Procházet** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-302">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="6165a-303">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - EX3. StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="6165a-303">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="6165a-304">Používáte **HttpUtility.HtmlEncode** nástroj metodu zabraňuje uživatelům vložení Javascript do zobrazení s odkazem jako   **/úložiště/Procházet? Genre =&lt;skriptu&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="6165a-304">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
    > 
    > <span data-ttu-id="6165a-305">Další vysvětlení, navštivte [tohoto článku na webu msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="6165a-305">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="6165a-306">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="6165a-307">V této úloze, můžete vyzkoušet na aplikaci ve webovém prohlížeči a používat **genre** parametr.</span><span class="sxs-lookup"><span data-stu-id="6165a-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="6165a-308">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-309">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="6165a-310">Změňte adresu URL na   */ukládání/Procházet? Genre = Disco* k ověření, že akce obdrží parametr genre.</span><span class="sxs-lookup"><span data-stu-id="6165a-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="6165a-311">![Procházení StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "procházení StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="6165a-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="6165a-312">*Procházení /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="6165a-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="6165a-313">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6165a-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="6165a-314">Úloha 3 – Přidání parametru Id adresy URL</span><span class="sxs-lookup"><span data-stu-id="6165a-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="6165a-315">V této úloze budete používat **URL** předat **Id** parametru **podrobnosti** metody akce **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="6165a-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="6165a-316">ASP.NET MVC výchozích konvencí směrování se zachází segment adresy URL po název metody akce jako parametr s názvem **Id**. Pokud vaše metoda akce má parametr s názvem Id, potom ASP.NET MVC automaticky předá segment adresy URL je jako parametr.</span><span class="sxs-lookup"><span data-stu-id="6165a-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="6165a-317">V adrese URL **úložiště/podrobnosti/5**, **Id** , bude vyhodnocen jako **5**.</span><span class="sxs-lookup"><span data-stu-id="6165a-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="6165a-318">Změna **podrobnosti** metodu **StoreController**, přidání **int** parametr s názvem **id**. Chcete-li to provést, nahraďte **podrobnosti** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="6165a-319">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - EX3. StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="6165a-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="6165a-320">Úloha 4 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="6165a-321">V této úloze, můžete vyzkoušet na aplikaci ve webovém prohlížeči a používat **Id** parametr.</span><span class="sxs-lookup"><span data-stu-id="6165a-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="6165a-322">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-323">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="6165a-324">Změňte adresu URL na */Store/Details/5* k ověření, že akce obdrží parametr id.</span><span class="sxs-lookup"><span data-stu-id="6165a-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="6165a-325">![Procházení StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "procházení StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="6165a-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="6165a-326">*Procházení /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="6165a-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="6165a-327">Cvičení 4: Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="6165a-328">Mít byla dosavadní vrácení řetězce z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6165a-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="6165a-329">I když snadno pochopení fungování řadiče, je to, jak nejsou vytvořené skutečné webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6165a-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="6165a-330">Zobrazení jsou součástí, které poskytují lepší přístup ke generování HTML zpět do prohlížeče s použitím soubory šablon.</span><span class="sxs-lookup"><span data-stu-id="6165a-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="6165a-331">V tomto cvičení se dozvíte, jak přidat rozložení stránky předlohy nastavit šablonu pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování webu a v neposlední řadě zobrazit šablonu umožňující HomeController vrátit HTML.</span><span class="sxs-lookup"><span data-stu-id="6165a-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="6165a-332">Úloha 1 - upravovat soubor \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="6165a-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="6165a-333">Soubor **~/Views/Shared/\_layout.cshtml** umožňuje nastavit šablonu pro běžné HTML a použít v rámci celého webu.</span><span class="sxs-lookup"><span data-stu-id="6165a-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="6165a-334">V této úloze přidáte rozložení stránky předlohy s běžné hlavička s odkazy na oblasti domovské stránky a úložiště.</span><span class="sxs-lookup"><span data-stu-id="6165a-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="6165a-335">Pokud už otevřený, spusťte **VS Express pro Web**.</span><span class="sxs-lookup"><span data-stu-id="6165a-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6165a-336">V **soubor** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="6165a-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6165a-337">V dialogovém okně Otevřít projekt přejděte do **Source\Ex04 CreatingAView\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="6165a-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6165a-338">Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="6165a-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="6165a-339">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6165a-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6165a-340">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6165a-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6165a-341">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="6165a-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6165a-342">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-343">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6165a-344">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6165a-345">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6165a-346">Soubor  **\_layout.cshtml** obsahuje rozložení HTML kontejner pro všechny stránky webu.</span><span class="sxs-lookup"><span data-stu-id="6165a-346">The file **\_layout.cshtml** contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="6165a-347">Obsahuje  **&lt;html&gt;**  element pro odpovědi HTML a taky  **&lt;head&gt;**  a  **&lt;textu&gt;**  elementy.</span><span class="sxs-lookup"><span data-stu-id="6165a-347">It includes the **&lt;html&gt;** element for the HTML response, as well as the **&lt;head&gt;** and **&lt;body&gt;** elements.</span></span> <span data-ttu-id="6165a-348">**@RenderBody()** v kódu HTML textu identifikovat oblasti tohoto zobrazení šablony budou moci uživatelé zadat dynamický obsah.</span><span class="sxs-lookup"><span data-stu-id="6165a-348">**@RenderBody()** within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
<span data-ttu-id="6165a-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="6165a-350">Přidáte hlavičku běžné s odkazy do oblasti domovské stránky a úložiště na všech stránkách v této lokalitě.</span><span class="sxs-lookup"><span data-stu-id="6165a-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="6165a-351">Aby bylo možné provést, přidejte následující kód níže &lt;textu&gt; příkaz.</span><span class="sxs-lookup"><span data-stu-id="6165a-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
<span data-ttu-id="6165a-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="6165a-353">Zahrnout div k vykreslení části textu každé stránce.</span><span class="sxs-lookup"><span data-stu-id="6165a-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="6165a-354">Nahraďte  **@RenderBody()** následujícím kódem higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-354">Replace **@RenderBody()** with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="6165a-355">Věděli jste?</span><span class="sxs-lookup"><span data-stu-id="6165a-355">Did you know?</span></span> <span data-ttu-id="6165a-356">Visual Studio 2012 má fragmenty kódu, které usnadňují přidejte běžně používané kód HTML, soubory kódu a další!</span><span class="sxs-lookup"><span data-stu-id="6165a-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="6165a-357">Vyzkoušet odhlašování zadáním  **&lt;div&gt;**  a stisknutím klávesy **KARTĚ** dvakrát k vložení úplná **div** značky.</span><span class="sxs-lookup"><span data-stu-id="6165a-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="6165a-358">Úloha 2 – Přidání šablony stylů CSS</span><span class="sxs-lookup"><span data-stu-id="6165a-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="6165a-359">Šablona prázdný projekt obsahuje soubor velmi efektivní šablon stylů CSS, který obsahuje pouze styly slouží k zobrazení základní formulářů a ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="6165a-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="6165a-360">Chcete-li vylepšení vzhledu a chování webu budete používat další šablon stylů CSS a bitové kopie (potenciálně poskytované návrháře).</span><span class="sxs-lookup"><span data-stu-id="6165a-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="6165a-361">V této úloze budete přidávat šablonu stylů CSS k definování stylů webu.</span><span class="sxs-lookup"><span data-stu-id="6165a-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="6165a-362">Soubor CSS a image jsou součástí **Source\Assets\Content** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="6165a-363">Chcete-li přidat je do aplikace, přetáhněte obsah z **Průzkumníka Windows** okno na **Průzkumníku řešení** ve Visual Studio Express pro Web, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6165a-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="6165a-364">![Přetahování styl obsah](aspnet-mvc-4-fundamentals/_static/image12.png "přetahování styl obsah")</span><span class="sxs-lookup"><span data-stu-id="6165a-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="6165a-365">*Přetahování styl obsah*</span><span class="sxs-lookup"><span data-stu-id="6165a-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="6165a-366">Dialogové okno upozornění se zobrazí, žádostí o potvrzení nahradit **Site.css** soubor a některé existujících bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="6165a-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="6165a-367">Zkontrolujte **použít pro všechny položky** a klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6165a-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="6165a-368">Úloha 3 – Přidání zobrazit šablonu</span><span class="sxs-lookup"><span data-stu-id="6165a-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="6165a-369">V této úloze budete přidávat šablony zobrazení pro generování odpovědi HTML, který bude používat rozložení stránky předlohy a šablon stylů CSS přidat v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="6165a-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="6165a-370">Pro šablonu zobrazení procházení na domovskou stránku, budete nejprve muset označuje, že místo vrací řetězec, **HomeController Index** metoda vrátí **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="6165a-371">Otevřete **HomeController** třídy a změňte jeho **Index** metodu pro návrat **ActionResult**, a mějte ho vrátit **View()**.</span><span class="sxs-lookup"><span data-stu-id="6165a-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="6165a-372">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="6165a-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="6165a-373">Teď je potřeba přidat šablonu odpovídající zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="6165a-374">K tomu, **klikněte pravým tlačítkem na** uvnitř **Index** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="6165a-375">Tím se otevře **přidat zobrazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6165a-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="6165a-376">![Přidání zobrazení z v rámci metody Index](aspnet-mvc-4-fundamentals/_static/image13.png "přidávání zobrazení v aplikaci Index – metoda")</span><span class="sxs-lookup"><span data-stu-id="6165a-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="6165a-377">*Přidání zobrazení v aplikaci Index – metoda*</span><span class="sxs-lookup"><span data-stu-id="6165a-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="6165a-378">**Přidat zobrazení** ke generování souboru šablony zobrazení zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6165a-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="6165a-379">Ve výchozím nastavení toto dialogové okno předem vyplní název šablony zobrazení tak, aby odpovídala metody akce, která bude používat.</span><span class="sxs-lookup"><span data-stu-id="6165a-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="6165a-380">Protože jste použili **přidat zobrazení** místní nabídky v rámci **Index** metody akce v rámci HomeController, **přidat zobrazení** dialogové okno má Index jako výchozí název zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="6165a-381">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-381">Click **Add**.</span></span>

    <span data-ttu-id="6165a-382">![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image14.png "Přidat Dialog zobrazení")</span><span class="sxs-lookup"><span data-stu-id="6165a-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="6165a-383">*Přidat Dialog zobrazení*</span><span class="sxs-lookup"><span data-stu-id="6165a-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="6165a-384">Visual Studio generuje **Index.cshtml** zobrazit šablonu uvnitř **Views\Home** složku a pak ho otevře.</span><span class="sxs-lookup"><span data-stu-id="6165a-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="6165a-385">![Domácí Index zobrazení vytvořené](aspnet-mvc-4-fundamentals/_static/image15.png "Domů Index zobrazení vytvořené")</span><span class="sxs-lookup"><span data-stu-id="6165a-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="6165a-386">*Domácí Index zobrazení vytvořené*</span><span class="sxs-lookup"><span data-stu-id="6165a-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-387">název a umístění **Index.cshtml** soubor je relevantní a dodržuje konvence pojmenování výchozí rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6165a-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="6165a-388">Složka \Views\**Domů** odpovídá názvu kontroleru (**Domů** řadič).</span><span class="sxs-lookup"><span data-stu-id="6165a-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="6165a-389">Název šablony zobrazení (**Index**), odpovídá metoda akce kontroleru, který bude zobrazit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="6165a-390">Tímto způsobem, ASP.NET MVC zabraňuje nutnosti explicitně určovat název nebo umístění zobrazení šablony při použití této zásady vytváření názvů lze vrátit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="6165a-391">Je na základě vygenerované šablony zobrazení  **\_layout.cshtml** dříve definované šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="6165a-392">Aktualizovat vlastnosti ViewBag.Title **Domů**a změnit hlavní obsahu **Toto je domovská stránka**, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="6165a-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="6165a-393">Vyberte **MvcMusicStore** projekt v Průzkumníku řešení a stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="6165a-394">Úloha 4: ověření</span><span class="sxs-lookup"><span data-stu-id="6165a-394">Task 4: Verification</span></span>

<span data-ttu-id="6165a-395">Chcete-li ověřit, že jste správně provedli všechny kroky v předchozím cvičení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6165a-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="6165a-396">S aplikací v prohlížeči otevřít by měl Pamatujte, že:</span><span class="sxs-lookup"><span data-stu-id="6165a-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="6165a-397">Metoda akce indexu HomeController najít a zobrazit **\Views\Home\Index.cshtml** zobrazit šablony, i když kód volá **vrátit View()**, protože zobrazit šablonu a potom standardní zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="6165a-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="6165a-398">Domovská stránka zobrazí zobrazení uvítací zprávy, které jsou definované v rámci **\Views\Home\Index.cshtml** zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="6165a-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="6165a-399">Domovská stránka používá  **\_layout.cshtml** šablony, a proto zobrazení uvítací zprávy je obsažena v rozložení standardní webu HTML.</span><span class="sxs-lookup"><span data-stu-id="6165a-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="6165a-400">![Domácí zobrazení Index pomocí definované LayoutPage a stylu](aspnet-mvc-4-fundamentals/_static/image16.png "Domů zobrazení Index pomocí definované LayoutPage a stylu")</span><span class="sxs-lookup"><span data-stu-id="6165a-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="6165a-401">*Domácí zobrazení Index pomocí definované LayoutPage a stylu*</span><span class="sxs-lookup"><span data-stu-id="6165a-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="6165a-402">Cvičení 5: Vytvoření modelu zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="6165a-403">Pokud jste provedli v zobrazeních Zobrazit pevně zakódované HTML, ale, aby bylo možné vytvářet dynamické webové aplikace, zobrazit šablonu získat informace z řadiče.</span><span class="sxs-lookup"><span data-stu-id="6165a-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="6165a-404">Jednou z běžných technik má být použit pro tento účel je **ViewModel** vzor, který umožňuje Kontroleru Zabalit všechny informace potřebné ke generování odpovídající odpověď protokolu HTML.</span><span class="sxs-lookup"><span data-stu-id="6165a-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="6165a-405">V tomto cvičení se dozvíte, jak vytvořit třídu ViewModel a přidat požadované vlastnosti: počet žánry v úložišti a seznam těchto žánry.</span><span class="sxs-lookup"><span data-stu-id="6165a-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="6165a-406">Je také aktualizovat StoreController používat vytvořený ViewModel a nakonec vytvoříte novou šablonu zobrazení, která se zobrazí vlastnosti uvedených na stránce.</span><span class="sxs-lookup"><span data-stu-id="6165a-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="6165a-407">Úloha 1 – Vytvoření třídy ViewModel</span><span class="sxs-lookup"><span data-stu-id="6165a-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="6165a-408">V této úloze vytvoříte ViewModel třídu, která bude implementovat scénář výpis genre úložiště.</span><span class="sxs-lookup"><span data-stu-id="6165a-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="6165a-409">Pokud už otevřený, spusťte **VS Express pro Web**.</span><span class="sxs-lookup"><span data-stu-id="6165a-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="6165a-410">V **soubor** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="6165a-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6165a-411">V dialogovém okně Otevřít projekt přejděte do **Source\Ex05 CreatingAViewModel\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="6165a-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6165a-412">Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="6165a-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="6165a-413">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6165a-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6165a-414">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6165a-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6165a-415">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="6165a-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6165a-416">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-417">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6165a-418">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6165a-419">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6165a-420">Vytvoření **ViewModels** složku pro uložení ViewModel.</span><span class="sxs-lookup"><span data-stu-id="6165a-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="6165a-421">Chcete-li to provést, klikněte pravým tlačítkem nejvyšší úrovně **MvcMusicStore** projekt, vyberte **přidat** a potom **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="6165a-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="6165a-422">![Přidání do nové složky](aspnet-mvc-4-fundamentals/_static/image17.png "přidání do nové složky")</span><span class="sxs-lookup"><span data-stu-id="6165a-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="6165a-423">*Přidání do nové složky*</span><span class="sxs-lookup"><span data-stu-id="6165a-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="6165a-424">Název složky *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="6165a-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="6165a-425">![ViewModels složky v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels složky v Průzkumníku řešení")</span><span class="sxs-lookup"><span data-stu-id="6165a-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="6165a-426">*ViewModels složky v Průzkumníku řešení*</span><span class="sxs-lookup"><span data-stu-id="6165a-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="6165a-427">Vytvoření **ViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="6165a-428">Chcete-li to provést, klikněte pravým tlačítkem na **ViewModels** složku nedávno vytvořili, vyberte **přidat** a potom **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="6165a-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="6165a-429">V části **kód**, vyberte **– třída** položky a název souboru *StoreIndexViewModel.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="6165a-430">![Přidání nové třídy](aspnet-mvc-4-fundamentals/_static/image19.png "přidání nové třídy")</span><span class="sxs-lookup"><span data-stu-id="6165a-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="6165a-431">*Přidání nové třídy*</span><span class="sxs-lookup"><span data-stu-id="6165a-431">*Adding a new Class*</span></span>

    <span data-ttu-id="6165a-432">![Vytvoření třídy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "vytváření StoreIndexViewModel – třída")</span><span class="sxs-lookup"><span data-stu-id="6165a-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="6165a-433">*Vytvoření třídy StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="6165a-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="6165a-434">Úloha 2 – Přidání vlastnosti do třídy ViewModel</span><span class="sxs-lookup"><span data-stu-id="6165a-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="6165a-435">Existují dva parametry mají být předány z StoreController zobrazit šablonu Pokud chcete generovat očekávané odpovědi HTML: počet žánry v úložišti a seznam těchto žánry.</span><span class="sxs-lookup"><span data-stu-id="6165a-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="6165a-436">V této úloze přidáte tyto 2 vlastnosti, které chcete **StoreIndexViewModel** – třída: **NumberOfGenres** (celé číslo) a **žánry** (seznam řetězců).</span><span class="sxs-lookup"><span data-stu-id="6165a-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="6165a-437">Přidat **NumberOfGenres** a **žánry** vlastnosti, které chcete **StoreIndexViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="6165a-438">Chcete-li to provést, přidejte následující řádky 2 v definici třídy:</span><span class="sxs-lookup"><span data-stu-id="6165a-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="6165a-439">(Code fragment kódu - *ASP.NET MVC 4 základy – vlastnosti Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="6165a-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="6165a-440">**{Získat; nastavit;}**  zápis využívá jazyka C# na funkce automaticky implementované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6165a-440">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="6165a-441">Poskytuje výhody vlastnosti bez nutnosti nám základní pole deklarovat.</span><span class="sxs-lookup"><span data-stu-id="6165a-441">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="6165a-442">Úloha 3 - aktualizace StoreController používat StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="6165a-442">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="6165a-443">**StoreIndexViewModel** třída zapouzdří informace potřebné k předání z **StoreController**na **Index** metodu zobrazit šablonu, aby bylo možné generovat odpověď .</span><span class="sxs-lookup"><span data-stu-id="6165a-443">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="6165a-444">V této úloze budete aktualizovat **StoreController** používat **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="6165a-444">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="6165a-445">Otevřete **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-445">Open **StoreController** class.</span></span>

    <span data-ttu-id="6165a-446">![Otevírání StoreController třída](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otevírání – třída")</span><span class="sxs-lookup"><span data-stu-id="6165a-446">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="6165a-447">*Otevírání StoreController – třída*</span><span class="sxs-lookup"><span data-stu-id="6165a-447">*Opening StoreController class*</span></span>
2. <span data-ttu-id="6165a-448">Chcete-li použít **StoreIndexViewModel** třídy z **StoreController**, přidat následující obor názvů v horní části **StoreController** kódu:</span><span class="sxs-lookup"><span data-stu-id="6165a-448">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="6165a-449">(Code fragment kódu - *ASP.NET MVC 4 základy - Ex5 StoreIndexViewModel pomocí ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="6165a-449">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="6165a-450">Změna **StoreController**na **Index** metody akce, které se vytvoří a naplní **StoreIndexViewModel** objektu a předává je pro šablonu zobrazení generování odpovědi HTML s ním.</span><span class="sxs-lookup"><span data-stu-id="6165a-450">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-451">V testovacím prostředí &quot;modely ASP.NET MVC a přístup k datům&quot; budete psát kód, který načte seznam žánry úložiště z databáze.</span><span class="sxs-lookup"><span data-stu-id="6165a-451">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="6165a-452">V následujícím kódu vytvoříte **seznamu** z žánry fiktivní data, která se naplní **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="6165a-452">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="6165a-453">Po vytvoření a nastavení **StoreIndexViewModel** objektu, bude předáno jako argument pro **zobrazení** metoda.</span><span class="sxs-lookup"><span data-stu-id="6165a-453">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="6165a-454">To znamená, že zobrazit šablonu použije k vygenerování odpovědi HTML s ním tento objekt.</span><span class="sxs-lookup"><span data-stu-id="6165a-454">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="6165a-455">Nahraďte **Index** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-455">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="6165a-456">(Code fragment kódu - *ASP.NET MVC 4 základy – metoda Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="6165a-456">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="6165a-457">Pokud jste obeznámeni s C#, může předpokládat to pomocí **var** znamená, že **viewModel** proměnná je pozdní vazbu.</span><span class="sxs-lookup"><span data-stu-id="6165a-457">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="6165a-458">Není správný – kompilátor jazyka C# používá odvození typu podle přiřadit proměnnou k určení, který **viewModel** je typu **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="6165a-458">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="6165a-459">Také pomocí kompilování místní **viewModel** jako proměnnou **StoreIndexViewModel** typ kontrolu kompilaci get a Visual Studio – podpora editor kódu.</span><span class="sxs-lookup"><span data-stu-id="6165a-459">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="6165a-460">Úloha 4 – vytvoření šablony zobrazení, která používá StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="6165a-460">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="6165a-461">V této úloze vytvoříte šablonu zobrazení, která se použije k zobrazení seznamu žánry objekt StoreIndexViewModel předán z řadiče.</span><span class="sxs-lookup"><span data-stu-id="6165a-461">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="6165a-462">Před vytvořením nové šablony zobrazení, Vytvořme projekt tak, aby **Přidat Dialog zobrazení** zná **StoreIndexViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-462">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="6165a-463">Sestavení projektu výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="6165a-463">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="6165a-464">![Sestavení projektu](aspnet-mvc-4-fundamentals/_static/image22.png "sestavení projektu")</span><span class="sxs-lookup"><span data-stu-id="6165a-464">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="6165a-465">*Sestavení projektu*</span><span class="sxs-lookup"><span data-stu-id="6165a-465">*Building the project*</span></span>
2. <span data-ttu-id="6165a-466">Vytvořte novou šablonu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-466">Create a new View template.</span></span> <span data-ttu-id="6165a-467">Chcete-li provést, klikněte pravým tlačítkem uvnitř **Index** metoda a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-467">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="6165a-468">![Přidání zobrazení](aspnet-mvc-4-fundamentals/_static/image23.png "přidávání zobrazení")</span><span class="sxs-lookup"><span data-stu-id="6165a-468">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="6165a-469">*Přidání zobrazení*</span><span class="sxs-lookup"><span data-stu-id="6165a-469">*Adding a View*</span></span>
3. <span data-ttu-id="6165a-470">Protože **Přidat Dialog zobrazení** byl vyvolán z **StoreController**, přidá šablony zobrazení ve výchozím nastavení v **\Views\Store\Index.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="6165a-470">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="6165a-471">Zkontrolujte **vytvoření důrazně zadali zobrazení** zaškrtávací políčko a potom vyberte **StoreIndexViewModel** jako **třída modelu**.</span><span class="sxs-lookup"><span data-stu-id="6165a-471">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="6165a-472">Taky se ujistěte, že je modul zobrazení vybrané **Razor**.</span><span class="sxs-lookup"><span data-stu-id="6165a-472">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="6165a-473">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-473">Click **Add**.</span></span>

    <span data-ttu-id="6165a-474">![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image24.png "Přidat Dialog zobrazení")</span><span class="sxs-lookup"><span data-stu-id="6165a-474">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="6165a-475">*Přidat Dialog zobrazení*</span><span class="sxs-lookup"><span data-stu-id="6165a-475">*Add View Dialog*</span></span>

    <span data-ttu-id="6165a-476">**\Views\Store\Index.cshtml** je vytvořen a otevřít soubor šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-476">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="6165a-477">Podle informací uvedených na **přidat zobrazení** dialogové okno v posledním kroku, zobrazení, bude očekávat šablony **StoreIndexViewModel** instance jako data sloužící ke generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="6165a-477">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="6165a-478">Si všimnete, že šablona dědí `ViewPage<musicstore.viewmodels.storeindexviewmodel>` v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="6165a-478">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="6165a-479">Úloha 5: aktualizace zobrazení šablony</span><span class="sxs-lookup"><span data-stu-id="6165a-479">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="6165a-480">V této úloze aktualizujte zobrazení šablony vytvořené v posledním úkolem načíst počet žánry a jejich názvy v rámci dané stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-480">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="6165a-481">Budete používat @ syntaxe (často označované jako &quot;code útržky&quot;) ke spouštění kódu v rámci šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-481">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="6165a-482">V **Index.cshtml** v souboru **úložiště** složky, jeho kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-482">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="6165a-483">Jakmile dokončíte zadáním období za slovo **modelu**, Visual Studio Intellisense zobrazí seznam možných vlastnosti a metody, které lze vybírat.</span><span class="sxs-lookup"><span data-stu-id="6165a-483">As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.</span></span>
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > <span data-ttu-id="6165a-484">*Získávání modelu vlastnosti a metody pomocí sady Visual Studio IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="6165a-484">*Getting Model properties and methods with Visual Studio's IntelliSense*</span></span>
    > 
    > <span data-ttu-id="6165a-485">**Modelu** vlastnost odkazy **StoreIndexViewModel** objekt, který řadičem předána do zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-485">The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template.</span></span> <span data-ttu-id="6165a-486">To znamená, že máte přístup všechna data z řadiče předaný zobrazit šablonu prostřednictvím **modelu** vlastnost a naformátujte ho do odpovídající HTML odpověď v rámci šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-486">This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.</span></span>
    > 
    > <span data-ttu-id="6165a-487">Můžete vybrat jen **NumberOfGenres** vlastnost z Intellisense seznam místo zadání ho a pak se bude automatického dokončování je stisknutím klávesy **tabulátor**.</span><span class="sxs-lookup"><span data-stu-id="6165a-487">You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.</span></span>
2. <span data-ttu-id="6165a-488">Smyčky přes seznamu genre v **StoreIndexViewModel** a vytvořit HTML  **&lt;ul&gt;**  seznamu pomocí **foreach** smyčky.</span><span class="sxs-lookup"><span data-stu-id="6165a-488">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
<span data-ttu-id="6165a-489">(C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-489">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="6165a-490">Stiskněte klávesu **F5** ke spuštění aplikace a Procházet **/úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6165a-490">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="6165a-491">Zobrazí se seznam žánry předaná **StoreIndexViewModel** objektu z **StoreController** do zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-491">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="6165a-492">![Zobrazení s přehledem žánry](aspnet-mvc-4-fundamentals/_static/image26.png "zobrazení s přehledem žánry")</span><span class="sxs-lookup"><span data-stu-id="6165a-492">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="6165a-493">*Zobrazení s přehledem žánry*</span><span class="sxs-lookup"><span data-stu-id="6165a-493">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="6165a-494">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6165a-494">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="6165a-495">Cvičení 6: Pomocí parametrů v zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-495">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="6165a-496">Cvičení 3 jste zjistili, jak předat parametry do Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6165a-496">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="6165a-497">V tomto cvičení se dozvíte, jak používat tyto parametry v šabloně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-497">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="6165a-498">K tomuto účelu můžete budou zavedeny nejprve do třídy modelu, které vám pomohou při správě logika dat a domény.</span><span class="sxs-lookup"><span data-stu-id="6165a-498">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="6165a-499">Kromě toho se dozvíte, jak vytvořit odkazy na stránky v aplikaci ASP.NET MVC bez obav věcí, jako kódování cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-499">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="6165a-500">Úloha 1 – přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="6165a-500">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="6165a-501">Na rozdíl od ViewModels, které vytváří jenom k předání informací z řadiče zobrazení, jsou vytvořené třídy modelu obsahovat a spravovat data a domény logiku.</span><span class="sxs-lookup"><span data-stu-id="6165a-501">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="6165a-502">V této úloze budete přidávat dvě třídy modelu představují tyto koncepty: **Genre** a **Album**.</span><span class="sxs-lookup"><span data-stu-id="6165a-502">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="6165a-503">Pokud už otevřený, spusťte **VS Express pro Web**</span><span class="sxs-lookup"><span data-stu-id="6165a-503">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="6165a-504">V **soubor** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="6165a-504">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="6165a-505">V dialogovém okně Otevřít projekt přejděte do **Source\Ex06 UsingParametersInView\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="6165a-505">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="6165a-506">Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="6165a-506">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="6165a-507">Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6165a-507">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6165a-508">Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6165a-508">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6165a-509">V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="6165a-509">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="6165a-510">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-510">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-511">Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-511">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6165a-512">Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="6165a-512">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6165a-513">Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-513">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6165a-514">Přidat **Genre** třída modelu.</span><span class="sxs-lookup"><span data-stu-id="6165a-514">Add a **Genre** Model class.</span></span> <span data-ttu-id="6165a-515">Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**, vyberte **přidat** a potom **nová položka** možnost.</span><span class="sxs-lookup"><span data-stu-id="6165a-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="6165a-516">V části **kód**, vyberte **– třída** položky a název souboru *Genre.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-516">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="6165a-517">![Přidání třídy](aspnet-mvc-4-fundamentals/_static/image27.png "přidání třídy")</span><span class="sxs-lookup"><span data-stu-id="6165a-517">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="6165a-518">*Přidání nové položky*</span><span class="sxs-lookup"><span data-stu-id="6165a-518">*Adding a new item*</span></span>

    <span data-ttu-id="6165a-519">![Přidání třídy modelu Genre](aspnet-mvc-4-fundamentals/_static/image28.png "přidejte třídu modelu Genre")</span><span class="sxs-lookup"><span data-stu-id="6165a-519">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="6165a-520">*Přidání třídy modelu Genre*</span><span class="sxs-lookup"><span data-stu-id="6165a-520">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="6165a-521">Přidat **název** vlastnost k třídě Genre.</span><span class="sxs-lookup"><span data-stu-id="6165a-521">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="6165a-522">Chcete-li to provést, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="6165a-522">To do this, add the following code:</span></span>

    <span data-ttu-id="6165a-523">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="6165a-523">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="6165a-524">Stejný postup jako předtím, přidejte **Album** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-524">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="6165a-525">Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**, vyberte **přidat** a potom **nová položka** možnost.</span><span class="sxs-lookup"><span data-stu-id="6165a-525">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="6165a-526">V části **kód**, vyberte **– třída** položky a název souboru *Album.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-526">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="6165a-527">Přidejte dvě vlastnosti do třídy alb: **Genre** a **název**.</span><span class="sxs-lookup"><span data-stu-id="6165a-527">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="6165a-528">Chcete-li to provést, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="6165a-528">To do this, add the following code:</span></span>

    <span data-ttu-id="6165a-529">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="6165a-529">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="6165a-530">Úloha 2 – přidání StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="6165a-530">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="6165a-531">A **StoreBrowseViewModel** se použije k zobrazení alb, které odpovídají vybrané Genre v této úloze.</span><span class="sxs-lookup"><span data-stu-id="6165a-531">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="6165a-532">V této úloze, vytvoříte této třídy a pak přidejte dvě vlastnosti pro zpracování **Genre** a jeho **Album**je seznam.</span><span class="sxs-lookup"><span data-stu-id="6165a-532">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="6165a-533">Přidat **StoreBrowseViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-533">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="6165a-534">Chcete-li to provést, klikněte pravým tlačítkem **ViewModels** složky v **Průzkumníku řešení**, vyberte **přidat** a potom **nová položka** možnost.</span><span class="sxs-lookup"><span data-stu-id="6165a-534">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="6165a-535">V části **kód**, vyberte **– třída** položky a název souboru *StoreBrowseViewModel.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-535">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="6165a-536">Přidat odkaz na modely v **StoreBrowseViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-536">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="6165a-537">Chcete-li to provést, přidejte následující pomocí oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="6165a-537">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="6165a-538">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="6165a-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="6165a-539">Přidat dvě vlastnosti do **StoreBrowseViewModel** – třída: **Genre** a **alb**.</span><span class="sxs-lookup"><span data-stu-id="6165a-539">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="6165a-540">Chcete-li to provést, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="6165a-540">To do this, add the following code:</span></span>

    <span data-ttu-id="6165a-541">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="6165a-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="6165a-542">Co je **seznamu&lt;Album&gt;**  ?: používá tuto definici **seznamu&lt;T&gt;**  typu, kde **T** omezí typ, na které elementy tohoto **seznamu** patří, v takovém případě **Album** (nebo některého z jejich potomků).</span><span class="sxs-lookup"><span data-stu-id="6165a-542">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
    > 
    > <span data-ttu-id="6165a-543">Tato schopnost třídy a metody, které odložení specifikace jeden nebo více typů, dokud třída nebo metoda je deklarován a vytvoření instance kódem na straně klienta je funkce jazyka C# návrhu názvem **obecné typy**.</span><span class="sxs-lookup"><span data-stu-id="6165a-543">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
    > 
    > <span data-ttu-id="6165a-544">**Seznam&lt;T&gt;**  je obecný ekvivalent **ArrayList** zadejte a je k dispozici v **System.Collections.Generic** obor názvů.</span><span class="sxs-lookup"><span data-stu-id="6165a-544">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="6165a-545">Jednou z výhod použití **obecné typy** je, že vzhledem k tomu, že je zadaný typ, není nutné provádět kontroly operací, jako je přetypování elementy do typu **Album** jako by se **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="6165a-545">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="6165a-546">Úloha 3 – v nové ViewModel StoreController</span><span class="sxs-lookup"><span data-stu-id="6165a-546">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="6165a-547">V této úloze budete upravovat **StoreController**na **Procházet** a **podrobnosti** metody akce použití nového **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="6165a-547">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="6165a-548">Přidat odkaz na složku modely v **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-548">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="6165a-549">Chcete-li to provést, rozbalte **řadiče** složky v **Průzkumníku řešení** a otevřete **StoreController** – třída.</span><span class="sxs-lookup"><span data-stu-id="6165a-549">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="6165a-550">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="6165a-550">Then add the following code:</span></span>

    <span data-ttu-id="6165a-551">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="6165a-551">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="6165a-552">Nahraďte **Procházet** metoda akce se má použít **StoreViewBrowseController** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-552">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="6165a-553">Vytvoříte Genre a dvě nové objekty alb s fiktivní dat (v testovacím prostředí další Hands-on vám bude spotřebovávat reálná data z databáze).</span><span class="sxs-lookup"><span data-stu-id="6165a-553">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="6165a-554">Chcete-li to provést, nahraďte **Procházet** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-554">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="6165a-555">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="6165a-555">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="6165a-556">Nahraďte **podrobnosti** metoda akce se má použít **StoreViewBrowseController** třídy.</span><span class="sxs-lookup"><span data-stu-id="6165a-556">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="6165a-557">Vytvoří nový **Album** objekt, který má být vrácen **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-557">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="6165a-558">Chcete-li to provést, nahraďte **podrobnosti** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6165a-558">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="6165a-559">(Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="6165a-559">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="6165a-560">Úloha 4 – přidání šablonu Procházet zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-560">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="6165a-561">V této úloze budete přidávat **Procházet** zobrazení zobrazíte alb pro konkrétní Genre nalezen.</span><span class="sxs-lookup"><span data-stu-id="6165a-561">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="6165a-562">Před vytvořením nové šablony zobrazení, měli byste vytvořit projekt tak, aby **přidat zobrazení** dialogové okno, že zná **ViewModel** třídu se má použít.</span><span class="sxs-lookup"><span data-stu-id="6165a-562">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="6165a-563">Sestavení projektu výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="6165a-563">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="6165a-564">Přidat **Procházet** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-564">Add a **Browse** View.</span></span> <span data-ttu-id="6165a-565">Chcete-li to provést, klikněte pravým tlačítkem v **Procházet** metody akce **StoreController** a klikněte na tlačítko **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-565">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="6165a-566">V **přidat zobrazení** dialogové okno pole, ověřte, zda je název zobrazení **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="6165a-566">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="6165a-567">Zkontrolujte **vytvořit zobrazení silného typu** zaškrtávací políčko a vyberte **StoreBrowseViewModel** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="6165a-567">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="6165a-568">V ostatních polích nechte výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6165a-568">Leave the other fields with their default value.</span></span> <span data-ttu-id="6165a-569">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-569">Then click **Add**.</span></span>

    <span data-ttu-id="6165a-570">![Přidání zobrazení Procházet](aspnet-mvc-4-fundamentals/_static/image29.png "přidání Procházet zobrazení")</span><span class="sxs-lookup"><span data-stu-id="6165a-570">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="6165a-571">*Přidání zobrazení procházení*</span><span class="sxs-lookup"><span data-stu-id="6165a-571">*Adding a Browse View*</span></span>
4. <span data-ttu-id="6165a-572">Změnit **Browse.cshtml** pro zobrazení informací Genre, přístup k **StoreBrowseViewModel** objekt, který je předán zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-572">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="6165a-573">K tomu, nahradí obsah následujícím kódem: (C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-573">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="6165a-574">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-574">Task 5 - Running the Application</span></span>

<span data-ttu-id="6165a-575">V této úloze budete testovat, **Procházet** metoda načte alb z **Procházet** metoda akce.</span><span class="sxs-lookup"><span data-stu-id="6165a-575">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="6165a-576">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-576">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-577">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="6165a-577">The project starts in the Home page.</span></span> <span data-ttu-id="6165a-578">Změňte adresu URL na   **/ukládání/Procházet? Genre = Disco** k ověření, že akce vrátí dvě alb.</span><span class="sxs-lookup"><span data-stu-id="6165a-578">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="6165a-579">![Procházení úložiště Disco alb](aspnet-mvc-4-fundamentals/_static/image30.png "procházení Disco alb úložiště")</span><span class="sxs-lookup"><span data-stu-id="6165a-579">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="6165a-580">*Procházení Disco alb úložiště*</span><span class="sxs-lookup"><span data-stu-id="6165a-580">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="6165a-581">Úloha 6 - zobrazení informace o konkrétní Album</span><span class="sxs-lookup"><span data-stu-id="6165a-581">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="6165a-582">V této úloze budete implementovat **úložiště/podrobnosti** zobrazení zobrazíte informace o konkrétní alb.</span><span class="sxs-lookup"><span data-stu-id="6165a-582">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="6165a-583">V tomto testovacím prostředí Hands-on všechno, co se zobrazí o alba je už obsažený v **zobrazení** šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-583">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="6165a-584">Ano, místo vytvoření **StoreDetailsViewModel** třída, budete používat aktuální **StoreBrowseViewModel** předáním alba šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-584">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="6165a-585">Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-585">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="6165a-586">Přidejte nový **podrobnosti** zobrazit **StoreController**na **podrobnosti** metody akce.</span><span class="sxs-lookup"><span data-stu-id="6165a-586">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="6165a-587">Chcete-li to provést, klikněte pravým tlačítkem **podrobnosti** metoda v **StoreController** třídu a klikněte na tlačítko **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-587">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="6165a-588">V **přidat zobrazení** dialogové okno, ověřte, zda **název zobrazení** je **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="6165a-588">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="6165a-589">Zkontrolujte **vytvořit zobrazení silného typu** zaškrtávací políčko a vyberte **Album** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="6165a-589">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="6165a-590">V ostatních polích nechte výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6165a-590">Leave the other fields with their default value.</span></span> <span data-ttu-id="6165a-591">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-591">Then click **Add**.</span></span> <span data-ttu-id="6165a-592">Tato akce vytvoří a otevře **\Views\Store\Details.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="6165a-592">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="6165a-593">![Přidání zobrazení podrobností o](aspnet-mvc-4-fundamentals/_static/image31.png "přidávání zobrazení podrobností")</span><span class="sxs-lookup"><span data-stu-id="6165a-593">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="6165a-594">*Přidání zobrazení podrobností*</span><span class="sxs-lookup"><span data-stu-id="6165a-594">*Adding a Details View*</span></span>
3. <span data-ttu-id="6165a-595">Změnit **Details.cshtml** soubor k zobrazení informací na Album, přístup k **Album** objekt, který je předán zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-595">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="6165a-596">K tomu, nahradí obsah následujícím kódem: (C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-596">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="6165a-597">Úloha 7 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-597">Task 7 - Running the Application</span></span>

<span data-ttu-id="6165a-598">V této úloze budete testovat, **podrobnosti** zobrazení načte informace o alba z **podrobnosti akce** metoda.</span><span class="sxs-lookup"><span data-stu-id="6165a-598">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="6165a-599">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-599">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-600">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-600">The project starts in the **Home** page.</span></span> <span data-ttu-id="6165a-601">Změňte adresu URL na **/Store/Details/5** pro ověření informací o alba.</span><span class="sxs-lookup"><span data-stu-id="6165a-601">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="6165a-602">![Procházení podrobností alb](aspnet-mvc-4-fundamentals/_static/image32.png "procházení podrobností alb")</span><span class="sxs-lookup"><span data-stu-id="6165a-602">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="6165a-603">*Procházení Album podrobností*</span><span class="sxs-lookup"><span data-stu-id="6165a-603">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="6165a-604">Úloha 8 – přidání odkazů mezi stránkami</span><span class="sxs-lookup"><span data-stu-id="6165a-604">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="6165a-605">V této úloze, přidáte pomocí odkazu v zobrazení úložiště tak, aby měl odkaz v každé Genre názvu na příslušné **/úložiště/Procházet** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-605">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="6165a-606">Tímto způsobem, když kliknete na Genre, například **Disco**, bude přejděte na **/úložiště/procházet? genre = Disco** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-606">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="6165a-607">Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-607">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="6165a-608">Aktualizace **Index** stránku přidáte odkaz **Procházet** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-608">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="6165a-609">Chcete-li to provést, v **Průzkumníku řešení** rozbalte **zobrazení** složku, pak se **úložiště** složku a dvojím kliknutím **Index.cshtml** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-609">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="6165a-610">Přidáte odkaz na zobrazení pro procházení označující genre vybrané.</span><span class="sxs-lookup"><span data-stu-id="6165a-610">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="6165a-611">K tomuto účelu nahradit následující zvýrazněný kód v rámci  **&lt;li&gt;**  značky: (C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-611">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="6165a-612">jiná možnost by propojení přímo na stránku s kódem takto:</span><span class="sxs-lookup"><span data-stu-id="6165a-612">another approach would be linking directly to the page, with a code like the following:</span></span>
    > 
    > <span data-ttu-id="6165a-613">&lt;href =&quot;/úložiště/procházet? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="6165a-613">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
    > 
    > <span data-ttu-id="6165a-614">I když tento přístup funguje, závisí na řetězci pevně zakódované.</span><span class="sxs-lookup"><span data-stu-id="6165a-614">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="6165a-615">Pokud přejmenujete později Kontroleru, je nutné tento pokyn ručně změnit.</span><span class="sxs-lookup"><span data-stu-id="6165a-615">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="6165a-616">Lepší alternativou je použít **pomocné rutiny HTML** metoda.</span><span class="sxs-lookup"><span data-stu-id="6165a-616">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="6165a-617">ASP.NET MVC zahrnuje metodu pomocné rutiny HTML, která je k dispozici pro takové úlohy.</span><span class="sxs-lookup"><span data-stu-id="6165a-617">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="6165a-618">**Html.ActionLink()** Pomocná metoda usnadňuje sestavení HTML  **&lt;&gt;**  odkazy, a ujistěte se, cest URL jsou správně kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-618">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
    > 
    > <span data-ttu-id="6165a-619">Htlm.ActionLink má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="6165a-619">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="6165a-620">V tomto cvičení budete používat ten, který přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="6165a-620">In this exercise you will use one that takes three parameters:</span></span>
    > 
    > 1. <span data-ttu-id="6165a-621">Text odkazu, který se zobrazí název Genre</span><span class="sxs-lookup"><span data-stu-id="6165a-621">Link text, which will display the Genre name</span></span>
    > 2. <span data-ttu-id="6165a-622">Název akce kontroleru (**Procházet**)</span><span class="sxs-lookup"><span data-stu-id="6165a-622">Controller action name (**Browse**)</span></span>
    > 3. <span data-ttu-id="6165a-623">Hodnoty parametru, zadat jak název trasy (**Genre**) a hodnotu (**Genre název**)</span><span class="sxs-lookup"><span data-stu-id="6165a-623">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="6165a-624">Úloha 9 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-624">Task 9 - Running the Application</span></span>

<span data-ttu-id="6165a-625">V této úloze budete testovat, zobrazí se každý Genre s odkazem na příslušné **/úložiště/Procházet** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-625">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="6165a-626">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-626">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-627">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="6165a-627">The project starts in the Home page.</span></span> <span data-ttu-id="6165a-628">Změňte adresu URL na **/úložiště** k ověření, že každý Genre odkazy na příslušné **/úložiště/Procházet** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6165a-628">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="6165a-629">![Procházení žánry s odkazy na stránce procházení](aspnet-mvc-4-fundamentals/_static/image33.png "procházení žánry s odkazy na stránce procházení")</span><span class="sxs-lookup"><span data-stu-id="6165a-629">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="6165a-630">*Procházení žánry s odkazy na stránce procházení*</span><span class="sxs-lookup"><span data-stu-id="6165a-630">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="6165a-631">Úlohu 10 - pomocí dynamické ViewModel kolekce předávání hodnot</span><span class="sxs-lookup"><span data-stu-id="6165a-631">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="6165a-632">V této úloze se dozvíte jednoduchá ale účinná metoda předat hodnoty mezi Kontrolerem a zobrazení bez jakýchkoli změn v modelu.</span><span class="sxs-lookup"><span data-stu-id="6165a-632">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="6165a-633">ASP.NET MVC 4 poskytuje kolekci &quot;ViewModel&quot;, což může být přiřazena na jakoukoli hodnotu dynamické a získat přístup k uvnitř řadiče a také zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-633">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="6165a-634">Teď použijete dynamické kolekce položek ViewBag předat seznam &quot; **Starred žánry** &quot; z řadiče zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6165a-634">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="6165a-635">Zobrazení Index úložiště bude přístup k **ViewModel** a zobrazení informací.</span><span class="sxs-lookup"><span data-stu-id="6165a-635">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="6165a-636">Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-636">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="6165a-637">Otevřete **StoreController.cs** a upravovat **Index** metodu pro vytvoření seznamu starred žánry do kolekce ViewModel:</span><span class="sxs-lookup"><span data-stu-id="6165a-637">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="6165a-638">Můžete také použít syntaxi **ViewBag [&quot;Starred&quot;]** pro přístup k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6165a-638">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="6165a-639">Na ikonu hvězdičky  **&quot;starred.png&quot;**  je součástí **Source\Assets\Images** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="6165a-639">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="6165a-640">Chcete-li přidat ji do aplikace, přetáhněte obsah z **Průzkumníka Windows** okno na **Průzkumníku řešení** v aplikaci Visual Web Developer Express, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6165a-640">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="6165a-641">![Přidání hvězdičkami bitovou kopii do řešení](aspnet-mvc-4-fundamentals/_static/image34.png "přidání hvězdičkami bitovou kopii do řešení")</span><span class="sxs-lookup"><span data-stu-id="6165a-641">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="6165a-642">*Přidávání hvězdičkami bitové kopie do řešení*</span><span class="sxs-lookup"><span data-stu-id="6165a-642">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="6165a-643">Otevření zobrazení **Store/Index.cshtml** a změnit obsah.</span><span class="sxs-lookup"><span data-stu-id="6165a-643">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="6165a-644">Budete ke čtení &quot;starred&quot; vlastnost **ViewBag** kolekce a požádejte, pokud aktuální genre název je v seznamu.</span><span class="sxs-lookup"><span data-stu-id="6165a-644">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="6165a-645">V takovém případě se zobrazí genre odkaz přímo na ikonu hvězdičky.</span><span class="sxs-lookup"><span data-stu-id="6165a-645">In that case you will show a star icon right to the genre link.</span></span>
<span data-ttu-id="6165a-646">(C#)</span><span class="sxs-lookup"><span data-stu-id="6165a-646">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="6165a-647">Úloha 11 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-647">Task 11 - Running the Application</span></span>

<span data-ttu-id="6165a-648">V této úloze budete testovat, označený hvězdičkou žánry zobrazit ikonu hvězdičky.</span><span class="sxs-lookup"><span data-stu-id="6165a-648">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="6165a-649">Stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-649">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="6165a-650">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-650">The project starts in the **Home** page.</span></span> <span data-ttu-id="6165a-651">Změňte adresu URL na **/úložiště** ověřte, že každé vybrané genre má pečlivě štítku:</span><span class="sxs-lookup"><span data-stu-id="6165a-651">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="6165a-652">![Procházení žánry označený hvězdičkou elementy](aspnet-mvc-4-fundamentals/_static/image35.png "procházení žánry označený hvězdičkou elementy")</span><span class="sxs-lookup"><span data-stu-id="6165a-652">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="6165a-653">*Procházení žánry označený hvězdičkou elementy*</span><span class="sxs-lookup"><span data-stu-id="6165a-653">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="6165a-654">Cvičení 7: Okruhu kolem nové šablony ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6165a-654">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="6165a-655">V tomto cvičení zaměříte vylepšení v šablonách projektu ASP.NET MVC 4, trvá podívejte se na nejdůležitější funkce nové šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-655">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="6165a-656">Úloha 1: Zkoumat šablony ASP.NET MVC 4 Internetové aplikace</span><span class="sxs-lookup"><span data-stu-id="6165a-656">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="6165a-657">Pokud už otevřený, spusťte **VS Express pro Web**</span><span class="sxs-lookup"><span data-stu-id="6165a-657">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="6165a-658">Vyberte **souboru | Nové | Projekt** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="6165a-658">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="6165a-659">V **nový projekt** dialogovém okně, vyberte **Visual C# | Webové** šablony v levém podokně stromu a vyberte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="6165a-659">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="6165a-660">**Název** projektu *MusicStore* a aktualizovat **název řešení** k *začít*, pak vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6165a-660">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="6165a-661">![Vytvoření nového projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "vytvoření nového projektu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6165a-661">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="6165a-662">*Vytvoření nového projektu ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="6165a-662">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="6165a-663">V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **Internetové aplikace** šablona projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6165a-663">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="6165a-664">Všimněte si, že jste vybrali jako zobrazovací modul Razor nebo ASPX.</span><span class="sxs-lookup"><span data-stu-id="6165a-664">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="6165a-665">![Vytvoření nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "vytvoření nové aplikace ASP.NET MVC 4 Internetu")</span><span class="sxs-lookup"><span data-stu-id="6165a-665">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="6165a-666">*Vytvoření nové aplikace ASP.NET MVC 4 Internetu*</span><span class="sxs-lookup"><span data-stu-id="6165a-666">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-667">Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="6165a-667">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="6165a-668">Jeho cílem je minimalizovat počet znaků a stisknutí kláves požadované v souboru, povolení fast a kapaliny kódování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6165a-668">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="6165a-669">Syntaxe Razor využívá existující C# / VB. (nebo jiné) znalosti jazyka a doručí syntaxi značek šablony, která umožňuje pracovním postupu vytváření Super HTML.</span><span class="sxs-lookup"><span data-stu-id="6165a-669">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="6165a-670">Stiskněte klávesu **F5** a spuštění řešení najdete v části obnoveného šablony.</span><span class="sxs-lookup"><span data-stu-id="6165a-670">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="6165a-671">Můžete zkontrolovat na následující funkce:</span><span class="sxs-lookup"><span data-stu-id="6165a-671">You can check out the following features:</span></span>

    1. <span data-ttu-id="6165a-672">**Styl moderních šablony**</span><span class="sxs-lookup"><span data-stu-id="6165a-672">**Modern-style templates**</span></span>

        <span data-ttu-id="6165a-673">Byla obnovena šablony, poskytuje další styly moderních vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="6165a-673">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="6165a-674">![Šablony MVC ASP.NET 4 přepracován tak](aspnet-mvc-4-fundamentals/_static/image38.png "přepracován tak šablony ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6165a-674">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="6165a-675">*ASP.NET MVC 4 přepracován tak šablony*</span><span class="sxs-lookup"><span data-stu-id="6165a-675">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="6165a-676">**Adaptivního vykreslování**</span><span class="sxs-lookup"><span data-stu-id="6165a-676">**Adaptive Rendering**</span></span>

        <span data-ttu-id="6165a-677">Podívejte se na změnu velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobení novou velikost okna.</span><span class="sxs-lookup"><span data-stu-id="6165a-677">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="6165a-678">Tyto šablony pomocí technik adaptivního vykreslování stolní počítače a mobilní platformy bez nutnosti přizpůsobení řádně vykreslit.</span><span class="sxs-lookup"><span data-stu-id="6165a-678">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="6165a-679">![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](aspnet-mvc-4-fundamentals/_static/image39.png "šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")</span><span class="sxs-lookup"><span data-stu-id="6165a-679">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="6165a-680">*Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*</span><span class="sxs-lookup"><span data-stu-id="6165a-680">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="6165a-681">Zavřete prohlížeč zastavení ladicího programu a vrátíte se k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-681">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="6165a-682">Nyní budete moci zkoumat řešení a podívejte se na některé z nových funkcí, které jsou zavedené službou ASP.NET MVC 4 v šabloně projektů.</span><span class="sxs-lookup"><span data-stu-id="6165a-682">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="6165a-683">![ASP.NET MVC4-internet aplikace--šablona projektu](aspnet-mvc-4-fundamentals/_static/image40.png "šablona projektu ASP.NET MVC 4 Internetové aplikace")</span><span class="sxs-lookup"><span data-stu-id="6165a-683">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="6165a-684">*Šablona projektu ASP.NET MVC 4 Internetové aplikace*</span><span class="sxs-lookup"><span data-stu-id="6165a-684">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    1. <span data-ttu-id="6165a-685">**Kód jazyka HTML5**</span><span class="sxs-lookup"><span data-stu-id="6165a-685">**HTML5 markup**</span></span>

        <span data-ttu-id="6165a-686">Procházet šablony zobrazení a zjistěte, nové značky motiv, který je třeba otevřít **About.cshtml** zobrazit v rámci **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="6165a-686">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

        <span data-ttu-id="6165a-687">![Nové šablony, pomocí syntaxe Razor a HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "novou šablonu, pomocí syntaxe Razor a HTML5 značek")</span><span class="sxs-lookup"><span data-stu-id="6165a-687">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

        <span data-ttu-id="6165a-688">*Nové šablony, pomocí syntaxe Razor a HTML5 značek*</span><span class="sxs-lookup"><span data-stu-id="6165a-688">*New template, using Razor and HTML5 markup*</span></span>
    2. <span data-ttu-id="6165a-689">**Zahrnuté knihoven jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6165a-689">**JavaScript libraries included**</span></span>

        1. <span data-ttu-id="6165a-690">**jQuery**: jQuery zjednodušuje procházení dokumentu HTML, zpracování událostí, animace a interakce Ajax.</span><span class="sxs-lookup"><span data-stu-id="6165a-690">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
        2. <span data-ttu-id="6165a-691">**jQuery UI**: Tato knihovna nabízí abstrakci pro nízké úrovně interakce a animace, pokročilé efekty a bylo pomůcky, nástavbou jQuery JavaScript Library.</span><span class="sxs-lookup"><span data-stu-id="6165a-691">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

            > [!NOTE]
            > <span data-ttu-id="6165a-692">Informace o jQuery a kalendáře jQuery UI najdete v [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="6165a-692">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
        3. <span data-ttu-id="6165a-693">**Kódem KnockoutJS**: výchozí šablonu ASP.NET MVC 4 nyní zahrnuje **kódem KnockoutJS**, rozhraní MVVM JavaScript rozhraní, které umožňuje vytvářet bohaté a vysoce přizpůsobivém webových aplikací pomocí jazyka JavaScript a HTML.</span><span class="sxs-lookup"><span data-stu-id="6165a-693">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="6165a-694">Jako v architektuře ASP.NET MVC 3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6165a-694">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

            > [!NOTE]
            > <span data-ttu-id="6165a-695">Můžete získat další informace o knihovně kódem KnockOutJS v tento odkaz: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="6165a-695">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
        4. <span data-ttu-id="6165a-696">**Modernizr**: tuto knihovnu pracuje automaticky, která váš web kompatibilní s starší prohlížeče, při použití jazyka HTML5 a CSS3 technologie.</span><span class="sxs-lookup"><span data-stu-id="6165a-696">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

            > [!NOTE]
            > <span data-ttu-id="6165a-697">Můžete získat další informace o knihovně Modernizr v tento odkaz: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="6165a-697">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
    3. <span data-ttu-id="6165a-698">**SimpleMembership součástí řešení**</span><span class="sxs-lookup"><span data-stu-id="6165a-698">**SimpleMembership included in the solution**</span></span>

        <span data-ttu-id="6165a-699">SimpleMembership jsou určeny jako náhrada za předchozí systému zprostředkovatele členství a Role technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6165a-699">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="6165a-700">Obsahuje mnoho nových funkcí, které bylo snazší pro vývojáře na zabezpečené webové stránky flexibilnější způsobem.</span><span class="sxs-lookup"><span data-stu-id="6165a-700">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

        <span data-ttu-id="6165a-701">Šablona Internet již má nastavit pár věcí integrovat SimpleMembership, například AccountController připravena k použití OAuthWebSecurity (pro registraci účtu OAuth, přihlášení, správu atd.) a zabezpečení webového.</span><span class="sxs-lookup"><span data-stu-id="6165a-701">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

        <span data-ttu-id="6165a-702">![SimpleMembership součástí řešení](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership součástí řešení")</span><span class="sxs-lookup"><span data-stu-id="6165a-702">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

        <span data-ttu-id="6165a-703">*SimpleMembership součástí řešení*</span><span class="sxs-lookup"><span data-stu-id="6165a-703">*SimpleMembership Included in the solution*</span></span>

        > [!NOTE]
        > <span data-ttu-id="6165a-704">Najít další informace o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="6165a-704">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="6165a-705">Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="6165a-705">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6165a-706">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6165a-706">Summary</span></span>

<span data-ttu-id="6165a-707">Provedením tohoto testovacího prostředí Hands-On jste se naučili základy ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="6165a-707">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="6165a-708">Základní prvky aplikace MVC a jejich vzájemné interakce</span><span class="sxs-lookup"><span data-stu-id="6165a-708">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="6165a-709">Jak vytvořit aplikaci ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6165a-709">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="6165a-710">Jak přidávat a konfigurovat řadičům pracovat s parametry předána adresu URL a řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="6165a-710">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="6165a-711">Postup přidání rozložení stránky předlohy nastavit šablonu pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování a šablonu zobrazení na Zobrazit obsah HTML</span><span class="sxs-lookup"><span data-stu-id="6165a-711">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="6165a-712">Postup použití vzoru ViewModel pro předávání vlastností šablony zobrazení pro zobrazení dynamické informace</span><span class="sxs-lookup"><span data-stu-id="6165a-712">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="6165a-713">Jak používat parametry předané do řadičů v šabloně zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-713">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="6165a-714">Postup přidání odkazů na stránky v aplikaci ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6165a-714">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="6165a-715">Jak přidat a používat dynamické vlastnosti v zobrazení</span><span class="sxs-lookup"><span data-stu-id="6165a-715">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="6165a-716">Vylepšení v šablonách projektu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6165a-716">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6165a-717">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="6165a-717">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6165a-718">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="6165a-718">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6165a-719">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="6165a-719">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6165a-720">Přejděte na [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="6165a-720">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6165a-721">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="6165a-721">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="6165a-722">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-722">Click on **Install Now**.</span></span> <span data-ttu-id="6165a-723">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="6165a-723">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6165a-724">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="6165a-724">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6165a-725">![Nainstalovat Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="6165a-725">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6165a-726">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="6165a-726">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6165a-727">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="6165a-727">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="6165a-729">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="6165a-729">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6165a-730">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="6165a-730">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="6165a-732">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="6165a-732">*Installation progress*</span></span>
6. <span data-ttu-id="6165a-733">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6165a-733">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="6165a-735">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="6165a-735">*Installation completed*</span></span>
7. <span data-ttu-id="6165a-736">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="6165a-736">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6165a-737">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="6165a-737">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="6165a-739">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="6165a-739">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6165a-740">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="6165a-740">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="6165a-741">Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="6165a-741">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="6165a-742">Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6165a-742">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="6165a-743">Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="6165a-743">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-744">S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="6165a-744">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="6165a-745">Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="6165a-745">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="6165a-746">![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6165a-746">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="6165a-747">*Přihlaste se k Windows Azure Management Portal*</span><span class="sxs-lookup"><span data-stu-id="6165a-747">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="6165a-748">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="6165a-748">Click **New** on the command bar.</span></span>

    <span data-ttu-id="6165a-749">![Vytvoření nového webu](aspnet-mvc-4-fundamentals/_static/image49.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="6165a-749">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="6165a-750">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="6165a-750">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="6165a-751">Klikněte na tlačítko **výpočetní** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="6165a-751">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="6165a-752">Potom vyberte **rychle vytvořit** možnost.</span><span class="sxs-lookup"><span data-stu-id="6165a-752">Then select **Quick Create** option.</span></span> <span data-ttu-id="6165a-753">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.</span><span class="sxs-lookup"><span data-stu-id="6165a-753">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-754">Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6165a-754">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="6165a-755">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="6165a-755">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="6165a-756">Postup pro nastavení databáze neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="6165a-756">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="6165a-757">![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-fundamentals/_static/image50.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="6165a-757">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="6165a-758">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="6165a-758">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="6165a-759">Počkejte, dokud nové **webu** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="6165a-759">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="6165a-760">Po vytvoření webu klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="6165a-760">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="6165a-761">Zkontrolujte, zda je funkční nový web.</span><span class="sxs-lookup"><span data-stu-id="6165a-761">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="6165a-762">![Procházení na nový web](aspnet-mvc-4-fundamentals/_static/image51.png "procházení na nový web")</span><span class="sxs-lookup"><span data-stu-id="6165a-762">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="6165a-763">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="6165a-763">*Browsing to the new web site*</span></span>

    <span data-ttu-id="6165a-764">![Webový server spuštěn](aspnet-mvc-4-fundamentals/_static/image52.png "webu systémem")</span><span class="sxs-lookup"><span data-stu-id="6165a-764">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="6165a-765">*Spuštění webu*</span><span class="sxs-lookup"><span data-stu-id="6165a-765">*Web site running*</span></span>
6. <span data-ttu-id="6165a-766">Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="6165a-766">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="6165a-767">![Otevření stránky Správa webu](aspnet-mvc-4-fundamentals/_static/image53.png "otevření stránek správu webového serveru")</span><span class="sxs-lookup"><span data-stu-id="6165a-767">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="6165a-768">*Otevření stránek správu webového serveru*</span><span class="sxs-lookup"><span data-stu-id="6165a-768">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="6165a-769">V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6165a-769">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-770">*Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-770">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="6165a-771">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena.</span><span class="sxs-lookup"><span data-stu-id="6165a-771">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="6165a-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="6165a-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="6165a-773">![Na webu stažení profilu publikování](aspnet-mvc-4-fundamentals/_static/image54.png "stahování webové stránky profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="6165a-773">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="6165a-774">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="6165a-774">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="6165a-775">Stáhněte si soubor profil publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="6165a-775">Download the publish profile file to a known location.</span></span> <span data-ttu-id="6165a-776">Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6165a-776">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="6165a-777">![Ukládání souboru profilu publikování](aspnet-mvc-4-fundamentals/_static/image55.png "ukládání profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="6165a-777">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="6165a-778">*Ukládání souboru profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="6165a-778">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="6165a-779">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="6165a-779">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="6165a-780">Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server.</span><span class="sxs-lookup"><span data-stu-id="6165a-780">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="6165a-781">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="6165a-781">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="6165a-782">Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="6165a-782">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="6165a-783">Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="6165a-783">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="6165a-784">Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="6165a-784">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="6165a-785">Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="6165a-785">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="6165a-786">Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="6165a-786">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="6165a-787">![Řídicí panel serveru databáze SQL](aspnet-mvc-4-fundamentals/_static/image56.png "řídicího panelu serveru databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="6165a-787">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="6165a-788">*Řídicí panel serveru databáze SQL*</span><span class="sxs-lookup"><span data-stu-id="6165a-788">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="6165a-789">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="6165a-789">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="6165a-790">To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6165a-790">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Přidávání IP adresy klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="6165a-792">*Přidávání IP adresy klienta*</span><span class="sxs-lookup"><span data-stu-id="6165a-792">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="6165a-793">Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="6165a-793">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="6165a-795">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="6165a-795">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6165a-796">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="6165a-796">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="6165a-797">Přejděte zpět na ASP.NET MVC 4 řešení.</span><span class="sxs-lookup"><span data-stu-id="6165a-797">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="6165a-798">V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-798">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="6165a-799">![Publikování aplikace](aspnet-mvc-4-fundamentals/_static/image60.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="6165a-799">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="6165a-800">*Publikování webu*</span><span class="sxs-lookup"><span data-stu-id="6165a-800">*Publishing the web site*</span></span>
2. <span data-ttu-id="6165a-801">Umožňuje naimportujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="6165a-801">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="6165a-802">![Import profilu publikování](aspnet-mvc-4-fundamentals/_static/image61.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="6165a-802">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="6165a-803">*Import profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="6165a-803">*Importing publish profile*</span></span>
3. <span data-ttu-id="6165a-804">Klikněte na tlačítko **ověření připojení**.</span><span class="sxs-lookup"><span data-stu-id="6165a-804">Click **Validate Connection**.</span></span> <span data-ttu-id="6165a-805">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6165a-805">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6165a-806">Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="6165a-806">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="6165a-807">![Ověření připojení](aspnet-mvc-4-fundamentals/_static/image62.png "ověřování připojení")</span><span class="sxs-lookup"><span data-stu-id="6165a-807">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="6165a-808">*Ověření připojení*</span><span class="sxs-lookup"><span data-stu-id="6165a-808">*Validating connection*</span></span>
4. <span data-ttu-id="6165a-809">V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="6165a-809">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="6165a-810">![Konfigurace nasazení webu](aspnet-mvc-4-fundamentals/_static/image63.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="6165a-810">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="6165a-811">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="6165a-811">*Web deploy configuration*</span></span>
5. <span data-ttu-id="6165a-812">Připojení k databázi nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6165a-812">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="6165a-813">V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="6165a-813">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="6165a-814">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="6165a-814">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="6165a-815">V **heslo** zadejte přihlašovací heslo správce serveru.</span><span class="sxs-lookup"><span data-stu-id="6165a-815">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="6165a-816">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="6165a-816">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="6165a-817">![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-fundamentals/_static/image64.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="6165a-817">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="6165a-818">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="6165a-818">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="6165a-819">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6165a-819">Then click **OK**.</span></span> <span data-ttu-id="6165a-820">Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6165a-820">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="6165a-821">![Vytvoření databáze](aspnet-mvc-4-fundamentals/_static/image65.png "vytváření řetězec databáze")</span><span class="sxs-lookup"><span data-stu-id="6165a-821">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="6165a-822">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="6165a-822">*Creating the database*</span></span>
7. <span data-ttu-id="6165a-823">Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="6165a-823">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="6165a-824">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6165a-824">Then click **Next**.</span></span>

    <span data-ttu-id="6165a-825">![Připojovací řetězec odkazující na databázi SQL](aspnet-mvc-4-fundamentals/_static/image66.png "připojovací řetězec odkazující na databázi SQL")</span><span class="sxs-lookup"><span data-stu-id="6165a-825">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="6165a-826">*Připojovací řetězec odkazující na databázi SQL*</span><span class="sxs-lookup"><span data-stu-id="6165a-826">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="6165a-827">V **Preview** klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6165a-827">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="6165a-828">![Publikování webové aplikace](aspnet-mvc-4-fundamentals/_static/image67.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="6165a-828">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="6165a-829">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="6165a-829">*Publishing the web application*</span></span>
9. <span data-ttu-id="6165a-830">Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="6165a-830">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="6165a-831">![Aplikace publikována do služby Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikace publikována do služby Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6165a-831">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="6165a-832">*Aplikace, které jsou publikovány do služby Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="6165a-832">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="6165a-833">Příloha C: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="6165a-833">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="6165a-834">S fragmenty kódu máte všechny kód, který je nutné na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="6165a-834">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="6165a-835">Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="6165a-835">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="6165a-836">![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-fundamentals/_static/image69.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="6165a-836">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="6165a-837">*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="6165a-837">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="6165a-838">***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***</span><span class="sxs-lookup"><span data-stu-id="6165a-838">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="6165a-839">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="6165a-839">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="6165a-840">Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).</span><span class="sxs-lookup"><span data-stu-id="6165a-840">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="6165a-841">Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.</span><span class="sxs-lookup"><span data-stu-id="6165a-841">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="6165a-842">Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).</span><span class="sxs-lookup"><span data-stu-id="6165a-842">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="6165a-843">Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="6165a-843">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="6165a-844">![Začněte psát název fragmentu](aspnet-mvc-4-fundamentals/_static/image70.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="6165a-844">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="6165a-845">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="6165a-845">*Start typing the snippet name*</span></span>

<span data-ttu-id="6165a-846">![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-fundamentals/_static/image71.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="6165a-846">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="6165a-847">*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="6165a-847">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="6165a-848">![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-fundamentals/_static/image72.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")</span><span class="sxs-lookup"><span data-stu-id="6165a-848">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="6165a-849">*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*</span><span class="sxs-lookup"><span data-stu-id="6165a-849">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="6165a-850">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="6165a-850">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="6165a-851">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="6165a-851">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="6165a-852">Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="6165a-852">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="6165a-853">Vyberte relevantní fragment kódu ze seznamu, kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="6165a-853">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="6165a-854">![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-fundamentals/_static/image73.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="6165a-854">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="6165a-855">*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="6165a-855">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="6165a-856">![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-fundamentals/_static/image74.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="6165a-856">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="6165a-857">*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="6165a-857">*Pick the relevant snippet from the list, by clicking on it*</span></span>
