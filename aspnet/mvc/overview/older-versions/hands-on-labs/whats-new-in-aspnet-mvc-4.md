---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co je nového v architektuře ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: ASP.NET MVC 4 je rozhraní pro vytváření aplikací škálovatelná webů založené na standardech pomocí zavedených návrhových postupů a sílu technologie ASP.NET a...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9d5a51a5887ecbbc96fce1416b88aa849bc3674e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912719"
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="d3c5e-103">Co je nového v architektuře ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d3c5e-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="d3c5e-104">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d3c5e-105">Stáhněte si Web Campy školení Kit</span><span class="sxs-lookup"><span data-stu-id="d3c5e-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="d3c5e-106">ASP.NET MVC 4 je rozhraní pro vytváření aplikací škálovatelná webů založené na standardech pomocí zavedených návrhových postupů a sílu technologie ASP.NET a .NET framework.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="d3c5e-107">Tento nový, čtvrtý verzi rozhraní framework se zaměřuje na zjednodušení vývoj mobilní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="d3c5e-108">Než začneme když vytvoříte nový projekt ASP.NET MVC 4 není nyní šablonu projektu mobilních aplikací, které můžete použít k vytvoření samostatné aplikace speciálně pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="d3c5e-109">ASP.NET MVC 4 navíc integruje jQuery Mobile prostřednictvím balíčku NuGet jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="d3c5e-110">jQuery Mobile je rozhraní založeného na technologii HTML5 umožňující vývoj webových aplikací, které jsou kompatibilní s všechny platformy oblíbených mobilních zařízení, včetně Windows Phone, iPhone, Android a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="d3c5e-111">Ale pokud potřebujete specializace, ASP.NET MVC 4 také umožňuje obsluhovat různá zobrazení pro různá zařízení a poskytují optimalizace pro konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="d3c5e-112">V této praktických cvičení začnete s ASP.NET MVC 4 &quot;internetovou aplikaci&quot; šablona projektu pro vytvoření aplikace Fotogalerie.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="d3c5e-113">Postupně se zlepšila aplikace pomocí jQuery Mobile a nové funkce technologie ASP.NET MVC 4, aby byl kompatibilní s různých mobilních zařízení a desktopového webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="d3c5e-114">Dozvíte se taky o nový kód návodů pro generování kódu a jak ASP.NET MVC 4 usnadňuje psaní metody asynchronní akce díky podpoře úloh&lt;ActionResult&gt; návratové typy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="d3c5e-115">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="d3c5e-116">Projekt specifické pro toto testovací prostředí je k dispozici na [co je nového ve webových formulářích v ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d3c5e-117">Cíle</span><span class="sxs-lookup"><span data-stu-id="d3c5e-117">Objectives</span></span>

<span data-ttu-id="d3c5e-118">V této praktická cvičení se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d3c5e-119">Využijte výhod rozšíření, která chcete šablony – včetně projektu ASP.NET MVC nové šablony projektů mobilních aplikací</span><span class="sxs-lookup"><span data-stu-id="d3c5e-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="d3c5e-120">Použít atribut zobrazení HTML5 a CSS media dotazy ke zlepšení zobrazení na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="d3c5e-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="d3c5e-121">Použít jQuery Mobile pro progresivní vylepšení a vytváření dotykového webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d3c5e-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="d3c5e-122">Vytvořit zobrazení specifické pro mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-122">Create mobile-specific views</span></span>
- <span data-ttu-id="d3c5e-123">Chcete-li přepnout mezi mobilními a desktopovými zobrazení v aplikaci použijte komponentu přepínači zobrazení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="d3c5e-124">Vytvoření asynchronní řadiče pomocí podpory úkolu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d3c5e-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3c5e-125">Prerequisites</span></span>

<span data-ttu-id="d3c5e-126">Musíte mít následující položky k dokončení tohoto testovacího prostředí:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="d3c5e-127">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [dodatku B](#AppendixB) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="d3c5e-128">[ASP.NET MVC 4](../../../mvc4.md) (součástí instalace – Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="d3c5e-129">Emulátor Windows Phone (součástí [7.1.1 Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="d3c5e-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="d3c5e-130">Volitelné – [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) s **Electric Plum iPhone simulátor** rozšíření (pouze pro cvičení 3 používaná k prohlížení webové aplikace se simulátor pro zařízení iPhone)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d3c5e-131">Instalace</span><span class="sxs-lookup"><span data-stu-id="d3c5e-131">Setup</span></span>

<span data-ttu-id="d3c5e-132">V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d3c5e-133">Pro usnadnění je k dispozici většinu kódu jako Visual Studio fragmenty kódu, který můžete použít z Visual Studia abyste ho nemuseli znovu přidat ručně.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="d3c5e-134">Chcete-li nainstalovat fragmenty kódu:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-134">To install the code snippets:</span></span>

1. <span data-ttu-id="d3c5e-135">Otevřete okno Průzkumníka Windows a přejděte do testovacího prostředí **Source\Setup** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="d3c5e-136">Dvakrát klikněte **Setup.cmd** soubor v této složce instalace fragmenty kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="d3c5e-137">Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí dodatku A:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d3c5e-138">Cvičení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-138">Exercises</span></span>

<span data-ttu-id="d3c5e-139">Toto praktické testovací prostředí obsahuje následující praktická cvičení:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d3c5e-140">Nové šablony projektu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d3c5e-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="d3c5e-141">Vytvoření webové aplikace Fotogalerie</span><span class="sxs-lookup"><span data-stu-id="d3c5e-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="d3c5e-142">Přidání podpory pro mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="d3c5e-143">Použití asynchronního řadiče</span><span class="sxs-lookup"><span data-stu-id="d3c5e-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="d3c5e-144">Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d3c5e-145">Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="d3c5e-146">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="d3c5e-147">Cvičení 1: Nové šablony projektu ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d3c5e-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="d3c5e-148">V tomto cvičení bude prozkoumat rozšíření v šablonách projektu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="d3c5e-149">Kromě šabloně Internetové aplikace již v MVC 3, tato verze nyní zahrnuje samostatné šablony pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="d3c5e-150">Nejprve se podíváte na některé důležité funkce všechny šablony.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="d3c5e-151">Potom bude fungovat na vykreslení stránky správně na různých platformách pomocí správný přístup.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="d3c5e-152">Úloha 1 – prohlížení Internetu šablony aplikace</span><span class="sxs-lookup"><span data-stu-id="d3c5e-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="d3c5e-153">Otevřít **sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="d3c5e-154">Vyberte **soubor | Nové | Projekt** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d3c5e-155">V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d3c5e-156">Pojmenujte projekt **Fotogalerie**, vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-157">Později budete přizpůsobovat Fotogalerie ASP.NET MVC 4 řešení, které se právě vytváří.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="d3c5e-158">![Vytvoření nového projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "vytvoření nového projektu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="d3c5e-159">*Vytvoření nového projektu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-159">*Creating a new project*</span></span>
3. <span data-ttu-id="d3c5e-160">V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **internetovou aplikaci** šablony projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d3c5e-161">Ujistěte se, že jste zvolili jako zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="d3c5e-162">![Vytvoření nové aplikace ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "vytvoření nové aplikace ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="d3c5e-163">*Vytvoření nové aplikace ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-164">Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="d3c5e-165">Jeho cílem je minimalizovat počet znaků a stisknutí kláves vyžaduje v souboru, umožňuje rychlé a dynamika kódování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="d3c5e-166">Razor využívá existující C# / VB (nebo jiné) jazykové dovednosti a poskytuje šablonu syntaxe značek, umožňující pracovním Super konstrukce jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="d3c5e-167">Stisknutím klávesy **F5** obnovené šablony a spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="d3c5e-168">Si můžete prohlédnout následující funkce:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-168">You can check out the following features:</span></span>

    <span data-ttu-id="d3c5e-169">**Moderní styl šablony**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-169">**Modern-style templates**</span></span>

    <span data-ttu-id="d3c5e-170">Šablony byly obnoveny, poskytuje další styly moderního vzhledu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="d3c5e-171">![Šablony ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "restyled šablony MVC 4")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="d3c5e-172">*Šablony ASP.NET MVC 4 restyled*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="d3c5e-173">![Nová stránka kontaktu](whats-new-in-aspnet-mvc-4/_static/image4.png "nový kontakt stránky")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="d3c5e-174">*Nová stránka kontaktu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-174">*New Contact page*</span></span>

    <span data-ttu-id="d3c5e-175">**Adaptivní vykreslování**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="d3c5e-176">Podívejte se na změně velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobí novou velikost okna.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="d3c5e-177">Tyto šablony pomocí adaptivního vykreslování techniku správně vykreslit, desktopových a mobilních platforem bez jakéhokoli přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="d3c5e-178">![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](whats-new-in-aspnet-mvc-4/_static/image5.png "šablonu projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="d3c5e-179">*Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="d3c5e-180">**Bohatší uživatelského rozhraní pomocí jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="d3c5e-181">Mezi další vylepšení patří do výchozí šablony projektu je použití jazyka JavaScript poskytovat vyšší interaktivitou jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="d3c5e-182">Přihlášení a zaregistrujte odkazů použité v šabloně exemplify použití jQuery ověření pro ověření vstupních polí ze strany klienta.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery ověření](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="d3c5e-184">*jQuery ověření*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-185">Všimněte si, že dvě přihlášení v oddílech v první části můžete protokolovat zaregistrovaný účet na webu a v druhé části můžete altenativelly přihlásit se pomocí jiná ověřovací služba jako google (ve výchozím nastavení vypnutá).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="d3c5e-186">Zavřete prohlížeč zastavení ladicího programu a vrátíte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="d3c5e-187">Otevřete soubor **AuthConfig.cs** umístěna ve složce **aplikace\_Start** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="d3c5e-188">Poslední řádek registrace klienta Google pro odebrání komentář *OAuth* ověřování.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="d3c5e-189">Všimněte si, že můžete snadno povolit ověřování pomocí libovolné služby OpenID nebo OAuth, jako je Facebook, Twitter, Microsoft, atd.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-189">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="d3c5e-190">Stisknutím klávesy **F5** ke spuštění řešení a přejděte na stránku pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-190">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="d3c5e-191">Vyberte **Google** službu pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-191">Select **Google** service to log in.</span></span>

    ![Výběr protokolu ve službě](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="d3c5e-193">*Výběr protokolu ve službě*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-193">*Selecting the log in service*</span></span>
10. <span data-ttu-id="d3c5e-194">Přihlaste se pomocí účtu Google.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-194">Log in using your Google account.</span></span>
11. <span data-ttu-id="d3c5e-195">Povolit server (localhost) k načtení informací z účtu Google.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-195">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="d3c5e-196">Nakonec budete muset zaregistrovat na webu, který přidružíte k účtu Google.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-196">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![Přidružení účtu Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="d3c5e-198">*Přidružení účtu Google*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-198">*Associating your Google account*</span></span>
13. <span data-ttu-id="d3c5e-199">Zavřete prohlížeč zastavení ladicího programu a vrátíte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-199">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="d3c5e-200">Teď si projděte řešení rezervovat několik nových funkcí zavedených v architektuře ASP.NET MVC 4 v šabloně projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-200">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="d3c5e-201">![Šablony projektu aplikace ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "šablonu projektu ASP.NET MVC 4 Internetové aplikace")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-201">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="d3c5e-202">*Šablona projektu ASP.NET MVC 4 Internetové aplikace*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-202">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="d3c5e-203">**HTML 5 značek**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-203">**HTML 5 Markup**</span></span>

       <span data-ttu-id="d3c5e-204">Procházejte šablony zobrazení a zjistěte, nový motiv značek.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-204">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="d3c5e-205">![Nové šablony, pomocí syntaxe Razor a HTML5 značek About.cshtml. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Novou šablonu, pomocí syntaxe Razor a HTML5 značek About.cshtml.")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-205">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="d3c5e-206">*Nové šablony pomocí značky Razor a HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-206">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="d3c5e-207">**Aktualizované knihovny jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-207">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="d3c5e-208">Výchozí šablony ASP.NET MVC 4 nyní zahrnuje KnockoutJS a architektura MVVM jazyka JavaScript, která vám umožní vytvářet bohaté s velmi rychlou odezvou webové aplikace pomocí jazyků JavaScript a HTML.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-208">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="d3c5e-209">Stejně jako v MVC3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-209">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d3c5e-210">Můžete získat další informace o knihovně KnockOutJS v tomto odkazu: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-210">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="d3c5e-211">Kromě toho se dozvíte o jQuery a uživatelské rozhraní jQuery v [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-211">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="d3c5e-212">Úloha 2 – zkoumání šablona mobilních aplikací</span><span class="sxs-lookup"><span data-stu-id="d3c5e-212">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="d3c5e-213">ASP.NET MVC 4 usnadňuje vývoj webů pro mobilní zařízení a prohlížečů tablet.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-213">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="d3c5e-214">Tato šablona má stejnou strukturu aplikace jako šablony internetovou aplikaci (Všimněte si, že kódu kontroleru je prakticky shodný), ale jeho styl byl upraven správně vykreslovat v mobilním zařízení s dotykovým ovládáním.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-214">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="d3c5e-215">Vyberte **soubor | Nové | Projekt** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-215">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d3c5e-216">V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-216">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d3c5e-217">Pojmenujte projekt **PhotoGallery.Mobile**, vyberte umístění (nebo ponechte výchozí nastavení), vyberte &quot;přidat do řešení&quot; a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-217">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="d3c5e-218">V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **aplikaci Mobile** šablony projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-218">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="d3c5e-219">Ujistěte se, že jste zvolili jako zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-219">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="d3c5e-220">![Vytvoření nové aplikace ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "vytvoření nové aplikace ASP.NET MVC 4 Mobile")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-220">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="d3c5e-221">*Vytvoření nové aplikace ASP.NET MVC 4 Mobile*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-221">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="d3c5e-222">Nyní budete moci prozkoumat řešení a podívejte se na některé z nových funkcích zavedených v architektuře ASP.NET MVC 4 šablonu řešení pro mobilní zařízení:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-222">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="d3c5e-223">**jQuery Mobile knihovny**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-223">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="d3c5e-224">Šablona projektu mobilní aplikace obsahuje mobilní knihovny jQuery, což je open source knihovna pro kompatibilitu mobilní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-224">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="d3c5e-225">jQuery Mobile postupném rozšiřování platí pro mobilní prohlížeče, které podporují šablon stylů CSS a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-225">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="d3c5e-226">Postupném rozšiřování umožňuje všechny prohlížeče zobrazíte základní obsah na webové stránce, zatímco umožňuje procesorově nejvýkonnější prohlížeče zobrazí formátovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-226">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="d3c5e-227">Soubory jazyka JavaScript a CSS součástí jQuery Mobile styl nápovědy mobilní prohlížeče a přizpůsobit obsah na obrazovce bez provedení jakékoli změny v kódu stránky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-227">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="d3c5e-229">*knihovny jQuery mobile zahrnuty v šabloně*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-229">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="d3c5e-230">**HTML5 podle značky**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-230">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="d3c5e-232">*Šablony mobilních aplikací pomocí značek HTML5, (Login.cshtml a index.cshtml)*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-232">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="d3c5e-233">Stisknutím klávesy **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-233">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="d3c5e-234">Otevřít **Windows Phone 7 emulátor**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-234">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="d3c5e-235">Na obrazovce start telefonu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-235">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="d3c5e-236">Podívejte se na adresu URL začínali desktopovou aplikaci a přejděte na tuto adresu URL z telefonu (třeba `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-236">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="d3c5e-237">Nyní máte možnost zadat přihlašovací stránky nebo se podívejte o stránce.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-237">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="d3c5e-238">Všimněte si, že styl webové stránky je založen na novou aplikaci Metro pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-238">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="d3c5e-239">Šablona projektu ASP.NET MVC 4 je správně zobrazen na mobilních zařízeních, ujistěte se, že jsou všechny prvky na stránce viditelný a povolený.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-239">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="d3c5e-240">Všimněte si, že jsou dostatečně velké na to, aby kliknul nebo klepnul na odkazy v hlavičce.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-240">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="d3c5e-241">![Projekt šablony stránky v mobilním zařízení](whats-new-in-aspnet-mvc-4/_static/image14.png "projektu šablony stránky v mobilním zařízení")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-241">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="d3c5e-242">*Šablona stránky projektu v mobilním zařízení*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-242">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="d3c5e-243">Nová šablona také používá **zobrazení metaznačku**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-243">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="d3c5e-244">Většina mobilních prohlížečů definovat šířku okna virtuálního prohlížeče nebo &quot;zobrazení&quot;, což je větší než skutečná šířka mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-244">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="d3c5e-245">To umožňuje mobilní prohlížeče pro zobrazení celé webové stránky uvnitř virtuální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-245">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="d3c5e-246">**Zobrazení metaznačku** umožňuje vývojářům nastavena na šířku, výšku a škálování oblasti prohlížeče na mobilních zařízeních **.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-246">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="d3c5e-247">Šablony ASP.NET MVC 4 pro mobilní aplikace nastaví zobrazení na šířku zařízení (&quot;width = šířka zařízení&quot;) v šabloně rozložení (*Views\Shared\_Layout.cshtml*) tak, aby všechny stránky budou mít jejich zobrazení nastavena na šířku obrazovce zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-247">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="d3c5e-248">Všimněte si, že zobrazení metaznačku nedojde ke změně výchozího zobrazení prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-248">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="d3c5e-249">Otevřít  **\_Layout.cshtml**, který je umístěn v **zobrazení | Sdílené** složky a komentář metaznačku zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-249">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="d3c5e-250">Spuštění aplikace, není-li již otevřít a prohlédněte si rozdíly.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-250">Run the application, if not already opened, and check out the differences.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

<span data-ttu-id="d3c5e-251">![Lokality po komentování metaznačku zobrazení](whats-new-in-aspnet-mvc-4/_static/image15.png "lokality po metaznačku zobrazení komentářů")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-251">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

<span data-ttu-id="d3c5e-252">*Lokality po metaznačku zobrazení komentářů*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-252">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="d3c5e-253">V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-253">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="d3c5e-254">Zrušením komentáře u metaznačku zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-254">Uncomment the viewport meta tag.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="d3c5e-255">Úloha 3 – pomocí adaptivního vykreslování</span><span class="sxs-lookup"><span data-stu-id="d3c5e-255">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="d3c5e-256">V této úloze se dozvíte jinou metodu k vykreslení stránky správně na mobilních zařízeních a webových prohlížečů ve stejnou dobu bez jakéhokoli přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-256">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="d3c5e-257">Zobrazení metaznačku jste už použili s k podobnému účelu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-257">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="d3c5e-258">Nyní bude vyhovovat jinou metodu výkonné: *adaptivního vykreslování*.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-258">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="d3c5e-259">Adaptivní vykreslování je technika, která používá **dotazy na média CSS3** přizpůsobit styl použitý pro stránku.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-259">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="d3c5e-260">Dotazy na média definovat podmínky uvnitř šablony stylů CSS styly pod určitou podmínku seskupení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-260">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="d3c5e-261">Pouze v případě, že je podmínka pravdivá, styl platí na objekty deklarované.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-261">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="d3c5e-262">Flexibilitu, kterou adaptivního vykreslování technika umožňuje jakéhokoli přizpůsobení pro zobrazení webu na různých zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-262">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="d3c5e-263">Můžete definovat tolik styly, které chcete na jedinou šablonu stylů bez psaní kódu logiku zvolte styl.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-263">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="d3c5e-264">Proto je velmi úhledné způsob přizpůsobení stylů stránky, protože snižuje množství duplicitním kódem a logiku pro účely vykreslování.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-264">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="d3c5e-265">Na druhé straně byste zvýšit využití šířky pásma, jak může nepatrně zvětšit velikost souborů šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-265">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="d3c5e-266">Pomocí adaptivního vykreslování techniku, bude váš web **zobrazovat správně, bez ohledu na to, v prohlížeči.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-266">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="d3c5e-267">Nicméně byste měli zvážit, jestli zatížení šířky pásma, navíc je důležité zajistit.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-267">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="d3c5e-268">Je základní formát dotazu médií: @media \[obor: všechny | handheld | tisk | projekce | obrazovky\] ([hodnota: vlastnosti] a... [: hodnota vlastnosti])</span><span class="sxs-lookup"><span data-stu-id="d3c5e-268">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="d3c5e-269">Příklady dotazů média: &gt;  <strong>@media všechny a (maximální šířka: 1000px) a (minimální šířka: 700px) {}:</strong> pro všechna rozlišení mezi 700px a 1000px.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-269">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="d3c5e-270"><strong>@media obrazovky a (minimální šířka: 400 px) a (maximální šířka: 700px) {…}:</strong> pouze pro obrazovky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-270"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="d3c5e-271">Rozlišení musí být v rozsahu od 400 do 700px.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-271">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="d3c5e-272"><strong>@media Ruční a (minimální šířka: 20em), obrazovky a (minimální šířka: 20em) {…}:</strong> kapesní zařízení (mobile a zařízení) a obrazovky se zadáváním.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-272"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="d3c5e-273">Minimální šířka musí být větší než 20em.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-273">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="d3c5e-274">Další informace o tomto najdete na [webu W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-274">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="d3c5e-275">Můžete se teď si projděte fungování adaptivní vykreslování, zlepšení čitelnosti ASP.NET MVC 4 výchozí šablony webu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-275">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="d3c5e-276">Otevřít **PhotoGallery.sln** řešení vytvořené v úloze 1 a vyberte **Fotogalerie** projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-276">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="d3c5e-277">Stisknutím klávesy **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-277">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="d3c5e-278">Změna velikosti prohlížeče šířku, nastavení systému windows polovina nebo méně než čtvrtletí původní velikosti.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-278">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="d3c5e-279">Všimněte si, co se stane s položkami v hlavičce: některé prvky se už nebude viditelná oblast záhlaví.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-279">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="d3c5e-280">Otevřít <strong>Site.css</strong> souboru v Průzkumníku řešení v sadě Visual Studio, umístěný ve <strong>obsahu</strong> složky projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-280">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="d3c5e-281">Stisknutím klávesy <strong>CTRL + F</strong> otevřete integrované hledání sady Visual Studio a zapisovat <strong>@media</strong> vyhledejte <strong>šablon stylů CSS media query</strong>.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-281">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="d3c5e-282">Podmínka media dotaz definovaný v této šabloně funguje takto: když je velikost okna prohlížeče níže **850 px**, pravidel šablon stylů CSS použitý jsou ty, které jsou definované v tomto bloku média.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-282">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="d3c5e-283">![Multimediální dotaz pro vyhledání](whats-new-in-aspnet-mvc-4/_static/image16.png "multimediální dotaz pro vyhledání")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-283">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="d3c5e-284">*Multimediální dotaz pro vyhledání*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-284">*Locating the media query*</span></span>
4. <span data-ttu-id="d3c5e-285">Nahraďte hodnotu maximální šířka atributu nastavit v 850 px s **10px**, aby bylo možné zakázat adaptivního vykreslování a stiskněte klávesu **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-285">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="d3c5e-286">Vraťte se do prohlížeče a stisknutím klávesy **CTRL + F5** aktualizovat stránku, provedené změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-286">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="d3c5e-287">Všimněte si rozdílů v obou stránek při úpravě šířku okna.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-287">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="d3c5e-288">![Na levé straně stránky použití @media stylu, v pravém styl vynecháte](whats-new-in-aspnet-mvc-4/_static/image17.png "na levé straně stránky použití @media stylu, v pravém styl je vynechán.")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="d3c5e-289"><em>Na levé straně stránky aplikuje @media stylu, v pravém styl je vynechán.</em></span><span class="sxs-lookup"><span data-stu-id="d3c5e-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="d3c5e-290">Teď se podíváme, co se stane, že na mobilních zařízeních:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-290">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="d3c5e-291">![Na levé straně stránky použití @media stylu, v pravém styl vynecháte](whats-new-in-aspnet-mvc-4/_static/image18.png "na levé straně stránky použití @media stylu, v pravém styl je vynechán.")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-291">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="d3c5e-292"><em>Na levé straně stránky aplikuje @media stylu, v pravém styl je vynechán.</em></span><span class="sxs-lookup"><span data-stu-id="d3c5e-292"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="d3c5e-293">I když si všimnete, že změny při vykreslování stránky ve webovém prohlížeči nejsou velmi důležité při používání mobilních zařízení budou rozdíly zřetelnější.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-293">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="d3c5e-294">Na levé straně na obrázku vidíme, že vlastní styl zlepšila čitelnost.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-294">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="d3c5e-295">V mnoha scénářích další, což usnadňuje použití podmíněného stylu na web a řešení běžných problémů styl úhledné přístupu lze použít adaptivní vykreslování.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-295">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="d3c5e-296">Zobrazení metaznačku a CSS media dotazy se netýkají technologie ASP.NET MVC 4, tak můžete využít tyto funkce v jakékoli webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-296">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="d3c5e-297">V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-297">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="d3c5e-298">Cvičení 2: Vytvoření webové aplikace Fotogalerie</span><span class="sxs-lookup"><span data-stu-id="d3c5e-298">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="d3c5e-299">V tomto cvičení bude fungovat u aplikace Fotogalerie pro zobrazení fotografií.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-299">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="d3c5e-300">Začnete pomocí šablony projektu ASP.NET MVC 4 a pak je přidána funkce k načtení fotografie ze služby a jejich zobrazení na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-300">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="d3c5e-301">V následujícím cvičení budete aktualizovat toto řešení k vylepšení způsob, jakým se zobrazí na mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-301">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="d3c5e-302">Úloha 1 – vytvoření služby Mock fotografií</span><span class="sxs-lookup"><span data-stu-id="d3c5e-302">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="d3c5e-303">V této úloze vytvoříte model služby fotografii k načtení obsahu, který se zobrazí v galerii.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-303">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="d3c5e-304">Chcete-li to provést, přidáte nový kontroler, který jednoduše vrátí soubor JSON s daty každou fotografii.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-304">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="d3c5e-305">Otevřít **sady Visual Studio** Pokud ještě není otevřen.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-305">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="d3c5e-306">Vyberte **soubor | Nové | Projekt** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-306">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="d3c5e-307">V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-307">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="d3c5e-308">Pojmenujte projekt **Fotogalerie**, vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-308">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="d3c5e-309">Alternativně můžete pokračovat v práci z existující ASP.NET MVC 4 **internetovou aplikaci** řešení z **cvičení 1** a přejděte na další krok.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-309">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="d3c5e-310">V **nového projektu ASP.NET MVC 4** dialogové okno, vyberte **internetovou aplikaci** šablony projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-310">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="d3c5e-311">Ujistěte se, že jste vybrali jako zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-311">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="d3c5e-312">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **aplikace\_Data** složky vašeho projektu a vyberte **přidat | Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-312">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="d3c5e-313">Přejděte **Source\Assets\App\_Data** složka tohoto testovacího prostředí a přidejte **Photos.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-313">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="d3c5e-314">Vytvořte nový kontroler s názvem **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-314">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="d3c5e-315">Chcete-li to provést, klikněte pravým tlačítkem na **řadiče** složku, přejděte na **přidat** a vyberte **Kontroleru.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-315">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="d3c5e-316">Úplný název kontroleru, nechat **prázdný kontroler MVC** šablonu a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-316">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="d3c5e-317">![Přidávání PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "přidání PhotoController")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-317">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="d3c5e-318">*Přidávání PhotoController*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-318">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="d3c5e-319">Nahradit **Index** metoda s tímto **Galerie** akce a vrátí obsah ze souboru JSON, které jste nedávno přidali do projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-319">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="d3c5e-320">(Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex02 - Galerie*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-320">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="d3c5e-321">Stisknutím klávesy **F5** pro spuštění řešení a pak přejděte na následující adresu URL k otestování služby imitaci fotografii: `http://localhost:[port]/photo/gallery` (hodnota [port] závisí na aktuální port, ve kterém se spustila aplikace).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-321">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="d3c5e-322">Požadavek na tuto adresu URL, načtěte obsah **Photos.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-322">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="d3c5e-323">![Testování služby imitaci fotografii](whats-new-in-aspnet-mvc-4/_static/image20.png "testování služby imitaci fotografií")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-323">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="d3c5e-324">*Testování služby imitaci fotografií*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-324">*Testing the mocked photo service*</span></span>

<span data-ttu-id="d3c5e-325">Ve skutečné implementaci může používat [rozhraní ASP.NET Web API](../../../../web-api/index.md) pro implementaci této služby Fotogalerie.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-325">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="d3c5e-326">ASP.NET Web API je architektura, která usnadňuje sestavování služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-326">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d3c5e-327">ASP.NET Web API je ideální platformu pro sestavování aplikací RESTful v rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-327">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="d3c5e-328">Úloha 2 – zobrazení galerie fotografií</span><span class="sxs-lookup"><span data-stu-id="d3c5e-328">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="d3c5e-329">V této úloze aktualizujte domovskou stránku galerie fotografií zobrazit pomocí imitaci službu, kterou jste vytvořili v prvním úkolem v tomto cvičení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-329">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="d3c5e-330">Přidáte soubory modelu a aktualizace zobrazení galerie.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-330">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="d3c5e-331">V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-331">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="d3c5e-332">Vytvořte **fotografii** třídy v **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-332">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="d3c5e-333">Chcete-li to provést, klikněte pravým tlačítkem na **modely** složky, vyberte **přidat** a klikněte na tlačítko **třídy**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-333">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="d3c5e-334">Nastavte název na **Photo.cs** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-334">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="d3c5e-335">Přidat následující členy do **fotografii** třídy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-335">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="d3c5e-336">(Fragment - kódu *modelu ASP.NET MVC 4 Lab – Ex02 – Photo*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="d3c5e-337">Otevřít **HomeController.cs** soubor **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-337">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="d3c5e-338">Přidejte následující příkazy using.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-338">Add the following using statements.</span></span>

    <span data-ttu-id="d3c5e-339">(Fragment - kódu *direktivy using HomeController Lab – Ex02 – ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-339">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="d3c5e-340">Aktualizace **Index** použití akce **HttpClient** k načtení dat Galerie a pak použít **JavaScriptSerializer** k deserializaci na model zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-340">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="d3c5e-341">(Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex02 - Index akce*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="d3c5e-342">Otevřít **Index.cshtml** soubor umístěný **Views\Home** složky a nahradit veškerý obsah následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-342">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="d3c5e-343">Tento kód projde všechny fotky načtených z této služby a zobrazí ho na Neseřazený seznam.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-343">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="d3c5e-344">(Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex02 – Photo List*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-344">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="d3c5e-345">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **obsahu** složky vašeho projektu a vyberte **přidat | Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-345">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="d3c5e-346">Přejděte **Source\Assets\Content** složka tohoto testovacího prostředí a přidejte **Site.css** souboru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-346">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="d3c5e-347">Musíte potvrdit jeho nahrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-347">You will have to confirm its replacement.</span></span> <span data-ttu-id="d3c5e-348">Pokud máte **Site.css** otevřít soubor, budete muset potvrdit také znovu načíst soubor.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-348">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="d3c5e-349">Otevřete Průzkumníka souborů a zkopírujte celý **fotografie** složky umístěna ve složce **Source\Assets** složka tohoto testovacího prostředí do kořenové složky vašeho projektu v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-349">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="d3c5e-350">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-350">Run the application.</span></span> <span data-ttu-id="d3c5e-351">Měli byste vidět na domovské stránce zobrazení fotografií v galerii.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-351">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="d3c5e-352">![Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-352">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="d3c5e-353">*Fotogalerie*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-353">*Photo Gallery*</span></span>
11. <span data-ttu-id="d3c5e-354">V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-354">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="d3c5e-355">Cvičení 3: Přidání podpory pro mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-355">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="d3c5e-356">Jednou z klíčových aktualizací v architektuře ASP.NET MVC 4 je podpora pro vývoj pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-356">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="d3c5e-357">V tomto cvičení bude prozkoumání architektury ASP.NET MVC 4 nových funkcí pro mobilní aplikace tím, že rozšíří Fotogalerie řešení, které jste vytvořili v předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-357">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="d3c5e-358">Úloha 1 – instalace jQuery Mobile v aplikaci ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d3c5e-358">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="d3c5e-359">Otevřít **začít** řešení nachází v **zdroj/EX3.-MobileSupport/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-359">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="d3c5e-360">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-360">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="d3c5e-361">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-361">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d3c5e-362">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-362">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d3c5e-363">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-363">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d3c5e-364">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-364">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d3c5e-365">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-365">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d3c5e-366">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-366">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d3c5e-367">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-367">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d3c5e-368">Otevřít **Konzola správce balíčků** kliknutím **nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**  nabídky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-368">Open the **Package Manager Console** by clicking the **Tools** > **NuGet Package Manager** > **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="d3c5e-369">![Otevření konzole Správce balíčků NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "otevření konzole Správce balíčků NuGet")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-369">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="d3c5e-370">*Otevření konzole Správce balíčků NuGet*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-370">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="d3c5e-371">V konzole Správce balíčků spusťte následující příkaz k instalaci **jQuery.Mobile.MVC** balíčku.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-371">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="d3c5e-372">jQuery Mobile je open source knihovna pro vytváření dotykového webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-372">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="d3c5e-373">Balíček NuGet jQuery.Mobile.MVC obsahuje pomocné rutiny pro použití s aplikací ASP.NET MVC 4 jQuery Mobile.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-373">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-374">Spuštěním následujícího příkazu budete stahovat jQuery.Mobile.MVC knihovny z Nuget.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-374">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="d3c5e-375">ODP.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-375">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="d3c5e-376">Tento příkaz nainstaluje jQuery Mobile a některé soubory pomocné rutiny, včetně následujících:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-376">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="d3c5e-377">**Zobrazení/Shared/\_Layout.Mobile.cshtml**: je optimalizované pro menší obrazovky založené na mobilní rozložení jQuery.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-377">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="d3c5e-378">Když na webu obdrží žádost z mobilní prohlížeče, nahradí původní rozložení (\_Layout.cshtml) s touto položkou.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-378">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="d3c5e-379">Komponenta přepínači zobrazení: se skládá z **zobrazení/Shared/\_ViewSwitcher.cshtml** částečné zobrazení a **ViewSwitcherController.cs** kontroleru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-379">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="d3c5e-380">Tato součást se zobrazí odkaz na mobilních prohlížečích umožňuje uživatelům přepínat desktopová verze stránky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-380">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="d3c5e-381">![Fotogalerie projektu s podporou mobilních](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotogalerie projekt mobilní podpora")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-381">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="d3c5e-382">*Projekt Fotogalerie mobilní podpora*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-382">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="d3c5e-383">Zaregistrujte mobilní sady.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-383">Register the Mobile bundles.</span></span> <span data-ttu-id="d3c5e-384">Chcete-li to provést, otevřete **Global.asax.cs** a přidejte následující řádek.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-384">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="d3c5e-385">(Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex03 – zaregistrovat mobilní sady*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-385">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="d3c5e-386">Spuštění aplikace klasické pracovní plochy webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-386">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="d3c5e-387">Otevřít **emulátoru pro Windows Phone 7,** umístěné v **nabídky Start | Všechny programy | Windows Phone SDK 7.1 | Emulátor Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-387">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="d3c5e-388">Na obrazovce start telefonu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-388">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="d3c5e-389">Podívejte se na adresu URL, kde je aplikace spuštěna a přejděte na tuto adresu URL s prohlížeči telefonu (třeba `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-389">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="d3c5e-390">Můžete si všimnout, že vaše aplikace bude vypadat jinak v emulátoru Windows Phone, protože jQuery.Mobile.MVC vytvořil nové prostředky ve vašem projektu, který zobrazení optimalizovány pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-390">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="d3c5e-391">Všimněte si, že zpráva v horní části telefon, zobrazuje odkaz, který se přepne do zobrazení plochy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-391">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="d3c5e-392">Kromě toho  **\_Layout.Mobile.cshtml** rozložení, který byl vytvořen balíček nainstalujete patří jiné rozložení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-392">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-393">Zatím není žádný odkaz se můžete vrátit k mobilní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-393">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="d3c5e-394">Se zahrne v pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-394">It will be included in later versions.</span></span>

    <span data-ttu-id="d3c5e-395">![Mobilní zobrazení fotografií Galerie domovské stránky](whats-new-in-aspnet-mvc-4/_static/image24.png "mobilní zobrazení fotografií Galerie domovské stránky")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-395">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="d3c5e-396">*Mobilní zobrazení fotografií Galerie domovské stránky*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-396">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="d3c5e-397">V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-397">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="d3c5e-398">Úloha 2 – Vytvoření mobilní zobrazení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-398">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="d3c5e-399">V této úloze vytvoříte mobilní verzi zobrazení indexu s obsahem přizpůsobený pro lepší appareance v mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-399">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="d3c5e-400">Kopírovat **Views\Home\Index.cshtml** zobrazení a vložte ho, chcete-li vytvořit kopii, přejmenujte nový soubor, který **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-400">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="d3c5e-401">Otevřít nové vytvoření **Index.Mobile.cshtml** zobrazení a nahraďte existující &lt;ul&gt; značky s tímto kódem.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-401">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="d3c5e-402">Tím se aktualizuje &lt;ul&gt; značka s jQuery mobilních dat poznámky k použití mobilní motivů z jQuery.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-402">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="d3c5e-403">Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-403">Notice that:</span></span>
    > 
    > - <span data-ttu-id="d3c5e-404">**Role dat** atribut nastaven na **listview** vykreslí seznamu pomocí ListView – styly.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-404">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="d3c5e-405">**Data inset** atribut nastavený na hodnotu true se zobrazí v seznamu s zakulacený okraj a okraj.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-405">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="d3c5e-406">**Filtr dat** atribut nastaven na **true** vygeneruje vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-406">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="d3c5e-407">Další informace o konvencích pro jQuery Mobile v dokumentaci k projektu: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-407">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="d3c5e-408">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-408">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="d3c5e-409">Přepněte **emulátoru Windows Phone** a aktualizovat lokalitu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-409">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="d3c5e-410">Všimněte si, že nový vzhled a chování seznamu galerie, stejně jako nové pole hledání umístěný v horní části.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-410">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="d3c5e-411">Potom zadejte do vyhledávacího pole slovo (například **Tulipány**) spustíte hledání v galerii fotografií.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-411">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="d3c5e-412">![Galerie pomocí filtrování styl listview](whats-new-in-aspnet-mvc-4/_static/image25.png "Galerie pomocí filtrování styl listview")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-412">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="d3c5e-413">*Styl listview pomocí filtrování Galerie*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-413">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="d3c5e-414">Souhrnně řečeno, jste použili předpisu Mobilizer zobrazení k vytvoření kopie zobrazení indexu s &quot;mobilní&quot; příponu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-414">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="d3c5e-415">Tato přípona znamená na ASP.NET MVC 4, že každý požadavek vygenerované z mobilního zařízení bude používat tuto kopii indexu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-415">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="d3c5e-416">Kromě toho aktualizována mobilní verzi zobrazení indexu pro použití jQuery Mobile pro zlepšení vzhledu a chování webu v mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-416">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="d3c5e-417">Vraťte se zpět do sady Visual Studio a otevřete **Site.Mobile.css** umístěna ve složce **obsahu** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-417">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="d3c5e-418">Opravte umístění název fotografie zobrazí na pravé straně bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-418">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="d3c5e-419">Chcete-li to provést, přidejte následující kód k **Site.Mobile.css** souboru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-419">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="d3c5e-420">CSS</span><span class="sxs-lookup"><span data-stu-id="d3c5e-420">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="d3c5e-421">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-421">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="d3c5e-422">Přepněte zpět **emulátoru Windows Phone** a aktualizovat lokalitu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-422">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="d3c5e-423">Všimněte si, že název fotografie správně je nyní umístěn.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-423">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="d3c5e-424">![Název umístěn na pravé straně obrázku](whats-new-in-aspnet-mvc-4/_static/image26.png "název umístěn na pravé straně bitové kopie")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-424">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="d3c5e-425">*Název na pravé straně bitové kopie*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-425">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="d3c5e-426">Úloha 3 – motivy jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="d3c5e-426">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="d3c5e-427">Každý rozložení a widget jQuery Mobile navržené s ohledem na nové šablony stylů CSS framework objektově orientované, která umožňuje použít kompletní jednotné vizuálním návrhem motiv k webům a aplikacím.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-427">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="d3c5e-428">jQuery Mobile výchozí motiv zahrnuje 5 vzorník, která jsou uvedena písmena (a, b, c, d e) pro rychlou referenci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-428">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="d3c5e-429">V této úloze budete aktualizovat mobilní rozložení chcete použít jiný motiv než výchozí.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-429">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="d3c5e-430">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-430">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d3c5e-431">Otevřít  **\_Layout.Mobile.cshtml** soubor umístěný ve **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-431">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="d3c5e-432">Najděte prvek div s nastavena na role dat &quot;stránky&quot; a aktualizovat **data-theme** k &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-432">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="d3c5e-433">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-433">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="d3c5e-434">Aktualizace v lokalitě **emulátoru Windows Phone** a Všimněte si, že nové schéma barvy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-434">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="d3c5e-435">![Mobilní rozložení se schématem jinou barvu](whats-new-in-aspnet-mvc-4/_static/image27.png "mobilní rozložení se schématem odlišnou barvou")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-435">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="d3c5e-436">*Mobilní rozložení se schématem odlišnou barvou*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-436">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="d3c5e-437">Úloha 4 – součást přepínači zobrazení a prohlížeče přepsání funkce</span><span class="sxs-lookup"><span data-stu-id="d3c5e-437">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="d3c5e-438">Konvence pro webové stránky optimalizovaných pro mobilní zařízení se přidat odkaz, jejichž text je něco jako zobrazení plochy nebo režim celý web, který umožňuje uživatelům přepínat desktopová verze stránky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-438">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="d3c5e-439">Balíček jQuery.Mobile.MVC obsahuje ukázkový **přepínači zobrazení** komponenty pro tento účel použít v  **\_Layout.Mobile.cshtml** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-439">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="d3c5e-440">![Odkazu k přepnutí na zobrazení plochy](whats-new-in-aspnet-mvc-4/_static/image28.png "odkaz k přepnutí na zobrazení plochy")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-440">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="d3c5e-441">*Odkaz k přepnutí na zobrazení plochy*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-441">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="d3c5e-442">Přepínání zobrazení používá novou funkci nazvanou **přepsání prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-442">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="d3c5e-443">Tato funkce umožňuje aplikaci zacházet se žádostmi, jako kdyby kdyby přicházely z jiného prohlížeče (uživatelského agenta) než ten, který ve skutečnosti přicházejí z.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-443">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="d3c5e-444">V této úloze bude prozkoumat ukázková implementace přepínači zobrazení přidal jQuery.Mobile.MVC a nový prohlížeč přepsání funkce v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-444">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="d3c5e-445">Přepněte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-445">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d3c5e-446">Otevřít  **\_Layout.Mobile.cshtml** zobrazení umístěna ve složce **Views\Shared** složky a Všimněte si, že součást přepínači zobrazení je odkazováno jako částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-446">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="d3c5e-447">![Mobilní rozložení pomocí komponenty přepínači zobrazení](whats-new-in-aspnet-mvc-4/_static/image29.png "mobilní rozložení pomocí komponenty přepínači zobrazení")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-447">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="d3c5e-448">*Mobilní rozložení pomocí komponenty přepínači zobrazení*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-448">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="d3c5e-449">Otevřít  **\_ViewSwitcher.cshtml** částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-449">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="d3c5e-450">Částečné zobrazení používá novou metodu **ViewContext.HttpContext.GetOverriddenBrowser()** určit původ na webový požadavek a zobrazit na příslušný odkaz přejděte buď do zobrazení Desktop nebo Mobile.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-450">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="d3c5e-451">**GetOverridenBrowser** vrátí metoda **HttpBrowserCapabilitiesBase** instanci, která odpovídá pro uživatelského agenta, který je aktuálně nastavený pro žádost (skutečné nebo přepsané).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-451">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="d3c5e-452">Tato hodnota slouží k získání vlastností, jako **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-452">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="d3c5e-453">![Částečné zobrazení ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher částečného zobrazení")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-453">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="d3c5e-454">*ViewSwitcher částečného zobrazení*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-454">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="d3c5e-455">Otevřít **ViewSwitcherController.cs** třídy umístěny v **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-455">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="d3c5e-456">Podívejte se na tuto funkci SwitchView akci je volán na odkaz v komponentě ViewSwitcher a Všimněte si nových metod objektu HttpContext.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-456">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="d3c5e-457">**HttpContext.ClearOverridenBrowser()** metoda odebere každého přepsaného uživatelského agenta pro aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-457">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="d3c5e-458">**HttpContext.SetOverridenBrowser()** metoda přepíše hodnotu skutečného uživatelského agenta žádosti pomocí zadaného uživatelského agenta.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-458">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="d3c5e-459">![Kontroler ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Kontroleru")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-459">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="d3c5e-460">*ViewSwitcher Kontroleru*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-460">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="d3c5e-461">Přepíše prohlížeče je funkce jádra ASP.NET MVC 4, což je také k dispozici i v případě, že není nainstalovaný balíček jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-461">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="d3c5e-462">Ale tuto funkci ovlivňuje pouze zobrazení, rozložení a částečného zobrazení a nemá žádnou z funkcí, které jsou závislé na objektu Request.Browser vliv.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-462">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="d3c5e-463">Úloha 5: Přidání přepínači zobrazení v zobrazení plochy</span><span class="sxs-lookup"><span data-stu-id="d3c5e-463">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="d3c5e-464">V této úloze budete aktualizovat rozložení pro počítače zahrnout přepínači zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-464">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="d3c5e-465">To vám umožní mobilní uživatelé se vrátíte do zobrazení mobilní, při procházení zobrazení plochy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-465">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="d3c5e-466">Aktualizace v lokalitě **emulátoru Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-466">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="d3c5e-467">Klikněte na **zobrazení plochy** odkazu v horní části v galerii.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-467">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="d3c5e-468">Všimněte si, že není žádná přepínači zobrazení v zobrazení plochy tak, aby umožnily že vrátit mobilní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-468">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="d3c5e-469">Vraťte se zpět do sady Visual Studio a otevřete  **\_Layout.cshtml** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-469">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="d3c5e-470">Najít v části přihlášení a vložit volání pro vykreslení  **\_ViewSwitcher** částečné zobrazení níže  **\_LogOnPartial** částečné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-470">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="d3c5e-471">Poté stiskněte **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-471">Then, press **CTRL + S** to save the changes.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="d3c5e-472">Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-472">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="d3c5e-473">Aktualizujte stránku v emulátoru Windows Phone a dvakrát klikněte na obrazovce přiblížit.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-473">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="d3c5e-474">Všimněte si, že na domovské stránce se teď zobrazí **mobilní zobrazení** odkaz, který od mobilních se přepne do zobrazení plochy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-474">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="d3c5e-475">![Zobrazit přepínání zobrazení v zobrazení plochy](whats-new-in-aspnet-mvc-4/_static/image32.png "přepínači zobrazení se zobrazují v zobrazení plochy")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-475">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="d3c5e-476">*Přepínání zobrazení se zobrazují v zobrazení plochy*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-476">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="d3c5e-477">Znovu přejděte do zobrazení mobilních a přejděte do <strong>o</strong> stránky (http://localhost[port] / Home/o).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-477">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="d3c5e-478">Všimněte si, že i v případě, že jste ještě nevytvořili pohledu About.Mobile.cshtml, o stránka se zobrazí pomocí mobilní rozložení (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-478">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="d3c5e-479">![O stránku](whats-new-in-aspnet-mvc-4/_static/image33.png "o stránce")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-479">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="d3c5e-480">*O stránku*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-480">*About page*</span></span>
8. <span data-ttu-id="d3c5e-481">Nakonec otevřete web ve webovém prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-481">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="d3c5e-482">Všimněte si, že žádná z předchozích aktualizací má vliv na zobrazení plochy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-482">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="d3c5e-483">![Zobrazení plochy Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotogalerie zobrazení plochy")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-483">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="d3c5e-484">*Zobrazení plochy Fotogalerie*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-484">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="d3c5e-485">Krok 6 – Vytvoření nového režimů zobrazení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-485">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="d3c5e-486">Nová funkce režimy zobrazení umožní aplikaci vyberte zobrazení v závislosti na prohlížeči, který generuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-486">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="d3c5e-487">Například pokud prohlížeč pro stolní počítač požadavků na domovskou stránku, aplikace se vrátí **Views\Home\Index.cshtml** šablony.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-487">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="d3c5e-488">Potom, pokud mobilního prohlížeče požadavků na domovskou stránku, aplikaci vrátí **Views\Home\Index.mobile.cshtml** šablony.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-488">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="d3c5e-489">V této úloze vytvoříte vlastní rozložení pro zařízení iPhone a budete muset simulovat žádosti v zařízení iPhone.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-489">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="d3c5e-490">K tomuto účelu můžete použít buď emulátor nebo simulátor pro zařízení iPhone (jako je [Electric simulátor Mobile](http://www.electricplum.com/)) nebo v prohlížeči pomocí doplňků, které upravují uživatelského agenta.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-490">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="d3c5e-491">Pokyny o tom, jak nastavit v prohlížeči Safari identifikační řetězec prohlížeče pro emulaci Iphonu najdete v tématu [jak chcete, aby Safari, které předstírají, že je aplikace Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison blogu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-491">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="d3c5e-492">**Všimněte si, že tato úloha je volitelné a můžete pokračovat v testovacím prostředí bez ale jeho spuštění.**</span><span class="sxs-lookup"><span data-stu-id="d3c5e-492">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="d3c5e-493">V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-493">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="d3c5e-494">Otevřít **Global.asax.cs** a přidejte následující příkaz using.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-494">Open **Global.asax.cs** and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="d3c5e-495">Přidejte následující zvýrazněný kód do aplikace\_začátek metody.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-495">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="d3c5e-496">(Fragment - kódu *ASP.NET MVC 4 Lab – Ex03 – iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-496">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

<span data-ttu-id="d3c5e-497">Jste si zaregistrovali nový **DefaultDisplayMode s názvem &quot;iPhone&quot;**, v rámci statických **DisplayModeProvider.Instance.Modes** statické seznamu, bude hledána jednotlivých příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-497">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="d3c5e-498">Pokud příchozí požadavek obsahuje řetězec &quot;iPhone&quot;, ASP.NET MVC najdete zobrazení, jejichž název obsahuje &quot;iPhone&quot; příponu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-498">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="d3c5e-499">Parametr 0 označuje, jak se konkrétní nový režim; například toto zobrazení je konkrétnější než Obecné &quot;.mobile&quot; pravidlo, které odpovídá požadavky mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-499">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

<span data-ttu-id="d3c5e-500">Jakmile tento kód se spustí, když prohlížeč zařízení iPhone vygeneruje žádost, bude aplikace používat **Views\Shared\\_Layout.iPhone.cshtml** rozložení, které vytvoříte v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-500">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

> [!NOTE]
> <span data-ttu-id="d3c5e-501">Tento způsob testování požadavku pro iPhone je jednodušší pro účely ukázky a nemusí fungovat podle očekávání pro každý řetězec uživatelského agenta pro iPhone (pro příklad testu je velká a malá písmena).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-501">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>

4. <span data-ttu-id="d3c5e-502">Vytvoření kopie  **\_Layout.Mobile.cshtml** soubor **Views\Shared** složku a přejmenujte ji na &quot; **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-502">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="d3c5e-503">Otevřít  **\_Layout.iPhone.csthml** jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-503">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="d3c5e-504">Najít div element s atribut data-role nastaven na **stránky** a změňte **data-theme** atribut &quot; **a**&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-504">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

<span data-ttu-id="d3c5e-505">Nyní máte 3 rozložení v aplikaci ASP.NET MVC 4:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-505">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

1. <span data-ttu-id="d3c5e-506">**\_Layout.cshtml**: výchozí rozložení se používá u stolních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-506">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
2. <span data-ttu-id="d3c5e-507">**\_Layout.Mobile.cshtml**: výchozí rozložení pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-507">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
3. <span data-ttu-id="d3c5e-508">**\_Layout.iPhone.cshtml**: specifické rozložení pro zařízení iPhone, pomocí jiné barevné schéma k odlišení od \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-508">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="d3c5e-509">Stisknutím klávesy **F5** ke spuštění aplikace a procházení webu v **emulátoru Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-509">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="d3c5e-510">Otevřít **simulátoru Iphonu** (naleznete v tématu [dodatku C](#AppendixC) pokyny o tom, jak nainstalovat a nakonfigurovat simulátor pro zařízení iPhone) a přejděte na web příliš.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-510">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="d3c5e-511">Všimněte si, že každý phone používá konkrétní šablonu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-511">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="d3c5e-513">*Použití různá zobrazení pro jednotlivá mobilní zařízení*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-513">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="d3c5e-514">Cvičení 4: Použití asynchronní řadiče</span><span class="sxs-lookup"><span data-stu-id="d3c5e-514">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="d3c5e-515">Microsoft .NET Framework 4.5 zavádí nové funkce jazyků C# a Visual Basic a poskytuje novou platformu pro asynchronii v programování rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-515">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="d3c5e-516">Tento nový foundation umožňuje asynchronní programování podobný – a přibližně stejně jednoduché jako - synchronní programování.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-516">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="d3c5e-517">Nyní budete moci napsat metody asynchronní akce v architektuře ASP.NET MVC 4 s využitím **AsyncController** třídy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-517">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="d3c5e-518">Můžete použít asynchronní akce metody pro dlouho běžící, procesor vázán požadavky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-518">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="d3c5e-519">Tím je zabráněno zablokování webového serveru z provádění práce během zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-519">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="d3c5e-520">Třída AsyncController se obvykle používá pro dlouhého volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-520">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="d3c5e-521">V tomto cvičení vysvětluje základy tohoto asynchronní operace v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-521">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="d3c5e-522">Pokud se chcete dozvědět více o, podívejte se na následující článek: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-522">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="d3c5e-523">Úloha 1 – implementace asynchronní kontroler</span><span class="sxs-lookup"><span data-stu-id="d3c5e-523">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="d3c5e-524">Otevřít **začít** řešení nachází v **zdroj/Ex4-Async/počáteční/** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-524">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="d3c5e-525">V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-525">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="d3c5e-526">Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-526">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d3c5e-527">Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-527">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d3c5e-528">V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-528">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d3c5e-529">Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-529">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d3c5e-530">Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-530">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d3c5e-531">Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-531">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d3c5e-532">To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-532">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="d3c5e-533">Otevřít **HomeController.cs** třídy z **řadiče** složky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-533">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="d3c5e-534">Přidejte následující příkaz using.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-534">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="d3c5e-535">Aktualizace **HomeController** třída dědí **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-535">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="d3c5e-536">Kontrolery, které jsou odvozeny z AsyncController povolit technologii ASP.NET ke zpracování asynchronních žádostí a mohou stále služby akce synchronní metody.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-536">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="d3c5e-537">Přidat **asynchronní** – klíčové slovo do **Index** metoda a nastavte ji typ vrácené **úloh&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-537">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="d3c5e-538">**Asynchronní** – klíčové slovo je jednou z nových klíčových slov poskytuje rozhraní .NET Framework 4.5, sděluje kompilátoru, že tato metoda obsahuje asynchronní kód.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-538">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="d3c5e-539">A **úloh** objekt představuje asynchronní operaci, která mohou být dokončeny v určitém okamžiku v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-539">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="d3c5e-540">Nahradit **klienta. GetAsync()** volání s použitím úplné asynchronní verze – klíčové slovo await, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-540">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="d3c5e-541">(Fragment - kódu *GetAsync Lab – Ex04 – ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-541">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="d3c5e-542">V předchozí verzi, kterou jste používali **výsledek** vlastnost z **úloh** objekt blokovat vlákno, dokud se výsledek se vrátí (verze synchronizace %).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-542">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="d3c5e-543">Přidávání **await** – klíčové slovo instruuje kompilátor, aby asynchronní čekání na úlohu vrácená z volání metody.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-543">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="d3c5e-544">To znamená, že zbytek kódu se spustí jako zpětné volání až po dokončení očekávané metody.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-544">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="d3c5e-545">Další věc a Všimněte si je, že není potřeba změnit váš bloku try-catch – aby bylo možné tuto práci: bez další práce pomocí obslužnou rutinu poskytovaného rámcem bude stále zachycena výjimky, ke kterým dochází v pozadí nebo v popředí.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-545">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="d3c5e-546">Změňte kód a pokračujte asynchronní provádění nahrazením řádky s novým kódem, jak je znázorněno níže</span><span class="sxs-lookup"><span data-stu-id="d3c5e-546">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="d3c5e-547">(Fragment - kódu *ReadAsStringAsync Lab – Ex04 – ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-547">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="d3c5e-548">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-548">Run the application.</span></span> <span data-ttu-id="d3c5e-549">Uvidíte žádné větší změny, ale váš kód neblokuje vlákno z fondu vláken provádění lepší využití serverových prostředků a zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-549">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-550">Další informace o nových funkcích asynchronního programování v testovacím prostředí &quot; **asynchronní programování v rozhraní .NET 4.5 s C# a Visual Basic** &quot; součástí této školicí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-550">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="d3c5e-551">Úloha 2 – zpracování vypršení časových limitů s tokeny zrušení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-551">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="d3c5e-552">Metody asynchronní akce, které vracejí instance může také podporovat vypršení časových limitů.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-552">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="d3c5e-553">V této úloze budete aktualizovat Index metoda kód pro zpracování vypršení časového limitu scénář pomocí tokenu zrušení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-553">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="d3c5e-554">Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-554">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="d3c5e-555">Přidejte následující příkaz k použití **HomeController.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-555">Add the following using statement to the **HomeController.cs** file.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="d3c5e-556">Akce indexu přijímat aktualizace **CancellationToken** argument.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-556">Update the Index action to receive a **CancellationToken** argument.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="d3c5e-557">Aktualizace **GetAsync** volání předat token zrušení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-557">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="d3c5e-558">(Fragment - kódu *SendAsync Lab – Ex04 – ASP.NET MVC 4 s CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-558">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="d3c5e-559">Uspořádání *Index* metodou **hodnota vlastnosti AsyncTimeout** atribut nastaven na hodnotu 500 milisekund a **HandleError** nakonfigurovaný pro zpracování atributu  **TaskCanceledException** přesměrováním na **vypršel časový limit** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-559">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="d3c5e-560">(Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex04 – atributy*)</span><span class="sxs-lookup"><span data-stu-id="d3c5e-560">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="d3c5e-561">Otevřít **PhotoController** třídy a aktualizace **Galerie** metoda zpoždění spuštění 1000 milisekund (1 sekunda) pro simulaci dlouho běžící úloha.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-561">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="d3c5e-562">Otevřít **Web.config** souboru a povolit tak, že přidáte následující element vlastních chyb.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-562">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="d3c5e-563">Vytvoření nového zobrazení v **Views\Shared** s názvem **vypršel časový limit** a použijte výchozí rozložení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-563">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="d3c5e-564">V Průzkumníku řešení klikněte pravým tlačítkem myši **Views\Shared** a pak zvolte položku **přidat | Zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-564">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="d3c5e-565">![Použití různá zobrazení pro jednotlivá mobilní zařízení,](whats-new-in-aspnet-mvc-4/_static/image36.png "pomocí různá zobrazení pro jednotlivá mobilní zařízení")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-565">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="d3c5e-566">*Použití různá zobrazení pro jednotlivá mobilní zařízení*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-566">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="d3c5e-567">Aktualizace **vypršel časový limit** zobrazit obsah, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-567">Update the **TimedOut** view content as shown below.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="d3c5e-568">Spusťte aplikaci a přejděte do kořenové adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-568">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="d3c5e-569">Jak jste přidali **Thread.Sleep** 1000 milisekund, získáte k vypršení časového limitu, vygenerovaná **hodnota vlastnosti AsyncTimeout** atribut a zachytit pomocí **HandleError** atribut.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-569">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="d3c5e-570">![Výjimka časového limitu zpracování](whats-new-in-aspnet-mvc-4/_static/image37.png "zpracována výjimka časového limitu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-570">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="d3c5e-571">*Výjimka časového limitu zpracování*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-571">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="d3c5e-572">Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha D: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-572">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d3c5e-573">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d3c5e-573">Summary</span></span>

<span data-ttu-id="d3c5e-574">V tomto praktická-testovací prostředí jste jsme zaznamenali některé nové funkce v architektuře ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-574">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="d3c5e-575">Projednat následující pojmy:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-575">The following concepts have been discussed:</span></span>

- <span data-ttu-id="d3c5e-576">Využijte výhod rozšíření, která chcete šablony – včetně projektu ASP.NET MVC nové šablony projektů mobilních aplikací</span><span class="sxs-lookup"><span data-stu-id="d3c5e-576">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="d3c5e-577">Použít atribut zobrazení HTML5 a CSS media dotazy ke zlepšení zobrazení na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="d3c5e-577">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="d3c5e-578">Použít jQuery Mobile pro progresivní vylepšení a vytváření dotykového webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d3c5e-578">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="d3c5e-579">Vytvořit zobrazení specifické pro mobilní zařízení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-579">Create mobile-specific views</span></span>
- <span data-ttu-id="d3c5e-580">Chcete-li přepnout mezi mobilními a desktopovými zobrazení v aplikaci použijte komponentu přepínači zobrazení</span><span class="sxs-lookup"><span data-stu-id="d3c5e-580">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="d3c5e-581">Vytvoření asynchronní řadiče pomocí podpory úkolu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-581">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="d3c5e-582">Příloha A: používání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-582">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="d3c5e-583">Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-583">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d3c5e-584">Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-584">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d3c5e-585">![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](whats-new-in-aspnet-mvc-4/_static/image38.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-585">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d3c5e-586">*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-586">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="d3c5e-587">***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***</span><span class="sxs-lookup"><span data-stu-id="d3c5e-587">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="d3c5e-588">Umístěte kurzor, kam chcete vložit kód.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-588">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d3c5e-589">Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-589">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d3c5e-590">Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-590">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d3c5e-591">Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-591">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d3c5e-592">Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-592">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="d3c5e-593">![Začněte psát název fragmentu kódu](whats-new-in-aspnet-mvc-4/_static/image39.png "začněte psát název fragmentu kódu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-593">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="d3c5e-594">*Začněte psát název fragmentu kódu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-594">*Start typing the snippet name*</span></span>

<span data-ttu-id="d3c5e-595">![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](whats-new-in-aspnet-mvc-4/_static/image40.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-595">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="d3c5e-596">*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-596">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="d3c5e-597">![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](whats-new-in-aspnet-mvc-4/_static/image41.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-597">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="d3c5e-598">*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-598">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="d3c5e-599">***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)***</span><span class="sxs-lookup"><span data-stu-id="d3c5e-599">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="d3c5e-600">Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-600">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="d3c5e-601">Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-601">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="d3c5e-602">Kliknutím na vyberte relevantní fragment kódu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-602">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="d3c5e-603">![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](whats-new-in-aspnet-mvc-4/_static/image42.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-603">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="d3c5e-604">*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-604">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="d3c5e-605">![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](whats-new-in-aspnet-mvc-4/_static/image43.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-605">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="d3c5e-606">*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-606">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d3c5e-607">Příloha B: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="d3c5e-607">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d3c5e-608">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-608">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d3c5e-609">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-609">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d3c5e-610">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-610">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d3c5e-611">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-611">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="d3c5e-612">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-612">Click on **Install Now**.</span></span> <span data-ttu-id="d3c5e-613">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-613">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d3c5e-614">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-614">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d3c5e-615">![Instalace sady Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-615">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d3c5e-616">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-616">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d3c5e-617">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-617">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="d3c5e-619">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-619">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d3c5e-620">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-620">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="d3c5e-622">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-622">*Installation progress*</span></span>
6. <span data-ttu-id="d3c5e-623">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-623">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="d3c5e-625">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-625">*Installation completed*</span></span>
7. <span data-ttu-id="d3c5e-626">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-626">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d3c5e-627">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-627">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="d3c5e-629">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-629">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="d3c5e-630">Příloha C: instalace služby WebMatrix 2 a simulátoru Iphonu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-630">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="d3c5e-631">Provoz vašeho webu v Iphonu s Simulovaná zařízení můžete použít rozšíření prostředí WebMatrix &quot;Electric simulátor Mobile pro iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-631">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="d3c5e-632">Navíc můžete nakonfigurovat stejnou příponu ke spuštění simulátoru ze sady Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-632">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="d3c5e-633">Úloha 1 – instalace služby WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="d3c5e-633">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="d3c5e-634">Přejděte na [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-634">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d3c5e-635">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>WebMatrix 2</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-635">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="d3c5e-636">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-636">Click on **Install Now**.</span></span> <span data-ttu-id="d3c5e-637">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-637">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d3c5e-638">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-638">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d3c5e-639">![Instalace služby WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalace služby WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-639">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="d3c5e-640">*Instalace služby WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-640">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="d3c5e-641">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-641">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="d3c5e-642">![Přijetí podmínek licence](whats-new-in-aspnet-mvc-4/_static/image50.png "přijetí podmínek licence")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-642">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="d3c5e-643">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-643">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d3c5e-644">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-644">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="d3c5e-645">![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image51.png "průběh instalace")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-645">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="d3c5e-646">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-646">*Installation progress*</span></span>
6. <span data-ttu-id="d3c5e-647">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-647">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="d3c5e-648">![Instalace byla dokončena](whats-new-in-aspnet-mvc-4/_static/image52.png "instalace byla dokončena.")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-648">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="d3c5e-649">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-649">*Installation completed*</span></span>
7. <span data-ttu-id="d3c5e-650">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-650">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="d3c5e-651">Úloha 2 – instalace rozšíření simulátoru Iphonu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-651">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="d3c5e-652">Spustit **WebMatrix** a otevřít všechny existující webovou stránku nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-652">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="d3c5e-653">Klikněte na tlačítko **spustit** tlačítko **Domů** pásu karet a vyberte **přidat nový**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-653">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="d3c5e-654">![Přidání nové rozšíření prostředí WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "přidání nové rozšíření nástroje WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-654">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="d3c5e-655">*Přidání nové rozšíření nástroje WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-655">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="d3c5e-656">Vyberte **iPhone simulátor** a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-656">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="d3c5e-657">![Procházení rozšíření nástroje WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "rozšíření nástroje WebMatrix procházení")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-657">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="d3c5e-658">*Procházení rozšíření nástroje WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-658">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="d3c5e-659">Podrobnosti balíčku, klikněte na tlačítko **nainstalovat** pokračujte v instalaci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-659">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="d3c5e-660">![iPhone simulátor rozšíření](whats-new-in-aspnet-mvc-4/_static/image55.png "rozšíření simulátoru Iphonu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-660">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="d3c5e-661">*rozšíření simulátoru Iphonu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-661">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="d3c5e-662">Přečtěte si a přijměte smlouvu EULA rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-662">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="d3c5e-663">![Rozšíření prostředí WebMatrix smlouva EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "rozšíření prostředí WebMatrix smlouvy EULA")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-663">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="d3c5e-664">*Rozšíření prostředí WebMatrix smlouvy EULA*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-664">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="d3c5e-665">Teď můžete spouštět webu ze služby WebMatrix pomocí možnosti simulátoru Iphonu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-665">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="d3c5e-666">![Spuštění pomocí Iphonu](whats-new-in-aspnet-mvc-4/_static/image57.png "spuštění pomocí Iphonu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-666">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="d3c5e-667">*Spuštění pomocí Iphonu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-667">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="d3c5e-668">Úloha 3 – konfigurace sady Visual Studio 2012 ke spuštění simulátoru Iphonu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-668">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="d3c5e-669">Otevřete **Visual Studio 2012** a otevřít libovolný web, nebo vytvořte nový projekt.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-669">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="d3c5e-670">Klikněte na šipku dolů na panelu spuštění a vyberte **procházet s**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-670">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="d3c5e-671">![Procházení s](whats-new-in-aspnet-mvc-4/_static/image58.png "nastavit prohlížeče")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-671">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="d3c5e-672">*Nastavit prohlížeče*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-672">*Browse with*</span></span>
3. <span data-ttu-id="d3c5e-673">V &quot;procházet s&quot; dialogového okna, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-673">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="d3c5e-674">V &quot;přidat Program&quot; dialogového okna, použijte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-674">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="d3c5e-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe \* (aktualizovat cestu odpovídajícím způsobem)</em></span><span class="sxs-lookup"><span data-stu-id="d3c5e-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe \*(update the path accordingly)</em></span></span>
   - <span data-ttu-id="d3c5e-676">**Argumenty**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="d3c5e-676">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="d3c5e-677">**Popisný název**: simulátoru Iphonu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-677">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="d3c5e-678">![Přidat program](whats-new-in-aspnet-mvc-4/_static/image59.png "přidat program")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-678">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="d3c5e-679">*Přidat program pro procházení s*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-679">*Add program to browse with*</span></span>
5. <span data-ttu-id="d3c5e-680">Klikněte na tlačítko **OK** a zavřete dialogová okna.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-680">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="d3c5e-681">Nyní budete moci spouštět webové aplikace v simulátoru Iphonu ze sady Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-681">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="d3c5e-682">![Procházení pomocí Iphonu simulátor](whats-new-in-aspnet-mvc-4/_static/image60.png "procházet pomocí simulátoru Iphonu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-682">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="d3c5e-683">*Procházení s simulátoru Iphonu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-683">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d3c5e-684">Příloha D: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-684">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="d3c5e-685">Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-685">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="d3c5e-686">Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3c5e-686">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="d3c5e-687">Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-687">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-688">Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-688">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="d3c5e-689">Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-689">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="d3c5e-690">![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-690">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="d3c5e-691">*Přihlaste se k portálu pro správu Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-691">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="d3c5e-692">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-692">Click **New** on the command bar.</span></span>

    <span data-ttu-id="d3c5e-693">![Vytvoření nového webu](whats-new-in-aspnet-mvc-4/_static/image62.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-693">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="d3c5e-694">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-694">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="d3c5e-695">Klikněte na tlačítko **Compute** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-695">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="d3c5e-696">Potom vyberte **rychlé vytvoření** možnost.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-696">Then select **Quick Create** option.</span></span> <span data-ttu-id="d3c5e-697">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-697">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-698">Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-698">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="d3c5e-699">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-699">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="d3c5e-700">Nezahrnuje kroky pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-700">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="d3c5e-701">![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-mvc-4/_static/image63.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-701">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="d3c5e-702">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-702">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="d3c5e-703">Počkejte, dokud nové **webu** se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-703">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="d3c5e-704">Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-704">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="d3c5e-705">Zkontrolujte, jestli funguje nový web.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-705">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="d3c5e-706">![Na nový web](whats-new-in-aspnet-mvc-4/_static/image64.png "přechodu na nový web")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-706">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="d3c5e-707">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-707">*Browsing to the new web site*</span></span>

    <span data-ttu-id="d3c5e-708">![Spuštění webu](whats-new-in-aspnet-mvc-4/_static/image65.png "spuštění webové stránky")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-708">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="d3c5e-709">*Spuštění webové stránky*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-709">*Web site running*</span></span>
6. <span data-ttu-id="d3c5e-710">Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-710">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="d3c5e-711">![Otevřete správu webových stránek](whats-new-in-aspnet-mvc-4/_static/image66.png "otevřete správu webových stránek")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-711">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="d3c5e-712">*Otevřete správu webových stránek*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-712">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="d3c5e-713">V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-713">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-714">*Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-714">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="d3c5e-715">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-715">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="d3c5e-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="d3c5e-717">![Stahování webové stránky publikovat profil](whats-new-in-aspnet-mvc-4/_static/image67.png "stahování webové stránky profil publikování")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-717">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="d3c5e-718">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-718">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="d3c5e-719">Stáhněte si soubor profilu publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-719">Download the publish profile file to a known location.</span></span> <span data-ttu-id="d3c5e-720">Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-720">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="d3c5e-721">![Ukládání souboru profilu publikování](whats-new-in-aspnet-mvc-4/_static/image68.png "ukládá se profil publikování")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-721">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="d3c5e-722">*Ukládá se profil publikování*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-722">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="d3c5e-723">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="d3c5e-723">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="d3c5e-724">Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-724">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="d3c5e-725">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-725">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="d3c5e-726">Pro uložení databáze aplikace budete potřebovat databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-726">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="d3c5e-727">Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-727">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="d3c5e-728">Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-728">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="d3c5e-729">Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-729">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="d3c5e-730">Nevytvářet databáze, protože vytvoří se v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-730">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="d3c5e-731">![Řídicí panel serveru SQL Database](whats-new-in-aspnet-mvc-4/_static/image69.png "řídicího panelu serveru SQL Database")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-731">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="d3c5e-732">*Řídicí panel serveru SQL Database*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-732">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="d3c5e-733">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-733">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="d3c5e-734">Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-734">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Přidat IP adresu klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="d3c5e-736">*Přidat IP adresu klienta*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-736">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="d3c5e-737">Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-737">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="d3c5e-739">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-739">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d3c5e-740">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="d3c5e-740">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="d3c5e-741">Vraťte se do řešení ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-741">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="d3c5e-742">V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-742">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="d3c5e-743">![Publikování aplikace](whats-new-in-aspnet-mvc-4/_static/image73.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-743">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="d3c5e-744">*Publikování na webu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-744">*Publishing the web site*</span></span>
2. <span data-ttu-id="d3c5e-745">Importujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-745">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="d3c5e-746">![Import profilu publikování](whats-new-in-aspnet-mvc-4/_static/image74.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-746">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="d3c5e-747">*Import publikačního profilu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-747">*Importing publish profile*</span></span>
3. <span data-ttu-id="d3c5e-748">Klikněte na tlačítko **ověřit připojení**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-748">Click **Validate Connection**.</span></span> <span data-ttu-id="d3c5e-749">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-749">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d3c5e-750">Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-750">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="d3c5e-751">![Ověřuje se připojení](whats-new-in-aspnet-mvc-4/_static/image75.png "ověřuje se připojení")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-751">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="d3c5e-752">*Ověřuje se připojení*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-752">*Validating connection*</span></span>
4. <span data-ttu-id="d3c5e-753">V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="d3c5e-753">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="d3c5e-754">![Konfigurace nasazení webu](whats-new-in-aspnet-mvc-4/_static/image76.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-754">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="d3c5e-755">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-755">*Web deploy configuration*</span></span>
5. <span data-ttu-id="d3c5e-756">Konfigurace připojení k databázi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d3c5e-756">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="d3c5e-757">V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-757">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="d3c5e-758">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-758">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="d3c5e-759">V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-759">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="d3c5e-760">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-760">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="d3c5e-761">![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-mvc-4/_static/image77.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-761">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="d3c5e-762">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-762">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="d3c5e-763">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-763">Then click **OK**.</span></span> <span data-ttu-id="d3c5e-764">Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-764">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="d3c5e-765">![Vytvoření databáze](whats-new-in-aspnet-mvc-4/_static/image78.png "vytvoření řetězce databáze")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-765">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="d3c5e-766">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-766">*Creating the database*</span></span>
7. <span data-ttu-id="d3c5e-767">Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-767">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="d3c5e-768">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-768">Then click **Next**.</span></span>

    <span data-ttu-id="d3c5e-769">![Připojovací řetězec odkazující na SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "připojovací řetězec odkazující na SQL Database")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-769">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="d3c5e-770">*Připojovací řetězec odkazující na SQL Database*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-770">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="d3c5e-771">V **ve verzi Preview** klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-771">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="d3c5e-772">![Publikování webové aplikace](whats-new-in-aspnet-mvc-4/_static/image80.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-772">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="d3c5e-773">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-773">*Publishing the web application*</span></span>
9. <span data-ttu-id="d3c5e-774">Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.</span><span class="sxs-lookup"><span data-stu-id="d3c5e-774">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="d3c5e-775">![Publikování aplikace do Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "publikování aplikace do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d3c5e-775">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="d3c5e-776">*Aplikace publikovaná do Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="d3c5e-776">*Application published to Windows Azure*</span></span>
