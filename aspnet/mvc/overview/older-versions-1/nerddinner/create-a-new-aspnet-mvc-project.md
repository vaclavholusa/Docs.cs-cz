---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Vytvoření nového projektu ASP.NET MVC | Microsoft Docs
author: microsoft
description: Krok 1 ukazuje, jak zavést základní strukturu NerdDinner aplikace.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="01ac5-103">Vytvoření nového projektu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="01ac5-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="01ac5-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="01ac5-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="01ac5-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="01ac5-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="01ac5-106">Toto je krok 1 ze bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="01ac5-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="01ac5-107">Krok 1 ukazuje, jak zavést základní strukturu NerdDinner aplikace.</span><span class="sxs-lookup"><span data-stu-id="01ac5-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="01ac5-108">Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="01ac5-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="01ac5-109">NerdDinner krok 1: Soubor -&gt;nový projekt</span><span class="sxs-lookup"><span data-stu-id="01ac5-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="01ac5-110">Začneme budete naše aplikace NerdDinner výběrem **souboru -&gt;nový projekt** položky nabídky v sadě Visual Studio 2008 nebo volné Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="01ac5-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="01ac5-111">Tím se otevře dialogové okno "Nový projekt".</span><span class="sxs-lookup"><span data-stu-id="01ac5-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="01ac5-112">K vytvoření nové aplikace ASP.NET MVC, jsme budete vyberte uzel "Web" na levé straně dialogového okna a potom vyberte šablonu projektu "Webové aplikace ASP.NET MVC" na pravé straně:</span><span class="sxs-lookup"><span data-stu-id="01ac5-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="01ac5-113">*Důležité: Zajistěte, aby si stáhli a nainstalovali ASP.NET MVC – jinak se nezobrazí v dialogovém okně Nový projekt. Můžete použít V2 z [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) Pokud jste ho ještě nenainstalovali (ASP.NET MVC je k dispozici v rámci "webové platformy -&gt;rozhraní a moduly runtime" části).*</span><span class="sxs-lookup"><span data-stu-id="01ac5-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="01ac5-114">Jsme budete název nového projektu, které jsme se chystáte vytvořit "NerdDinner" a klikněte na tlačítko "ok" k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="01ac5-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="01ac5-115">Když jsme klikněte na tlačítko "ok" sady Visual Studio zobrazíte další dialogové okno, které nám pro volitelné vytvoření projektu testů jednotek pro nové aplikace vyzve.</span><span class="sxs-lookup"><span data-stu-id="01ac5-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="01ac5-116">Tato projektu testování částí umožňuje vytvořit automatizované testy, které ověřují funkce a chování aplikace (něco jsme zaměříme jak úkolů později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="01ac5-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="01ac5-117">"Test framework" rozevíracího seznamu v dialogovém okně výše bude zahrnovat všechny dostupné ASP.NET jednotky testovací šablon projektu MVC v počítači nainstalován.</span><span class="sxs-lookup"><span data-stu-id="01ac5-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="01ac5-118">Verze si můžete stáhnout pro NUnit, MBUnit a XUnit.</span><span class="sxs-lookup"><span data-stu-id="01ac5-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="01ac5-119">Vestavěnou architekturou testu jednotek sady Visual Studio je také podporována.</span><span class="sxs-lookup"><span data-stu-id="01ac5-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="01ac5-120">*Poznámka: Visual Studio Unit Test Framework je dostupná jenom s Visual Studio 2008 Professional a vyšších verzí. Pokud používáte VS 2008 Standard Edition nebo Visual Web Developer 2008 Express je nutné stáhnout a nainstalovat rozšíření NUnit, MBUnit nebo XUnit pro architekturu ASP.NET MVC v pořadí pro toto dialogové okno, která se má zobrazit. Dialogové okno se nezobrazí, pokud nejsou k dispozici žádné rozhraní test nainstalována.*</span><span class="sxs-lookup"><span data-stu-id="01ac5-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="01ac5-121">Jsme budete používat výchozí název "NerdDinner.Tests" k testovacímu projektu, který vytváříme a použijete možnost "Visual Studio jednotkové testování" framework.</span><span class="sxs-lookup"><span data-stu-id="01ac5-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="01ac5-122">Když kliknete na tlačítko "ok" sady Visual Studio vytvoří řešení pro nám s dva projekty v ní – jeden pro naše webové aplikace a jeden pro naše testy:</span><span class="sxs-lookup"><span data-stu-id="01ac5-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="01ac5-123">Zkoumání NerdDinner strukturu adresáře</span><span class="sxs-lookup"><span data-stu-id="01ac5-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="01ac5-124">Když vytvoříte novou aplikaci ASP.NET MVC pomocí sady Visual Studio, automaticky se přidá počet souborů a adresářů do projektu:</span><span class="sxs-lookup"><span data-stu-id="01ac5-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="01ac5-125">Projekty ASP.NET MVC ve výchozím nastavení mají šesti nejvyšší úrovně adresáře:</span><span class="sxs-lookup"><span data-stu-id="01ac5-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="01ac5-126">**Adresář**</span><span class="sxs-lookup"><span data-stu-id="01ac5-126">**Directory**</span></span> | <span data-ttu-id="01ac5-127">**Účel**</span><span class="sxs-lookup"><span data-stu-id="01ac5-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="01ac5-128">**/ Řadiče**</span><span class="sxs-lookup"><span data-stu-id="01ac5-128">**/Controllers**</span></span> | <span data-ttu-id="01ac5-129">Kam jste umístili řadiče třídy, které zpracovávají požadavky na adresu URL</span><span class="sxs-lookup"><span data-stu-id="01ac5-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="01ac5-130">**/ Modely**</span><span class="sxs-lookup"><span data-stu-id="01ac5-130">**/Models**</span></span> | <span data-ttu-id="01ac5-131">Kam jste umístili třídy, které představují a pracovat s daty</span><span class="sxs-lookup"><span data-stu-id="01ac5-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="01ac5-132">**Nebo zobrazení.**</span><span class="sxs-lookup"><span data-stu-id="01ac5-132">**/Views**</span></span> | <span data-ttu-id="01ac5-133">Kde umístit soubory šablon uživatelského rozhraní, které jsou zodpovědní za vykreslování výstup</span><span class="sxs-lookup"><span data-stu-id="01ac5-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="01ac5-134">**A skriptů**</span><span class="sxs-lookup"><span data-stu-id="01ac5-134">**/Scripts**</span></span> | <span data-ttu-id="01ac5-135">Kde umístit soubory knihovny JavaScript a skripty (.js)</span><span class="sxs-lookup"><span data-stu-id="01ac5-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="01ac5-136">**A obsah**</span><span class="sxs-lookup"><span data-stu-id="01ac5-136">**/Content**</span></span> | <span data-ttu-id="01ac5-137">Kam jste umístili šablon stylů CSS a soubory obrázků a další obsah bez dynamické/JavaScript</span><span class="sxs-lookup"><span data-stu-id="01ac5-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="01ac5-138">**/ Aplikace\_dat**</span><span class="sxs-lookup"><span data-stu-id="01ac5-138">**/App\_Data**</span></span> | <span data-ttu-id="01ac5-139">Tam, kde se ukládají datové soubory chcete pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="01ac5-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="01ac5-140">ASP.NET MVC nevyžaduje, aby tuto strukturu.</span><span class="sxs-lookup"><span data-stu-id="01ac5-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="01ac5-141">Ve skutečnosti vývojáře, kteří pracují na velké aplikace bude obvykle oddílu aplikace se ve více projektech, aby se daly více spravovat (například: třídy modelu dat se často přejděte v projektu knihovny tříd samostatné z webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="01ac5-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="01ac5-142">Výchozí projekt struktury však poskytuje dobrý výchozí adresář konvenci, jsme můžete zachovat čisté riziko z hlediska naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="01ac5-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="01ac5-143">Když jsme rozbalte adresář /Controllers jsme najdete, Visual Studio přidali dvě třídy controller – HomeController a AccountController – ve výchozím nastavení do projektu:</span><span class="sxs-lookup"><span data-stu-id="01ac5-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="01ac5-144">Když jsme rozšířit adresáři /Views, jsme najdete tři podadresářích – / Home, /Account a /Shared –, jakož i několik šablonu, kterou soubory v nich byly také k projektu nepřidají ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="01ac5-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="01ac5-145">Když jsme rozšířit/Content a/Scripts adresářů, jsme najdete podporu souboru Site.css, která je použita k formátování všechny HTML na webu a také můžete povolit prvku ASP.NET AJAX a knihovnu jQuery knihovny jazyka JavaScript v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="01ac5-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="01ac5-146">Když jsme rozbalte projekt NerdDinner.Tests jsme najdete dvou tříd, které obsahují testování částí pro naše řadiče třídy:</span><span class="sxs-lookup"><span data-stu-id="01ac5-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="01ac5-147">Tyto soubory výchozí přidat Visual Studio poskytnout nám základní strukturu pro funkční aplikaci - kompletní s domovskou stránku, o stránku, účet přihlášení/odhlášení nebo registrace stránky a neošetřené chybová stránka (všechny drátové zatížení a pracovní mimo pole).</span><span class="sxs-lookup"><span data-stu-id="01ac5-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="01ac5-148">Spuštění aplikace NerdDinner</span><span class="sxs-lookup"><span data-stu-id="01ac5-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="01ac5-149">Můžeme spustit projekt tak, že buď zvolíte **ladění -&gt;spustit ladění** nebo **ladění -&gt;spustit bez ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="01ac5-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="01ac5-150">Tím spustíte předdefinované ASP.NET webový server, který se dodává s Visual Studio a spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="01ac5-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="01ac5-151">Níže je domovská stránka pro naše nový projekt (adresa URL: "/") při spuštění:</span><span class="sxs-lookup"><span data-stu-id="01ac5-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="01ac5-152">Kliknutím na kartu "O" zobrazí o stránce (adresa URL: "/ domácí, / o"):</span><span class="sxs-lookup"><span data-stu-id="01ac5-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="01ac5-153">Kliknutím na odkaz "Přihlášení" v pravé horní trvá na přihlašovací stránku (adresa URL: "/ Account/přihlášení")</span><span class="sxs-lookup"><span data-stu-id="01ac5-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="01ac5-154">Pokud nemáme přihlašovací účet jsme klikněte na odkaz registrace (adresa URL: "/ Account/Register") Chcete-li vytvořit:</span><span class="sxs-lookup"><span data-stu-id="01ac5-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="01ac5-155">Kód k provedení výše Domů, o a odhlášení Register funkce byla přidána ve výchozím nastavení, když jsme vytvořili naše nový projekt.</span><span class="sxs-lookup"><span data-stu-id="01ac5-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="01ac5-156">Použijeme ji jako výchozí bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="01ac5-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="01ac5-157">Testování aplikace NerdDinner</span><span class="sxs-lookup"><span data-stu-id="01ac5-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="01ac5-158">Pokud se používá edice Professional nebo vyšší verze sady Visual Studio 2008, jsme můžete použít integrovanou jednotku testování podpora rozhraní IDE v sadě Visual Studio k testování projektu:</span><span class="sxs-lookup"><span data-stu-id="01ac5-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="01ac5-159">Vybrat jednu z výše uvedených možností otevřete podokno "Výsledky testů" v prostředí IDE a poskytnout nám průchodu nebo selhání stavu na testování částí 27 součástí naší nový projekt, které se týkají integrovanou funkci:</span><span class="sxs-lookup"><span data-stu-id="01ac5-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="01ac5-160">Později v tomto kurzu jsme budete komunikovat více informací o automatizované testování a přidávat další jednotky testy, které se týkají funkce aplikace, kterou jsme implementovat.</span><span class="sxs-lookup"><span data-stu-id="01ac5-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="01ac5-161">Další krok</span><span class="sxs-lookup"><span data-stu-id="01ac5-161">Next Step</span></span>

<span data-ttu-id="01ac5-162">Jste nyní My struktura základní aplikace na místě.</span><span class="sxs-lookup"><span data-stu-id="01ac5-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="01ac5-163">Nyní Pojďme [vytvořit databázi k ukládání dat naše aplikace](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="01ac5-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01ac5-164">[Předchozí](introducing-the-nerddinner-tutorial.md)
> [další](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="01ac5-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
