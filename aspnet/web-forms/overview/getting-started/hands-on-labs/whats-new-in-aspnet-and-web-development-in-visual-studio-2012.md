---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "Co je nového v technologii ASP.NET a vývoj webů v sadě Visual Studio 2012 | Microsoft Docs"
author: rick-anderson
description: "Nová verze sady Visual Studio přináší řadu vylepšení, které jsou zaměřené na vylepšení zkušeností a výkon při práci s technologiemi Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: e57f1200aaa207c9109f2832cbf88629ed385bb5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="c0025-103">Co je nového v technologii ASP.NET a vývoj webů v sadě Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c0025-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="c0025-104">podle [webové táborech Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c0025-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c0025-105">Nová verze sady Visual Studio přináší řadu vylepšení, které jsou zaměřené na vylepšení zkušeností a výkon při práci s technologiemi Web.</span><span class="sxs-lookup"><span data-stu-id="c0025-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="c0025-106">Visual Studio editory pro šablon stylů CSS, JavaScript a HTML byl zcela renovována zahrnout mnoho podpor kód nejvíce vyžádání, jako je například technologie IntelliSense a Automatické odsazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="c0025-107">Z hlediska výkonu sdružování a minimalizace jsou teď integrované jako doba načítání integrované funkce snadno snížit stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="c0025-108">Visual Studio umožňuje pracovat s nejnovější technologie webu.</span><span class="sxs-lookup"><span data-stu-id="c0025-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="c0025-109">Fragmenty CSS3 různé prohlížeče vám pomůže Ujistěte se, že váš web funguje bez ohledu na platformu klienta také s využitím nové prvky HTML5 a funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="c0025-110">Zápis a profilování kódu JavaScript by měl být snazší s touto verzí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0025-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="c0025-111">Seznamy IntelliSense, integrované funkce XML dokumentace a navigační jsou nyní k dispozici pro kód jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0025-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="c0025-112">Nyní máte katalogu JavaScript na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="c0025-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="c0025-113">Kromě toho můžete zkontrolovat dodržování předpisů ECMAScript5 pomocí skriptů a zjištění chyby syntaxe včas.</span><span class="sxs-lookup"><span data-stu-id="c0025-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="c0025-114">Poslední, ale není alespoň tato verze sady Visual Studio implementuje předdefinované sdružování a minimalizace.</span><span class="sxs-lookup"><span data-stu-id="c0025-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="c0025-115">Soubory skriptů a stylů bude mnoha funkcemi a komprimované tak, aby lokality provádí rychleji.</span><span class="sxs-lookup"><span data-stu-id="c0025-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="c0025-116">Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.</span><span class="sxs-lookup"><span data-stu-id="c0025-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="c0025-117">Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c0025-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c0025-118">Cíle</span><span class="sxs-lookup"><span data-stu-id="c0025-118">Objectives</span></span>

<span data-ttu-id="c0025-119">V této rukou na testovacího prostředí se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="c0025-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="c0025-120">Použít nové funkce a vylepšení v editoru stylů CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="c0025-121">Použít nové funkce a vylepšení v editoru HTML</span><span class="sxs-lookup"><span data-stu-id="c0025-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="c0025-122">Použít nové funkce a vylepšení v editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="c0025-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="c0025-123">Konfigurovat a používat sdružování a minimalizace</span><span class="sxs-lookup"><span data-stu-id="c0025-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c0025-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0025-124">Prerequisites</span></span>

- <span data-ttu-id="c0025-125">[Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="c0025-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="c0025-126">[Prostředí Windows PowerShell](https://support.microsoft.com/kb/968930/) (pro skripty instalace – již nainstalován ve Windows 8 a Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="c0025-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="c0025-127">[Internet Explorer 10](https://windows.microsoft.com/en-US/internet-explorer/products/ie/home) – prohlížeč kompatibilní HTML5 nebo</span><span class="sxs-lookup"><span data-stu-id="c0025-127">[Internet Explorer 10](https://windows.microsoft.com/en-US/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c0025-128">Cvičení</span><span class="sxs-lookup"><span data-stu-id="c0025-128">Exercises</span></span>

<span data-ttu-id="c0025-129">Tato rukou na testovacím zahrnuje následující cvičení:</span><span class="sxs-lookup"><span data-stu-id="c0025-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="c0025-130">Cvičení 1: Co je nového v editoru stylů CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="c0025-131">Cvičení 2: Co je nového v editoru HTML</span><span class="sxs-lookup"><span data-stu-id="c0025-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="c0025-132">Cvičení 3: Co je nového v editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="c0025-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="c0025-133">Cvičení 4: Sdružování a Minifikace</span><span class="sxs-lookup"><span data-stu-id="c0025-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="c0025-134">Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.</span><span class="sxs-lookup"><span data-stu-id="c0025-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="c0025-135">Cvičení 1: Co je nového v editoru stylů CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="c0025-136">Vývojáři webů by měla být obeznámeni s mnoha problémy související s úpravy šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="c0025-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="c0025-137">Jeden z největších problémů, o stylu CSS je kompatibility mezi prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c0025-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="c0025-138">Často dochází, že po použití stylů na váš web, si všimnete, že vypadá tak různé Pokud otevřete v jiném prohlížeči nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0025-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="c0025-139">Proto může trávit mnoho času na opravě těchto problémů visual si uvědomí, že pokud provedete nakonec ho fungovat v jedné prohlížeče, ho je rozděleno v jiné.</span><span class="sxs-lookup"><span data-stu-id="c0025-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="c0025-140">Visual Studio teď obsahuje funkce, které vývojáři přístup, fungovat a efektivně uspořádání šablony stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="c0025-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="c0025-141">Během tohoto cvičení bude vyhovovat nové funkce pro organizaci efektivní a edice, a také CSS3 fragmenty kódu pro různé prohlížeče kompatibility.</span><span class="sxs-lookup"><span data-stu-id="c0025-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="c0025-142">Úloha 1 – nové funkce editoru</span><span class="sxs-lookup"><span data-stu-id="c0025-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="c0025-143">V této úloze bude zjišťovat nové funkce Editor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="c0025-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="c0025-144">Tento nový editor vám pomůže zvýšit produktivitu díky nové inteligentní odsazení, komentáře vylepšené kódu a seznamu rozšířené IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c0025-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="c0025-145">Spustit **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0025-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="c0025-146">V Průzkumníku řešení otevřete **Site.css** soubor umístěný v části **styly** složky.</span><span class="sxs-lookup"><span data-stu-id="c0025-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="c0025-147">Zajistěte, aby **textového editoru** nástroje se zobrazí na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c0025-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="c0025-148">To lze provést, vyberte **zobrazení** | **panely nástrojů** nabídky a zkontrolujte **textového editoru** možnosti.</span><span class="sxs-lookup"><span data-stu-id="c0025-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="c0025-149">Si všimnete, protože tuto novou verzi **komentář** tlačítko (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) a **zrušte komentáře u** tlačítko (![zrušte komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) jsou povolená i u editor šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="c0025-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="c0025-150">![Povolení editoru a šablon stylů CSS nástroje](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "povolení editoru a šablon stylů CSS nástroje")</span><span class="sxs-lookup"><span data-stu-id="c0025-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="c0025-151">*Povolení editoru a šablon stylů CSS nástroje*</span><span class="sxs-lookup"><span data-stu-id="c0025-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="c0025-152">Posuňte zobrazení kódu a vyberte všechny definice třídy CSS.</span><span class="sxs-lookup"><span data-stu-id="c0025-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="c0025-153">Klikněte **komentář** (![komentář – tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) tlačítko komentář vybrané řádky.</span><span class="sxs-lookup"><span data-stu-id="c0025-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="c0025-154">Potom klikněte **zrušte komentáře u** (![zrušte komentář u tlačítko](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) tlačítko Zrušit změny.</span><span class="sxs-lookup"><span data-stu-id="c0025-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="c0025-155">Klikněte na tlačítko **sbalit** (![sbalit](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) a **rozbalte** (![rozbalte](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) tlačítka umístěný na levém okraji textu.</span><span class="sxs-lookup"><span data-stu-id="c0025-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="c0025-156">Všimněte si, že teď můžete skrýt styly, které nepoužívají tak, aby měl čisticí zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="c0025-157">![Sbalení tříd CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "tříd CSS sbalení")</span><span class="sxs-lookup"><span data-stu-id="c0025-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="c0025-158">*Sbalení tříd CSS*</span><span class="sxs-lookup"><span data-stu-id="c0025-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="c0025-159">Ujistěte se, že je povolena funkce Inteligentní odsazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="c0025-160">Vyberte **nástroje** | **možnosti** možnosti nabídky a potom vyberte **textového editoru** | **šablon stylů CSS**  |  **Formátování** stránky v levém podokně na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="c0025-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="c0025-161">Zkontrolujte **hierarchické odsazení** možnost.</span><span class="sxs-lookup"><span data-stu-id="c0025-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="c0025-162">![Povolení hierarchické odsazení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "povolení hierarchické odsazení")</span><span class="sxs-lookup"><span data-stu-id="c0025-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="c0025-163">*Povolení hierarchické odsazení*</span><span class="sxs-lookup"><span data-stu-id="c0025-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="c0025-164">Vyhledejte definice hlavní třídy (.main) a připojit stylu pro elementy div.</span><span class="sxs-lookup"><span data-stu-id="c0025-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="c0025-165">Si všimnete, že kód zarovnaná automaticky, pomoc uživatelům najít nadřazené třídy na první pohled.</span><span class="sxs-lookup"><span data-stu-id="c0025-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="c0025-166">CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="c0025-167">![Hierarchická zarovnání v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchické zarovnání v jazyce CSS")</span><span class="sxs-lookup"><span data-stu-id="c0025-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="c0025-168">*Hierarchická zarovnání v jazyce CSS*</span><span class="sxs-lookup"><span data-stu-id="c0025-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="c0025-169">Uvnitř **.main div** třídy, vyhledejte kurzor na konci **ohraničení: 0px;** a stiskněte klávesu **Enter** zobrazíte seznam technologie IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c0025-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="c0025-170">Začněte psát **horní** a Všimněte si filtrování seznamu během psaní.</span><span class="sxs-lookup"><span data-stu-id="c0025-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="c0025-171">V seznamu se zobrazí prvky, které obsahují **horní** v žádné části slova (v předchozích verzích sady Visual Studio, v seznamu se filtrují podle položek, *začít* s označením).</span><span class="sxs-lookup"><span data-stu-id="c0025-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="c0025-172">![Vylepšení IntelliSense v jazyce CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "rozšíření technologie IntelliSense v jazyce CSS")</span><span class="sxs-lookup"><span data-stu-id="c0025-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="c0025-173">*Vylepšení IntelliSense v jazyce CSS*</span><span class="sxs-lookup"><span data-stu-id="c0025-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="c0025-174">Úloha 2 – Výběr barvy</span><span class="sxs-lookup"><span data-stu-id="c0025-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="c0025-175">V této úloze zjistíte, že nové volby barev šablon stylů CSS je integrována do Visual Studio IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c0025-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="c0025-176">V **Site.css,** vyhledejte definice třídy hlavičky (.header) a umístěte kurzor do **barvu pozadí** atribut mezi &quot;:&quot; a &quot; # &quot; znaky na tento řádek kódu **.**</span><span class="sxs-lookup"><span data-stu-id="c0025-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="c0025-177">![Vyhledání kurzor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "vyhledání kurzor")</span><span class="sxs-lookup"><span data-stu-id="c0025-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="c0025-178">*Vyhledání kurzor*</span><span class="sxs-lookup"><span data-stu-id="c0025-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="c0025-179">Odstranit **dvojtečkou** (:) a zapisují znovu zobrazíte volby barev.</span><span class="sxs-lookup"><span data-stu-id="c0025-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="c0025-180">Všimněte si, že první barev, které se zobrazí jsou nejčastěji se vyskytující barvy vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="c0025-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="c0025-181">Když kliknete na barvu bílé, jeho kód HTML (#fff) nahradí aktuální kód barvy v šabloně stylů.</span><span class="sxs-lookup"><span data-stu-id="c0025-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="c0025-182">![Výběr barvy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "volby barev")</span><span class="sxs-lookup"><span data-stu-id="c0025-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="c0025-183">*Výběr barvy*</span><span class="sxs-lookup"><span data-stu-id="c0025-183">*Color picker*</span></span>
3. <span data-ttu-id="c0025-184">Stiskněte **rozbalte** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) na výběr barvy zobrazovat barvy, a poté přetáhněte přechodu kurzor vybrat barvu tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0025-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="c0025-185">Potom klikněte **kapátko** tlačítko a vybrat libovolnou barvu na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="c0025-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="c0025-186">Všimněte si, že hodnoty barvy pozadí se dynamicky mění při přesunutí kurzoru.</span><span class="sxs-lookup"><span data-stu-id="c0025-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="c0025-187">![Barva výběru přechodu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "přechod výběr barev")</span><span class="sxs-lookup"><span data-stu-id="c0025-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="c0025-188">*Přechod výběr barev*</span><span class="sxs-lookup"><span data-stu-id="c0025-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="c0025-189">V **krytí** posuvníku, k centru panelu ke snížení krytí přesunout modulu pro výběr.</span><span class="sxs-lookup"><span data-stu-id="c0025-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="c0025-190">Všimněte si, že hodnota barvu pozadí nyní změny jeho měřítku RGBA.</span><span class="sxs-lookup"><span data-stu-id="c0025-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="c0025-191">![Výběr barvy krytí](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "volby barev neprůhlednost.")</span><span class="sxs-lookup"><span data-stu-id="c0025-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="c0025-192">*Výběr barvy neprůhlednost.*</span><span class="sxs-lookup"><span data-stu-id="c0025-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-193">Definice barvy RGBA (červená, zelená, modrá, Alpha) ve specifikaci CSS3 umožňuje definovat hodnota neprůhlednosti barvu pro jednu položku.</span><span class="sxs-lookup"><span data-stu-id="c0025-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="c0025-194">Na rozdíl od **krytí -** podobné atribut CSS  **-**  RGBA barev je také kompatibilní s nejnovějšími prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c0025-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="c0025-195">Úloha 3 – fragmenty kódu kompatibilní šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="c0025-196">V této úloze se dozvíte, jak používat různé prohlížeče kompatibilní CSS3 fragmenty kvůli implementaci některé funkce ve vašem webu.</span><span class="sxs-lookup"><span data-stu-id="c0025-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="c0025-197">V **Site.css** souboru, vyhledejte **záhlaví** šablon stylů CSS třídy definice (.header) a umístěte kurzor níže  **/ \*ohraničení radius\* /**  zástupný symbol pro přidání nové fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="c0025-198">Stiskněte klávesu **Enter** k zobrazení seznamu IntelliSense a typ **radius** pro filtrování seznamu.</span><span class="sxs-lookup"><span data-stu-id="c0025-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="c0025-199">Vyberte **border-radius** možnost ze seznamu s klikněte a potom stiskněte klávesu **KARTĚ** klíč vložit fragment.</span><span class="sxs-lookup"><span data-stu-id="c0025-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="c0025-200">Poté zadejte velikost protokolu radius v pixelech a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c0025-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="c0025-201">Například zadejte **15px**.</span><span class="sxs-lookup"><span data-stu-id="c0025-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="c0025-202">Atributy CSS3 přidal fragmentu vykreslí zaokrouhlené ohraničení ve většině prohlížečů dodržování předpisů HTML5, včetně Mozilla a na základě WebKit prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c0025-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="c0025-203">![Pomocí fragment border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "pomocí fragment border-radius")</span><span class="sxs-lookup"><span data-stu-id="c0025-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="c0025-204">*Pomocí fragment border-radius*</span><span class="sxs-lookup"><span data-stu-id="c0025-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="c0025-205">Použít stejné **ohraničení** fragmenty kódu v styl stránky (.page).</span><span class="sxs-lookup"><span data-stu-id="c0025-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="c0025-206">CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="c0025-207">Stiskněte klávesu **F5** ke spuštění řešení.</span><span class="sxs-lookup"><span data-stu-id="c0025-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="c0025-208">Všimněte si, že každé stránce teď má zaokrouhlené ohraničení.</span><span class="sxs-lookup"><span data-stu-id="c0025-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="c0025-209">![Zaokrouhlí rozích](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaokrouhlené rozích")</span><span class="sxs-lookup"><span data-stu-id="c0025-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="c0025-210">*Zaoblenými hranami*</span><span class="sxs-lookup"><span data-stu-id="c0025-210">*Rounded corners*</span></span>
4. <span data-ttu-id="c0025-211">Zavřete prohlížeč a vraťte se k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0025-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="c0025-212">Otevřete **Custom.css** soubor umístěný v části **styly** složky a umístěte kurzor do **div.images ul li img** definici třídy.</span><span class="sxs-lookup"><span data-stu-id="c0025-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="c0025-213">Zobrazte seznam technologie IntelliSense, typ **stín pole** a stiskněte klávesu **KARTĚ** klíč dvakrát, chcete-li vložit fragment kódu stínové výchozí do definice třídy.</span><span class="sxs-lookup"><span data-stu-id="c0025-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="c0025-214">Nastavte hodnoty stínové na **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="c0025-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="c0025-215">Poté zadejte **border-radius** a Vložit fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="c0025-216">Typ **15px** nastavit velikost protokolu radius a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="c0025-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="c0025-217">![Zaokrouhlí rozích s stínové](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaokrouhlené rozích s stínové")</span><span class="sxs-lookup"><span data-stu-id="c0025-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="c0025-218">*Zaoblenými hranami s stínové*</span><span class="sxs-lookup"><span data-stu-id="c0025-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-219">V tuto chvíli je atribut stínové Vložit s odpovídající předpony (moz webkit, o) pro podporu standardu Mozilla a prohlížeče Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="c0025-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="c0025-220">Vytvořte novou třídu **div.images ul li img:hover** níže **div.images ul li img** definici třídy a umístěte kurzor do hranatých závorkách **.**</span><span class="sxs-lookup"><span data-stu-id="c0025-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="c0025-221">CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="c0025-222">Typ **transformace** a stiskněte klávesu **KARTĚ** klíč dvakrát, aby bylo možné vložit fragment transformace.</span><span class="sxs-lookup"><span data-stu-id="c0025-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="c0025-223">Potom zadejte **rotate(-15deg)** k provedení změny hodnota úhlu otočení, když se při přechodu myší bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c0025-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="c0025-224">CSS</span><span class="sxs-lookup"><span data-stu-id="c0025-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="c0025-225">Stiskněte klávesu **F5** ke spuštění řešení a přejděte na stránku CSS3.</span><span class="sxs-lookup"><span data-stu-id="c0025-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="c0025-226">Všimněte si, že bitové kopie mít zaokrouhlené rozích a pole stínů.</span><span class="sxs-lookup"><span data-stu-id="c0025-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="c0025-227">Najeďte myší obrázky a jejich otočit sledování.</span><span class="sxs-lookup"><span data-stu-id="c0025-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="c0025-228">![Transformace fragment otáčení bitovou kopii](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "transformace fragment otáčení obrázku")</span><span class="sxs-lookup"><span data-stu-id="c0025-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="c0025-229">*Transformace fragment otáčení obrázku*</span><span class="sxs-lookup"><span data-stu-id="c0025-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-230">Pokud používáte Internet Explorer 10 a neuvidí stíny, zkontrolujte, zda že dokument režim je nastaven na IE10 standardů.</span><span class="sxs-lookup"><span data-stu-id="c0025-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="c0025-231">Stiskněte klávesu **F12** otevření nástrojů pro vývojáře aplikace Internet Explorer a klikněte na tlačítko **režim dokumentu** změnit na IE10 standardů.</span><span class="sxs-lookup"><span data-stu-id="c0025-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![o-nám](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="c0025-233">Cvičení 2: Co je nového v editoru HTML</span><span class="sxs-lookup"><span data-stu-id="c0025-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="c0025-234">Visual Studio obsahuje vylepšené editor HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="c0025-235">Některé z vylepšení obsažené v této verzi jsou inteligentní odsazení v dokumentech HTML, fragmenty HTML5, HTML počáteční a koncové značky odpovídající a ověřování jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="c0025-236">Během tohoto cvičení uvidíte, jak tyto změny vylepšit vaše fluency při práci v kód webu.</span><span class="sxs-lookup"><span data-stu-id="c0025-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="c0025-237">Jako editor šablon stylů CSS je taky vylepšené HTML editor.</span><span class="sxs-lookup"><span data-stu-id="c0025-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="c0025-238">Většina tato vylepšení jsou malé ty, které usnadňují životnosti Web developer.</span><span class="sxs-lookup"><span data-stu-id="c0025-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="c0025-239">Věci jako další fragmenty kódu pro jazyk HTML5, inteligentní odsazení, když úpravy a ověření cílení dokumentu HTML DOCTYPE jsou některé z těchto vylepšení odpovídající počáteční a koncové značky.</span><span class="sxs-lookup"><span data-stu-id="c0025-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="c0025-240">Úloha 1 – ověření vylepšené DOCTYPE</span><span class="sxs-lookup"><span data-stu-id="c0025-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="c0025-241">HTML editor má teď možnost zkontrolovat DOCTYPE stránky, i když může být definici na hlavní stránce.</span><span class="sxs-lookup"><span data-stu-id="c0025-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="c0025-242">HTML editor v závislosti na DOCTYPE stránky, vyhodnotí se správnou sadou pravidel a bude filtrovat seznam technologie IntelliSense s DOCTYPE elementy.</span><span class="sxs-lookup"><span data-stu-id="c0025-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="c0025-243">V této úloze se změní DOCTYPE stránky, pokud chcete zobrazit, jak se příslušným způsobem změní chování editor HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="c0025-244">Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0025-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="c0025-245">Otevřete **Site.Master** stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="c0025-246">Všimněte si je cílové schéma pro ověření panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c0025-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="c0025-247">Způsob, jak se chová HTML editor (ověření, IntelliSense atd.) se změní správně podle Doctype vybrané.</span><span class="sxs-lookup"><span data-stu-id="c0025-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="c0025-248">![Použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")</span><span class="sxs-lookup"><span data-stu-id="c0025-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="c0025-249">*Použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*</span><span class="sxs-lookup"><span data-stu-id="c0025-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="c0025-250">Změňte schéma cílové na HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="c0025-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="c0025-251">![Změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")</span><span class="sxs-lookup"><span data-stu-id="c0025-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="c0025-252">*Změna Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*</span><span class="sxs-lookup"><span data-stu-id="c0025-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="c0025-253">Umístěte kurzor v části **textu** prvek a začněte psát název elementu HTML5 (například **video**).</span><span class="sxs-lookup"><span data-stu-id="c0025-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="c0025-254">Všimněte si, že element není k dispozici v seznamu IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c0025-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="c0025-255">![Elementy jazyka HTML5 nejsou uvedené](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elementy, které nejsou uvedené")</span><span class="sxs-lookup"><span data-stu-id="c0025-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="c0025-256">*Elementy jazyka HTML5 nejsou uvedené*</span><span class="sxs-lookup"><span data-stu-id="c0025-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="c0025-257">Vrátit zpět změny schématu cílové pro ověření nástrojů výběr DOCTYPE: XHTML5 z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="c0025-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="c0025-258">![Použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "použití Doctype v panelu nástrojů Úpravy zdrojového kódu HTML")</span><span class="sxs-lookup"><span data-stu-id="c0025-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="c0025-259">*Resetování Doctype v panelu nástrojů Úpravy zdrojového kódu HTML*</span><span class="sxs-lookup"><span data-stu-id="c0025-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="c0025-260">Umístěte kurzor v části **textu** prvek a začněte psát HTML5 element znovu (například jako **video**).</span><span class="sxs-lookup"><span data-stu-id="c0025-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="c0025-261">Všimněte si, že HTML5 elementy jsou nyní k dispozici v seznamu IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c0025-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="c0025-262">![Elementy jazyka HTML5 se uvedené](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "se uvedené prvky HTML5")</span><span class="sxs-lookup"><span data-stu-id="c0025-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="c0025-263">*Probíhá uvedené prvky HTML5*</span><span class="sxs-lookup"><span data-stu-id="c0025-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="c0025-264">Úloha 2 – počáteční nebo koncové značky automatických aktualizací</span><span class="sxs-lookup"><span data-stu-id="c0025-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="c0025-265">Visual Studio nyní aktualizuje HTML otevírání nebo při zavření značky elementu, který upravujete vzájemně odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="c0025-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="c0025-266">Tato nová funkce bude zvýšení produktivity při úpravě značky HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="c0025-267">Na **Default.aspx** přidejte **H3** element s názvem (například Visual Studio 2012 skály!).</span><span class="sxs-lookup"><span data-stu-id="c0025-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="c0025-268">Změna **H3** značky a typ **H2** nebo **H1.**</span><span class="sxs-lookup"><span data-stu-id="c0025-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="c0025-269">Všimněte si, že koncová značka automaticky aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="c0025-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="c0025-270">Můžete také upravit koncovou značku, pokud chcete zobrazit, že počáteční značky se aktualizuje příliš.</span><span class="sxs-lookup"><span data-stu-id="c0025-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="c0025-271">![Automatická aktualizace koncová značka](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatické aktualizace koncová značka")</span><span class="sxs-lookup"><span data-stu-id="c0025-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="c0025-272">*Automatická aktualizace koncová značka*</span><span class="sxs-lookup"><span data-stu-id="c0025-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="c0025-273">Úloha 3 – nové HTML5 fragmenty kódu</span><span class="sxs-lookup"><span data-stu-id="c0025-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="c0025-274">Visual Studio teď obsahuje několik fragmenty kódu jazyka HTML5.</span><span class="sxs-lookup"><span data-stu-id="c0025-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="c0025-275">V této úloze budete používat některé z těchto fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="c0025-276">Přidejte novou složku s názvem **zvuk** kořenové složky na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="c0025-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="c0025-277">Otevřete Průzkumníka Windows a zkopírujte všechny zvukové soubory do **zvuk** složky **WhatsNewASPNET.sln** řešení.</span><span class="sxs-lookup"><span data-stu-id="c0025-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="c0025-278">V **Default.aspx** stránky, vyhledejte kurzor v části webu 11 skály!!</span><span class="sxs-lookup"><span data-stu-id="c0025-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="c0025-279">Hlavička.</span><span class="sxs-lookup"><span data-stu-id="c0025-279">Header.</span></span> <span data-ttu-id="c0025-280">Typ **zvuk** a stiskněte klávesu Tabulátor.</span><span class="sxs-lookup"><span data-stu-id="c0025-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="c0025-281">Nový editor HTML zahrnuje fragmenty kódu pro obsah HTML5.</span><span class="sxs-lookup"><span data-stu-id="c0025-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="c0025-282">Nezapomeňte použít správné definice DOCTYPE povolit fragmenty HTML5.</span><span class="sxs-lookup"><span data-stu-id="c0025-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="c0025-283">![Vkládání fragmenty kódu jazyka HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "vkládání HTML5 fragmenty kódu")</span><span class="sxs-lookup"><span data-stu-id="c0025-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="c0025-284">*Vkládání HTML5 fragmenty kódu*</span><span class="sxs-lookup"><span data-stu-id="c0025-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="c0025-285">Aktualizujte zdroj zvuku tak, aby odkazoval na existující zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="c0025-285">Update the audio source to point to an existing audio file.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="c0025-286">Musíte přidat zvukový soubor k řešení.</span><span class="sxs-lookup"><span data-stu-id="c0025-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="c0025-287">Stiskněte klávesu **F5** přehrávání zvuku a spuštění tohoto webu.</span><span class="sxs-lookup"><span data-stu-id="c0025-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="c0025-288">![Spuštěním ovládacího prvku zvuk](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "systémem zvuk ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="c0025-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="c0025-289">*Spuštěním zvuk ovládacího prvku*</span><span class="sxs-lookup"><span data-stu-id="c0025-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-290">Můžete také zkusit další fragmenty zahrnuté v sadě Visual Studio, jako je například video, obrázek, atd.</span><span class="sxs-lookup"><span data-stu-id="c0025-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="c0025-291">Nyní zkuste vložení ovládacího prvku v některé části stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="c0025-292">Zkuste například vložení **GridView** ovládací prvek, ale místo zadání  **&lt;Gri,** začněte psát  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="c0025-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="c0025-293">Všimněte si, že IntelliSense seznamu jsou uvedeny **asp: GridView** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c0025-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="c0025-294">IntelliSense v editoru HTML teď poskytuje vyhledávací název-velká a malá písmena, jakož i částečné odpovídající (načítání všechny elementy, které obsahuje termín).</span><span class="sxs-lookup"><span data-stu-id="c0025-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="c0025-295">![Vkládání GridView s IntelliSense seznamy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "vkládání GridView pomocí IntelliSense seznamů")</span><span class="sxs-lookup"><span data-stu-id="c0025-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="c0025-296">*Vkládání GridView pomocí IntelliSense seznamů*</span><span class="sxs-lookup"><span data-stu-id="c0025-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="c0025-297">Pokud zadáte  **&lt;mřížky** zobrazí se všechny položky, které odpovídají termín, ale Visual Studio navrhne **gridview** ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="c0025-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="c0025-298">![Vkládání GridView s částečnou shodu a seznamy IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "vkládání GridView s částečnou shodu a seznamy IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="c0025-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="c0025-299">*Vkládání GridView s částečnou shodu a seznamy IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="c0025-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="c0025-300">Úloha 4 – HTML Editor inteligentní značky</span><span class="sxs-lookup"><span data-stu-id="c0025-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="c0025-301">Jiné zlepšování v editoru HTML je funkce inteligentních značek.</span><span class="sxs-lookup"><span data-stu-id="c0025-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="c0025-302">Inteligentní značky usnadňují vývoj běžných nebo opakovaných úloh na základě-control.</span><span class="sxs-lookup"><span data-stu-id="c0025-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="c0025-303">Tato funkce již byla k dispozici v Návrháři HTML, ale ne v editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="c0025-304">Otevřete **Site.Master** a najděte **asp: nabídky** elementu.</span><span class="sxs-lookup"><span data-stu-id="c0025-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="c0025-305">Umístěte kurzor na počáteční značce a Všimněte si, že malé glyfy zobrazí v dolní části elementu - kliknutím ho otevřete nabídce inteligentní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c0025-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="c0025-306">Všimněte si, že máte rychlý přístup k některé úlohy týkající se řízení nabídky.</span><span class="sxs-lookup"><span data-stu-id="c0025-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="c0025-307">![Inteligentní úlohy pro ovládací prvek nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentní úlohy pro ovládací prvek nabídky")</span><span class="sxs-lookup"><span data-stu-id="c0025-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="c0025-308">*Inteligentní úlohy pro ovládací prvek nabídky*</span><span class="sxs-lookup"><span data-stu-id="c0025-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="c0025-309">Úloha 5: inteligentní odsazení</span><span class="sxs-lookup"><span data-stu-id="c0025-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="c0025-310">Jedním z doporučených postupů ve formátu HTML je odsazení vnořených elementů zachovat kód čitelné.</span><span class="sxs-lookup"><span data-stu-id="c0025-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="c0025-311">V sadě Visual Studio 2012 si všimnete, že editor automaticky odsadí elementy při psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="c0025-312">V předchozí verze sady Visual Studio, byl k dispozici inteligentní odsazení v editoru XML, ale není v editoru HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="c0025-313">Ujistěte se, že konfigurace Indenting na Editor HTML je nastavena na inteligentní odsazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="c0025-314">Chcete-li to udělat, vyberte **nástroje | Možnosti** nabídce a potom vyberte **textového editoru | HTML | Karty** stránky v levém podokně na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="c0025-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="c0025-315">Vyberte možnost Inteligentní odsazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="c0025-316">![HTML Editor nastavení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "nastavení editoru HTML")</span><span class="sxs-lookup"><span data-stu-id="c0025-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="c0025-317">*Nastavení editoru HTML*</span><span class="sxs-lookup"><span data-stu-id="c0025-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="c0025-318">Na **Default.aspx** stránky, odeberte veškerý obsah v části audio element.</span><span class="sxs-lookup"><span data-stu-id="c0025-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="c0025-319">Umístěte kurzor na konci otevření **zvuk** element a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="c0025-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="c0025-320">Všimněte si, že nové pozice kurzoru má úroveň další odsazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="c0025-321">![Inteligentní odsazení v editoru HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentní odsazení v editoru HTML")</span><span class="sxs-lookup"><span data-stu-id="c0025-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="c0025-322">*Inteligentní odsazení v editoru HTML*</span><span class="sxs-lookup"><span data-stu-id="c0025-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="c0025-323">Obnovení zvuk značky s obsahem jste odstranili, nebo zavřete **Default.aspx** bez uložení změn.</span><span class="sxs-lookup"><span data-stu-id="c0025-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="c0025-324">Úloha 6 - Extract do uživatelského ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="c0025-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="c0025-325">Nástroje Refactoring zahrnuté v sadě Visual Studio, jako je například extrahování části kódu pro funkce, se skvělými funkcemi, které usnadňují zlepšení a refaktoring existující kód.</span><span class="sxs-lookup"><span data-stu-id="c0025-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="c0025-326">Protějšku pro stránky ASP.NET by extrakce kódu HTML do uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c0025-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="c0025-327">Provádění modulu ručně zahrnovat několik kroků, jako je vytvoření uživatelského ovládacího prvku nové, přesunutí části kódu do uživatelského ovládacího prvku, registrace předponu značky uživatelského ovládacího prvku a, nakonec, vytváření instancí uživatelského ovládacího prvku na stránkách.</span><span class="sxs-lookup"><span data-stu-id="c0025-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="c0025-328">Nyní, nové *extrahovat do uživatelského ovládacího prvku* nástroj automaticky provede tyto kroky pro vás.</span><span class="sxs-lookup"><span data-stu-id="c0025-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="c0025-329">V této úloze budete používat nové Extract kontextové operace uživatelský ovládací prvek vygenerujte nový uživatelský ovládací prvek z na vybraný úsek kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="c0025-330">Na **Default.aspx** vyberte **H2** a **zvuk** elementy.</span><span class="sxs-lookup"><span data-stu-id="c0025-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="c0025-331">Klikněte pravým tlačítkem a vyberte **extrahovat do uživatelského ovládacího prvku**.</span><span class="sxs-lookup"><span data-stu-id="c0025-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="c0025-332">![Extrahovat do uživatelského ovládacího prvku nabídky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extrahovat do uživatelského ovládacího prvku nabídky")</span><span class="sxs-lookup"><span data-stu-id="c0025-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="c0025-333">*Extrahovat do uživatelského ovládacího prvku nabídky*</span><span class="sxs-lookup"><span data-stu-id="c0025-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="c0025-334">Zadejte název pro nový uživatelský ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="c0025-334">Type a name for the new user control.</span></span> <span data-ttu-id="c0025-335">Například **Jukebox.ascx**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0025-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="c0025-336">![Uložení extrahovaných uživatelského ovládacího prvku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "uložení extrahovaných uživatelského ovládacího prvku")</span><span class="sxs-lookup"><span data-stu-id="c0025-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="c0025-337">*Uložení extrahovaných uživatelského ovládacího prvku*</span><span class="sxs-lookup"><span data-stu-id="c0025-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="c0025-338">Všimněte si, že jste extrahovali kód vybraný uživatelský ovládací prvek a byl nahrazen původní umístění pro vybraný úsek kódu s instancí nového uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c0025-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="c0025-339">![Stránka automaticky aktualizovaly a používají nový uživatelský ovládací prvek](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "stránky se automaticky aktualizovaly a používají nový uživatelský ovládací prvek")</span><span class="sxs-lookup"><span data-stu-id="c0025-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="c0025-340">*Stránka automaticky aktualizovaly a používají nový uživatelský ovládací prvek*</span><span class="sxs-lookup"><span data-stu-id="c0025-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="c0025-341">Stiskněte klávesu **F5** spuštění stránky a ověřte, že funguje ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c0025-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="c0025-342">Cvičení 3: Co je nového v editoru jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="c0025-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="c0025-343">Zápis nebo úpravy kódu JavaScript není jednoduchý úkol, zejména v případě, že vaše aplikace spustí zvětšují a sami najít práci s dlouhé soubory a stovky funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="c0025-344">Skript vývojáři obvykle mají nějakou práci navíc zachovat čitelnost kódu a přejděte v souborech.</span><span class="sxs-lookup"><span data-stu-id="c0025-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="c0025-345">Se zahrnutím knihovny jazyka JavaScript jako jQuery stala skriptu navigační výzvy sám sebe z důvodu délka kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="c0025-346">Visual Studio se obnovuje v editoru jazyka JavaScript s promise, aby režim kódu, dostupné a uspořádány.</span><span class="sxs-lookup"><span data-stu-id="c0025-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="c0025-347">V editoru jazyka JavaScript jsou teď implementuje řadu funkcí sady Visual Studio, které už existovalo v C# nebo VB editory: Přejít k definici, Automatické odsazení, dokumentace a ověření v případě, že jsou zápis.</span><span class="sxs-lookup"><span data-stu-id="c0025-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="c0025-348">S prodlouženou platností seznam technologie IntelliSense máte katalogu funkce JavaScript na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="c0025-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="c0025-349">V tomto cvičení se dozvíte některé z nových funkcí a vylepšení JavaScript – editor.</span><span class="sxs-lookup"><span data-stu-id="c0025-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="c0025-350">Bude procházet ukázkové soubory a zjistit všechny nové charakteristiky, které budou provedeny programování v jazyce JavaScript v rámci sady Visual Studio 2012 efektivnější.</span><span class="sxs-lookup"><span data-stu-id="c0025-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="c0025-351">Úloha 1 – nové funkce JavaScript editoru</span><span class="sxs-lookup"><span data-stu-id="c0025-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="c0025-352">Tato úloha vás seznámí s některé z nových funkcí editoru jazyka JavaScript, které se zaměřují na uspořádání kódu a přináší lepší uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0025-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="c0025-353">Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0025-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="c0025-354">Stiskněte klávesu **F5** ke spuštění aplikace, pak klikněte na odkaz JavaScript na navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="c0025-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="c0025-355">Aktualizujte stránku několik časy a kontrola jak zvýší čítače.</span><span class="sxs-lookup"><span data-stu-id="c0025-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="c0025-356">![Čítač stránky](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "čítač stránky")</span><span class="sxs-lookup"><span data-stu-id="c0025-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="c0025-357">*Čítač stránky*</span><span class="sxs-lookup"><span data-stu-id="c0025-357">*Page counter*</span></span>
3. <span data-ttu-id="c0025-358">Zavřete prohlížeč a přejděte zpátky na Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0025-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="c0025-359">Otevřete **JavaScript.aspx** stránky a najděte  **&lt;skriptu&gt;**  blok (zobrazené dole).</span><span class="sxs-lookup"><span data-stu-id="c0025-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="c0025-360">Následující kód používá místní úložiště HTML5 k uložení *pageLoadCount* proměnné, která ukládá počet, kolikrát má aktuální uživatel navštívil stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="c0025-361">Místní úložiště je databáze klienta klíč hodnota, zavedl se standardem HTML5.</span><span class="sxs-lookup"><span data-stu-id="c0025-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="c0025-362">Data budou uložena v místním počítači, do prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="c0025-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="c0025-363">Zkontrolujte, zda že deklarace DOCTYPE nastavena na XHTML5 než budete pokračovat v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="c0025-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="c0025-364">Upravit kód a Všimněte si, že obsahuje IntelliSense pro JavaScript HTML5 funkce, jako místní úložiště a jejich vnitřní metody.</span><span class="sxs-lookup"><span data-stu-id="c0025-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="c0025-365">![Funkce HTML5 JavaScript v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "funkce HTML5 JavaScript v jazyce JavaScript")</span><span class="sxs-lookup"><span data-stu-id="c0025-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="c0025-366">*Funkce HTML5 JavaScript v jazyce JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c0025-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="c0025-367">Klikněte na možnost žádné levá hranatá závorka (**{**) z skriptování kód a Všimněte si, že jsou vyznačené hranatých závorkách.</span><span class="sxs-lookup"><span data-stu-id="c0025-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="c0025-368">![Závorky jsou vyznačené](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "jsou vyznačené závorky")</span><span class="sxs-lookup"><span data-stu-id="c0025-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="c0025-369">*Jsou vyznačené závorky*</span><span class="sxs-lookup"><span data-stu-id="c0025-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="c0025-370">Zrušením komentáře u funkce **testAutoAlign()** (Vyberte tři řádky, a můžete použít **CTRL** + **tisíc**; **CTRL** + **U**) a najděte kurzor uvnitř kód funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="c0025-371">Stisknutím klávesy enter připojit druhý řádek.</span><span class="sxs-lookup"><span data-stu-id="c0025-371">Press enter to append a second line.</span></span> <span data-ttu-id="c0025-372">Všimněte si, že je kód **zarovnán** a **odsazeny automaticky**.</span><span class="sxs-lookup"><span data-stu-id="c0025-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="c0025-373">![Kód jazyka JavaScript je automaticky zarovnán](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kódu jazyka JavaScript je automaticky zarovnán")</span><span class="sxs-lookup"><span data-stu-id="c0025-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="c0025-374">*Kód jazyka JavaScript je automaticky zarovnán*</span><span class="sxs-lookup"><span data-stu-id="c0025-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="c0025-375">Úloha 2 – ověřování jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="c0025-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="c0025-376">V této úloze bude zjišťovat nové ověření JavaScript pro standardní ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="c0025-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="c0025-377">Tato funkce vám pomůže vytvořit kompatibilní kód JavaScript, při ukládání skriptování problémy před nasazením webu.</span><span class="sxs-lookup"><span data-stu-id="c0025-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="c0025-378">Visual Studio 2010 implementována ECMAStript3 dodržování předpisů, zatímco Visual Studio 2012 poskytuje ECMAScript5 dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="c0025-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="c0025-379">Otevřete **ECMA5script5.js** umístěná **Scripts\custom** složce projektu.</span><span class="sxs-lookup"><span data-stu-id="c0025-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="c0025-380">Nyní budete testovat ověřování pro ECMAScript5 standardní.</span><span class="sxs-lookup"><span data-stu-id="c0025-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="c0025-381">Můžete zkontrolovat &quot; **použití Striktního režimu** &quot; směr v první řádek souboru, který umožňuje ECMAScript5 **striktní režim**.</span><span class="sxs-lookup"><span data-stu-id="c0025-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="c0025-382">Tento režim se skládá v podmnožině jazyk, který vysvětluje z posledních edici mnohoznačnosti a přidá některé nové funkce, jako je například metod getter a setter a podpora knihovny pro JSON a podrobnější reflexe na vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="c0025-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="c0025-383">Otevřete **seznam chyb** Pokud ještě není otevřené (**zobrazení** nabídky | **Seznam chyb**).</span><span class="sxs-lookup"><span data-stu-id="c0025-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="c0025-384">Upozornění **funkce** deklarace jsou podtržené.</span><span class="sxs-lookup"><span data-stu-id="c0025-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="c0025-385">Je to proto, že v ECMA5 standardní funkce nelze vnořit do struktury jazyka.</span><span class="sxs-lookup"><span data-stu-id="c0025-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="c0025-386">V chybě bude seznamu můžete zobrazit podrobnosti upozornění.</span><span class="sxs-lookup"><span data-stu-id="c0025-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="c0025-387">![Chybovou zprávu ověření JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "chybovou zprávu ověření JavaScript")</span><span class="sxs-lookup"><span data-stu-id="c0025-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="c0025-388">*Chybovou zprávu ověření JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c0025-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="c0025-389">Komentář  **&quot;použití Striktního režimu&quot;**  směr a Všimněte si, že zmizí chyby, ale zůstal upozornění.</span><span class="sxs-lookup"><span data-stu-id="c0025-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="c0025-390">V poslední řádek souboru, zápis libovolný řetězec jako  **&quot;testování&quot;**  (včetně uvozovek to znamená, je jako řetězec).</span><span class="sxs-lookup"><span data-stu-id="c0025-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="c0025-391">Zápis období vedle řetězec k zobrazení seznamu IntelliSense a vyberte **trim** možnost.</span><span class="sxs-lookup"><span data-stu-id="c0025-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="c0025-392">Ve verzi ECMAScript5 standard řetězcové hodnoty a proměnné mají řetězcových metod, které jsou definované jako uvolnění dočasné paměti, velká písmena, vyhledávání a nahrazení.</span><span class="sxs-lookup"><span data-stu-id="c0025-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="c0025-393">![Seznam technologie IntelliSense v jazyce JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "seznam technologie IntelliSense v jazyce JavaScript")</span><span class="sxs-lookup"><span data-stu-id="c0025-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="c0025-394">*Seznam technologie IntelliSense v jazyce JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c0025-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="c0025-395">Úloha 3 – dokumentace XML pro JavaScript</span><span class="sxs-lookup"><span data-stu-id="c0025-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="c0025-396">V této úloze zaměříte funkcích nástroje Visual Studio pro dokumentace XML v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0025-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="c0025-397">Uvidíte, že v seznamu JavaScript IntelliSense zobrazí dokumentace XML každé funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="c0025-398">Kromě toho bude zjišťovat funkci navigace v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0025-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="c0025-399">Otevřete **XMLDoc.js** soubor umístěný ve **skripty nebo vlastní** složce projektu.</span><span class="sxs-lookup"><span data-stu-id="c0025-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="c0025-400">Tento soubor obsahuje dokumentace XML na každém z funkce jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0025-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="c0025-401">![Dokumentace JavaScript XML integrované technologie IntelliSense,](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "integrované dokumentace JavaScript XML IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="c0025-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="c0025-402">*Dokumentace JavaScript XML integrované technologie IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="c0025-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="c0025-403">Pod **přidat** fungovat v **XMLDoc.js** souboru, vytvořte novou funkci s názvem **testování**.</span><span class="sxs-lookup"><span data-stu-id="c0025-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="c0025-404">V **testování** fungovat, volání **násobení** funkce, která přijímá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="c0025-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="c0025-405">Všimněte si pole popisu tlačítka se zobrazuje **násobení** funkce dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c0025-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="c0025-406">![Dokumentace XML pro funkce jazyka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentace XML pro funkce jazyka JavaScript")</span><span class="sxs-lookup"><span data-stu-id="c0025-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="c0025-407">*Dokumentace XML pro funkce jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c0025-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="c0025-408">Dokončení příkazu call funkce a typ *tečkou* o otevření seznamu IntelliSense pro vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c0025-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="c0025-409">Všimněte si, že je zjišťování sady Visual Studio **vrátit** hodnotu v dokumentaci, považuje hodnotu jako číslo.</span><span class="sxs-lookup"><span data-stu-id="c0025-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="c0025-410">![Dokumentace XML pro návratové typy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentace XML pro návratové typy")</span><span class="sxs-lookup"><span data-stu-id="c0025-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="c0025-411">*Dokumentace XML pro návratové typy*</span><span class="sxs-lookup"><span data-stu-id="c0025-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="c0025-412">Nyní vložte volání pro přidání funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-412">Now, insert a call to add function.</span></span> <span data-ttu-id="c0025-413">Všimněte si, že editoru JavaScript teď podporuje přetížení funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="c0025-414">Když píšete název funkce, bude moci vyberte některé z dostupných přetížení zadané v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c0025-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="c0025-415">![Dokumentace XML pro přetížení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentace XML pro přetížení")</span><span class="sxs-lookup"><span data-stu-id="c0025-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="c0025-416">*Dokumentace XML pro přetížení*</span><span class="sxs-lookup"><span data-stu-id="c0025-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="c0025-417">Otevřete **GotoDefinition.js** souborů a vyhledejte **$().html()** volání funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="c0025-418">Vyhledejte kurzor na **html**.</span><span class="sxs-lookup"><span data-stu-id="c0025-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="c0025-419">Stiskněte klávesu **F12** a přejděte k definici.</span><span class="sxs-lookup"><span data-stu-id="c0025-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="c0025-420">Všimněte si teď můžete přístup a procházet bez použití kódu jazyka JavaScript **najít** nástroj.</span><span class="sxs-lookup"><span data-stu-id="c0025-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="c0025-421">Vyhledejte ukazatele na instrukce jQuery před blok signatury na konci souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="c0025-422">Stiskněte klávesu **F12**.</span><span class="sxs-lookup"><span data-stu-id="c0025-422">Press **F12**.</span></span> <span data-ttu-id="c0025-423">Bude přejděte na soubor knihovny jQuery.</span><span class="sxs-lookup"><span data-stu-id="c0025-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="c0025-424">Všimněte si můžete taky přejít v souborech jQuery pomocí **F12**.</span><span class="sxs-lookup"><span data-stu-id="c0025-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="c0025-425">![Navigace na definice, jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "přejdete na definice jQuery")</span><span class="sxs-lookup"><span data-stu-id="c0025-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="c0025-426">*Navigace na definice, jQuery*</span><span class="sxs-lookup"><span data-stu-id="c0025-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="c0025-427">Ujistěte se, že GotoDefinition.js neobsahuje chyby syntaxe před uložením souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="c0025-428">Cvičení 4: Sdružování a Minifikace</span><span class="sxs-lookup"><span data-stu-id="c0025-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="c0025-429">Kolikrát své weby obsahují víc než jeden JavaScript nebo šablon stylů CSS souboru?</span><span class="sxs-lookup"><span data-stu-id="c0025-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="c0025-430">Jde o velmi běžný scénář, kde sdružování a minimalizace může pomoci snížit velikost souboru a proveďte webu provádět rychleji.</span><span class="sxs-lookup"><span data-stu-id="c0025-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="c0025-431">Nová funkce instalujícího v technologii ASP.NET 4.5 sady určitou sadu souborů JS nebo šablon stylů CSS do jednoho elementu a zmenší velikost podle zmenšování obsah (tj. odebrání není požadováno mezery, odebírání komentářů, snižuje identifikátory).</span><span class="sxs-lookup"><span data-stu-id="c0025-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="c0025-432">Sdružování a minimalizace v technologii ASP.NET 4.5 se provádí v době běhu, aby proces můžete identifikovat uživatelský agent (např. aplikace Internet Explorer, Mozilla atd.) a proto komprese zlepšit cílení na uživatele prohlížeče (například odebírání vy, je Mozilla konkrétní Když požadavek pochází z Internet Exploreru).</span><span class="sxs-lookup"><span data-stu-id="c0025-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="c0025-433">V tomto cvičení se dozvíte, jak povolit a používat různé typy sdružování a minimalizace v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c0025-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="c0025-434">Úloha 1 – instalace sdružování a minimalizace balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="c0025-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="c0025-435">Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0025-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="c0025-436">Otevřete konzolu Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="c0025-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="c0025-437">Chcete-li to provést, použijte nabídku **zobrazení** | **ostatní okna** | **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c0025-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="c0025-438">![Otevírání file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole správce balíčku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otevřením konzoly Správce balíčků")</span><span class="sxs-lookup"><span data-stu-id="c0025-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="c0025-439">*Otevření konzoly Správce balíčků*</span><span class="sxs-lookup"><span data-stu-id="c0025-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="c0025-440">V **Konzola správce balíčků** typ **Install-Package Microsoft.Web.Optimization** a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="c0025-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="c0025-441">Úloha 2 – výchozí sady</span><span class="sxs-lookup"><span data-stu-id="c0025-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="c0025-442">Nejjednodušší způsob, jak používat sdružování a minimalizace je umožnit výchozí sady.</span><span class="sxs-lookup"><span data-stu-id="c0025-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="c0025-443">Tato metoda používá konvence, abyste mohli odkazovat připojené a minifikovaný verze pro JS a šablon stylů CSS soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="c0025-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="c0025-444">V této úloze se dozvíte, jak povolit odkazovat připojené a minifikovaný soubory JS a šablon stylů CSS a zobrazit výsledný výstup.</span><span class="sxs-lookup"><span data-stu-id="c0025-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="c0025-445">Pokud ještě není otevřené, spusťte **Visual Studio** a otevřete **WhatsNewASPNET.sln** řešení umístěný v **Source\WhatsNewASPNET** složky tohoto testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0025-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="c0025-446">V **Průzkumníku řešení**, rozbalte **styly**, **Scripts\custom** a **Scripts\bundle** složky.</span><span class="sxs-lookup"><span data-stu-id="c0025-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="c0025-447">Všimněte si, že aplikace používá více než jeden šablon stylů CSS a JS souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="c0025-448">![Soubory více předlohy se styly a JavaScript v aplikaci](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "soubory více předlohy se styly a JavaScript v aplikaci")</span><span class="sxs-lookup"><span data-stu-id="c0025-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="c0025-449">*Více souborů předlohy se styly a JavaScript v aplikaci*</span><span class="sxs-lookup"><span data-stu-id="c0025-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="c0025-450">Otevřete **Global.asax.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="c0025-451">Všimněte si, že nové **Microsoft.Web.Optimization** obor názvů je označeno jako komentář na začátku souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="c0025-452">Zrušením komentáře u použití direktiva k obsahují sdružování a minimalizace funkce.</span><span class="sxs-lookup"><span data-stu-id="c0025-452">Uncomment the using directive to include the bundling and minification features.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="c0025-453">Vyhledejte **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="c0025-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="c0025-454">Tato metoda zrušte komentář u volání EnableDefaultBundles, jak je znázorněno v následujícím fragmentu.</span><span class="sxs-lookup"><span data-stu-id="c0025-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="c0025-455">To umožňuje nám tak, aby odkazovaly připojené kolekce souborů CSS ve složce pomocí cesty ke složce, a &quot;šablon stylů CSS&quot; nebo &quot;JS&quot; příponu.</span><span class="sxs-lookup"><span data-stu-id="c0025-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="c0025-456">Otevřete **Optimization.aspx** souborů a vyhledejte obsahu ovládací prvek pro **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="c0025-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="c0025-457">Všimněte si, že soubory šablon stylů CSS a soubory JS mít jedinou značku odkazované.</span><span class="sxs-lookup"><span data-stu-id="c0025-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="c0025-458">Tento kód je pro účely ukázky.</span><span class="sxs-lookup"><span data-stu-id="c0025-458">This code is for demo purposes.</span></span> <span data-ttu-id="c0025-459">V ideálním případě bude odkazovat sad v souboru Site.Master.</span><span class="sxs-lookup"><span data-stu-id="c0025-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="c0025-460">V ukázkový kód zjistíte, že některé připojené soubory jsou také se na ně odkazovat soubor Site.Master provádění tohoto odkazu na poslední redundantní.</span><span class="sxs-lookup"><span data-stu-id="c0025-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="c0025-461">Všimněte si, že odkazy používají instalujícího názvů v **href** atribut k získání souborů CSS nebo JS ze styly a Scripts\custom složku v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c0025-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="c0025-462">Cestu můžete použít **skripty nebo vlastní/JS** jak je uvedeno dále sady a všechny soubory JS uvnitř minifikaci **skripty nebo vlastní** složky.</span><span class="sxs-lookup"><span data-stu-id="c0025-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="c0025-463">Toto je výchozí chování na výchozí sady.</span><span class="sxs-lookup"><span data-stu-id="c0025-463">This is the default behavior with the default bundles.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="c0025-464">Otevřete **Styles\Site.css** souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="c0025-465">Všimněte si, že původní soubor CSS obsahuje zobrazují odsazené kód, mezery a komentáře, které zvětšit soubor.</span><span class="sxs-lookup"><span data-stu-id="c0025-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="c0025-466">(Také soubor JavaScript obsahuje mezery a komentáře).</span><span class="sxs-lookup"><span data-stu-id="c0025-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="c0025-467">![Jeden z původní šablony stylů CSS soubory ve složce skripty](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jeden z původní šablony stylů CSS soubory ve složce skripty")</span><span class="sxs-lookup"><span data-stu-id="c0025-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="c0025-468">*Jeden z původní šablony stylů CSS souborů ve složce skripty*</span><span class="sxs-lookup"><span data-stu-id="c0025-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="c0025-469">Stiskněte klávesu **F5** spusťte aplikaci a přejít na **optimalizace** stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="c0025-470">Klikněte na **šablon stylů CSS sady** odkaz ke stažení a otevřete soubor.</span><span class="sxs-lookup"><span data-stu-id="c0025-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="c0025-471">Projděte si soubor minifikovaný připojené.</span><span class="sxs-lookup"><span data-stu-id="c0025-471">Check out the minified bundled file.</span></span> <span data-ttu-id="c0025-472">Si všimněte, že byly odstraněny všechny mezery, komentáře a odsazení znaky, generování menší soubor.</span><span class="sxs-lookup"><span data-stu-id="c0025-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="c0025-473">![Soubory šablon stylů CSS v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "soubory dodávat šablon stylů CSS")</span><span class="sxs-lookup"><span data-stu-id="c0025-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="c0025-474">*Připojené soubory šablon stylů CSS*</span><span class="sxs-lookup"><span data-stu-id="c0025-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="c0025-475">Klikněte na tlačítko **JS sady** odkazu k otevření souboru JavaScript dodávat.</span><span class="sxs-lookup"><span data-stu-id="c0025-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="c0025-476">Průzkumník upozornění můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="c0025-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="c0025-477">Všimněte si, soubory jazyka JavaScript v části **vlastní** složky jsou taky spojeno dohromady a minifikovaný.</span><span class="sxs-lookup"><span data-stu-id="c0025-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="c0025-478">![JavaScript soubory v sadě](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "soubory dodávat JavaScript")</span><span class="sxs-lookup"><span data-stu-id="c0025-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="c0025-479">*Připojené soubory jazyka JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c0025-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="c0025-480">Povolení komprese pro soubory šablon stylů CSS nebo JS bylo mnohem složitější v předchozí verzi technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0025-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="c0025-481">Nyní, jako jste viděli, stačí přidat jeden řádek v *Global.asax* souboru povolit sdružování a připojené soubory pak odkazovat z vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="c0025-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="c0025-482">Úloha 3 – statické sady</span><span class="sxs-lookup"><span data-stu-id="c0025-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="c0025-483">Statické sady přístupu umožňuje přizpůsobit sadu souborů sady, odkaz a minimalizaci metoda, která bude použita.</span><span class="sxs-lookup"><span data-stu-id="c0025-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="c0025-484">V této úloze nakonfigurujete sady statické definovat určitou sadu souborů sady a minifikaci.</span><span class="sxs-lookup"><span data-stu-id="c0025-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="c0025-485">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c0025-485">Close the browser.</span></span>
2. <span data-ttu-id="c0025-486">Otevřete **Global.asax.cs** souborů a vyhledejte **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="c0025-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="c0025-487">Zrušte komentář kódu statických sady, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="c0025-488">Definování statické sadu, která se bude odkazovat s &quot; **~/StaticBundle** &quot; virtuální cestou a použití **JsMinify** pro minimalizaci všechny zadané soubory s **AddFile** metoda.</span><span class="sxs-lookup"><span data-stu-id="c0025-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="c0025-489">Nakonec přidáváte statické sada, která má **BundleTable** a jeho povolení.</span><span class="sxs-lookup"><span data-stu-id="c0025-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="c0025-490">Všimněte si, že nejsou soubory umístěné na stejném místě; To je Další výhodou přes výchozí sdružování.</span><span class="sxs-lookup"><span data-stu-id="c0025-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="c0025-491">Otevřete **Optimization.aspx** souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="c0025-492">Všimněte si, že odkaz na **statické JS sady** je pomocí cesty deklarujete při konfiguraci sady statické v souboru Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="c0025-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="c0025-493">Stiskněte klávesu **F5** aplikaci spustit, a potom přejděte na **optimalizace** stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="c0025-494">Klikněte na **statické JS sady** odkazu k otevření souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="c0025-495">Všimněte si, že minifikovaný dodávat soubor JavaScript je výstup pro všechny soubory JavaScript nakonfigurované v souboru statické sady v cestě &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0025-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="c0025-496">![Statické soubory sady JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "JavaScript statické soubory sady")</span><span class="sxs-lookup"><span data-stu-id="c0025-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="c0025-497">*Statické soubory JavaScript sady*</span><span class="sxs-lookup"><span data-stu-id="c0025-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="c0025-498">Zavřete prohlížeč a vraťte se k sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0025-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="c0025-499">Úloha 4 – složka dynamické sady</span><span class="sxs-lookup"><span data-stu-id="c0025-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="c0025-500">V této úloze se dozvíte, jak nakonfigurovat dynamické složky sad.</span><span class="sxs-lookup"><span data-stu-id="c0025-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="c0025-501">Dynamické sdružování je, že můžete zahrnout statické JavaScript, stejně jako ostatní soubory jazyky, které kompilovaný do jazyka JavaScript a proto vyžadovat některé zpracování předtím, než se spustí sdružování.</span><span class="sxs-lookup"><span data-stu-id="c0025-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="c0025-502">V tomto příkladu se dozvíte, jak používat **DynamicFolderBundle** třídy za účelem vytvoření dynamického sady pro soubory, které jsou napsané v CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="c0025-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="c0025-503">CofeeScript je programovací jazyk kompilovaný do jazyka JavaScript a nabízí jednodušší syntaxi pro psaní kódu jazyka JavaScript, rozšíření jako stručný výtah a čitelnost jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0025-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="c0025-504">Otevřete **Global.asax.cs** souborů a vyhledejte **aplikace\_spustit** metoda.</span><span class="sxs-lookup"><span data-stu-id="c0025-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="c0025-505">Zrušte komentář kódu dynamické sady, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="c0025-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="c0025-506">Definování sady dynamické složky, který bude používat **CoffeeMinify** vlastní minimalizace procesor, které se vztahuje pouze na soubory s &quot; **.coffee** &quot; (rozšíření CoffeeScript soubory).</span><span class="sxs-lookup"><span data-stu-id="c0025-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="c0025-507">Všimněte si, že vzor hledání můžete vybrat soubory sady ve složce, jako je třeba se\*.coffee'.</span><span class="sxs-lookup"><span data-stu-id="c0025-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="c0025-508">Otevřete konzolu Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="c0025-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="c0025-509">Chcete-li to provést, použijte nabídku **zobrazení** | **ostatní okna** | **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c0025-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="c0025-510">V **Konzola správce balíčků** typ **Install-Package CoffeeSharp** a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="c0025-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="c0025-511">Klikněte **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** okno</span><span class="sxs-lookup"><span data-stu-id="c0025-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="c0025-512">![Zobrazení všech souborů](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "zobrazení všech souborů")</span><span class="sxs-lookup"><span data-stu-id="c0025-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="c0025-513">*Zobrazení všech souborů*</span><span class="sxs-lookup"><span data-stu-id="c0025-513">*Showing all files*</span></span>
6. <span data-ttu-id="c0025-514">Klikněte pravým tlačítkem **CoffeeMinify.cs** v soubor **Průzkumníku řešení** a vyberte **zahrnout do projektu**</span><span class="sxs-lookup"><span data-stu-id="c0025-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="c0025-515">![Zahrnout soubor CoffeeMinify.cs v projektu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "zahrnout soubor CoffeeMinify.cs v projektu")</span><span class="sxs-lookup"><span data-stu-id="c0025-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="c0025-516">*Zahrnout soubor CoffeeMinify.cs v projektu*</span><span class="sxs-lookup"><span data-stu-id="c0025-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="c0025-517">Otevřete **CoffeeMinify.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="c0025-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="c0025-518">Tato třída dědí od JsMinify k minifikaci vyplývající z CoffeeScript kompilace kódu JavaScript výstup.</span><span class="sxs-lookup"><span data-stu-id="c0025-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="c0025-519">Zavolá CoffeeScript kompilátoru nejprve generovat kód jazyka JavaScript a pak se odešle do metodu JsMinify.Process k minifikaci výsledný kód.</span><span class="sxs-lookup"><span data-stu-id="c0025-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="c0025-520">Otevřete **Script1.coffee** a **Script2.coffee** souborů z **skripty nebo sady** složky.</span><span class="sxs-lookup"><span data-stu-id="c0025-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="c0025-521">Tyto soubory bude obsahovat kód CoffeScript sestavují při provádění sdružování pomocí třídy CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="c0025-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="c0025-522">Pro účely jednoduchost jsou soubory CoffeeScript zadat pouze včetně CoffeeScript ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="c0025-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="c0025-523">Komentáře jsou vyloučeny JsMinify procesem.</span><span class="sxs-lookup"><span data-stu-id="c0025-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="c0025-524">![Soubory CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "soubory CoffeeScript")</span><span class="sxs-lookup"><span data-stu-id="c0025-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="c0025-525">*Soubory CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="c0025-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) nabízí jednodušší syntaxi pro psaní kódu jazyka JavaScript, rozšíření jako stručný výtah a čitelnost jazyce JavaScript, jakož i přidávání dalších funkcí jako pole míru porozumění a porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="c0025-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="c0025-527">Otevřete **Optimization.aspx** souboru a najděte sady odkazy.</span><span class="sxs-lookup"><span data-stu-id="c0025-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="c0025-528">Všimněte si, že odkaz na **dynamické sady JS** odkazuje **skripty nebo sady** složky pomocí **/kávy** přípon, které jste nakonfigurovali pro sadu složek dynamické.</span><span class="sxs-lookup"><span data-stu-id="c0025-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="c0025-529">Stiskněte klávesu **F5** aplikaci spustit, a potom přejděte na **optimalizace** stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="c0025-530">Klikněte na **dynamické sady JS** odkazu k otevření vygenerovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="c0025-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="c0025-531">Všimněte si, že obsahuje pouze obsah, který je zahrnutý v této sadě **.coffee** soubory.</span><span class="sxs-lookup"><span data-stu-id="c0025-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="c0025-532">Můžete také zjistí, že kód CoffeeScript byl zkompilován pro jazyk JavaScript a řádky komentované byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="c0025-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="c0025-533">![Dynamické soubory JS sady](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamické JS soubory sady")</span><span class="sxs-lookup"><span data-stu-id="c0025-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="c0025-534">*Dynamické sady soubory JS*</span><span class="sxs-lookup"><span data-stu-id="c0025-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="c0025-535">Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="c0025-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c0025-536">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c0025-536">Summary</span></span>

<span data-ttu-id="c0025-537">Toto testovací prostředí vám umožní pochopit, co je nového v technologii ASP.NET a vývoj webů v sadě Visual Studio 2012 a jak využívat řadu vylepšení v sadě Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c0025-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="c0025-538">Provedením tohoto testovacího prostředí Hands-On mít dozvědět, jak používat nové funkce a vylepšení v aplikaci Visual Studio 2012 editory pro šablon stylů CSS, JavaScript a HTML.</span><span class="sxs-lookup"><span data-stu-id="c0025-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="c0025-539">Kromě toho mají dozvědět, jak Visual Studio 2012 implementuje předdefinované sdružování a minimalizace.</span><span class="sxs-lookup"><span data-stu-id="c0025-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c0025-540">Příloha A: instalaci sady Visual Studio Express 2012 pro Web</span><span class="sxs-lookup"><span data-stu-id="c0025-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c0025-541">Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c0025-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c0025-542">Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="c0025-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c0025-543">Přejděte na [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c0025-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c0025-544">Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0025-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="c0025-545">Klikněte na **nyní nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c0025-545">Click on **Install Now**.</span></span> <span data-ttu-id="c0025-546">Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="c0025-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c0025-547">Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="c0025-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c0025-548">![Nainstalovat Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "nainstalovat Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c0025-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c0025-549">*Nainstalovat Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c0025-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c0025-550">Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="c0025-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Vyjádření souhlasu s podmínkami licence](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="c0025-552">*Vyjádření souhlasu s podmínkami licence*</span><span class="sxs-lookup"><span data-stu-id="c0025-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c0025-553">Počkejte na dokončení procesu stahování a instalaci.</span><span class="sxs-lookup"><span data-stu-id="c0025-553">Wait until the downloading and installation process completes.</span></span>

    ![Průběh instalace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="c0025-555">*Průběh instalace*</span><span class="sxs-lookup"><span data-stu-id="c0025-555">*Installation progress*</span></span>
6. <span data-ttu-id="c0025-556">Po dokončení instalace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c0025-556">When the installation completes, click **Finish**.</span></span>

    ![Instalace byla dokončena.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="c0025-558">*Instalace byla dokončena.*</span><span class="sxs-lookup"><span data-stu-id="c0025-558">*Installation completed*</span></span>
7. <span data-ttu-id="c0025-559">Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.</span><span class="sxs-lookup"><span data-stu-id="c0025-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c0025-560">Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.</span><span class="sxs-lookup"><span data-stu-id="c0025-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pro Web dlaždice](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="c0025-562">*VS Express pro Web dlaždice*</span><span class="sxs-lookup"><span data-stu-id="c0025-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c0025-563">Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="c0025-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c0025-564">Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c0025-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="c0025-565">Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0025-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="c0025-566">Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="c0025-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-567">S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu.</span><span class="sxs-lookup"><span data-stu-id="c0025-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c0025-568">Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c0025-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c0025-569">![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Přihlaste se k portálu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c0025-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c0025-570">*Přihlaste se k Windows Azure Management Portal*</span><span class="sxs-lookup"><span data-stu-id="c0025-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="c0025-571">Klikněte na tlačítko **nový** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="c0025-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c0025-572">![Vytvoření nového webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "vytváření nového webu")</span><span class="sxs-lookup"><span data-stu-id="c0025-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c0025-573">*Vytvoření nového webu*</span><span class="sxs-lookup"><span data-stu-id="c0025-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c0025-574">Klikněte na tlačítko **výpočetní** | **webu**.</span><span class="sxs-lookup"><span data-stu-id="c0025-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c0025-575">Potom vyberte **rychle vytvořit** možnost.</span><span class="sxs-lookup"><span data-stu-id="c0025-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="c0025-576">Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.</span><span class="sxs-lookup"><span data-stu-id="c0025-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-577">Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c0025-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c0025-578">Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál.</span><span class="sxs-lookup"><span data-stu-id="c0025-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="c0025-579">Postup pro nastavení databáze neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="c0025-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c0025-580">![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "vytváření nového webu pomocí metody rychlého vytvoření")</span><span class="sxs-lookup"><span data-stu-id="c0025-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c0025-581">*Vytvoření nového webu pomocí metody rychlého vytvoření*</span><span class="sxs-lookup"><span data-stu-id="c0025-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c0025-582">Počkejte, dokud nové **webu** je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="c0025-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c0025-583">Po vytvoření webu klikněte na odkaz v části **URL** sloupce.</span><span class="sxs-lookup"><span data-stu-id="c0025-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c0025-584">Zkontrolujte, zda je funkční nový web.</span><span class="sxs-lookup"><span data-stu-id="c0025-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c0025-585">![Procházení na nový web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "procházení na nový web")</span><span class="sxs-lookup"><span data-stu-id="c0025-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c0025-586">*Procházení na nový web*</span><span class="sxs-lookup"><span data-stu-id="c0025-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c0025-587">![Webový server spuštěn](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "webu systémem")</span><span class="sxs-lookup"><span data-stu-id="c0025-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="c0025-588">*Spuštění webu*</span><span class="sxs-lookup"><span data-stu-id="c0025-588">*Web site running*</span></span>
6. <span data-ttu-id="c0025-589">Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="c0025-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c0025-590">![Otevření stránky Správa webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otevření stránek správu webového serveru")</span><span class="sxs-lookup"><span data-stu-id="c0025-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c0025-591">*Otevření stránek správu webového serveru*</span><span class="sxs-lookup"><span data-stu-id="c0025-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c0025-592">V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="c0025-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-593">*Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace.</span><span class="sxs-lookup"><span data-stu-id="c0025-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="c0025-594">Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena.</span><span class="sxs-lookup"><span data-stu-id="c0025-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c0025-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c0025-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="c0025-596">![Na webu stažení profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "stahování webové stránky profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="c0025-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c0025-597">*Na webu stažení profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="c0025-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c0025-598">Stáhněte si soubor profil publikování do vhodného umístění.</span><span class="sxs-lookup"><span data-stu-id="c0025-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c0025-599">Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0025-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="c0025-600">![Ukládání souboru profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "ukládání profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="c0025-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c0025-601">*Ukládání souboru profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="c0025-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c0025-602">Úloha 2 – konfigurování serveru databáze</span><span class="sxs-lookup"><span data-stu-id="c0025-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c0025-603">Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server.</span><span class="sxs-lookup"><span data-stu-id="c0025-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c0025-604">Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.</span><span class="sxs-lookup"><span data-stu-id="c0025-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c0025-605">Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0025-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c0025-606">Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="c0025-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c0025-607">Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="c0025-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c0025-608">Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly.</span><span class="sxs-lookup"><span data-stu-id="c0025-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c0025-609">Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="c0025-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c0025-610">![Řídicí panel serveru databáze SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "řídicího panelu serveru databáze SQL")</span><span class="sxs-lookup"><span data-stu-id="c0025-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c0025-611">*Řídicí panel serveru databáze SQL*</span><span class="sxs-lookup"><span data-stu-id="c0025-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c0025-612">V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="c0025-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c0025-613">To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textových polí.</span><span class="sxs-lookup"><span data-stu-id="c0025-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="c0025-614">Zadejte název pravidla a klikněte ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0025-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Přidávání IP adresy klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="c0025-616">*Přidávání IP adresy klienta*</span><span class="sxs-lookup"><span data-stu-id="c0025-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c0025-617">Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="c0025-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potvrzení změn](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="c0025-619">*Potvrzení změn*</span><span class="sxs-lookup"><span data-stu-id="c0025-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c0025-620">Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu</span><span class="sxs-lookup"><span data-stu-id="c0025-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c0025-621">Přejděte zpět na ASP.NET MVC 4 řešení.</span><span class="sxs-lookup"><span data-stu-id="c0025-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c0025-622">V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c0025-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c0025-623">![Publikování aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikování aplikace")</span><span class="sxs-lookup"><span data-stu-id="c0025-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="c0025-624">*Publikování webu*</span><span class="sxs-lookup"><span data-stu-id="c0025-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="c0025-625">Umožňuje naimportujte profil publikování, který jste uložili v první úloze.</span><span class="sxs-lookup"><span data-stu-id="c0025-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c0025-626">![Import profilu publikování](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "import profilu publikování")</span><span class="sxs-lookup"><span data-stu-id="c0025-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c0025-627">*Import profilu publikování*</span><span class="sxs-lookup"><span data-stu-id="c0025-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="c0025-628">Klikněte na tlačítko **ověření připojení**.</span><span class="sxs-lookup"><span data-stu-id="c0025-628">Click **Validate Connection**.</span></span> <span data-ttu-id="c0025-629">Po dokončení ověření klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c0025-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0025-630">Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.</span><span class="sxs-lookup"><span data-stu-id="c0025-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c0025-631">![Ověření připojení](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "ověřování připojení")</span><span class="sxs-lookup"><span data-stu-id="c0025-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="c0025-632">*Ověření připojení*</span><span class="sxs-lookup"><span data-stu-id="c0025-632">*Validating connection*</span></span>
4. <span data-ttu-id="c0025-633">V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c0025-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c0025-634">![Konfigurace nasazení webu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "konfigurace nasazení webu")</span><span class="sxs-lookup"><span data-stu-id="c0025-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c0025-635">*Konfigurace nasazení webu*</span><span class="sxs-lookup"><span data-stu-id="c0025-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c0025-636">Připojení k databázi nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0025-636">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="c0025-637">V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.</span><span class="sxs-lookup"><span data-stu-id="c0025-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="c0025-638">V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="c0025-638">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="c0025-639">V **heslo** zadejte přihlašovací heslo správce serveru.</span><span class="sxs-lookup"><span data-stu-id="c0025-639">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="c0025-640">Zadejte nový název databáze, například: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="c0025-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="c0025-641">![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "konfigurace cílový připojovací řetězec")</span><span class="sxs-lookup"><span data-stu-id="c0025-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="c0025-642">*Konfigurace cílový připojovací řetězec*</span><span class="sxs-lookup"><span data-stu-id="c0025-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c0025-643">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0025-643">Then click **OK**.</span></span> <span data-ttu-id="c0025-644">Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c0025-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c0025-645">![Vytvoření databáze](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "vytváření řetězec databáze")</span><span class="sxs-lookup"><span data-stu-id="c0025-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="c0025-646">*Vytvoření databáze*</span><span class="sxs-lookup"><span data-stu-id="c0025-646">*Creating the database*</span></span>
7. <span data-ttu-id="c0025-647">Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="c0025-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c0025-648">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c0025-648">Then click **Next**.</span></span>

    <span data-ttu-id="c0025-649">![Připojovací řetězec odkazující na databázi SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "připojovací řetězec odkazující na databázi SQL")</span><span class="sxs-lookup"><span data-stu-id="c0025-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c0025-650">*Připojovací řetězec odkazující na databázi SQL*</span><span class="sxs-lookup"><span data-stu-id="c0025-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c0025-651">V **Preview** klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c0025-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c0025-652">![Publikování webové aplikace](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikování webové aplikace")</span><span class="sxs-lookup"><span data-stu-id="c0025-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="c0025-653">*Publikování webové aplikace*</span><span class="sxs-lookup"><span data-stu-id="c0025-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="c0025-654">Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="c0025-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="c0025-655">![Aplikace publikována do služby Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplikace publikována do služby Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c0025-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="c0025-656">*Aplikace, které jsou publikovány do služby Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="c0025-656">*Application published to Windows Azure*</span></span>
