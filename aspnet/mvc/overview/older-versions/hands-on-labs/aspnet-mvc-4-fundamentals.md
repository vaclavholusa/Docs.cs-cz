---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 – základy | Dokumentace Microsoftu
author: rick-anderson
description: Tohoto praktického testovacího prostředí je založena na MVC (Model View Controller) Music Store kurz aplikace, která představuje a najdete podrobné pokyny ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386572"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="90264-103">ASP.NET MVC 4 – základy</span><span class="sxs-lookup"><span data-stu-id="90264-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="90264-104">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="90264-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="90264-105">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="90264-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="90264-106">Tohoto praktického testovacího prostředí je založena na MVC (Model View Controller) Music Store aplikace, která představuje a vysvětluje krok za krokem, jak pomocí technologie ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="90264-107">V rámci testovacího prostředí se dozvíte, jednoduchost, ještě výkonu společně použití těchto technologií.</span><span class="sxs-lookup"><span data-stu-id="90264-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="90264-108">Začne fungovat s jednoduchou aplikaci a budou sestavte ho, dokud máte plně funkční technologie ASP.NET MVC 4 webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90264-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="90264-109">Toto testovací prostředí funguje s ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="90264-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="90264-110">Pokud chcete prozkoumat verze technologie ASP.NET MVC 3 kurz aplikace, najdete ho v [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="90264-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="90264-111">Tohoto praktického testovacího prostředí se předpokládá, že vývojář má prostředí v oblasti technologií vývoje webových, jako je HTML a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="90264-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="90264-112">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="90264-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="90264-113">Projekt specifické pro toto testovací prostředí je k dispozici na [základy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="90264-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="90264-114">Aplikace Music Store</span><span class="sxs-lookup"><span data-stu-id="90264-114">The Music Store application</span></span>

<span data-ttu-id="90264-115">Webová aplikace Music Store, které budou vytvořeny v rámci tohoto testovacího prostředí zahrnuje tři hlavní části: nákupní, registrace a správa.</span><span class="sxs-lookup"><span data-stu-id="90264-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="90264-116">Uživatelé budou moci procházet podle žánru alb, přidat alb jejich košíku, zkontrolujte jejich výběr a nakonec jich přešlo k platbě přihlásit se a dokončete pořadí.</span><span class="sxs-lookup"><span data-stu-id="90264-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="90264-117">Kromě toho úložiště správci budou moct spravovat dostupné alb, jakož i jejich hlavní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="90264-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="90264-118">![Obrazovky Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "obrazovky Music Store")</span><span class="sxs-lookup"><span data-stu-id="90264-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="90264-119">*Obrazovky Music Store*</span><span class="sxs-lookup"><span data-stu-id="90264-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="90264-120">Základy ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="90264-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="90264-121">Aplikace Music Store bude vytvořen pomocí **řadiče MVC (Model View)**, schéma architektury, který rozděluje aplikace do tří hlavních součástí:</span><span class="sxs-lookup"><span data-stu-id="90264-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="90264-122">**Modely**: objekty modelů jsou části aplikace, které implementují logiku domény.</span><span class="sxs-lookup"><span data-stu-id="90264-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="90264-123">Objekty modelů často, také načíst a ukládají stav modelu v databázi.</span><span class="sxs-lookup"><span data-stu-id="90264-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="90264-124">**Zobrazení:** zobrazení jsou komponenty, které zobrazují aplikace uživatelského rozhraní (UI).</span><span class="sxs-lookup"><span data-stu-id="90264-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="90264-125">Standardně toto uživatelské rozhraní je vytvořena z dat modelu.</span><span class="sxs-lookup"><span data-stu-id="90264-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="90264-126">Příkladem může být zobrazení pro úpravy alb, které zobrazuje textová pole a rozevírací seznam na základě aktuálního stavu objektu alba.</span><span class="sxs-lookup"><span data-stu-id="90264-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="90264-127">**Řadiče:** Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracovat s modelem a konečně vybírají vykreslené uživatelské rozhraní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="90264-128">V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.</span><span class="sxs-lookup"><span data-stu-id="90264-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="90264-129">Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy.</span><span class="sxs-lookup"><span data-stu-id="90264-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="90264-130">Tato separace pomáhá zvládnout složitost při sestavování aplikace, protože umožňuje vám umožní zaměřit se na jeden aspekt jejich implementace v čase.</span><span class="sxs-lookup"><span data-stu-id="90264-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="90264-131">Kromě toho vzor MVC usnadňuje testování aplikací také podporují použití vývoj řízený testováním (TDD) pro vytváření aplikací.</span><span class="sxs-lookup"><span data-stu-id="90264-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="90264-132">**ASP.NET MVC** framework představuje alternativu ke vzoru webových formulářů ASP.NET pro vytvoření webové aplikace založené na ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="90264-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="90264-133">**ASP.NET MVC** framework je jednoduchý, s možností intenzivního testování prezentační platforma, která (stejně jako u aplikací na základě webových formulářů) je integrovaná s stávajících funkcí technologie ASP.NET, jako je například stránky předlohy a na základě členství ověřování, takže získáte všechny funkce rozhraní .NET core.</span><span class="sxs-lookup"><span data-stu-id="90264-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="90264-134">To je užitečné, pokud jste už obeznámení s webovými formuláři ASP.NET protože všech knihoven, které už používáte jsou také k dispozici v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="90264-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="90264-135">Kromě toho volné párování mezi třemi hlavními komponentami architektury MVC rovněž podporuje paralelní vývoj.</span><span class="sxs-lookup"><span data-stu-id="90264-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="90264-136">Například jeden vývojář může pracovat na zobrazení, druhý může pracovat na logice kontroleru a třetí se můžete soustředit na obchodní logiku v modelu.</span><span class="sxs-lookup"><span data-stu-id="90264-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="90264-137">Cíle</span><span class="sxs-lookup"><span data-stu-id="90264-137">Objectives</span></span>

<span data-ttu-id="90264-138">V tomto praktického testovacího prostředí se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="90264-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="90264-139">Vytvoření aplikace ASP.NET MVC zcela podle kurzu aplikace Music Store</span><span class="sxs-lookup"><span data-stu-id="90264-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="90264-140">Přidat Kontrolery pro zpracování adresy URL na domovskou stránku webu a jeho hlavní funkce procházení</span><span class="sxs-lookup"><span data-stu-id="90264-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="90264-141">Přidání zobrazení upravit obsah zobrazený spolu s jeho styl</span><span class="sxs-lookup"><span data-stu-id="90264-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="90264-142">Přidání třídy modelu obsahovat a spravovat data a domény logiku</span><span class="sxs-lookup"><span data-stu-id="90264-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="90264-143">Použití vzoru modelu zobrazení k předávání informací z akce kontroleru zobrazení šablon</span><span class="sxs-lookup"><span data-stu-id="90264-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="90264-144">Prozkoumejte nové šablony ASP.NET MVC 4 pro internetové aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="90264-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90264-145">Prerequisites</span></span>

<span data-ttu-id="90264-146">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="90264-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="90264-147">[Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat)</span><span class="sxs-lookup"><span data-stu-id="90264-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="90264-148">Instalace</span><span class="sxs-lookup"><span data-stu-id="90264-148">Setup</span></span>

<span data-ttu-id="90264-149">**Instalace fragmenty kódu**</span><span class="sxs-lookup"><span data-stu-id="90264-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="90264-150">Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="90264-151">K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.</span><span class="sxs-lookup"><span data-stu-id="90264-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="90264-152">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="90264-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="90264-153">Cvičení</span><span class="sxs-lookup"><span data-stu-id="90264-153">Exercises</span></span>

<span data-ttu-id="90264-154">Podle následující praktická cvičení se skládá tohoto praktického testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="90264-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="90264-155">Cvičení 1: Vytvoření projektu webové aplikace MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90264-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="90264-156">Cvičení 2: Vytvoření Kontroleru</span><span class="sxs-lookup"><span data-stu-id="90264-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="90264-157">Cvičení 3: Předání parametrů do Kontroleru</span><span class="sxs-lookup"><span data-stu-id="90264-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="90264-158">Cvičení 4: Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="90264-159">Cvičení 5: Vytvoření modelu zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="90264-160">Cvičení 6: Použití parametrů v zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="90264-161">Cvičení 7: Kolečko okolo nové šablony ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="90264-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="90264-162">Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="90264-163">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="90264-164">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="90264-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="90264-165">Cvičení 1: Vytvoření projektu webové aplikace MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90264-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="90264-166">V tomto cvičení se dozvíte, jak vytvořit aplikaci ASP.NET MVC v sadě Visual Studio 2012 Express pro Web, stejně jako jeho hlavní složky organizace.</span><span class="sxs-lookup"><span data-stu-id="90264-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="90264-167">Kromě toho se dozvíte, jak přidat nový kontroler a nastavte ji zobrazit na domovské stránce aplikace jednoduchým řetězcem.</span><span class="sxs-lookup"><span data-stu-id="90264-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="90264-168">Úloha 1 – Vytvoření projektu webové aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90264-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="90264-169">V této úloze vytvoříte prázdný projekt aplikace ASP.NET MVC pomocí šablony MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="90264-170">Spustit **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="90264-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="90264-171">Na **souboru** nabídky, klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="90264-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="90264-172">V **nový projekt** dialogové okno Vyberte **webové aplikace ASP.NET MVC 4** typ nachází v rámci projektu **Visual C#,** **webové** šablony seznam.</span><span class="sxs-lookup"><span data-stu-id="90264-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="90264-173">Změnit **název** k *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="90264-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="90264-174">Nastavit umístění řešení uvnitř nové **začít** složky v tomto cvičení zdrojové složky, například **[YOUR-Mezidobí-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="90264-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="90264-175">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="90264-175">Click **OK**.</span></span>

    <span data-ttu-id="90264-176">![Vytvoření dialogového okna Nový projekt](aspnet-mvc-4-fundamentals/_static/image2.png "vytvořit dialogové okno Nový projekt")</span><span class="sxs-lookup"><span data-stu-id="90264-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="90264-177">*Vytvoření dialogového okna Nový projekt*</span><span class="sxs-lookup"><span data-stu-id="90264-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="90264-178">V **nového projektu ASP.NET MVC 4** dialogové okno Vyberte **základní** šablony a ujistěte se, že **zobrazovací modul** vybraného je **Razor**.</span><span class="sxs-lookup"><span data-stu-id="90264-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="90264-179">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="90264-179">Click **OK**.</span></span>

    <span data-ttu-id="90264-180">![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-fundamentals/_static/image3.png "nové pole dialogové okno projektu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="90264-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="90264-181">*Nové pole dialogové okno projektu ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="90264-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="90264-182">Úloha 2 – zkoumání struktury řešení</span><span class="sxs-lookup"><span data-stu-id="90264-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="90264-183">Architektura ASP.NET MVC zahrnuje šablony projektu sady Visual Studio, který vám pomůže vytvořit webové aplikace, které podporují vzor MVC.</span><span class="sxs-lookup"><span data-stu-id="90264-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="90264-184">Tato šablona vytvoří novou aplikaci ASP.NET MVC Web s požadované složky, šablony položek a položek konfigurace souboru.</span><span class="sxs-lookup"><span data-stu-id="90264-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="90264-185">V této úloze se zaměřuje na rozumět elementům sady, které se podílejí struktury řešení a jejich vztahy.</span><span class="sxs-lookup"><span data-stu-id="90264-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="90264-186">Následující složky jsou součástí všechny aplikace ASP.NET MVC, protože používá rozhraní ASP.NET MVC ve výchozím nastavení &quot;konvence nad konfigurací&quot; přístup a provede některé předpoklady výchozí podle pojmenování složky konvence.</span><span class="sxs-lookup"><span data-stu-id="90264-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="90264-187">Jakmile se vytvoří projekt, zkontrolujte strukturu složek, který byl vytvořen v Průzkumníku řešení na pravé straně:</span><span class="sxs-lookup"><span data-stu-id="90264-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="90264-188">![Struktura složek ASP.NET MVC v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image4.png "strukturu složek ASP.NET MVC v Průzkumníku řešení")</span><span class="sxs-lookup"><span data-stu-id="90264-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="90264-189">*Struktura složek ASP.NET MVC v Průzkumníku řešení*</span><span class="sxs-lookup"><span data-stu-id="90264-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="90264-190">**Kontrolery**.</span><span class="sxs-lookup"><span data-stu-id="90264-190">**Controllers**.</span></span> <span data-ttu-id="90264-191">Tato složka bude obsahovat třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="90264-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="90264-192">V aplikaci MVC na základě řadiče jsou zodpovědná za zpracování interakce s koncovým uživatelem, manipulace s modelem a nakonec výběrem zobrazení k vykreslení uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="90264-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="90264-193">Rozhraní MVC požaduje názvy všech řadičů bude končit &quot;řadič&quot;– například HomeController LoginController či ProductController.</span><span class="sxs-lookup"><span data-stu-id="90264-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="90264-194">**Modely**.</span><span class="sxs-lookup"><span data-stu-id="90264-194">**Models**.</span></span> <span data-ttu-id="90264-195">Tato složka se poskytuje pro třídy, které představují aplikačního modelu pro MVC webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90264-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="90264-196">To obvykle zahrnuje kód, který definuje objekty a logiku pro interakci s úložišti.</span><span class="sxs-lookup"><span data-stu-id="90264-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="90264-197">Obvykle budou pro objekty skutečné model v samostatné třídy knihovny.</span><span class="sxs-lookup"><span data-stu-id="90264-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="90264-198">Ale když vytvoříte novou aplikaci, můžete zahrnout třídy a přesuňte je do samostatné třídy knihovny později v cyklu vývoje.</span><span class="sxs-lookup"><span data-stu-id="90264-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="90264-199">**Zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-199">**Views**.</span></span> <span data-ttu-id="90264-200">Tato složka je doporučené umístění pro zobrazení, součásti za zobrazení uživatelského rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="90264-201">Zobrazení pomocí souborů .aspx, .ascx, .master a cshtml, kromě jiných souborů, které se vztahují k vykreslení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="90264-202">Zobrazení složky obsahuje složku pro každý kontroler; složka je název s předponou názvu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="90264-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="90264-203">Například, pokud máte řadič s názvem **HomeController**, složka zobrazení bude obsahovat složku s názvem Domů.</span><span class="sxs-lookup"><span data-stu-id="90264-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="90264-204">Ve výchozím nastavení, pokud rozhraní ASP.NET MVC načte zobrazení, hledá soubor .aspx s názvem požadované zobrazení ve složce Views\controllerName (**zobrazení [parametr ControllerName] [Action] .aspx**) nebo (**zobrazení [parametr ControllerName] [Akce] .cshtml**) pro zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="90264-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="90264-205">Kromě složky uvedených výše, aplikace MVC Web používá **Global.asax** výchozí soubor nastavení globální směrování adres URL a používá **Web.config** souboru konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="90264-206">Úloha 3 – Přidání HomeController</span><span class="sxs-lookup"><span data-stu-id="90264-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="90264-207">V aplikacích ASP.NET, které nepoužívají rozhraní MVC je interakci s uživatelem uspořádaná kolem stránky a vyvolávání a zpracování událostí z těchto stránek.</span><span class="sxs-lookup"><span data-stu-id="90264-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="90264-208">Interakce uživatele s aplikací ASP.NET MVC se naopak věnuje kontrolerů a jejich metod akcí.</span><span class="sxs-lookup"><span data-stu-id="90264-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="90264-209">Architektura ASP.NET MVC na druhé straně mapuje adresy URL na třídy, které se označují jako řadiče.</span><span class="sxs-lookup"><span data-stu-id="90264-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="90264-210">Řadiče zpracovat příchozí požadavky, zpracování vstupu uživatele a interakce, provést příslušné aplikace logiky a určit odpověď k odeslání zpět do klienta (zobrazení HTML, stáhněte si soubor, přesměrovat na jinou adresu URL, atd.).</span><span class="sxs-lookup"><span data-stu-id="90264-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="90264-211">V případě zobrazení HTML, volá třídu kontroleru obvykle součástí samostatné zobrazení generovat kód HTML pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="90264-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="90264-212">V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.</span><span class="sxs-lookup"><span data-stu-id="90264-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="90264-213">V této úloze přidejte třídu Kontroleru, který bude zpracovávat adresy URL na domovské stránce webu Music Store.</span><span class="sxs-lookup"><span data-stu-id="90264-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="90264-214">Klikněte pravým tlačítkem na **řadiče** složku v Průzkumníku řešení vyberte **přidat** a potom **řadič** příkaz:</span><span class="sxs-lookup"><span data-stu-id="90264-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="90264-215">![Přidání Kontroleru příkazu](aspnet-mvc-4-fundamentals/_static/image5.png "přidat příkaz Kontroleru")</span><span class="sxs-lookup"><span data-stu-id="90264-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="90264-216">*Přidat kontroler – příkaz*</span><span class="sxs-lookup"><span data-stu-id="90264-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="90264-217">**Přidat kontroler** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90264-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="90264-218">Název kontroleru *HomeController* a stiskněte klávesu **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="90264-219">![Přidat kontroler Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "přidat kontroler Dialog")</span><span class="sxs-lookup"><span data-stu-id="90264-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="90264-220">*Přidat Dialog Kontroleru*</span><span class="sxs-lookup"><span data-stu-id="90264-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="90264-221">Soubor **HomeController.cs** se vytvoří v **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="90264-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="90264-222">Abyste měli **HomeController** vrátit řetězec na jeho akce indexu, nahraďte **Index** metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="90264-223">(Fragment - kódu *základy ASP.NET MVC 4 – Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="90264-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="90264-224">Úloha 4 – spouštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="90264-225">V této úloze bude vyzkoušet aplikace ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="90264-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="90264-226">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="90264-227">Projekt je zkompilován a spustí místní webový Server služby IIS.</span><span class="sxs-lookup"><span data-stu-id="90264-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="90264-228">Místní webový Server služby IIS se automaticky otevře webový prohlížeč a přejděte na adresu URL webového serveru.</span><span class="sxs-lookup"><span data-stu-id="90264-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="90264-229">![Aplikace běžící ve webovém prohlížeči](aspnet-mvc-4-fundamentals/_static/image7.png "aplikaci běžící ve webovém prohlížeči")</span><span class="sxs-lookup"><span data-stu-id="90264-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="90264-230">*Aplikace běžící ve webovém prohlížeči*</span><span class="sxs-lookup"><span data-stu-id="90264-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-231">Na místním webovém serveru IIS se spustí na webu na několika náhodný volný port.</span><span class="sxs-lookup"><span data-stu-id="90264-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="90264-232">Na obrázku výše je web spuštěný v `http://localhost:50103/`, takže se používá port 50103.</span><span class="sxs-lookup"><span data-stu-id="90264-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="90264-233">Vaše číslo portu se může lišit.</span><span class="sxs-lookup"><span data-stu-id="90264-233">Your port number may vary.</span></span>
2. <span data-ttu-id="90264-234">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="90264-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="90264-235">Cvičení 2: Vytvoření Kontroleru</span><span class="sxs-lookup"><span data-stu-id="90264-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="90264-236">V tomto cvičení se dozvíte, jak aktualizovat kontroler k implementaci funkcí jednoduchého aplikace Music Store.</span><span class="sxs-lookup"><span data-stu-id="90264-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="90264-237">Tento kontroler budou definovat metody akce pro zpracování každé z následujících konkrétní požadavky:</span><span class="sxs-lookup"><span data-stu-id="90264-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="90264-238">Stránce žánrů Hudba v Music Store</span><span class="sxs-lookup"><span data-stu-id="90264-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="90264-239">Procházet stránku, která obsahuje seznam všech hudebních alb pro konkrétní žánr</span><span class="sxs-lookup"><span data-stu-id="90264-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="90264-240">Stránka s podrobnostmi s informacemi o alba konkrétní Hudba</span><span class="sxs-lookup"><span data-stu-id="90264-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="90264-241">Pro obor v tomto cvičení tyto akce jednoduše vrátí řetězec nyní.</span><span class="sxs-lookup"><span data-stu-id="90264-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="90264-242">Úloha 1 – přidání nové třídy StoreController</span><span class="sxs-lookup"><span data-stu-id="90264-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="90264-243">V této úloze přidáte nový kontroler.</span><span class="sxs-lookup"><span data-stu-id="90264-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="90264-244">Pokud ještě není otevřený, začněte **VS Express for Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="90264-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="90264-245">V **souboru** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="90264-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="90264-246">V dialogovém okně Otevřít projekt, přejděte do **Source\Ex02 CreatingAController\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="90264-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="90264-247">Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="90264-248">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90264-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90264-249">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="90264-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="90264-250">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="90264-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="90264-251">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="90264-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="90264-252">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90264-253">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90264-254">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="90264-255">Přidáte nový kontroler.</span><span class="sxs-lookup"><span data-stu-id="90264-255">Add the new controller.</span></span> <span data-ttu-id="90264-256">Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složku v Průzkumníku řešení vyberte **přidat** a pak **řadič** příkazu.</span><span class="sxs-lookup"><span data-stu-id="90264-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="90264-257">Změnit **názvu Kontroleru** k *StoreController*a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="90264-258">![Přidat kontroler Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "přidat kontroler Dialog")</span><span class="sxs-lookup"><span data-stu-id="90264-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="90264-259">*Přidat Dialog Kontroleru*</span><span class="sxs-lookup"><span data-stu-id="90264-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="90264-260">Úloha 2 - Úprava StoreController akce</span><span class="sxs-lookup"><span data-stu-id="90264-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="90264-261">V této úloze budete upravovat metodách Kontroleru, které jsou volány **akce**.</span><span class="sxs-lookup"><span data-stu-id="90264-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="90264-262">Akce jsou zodpovědná za zpracování žádostí adresy URL a určení obsahu, který by měl být odesílaných zpět do prohlížeče nebo uživatel, který vyvolal adresu URL.</span><span class="sxs-lookup"><span data-stu-id="90264-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="90264-263">**StoreController** třída již má **Index** metody.</span><span class="sxs-lookup"><span data-stu-id="90264-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="90264-264">Použijete ji později v tomto testovacím prostředí k implementaci stránky, která obsahuje seznam všech žánry music store.</span><span class="sxs-lookup"><span data-stu-id="90264-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="90264-265">Teď stačí nahradit **Index** metodu s následujícím kódem, který vrací řetězec &quot;Dobrý den ze Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="90264-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="90264-266">(Fragment - kódu *základy ASP.NET MVC 4 – Ex2 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="90264-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="90264-267">Přidat **Procházet** a **podrobnosti** metody.</span><span class="sxs-lookup"><span data-stu-id="90264-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="90264-268">Chcete-li to provést, přidejte následující kód k **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="90264-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="90264-269">(Fragment - kódu *základy ASP.NET MVC 4 – Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="90264-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="90264-270">Úloha 3 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="90264-271">V této úloze bude vyzkoušet aplikace ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="90264-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="90264-272">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-273">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="90264-274">Změňte adresu URL na ověřit implementaci každou akci.</span><span class="sxs-lookup"><span data-stu-id="90264-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="90264-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="90264-275">**/Store**.</span></span> <span data-ttu-id="90264-276">Zobrazí se  **&quot;Dobrý den ze Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="90264-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="90264-277">**/ Store/procházení**.</span><span class="sxs-lookup"><span data-stu-id="90264-277">**/Store/Browse**.</span></span> <span data-ttu-id="90264-278">Zobrazí se  **&quot;Dobrý den ze Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="90264-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="90264-279">**/ Store/podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="90264-279">**/Store/Details**.</span></span> <span data-ttu-id="90264-280">Zobrazí se  **&quot;Dobrý den ze Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="90264-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="90264-281">![Procházení StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "procházení StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="90264-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="90264-282">*Procházení /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="90264-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="90264-283">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="90264-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="90264-284">Cvičení 3: Předání parametrů do Kontroleru</span><span class="sxs-lookup"><span data-stu-id="90264-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="90264-285">Až doteď mají se vrací konstantní řetězce z řadičů.</span><span class="sxs-lookup"><span data-stu-id="90264-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="90264-286">V tomto cvičení se dozvíte, jak předat parametry Kontroleru pomocí adresy URL a řetězec dotazu a potom provádění metody akce, které reakce do prohlížeče s textem.</span><span class="sxs-lookup"><span data-stu-id="90264-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="90264-287">Úloha 1 – Přidání parametru žánr StoreController</span><span class="sxs-lookup"><span data-stu-id="90264-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="90264-288">V této úloze budete používat **querystring** parametry se mají odeslat **Procházet** metodu akce v **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="90264-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="90264-289">Pokud ještě není otevřený, začněte **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="90264-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="90264-290">V **souboru** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="90264-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="90264-291">V dialogovém okně Otevřít projekt, přejděte do **Source\Ex03 PassingParametersToAController\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="90264-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="90264-292">Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="90264-293">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90264-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90264-294">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="90264-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="90264-295">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="90264-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="90264-296">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="90264-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="90264-297">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90264-298">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90264-299">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="90264-300">Otevřít **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-300">Open **StoreController** class.</span></span> <span data-ttu-id="90264-301">Chcete-li to provést, v **Průzkumníka řešení**, rozbalte **řadiče** složky a dvojím kliknutím **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="90264-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="90264-302">Změnit **Procházet** metoda přidáním parametru řetězce vyžádat pro konkrétní žánr.</span><span class="sxs-lookup"><span data-stu-id="90264-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="90264-303">ASP.NET MVC automaticky předávat všechny řetězce dotazu nebo pojmenované parametry formuláře **žánr** k této metodě akce při vyvolání.</span><span class="sxs-lookup"><span data-stu-id="90264-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="90264-304">Chcete-li to provést, nahraďte **Procházet** metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="90264-305">(Fragment - kódu *základy ASP.NET MVC 4 – EX3. StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="90264-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="90264-306">Používáte **HttpUtility.HtmlEncode** nástroj metody zabrání uživatelům v vkládá jazyka Javascript do zobrazení s odkazem jako   **/Store/Procházet? Rozšířením podle tematických =&lt;skript&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="90264-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="90264-307">Další vysvětlení, navštivte prosím [článku na webu msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="90264-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="90264-308">Úloha 2 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="90264-309">V této úloze, vyzkoušejte si aplikace ve webovém prohlížeči a použít **žánr** parametru.</span><span class="sxs-lookup"><span data-stu-id="90264-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="90264-310">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-311">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="90264-312">Změňte adresu URL na   */Store/Procházet? Rozšířením podle tematických = Roz* k ověření, že akce přijímá parametr žánr.</span><span class="sxs-lookup"><span data-stu-id="90264-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="90264-313">![Procházení StoreBrowseGenre = Roz](aspnet-mvc-4-fundamentals/_static/image10.png "procházení StoreBrowseGenre = Roz")</span><span class="sxs-lookup"><span data-stu-id="90264-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="90264-314">*Procházení /Store/Browse? Rozšířením podle tematických = Roz*</span><span class="sxs-lookup"><span data-stu-id="90264-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="90264-315">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="90264-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="90264-316">Úloha 3 – Přidání parametr Id vložené v adrese URL</span><span class="sxs-lookup"><span data-stu-id="90264-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="90264-317">V této úloze budete používat **URL** předat **Id** parametr **podrobnosti** metody akce **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="90264-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="90264-318">ASP.NET MVC výchozí konvenci směrování se zachází segment adresy URL po názvu metody akce, jako parametr s názvem **Id**. Pokud vaše metoda akce má parametr s názvem Id, ASP.NET MVC se automaticky předat segment adresy URL pro vás jako parametr.</span><span class="sxs-lookup"><span data-stu-id="90264-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="90264-319">V adrese URL **Store/podrobnosti/5**, **Id** bude vyhodnocen jako **5**.</span><span class="sxs-lookup"><span data-stu-id="90264-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="90264-320">Změnit **podrobnosti** metodu **StoreController**, přidání **int** parametr s názvem **id**. Chcete-li to provést, nahraďte **podrobnosti** metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="90264-321">(Fragment - kódu *základy ASP.NET MVC 4 – EX3. StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="90264-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="90264-322">Úloha 4 – spouštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="90264-323">V této úloze, vyzkoušejte si aplikace ve webovém prohlížeči a použít **Id** parametru.</span><span class="sxs-lookup"><span data-stu-id="90264-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="90264-324">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-325">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="90264-326">Změňte adresu URL na */Store/Details/5* k ověření, že akce přijímá parametr id.</span><span class="sxs-lookup"><span data-stu-id="90264-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="90264-327">![Procházení StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "procházení StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="90264-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="90264-328">*Procházení /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="90264-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="90264-329">Cvičení 4: Vytvoření zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="90264-330">Mají se zatím vracení řetězců z akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="90264-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="90264-331">I když, který je užitečný způsob, jak Princip fungování řadiče, je postup nejsou sestavení webové aplikace skutečný.</span><span class="sxs-lookup"><span data-stu-id="90264-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="90264-332">Zobrazení jsou komponenty, které poskytují lepší přístup ke generování HTML zpět do prohlížeče s použitím souborů šablon.</span><span class="sxs-lookup"><span data-stu-id="90264-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="90264-333">V tomto cvičení se dozvíte, jak přidat rozložení stránky předlohy k nastavení šablony pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování webu a nakonec zobrazení šablony pro povolení HomeController vrátit ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="90264-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="90264-334">Úloha 1 - úprava souboru \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="90264-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="90264-335">Soubor **~/Views/Shared/\_layout.cshtml** umožní vám nastavit šablonu pro společný kód HTML pro použití na různých celého webu.</span><span class="sxs-lookup"><span data-stu-id="90264-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="90264-336">V této úloze přidáte rozložení stránky předlohy s společné hlavičky s odkazy na oblast domovské stránky a Store.</span><span class="sxs-lookup"><span data-stu-id="90264-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="90264-337">Pokud ještě není otevřený, začněte **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="90264-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="90264-338">V **souboru** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="90264-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="90264-339">V dialogovém okně Otevřít projekt, přejděte do **Source\Ex04 CreatingAView\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="90264-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="90264-340">Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="90264-341">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90264-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90264-342">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="90264-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="90264-343">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="90264-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="90264-344">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="90264-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="90264-345">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90264-346">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90264-347">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="90264-348">Soubor  <strong>\_layout.cshtml</strong> obsahuje rozložení HTML kontejner pro všechny stránky na webu.</span><span class="sxs-lookup"><span data-stu-id="90264-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="90264-349">Zahrnuje <strong>&lt;html&gt;</strong> – element pro odpovědi HTML, stejně jako <strong>&lt;head&gt;</strong> a <strong>&lt;tělo&gt;</strong> elementy.</span><span class="sxs-lookup"><span data-stu-id="90264-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="90264-350"><strong>@RenderBody()</strong> v kódu HTML tělo identifikovat oblasti zobrazení šablony budete moct vyplní dynamický obsah.</span><span class="sxs-lookup"><span data-stu-id="90264-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="90264-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="90264-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="90264-352">Přidáte společné hlavičky s odkazy na domovskou stránku a Store oblast na všech stránkách v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="90264-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="90264-353">Aby bylo možné provést, přidejte následující kód níže &lt;tělo&gt; příkazu.</span><span class="sxs-lookup"><span data-stu-id="90264-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="90264-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="90264-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="90264-355">Zahrnout div k vykreslení textu části každé stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="90264-356">Nahraďte  <strong>@RenderBody()</strong> následujícím kódem higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="90264-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="90264-357">Věděli jste?</span><span class="sxs-lookup"><span data-stu-id="90264-357">Did you know?</span></span> <span data-ttu-id="90264-358">Visual Studio 2012 obsahuje fragmenty kódu, které usnadňují přidejte kód pro běžně používané v HTML, soubory kódu a další.</span><span class="sxs-lookup"><span data-stu-id="90264-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="90264-359">Vyzkoušejte si zadáním **&lt;div&gt;** a stisknutím klávesy **kartu** dvakrát pro vložení kompletní **div** značky.</span><span class="sxs-lookup"><span data-stu-id="90264-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="90264-360">Úloha 2 – Přidání šablony stylů CSS</span><span class="sxs-lookup"><span data-stu-id="90264-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="90264-361">Šablonu prázdného projektu obsahuje velmi zjednodušený soubor šablony stylů CSS, který obsahuje pouze styly slouží k zobrazení založeného na základních formulářích a ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="90264-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="90264-362">Aby bylo možné vylepšit vzhled a chování webu bude používat další šablony stylů CSS a Image (potenciálně poskytované "designer").</span><span class="sxs-lookup"><span data-stu-id="90264-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="90264-363">V této úloze přidá šablona stylů CSS pro definování styly lokality.</span><span class="sxs-lookup"><span data-stu-id="90264-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="90264-364">Soubor šablony stylů CSS a image jsou součástí **Source\Assets\Content** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="90264-365">Aby bylo možné je přidat do aplikace, přetáhněte jejich obsah z **Windows Explorer** okno do **Průzkumníka řešení** v sadě Visual Studio Express for Web, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="90264-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="90264-366">![Přetažení obsah stylu](aspnet-mvc-4-fundamentals/_static/image12.png "přetažením obsah stylu")</span><span class="sxs-lookup"><span data-stu-id="90264-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="90264-367">*Přetažení obsah stylu*</span><span class="sxs-lookup"><span data-stu-id="90264-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="90264-368">Dialogové okno upozornění se zobrazí, žádostí o potvrzení k nahrazení **Site.css** souboru a některé existující Image.</span><span class="sxs-lookup"><span data-stu-id="90264-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="90264-369">Zkontrolujte **použít u všech položek** a klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="90264-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="90264-370">Úloha 3 – Přidání zobrazit šablonu</span><span class="sxs-lookup"><span data-stu-id="90264-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="90264-371">V této úloze přidá šablonu zobrazení k vygenerování odpovědi HTML, který bude používat rozložení stránky předlohy a šablon stylů CSS přidaný v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="90264-372">Použití zobrazení šablony při procházení na domovskou stránku, musíte nejdřív místo vrácení řetězce, která označuje, že **HomeController Index** metoda vrátí **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="90264-373">Otevřít **HomeController** třídy a změňte jeho **Index** metodu pro návrat **ActionResult**, a vrátí **View()**.</span><span class="sxs-lookup"><span data-stu-id="90264-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="90264-374">(Fragment - kódu *základy ASP.NET MVC 4 – Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="90264-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="90264-375">Teď budete muset přidat odpovídající šablonu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="90264-376">K tomu **klikněte pravým tlačítkem na** uvnitř **Index** metody akce a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="90264-377">Tím se otevře **přidat zobrazení** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="90264-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="90264-378">![Přidání zobrazení z v rámci metody Index](aspnet-mvc-4-fundamentals/_static/image13.png "přidání zobrazení z v rámci Index – metoda")</span><span class="sxs-lookup"><span data-stu-id="90264-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="90264-379">*Přidání zobrazení z v rámci Index – metoda*</span><span class="sxs-lookup"><span data-stu-id="90264-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="90264-380">**Přidat zobrazení** zobrazí se dialogové okno pro vytvoření souboru šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="90264-381">Ve výchozím nastavení toto dialogové okno zadá název zobrazení šablony tak, aby odpovídalo metodě akce, která bude používat.</span><span class="sxs-lookup"><span data-stu-id="90264-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="90264-382">Protože jste použili **přidat zobrazení** místní nabídky v rámci **Index** metody akce v rámci HomeController, **přidat zobrazení** dialogové okno obsahuje Index jako výchozí název zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="90264-383">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-383">Click **Add**.</span></span>

    <span data-ttu-id="90264-384">![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image14.png "Přidat Dialog zobrazení")</span><span class="sxs-lookup"><span data-stu-id="90264-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="90264-385">*Přidat Dialog zobrazení*</span><span class="sxs-lookup"><span data-stu-id="90264-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="90264-386">Visual Studio generuje **Index.cshtml** zobrazit šablonu uvnitř **Views\Home** složku a pak ho otevře.</span><span class="sxs-lookup"><span data-stu-id="90264-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="90264-387">![Domácí Index zobrazení vytvořené](aspnet-mvc-4-fundamentals/_static/image15.png "Domů Index zobrazení vytvořené")</span><span class="sxs-lookup"><span data-stu-id="90264-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="90264-388">*Vytvoření domovské zobrazení indexu*</span><span class="sxs-lookup"><span data-stu-id="90264-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-389">název a umístění **Index.cshtml** soubor je relevantní a dodržuje zásady vytváření názvů výchozí rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="90264-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="90264-390">Složka \Views\**Domů** odpovídá názvu kontroleru (**Domů** Kontroleru).</span><span class="sxs-lookup"><span data-stu-id="90264-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="90264-391">Název zobrazení šablony (**Index**), odpovídá metodu akce kontroleru, který se zobrazuje zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="90264-392">Tímto způsobem, ASP.NET MVC díky tomu není nutné explicitně zadat název nebo umístění šablony zobrazení při použití tyto zásady vytváření názvů pro vrácení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="90264-393">Generované zobrazení šablona je založena na  **\_layout.cshtml** dříve definované šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="90264-394">Umožňuje aktualizovat vlastnost ViewBag.Title k **Domů**a změnit hlavní obsah na **Toto je domovská stránka**, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="90264-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="90264-395">Vyberte **MvcMusicStore** projekt v Průzkumníku řešení a stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="90264-396">Úloha 4: ověření</span><span class="sxs-lookup"><span data-stu-id="90264-396">Task 4: Verification</span></span>

<span data-ttu-id="90264-397">Pokud chcete ověřit, že jste správně provedli všechny kroky v předchozím cvičení, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="90264-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="90264-398">V aplikaci otevřít v prohlížeči by měl Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="90264-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="90264-399">Metoda akce indexu HomeController nalezen a zobrazí **\Views\Home\Index.cshtml** zobrazit šablonu, i když kód volá **vrátit View()**, protože zobrazit šablonu a potom standardní zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="90264-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="90264-400">Na domovské stránce se zobrazí zobrazení uvítací zprávy definovaných v rámci **\Views\Home\Index.cshtml** zobrazit šablonu.</span><span class="sxs-lookup"><span data-stu-id="90264-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="90264-401">Domovská stránka používá  **\_layout.cshtml** šablony, a proto se uvítací zpráva je obsažena v rozložení standardní webu ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="90264-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="90264-402">![Domácí zobrazení indexu pomocí definovaných LayoutPage a styl](aspnet-mvc-4-fundamentals/_static/image16.png "Domů zobrazení indexu pomocí definovaných LayoutPage a stylu")</span><span class="sxs-lookup"><span data-stu-id="90264-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="90264-403">*Domovské zobrazení indexu pomocí definovaných LayoutPage a stylu*</span><span class="sxs-lookup"><span data-stu-id="90264-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="90264-404">Cvičení 5: Vytvoření modelu zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="90264-405">Zatím jste provedli v zobrazeních Zobrazit pevně zakódované HTML, ale chcete-li vytvořit dynamické webové aplikace, zobrazit šablonu získat informace z Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="90264-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="90264-406">Jednou z běžných metod má být použit pro tento účel je **ViewModel** vzor, který umožňuje Kontroleru Zabalit všechny informace potřebné ke generování odpovídající odpověď ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="90264-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="90264-407">V tomto cvičení se dozvíte, jak vytvořit třídu ViewModel a přidejte požadované vlastnosti: počet žánry v úložišti a seznam těchto žánrů.</span><span class="sxs-lookup"><span data-stu-id="90264-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="90264-408">Budete také aktualizovat StoreController používat vytvořený ViewModel a nakonec vytvoříte novou šablonu zobrazení, které se zobrazí vlastnosti uvedených na stránce.</span><span class="sxs-lookup"><span data-stu-id="90264-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="90264-409">Úloha 1 – vytváření tříd ViewModel</span><span class="sxs-lookup"><span data-stu-id="90264-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="90264-410">V této úloze vytvoříte třídu ViewModel, který implementuje scénář výpis žánr Store.</span><span class="sxs-lookup"><span data-stu-id="90264-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="90264-411">Pokud ještě není otevřený, začněte **VS Express for Web**.</span><span class="sxs-lookup"><span data-stu-id="90264-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="90264-412">V **souboru** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="90264-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="90264-413">V dialogovém okně Otevřít projekt, přejděte do **Source\Ex05 CreatingAViewModel\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="90264-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="90264-414">Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="90264-415">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90264-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90264-416">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="90264-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="90264-417">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="90264-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="90264-418">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="90264-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="90264-419">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90264-420">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90264-421">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="90264-422">Vytvoření **modely ViewModels** složku pro uložení ViewModel.</span><span class="sxs-lookup"><span data-stu-id="90264-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="90264-423">Chcete-li to provést, klikněte pravým tlačítkem myši na nejvyšší úrovni **MvcMusicStore** projekt, vyberte **přidat** a potom **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="90264-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="90264-424">![Přidat novou složku](aspnet-mvc-4-fundamentals/_static/image17.png "přidat novou složku")</span><span class="sxs-lookup"><span data-stu-id="90264-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="90264-425">*Přidání nové složky*</span><span class="sxs-lookup"><span data-stu-id="90264-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="90264-426">Název složky *modely ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="90264-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="90264-427">![Modely ViewModels složku v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image18.png "modely ViewModels složku v Průzkumníku řešení")</span><span class="sxs-lookup"><span data-stu-id="90264-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="90264-428">*Modely ViewModels složku v Průzkumníku řešení*</span><span class="sxs-lookup"><span data-stu-id="90264-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="90264-429">Vytvoření **ViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="90264-430">Chcete-li to provést, klikněte pravým tlačítkem na **modely ViewModels** složky, naposledy vytvořené, vyberte **přidat** a potom **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="90264-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="90264-431">V části **kód**, zvolte **třídy** položku a zadejte název souboru *StoreIndexViewModel.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="90264-432">![Přidání nové třídy](aspnet-mvc-4-fundamentals/_static/image19.png "přidání nové třídy")</span><span class="sxs-lookup"><span data-stu-id="90264-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="90264-433">*Přidání nové třídy*</span><span class="sxs-lookup"><span data-stu-id="90264-433">*Adding a new Class*</span></span>

    <span data-ttu-id="90264-434">![Vytvoření třídy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel vytváření tříd")</span><span class="sxs-lookup"><span data-stu-id="90264-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="90264-435">*Vytvoření třídy StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="90264-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="90264-436">Úloha 2 – Přidání vlastností do třídy ViewModel</span><span class="sxs-lookup"><span data-stu-id="90264-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="90264-437">Existují dva parametry se mají předat z StoreController zobrazit šablonu k vytvoření odpovědi HTML očekávané: počet žánry v úložišti a seznam těchto žánrů.</span><span class="sxs-lookup"><span data-stu-id="90264-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="90264-438">V této úloze budete přidávat tyto 2 vlastností **StoreIndexViewModel** třídy: **NumberOfGenres** (celé číslo) a **žánry** (seznam řetězců).</span><span class="sxs-lookup"><span data-stu-id="90264-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="90264-439">Přidat **NumberOfGenres** a **žánry** vlastnosti, které chcete **StoreIndexViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="90264-440">Chcete-li to provést, přidejte následující řádky 2 do definice třídy:</span><span class="sxs-lookup"><span data-stu-id="90264-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="90264-441">(Fragment - kódu *ASP.NET MVC 4 základy - Ex5 StoreIndexViewModel vlastnosti*)</span><span class="sxs-lookup"><span data-stu-id="90264-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="90264-442">**{Get; nastavit;}**  notation využívá jazyka C# pro funkce automaticky implementované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="90264-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="90264-443">Poskytuje výhody vlastnost bez nutnosti nám chcete-li deklarovat pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="90264-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="90264-444">Úloha 3 - aktualizace StoreController používat StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="90264-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="90264-445">**StoreIndexViewModel** třída zapouzdří informace potřebné pro předání z **StoreController**společnosti **Index** metodu zobrazit šablonu, aby bylo možné generovat odpověď .</span><span class="sxs-lookup"><span data-stu-id="90264-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="90264-446">V této úloze budete aktualizovat **StoreController** používat **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="90264-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="90264-447">Otevřít **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="90264-448">![Otevírání StoreController třídy](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController počáteční třída")</span><span class="sxs-lookup"><span data-stu-id="90264-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="90264-449">*Otevírání StoreController třídy*</span><span class="sxs-lookup"><span data-stu-id="90264-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="90264-450">Chcete-li použít **StoreIndexViewModel** třídy z **StoreController**, přidat následující obor názvů v horní části **StoreController** kódu:</span><span class="sxs-lookup"><span data-stu-id="90264-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="90264-451">(Fragment - kódu *ASP.NET MVC 4 základy - Ex5 StoreIndexViewModel pomocí modely ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="90264-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="90264-452">Změnit **StoreController**společnosti **Index** metody akce, takže se vytvoří a naplní **StoreIndexViewModel** objektu a pak předá zobrazení šablony Generovat odpověď jazyka HTML s ním.</span><span class="sxs-lookup"><span data-stu-id="90264-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-453">V testovacím prostředí &quot;ASP.NET MVC modely a přístup k datům&quot; budete psát kód, který načte seznam objektů úložiště žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="90264-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="90264-454">V následujícím kódu, kterou vytvoříte **seznamu** žánrů fiktivní data, které se vyplní **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="90264-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="90264-455">Po vytvoření a nastavení **StoreIndexViewModel** objektu, bude předáno jako argument **zobrazení** metody.</span><span class="sxs-lookup"><span data-stu-id="90264-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="90264-456">To znamená, že zobrazení šablony použije k vygenerování odpověď jazyka HTML s ním tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="90264-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="90264-457">Nahradit **Index** metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="90264-458">(Fragment - kódu *ASP.NET MVC 4 základy - metoda Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="90264-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="90264-459">Pokud nejste obeznámeni s jazykem C#, mohou předpokládat, který používá **var** znamená, že **viewModel** proměnná je s pozdní vazbou.</span><span class="sxs-lookup"><span data-stu-id="90264-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="90264-460">Který není správný – kompilátor jazyka C# používá podle přiřadit k proměnné odvození typu k určení, která **viewModel** je typu **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="90264-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="90264-461">Také, kompilací místní **viewModel** proměnnou **StoreIndexViewModel** typ kontroly kompilace get a podpora editoru kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="90264-462">Úloha 4 – vytvoření zobrazit šablonu, která používá StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="90264-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="90264-463">V této úloze vytvoříte zobrazení šablony, která se použije k zobrazení seznamu žánrů StoreIndexViewModel objekt předaný z Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="90264-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="90264-464">Před vytvořením nové šablony zobrazení, můžeme sestavit projekt tak, aby **Přidat Dialog zobrazení** ví o **StoreIndexViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="90264-465">Sestavte projekt výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="90264-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="90264-466">![Sestavení projektu](aspnet-mvc-4-fundamentals/_static/image22.png "sestavení projektu")</span><span class="sxs-lookup"><span data-stu-id="90264-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="90264-467">*Sestavení projektu*</span><span class="sxs-lookup"><span data-stu-id="90264-467">*Building the project*</span></span>
2. <span data-ttu-id="90264-468">Vytvořte novou šablonu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-468">Create a new View template.</span></span> <span data-ttu-id="90264-469">K tomu, klepněte pravým tlačítkem myši **Index** metody a vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="90264-470">![Přidání zobrazení](aspnet-mvc-4-fundamentals/_static/image23.png "přidání zobrazení")</span><span class="sxs-lookup"><span data-stu-id="90264-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="90264-471">*Přidání zobrazení*</span><span class="sxs-lookup"><span data-stu-id="90264-471">*Adding a View*</span></span>
3. <span data-ttu-id="90264-472">Protože **Přidat Dialog zobrazení** byla vyvolána z **StoreController**, přidá ve výchozím nastavení v šabloně zobrazení **\Views\Store\Index.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="90264-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="90264-473">Zkontrolujte **vytvoření silně zadali – zobrazení** zaškrtávací políčko a potom vyberte **StoreIndexViewModel** jako **třída modelu**.</span><span class="sxs-lookup"><span data-stu-id="90264-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="90264-474">Také se ujistěte, že je modul zobrazení vybrané **Razor**.</span><span class="sxs-lookup"><span data-stu-id="90264-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="90264-475">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-475">Click **Add**.</span></span>

    <span data-ttu-id="90264-476">![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image24.png "Přidat Dialog zobrazení")</span><span class="sxs-lookup"><span data-stu-id="90264-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="90264-477">*Přidat Dialog zobrazení*</span><span class="sxs-lookup"><span data-stu-id="90264-477">*Add View Dialog*</span></span>

    <span data-ttu-id="90264-478">**\Views\Store\Index.cshtml** je vytvořen a otevřít soubor šablony zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="90264-479">Podle informací uvedených na **přidat zobrazení** dialogového okna v posledním kroku, zobrazení bude očekávat šablony **StoreIndexViewModel** instance jako data pro generování odpověď jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="90264-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="90264-480">Všimnete si, že šablona dědí `ViewPage<musicstore.viewmodels.storeindexviewmodel>` v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="90264-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="90264-481">Úloha 5: aktualizuje se šablona zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="90264-482">V této úloze budete aktualizovat zobrazení šablony vytvořili v minulé úloze se načíst počet žánry a jejich názvy v rámci stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="90264-483">Budete používat @ syntaxe (označovaný také jako &quot;kódu útržky&quot;) ke spouštění kódu v rámci zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="90264-484">V **Index.cshtml** souborů v rámci **Store** složky, nahraďte jeho kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="90264-485">Smyčka v seznamu žánr v **StoreIndexViewModel** a vytvoření HTML **&lt;ul&gt;** seznamu pomocí **foreach** smyčky.</span><span class="sxs-lookup"><span data-stu-id="90264-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="90264-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="90264-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="90264-487">Stisknutím klávesy **F5** ke spuštění aplikace a Procházet **/Store**.</span><span class="sxs-lookup"><span data-stu-id="90264-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="90264-488">Zobrazí se seznam žánry předaný **StoreIndexViewModel** objektu z **StoreController** k šabloně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="90264-489">![Zobrazení seznamu žánry](aspnet-mvc-4-fundamentals/_static/image26.png "zobrazení seznamu žánry")</span><span class="sxs-lookup"><span data-stu-id="90264-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="90264-490">*Zobrazení seznamu žánry*</span><span class="sxs-lookup"><span data-stu-id="90264-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="90264-491">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="90264-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="90264-492">Cvičení 6: Použití parametrů v zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="90264-493">Cvičení 3 jste zjistili, jak pro předání parametrů do Kontroleru.</span><span class="sxs-lookup"><span data-stu-id="90264-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="90264-494">V tomto cvičení se dozvíte, jak používat tyto parametry v šabloně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="90264-495">Pro tento účel vám představíme nejprve do tříd modelu, které vám pomohou spravovat data a domény logiku.</span><span class="sxs-lookup"><span data-stu-id="90264-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="90264-496">Kromě toho se dozvíte, jak vytvořit odkazy na stránky v aplikaci ASP.NET MVC bez obav z věci, jako je kódování cesty adresy URL.</span><span class="sxs-lookup"><span data-stu-id="90264-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="90264-497">Úloha 1 – přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="90264-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="90264-498">Na rozdíl od modely ViewModel, který se vytvoří pouze k předávání informací z Kontroleru zobrazení, jsou integrované tříd modelu obsahovat a spravovat data a domény logiku.</span><span class="sxs-lookup"><span data-stu-id="90264-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="90264-499">V této úloze budete přidávat dvou tříd modelu pro reprezentaci těchto konceptů: **žánr** a **alba**.</span><span class="sxs-lookup"><span data-stu-id="90264-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="90264-500">Pokud ještě není otevřený, začněte **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="90264-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="90264-501">V **souboru** nabídce zvolte **otevřít projekt**.</span><span class="sxs-lookup"><span data-stu-id="90264-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="90264-502">V dialogovém okně Otevřít projekt, přejděte do **Source\Ex06 UsingParametersInView\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="90264-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="90264-503">Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="90264-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="90264-504">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90264-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90264-505">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="90264-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="90264-506">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="90264-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="90264-507">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="90264-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="90264-508">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90264-509">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90264-510">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="90264-511">Přidat **žánr** třída modelu.</span><span class="sxs-lookup"><span data-stu-id="90264-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="90264-512">Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**vyberte **přidat** a pak **nová položka** možnost.</span><span class="sxs-lookup"><span data-stu-id="90264-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="90264-513">V části **kód**, zvolte **třídy** položku a zadejte název souboru *Genre.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="90264-514">![Přidání třídy](aspnet-mvc-4-fundamentals/_static/image27.png "přidání třídy")</span><span class="sxs-lookup"><span data-stu-id="90264-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="90264-515">*Přidání nové položky*</span><span class="sxs-lookup"><span data-stu-id="90264-515">*Adding a new item*</span></span>

    <span data-ttu-id="90264-516">![Přidejte třídu modelu s rozšířením podle tematických](aspnet-mvc-4-fundamentals/_static/image28.png "přidejte třídu modelu s rozšířením podle tematických")</span><span class="sxs-lookup"><span data-stu-id="90264-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="90264-517">*Přidejte třídu modelu s rozšířením podle tematických*</span><span class="sxs-lookup"><span data-stu-id="90264-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="90264-518">Přidat **název** vlastností do třídy rozšířením podle tematických.</span><span class="sxs-lookup"><span data-stu-id="90264-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="90264-519">Chcete-li to provést, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="90264-519">To do this, add the following code:</span></span>

    <span data-ttu-id="90264-520">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 žánr*)</span><span class="sxs-lookup"><span data-stu-id="90264-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="90264-521">Stejný postup jako předtím, přidejte **alba** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="90264-522">Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**vyberte **přidat** a pak **nová položka** možnost.</span><span class="sxs-lookup"><span data-stu-id="90264-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="90264-523">V části **kód**, zvolte **třídy** položku a zadejte název souboru *Album.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="90264-524">Přidejte dvě vlastnosti do třídy alba: **žánr** a **Title**.</span><span class="sxs-lookup"><span data-stu-id="90264-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="90264-525">Chcete-li to provést, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="90264-525">To do this, add the following code:</span></span>

    <span data-ttu-id="90264-526">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 alba*)</span><span class="sxs-lookup"><span data-stu-id="90264-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="90264-527">Úloha 2 – přidání StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="90264-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="90264-528">A **StoreBrowseViewModel** se použije k zobrazení alb, která odpovídá vybrané žánr při plnění tohoto úkolu.</span><span class="sxs-lookup"><span data-stu-id="90264-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="90264-529">V této úloze vytvoříte této třídy a pak přidejte dvě vlastnosti pro zpracování **žánr** a jeho **alba**v seznamu.</span><span class="sxs-lookup"><span data-stu-id="90264-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="90264-530">Přidat **StoreBrowseViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="90264-531">Chcete-li to provést, klikněte pravým tlačítkem **modely ViewModels** složky v **Průzkumníku řešení**vyberte **přidat** a pak **nová položka** možnost.</span><span class="sxs-lookup"><span data-stu-id="90264-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="90264-532">V části **kód**, zvolte **třídy** položku a zadejte název souboru *StoreBrowseViewModel.cs*, pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="90264-533">Přidejte odkaz na modely v **StoreBrowseViewModel** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="90264-534">Chcete-li to provést, přidejte následující s použitím oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="90264-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="90264-535">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="90264-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="90264-536">Přidejte dvě vlastnosti do **StoreBrowseViewModel** třídy: **žánr** a **alb**.</span><span class="sxs-lookup"><span data-stu-id="90264-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="90264-537">Chcete-li to provést, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="90264-537">To do this, add the following code:</span></span>

    <span data-ttu-id="90264-538">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="90264-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="90264-539">Co je **seznamu&lt;alba&gt;**  ?: Tato definice používá **seznamu&lt;T&gt;**  typ, ve kterém **T** omezuje typ, na čem to **seznamu** patří, v tomto případě **alba** (nebo libovolného z jeho potomků).</span><span class="sxs-lookup"><span data-stu-id="90264-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="90264-540">Volá se tato schopnost navrhování tříd a metod, které specifikace jeden nebo více typů odložit, dokud třídy nebo metody je deklarovaný a vytvořena kódem na straně klienta je funkce jazyka C# **obecných typů**.</span><span class="sxs-lookup"><span data-stu-id="90264-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="90264-541">**Seznam&lt;T&gt;**  je obecný ekvivalent **ArrayList** typ a je k dispozici **System.Collections.Generic** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="90264-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="90264-542">Jednou z výhod použití **obecných typů** je, že vzhledem k tomu, že typ je určen, není potřeba starat o kontrolu operace, jako jsou prvky převedete do každodenního přetypování typu **alba** jako by tomu u **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="90264-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="90264-543">Úloha 3 – používání nového ViewModel v StoreController</span><span class="sxs-lookup"><span data-stu-id="90264-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="90264-544">V této úloze budete upravovat **StoreController**společnosti **Procházet** a **podrobnosti** metody akce k používání nového **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="90264-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="90264-545">Přidejte odkaz na složku modely v **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="90264-546">Chcete-li to provést, rozbalte **řadiče** složky **Průzkumníku řešení** a otevřete **StoreController** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="90264-547">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="90264-547">Then add the following code:</span></span>

    <span data-ttu-id="90264-548">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="90264-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="90264-549">Nahradit **Procházet** metoda akce se má použít **StoreViewBrowseController** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="90264-550">S fiktivními daty se vytvoří rozšířením podle tematických a dva nové objekty alb (v dalším praktického testovacího prostředí budete využívat reálná data z databáze).</span><span class="sxs-lookup"><span data-stu-id="90264-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="90264-551">Chcete-li to provést, nahraďte **Procházet** metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="90264-552">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="90264-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="90264-553">Nahradit **podrobnosti** metoda akce se má použít **StoreViewBrowseController** třídy.</span><span class="sxs-lookup"><span data-stu-id="90264-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="90264-554">Vytvoří nový **alba** objekt, který se má vrátit **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="90264-555">Chcete-li to provést, nahraďte **podrobnosti** metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="90264-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="90264-556">(Fragment - kódu *základy ASP.NET MVC 4 – Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="90264-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="90264-557">Úloha 4 – přidání procházet zobrazit šablonu</span><span class="sxs-lookup"><span data-stu-id="90264-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="90264-558">V této úloze, které přidáte **Procházet** zobrazení alb pro konkrétní žánr se nenašly.</span><span class="sxs-lookup"><span data-stu-id="90264-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="90264-559">Před vytvořením nové šablony zobrazení se má sestavit projekt tak, aby **přidat zobrazení** dialogové okno ví o **ViewModel** třídu použít.</span><span class="sxs-lookup"><span data-stu-id="90264-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="90264-560">Sestavte projekt výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="90264-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="90264-561">Přidat **Procházet** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-561">Add a **Browse** View.</span></span> <span data-ttu-id="90264-562">Chcete-li to provést, klikněte pravým tlačítkem **Procházet** metody akce **StoreController** a klikněte na tlačítko **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="90264-563">V **přidat zobrazení** dialogového okna zkontrolujte, zda je název zobrazení, **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="90264-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="90264-564">Zkontrolujte **vytvoření zobrazení se silnými typy** zaškrtávací políčko a vyberte **StoreBrowseViewModel** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="90264-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="90264-565">V ostatních polích ponechte jejich výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="90264-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="90264-566">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-566">Then click **Add**.</span></span>

    <span data-ttu-id="90264-567">![Přidání zobrazení Procházet](aspnet-mvc-4-fundamentals/_static/image29.png "přidání Procházet zobrazení")</span><span class="sxs-lookup"><span data-stu-id="90264-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="90264-568">*Přidání zobrazení procházení*</span><span class="sxs-lookup"><span data-stu-id="90264-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="90264-569">Upravit **Browse.cshtml** k zobrazení informací Genre, přístup k **StoreBrowseViewModel** objektu, který je předán do zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="90264-570">Chcete-li to provést, nahraďte obsah následujícím kódem: (C#)</span><span class="sxs-lookup"><span data-stu-id="90264-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="90264-571">Úloha 5: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="90264-572">V této úloze budete testovat, který **Procházet** metoda načte alb z **Procházet** metody akce.</span><span class="sxs-lookup"><span data-stu-id="90264-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="90264-573">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-574">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="90264-574">The project starts in the Home page.</span></span> <span data-ttu-id="90264-575">Změňte adresu URL na   **/Store/Procházet? Rozšířením podle tematických = Roz** k ověření, že akce vrací dva alb.</span><span class="sxs-lookup"><span data-stu-id="90264-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="90264-576">![Procházení Disco alb Store](aspnet-mvc-4-fundamentals/_static/image30.png "procházení Disco alb Store")</span><span class="sxs-lookup"><span data-stu-id="90264-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="90264-577">*Procházení Disco alb Store*</span><span class="sxs-lookup"><span data-stu-id="90264-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="90264-578">Krok 6 – zobrazení informací o určité Album</span><span class="sxs-lookup"><span data-stu-id="90264-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="90264-579">V této úloze budete implementovat **Store/podrobnosti** zobrazení pro zobrazení informací o určité album.</span><span class="sxs-lookup"><span data-stu-id="90264-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="90264-580">V tomto testovacím prostředí praktického všechno, co se zobrazí informace o alba je již součástí **zobrazení** šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="90264-581">Ano, místo vytvoření **StoreDetailsViewModel** třídy, budete používat aktuální **StoreBrowseViewModel** předáním alba šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="90264-582">Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="90264-583">Přidat nový **podrobnosti** zobrazit **StoreController**společnosti **podrobnosti** metody akce.</span><span class="sxs-lookup"><span data-stu-id="90264-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="90264-584">Chcete-li to provést, klikněte pravým tlačítkem **podrobnosti** metoda ve **StoreController** třídy a klikněte na tlačítko **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="90264-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="90264-585">V **přidat zobrazení** dialogového okna, ověřte, že **název zobrazení** je **podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="90264-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="90264-586">Zkontrolujte **vytvoření zobrazení se silnými typy** zaškrtávací políčko a vyberte **alba** z **třída modelu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="90264-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="90264-587">V ostatních polích ponechte jejich výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="90264-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="90264-588">Pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="90264-588">Then click **Add**.</span></span> <span data-ttu-id="90264-589">To vytvoří a otevře **\Views\Store\Details.cshtml** souboru.</span><span class="sxs-lookup"><span data-stu-id="90264-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="90264-590">![Přidání zobrazení podrobností o](aspnet-mvc-4-fundamentals/_static/image31.png "přidávání zobrazení podrobností")</span><span class="sxs-lookup"><span data-stu-id="90264-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="90264-591">*Přidání zobrazení podrobností*</span><span class="sxs-lookup"><span data-stu-id="90264-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="90264-592">Upravit **Details.cshtml** soubor k zobrazení informací na Album přístup k **alba** objektu, který je předán do zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="90264-593">Chcete-li to provést, nahraďte obsah následujícím kódem: (C#)</span><span class="sxs-lookup"><span data-stu-id="90264-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="90264-594">Úloha 7: spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="90264-595">V této úloze budete testovat, který **podrobnosti** zobrazení načte informace o alba z **podrobnosti akce** metody.</span><span class="sxs-lookup"><span data-stu-id="90264-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="90264-596">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-597">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="90264-598">Změňte adresu URL na **/Store/Details/5** potvrzení alba informací.</span><span class="sxs-lookup"><span data-stu-id="90264-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="90264-599">![Procházení podrobností alb](aspnet-mvc-4-fundamentals/_static/image32.png "procházení podrobností alb")</span><span class="sxs-lookup"><span data-stu-id="90264-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="90264-600">*Procházení podrobností alba*</span><span class="sxs-lookup"><span data-stu-id="90264-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="90264-601">Úloha 8 – přidání propojení mezi stránkami</span><span class="sxs-lookup"><span data-stu-id="90264-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="90264-602">V této úloze budete přidávat prostřednictvím odkazu ve Store zobrazení má odkaz v názvu každý žánr na příslušné **/Store/Procházet** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="90264-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="90264-603">Tímto způsobem, po kliknutí na Genre, například **Roz**, přejdete na **/Store/procházet? žánr = Roz** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="90264-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="90264-604">Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="90264-605">Aktualizace **Index** stránky a přidat odkaz **Procházet** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="90264-606">Chcete-li to provést, v **Průzkumníka řešení** rozbalte **zobrazení** složku, pak bude **Store** složky a dvojím kliknutím **Index.cshtml** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="90264-607">Přidáte odkaz na zobrazení procházet označující žánr vybrali.</span><span class="sxs-lookup"><span data-stu-id="90264-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="90264-608">Chcete-li to provést, nahradit následující zvýrazněný kód v rámci **&lt;li&gt;** značky: (C#)</span><span class="sxs-lookup"><span data-stu-id="90264-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="90264-609">Další možností by propojení přímo na stránku s kódem, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="90264-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="90264-610">&lt;href =&quot;/Store/procházet? žánr =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="90264-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="90264-611">Přestože tento přístup funguje, závisí na pevně zakódované řetězce.</span><span class="sxs-lookup"><span data-stu-id="90264-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="90264-612">Pokud později přejmenovat Kontroleru, budete muset změnit tento pokyn ručně.</span><span class="sxs-lookup"><span data-stu-id="90264-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="90264-613">Lepší alternativou je použití **pomocné rutiny HTML** metody.</span><span class="sxs-lookup"><span data-stu-id="90264-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="90264-614">ASP.NET MVC zahrnuje metodu pomocné rutiny HTML, který je k dispozici u takových úloh.</span><span class="sxs-lookup"><span data-stu-id="90264-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="90264-615">**Html.ActionLink()** Pomocná metoda usnadňuje vytváření HTML **&lt;&gt;** odkazy, ujistěte se cesty URL jsou správně kódování URL.</span><span class="sxs-lookup"><span data-stu-id="90264-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="90264-616">Htlm.ActionLink má několik přetížení.</span><span class="sxs-lookup"><span data-stu-id="90264-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="90264-617">V tomto cvičení budete používat ten, který přijímá tři parametry:</span><span class="sxs-lookup"><span data-stu-id="90264-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="90264-618">Text odkazu, který se zobrazí název žánru</span><span class="sxs-lookup"><span data-stu-id="90264-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="90264-619">Název akce kontroleru (**Procházet**)</span><span class="sxs-lookup"><span data-stu-id="90264-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="90264-620">Hodnoty parametrů, zadáte název trasy (**žánr**) a hodnota (**název žánru**)</span><span class="sxs-lookup"><span data-stu-id="90264-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="90264-621">Úloha 9 - spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="90264-622">V této úloze budete testovat, že se zobrazí každý žánr s odkazem na příslušné **/Store/Procházet** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="90264-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="90264-623">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-624">Projekt se spustí na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="90264-624">The project starts in the Home page.</span></span> <span data-ttu-id="90264-625">Změňte adresu URL na **/Store** k ověření, že každý žánr odkazuje na příslušnou **/Store/Procházet** adresy URL.</span><span class="sxs-lookup"><span data-stu-id="90264-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="90264-626">![Procházení žánry s odkazy na stránky Procházet](aspnet-mvc-4-fundamentals/_static/image33.png "procházení žánry s odkazy na stránky procházení")</span><span class="sxs-lookup"><span data-stu-id="90264-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="90264-627">*Procházení žánry s odkazy na stránky procházení*</span><span class="sxs-lookup"><span data-stu-id="90264-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="90264-628">Úloha 10 - pomocí dynamické ViewModel kolekce k předání hodnot</span><span class="sxs-lookup"><span data-stu-id="90264-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="90264-629">V této úloze se dozvíte jednoduchá ale účinná metoda k předání hodnot mezi Kontrolerem a zobrazení bez jakýchkoli změn v modelu.</span><span class="sxs-lookup"><span data-stu-id="90264-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="90264-630">ASP.NET MVC 4 poskytuje kolekci &quot;ViewModel&quot;, což může být přiřazena na libovolnou hodnotu dynamické a získat přístup do kontrolerů a zobrazení také.</span><span class="sxs-lookup"><span data-stu-id="90264-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="90264-631">Teď použijete dynamické kolekce položek ViewBag předat seznam &quot; **označený hvězdičkou žánry** &quot; z kontroleru zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90264-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="90264-632">Zobrazení indexu Store bude přístup k **ViewModel** a zobrazení informací.</span><span class="sxs-lookup"><span data-stu-id="90264-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="90264-633">Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="90264-634">Otevřít **StoreController.cs** a upravit **Index** metodu pro vytvoření seznamu hvězdičkou žánry do ViewModel kolekce:</span><span class="sxs-lookup"><span data-stu-id="90264-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="90264-635">Můžete také použít syntaxi **objekt ViewBag [&quot;Starred&quot;]** pro přístup k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="90264-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="90264-636">Ikona hvězdičky **&quot;starred.png&quot;** je součástí **Source\Assets\Images** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="90264-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="90264-637">Chcete-li přidat ji do aplikace, přetáhněte jejich obsah z **Windows Explorer** okno do **Průzkumníka řešení** v aplikaci Visual Web Developer Express, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="90264-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="90264-638">![Přidáním hvězdičky image do řešení](aspnet-mvc-4-fundamentals/_static/image34.png "přidáním hvězdičky image do řešení")</span><span class="sxs-lookup"><span data-stu-id="90264-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="90264-639">*Přidání hvězdičky bitové kopie do řešení*</span><span class="sxs-lookup"><span data-stu-id="90264-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="90264-640">Otevřete zobrazení **Store/Index.cshtml** a upravovat obsah.</span><span class="sxs-lookup"><span data-stu-id="90264-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="90264-641">Bude číst &quot;označený hvězdičkou&quot; vlastnost **objekt ViewBag** kolekce a požádejte ho, pokud aktuální žánr název je v seznamu.</span><span class="sxs-lookup"><span data-stu-id="90264-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="90264-642">V takovém případě se zobrazí ikona hvězdičky zprava žánr odkaz.</span><span class="sxs-lookup"><span data-stu-id="90264-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="90264-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="90264-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="90264-644">Úloha 11 – spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="90264-645">V této úloze budete testovat, Ohodnoťte žánry zobrazit označené ikonou s hvězdičkou.</span><span class="sxs-lookup"><span data-stu-id="90264-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="90264-646">Stisknutím klávesy **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90264-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="90264-647">Projekt se spustí v **Domů** stránky.</span><span class="sxs-lookup"><span data-stu-id="90264-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="90264-648">Změňte adresu URL na **/Store** ověřte, že má každý žánr vybrané pečlivě popisku:</span><span class="sxs-lookup"><span data-stu-id="90264-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="90264-649">![Procházení žánry Ohodnoťte elementy](aspnet-mvc-4-fundamentals/_static/image35.png "procházení žánry Ohodnoťte elementy")</span><span class="sxs-lookup"><span data-stu-id="90264-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="90264-650">*Procházení žánry Ohodnoťte elementy*</span><span class="sxs-lookup"><span data-stu-id="90264-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="90264-651">Cvičení 7: Kolečko okolo nové šablony ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="90264-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="90264-652">V tomto cvičení bude prozkoumat rozšíření v šablonách projektů ASP.NET MVC 4, podívali se do nanejvýš relevantní funkce nové šablony.</span><span class="sxs-lookup"><span data-stu-id="90264-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="90264-653">Úloha 1: Zkoumání šablony ASP.NET MVC 4 Internetové aplikace</span><span class="sxs-lookup"><span data-stu-id="90264-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="90264-654">Pokud ještě není otevřený, začněte **VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="90264-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="90264-655">Vyberte **soubor | Nové | Projekt** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="90264-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="90264-656">V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="90264-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="90264-657">**Název** projektu *MusicStore* a aktualizovat **název řešení** k *začít*, pak vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK** .</span><span class="sxs-lookup"><span data-stu-id="90264-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="90264-658">![Vytvoření nového projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "vytvoření nového projektu ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="90264-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="90264-659">*Vytvoření nového projektu ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="90264-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="90264-660">V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **internetovou aplikaci** šablony projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="90264-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="90264-661">Všimněte si, že jste vybrali jako zobrazovací modul Razor nebo ASPX.</span><span class="sxs-lookup"><span data-stu-id="90264-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="90264-662">![Vytvoření nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "vytvoření nové aplikace ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="90264-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="90264-663">*Vytvoření nové aplikace ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="90264-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-664">Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="90264-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="90264-665">Jeho cílem je minimalizovat počet znaků a stisknutí kláves vyžaduje v souboru, umožňuje rychlé a dynamika kódování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="90264-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="90264-666">Razor využívá existující C# /VB (nebo jiné) jazykové dovednosti a poskytuje šablonu syntaxe značek, umožňující pracovním Super konstrukce jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="90264-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="90264-667">Stisknutím klávesy **F5** ke spuštění řešení a zobrazit obnovené šablonu.</span><span class="sxs-lookup"><span data-stu-id="90264-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="90264-668">Si můžete prohlédnout následující funkce:</span><span class="sxs-lookup"><span data-stu-id="90264-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="90264-669">**Moderní styl šablony**</span><span class="sxs-lookup"><span data-stu-id="90264-669">**Modern-style templates**</span></span>

        <span data-ttu-id="90264-670">Šablony byly obnoveny, poskytuje další styly moderního vzhledu.</span><span class="sxs-lookup"><span data-stu-id="90264-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="90264-671">![Šablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled šablony ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="90264-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="90264-672">*Šablony ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="90264-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="90264-673">**Adaptivní vykreslování**</span><span class="sxs-lookup"><span data-stu-id="90264-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="90264-674">Podívejte se na změně velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobí novou velikost okna.</span><span class="sxs-lookup"><span data-stu-id="90264-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="90264-675">Tyto šablony pomocí adaptivního vykreslování techniku správně vykreslit, desktopových a mobilních platforem bez jakéhokoli přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="90264-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="90264-676">![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](aspnet-mvc-4-fundamentals/_static/image39.png "šablonu projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")</span><span class="sxs-lookup"><span data-stu-id="90264-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="90264-677">*Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*</span><span class="sxs-lookup"><span data-stu-id="90264-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="90264-678">Zavřete prohlížeč zastavení ladicího programu a vrátíte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90264-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="90264-679">Nyní budete moci prozkoumat řešení a podívejte se na některé z nových funkcích zavedených v architektuře ASP.NET MVC 4 v šabloně projektu.</span><span class="sxs-lookup"><span data-stu-id="90264-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="90264-680">![Technologie ASP.NET MVC4 – internet--šablony projektu aplikace-](aspnet-mvc-4-fundamentals/_static/image40.png "šablonu projektu ASP.NET MVC 4 Internetové aplikace")</span><span class="sxs-lookup"><span data-stu-id="90264-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="90264-681">*Šablona projektu ASP.NET MVC 4 Internetové aplikace*</span><span class="sxs-lookup"><span data-stu-id="90264-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="90264-682">**Značek HTML5**</span><span class="sxs-lookup"><span data-stu-id="90264-682">**HTML5 markup**</span></span>

       <span data-ttu-id="90264-683">Procházet šablony zobrazení a zjistěte, nové značky motiv, který je třeba otevřít **About.cshtml** zobrazit v rámci **Domů** složky.</span><span class="sxs-lookup"><span data-stu-id="90264-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="90264-684">![Nové šablony, pomocí značky Razor a HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "novou šablonu, pomocí značky Razor a HTML5")</span><span class="sxs-lookup"><span data-stu-id="90264-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="90264-685">*Nové šablony, pomocí značky Razor a HTML5*</span><span class="sxs-lookup"><span data-stu-id="90264-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="90264-686">**Zahrnuté knihovny jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="90264-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="90264-687">**jQuery**: jQuery usnadňuje procházení dokumentu HTML, zpracování událostí, animace a interakce Ajax.</span><span class="sxs-lookup"><span data-stu-id="90264-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="90264-688">**uživatelské rozhraní jQuery**: Tato knihovna poskytuje abstrakci pro nízké úrovni interakce a animace, pokročilé efekty a bylo widgetů, postavený na Javascriptovou knihovnu jQuery.</span><span class="sxs-lookup"><span data-stu-id="90264-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="90264-689">Informace o jQuery a uživatelské rozhraní jQuery v [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="90264-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="90264-690">**KnockoutJS**: nyní zahrnuje výchozí šablonu pro ASP.NET MVC 4 **KnockoutJS**, rozhraní MVVM jazyka JavaScript, které vám umožní vytvářet bohaté a s velmi rychlou odezvou webové aplikace pomocí jazyků JavaScript a HTML.</span><span class="sxs-lookup"><span data-stu-id="90264-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="90264-691">Stejně jako v architektuře ASP.NET MVC 3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="90264-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="90264-692">Můžete získat další informace o knihovně KnockOutJS v tomto odkazu: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="90264-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="90264-693">**Modernizr**: Tato knihovna spouští automaticky, vytváření webu kompatibilní s starší prohlížeče, při použití technologií HTML5 a CSS3.</span><span class="sxs-lookup"><span data-stu-id="90264-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="90264-694">Můžete získat další informace o knihovně Modernizr v tomto odkazu: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="90264-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="90264-695">**SimpleMembership zahrnutý v řešení**</span><span class="sxs-lookup"><span data-stu-id="90264-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="90264-696">SimpleMembership byly navržené jako náhrada za předchozí systému zprostředkovatele rolí ASP.NET a členství.</span><span class="sxs-lookup"><span data-stu-id="90264-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="90264-697">Obsahuje mnoho nových funkcí, které usnadňují pro vývojáře na zabezpečené webové stránky tak flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="90264-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="90264-698">Šablona Internet již má nastavit pár věcí, které integrují SimpleMembership, například AccountController připravena k použití OAuthWebSecurity (pro registraci účtu OAuth, přihlášení, správu, atd.) a zabezpečení webového.</span><span class="sxs-lookup"><span data-stu-id="90264-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="90264-699">![SimpleMembership zahrnutý v řešení](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zahrnutý v řešení")</span><span class="sxs-lookup"><span data-stu-id="90264-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="90264-700">*SimpleMembership zahrnutý v řešení*</span><span class="sxs-lookup"><span data-stu-id="90264-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="90264-701">Najít další informace o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="90264-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="90264-702">Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="90264-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="90264-703">Souhrn</span><span class="sxs-lookup"><span data-stu-id="90264-703">Summary</span></span>

<span data-ttu-id="90264-704">Po dokončení tohoto praktického testovacího prostředí jste se naučili základy ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="90264-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="90264-705">Základní prvky aplikace MVC a jak pracují</span><span class="sxs-lookup"><span data-stu-id="90264-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="90264-706">Jak vytvořit aplikaci ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90264-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="90264-707">Postup přidání a konfigurace řadiče pro zpracování parametry předat prostřednictvím adresy URL a řetězec dotazu</span><span class="sxs-lookup"><span data-stu-id="90264-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="90264-708">Postup přidání rozložení stránky předlohy k nastavení šablony pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování a zobrazit šablonu pro zobrazení obsahu HTML</span><span class="sxs-lookup"><span data-stu-id="90264-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="90264-709">Použití vzoru ViewModel pro předání vlastnosti chcete zobrazit šablonu k zobrazení dynamických informací</span><span class="sxs-lookup"><span data-stu-id="90264-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="90264-710">Jak používat parametry předané do řadičů v zobrazení šablony</span><span class="sxs-lookup"><span data-stu-id="90264-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="90264-711">Jak přidat odkazy na stránky v aplikaci ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="90264-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="90264-712">Jak přidat a používat dynamické vlastnosti v zobrazení</span><span class="sxs-lookup"><span data-stu-id="90264-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="90264-713">Rozšíření v šablonách projektů ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="90264-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="90264-714">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="90264-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="90264-715">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="90264-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="90264-716">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="90264-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="90264-717">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="90264-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="90264-718">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="90264-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="90264-719">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="90264-719">Click on **Install Now**.</span></span> <span data-ttu-id="90264-720">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="90264-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="90264-721">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="90264-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="90264-722">![Instalace sady Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="90264-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="90264-723">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="90264-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="90264-724">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="90264-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="90264-726">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="90264-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="90264-727">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="90264-727">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="90264-729">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="90264-729">*Installation progress*</span></span>
6. <span data-ttu-id="90264-730">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="90264-730">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="90264-732">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="90264-732">*Installation completed*</span></span>
7. <span data-ttu-id="90264-733">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="90264-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="90264-734">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="90264-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="90264-736">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="90264-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="90264-737">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="90264-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="90264-738">Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="90264-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="90264-739">Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="90264-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="90264-740">Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="90264-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-741">Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="90264-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="90264-742">Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="90264-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="90264-743">![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="90264-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="90264-744">*Přihlaste se k portálu pro správu Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="90264-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="90264-745">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="90264-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="90264-746">![Vytvoření nového webu](aspnet-mvc-4-fundamentals/_static/image49.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="90264-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="90264-747">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="90264-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="90264-748">Klikněte na tlačítko **Compute** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="90264-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="90264-749">Potom vyberte **rychlé vytvoření** možnost.</span><span class="sxs-lookup"><span data-stu-id="90264-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="90264-750">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.</span><span class="sxs-lookup"><span data-stu-id="90264-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-751">Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="90264-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="90264-752">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="90264-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="90264-753">Nezahrnuje kroky pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="90264-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="90264-754">![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-fundamentals/_static/image50.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="90264-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="90264-755">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="90264-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="90264-756">Počkejte, dokud nové **webu** se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="90264-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="90264-757">Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="90264-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="90264-758">Zkontrolujte, jestli funguje nový web.</span><span class="sxs-lookup"><span data-stu-id="90264-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="90264-759">![Na nový web](aspnet-mvc-4-fundamentals/_static/image51.png "přechodu na nový web")</span><span class="sxs-lookup"><span data-stu-id="90264-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="90264-760">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="90264-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="90264-761">![Spuštění webu](aspnet-mvc-4-fundamentals/_static/image52.png "spuštění webové stránky")</span><span class="sxs-lookup"><span data-stu-id="90264-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="90264-762">*Spuštění webové stránky*</span><span class="sxs-lookup"><span data-stu-id="90264-762">*Web site running*</span></span>
6. <span data-ttu-id="90264-763">Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.</span><span class="sxs-lookup"><span data-stu-id="90264-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="90264-764">![Otevřete správu webových stránek](aspnet-mvc-4-fundamentals/_static/image53.png "otevřete správu webových stránek")</span><span class="sxs-lookup"><span data-stu-id="90264-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="90264-765">*Otevřete správu webových stránek*</span><span class="sxs-lookup"><span data-stu-id="90264-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="90264-766">V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="90264-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-767">*Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování.</span><span class="sxs-lookup"><span data-stu-id="90264-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="90264-768">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace.</span><span class="sxs-lookup"><span data-stu-id="90264-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="90264-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="90264-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="90264-770">![Stahování webové stránky publikovat profil](aspnet-mvc-4-fundamentals/_static/image54.png "stahování webové stránky profil publikování")</span><span class="sxs-lookup"><span data-stu-id="90264-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="90264-771">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="90264-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="90264-772">Stáhněte si soubor profilu publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="90264-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="90264-773">Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="90264-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="90264-774">![Ukládání souboru profilu publikování](aspnet-mvc-4-fundamentals/_static/image55.png "ukládá se profil publikování")</span><span class="sxs-lookup"><span data-stu-id="90264-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="90264-775">*Ukládá se profil publikování*</span><span class="sxs-lookup"><span data-stu-id="90264-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="90264-776">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="90264-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="90264-777">Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="90264-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="90264-778">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="90264-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="90264-779">Pro uložení databáze aplikace budete potřebovat databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="90264-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="90264-780">Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="90264-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="90264-781">Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="90264-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="90264-782">Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="90264-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="90264-783">Nevytvářet databáze, protože vytvoří se v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="90264-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="90264-784">![Řídicí panel serveru SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "řídicího panelu serveru SQL Database")</span><span class="sxs-lookup"><span data-stu-id="90264-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="90264-785">*Řídicí panel serveru SQL Database*</span><span class="sxs-lookup"><span data-stu-id="90264-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="90264-786">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="90264-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="90264-787">Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90264-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Přidat IP adresu klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="90264-789">*Přidat IP adresu klienta*</span><span class="sxs-lookup"><span data-stu-id="90264-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="90264-790">Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="90264-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="90264-792">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="90264-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="90264-793">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="90264-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="90264-794">Vraťte se do řešení ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="90264-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="90264-795">V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="90264-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="90264-796">![Publikování aplikace](aspnet-mvc-4-fundamentals/_static/image60.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="90264-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="90264-797">*Publikování na webu*</span><span class="sxs-lookup"><span data-stu-id="90264-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="90264-798">Importujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="90264-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="90264-799">![Import profilu publikování](aspnet-mvc-4-fundamentals/_static/image61.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="90264-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="90264-800">*Import publikačního profilu*</span><span class="sxs-lookup"><span data-stu-id="90264-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="90264-801">Klikněte na tlačítko **ověřit připojení**.</span><span class="sxs-lookup"><span data-stu-id="90264-801">Click **Validate Connection**.</span></span> <span data-ttu-id="90264-802">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="90264-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90264-803">Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="90264-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="90264-804">![Ověřuje se připojení](aspnet-mvc-4-fundamentals/_static/image62.png "ověřuje se připojení")</span><span class="sxs-lookup"><span data-stu-id="90264-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="90264-805">*Ověřuje se připojení*</span><span class="sxs-lookup"><span data-stu-id="90264-805">*Validating connection*</span></span>
4. <span data-ttu-id="90264-806">V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="90264-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="90264-807">![Konfigurace nasazení webu](aspnet-mvc-4-fundamentals/_static/image63.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="90264-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="90264-808">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="90264-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="90264-809">Konfigurace připojení k databázi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="90264-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="90264-810">V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="90264-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="90264-811">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="90264-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="90264-812">V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="90264-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="90264-813">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="90264-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="90264-814">![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-fundamentals/_static/image64.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="90264-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="90264-815">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="90264-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="90264-816">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="90264-816">Then click **OK**.</span></span> <span data-ttu-id="90264-817">Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="90264-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="90264-818">![Vytvoření databáze](aspnet-mvc-4-fundamentals/_static/image65.png "vytvoření řetězce databáze")</span><span class="sxs-lookup"><span data-stu-id="90264-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="90264-819">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="90264-819">*Creating the database*</span></span>
7. <span data-ttu-id="90264-820">Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="90264-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="90264-821">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="90264-821">Then click **Next**.</span></span>

    <span data-ttu-id="90264-822">![Připojovací řetězec odkazující na SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "připojovací řetězec odkazující na SQL Database")</span><span class="sxs-lookup"><span data-stu-id="90264-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="90264-823">*Připojovací řetězec odkazující na SQL Database*</span><span class="sxs-lookup"><span data-stu-id="90264-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="90264-824">V **ve verzi Preview** klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="90264-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="90264-825">![Publikování webové aplikace](aspnet-mvc-4-fundamentals/_static/image67.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="90264-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="90264-826">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="90264-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="90264-827">Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.</span><span class="sxs-lookup"><span data-stu-id="90264-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="90264-828">![Publikování aplikace do Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "publikování aplikace do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="90264-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="90264-829">*Aplikace publikovaná do Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="90264-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="90264-830">Příloha C: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="90264-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="90264-831">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="90264-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="90264-832">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="90264-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="90264-833">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-fundamentals/_static/image69.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="90264-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="90264-834">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="90264-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="90264-835">***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***</span><span class="sxs-lookup"><span data-stu-id="90264-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="90264-836">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="90264-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="90264-837">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="90264-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="90264-838">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="90264-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="90264-839">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="90264-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="90264-840">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="90264-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="90264-841">![Začněte psát název fragmentu kódu](aspnet-mvc-4-fundamentals/_static/image70.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="90264-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="90264-842">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="90264-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="90264-843">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-fundamentals/_static/image71.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="90264-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="90264-844">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="90264-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="90264-845">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-fundamentals/_static/image72.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="90264-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="90264-846">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="90264-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="90264-847">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="90264-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="90264-848">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="90264-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="90264-849">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="90264-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="90264-850">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="90264-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="90264-851">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-fundamentals/_static/image73.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="90264-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="90264-852">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="90264-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="90264-853">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-fundamentals/_static/image74.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="90264-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="90264-854">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="90264-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
