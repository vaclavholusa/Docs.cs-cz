---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Vytvoření nového projektu ASP.NET MVC | Dokumentace Microsoftu
author: microsoft
description: Krok 1 ukazuje, jak změnit základní struktury aplikace NerdDinner na místě.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374273"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="a611d-103">Vytvoření nového projektu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a611d-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="a611d-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a611d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a611d-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="a611d-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a611d-106">Toto je krok 1 bezplatný [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="a611d-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="a611d-107">Krok 1 ukazuje, jak změnit základní struktury aplikace NerdDinner na místě.</span><span class="sxs-lookup"><span data-stu-id="a611d-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="a611d-108">Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="a611d-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="a611d-109">NerdDinner krok 1: Soubor -&gt;nový projekt</span><span class="sxs-lookup"><span data-stu-id="a611d-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="a611d-110">Naše aplikace NerdDinner jsme zobrazí za přibližně tak, že vyberete **souboru -&gt;nový projekt** položky nabídky v rámci sady Visual Studio 2008 nebo na bezplatné Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="a611d-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="a611d-111">Tím se otevře dialogové okno "Nový projekt".</span><span class="sxs-lookup"><span data-stu-id="a611d-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="a611d-112">K vytvoření nové aplikace ASP.NET MVC, budete výběru uzel "Web" na levé straně dialogového okna a pak vyberte šablonu projektu "Webová aplikace ASP.NET MVC" na pravé straně:</span><span class="sxs-lookup"><span data-stu-id="a611d-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="a611d-113">*Důležité: Ujistěte se, že jste stáhli a nainstalovali ASP.NET MVC – jinak se nezobrazí v dialogovém okně Nový projekt. Můžete použít verzí V2 [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) pokud ho ještě nemáte nainstalované (ASP.NET MVC je k dispozici v rámci "webové platformy –&gt;rámce a moduly runtime" části).*</span><span class="sxs-lookup"><span data-stu-id="a611d-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="a611d-114">Jsme budete pojmenujte nový projekt, který budeme vytvářet "NerdDinner" a potom klikněte na tlačítko "ok" k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a611d-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="a611d-115">Když jsme na tlačítko "ok" Visual Studio se zobrazí další dialog, který zobrazí výzvu, volitelně vytvořit projekt testování částí pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a611d-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="a611d-116">Tento projekt testu jednotky umožňuje vytvořit automatizované testy, které ověřují funkce a chování našich aplikace (něco si probereme jak úkolů později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="a611d-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="a611d-117">"Testovací rozhraní" rozevírací seznam v dialogovém okně výše se vyplní všechny dostupné technologie ASP.NET jednotka testu šablon projektu MVC na počítači nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="a611d-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="a611d-118">Verze si můžete stáhnout pro XUnit, NUnit a MBUnit.</span><span class="sxs-lookup"><span data-stu-id="a611d-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="a611d-119">Vestavěnou architekturou testování částí Visual Studio je také podporována.</span><span class="sxs-lookup"><span data-stu-id="a611d-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="a611d-120">*Poznámka: Rozhraní testování částí Visual Studio je k dispozici pouze s Visual Studio 2008 Professional a vyšší verze. Pokud používáte VS 2008 Standard Edition nebo Visual Web Developer 2008 Express je potřeba stáhnout a nainstalovat rozšíření NUnit, MBUnit nebo XUnit pro architekturu ASP.NET MVC v pořadí pro toto dialogové okno má být zobrazen. Dialogové okno nezobrazí, pokud nejsou k dispozici žádné testovací rozhraní nainstalované.*</span><span class="sxs-lookup"><span data-stu-id="a611d-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="a611d-121">Použijeme výchozí název "NerdDinner.Tests" pro testovací projekt, který jsme vytvořili a pomocí možnosti "Visual Studio testování částí" rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="a611d-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="a611d-122">Když kliknete na tlačítko "ok" Visual Studio vytvoří řešení pro nás s dva projekty v něm – jeden pro naši webovou aplikaci a jeden pro naše testy částí:</span><span class="sxs-lookup"><span data-stu-id="a611d-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="a611d-123">Zkoumání adresářovou strukturu NerdDinner</span><span class="sxs-lookup"><span data-stu-id="a611d-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="a611d-124">Při vytváření nové aplikace ASP.NET MVC pomocí sady Visual Studio automaticky přidá několik souborů a adresářů do projektu:</span><span class="sxs-lookup"><span data-stu-id="a611d-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="a611d-125">Projekty ASP.NET MVC ve výchozím nastavení mají šest adresářů nejvyšší úrovně:</span><span class="sxs-lookup"><span data-stu-id="a611d-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="a611d-126">**Adresář**</span><span class="sxs-lookup"><span data-stu-id="a611d-126">**Directory**</span></span> | <span data-ttu-id="a611d-127">**Účel**</span><span class="sxs-lookup"><span data-stu-id="a611d-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="a611d-128">**/ Řadiče**</span><span class="sxs-lookup"><span data-stu-id="a611d-128">**/Controllers**</span></span> | <span data-ttu-id="a611d-129">Místo, kam dáte třídách, které zpracovávají požadavky pro adresy URL</span><span class="sxs-lookup"><span data-stu-id="a611d-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="a611d-130">**/ Modelů**</span><span class="sxs-lookup"><span data-stu-id="a611d-130">**/Models**</span></span> | <span data-ttu-id="a611d-131">Místo, kam dáte tříd, které představují a manipulaci s daty</span><span class="sxs-lookup"><span data-stu-id="a611d-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="a611d-132">**/ Zobrazení**</span><span class="sxs-lookup"><span data-stu-id="a611d-132">**/Views**</span></span> | <span data-ttu-id="a611d-133">Místo, kam dáte soubory šablon uživatelského rozhraní, které jsou zodpovědné za vykreslování výstup</span><span class="sxs-lookup"><span data-stu-id="a611d-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="a611d-134">**/ Skripty**</span><span class="sxs-lookup"><span data-stu-id="a611d-134">**/Scripts**</span></span> | <span data-ttu-id="a611d-135">Místo, kam dáte soubory knihoven jazyka JavaScript a skripty (.js)</span><span class="sxs-lookup"><span data-stu-id="a611d-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="a611d-136">**/ Obsahu**</span><span class="sxs-lookup"><span data-stu-id="a611d-136">**/Content**</span></span> | <span data-ttu-id="a611d-137">Místo, kam dáte šablon stylů CSS a obrázkové soubory a jiný obsah než dynamické/JavaScript</span><span class="sxs-lookup"><span data-stu-id="a611d-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="a611d-138">**/ Aplikace\_dat**</span><span class="sxs-lookup"><span data-stu-id="a611d-138">**/App\_Data**</span></span> | <span data-ttu-id="a611d-139">Tam, kde se ukládají datové soubory byste měli pro čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="a611d-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="a611d-140">ASP.NET MVC nevyžaduje, aby tuto strukturu.</span><span class="sxs-lookup"><span data-stu-id="a611d-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="a611d-141">Ve skutečnosti vývojáře, kteří pracují na velkých aplikací se obvykle oddílu aplikace nahoru ve více projektech, aby lépe zvládnutelné (například: tříd datových modelů často přejít v projektu knihovny samostatné třídy z webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="a611d-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="a611d-142">Výchozí strukturu projektu, ale poskytuje dobré výchozí adresář konvenci, můžeme použít naše aplikace priority udržovat čisté.</span><span class="sxs-lookup"><span data-stu-id="a611d-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="a611d-143">Když jsme rozbalte adresář /Controllers jsme, že Visual Studio přidá dvě třídy kontroleru – HomeController a AccountController – ve výchozím nastavení se do projektu najdete:</span><span class="sxs-lookup"><span data-stu-id="a611d-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="a611d-144">Rozbalením adresáři /Views jsme jsme najdete tři podadresáře – / Home, / Account a /Shared –, jakož i několik šablon, které soubory v nich byly také přidali do projektu ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="a611d-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="a611d-145">Rozbalením/Content a adresáře/Scripts jsme najdete podporu Site.css soubor, který slouží k úpravě stylu HTML v lokalitě, jakož i knihoven jazyka JavaScript, které můžete povolit a jQuery v ASP.NET AJAX v rámci aplikace:</span><span class="sxs-lookup"><span data-stu-id="a611d-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="a611d-146">Když jsme rozbalte projekt NerdDinner.Tests jsme najdete dvě třídy, které obsahují testy jednotek pro naše třídy kontroleru:</span><span class="sxs-lookup"><span data-stu-id="a611d-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="a611d-147">Tyto výchozí soubory přidané pomocí sady Visual Studio nabízí základní strukturu pro fungující aplikaci – dokončeno s domovské stránky, stránky, stránky účtu přihlášení/odhlášení/registrace a stránku neošetřené chybě (všechny drátové nahoru a v provozu mimo pole).</span><span class="sxs-lookup"><span data-stu-id="a611d-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="a611d-148">Spuštění aplikace NerdDinner</span><span class="sxs-lookup"><span data-stu-id="a611d-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="a611d-149">Provozujeme projektu výběrem buď **ladění –&gt;spustit ladění** nebo **ladění –&gt;spustit bez ladění** položky nabídky:</span><span class="sxs-lookup"><span data-stu-id="a611d-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="a611d-150">Tím spustíte integrovaný ASP.NET Web-server, který je součástí sady Visual Studio a spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="a611d-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="a611d-151">Tady je domovská stránka pro náš nový projekt (adresa URL: "/") během spuštění:</span><span class="sxs-lookup"><span data-stu-id="a611d-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="a611d-152">Klikněte na kartu "O" zobrazí o stránce (adresa URL: "/ Home, nebo brzo"):</span><span class="sxs-lookup"><span data-stu-id="a611d-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="a611d-153">Kliknutím na odkaz "Přihlásit" v pravém horním trvá na přihlašovací stránku (adresa URL: "/ účet/přihlášení")</span><span class="sxs-lookup"><span data-stu-id="a611d-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="a611d-154">Pokud nemáme přihlašovací účet, můžeme kliknout na odkaz registrovat (adresa URL: "/ účtu/registrace") k jejímu vytvoření:</span><span class="sxs-lookup"><span data-stu-id="a611d-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="a611d-155">Kód pro implementaci nahoře na domovské stránce a odhlášení / register funkce přidané ve výchozím nastavení, když jsme vytvořili náš nový projekt.</span><span class="sxs-lookup"><span data-stu-id="a611d-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="a611d-156">Použijeme jej jako výchozí bod naši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a611d-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="a611d-157">Testování aplikace NerdDinner</span><span class="sxs-lookup"><span data-stu-id="a611d-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="a611d-158">Pokud se používá edice Professional nebo vyšší verzi sady Visual Studio 2008, můžeme použít integrované testování podpora integrované vývojové prostředí sady Visual Studio k testování projektu:</span><span class="sxs-lookup"><span data-stu-id="a611d-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="a611d-159">Výběrem jedné z výše uvedených možností otevřete podokno "Výsledků testů" integrovaného vývojového prostředí a poskytovat nám stav úspěšného/neúspěšného 27 jednotkovými testy součástí náš nový projekt, které zahrnují integrované funkce:</span><span class="sxs-lookup"><span data-stu-id="a611d-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="a611d-160">Později v tomto kurzu vytvoříme se jimi více o automatizované testování a přidejte další jednotkové testy, které se týkají funkčnost aplikace, které můžeme implementovat.</span><span class="sxs-lookup"><span data-stu-id="a611d-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="a611d-161">Dalším krokem</span><span class="sxs-lookup"><span data-stu-id="a611d-161">Next Step</span></span>

<span data-ttu-id="a611d-162">Teď máme strukturu základní aplikaci v místě.</span><span class="sxs-lookup"><span data-stu-id="a611d-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="a611d-163">Pojďme teď [vytvořit databázi pro ukládání dat našich aplikací](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="a611d-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a611d-164">[Předchozí](introducing-the-nerddinner-tutorial.md)
> [další](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="a611d-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
