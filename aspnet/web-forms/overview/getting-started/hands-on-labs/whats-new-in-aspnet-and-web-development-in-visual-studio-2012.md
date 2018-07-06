---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Co je nového v ASP.NET a webového vývoje v sadě Visual Studio 2012 | Dokumentace Microsoftu
author: rick-anderson
description: Nová verze sady Visual Studio přináší celou řadu vylepšení, zaměřuje na vylepšení možnosti a výkon při práci s technologiemi Web...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 474df5c8e2cee820a3bdd80ba45e6504a025cdf1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823605"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="e408d-103">Co je nového v ASP.NET a webového vývoje v sadě Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e408d-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="e408d-104">podle [Campy Web týmu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e408d-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="e408d-105">Nová verze sady Visual Studio přináší celou řadu vylepšení, zaměřuje na vylepšení možnosti a výkon při práci s technologiemi Web.</span><span class="sxs-lookup"><span data-stu-id="e408d-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="e408d-106">Visual Studio editory pro šablony stylů CSS, JavaScript a HTML byl zcela přepracované zahrnout mnoho nejvíce vyžádání kód podpory, jako je například technologie IntelliSense a Automatické odsazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="e408d-107">Z hlediska výkonu sdružování a minifikace jsou teď integrované jako dobu načítání integrované funkce, které chcete jednoduše redukovat stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="e408d-108">Visual Studio umožňuje pracovat s nejnovějšími technologiemi Web.</span><span class="sxs-lookup"><span data-stu-id="e408d-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="e408d-109">Abyste měli jistotu, že váš web fungovat bez ohledu na platformu klienta a využití nové funkce a prvky HTML5 můžete fragmenty CSS3 prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="e408d-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="e408d-110">Psaní a profilování kódu JavaScript by mělo být jednodušší s touto verzí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e408d-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="e408d-111">Seznamy IntelliSense, integrované funkce XML dokumentace a navigace jsou teď k dispozici pro kód jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="e408d-112">Teď máte katalogu JavaScript na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="e408d-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="e408d-113">Kromě toho můžete zkontrolovat dodržování předpisů ECMAScript5 pomocí skriptů a zjistit chyby syntaxe v rané fázi.</span><span class="sxs-lookup"><span data-stu-id="e408d-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="e408d-114">Poslední, ale ne alespoň tato verze sady Visual Studio implementuje integrované sdružování a minifikace.</span><span class="sxs-lookup"><span data-stu-id="e408d-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="e408d-115">Soubory skriptů a stylů budou balené a komprimované tak, aby lokality provádí rychleji.</span><span class="sxs-lookup"><span data-stu-id="e408d-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="e408d-116">Tato laboratoř vás provede vylepšení a nových funkcí popsaných dříve použitím menší změny na ukázkovou webovou aplikaci ve zdrojové složce k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e408d-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="e408d-117">Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e408d-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e408d-118">Cíle</span><span class="sxs-lookup"><span data-stu-id="e408d-118">Objectives</span></span>

<span data-ttu-id="e408d-119">V tomto praktická cvičení se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="e408d-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="e408d-120">Používat nové funkce a vylepšení v editoru stylů CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="e408d-121">Používat nové funkce a vylepšení v editoru HTML</span><span class="sxs-lookup"><span data-stu-id="e408d-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="e408d-122">Používat nové funkce a vylepšení v editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e408d-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="e408d-123">Konfigurace a používání sdružování a minifikace</span><span class="sxs-lookup"><span data-stu-id="e408d-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e408d-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e408d-124">Prerequisites</span></span>

- <span data-ttu-id="e408d-125">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).</span><span class="sxs-lookup"><span data-stu-id="e408d-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="e408d-126">[Prostředí Windows PowerShell](https://support.microsoft.com/kb/968930/) (pro skripty instalace – na Windows 8 a Windows Server 2008 R2 nainstalovaná)</span><span class="sxs-lookup"><span data-stu-id="e408d-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="e408d-127">[Aplikace Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - nebo prohlížeč kompatibilní HTML5</span><span class="sxs-lookup"><span data-stu-id="e408d-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e408d-128">Cvičení</span><span class="sxs-lookup"><span data-stu-id="e408d-128">Exercises</span></span>

<span data-ttu-id="e408d-129">Tato praktická cvičení zahrnuje následující praktická cvičení:</span><span class="sxs-lookup"><span data-stu-id="e408d-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e408d-130">Cvičení 1: Co je nového v editoru stylů CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="e408d-131">Cvičení 2: Co je nového v editoru HTML</span><span class="sxs-lookup"><span data-stu-id="e408d-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="e408d-132">Cvičení 3: Co je nového v editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e408d-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="e408d-133">Cvičení 4: Sdružování a Minifikace</span><span class="sxs-lookup"><span data-stu-id="e408d-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="e408d-134">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="e408d-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="e408d-135">Cvičení 1: Co je nového v editoru stylů CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="e408d-136">Weboví vývojáři měli seznámit s mnoha problémy související s úpravy šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e408d-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="e408d-137">Jedním z největších problémů stylu CSS se kompatibility prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="e408d-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="e408d-138">Často dochází, že po aplikování stylů na váš web, si všimnete, že vypadá jinak Pokud otevřete v jiném prohlížeči nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="e408d-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="e408d-139">Proto může trávit mnoho času na řešení těchto problémů visual si uvědomit, že pokud provedete nakonec pracovat v jeden prohlížeč, to je v fungovat, ostatní.</span><span class="sxs-lookup"><span data-stu-id="e408d-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="e408d-140">Visual Studio nyní zahrnuje funkce, které pomáhá vývojářům přístup, práce a efektivní uspořádání šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e408d-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="e408d-141">Během tohoto cvičení bude vyhovovat nové funkce pro organizaci efektivní a edition, stejně jako CSS3 fragmenty kódu pro kompatibilitu prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="e408d-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="e408d-142">Úloha 1 – nové funkce editoru</span><span class="sxs-lookup"><span data-stu-id="e408d-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="e408d-143">V této úloze bude zjišťovat nové funkce editoru šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e408d-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="e408d-144">Tento nový editor vám pomůže zvýšit produktivitu s využitím novou inteligentní odsazení, komentáře vylepšení kódu a vylepšené technologie IntelliSense seznam.</span><span class="sxs-lookup"><span data-stu-id="e408d-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="e408d-145">Spustit **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e408d-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e408d-146">V Průzkumníku řešení otevřete **Site.css** soubor umístěný **styly** složky.</span><span class="sxs-lookup"><span data-stu-id="e408d-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="e408d-147">Ujistěte se, že **textový Editor** nástroje jsou viditelné na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="e408d-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="e408d-148">Chcete-li to mohli udělat, vyberte **zobrazení** | **panely nástrojů** možnost nabídky a zkontrolujte **textový Editor** možnosti.</span><span class="sxs-lookup"><span data-stu-id="e408d-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="e408d-149">Uvidíte, že od této nové verzi **komentář** tlačítko (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) a **zrušit komentář** tlačítko (![zrušte komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)), také dostupných pro editor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e408d-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="e408d-150">![Povolení nástroje šablon stylů CSS a Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "povolení editoru a nástroje pro šablony stylů CSS")</span><span class="sxs-lookup"><span data-stu-id="e408d-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="e408d-151">*Povolení nástroje šablon stylů CSS a Editor*</span><span class="sxs-lookup"><span data-stu-id="e408d-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="e408d-152">Posunout kód a vybrat všechny definice třídy šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e408d-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="e408d-153">Klikněte na tlačítko **komentář** (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) tlačítko vyjádřit vybrané řádky.</span><span class="sxs-lookup"><span data-stu-id="e408d-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="e408d-154">Klikněte **zrušit komentář** (![Odkomentujte tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) tlačítko pro vrácení změn.</span><span class="sxs-lookup"><span data-stu-id="e408d-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="e408d-155">Klikněte na tlačítko **sbalit** (![sbalit](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) a **Rozbalit** (![rozbalte](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) tlačítka umístěny na levý okraj textu.</span><span class="sxs-lookup"><span data-stu-id="e408d-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="e408d-156">Všimněte si, že teď můžete skrýt styly, které nepoužíváte čisticího modulu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="e408d-157">![Sbalení tříd CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "tříd CSS sbalení")</span><span class="sxs-lookup"><span data-stu-id="e408d-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="e408d-158">*Sbalení třídy šablony stylů CSS*</span><span class="sxs-lookup"><span data-stu-id="e408d-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="e408d-159">Ujistěte se, že je povolena funkce Inteligentní odsazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="e408d-160">Vyberte **nástroje** | **možnosti** nabídky a pak vyberte **textový Editor** | **šablon stylů CSS**  |  **Formátování** stránky v levém podokně na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="e408d-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="e408d-161">Zkontrolujte, **hierarchické odsazení** možnost.</span><span class="sxs-lookup"><span data-stu-id="e408d-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="e408d-162">![Povolení hierarchické odsazení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "povolení hierarchické odsazení")</span><span class="sxs-lookup"><span data-stu-id="e408d-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="e408d-163">*Povolení hierarchické odsazení*</span><span class="sxs-lookup"><span data-stu-id="e408d-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="e408d-164">Vyhledejte definici hlavní třídy (.main) a doplňovací style pro prvky elementů div.</span><span class="sxs-lookup"><span data-stu-id="e408d-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="e408d-165">Můžete si všimnout, že kód zarovná automaticky, kterým pomůže uživatelům najít nadřazené třídy na první pohled.</span><span class="sxs-lookup"><span data-stu-id="e408d-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="e408d-166">CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="e408d-167">![Hierarchické zarovnání v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchické zarovnání v jazyce CSS")</span><span class="sxs-lookup"><span data-stu-id="e408d-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="e408d-168">*Hierarchické zarovnání v jazyce CSS*</span><span class="sxs-lookup"><span data-stu-id="e408d-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="e408d-169">Uvnitř **.main div** třídy, vyhledejte kurzor na konci **ohraničení: 0px;** a stiskněte klávesu **Enter** zobrazíte seznam technologie IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e408d-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="e408d-170">Začněte psát **horní** a Všimněte si, jak v seznamu vyfiltrují během psaní.</span><span class="sxs-lookup"><span data-stu-id="e408d-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="e408d-171">V seznamu budou uvedeny prvky, které obsahují **horní** v jakékoli části slova (v předchozích verzích sady Visual Studio, v seznamu vyfiltrují podle položky, které *začít* s označením).</span><span class="sxs-lookup"><span data-stu-id="e408d-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="e408d-172">![Vylepšení technologie IntelliSense v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "vylepšení technologie IntelliSense v jazyce CSS")</span><span class="sxs-lookup"><span data-stu-id="e408d-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="e408d-173">*Vylepšení technologie IntelliSense v jazyce CSS*</span><span class="sxs-lookup"><span data-stu-id="e408d-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="e408d-174">Úloha 2 – Výběr barvy</span><span class="sxs-lookup"><span data-stu-id="e408d-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="e408d-175">V této úloze bude zjišťovat, že nový výběr barvy šablon stylů CSS integrována s IntelliSense ve Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e408d-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="e408d-176">V **Site.css,** vyhledejte definici třídy záhlaví (.header) a umístěte kurzor vedle **barvu pozadí** atribut mezi &quot;:&quot; a &quot; # &quot; znaků na daném řádku kódu **.**</span><span class="sxs-lookup"><span data-stu-id="e408d-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="e408d-177">![Vyhledání kurzor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "umístění kurzoru")</span><span class="sxs-lookup"><span data-stu-id="e408d-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="e408d-178">*Umístění kurzoru*</span><span class="sxs-lookup"><span data-stu-id="e408d-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="e408d-179">Odstranit **dvojtečka** (:) a zapište ho znovu zobrazíte volby barev.</span><span class="sxs-lookup"><span data-stu-id="e408d-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="e408d-180">Všimněte si, že první barvy, které se nejčastěji se vyskytujících barvy vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="e408d-181">Pokud kliknete na bílou barvu, jeho kód HTML (#fff) nahradí aktuální kód barvy v šabloně stylů.</span><span class="sxs-lookup"><span data-stu-id="e408d-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="e408d-182">![Výběr barvy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "výběr barvy")</span><span class="sxs-lookup"><span data-stu-id="e408d-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="e408d-183">*Výběr barvy*</span><span class="sxs-lookup"><span data-stu-id="e408d-183">*Color picker*</span></span>
3. <span data-ttu-id="e408d-184">Stisknutím klávesy **Rozbalit** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) tlačítko pro výběr barvy a zobrazte barvy, přesuňte kurzor přechodu na jinou barvu.</span><span class="sxs-lookup"><span data-stu-id="e408d-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="e408d-185">Potom klikněte na tlačítko **kapátko** tlačítko a vyberte libovolnou barvu z obrazovky.</span><span class="sxs-lookup"><span data-stu-id="e408d-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="e408d-186">Všimněte si, že hodnota barvy pozadí se dynamicky mění při přesunutí kurzoru.</span><span class="sxs-lookup"><span data-stu-id="e408d-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="e408d-187">![Přechod pro výběr barev](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "přechod pro výběr barev")</span><span class="sxs-lookup"><span data-stu-id="e408d-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="e408d-188">*Přechod pro výběr barev*</span><span class="sxs-lookup"><span data-stu-id="e408d-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="e408d-189">V **krytí** posuvník, přesunout do středu panelu ke snížení krytí modulu pro výběr.</span><span class="sxs-lookup"><span data-stu-id="e408d-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="e408d-190">Všimněte si, že hodnota barvy pozadí nyní změní měřítko na RGBA.</span><span class="sxs-lookup"><span data-stu-id="e408d-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="e408d-191">![Výběr barvy krytí](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "výběr barvy krytí")</span><span class="sxs-lookup"><span data-stu-id="e408d-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="e408d-192">*Výběr barvy krytí*</span><span class="sxs-lookup"><span data-stu-id="e408d-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-193">Definice barvy RGBA (červená, zelená, modrá, alfa) ve specifikaci CSS3 umožňuje definovat hodnotu neprůhlednosti barvy pro jednu položku.</span><span class="sxs-lookup"><span data-stu-id="e408d-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="e408d-194">Na rozdíl od **krytí -** podobně jako atribut CSS **-** RGBA barvy budou také kompatibilní s nejnovější prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e408d-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="e408d-195">Úloha 3 – fragmenty kódu kompatibilní šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="e408d-196">V této úloze se dozvíte, jak používat napříč prohlížeči kompatibilní CSS3 fragmenty kvůli implementaci některé funkce na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="e408d-197">V **Site.css** souboru, vyhledejte **záhlaví** šablon stylů CSS třídy definice (.header) a umístěte kurzor na slovo níže **/ \*poloměr ohraničení\* /** zástupný symbol pro přidání nové fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="e408d-198">Stisknutím klávesy **Enter** zobrazíte seznam technologie IntelliSense a typ **radius** pro filtrování seznamu.</span><span class="sxs-lookup"><span data-stu-id="e408d-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="e408d-199">Vyberte **border-radius** možnost ze seznamu poklikejte na a pak stiskněte klávesu **kartu** klíče pro vložení fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="e408d-200">Zadejte velikost protokolu radius v pixelech a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e408d-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="e408d-201">Například zadejte **15px**.</span><span class="sxs-lookup"><span data-stu-id="e408d-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="e408d-202">Vykreslí zakulacený ohraničení u většiny prohlížečů HTML5 dodržování předpisů, včetně, Mozilla a na základě WebKit prohlížeče se CSS3 atributy přidané aplikací fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="e408d-203">![Pomocí fragmentu kódu border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "pomocí fragmentu kódu border-radius")</span><span class="sxs-lookup"><span data-stu-id="e408d-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="e408d-204">*Pomocí fragmentu kódu border-radius*</span><span class="sxs-lookup"><span data-stu-id="e408d-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="e408d-205">Použít stejné **ohraničení** fragmenty kódu ve stylu stránky (.page).</span><span class="sxs-lookup"><span data-stu-id="e408d-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="e408d-206">CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="e408d-207">Stisknutím klávesy **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="e408d-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="e408d-208">Všimněte si, že každá stránka nyní zaoblené ohraničení.</span><span class="sxs-lookup"><span data-stu-id="e408d-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="e408d-209">![Zaoblené rohy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaoblené rohy")</span><span class="sxs-lookup"><span data-stu-id="e408d-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="e408d-210">*Zaoblení rohů*</span><span class="sxs-lookup"><span data-stu-id="e408d-210">*Rounded corners*</span></span>
4. <span data-ttu-id="e408d-211">Zavřete prohlížeč a vraťte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e408d-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="e408d-212">Otevřít **Custom.css** soubor umístěný **styly** složky a umístěte kurzor mezi **div.images ul li img** definici třídy.</span><span class="sxs-lookup"><span data-stu-id="e408d-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="e408d-213">Stisknutím klávesy enter zobrazte seznam technologie IntelliSense, typ **box-shadow** a stiskněte klávesu **kartu** klíč dvakrát pro vložení fragmentu kódu stínové výchozí uvnitř definice třídy.</span><span class="sxs-lookup"><span data-stu-id="e408d-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="e408d-214">Nastavte hodnoty stínové na **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="e408d-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="e408d-215">Potom zadejte **border-radius** a Vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="e408d-216">Typ **15px** nastavit velikost protokolu radius a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="e408d-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="e408d-217">![Zaoblené rohy se stínovou](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaoblené rohy se stínovou")</span><span class="sxs-lookup"><span data-stu-id="e408d-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="e408d-218">*Zaoblené rohy se stínovou*</span><span class="sxs-lookup"><span data-stu-id="e408d-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-219">V tomto okamžiku je vložen atribut stínové s odpovídající předpony (moz komponenty webkit, o) pro podporu Mozilla a prohlížeče komponenty Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="e408d-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="e408d-220">Vytvořte novou třídu **div.images ul li img:hover** níže **div.images ul li img** definici třídy a umístěte kurzor dovnitř závorek **.**</span><span class="sxs-lookup"><span data-stu-id="e408d-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="e408d-221">CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="e408d-222">Typ **transformace** a stiskněte klávesu **kartu** klíč dvakrát, aby bylo možné vložit fragment kódu transformace.</span><span class="sxs-lookup"><span data-stu-id="e408d-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="e408d-223">Potom zadejte **rotate(-15deg)** změnit hodnota úhlu otočení při bitové kopie jsou při přechodu myší.</span><span class="sxs-lookup"><span data-stu-id="e408d-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="e408d-224">CSS</span><span class="sxs-lookup"><span data-stu-id="e408d-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="e408d-225">Stisknutím klávesy **F5** ke spuštění řešení a přejděte na stránku CSS3.</span><span class="sxs-lookup"><span data-stu-id="e408d-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="e408d-226">Všimněte si, že mají zaoblené rohy a pole stíny imagí.</span><span class="sxs-lookup"><span data-stu-id="e408d-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="e408d-227">Najeďte myší obrázky a podívejte se na nich otočit.</span><span class="sxs-lookup"><span data-stu-id="e408d-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="e408d-228">![Transformace fragmentu otočení obrázku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragment kódu transformace otočení obrázku")</span><span class="sxs-lookup"><span data-stu-id="e408d-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="e408d-229">*Transformace fragmentu otočení obrázku*</span><span class="sxs-lookup"><span data-stu-id="e408d-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-230">Pokud používáte Internet Explorer 10 a nevidí stínů, ujistěte se, že je nastaven režimu dokumentů standardy aplikace Internet Explorer 10.</span><span class="sxs-lookup"><span data-stu-id="e408d-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="e408d-231">Stisknutím klávesy **F12** otevřete vývojářské nástroje aplikace Internet Explorer a klikněte na tlačítko **režim dokumentu** změnit standardy aplikace Internet Explorer 10.</span><span class="sxs-lookup"><span data-stu-id="e408d-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![o-USA](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="e408d-233">Cvičení 2: Co je nového v editoru HTML</span><span class="sxs-lookup"><span data-stu-id="e408d-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="e408d-234">Visual Studio poskytuje Vylepšený editor HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="e408d-235">Některá vylepšení zahrnutá v této verzi jsou inteligentní odsazení dokumentů HTML, fragmenty kódu HTML5, HTML start a odpovídající koncové značky a ověřováním HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="e408d-236">Během tohoto cvičení uvidíte, jak tyto změny vylepšit vaše fluency při práci v kódu na webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="e408d-237">Také jsme vylepšili editoru HTML, jako editor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="e408d-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="e408d-238">Většina tato vylepšení jsou malé ty, které usnadňují života Web developer.</span><span class="sxs-lookup"><span data-stu-id="e408d-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="e408d-239">Věci jako další fragmenty kódu pro HTML5, inteligentní odsazení při úpravách a ověření cílení na dokument DOCTYPE v HTML jsou některá z těchto vylepšení odpovídající počáteční a koncovou značku.</span><span class="sxs-lookup"><span data-stu-id="e408d-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="e408d-240">Úloha 1 – vylepšené DOCTYPE ověření</span><span class="sxs-lookup"><span data-stu-id="e408d-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="e408d-241">HTML editor má teď možnost zkontrolovat deklarace DOCTYPE stránky, i když definice může být na stránce předlohy.</span><span class="sxs-lookup"><span data-stu-id="e408d-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="e408d-242">HTML editor v závislosti na deklarace DOCTYPE stránky, ověří se správnou sadou pravidel a bude filtrovat seznam technologie IntelliSense, vzhledem k tomu prvky DOCTYPE.</span><span class="sxs-lookup"><span data-stu-id="e408d-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="e408d-243">V této úloze se změní deklarace DOCTYPE stránky, pokud chcete zobrazit, jak se příslušným způsobem mění chování editoru HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="e408d-244">Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e408d-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e408d-245">Otevřít **Site.Master** stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="e408d-246">Všimněte si, že na cílové schéma pro ověření nástrojů.</span><span class="sxs-lookup"><span data-stu-id="e408d-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="e408d-247">Deklarace Doctype vybrané se k správně změní způsob, jakým se chová editoru HTML (ověření, technologie IntelliSense atd.).</span><span class="sxs-lookup"><span data-stu-id="e408d-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="e408d-248">![Používat typ Doctype v úpravy zdrojového kódu HTML nástrojů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")</span><span class="sxs-lookup"><span data-stu-id="e408d-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="e408d-249">*Používat typ Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*</span><span class="sxs-lookup"><span data-stu-id="e408d-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="e408d-250">Změna cílové schéma do formátu HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="e408d-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="e408d-251">![Změna Doctype v úpravy zdrojového kódu HTML nástrojů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")</span><span class="sxs-lookup"><span data-stu-id="e408d-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="e408d-252">*Změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*</span><span class="sxs-lookup"><span data-stu-id="e408d-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="e408d-253">Umístěte kurzor na slovo v rámci **tělo** prvek a začněte psát název HTML5 elementu (například **videa**).</span><span class="sxs-lookup"><span data-stu-id="e408d-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="e408d-254">Všimněte si, že prvek není k dispozici v seznamu technologie IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e408d-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="e408d-255">![Nejsou uvedeny prvky HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "nejsou uvedeny prvky HTML5")</span><span class="sxs-lookup"><span data-stu-id="e408d-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="e408d-256">*Nejsou uvedeny prvky HTML5*</span><span class="sxs-lookup"><span data-stu-id="e408d-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="e408d-257">Vrátit zpět změny na cílové schéma pro ověření nástrojů vybrat typ dokumentu: XHTML5 z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="e408d-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="e408d-258">![Používat typ Doctype v úpravy zdrojového kódu HTML nástrojů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")</span><span class="sxs-lookup"><span data-stu-id="e408d-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="e408d-259">*Resetovat Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*</span><span class="sxs-lookup"><span data-stu-id="e408d-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="e408d-260">Umístěte kurzor na slovo v rámci **tělo** prvek a začněte psát HTML5 elementu znovu (třeba jako **videa**).</span><span class="sxs-lookup"><span data-stu-id="e408d-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="e408d-261">Všimněte si, že prvky HTML5, jsou teď dostupné v seznamu technologie IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e408d-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="e408d-262">![Prvky HTML5 zobrazovaly](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "zobrazovaly prvky HTML5")</span><span class="sxs-lookup"><span data-stu-id="e408d-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="e408d-263">*Prvky HTML5 zobrazovaly*</span><span class="sxs-lookup"><span data-stu-id="e408d-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="e408d-264">Úloha 2 – začátek/konec značky automatických aktualizací</span><span class="sxs-lookup"><span data-stu-id="e408d-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="e408d-265">Visual Studio nyní aktualizuje HTML otevření nebo zavření značky elementu, který upravujete vzájemně odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="e408d-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="e408d-266">Tato nová funkce budou zvyšovat produktivitu při úpravách značky HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="e408d-267">Na **Default.aspx** stránce, přidejte **H3** element s názvem (například Visual Studio 2012 Rocks!).</span><span class="sxs-lookup"><span data-stu-id="e408d-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="e408d-268">Změnit **H3** značku a typ **H2** nebo **H1.**</span><span class="sxs-lookup"><span data-stu-id="e408d-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="e408d-269">Všimněte si, že automaticky aktualizuje koncová značka.</span><span class="sxs-lookup"><span data-stu-id="e408d-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="e408d-270">Můžete také upravit koncovou značku, pokud chcete zobrazit, že počáteční značce odpovídajícím způsobem aktualizuje příliš.</span><span class="sxs-lookup"><span data-stu-id="e408d-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="e408d-271">![Automatická aktualizace koncovou značku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatických aktualizací koncová značka")</span><span class="sxs-lookup"><span data-stu-id="e408d-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="e408d-272">*Automatická aktualizace koncová značka*</span><span class="sxs-lookup"><span data-stu-id="e408d-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="e408d-273">Úloha 3 – nové fragmenty kódu HTML5</span><span class="sxs-lookup"><span data-stu-id="e408d-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="e408d-274">Visual Studio teď obsahuje několik fragmentů kódu HTML5.</span><span class="sxs-lookup"><span data-stu-id="e408d-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="e408d-275">V této úloze budete používat některé fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="e408d-276">Přidat novou složku s názvem **zvuku** do kořenové složky webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="e408d-277">Otevřete Průzkumníka Windows a zkopírujte do jakékoli zvukový soubor **zvuku** složky **WhatsNewASPNET.sln** řešení.</span><span class="sxs-lookup"><span data-stu-id="e408d-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="e408d-278">V **Default.aspx** stránky, vyhledejte kurzor v rámci webu 11 Rocks!</span><span class="sxs-lookup"><span data-stu-id="e408d-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="e408d-279">Záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e408d-279">Header.</span></span> <span data-ttu-id="e408d-280">Typ **zvuku** a stiskněte klávesu Tabulátor.</span><span class="sxs-lookup"><span data-stu-id="e408d-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="e408d-281">Nový editor HTML obsahuje fragmenty kódu pro HTML5 obsah.</span><span class="sxs-lookup"><span data-stu-id="e408d-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="e408d-282">Nezapomeňte použít správné definice DOCTYPE umožňující fragmenty kódu HTML5.</span><span class="sxs-lookup"><span data-stu-id="e408d-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="e408d-283">![Vkládají se fragmenty kódu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "vkládají se fragmenty kódu HTML5")</span><span class="sxs-lookup"><span data-stu-id="e408d-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="e408d-284">*Vkládají se fragmenty kódu HTML5*</span><span class="sxs-lookup"><span data-stu-id="e408d-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="e408d-285">Aktualizace zdroje zvuku tak, aby odkazovala na existující zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="e408d-285">Update the audio source to point to an existing audio file.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="e408d-286">Je potřeba přidat zvukový soubor k řešení.</span><span class="sxs-lookup"><span data-stu-id="e408d-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="e408d-287">Stisknutím klávesy **F5** spuštění tohoto webu a přehrávání zvuku.</span><span class="sxs-lookup"><span data-stu-id="e408d-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="e408d-288">![Spuštěním ovládacího prvku zvuk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "spuštěním ovládacího prvku zvuku")</span><span class="sxs-lookup"><span data-stu-id="e408d-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="e408d-289">*Spuštěním ovládacího prvku zvuku*</span><span class="sxs-lookup"><span data-stu-id="e408d-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-290">Můžete také zkusit víc fragmentů kódu, které jsou zahrnuty v sadě Visual Studio, jako jsou videa, obrázek, atd.</span><span class="sxs-lookup"><span data-stu-id="e408d-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="e408d-291">Teď si vyzkoušejte vložíte ovládací prvek v některé části stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="e408d-292">Například, pokuste se vložit **GridView** ovládacího prvku, ale místo zadání  **&lt;mřížky,** začněte psát  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="e408d-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="e408d-293">Všimněte si, že v seznamu technologie IntelliSense se zobrazí **asp: GridView** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e408d-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="e408d-294">Technologie IntelliSense v editoru HTML nyní poskytuje vyhledávání – název malých a velkých písmen, stejně jako částečné odpovídající (načítání všechny elementy, které obsahuje výraz).</span><span class="sxs-lookup"><span data-stu-id="e408d-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="e408d-295">![Vložení prvku GridView se seznamy IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "vkládání prvek GridView s seznamy IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="e408d-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="e408d-296">*Vložení prvku GridView se seznamy IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="e408d-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="e408d-297">Pokud zadáte  **&lt;mřížky** se zobrazí všechny položky, které odpovídají termín, ale Visual Studio navrhne **gridview** ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="e408d-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="e408d-298">![Vkládání prvek GridView s částečnou shodu a seznamy IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "vkládání prvek GridView s částečnou shodu a seznamy IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="e408d-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="e408d-299">*Vkládání prvek GridView s částečnou shodu a seznamy IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="e408d-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="e408d-300">Úloha 4 – HTML Editor inteligentní značky</span><span class="sxs-lookup"><span data-stu-id="e408d-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="e408d-301">Další vylepšení v editoru HTML je funkce inteligentních značek.</span><span class="sxs-lookup"><span data-stu-id="e408d-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="e408d-302">Inteligentní značky umožňují snadno provádět úlohy běžné nebo opakované vývoje na základě-control.</span><span class="sxs-lookup"><span data-stu-id="e408d-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="e408d-303">Tato funkce už je k dispozici v Návrháři HTML, ale ne v editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="e408d-304">Otevřít **Site.Master** a vyhledejte **nabídku asp:** elementu.</span><span class="sxs-lookup"><span data-stu-id="e408d-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="e408d-305">Umístěte kurzor na počáteční značce a Všimněte si, že malé piktogram zobrazí v dolní části elementu - kliknutím ho otevřete v nabídce inteligentních úlohy.</span><span class="sxs-lookup"><span data-stu-id="e408d-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="e408d-306">Všimněte si, že máte rychlý přístup k některé úkoly související s ovládací prvek nabídky.</span><span class="sxs-lookup"><span data-stu-id="e408d-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="e408d-307">![Inteligentní úlohy pro ovládací prvek nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentní úlohy pro ovládací prvek nabídky")</span><span class="sxs-lookup"><span data-stu-id="e408d-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="e408d-308">*Chytré úkoly pro ovládací prvek nabídky*</span><span class="sxs-lookup"><span data-stu-id="e408d-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="e408d-309">Úloha 5: inteligentní odsazení</span><span class="sxs-lookup"><span data-stu-id="e408d-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="e408d-310">Jeden z osvědčených postupů ve formátu HTML je odsazení vnořené prvky, které mají být kód čitelné.</span><span class="sxs-lookup"><span data-stu-id="e408d-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="e408d-311">V sadě Visual Studio 2012 můžete si všimnout, že v editoru automaticky odsadí prvky při psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="e408d-312">V předchozí verzi sady Visual Studio, inteligentní odsazení byla k dispozici v editoru XML, ale ne v editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="e408d-313">Ujistěte se, že konfigurace Indenting v editoru HTML je nastavena na inteligentní odsazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="e408d-314">Chcete-li to mohli udělat, vyberte **nástroje | Možnosti** nabídky a pak vyberte **textový Editor | HTML | Karty** stránky v levém podokně na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="e408d-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="e408d-315">Vyberte možnost Inteligentní odsazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="e408d-316">![Nastavení editoru HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "nastavení editoru HTML")</span><span class="sxs-lookup"><span data-stu-id="e408d-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="e408d-317">*Nastavení editoru HTML*</span><span class="sxs-lookup"><span data-stu-id="e408d-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="e408d-318">Na **Default.aspx** stránce, odstranit veškerý obsah v rámci elementu zvuku.</span><span class="sxs-lookup"><span data-stu-id="e408d-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="e408d-319">Umístěte kurzor na konec otevírání **zvuku** elementu a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="e408d-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="e408d-320">Všimněte si, že nová pozice kurzoru má úroveň další odsazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="e408d-321">![Inteligentní odsazení v editoru HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentní odsazení v editoru HTML")</span><span class="sxs-lookup"><span data-stu-id="e408d-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="e408d-322">*Inteligentní odsazení v editoru HTML*</span><span class="sxs-lookup"><span data-stu-id="e408d-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="e408d-323">Obnovit značku pro zvuk s obsahem jste odstranili, nebo zavřete **Default.aspx** bez uložení změn.</span><span class="sxs-lookup"><span data-stu-id="e408d-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="e408d-324">Krok 6 – extrahovat do uživatelského ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="e408d-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="e408d-325">Refactoring nástroje zahrnuté v sadě Visual Studio, jako je například extrahování část kódu na funkci, jsou skvělé funkce, které usnadňují zlepšení závorek a refaktoring stávajícího kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="e408d-326">Protějšek pro stránky ASP.NET by extrakce kódu HTML do uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e408d-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="e408d-327">Teď už ručně by vyžadovalo několik kroků, jako vytvoření nového uživatelského ovládacího prvku, přesunutí části kódu do uživatelského ovládacího prvku, registrace předponu značky uživatelského ovládacího prvku a, nakonec vytvoření instance uživatelského ovládacího prvku na stránkách.</span><span class="sxs-lookup"><span data-stu-id="e408d-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="e408d-328">Nyní, nové *extrahovat do uživatelského ovládacího prvku* nástroj všechny tyto kroky automaticky provede za vás.</span><span class="sxs-lookup"><span data-stu-id="e408d-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="e408d-329">Nové extrahovat do uživatelského ovládacího prvku kontextové operace v této úloze, použije k vygenerování nového uživatelského ovládacího prvku z vybraného kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="e408d-330">Na **Default.aspx** stránky, vyberte **H2** a **zvuku** elementy.</span><span class="sxs-lookup"><span data-stu-id="e408d-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="e408d-331">Klikněte pravým tlačítkem a vyberte **extrahovat do uživatelského ovládacího prvku**.</span><span class="sxs-lookup"><span data-stu-id="e408d-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="e408d-332">![Extrahovat do uživatelského ovládacího prvku nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extrahovat do uživatelského ovládacího prvku nabídky")</span><span class="sxs-lookup"><span data-stu-id="e408d-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="e408d-333">*Extrahovat do uživatelského ovládacího prvku nabídky*</span><span class="sxs-lookup"><span data-stu-id="e408d-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="e408d-334">Zadejte název nového uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e408d-334">Type a name for the new user control.</span></span> <span data-ttu-id="e408d-335">Například **Jukebox.ascx**a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e408d-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="e408d-336">![Uložení extrahovaných uživatelského ovládacího prvku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "uložení extrahovaných uživatelského ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="e408d-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="e408d-337">*Uložení extrahovaných uživatelského ovládacího prvku*</span><span class="sxs-lookup"><span data-stu-id="e408d-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="e408d-338">Všimněte si, že vybraný kód byl extrahován do uživatelského ovládacího prvku a byl nahrazen původní umístění vybraném kódu s instancí nového uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e408d-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="e408d-339">![Stránka automaticky aktualizována na používání nového uživatelského ovládacího prvku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "stránky automaticky aktualizována na používání nového uživatelského ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="e408d-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="e408d-340">*Stránka automaticky aktualizována na používání nového uživatelského ovládacího prvku*</span><span class="sxs-lookup"><span data-stu-id="e408d-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="e408d-341">Stisknutím klávesy **F5** spusťte a ověřte, zda ovládací prvek funguje.</span><span class="sxs-lookup"><span data-stu-id="e408d-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="e408d-342">Cvičení 3: Co je nového v editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e408d-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="e408d-343">Zápis nebo úpravy kódu jazyka JavaScript není snadný úkol, zejména v případě, že aplikace se spustí v velikost a zjistíte, se zabývá dlouhé soubory a stovky funkce.</span><span class="sxs-lookup"><span data-stu-id="e408d-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="e408d-344">Skript vývojáři obvykle nemusí provést další úkony a zachovat čitelnost kódu a navigace mezi soubory.</span><span class="sxs-lookup"><span data-stu-id="e408d-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="e408d-345">Díky zahrnutí knihovny jazyka JavaScript, jako je jQuery skript navigace přestal samotné složité kvůli délka kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="e408d-346">Visual Studio obnovil editor jazyka JavaScript s velcí přístupné a uspořádané režimu kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="e408d-347">Mnoho funkcí sady Visual Studio, které existovaly už v editoru jazyka C# nebo VB je nyní implementována v JavaScript editoru: přechod na definici, Automatické odsazení, dokumentace a ověření v případě, že píšete.</span><span class="sxs-lookup"><span data-stu-id="e408d-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="e408d-348">Pomocí seznamu obnovené IntelliSense máte katalogu funkce jazyka JavaScript na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="e408d-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="e408d-349">V tomto cvičení se dozvíte některé nové funkce a vylepšení editoru jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="e408d-350">Budete procházet ukázkové soubory a každé nové vlastnosti, které způsobí, že programování v jazyce JavaScript v sadě Visual Studio 2012 efektivnější zjišťování.</span><span class="sxs-lookup"><span data-stu-id="e408d-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="e408d-351">Úloha 1 – nové funkce editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e408d-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="e408d-352">Tato úloha vás seznámí s některé nové funkce editoru jazyka JavaScript, která se zaměřují na uspořádání kódu a přináší lepší uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="e408d-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="e408d-353">Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e408d-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e408d-354">Stisknutím klávesy **F5** ke spuštění aplikace, pak klikněte na odkaz JavaScript na navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="e408d-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="e408d-355">Aktualizujte stránku několik časy a kontrola jak zvýší čítač.</span><span class="sxs-lookup"><span data-stu-id="e408d-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="e408d-356">![Čítač stránky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "čítač stránky")</span><span class="sxs-lookup"><span data-stu-id="e408d-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="e408d-357">*Čítač stránky*</span><span class="sxs-lookup"><span data-stu-id="e408d-357">*Page counter*</span></span>
3. <span data-ttu-id="e408d-358">Zavřete prohlížeč a přejděte zpět do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e408d-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="e408d-359">Otevřít **JavaScript.aspx** stránky a vyhledejte **&lt;skript&gt;** blok (viz dole).</span><span class="sxs-lookup"><span data-stu-id="e408d-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="e408d-360">Následující kód používá místní úložiště HTML5 pro ukládání *pageLoadCount* proměnná, která ukládá počet, kolikrát má aktuální uživatel navštívil stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="e408d-361">Místní úložiště je databáze na straně klienta klíč hodnota, zavedl se standardem HTML5.</span><span class="sxs-lookup"><span data-stu-id="e408d-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="e408d-362">Data uložená na místním počítači uvnitř webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e408d-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="e408d-363">Ujistěte se, že deklarace DOCTYPE je nastaven na XHTML5 než budete pokračovat s dalšími kroky.</span><span class="sxs-lookup"><span data-stu-id="e408d-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="e408d-364">Upravit kód a Všimněte si, že obsahuje IntelliSense pro JavaScript HTML5 funkce, jako je místní úložiště a jejich vnitřní metody.</span><span class="sxs-lookup"><span data-stu-id="e408d-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="e408d-365">![Funkce jazyka JavaScript HTML5 v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript funkce v jazyce JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e408d-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="e408d-366">*Funkce jazyka JavaScript HTML5 v jazyce JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e408d-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="e408d-367">Klikněte na libovolné levá hranatá závorka (**{**) z skriptování kódu a Všimněte si, že jsou zvýrazněny uvedených v závorkách.</span><span class="sxs-lookup"><span data-stu-id="e408d-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="e408d-368">![Závorky jsou zvýrazněny](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "závorky jsou zvýrazněné.")</span><span class="sxs-lookup"><span data-stu-id="e408d-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="e408d-369">*Závorky jsou zvýrazněné.*</span><span class="sxs-lookup"><span data-stu-id="e408d-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="e408d-370">Zrušením komentáře u funkce **testAutoAlign()** (můžete si vybrat tři řádky **CTRL** + **K**; **CTRL** + **U**) a vyhledejte kurzor myši do kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="e408d-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="e408d-371">Stisknutím klávesy enter přidat druhý řádek.</span><span class="sxs-lookup"><span data-stu-id="e408d-371">Press enter to append a second line.</span></span> <span data-ttu-id="e408d-372">Všimněte si, že kód je nyní **zarovnané** a **automaticky odsazený**.</span><span class="sxs-lookup"><span data-stu-id="e408d-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="e408d-373">![Kód jazyka JavaScript je automaticky zarovnány](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kód jazyka JavaScript je automaticky zarovnání")</span><span class="sxs-lookup"><span data-stu-id="e408d-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="e408d-374">*Kód jazyka JavaScript je automaticky zarovnání*</span><span class="sxs-lookup"><span data-stu-id="e408d-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="e408d-375">Úloha 2 – ověření jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="e408d-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="e408d-376">V této úloze bude zjišťovat nové ověření jazyka JavaScript pro standardní ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="e408d-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="e408d-377">Tato funkce vám pomůže psát kompatibilní kód jazyka JavaScript, při brání problémy skriptování před nasazením webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="e408d-378">Visual Studio 2010 implementované ECMAStript3 dodržování předpisů, zatímco služba Visual Studio 2012 poskytuje ECMAScript5 dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="e408d-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="e408d-379">Otevřít **ECMA5script5.js** umístěna ve složce **Scripts\custom** složky projektu.</span><span class="sxs-lookup"><span data-stu-id="e408d-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="e408d-380">Teď budete testovat ověřování pro ECMAScript5 standard.</span><span class="sxs-lookup"><span data-stu-id="e408d-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="e408d-381">Si můžete prohlédnout &quot; **použití Striktního režimu** &quot; směr v první řádek souboru, který umožňuje ECMAScript5 **přísném režimu**.</span><span class="sxs-lookup"><span data-stu-id="e408d-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="e408d-382">Tento režim se skládá z podmnožinu jazyka, který vysvětluje nejednoznačnosti z posledních edice a přidá některé nové funkce, jako jsou gettery a settery, podpora knihovny pro JSON a úplnější reflexe na vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="e408d-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="e408d-383">Otevřít **seznam chyb** Pokud ještě není otevřené (**zobrazení** nabídky | **Seznam chyb**).</span><span class="sxs-lookup"><span data-stu-id="e408d-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="e408d-384">Všimněte si, že **funkce** podtržené deklarace.</span><span class="sxs-lookup"><span data-stu-id="e408d-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="e408d-385">Je to proto, že v ECMA5 standardní funkce nelze vnořit do struktury jazyka.</span><span class="sxs-lookup"><span data-stu-id="e408d-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="e408d-386">V chybě bude seznam níže můžete zobrazit podrobnosti upozornění.</span><span class="sxs-lookup"><span data-stu-id="e408d-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="e408d-387">![Chybovou zprávu ověření jazyka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "chybovou zprávu ověření jazyka JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e408d-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="e408d-388">*Chybovou zprávu ověření jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e408d-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="e408d-389">Okomentujte **&quot;použití Striktního režimu&quot;** směr a Všimněte si, že chyby zmizí, ale zůstávají upozornění.</span><span class="sxs-lookup"><span data-stu-id="e408d-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="e408d-390">Napište libovolný řetězec jako v poslední řádek souboru **&quot;testování&quot;** (včetně uvozovek označuje jako řetězec).</span><span class="sxs-lookup"><span data-stu-id="e408d-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="e408d-391">Zápis období vedle řetězec, který se zobrazí seznam technologie IntelliSense a vyberte **trim** možnost.</span><span class="sxs-lookup"><span data-stu-id="e408d-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="e408d-392">Ve standardním ECMAScript5 řetězcové hodnoty a proměnné nemají řetězcových metod, které jsou definované jako oříznout, velká písmena, vyhledávání a nahrazení.</span><span class="sxs-lookup"><span data-stu-id="e408d-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="e408d-393">![Seznam technologie IntelliSense v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "seznam technologie IntelliSense v jazyce JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e408d-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="e408d-394">*Seznam technologie IntelliSense v jazyce JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e408d-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="e408d-395">Úloha 3 – dokumentace XML pro jazyk JavaScript</span><span class="sxs-lookup"><span data-stu-id="e408d-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="e408d-396">V této úloze bude prozkoumání funkcí sady Visual Studio pro dokumentaci XML v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="e408d-397">Zobrazí se že seznam technologie IntelliSense jazyka JavaScript nyní zobrazuje dokumentace XML jednotlivých funkcí.</span><span class="sxs-lookup"><span data-stu-id="e408d-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="e408d-398">Kromě toho bude zjišťovat navigační funkce v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="e408d-399">Otevřít **XMLDoc.js** soubor umístěný ve **skripty a vlastní** složky projektu.</span><span class="sxs-lookup"><span data-stu-id="e408d-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="e408d-400">Tento soubor obsahuje dokumentaci XML na všech funkcí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="e408d-401">![Dokumentace JavaScript XML integrované technologie IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "dokumentace JavaScript XML integrované technologie IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="e408d-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="e408d-402">*Dokumentace JavaScript XML integrované technologie IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="e408d-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="e408d-403">Pod **přidat** fungovat v **XMLDoc.js** soubor, vytvořte novou funkci s názvem **testování**.</span><span class="sxs-lookup"><span data-stu-id="e408d-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="e408d-404">V **testování** fungovat, zavolejte **vynásobit** funkce, která přijímá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="e408d-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="e408d-405">Všimněte si, že pole Popis se zobrazuje **vynásobit** dokumentaci k funkci.</span><span class="sxs-lookup"><span data-stu-id="e408d-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="e408d-406">![Dokumentace XML pro funkce jazyka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentace XML pro funkce jazyka JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e408d-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="e408d-407">*Dokumentace XML pro funkce jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e408d-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="e408d-408">Dokončení příkazu call funkce a typ *tečkou* o otevření seznamu technologie IntelliSense pro vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e408d-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="e408d-409">Všimněte si, že se detekuje aplikace Visual Studio **vrátit** hodnotu v dokumentaci, zpracuje hodnotu jako číslo.</span><span class="sxs-lookup"><span data-stu-id="e408d-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="e408d-410">![Dokumentace XML pro typy vrácených hodnot](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentace XML pro návratové typy")</span><span class="sxs-lookup"><span data-stu-id="e408d-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="e408d-411">*Dokumentace XML pro návratové typy*</span><span class="sxs-lookup"><span data-stu-id="e408d-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="e408d-412">Nyní vložte volání pro přidání funkce.</span><span class="sxs-lookup"><span data-stu-id="e408d-412">Now, insert a call to add function.</span></span> <span data-ttu-id="e408d-413">Všimněte si, že editor JavaScriptu teď podporuje přetížení funkce.</span><span class="sxs-lookup"><span data-stu-id="e408d-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="e408d-414">Při zápisu název funkce, budou moct vyberte některou z dostupných přetížení zadané v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e408d-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="e408d-415">![Dokumentace XML pro přetížení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentace XML pro přetížení")</span><span class="sxs-lookup"><span data-stu-id="e408d-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="e408d-416">*Dokumentace XML pro přetížení*</span><span class="sxs-lookup"><span data-stu-id="e408d-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="e408d-417">Otevřít **GotoDefinition.js** souboru a vyhledejte **$().html()** volání funkce.</span><span class="sxs-lookup"><span data-stu-id="e408d-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="e408d-418">Najít kurzor na **html**.</span><span class="sxs-lookup"><span data-stu-id="e408d-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="e408d-419">Stisknutím klávesy **F12** a přejít k definici.</span><span class="sxs-lookup"><span data-stu-id="e408d-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="e408d-420">Všimněte si, že teď můžete používat a procházet kód JavaScriptu bez použití **najít** nástroj.</span><span class="sxs-lookup"><span data-stu-id="e408d-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="e408d-421">Najděte kurzor na instrukci jQuery před blok signatury na konci souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="e408d-422">Stisknutím klávesy **F12**.</span><span class="sxs-lookup"><span data-stu-id="e408d-422">Press **F12**.</span></span> <span data-ttu-id="e408d-423">Bude přejděte na soubor knihovny jQuery.</span><span class="sxs-lookup"><span data-stu-id="e408d-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="e408d-424">Všimněte si, že se můžete dostat také jQuery souborů pomocí **F12**.</span><span class="sxs-lookup"><span data-stu-id="e408d-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="e408d-425">![Přejdete na definice jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "přejdete na definice jQuery")</span><span class="sxs-lookup"><span data-stu-id="e408d-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="e408d-426">*Přejděte na definice jQuery*</span><span class="sxs-lookup"><span data-stu-id="e408d-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="e408d-427">Ujistěte se, že GotoDefinition.js neobsahuje žádné chyby syntaxe před uložením tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="e408d-428">Cvičení 4: Sdružování a Minifikace</span><span class="sxs-lookup"><span data-stu-id="e408d-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="e408d-429">Kolikrát websites obsahují více než jeden JavaScript nebo šablon stylů CSS soubor?</span><span class="sxs-lookup"><span data-stu-id="e408d-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="e408d-430">Jde o velmi běžný scénář, kde sdružování a minifikace může pomoct snížit velikost souboru a nastavte lokality rychlejší.</span><span class="sxs-lookup"><span data-stu-id="e408d-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="e408d-431">Nová funkce pro vytváření prostředků v ASP.NET 4.5 sady sadu soubory JS a CSS do jednoho elementu a snižuje jeho velikost pomocí minifikace obsah (to znamená odebrání mezery nejsou vyžadovány, odebírání komentářů, snížení identifikátory).</span><span class="sxs-lookup"><span data-stu-id="e408d-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="e408d-432">Sdružování a minifikace v technologii ASP.NET 4.5 je provést v době běhu, tak, aby proces identifikace uživatelský agent (např. aplikace Internet Explorer, Mozilla atd.) a proto komprese zlepšit cílení na uživatele prohlížeče (například odebrání položky, který je Mozilla konkrétní Když požadavek pochází z aplikace Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="e408d-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="e408d-433">V tomto cvičení se dozvíte, jak povolit a použít různé typy sdružování a minifikace v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e408d-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="e408d-434">Úloha 1 – instalace sdružování a Minifikace balíčku od Nugetu</span><span class="sxs-lookup"><span data-stu-id="e408d-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="e408d-435">Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e408d-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e408d-436">Otevřete konzolu Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="e408d-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="e408d-437">K tomuto účelu použijte nabídku **zobrazení** | **ostatní Windows** | **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e408d-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="e408d-438">![Otevření správce file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole balíčku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otevřete konzoly Správce balíčků")</span><span class="sxs-lookup"><span data-stu-id="e408d-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="e408d-439">*Otevření konzole Správce balíčků*</span><span class="sxs-lookup"><span data-stu-id="e408d-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="e408d-440">V **Konzola správce balíčků** typ **Install-Package Microsoft.Web.Optimization** a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="e408d-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="e408d-441">Úloha 2 – výchozí sady</span><span class="sxs-lookup"><span data-stu-id="e408d-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="e408d-442">Nejjednodušší způsob, jak používat sdružování a minifikace je umožnit výchozí sady.</span><span class="sxs-lookup"><span data-stu-id="e408d-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="e408d-443">Tato metoda používá konvence umožňuje odkazovat na připojené a minifikovaný verze pro soubory JS a CSS ve složce.</span><span class="sxs-lookup"><span data-stu-id="e408d-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="e408d-444">V této úloze se dozvíte, jak povolit a odkazovat na připojené a minifikovaný soubory JS a CSS a zobrazit výsledný výstup.</span><span class="sxs-lookup"><span data-stu-id="e408d-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="e408d-445">Pokud ještě není otevřen, začněte **sady Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení nachází v **Source\WhatsNewASPNET** složka tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="e408d-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="e408d-446">V **Průzkumníka řešení**, rozbalte **styly**, **Scripts\custom** a **Scripts\bundle** složek.</span><span class="sxs-lookup"><span data-stu-id="e408d-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="e408d-447">Všimněte si, že aplikace používá více než jeden šablon stylů CSS a JS souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="e408d-448">![Soubory více šablon stylů a jazyka JavaScript v aplikaci](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "soubory více šablon stylů a jazyka JavaScript v aplikaci")</span><span class="sxs-lookup"><span data-stu-id="e408d-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="e408d-449">*Více souborů šablon stylů a jazyka JavaScript v aplikaci*</span><span class="sxs-lookup"><span data-stu-id="e408d-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="e408d-450">Otevřít **Global.asax.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="e408d-451">Všimněte si, že nový **Microsoft.Web.Optimization** obor názvů je označené jako komentář na začátku souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="e408d-452">Zrušte komentář na pomocí směrnice obsahují sdružování a minifikace funkce.</span><span class="sxs-lookup"><span data-stu-id="e408d-452">Uncomment the using directive to include the bundling and minification features.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="e408d-453">Vyhledejte **aplikace\_Start** metody.</span><span class="sxs-lookup"><span data-stu-id="e408d-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="e408d-454">V této metodě zrušte komentář u volání EnableDefaultBundles, jak je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="e408d-455">Umožňuje nám to odkazuje na připojené kolekci souborů šablon stylů CSS ve složce pomocí cesty ke složce, plus &quot;šablon stylů CSS&quot; nebo &quot;JS&quot; příponu.</span><span class="sxs-lookup"><span data-stu-id="e408d-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="e408d-456">Otevřít **Optimization.aspx** souboru a vyhledejte ovládací prvek obsahu pro **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="e408d-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="e408d-457">Všimněte si, že soubory šablon stylů CSS a JS mít jednu značku odkazované.</span><span class="sxs-lookup"><span data-stu-id="e408d-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="e408d-458">Tento kód je pro účely ukázky.</span><span class="sxs-lookup"><span data-stu-id="e408d-458">This code is for demo purposes.</span></span> <span data-ttu-id="e408d-459">V ideálním případě bude odkazovat sad v souboru Site.Master.</span><span class="sxs-lookup"><span data-stu-id="e408d-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="e408d-460">V tomto ukázkovém kódu zjistíte, že některé soubory v sadě jsou také se na ně odkazovat soubor Site.Master odkazu poslední redundantní.</span><span class="sxs-lookup"><span data-stu-id="e408d-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="e408d-461">Všimněte si, že odkazy používají konvence vytváření prostředků v **href** atribut zobrazíte všechny šablony stylů CSS a JS soubory z stylů a Scripts\custom složky v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e408d-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="e408d-462">Může používat cestu **skripty/vlastní/JS** jak je znázorněno níže, k vytvoření balíčku a minifikace všechny soubory JS v **skripty a vlastní** složky.</span><span class="sxs-lookup"><span data-stu-id="e408d-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="e408d-463">Toto je výchozí chování sady výchozí.</span><span class="sxs-lookup"><span data-stu-id="e408d-463">This is the default behavior with the default bundles.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="e408d-464">Otevřít **Styles\Site.css** souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="e408d-465">Všimněte si, že původní soubor CSS obsahuje kód odsazený, mezery a komentáře, které zvětšení souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="e408d-466">(Také soubor jazyka JavaScript obsahuje mezery a komentáře).</span><span class="sxs-lookup"><span data-stu-id="e408d-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="e408d-467">![Jeden z původní šablony stylů CSS soubory ve složce Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jeden z původní šablony stylů CSS soubory ve složce Scripts")</span><span class="sxs-lookup"><span data-stu-id="e408d-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="e408d-468">*Jeden z původní šablony stylů CSS souborů ve složce Scripts*</span><span class="sxs-lookup"><span data-stu-id="e408d-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="e408d-469">Stisknutím klávesy **F5** spusťte aplikaci a přejděte **optimalizace** stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="e408d-470">Klikněte na **sady šablon stylů CSS** odkaz ke stažení a otevřete soubor.</span><span class="sxs-lookup"><span data-stu-id="e408d-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="e408d-471">Projděte si soubor minifikovaný jako součást balíčku.</span><span class="sxs-lookup"><span data-stu-id="e408d-471">Check out the minified bundled file.</span></span> <span data-ttu-id="e408d-472">Můžete si všimnout, že byly odebrány všechny mezery, komentáře a odsazení znaků, generování menší soubor.</span><span class="sxs-lookup"><span data-stu-id="e408d-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="e408d-473">![Soubory šablon stylů CSS v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "soubory balíčků šablon stylů CSS")</span><span class="sxs-lookup"><span data-stu-id="e408d-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="e408d-474">*Soubory šablon stylů CSS v sadě*</span><span class="sxs-lookup"><span data-stu-id="e408d-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="e408d-475">Nyní klikejte na příkaz **JS sady** odkaz k otevření souboru JavaScriptu spojeny.</span><span class="sxs-lookup"><span data-stu-id="e408d-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="e408d-476">V Průzkumníku upozornění můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="e408d-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="e408d-477">Všimněte si, že soubory jazyka JavaScript v rámci **vlastní** složky jsou taky spojeny a minifikovaný.</span><span class="sxs-lookup"><span data-stu-id="e408d-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="e408d-478">![Soubory jazyka JavaScript v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "soubory balíčků jazyka JavaScript")</span><span class="sxs-lookup"><span data-stu-id="e408d-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="e408d-479">*Soubory jazyka JavaScript v sadě*</span><span class="sxs-lookup"><span data-stu-id="e408d-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="e408d-480">Povolení komprese pro soubory CSS a JS bylo mnohem složitější v předchozí verzi technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e408d-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="e408d-481">Nyní, jak jste viděli, stačí přidat jeden řádek *Global.asax* soubor povolit sdružování a odkázat na připojené soubory z vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="e408d-482">Úloha 3 – statické sady</span><span class="sxs-lookup"><span data-stu-id="e408d-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="e408d-483">Statická sada přístupu umožňuje přizpůsobit sadu souborů sady, odkaz a připravenost k minifikaci metodu, která se použije.</span><span class="sxs-lookup"><span data-stu-id="e408d-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="e408d-484">V této úlohy můžete nakonfigurovat statické sady definovat konkrétní sadu souborů pro vytvoření balíčku a minifikace.</span><span class="sxs-lookup"><span data-stu-id="e408d-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="e408d-485">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="e408d-485">Close the browser.</span></span>
2. <span data-ttu-id="e408d-486">Otevřít **Global.asax.cs** souboru a vyhledejte **aplikace\_Start** metoda.</span><span class="sxs-lookup"><span data-stu-id="e408d-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="e408d-487">Zrušením komentáře u statické sady kódu, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="e408d-488">Definování statickou sadu, která se bude odkazovat &quot; **~/StaticBundle** &quot; virtuální cesty a použití **JsMinify** pro minimalizaci všechny zadané soubory s **AddFile** metody.</span><span class="sxs-lookup"><span data-stu-id="e408d-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="e408d-489">Nakonec přidáte statická sada, která má **BundleTable** a že ho povolíte.</span><span class="sxs-lookup"><span data-stu-id="e408d-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="e408d-490">Všimněte si, že soubory nejsou umístěny na stejném místě; Toto je Další výhodou nad výchozí sdružování.</span><span class="sxs-lookup"><span data-stu-id="e408d-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="e408d-491">Otevřít **Optimization.aspx** souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="e408d-492">Všimněte si, že odkaz na **statické JS sady** je pomocí cesty je deklarován při konfiguraci sady statických v souboru Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="e408d-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="e408d-493">Stisknutím klávesy **F5** ke spuštění aplikace a potom přejděte na **optimalizace** stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="e408d-494">Klikněte na **statické JS sady** odkaz k otevření souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="e408d-495">Všimněte si, že minifikovaný dodávat soubor jazyka JavaScript je výstup pro všechny soubory jazyka JavaScript nakonfigurovaný v souboru statické sady v rámci cesty &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="e408d-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="e408d-496">![Statické soubory sady JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "sady JavaScript statické soubory")</span><span class="sxs-lookup"><span data-stu-id="e408d-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="e408d-497">*Seskupit statické soubory jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="e408d-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="e408d-498">Zavřete prohlížeč a vraťte se do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e408d-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="e408d-499">Úloha 4 – dynamické složku sady</span><span class="sxs-lookup"><span data-stu-id="e408d-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="e408d-500">V této úloze se dozvíte, jak nakonfigurovat dynamické složku sady.</span><span class="sxs-lookup"><span data-stu-id="e408d-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="e408d-501">Výkon dynamické sdružování je, že můžete zahrnout statické JavaScript, jakož i jiné soubory v jazycích, které se kompiluje do jazyka JavaScript a proto nevyžadují nějaké zpracování předtím, než se spustí sdružování.</span><span class="sxs-lookup"><span data-stu-id="e408d-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="e408d-502">V tomto příkladu se dozvíte, jak používat **DynamicFolderBundle** třídy za účelem vytvoření dynamického sada prostředků pro soubory, které jsou napsané v CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="e408d-503">CofeeScript je programovací jazyk, který kompiluje do jazyka JavaScript a nabízí jednodušší syntaxí pro psaní kódu jazyka JavaScript, lepší čitelnost a zkrácení v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="e408d-504">Otevřít **Global.asax.cs** souboru a vyhledejte **aplikace\_Start** metoda.</span><span class="sxs-lookup"><span data-stu-id="e408d-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="e408d-505">Zrušením komentáře u dynamických sady kódu, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="e408d-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="e408d-506">Definujete sadu dynamické složku, která bude používat **CoffeeMinify** připravenost k minifikaci vlastní procesor, který se bude vztahovat jenom na soubory s &quot; **.coffee** &quot; (rozšíření Soubory CoffeeScript).</span><span class="sxs-lookup"><span data-stu-id="e408d-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="e408d-507">Všimněte si, že můžete použít vzor hledání pro výběr souborů pro vytvoření balíčku ve složce, jako je třeba "\*.coffee".</span><span class="sxs-lookup"><span data-stu-id="e408d-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="e408d-508">Otevřete konzolu Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="e408d-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="e408d-509">K tomuto účelu použijte nabídku **zobrazení** | **ostatní Windows** | **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="e408d-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="e408d-510">V **Konzola správce balíčků** typ **Install-Package CoffeeSharp** a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="e408d-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="e408d-511">Klikněte na tlačítko **zobrazit všechny soubory** tlačítko **Průzkumníka řešení** okna</span><span class="sxs-lookup"><span data-stu-id="e408d-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="e408d-512">![Zobrazení všech souborů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "zobrazující všechny soubory")</span><span class="sxs-lookup"><span data-stu-id="e408d-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="e408d-513">*Zobrazení všech souborů*</span><span class="sxs-lookup"><span data-stu-id="e408d-513">*Showing all files*</span></span>
6. <span data-ttu-id="e408d-514">Klikněte pravým tlačítkem myši **CoffeeMinify.cs** soubor **Průzkumníku řešení** a vyberte **zahrnout do projektu**</span><span class="sxs-lookup"><span data-stu-id="e408d-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="e408d-515">![Zahrnout do projektu soubor CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs soubor zahrnout projektu")</span><span class="sxs-lookup"><span data-stu-id="e408d-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="e408d-516">*Zahrnout soubor CoffeeMinify.cs do projektu*</span><span class="sxs-lookup"><span data-stu-id="e408d-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="e408d-517">Otevřít **CoffeeMinify.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="e408d-518">Tato třída dědí z JsMinify k minifikaci Javascriptového výstupu plynoucí z kompilování kódu CoffeeScript.</span><span class="sxs-lookup"><span data-stu-id="e408d-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="e408d-519">Volá CoffeeScript kompilátor nejprve generovat kód jazyka JavaScript, a potom ho odešle JsMinify.Process metody k minifikaci výsledný kód.</span><span class="sxs-lookup"><span data-stu-id="e408d-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="e408d-520">Otevřít **Script1.coffee** a **Script2.coffee** souborů z doručené pošty **skripty/sady** složky.</span><span class="sxs-lookup"><span data-stu-id="e408d-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="e408d-521">Tyto soubory bude obsahovat CoffeScript kód se zkompiluje při provádění sdružování s třídou CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="e408d-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="e408d-522">V zájmu jednoduchosti soubory CoffeeScript k dispozici jsou pouze včetně CoffeeScript ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="e408d-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="e408d-523">Komentáře jsou vyloučeny JsMinify procesem.</span><span class="sxs-lookup"><span data-stu-id="e408d-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="e408d-524">![Soubory CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "soubory CoffeeScript")</span><span class="sxs-lookup"><span data-stu-id="e408d-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="e408d-525">*Soubory CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="e408d-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) nabízí jednodušší syntaxi pro psaní kódu jazyka JavaScript, zkrácení v jazyce JavaScript a čitelnost, stejně jako přidávání jiných funkcí, jako je obsah pole a porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="e408d-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="e408d-527">Otevřít **Optimization.aspx** souboru a vyhledejte odkazy sady.</span><span class="sxs-lookup"><span data-stu-id="e408d-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="e408d-528">Všimněte si, že odkaz na **dynamické JS sady** odkazuje **skripty/sady** složky pomocí **/kávy** přípony, které jste nakonfigurovali pro sadu složek dynamické.</span><span class="sxs-lookup"><span data-stu-id="e408d-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="e408d-529">Stisknutím klávesy **F5** ke spuštění aplikace a potom přejděte na **optimalizace** stránky.</span><span class="sxs-lookup"><span data-stu-id="e408d-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="e408d-530">Klikněte na **dynamické JS sady** odkaz k otevření generovaného souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="e408d-531">Všimněte si, že obsah, který byl součástí Tato sada obsahuje pouze **.coffee** soubory.</span><span class="sxs-lookup"><span data-stu-id="e408d-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="e408d-532">Můžete také zobrazit, že kód CoffeeScript byla zkompilována do jazyka JavaScript a komentovaná řádky se odebrala.</span><span class="sxs-lookup"><span data-stu-id="e408d-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="e408d-533">![Dynamické soubory JS sady](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamické JS sady souborů")</span><span class="sxs-lookup"><span data-stu-id="e408d-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="e408d-534">*Dynamické sady soubory JS*</span><span class="sxs-lookup"><span data-stu-id="e408d-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="e408d-535">Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="e408d-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e408d-536">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e408d-536">Summary</span></span>

<span data-ttu-id="e408d-537">Toto testovací prostředí vám umožní pochopit, co je nového v ASP.NET a vývoj v sadě Visual Studio 2012 pro Web a jak využívat řadu vylepšení v sadě Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="e408d-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="e408d-538">Po dokončení tohoto praktického testovacího prostředí, mají zkušenosti použití nových funkcí a vylepšení v aplikaci Visual Studio 2012 editory pro šablony stylů CSS, JavaScript a HTML.</span><span class="sxs-lookup"><span data-stu-id="e408d-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="e408d-539">Kromě toho mají zkušenosti, jak Visual Studio 2012 implementuje integrované sdružování a minifikace.</span><span class="sxs-lookup"><span data-stu-id="e408d-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e408d-540">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="e408d-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e408d-541">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e408d-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e408d-542">Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e408d-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e408d-543">Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e408d-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e408d-544">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e408d-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e408d-545">Klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e408d-545">Click on **Install Now**.</span></span> <span data-ttu-id="e408d-546">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="e408d-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e408d-547">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.</span><span class="sxs-lookup"><span data-stu-id="e408d-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e408d-548">![Instalace sady Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalace sady Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e408d-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e408d-549">*Instalace sady Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e408d-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e408d-550">Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e408d-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Přijetí podmínek licence](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="e408d-552">*Přijetí podmínek licence*</span><span class="sxs-lookup"><span data-stu-id="e408d-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e408d-553">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="e408d-553">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="e408d-555">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="e408d-555">*Installation progress*</span></span>
6. <span data-ttu-id="e408d-556">Až instalace skončí, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e408d-556">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="e408d-558">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="e408d-558">*Installation completed*</span></span>
7. <span data-ttu-id="e408d-559">Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="e408d-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e408d-560">Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="e408d-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web dlaždice](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="e408d-562">*VS Express for Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="e408d-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e408d-563">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="e408d-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="e408d-564">Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e408d-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="e408d-565">Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e408d-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="e408d-566">Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="e408d-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-567">Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="e408d-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="e408d-568">Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="e408d-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="e408d-569">![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e408d-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="e408d-570">*Přihlaste se k portálu pro správu Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="e408d-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="e408d-571">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e408d-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="e408d-572">![Vytvoření nového webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="e408d-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="e408d-573">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="e408d-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="e408d-574">Klikněte na tlačítko **Compute** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="e408d-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="e408d-575">Potom vyberte **rychlé vytvoření** možnost.</span><span class="sxs-lookup"><span data-stu-id="e408d-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="e408d-576">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.</span><span class="sxs-lookup"><span data-stu-id="e408d-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-577">Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e408d-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="e408d-578">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="e408d-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="e408d-579">Nezahrnuje kroky pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="e408d-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="e408d-580">![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="e408d-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="e408d-581">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="e408d-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="e408d-582">Počkejte, dokud nové **webu** se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="e408d-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="e408d-583">Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="e408d-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="e408d-584">Zkontrolujte, jestli funguje nový web.</span><span class="sxs-lookup"><span data-stu-id="e408d-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="e408d-585">![Na nový web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "přechodu na nový web")</span><span class="sxs-lookup"><span data-stu-id="e408d-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="e408d-586">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="e408d-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="e408d-587">![Spuštění webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "spuštění webové stránky")</span><span class="sxs-lookup"><span data-stu-id="e408d-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="e408d-588">*Spuštění webové stránky*</span><span class="sxs-lookup"><span data-stu-id="e408d-588">*Web site running*</span></span>
6. <span data-ttu-id="e408d-589">Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.</span><span class="sxs-lookup"><span data-stu-id="e408d-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="e408d-590">![Otevřete správu webových stránek](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otevřete správu webových stránek")</span><span class="sxs-lookup"><span data-stu-id="e408d-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="e408d-591">*Otevřete správu webových stránek*</span><span class="sxs-lookup"><span data-stu-id="e408d-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="e408d-592">V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="e408d-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-593">*Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování.</span><span class="sxs-lookup"><span data-stu-id="e408d-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="e408d-594">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace.</span><span class="sxs-lookup"><span data-stu-id="e408d-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="e408d-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="e408d-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="e408d-596">![Stahování webové stránky publikovat profil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "stahování webové stránky profil publikování")</span><span class="sxs-lookup"><span data-stu-id="e408d-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="e408d-597">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="e408d-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="e408d-598">Stáhněte si soubor profilu publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="e408d-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="e408d-599">Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="e408d-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="e408d-600">![Ukládání souboru profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "ukládá se profil publikování")</span><span class="sxs-lookup"><span data-stu-id="e408d-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="e408d-601">*Ukládá se profil publikování*</span><span class="sxs-lookup"><span data-stu-id="e408d-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="e408d-602">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="e408d-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="e408d-603">Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e408d-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="e408d-604">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="e408d-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="e408d-605">Pro uložení databáze aplikace budete potřebovat databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="e408d-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="e408d-606">Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="e408d-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="e408d-607">Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="e408d-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="e408d-608">Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="e408d-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="e408d-609">Nevytvářet databáze, protože vytvoří se v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="e408d-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="e408d-610">![Řídicí panel serveru SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "řídicího panelu serveru SQL Database")</span><span class="sxs-lookup"><span data-stu-id="e408d-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="e408d-611">*Řídicí panel serveru SQL Database*</span><span class="sxs-lookup"><span data-stu-id="e408d-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="e408d-612">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="e408d-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="e408d-613">Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole.</span><span class="sxs-lookup"><span data-stu-id="e408d-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="e408d-614">Zadejte název pravidla a klikněte na tlačítko ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e408d-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Přidat IP adresu klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="e408d-616">*Přidat IP adresu klienta*</span><span class="sxs-lookup"><span data-stu-id="e408d-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="e408d-617">Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e408d-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="e408d-619">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="e408d-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e408d-620">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu</span><span class="sxs-lookup"><span data-stu-id="e408d-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="e408d-621">Vraťte se do řešení ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e408d-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="e408d-622">V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e408d-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="e408d-623">![Publikování aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="e408d-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="e408d-624">*Publikování na webu*</span><span class="sxs-lookup"><span data-stu-id="e408d-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="e408d-625">Importujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="e408d-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="e408d-626">![Import profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="e408d-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="e408d-627">*Import publikačního profilu*</span><span class="sxs-lookup"><span data-stu-id="e408d-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="e408d-628">Klikněte na tlačítko **ověřit připojení**.</span><span class="sxs-lookup"><span data-stu-id="e408d-628">Click **Validate Connection**.</span></span> <span data-ttu-id="e408d-629">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e408d-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e408d-630">Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="e408d-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="e408d-631">![Ověřuje se připojení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "ověřuje se připojení")</span><span class="sxs-lookup"><span data-stu-id="e408d-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="e408d-632">*Ověřuje se připojení*</span><span class="sxs-lookup"><span data-stu-id="e408d-632">*Validating connection*</span></span>
4. <span data-ttu-id="e408d-633">V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="e408d-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="e408d-634">![Konfigurace nasazení webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="e408d-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="e408d-635">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="e408d-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="e408d-636">Konfigurace připojení k databázi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e408d-636">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="e408d-637">V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="e408d-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="e408d-638">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="e408d-638">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="e408d-639">V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="e408d-639">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="e408d-640">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="e408d-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="e408d-641">![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="e408d-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="e408d-642">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="e408d-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="e408d-643">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e408d-643">Then click **OK**.</span></span> <span data-ttu-id="e408d-644">Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e408d-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="e408d-645">![Vytvoření databáze](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "vytvoření řetězce databáze")</span><span class="sxs-lookup"><span data-stu-id="e408d-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="e408d-646">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="e408d-646">*Creating the database*</span></span>
7. <span data-ttu-id="e408d-647">Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="e408d-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="e408d-648">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e408d-648">Then click **Next**.</span></span>

    <span data-ttu-id="e408d-649">![Připojovací řetězec odkazující na SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "připojovací řetězec odkazující na SQL Database")</span><span class="sxs-lookup"><span data-stu-id="e408d-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="e408d-650">*Připojovací řetězec odkazující na SQL Database*</span><span class="sxs-lookup"><span data-stu-id="e408d-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="e408d-651">V **ve verzi Preview** klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="e408d-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="e408d-652">![Publikování webové aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="e408d-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="e408d-653">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="e408d-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="e408d-654">Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.</span><span class="sxs-lookup"><span data-stu-id="e408d-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="e408d-655">![Publikování aplikace do Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "publikování aplikace do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e408d-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="e408d-656">*Aplikace publikovaná do Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="e408d-656">*Application published to Windows Azure*</span></span>
