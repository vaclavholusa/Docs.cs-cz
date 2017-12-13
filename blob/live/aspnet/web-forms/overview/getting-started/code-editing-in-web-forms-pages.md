---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: "ASP.NET – webové formuláře v sadě Visual Studio 2013 úpravy kódu | Microsoft Docs"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: dfcddb4373fbf17ca29c5ab94c6ab3387ed6b526
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="60c6a-102">Kód úpravy webových formulářů ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="60c6a-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="60c6a-103">Podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="60c6a-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="60c6a-104">V mnoha stránky webového formuláře ASP.NET můžete napsat kód v jazyce Visual Basic, C# nebo jiném jazyce.</span><span class="sxs-lookup"><span data-stu-id="60c6a-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="60c6a-105">Editor kódu v sadě Visual Studio můžete psát kód rychle a vyhnout se chybám.</span><span class="sxs-lookup"><span data-stu-id="60c6a-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="60c6a-106">Kromě toho nabízí editor způsoby, jak vytvořit opakovaně použitelný kód snížit množství práce, kterou je potřeba udělat.</span><span class="sxs-lookup"><span data-stu-id="60c6a-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="60c6a-107">Tento návod ukazuje různé funkce editoru kódu aplikace Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60c6a-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="60c6a-108">Během tohoto návodu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="60c6a-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="60c6a-109">Opravte chyby kódování vložené.</span><span class="sxs-lookup"><span data-stu-id="60c6a-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="60c6a-110">Refaktorovat a přejmenovat kód.</span><span class="sxs-lookup"><span data-stu-id="60c6a-110">Refactor and rename code.</span></span>
- <span data-ttu-id="60c6a-111">Přejmenujte proměnné a objekty.</span><span class="sxs-lookup"><span data-stu-id="60c6a-111">Rename variables and objects.</span></span>
- <span data-ttu-id="60c6a-112">Vložte fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60c6a-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60c6a-113">Prerequisites</span></span>


<span data-ttu-id="60c6a-114">K dokončení tohoto návodu, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="60c6a-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="60c6a-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) nebo [sady Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="60c6a-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).</span></span> <span data-ttu-id="60c6a-116">Rozhraní .NET Framework se instaluje automaticky.</span><span class="sxs-lookup"><span data-stu-id="60c6a-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="60c6a-117">Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 pro Web se často se označuje jako Visual Studio v rámci tohoto kurzu řady.</span><span class="sxs-lookup"><span data-stu-id="60c6a-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="60c6a-118">Pokud používáte Visual Studio, tento návod předpokládá, že jste vybrali **vývoj webů** kolekce nastavení při prvním spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60c6a-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="60c6a-119">Další informace najdete v tématu [postupy: nastavení prostředí vyberte vývoj webové](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="60c6a-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

 <span data-ttu-id="60c6a-120">Úvod do sady Visual Studio a ASP.NET, najdete v části [vytvoření základní stránky webových formulářů ASP.NET 4.5 ve Visual Studiu 2013](creating-a-basic-web-forms-page.md).</span><span class="sxs-lookup"><span data-stu-id="60c6a-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="60c6a-121">Vytvoření projektu webové aplikace a stránky</span><span class="sxs-lookup"><span data-stu-id="60c6a-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="60c6a-122">V této části průvodce vytvoříte projekt webové aplikace a do ní přidejte nová stránka.</span><span class="sxs-lookup"><span data-stu-id="60c6a-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="60c6a-123">Chcete-li vytvořit projekt webové aplikace</span><span class="sxs-lookup"><span data-stu-id="60c6a-123">To create a Web application project</span></span>

1. <span data-ttu-id="60c6a-124">Otevřete sadu Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60c6a-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="60c6a-125">Na **soubor** nabídce vyberte možnost **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="60c6a-126">![Nabídka Soubor](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="60c6a-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="60c6a-127">**Nový projekt** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60c6a-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="60c6a-128">Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** skupiny šablony na levé straně.</span><span class="sxs-lookup"><span data-stu-id="60c6a-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="60c6a-129">Vyberte **webové aplikace ASP.NET** šablony v prostředním sloupci.</span><span class="sxs-lookup"><span data-stu-id="60c6a-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="60c6a-130">Název projektu ***BasicWebApp*** a klikněte na **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60c6a-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="60c6a-131">![Dialogové okno Nový projekt](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="60c6a-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="60c6a-132">Potom vyberte **webových formulářů** šablonu a klikněte **OK** tlačítko pro vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="60c6a-133">![Dialogové okno Nový projekt ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="60c6a-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="60c6a-134">Visual Studio vytvoří nový projekt, který zahrnuje předkompilované funkce na základě šablony webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="60c6a-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="60c6a-135">Vytvoření nové stránky webového formuláře ASP.NET</span><span class="sxs-lookup"><span data-stu-id="60c6a-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="60c6a-136">Při vytváření nové aplikace webových formulářů pomocí **webové aplikace ASP.NET** šablona projektu sady Visual Studio přidá stránku ASP.NET (stránky webových formulářů) s názvem *Default.aspx*, stejně jako několik dalších souborů a složky.</span><span class="sxs-lookup"><span data-stu-id="60c6a-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="60c6a-137">Můžete použít *Default.aspx* stránky jako domovské stránky pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="60c6a-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="60c6a-138">Ale v tomto návodu vytvoříte a pracovat s novou stránku.</span><span class="sxs-lookup"><span data-stu-id="60c6a-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="60c6a-139">Přidání stránky do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="60c6a-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="60c6a-140">V **Průzkumníku řešení**, klikněte pravým tlačítkem na název webové aplikace (v tomto kurzu, název aplikace je **BasicWebSite**) a potom klikněte na **přidat**  - &gt; **Novou položku**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="60c6a-141">**Přidat novou položku** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60c6a-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="60c6a-142">Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně.</span><span class="sxs-lookup"><span data-stu-id="60c6a-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="60c6a-143">Pak vyberte **webového formuláře** ze středu seznamu a pojmenujte ji *FirstWebPage.aspx*.</span><span class="sxs-lookup"><span data-stu-id="60c6a-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="60c6a-144">![Přidat novou položku – dialogové okno](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="60c6a-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="60c6a-145">Klikněte na tlačítko **přidat** přidání stránky webových formulářů do projektu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="60c6a-146">Visual Studio vytvoří novou stránku a otevře ji.</span><span class="sxs-lookup"><span data-stu-id="60c6a-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="60c6a-147">Dále nastavte tuto novou stránku jako výchozí spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="60c6a-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="60c6a-148">V **Průzkumníku řešení**, klikněte pravým tlačítkem na novou stránku s názvem *FirstWebPage.aspx* a vyberte **nastavit jako úvodní stránku**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="60c6a-149">Při příštím spuštění této aplikace k otestování naší průběh, se automaticky zobrazí tuto novou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="60c6a-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="60c6a-150">Odstranění vložené chyby kódování</span><span class="sxs-lookup"><span data-stu-id="60c6a-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="60c6a-151">Editor kódu v sadě Visual Studio pomáhá zabránit chybám, jak napsat kód, a pokud jste provedli chybu, editoru kódu umožňuje opravte chybu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="60c6a-152">V této části návodu budete psát řádek kódu ilustrující funkce korekce chyb v editoru.</span><span class="sxs-lookup"><span data-stu-id="60c6a-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="60c6a-153">Chcete-li opravit jednoduché kódování chyby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60c6a-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="60c6a-154">V **návrhu** klikněte dvakrát na prázdnou stránku vytvořit obslužnou rutinu události pro **zatížení** událostí pro stránku.</span><span class="sxs-lookup"><span data-stu-id="60c6a-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
<span data-ttu-id="60c6a-155">Obslužné rutiny události používají pouze jako místo napsat kód.</span><span class="sxs-lookup"><span data-stu-id="60c6a-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="60c6a-156">Uvnitř obslužné rutiny, zadejte následující řádek obsahující chyby a stiskněte klávesu **ENTER**:</span><span class="sxs-lookup"><span data-stu-id="60c6a-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

 <span data-ttu-id="60c6a-157">Po stisknutí klávesy **ENTER**, editoru kódu umístí podtržení zelenou a červený (běžně volání &quot;vlnovkou&quot; řádky) v oblasti kódu, které mají problémy.</span><span class="sxs-lookup"><span data-stu-id="60c6a-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="60c6a-158">Zelená podtržení zobrazuje varování.</span><span class="sxs-lookup"><span data-stu-id="60c6a-158">A green underline indicates a warning.</span></span> <span data-ttu-id="60c6a-159">Červené podtržení označuje chybu, která je nutné opravit.</span><span class="sxs-lookup"><span data-stu-id="60c6a-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="60c6a-160">Podržte ukazatel myši nad `myStr` zobrazíte popis toho, o upozornění.</span><span class="sxs-lookup"><span data-stu-id="60c6a-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="60c6a-161">Navíc podržte ukazatel myši nad červené podtržení zobrazíte chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="60c6a-162">Následující obrázek znázorňuje kód s podtržení.</span><span class="sxs-lookup"><span data-stu-id="60c6a-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="60c6a-163">![Úvodní text v zobrazení návrhu](code-editing-in-web-forms-pages/_static/image5.png "uvítací text v zobrazení návrhu")</span><span class="sxs-lookup"><span data-stu-id="60c6a-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
 <span data-ttu-id="60c6a-164">Chyba musí být vyřešeny přidáním středníkem `;` na konec řádku.</span><span class="sxs-lookup"><span data-stu-id="60c6a-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="60c6a-165">Upozornění jednoduše vás upozorní, že jste nepoužili `myStr` ještě proměnné.</span><span class="sxs-lookup"><span data-stu-id="60c6a-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="60c6a-166">Zobrazit aktuální kódu formátování nastavení v sadě Visual Studio výběrem **nástroje**  - &gt; **možnosti**  - &gt; **písem a Barvy**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="60c6a-167">Refaktoring a přejmenování</span><span class="sxs-lookup"><span data-stu-id="60c6a-167">Refactoring and Renaming</span></span>

<span data-ttu-id="60c6a-168">Refaktoring je softwarová metodologie, která zahrnuje restrukturalizaci kódu aby bylo snazší, abyste pochopili a pokud chcete zachovat, při zachování jeho funkcí.</span><span class="sxs-lookup"><span data-stu-id="60c6a-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="60c6a-169">Jednoduchým příkladem může být psaní kódu v obslužné rutiny události se získat data z databáze.</span><span class="sxs-lookup"><span data-stu-id="60c6a-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="60c6a-170">Když budete vyvíjet stránku, zjistíte, že potřebujete přístup k datům z několika různých obslužných rutin.</span><span class="sxs-lookup"><span data-stu-id="60c6a-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="60c6a-171">Proto Refaktorovat kódu stránky vytvořením metoda přístupu k datům na stránce a vkládání volání do metody do obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="60c6a-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="60c6a-172">Editor kódu obsahuje nástroje, které můžete provádět různé úlohy refaktoringu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="60c6a-173">V tomto návodu budete pracovat se dvěma technikami refaktoringu: Přejmenování proměnné a extrahování metody.</span><span class="sxs-lookup"><span data-stu-id="60c6a-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="60c6a-174">Další možnosti refaktoringu zahrnují zapouzdření polí, převod místních proměnných na parametry metody a správě parametry metody.</span><span class="sxs-lookup"><span data-stu-id="60c6a-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="60c6a-175">Dostupnost těchto možností refaktoringu závisí na umístění v kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="60c6a-176">Refaktoring kódu</span><span class="sxs-lookup"><span data-stu-id="60c6a-176">Refactoring Code</span></span>

<span data-ttu-id="60c6a-177">Běžný scénář refaktoringu je vytvoření (extrakce) z kód, který je uvnitř jiného člena, jako je například metoda metodu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="60c6a-178">To snižuje velikost původní člena a umožňuje opakovaně použitelný kód extrahované.</span><span class="sxs-lookup"><span data-stu-id="60c6a-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="60c6a-179">V této části Průvodce napsat jednoduchý kód a potom z něj extrahujte metodu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="60c6a-180">Refaktoring je podporována pro jazyk C#, takže vytvoříte stránky, která používá c jako jeho programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="60c6a-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="60c6a-181">Extrahování metody stránce C#</span><span class="sxs-lookup"><span data-stu-id="60c6a-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="60c6a-182">Přepnout na **návrhu** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="60c6a-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="60c6a-183">V **sada nástrojů**, z **standardní** kartě, přetáhněte ji [tlačítko](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku na stránku.</span><span class="sxs-lookup"><span data-stu-id="60c6a-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="60c6a-184">Dvakrát klikněte na **tlačítko** řízení vytvořit obslužnou rutinu události pro jeho [klikněte na tlačítko](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.click.aspx) události a poté přidejte následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="60c6a-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

 <span data-ttu-id="60c6a-185">Kód vytvoří **ArrayList** používá smyčku do něj hodnoty objektu a potom pomocí další smyčky zobrazí obsah **ArrayList** objektu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="60c6a-186">Stiskněte klávesu **CTRL + F5** spuštění stránky, a pak klikněte na **tlačítko** a ujistěte se, že se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="60c6a-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="60c6a-187">Vrátit do editoru kódu a pak vyberte následující řádky v obslužné rutině.</span><span class="sxs-lookup"><span data-stu-id="60c6a-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="60c6a-188">Klikněte pravým tlačítkem na výběr, klikněte na tlačítko **Refaktorovat**a potom zvolte **extrahovat metodu**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="60c6a-189">**Extrahovat metodu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60c6a-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="60c6a-190">V **nový název metody** zadejte **DisplayArray**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="60c6a-191">Editor kódu vytvoří novou metodu s názvem `DisplayArray`a vloží volání nové metody v **klikněte na tlačítko** obslužné rutiny, kde byla původně smyčky.</span><span class="sxs-lookup"><span data-stu-id="60c6a-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="60c6a-192">Stiskněte klávesu **CTRL + F5** stránku znovu spustíte, a klikněte na **tlačítko**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="60c6a-193">Stránce funguje stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="60c6a-193">The page functions the same as it did before.</span></span> <span data-ttu-id="60c6a-194">`DisplayArray` Metoda může být nyní volání z libovolného místa v třídy stránky.</span><span class="sxs-lookup"><span data-stu-id="60c6a-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="60c6a-195">Přejmenování proměnné</span><span class="sxs-lookup"><span data-stu-id="60c6a-195">Renaming Variables</span></span>

<span data-ttu-id="60c6a-196">Při práci s proměnné a také objekty, můžete je přejmenovat, až se už neodkazuje v kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="60c6a-197">Přejmenování proměnné a objekty může způsobit kód pro přerušení, pokud zapomenete přejmenovat jeden z odkazů.</span><span class="sxs-lookup"><span data-stu-id="60c6a-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="60c6a-198">Proto můžete refaktoring pro provedení přejmenování.</span><span class="sxs-lookup"><span data-stu-id="60c6a-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="60c6a-199">Chcete-li použít refaktoring pro přejmenování proměnné</span><span class="sxs-lookup"><span data-stu-id="60c6a-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="60c6a-200">V **klikněte na tlačítko** obslužné rutiny události, vyhledejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="60c6a-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="60c6a-201">Klikněte pravým tlačítkem na název proměnné `alist`, zvolte **Refaktorovat**a potom zvolte **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="60c6a-202">**Přejmenovat** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60c6a-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="60c6a-203">V **nový název** zadejte **ArrayList1** a zajistěte, aby **zobrazení náhledu změn odkaz** vybral zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="60c6a-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="60c6a-204">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="60c6a-204">Then click **OK**.</span></span>

    <span data-ttu-id="60c6a-205">**Zobrazení náhledu změn** se zobrazí dialogové okno a zobrazí stromové struktury, která obsahuje všechny odkazy na proměnné, kterou chcete přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="60c6a-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="60c6a-206">Klikněte na tlačítko **použít** zavřete **zobrazení náhledu změn** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60c6a-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="60c6a-207">Přejmenování proměnné, které odkazují na instanci, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="60c6a-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="60c6a-208">Upozorňujeme však, který proměnnou `alist` není přejmenována na následujícím řádku.</span><span class="sxs-lookup"><span data-stu-id="60c6a-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="60c6a-209">Proměnná `alist` v tomto řádku není přejmenovat, protože nepředstavuje stejnou hodnotu jako proměnnou `alist` přejmenovaný.</span><span class="sxs-lookup"><span data-stu-id="60c6a-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="60c6a-210">Proměnná `alist` v `DisplayArray` deklarace je místní proměnné pro tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="60c6a-211">To ukazuje, že pomocí refaktoring pro přejmenování proměnné se liší od jednoduchého provedení akce hledání a nahrazování v editoru; refaktoring přejmenuje proměnné s znalostmi sémantika proměnné, která je práce.</span><span class="sxs-lookup"><span data-stu-id="60c6a-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="60c6a-212">Vkládání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="60c6a-212">Inserting Snippets</span></span>

<span data-ttu-id="60c6a-213">Protože nejsou k dispozici mnoho kódování úlohy, které vývojáři webové formuláře se často potřeba udělat, editoru kódu poskytuje knihovnu fragmentů kódu nebo bloků předepsaného kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="60c6a-214">Tyto fragmenty kódu můžete vložit do vaší stránky.</span><span class="sxs-lookup"><span data-stu-id="60c6a-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="60c6a-215">Pro každý jazyk, který používáte v sadě Visual Studio existuje drobné rozdíly ve způsobu, jakým Vložit fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="60c6a-216">Informace o vkládání fragmentů najdete v tématu [jazyka Visual Basic IntelliSense – fragmenty kódu](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="60c6a-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx).</span></span> <span data-ttu-id="60c6a-217">Informace o vkládání fragmentů kódu v jazyce Visual C# najdete v tématu [fragmenty kódu Visual C#](https://msdn.microsoft.com/en-us/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="60c6a-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/en-us/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="60c6a-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60c6a-218">Next Steps</span></span>

<span data-ttu-id="60c6a-219">Tento návod znázorňuje základní funkce editoru kódu Visual Studio 2010 pro opravu chyb v kódu, refaktoring kódu, přejmenování proměnné a vkládání fragmentů kódu do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="60c6a-220">Vývoj aplikací rychlý a snadný můžete nastavit další funkce v editoru.</span><span class="sxs-lookup"><span data-stu-id="60c6a-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="60c6a-221">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="60c6a-221">For example, you might want to:</span></span>

- <span data-ttu-id="60c6a-222">Další informace o funkcích technologie IntelliSense, jako je například úprava možnosti IntelliSense, správě fragmentů kódu a vyhledávání fragmentů kódu online.</span><span class="sxs-lookup"><span data-stu-id="60c6a-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="60c6a-223">Další informace najdete v tématu [pomocí IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b.aspx).</span><span class="sxs-lookup"><span data-stu-id="60c6a-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="60c6a-224">Naučte se vytvářet vlastní fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="60c6a-225">Další informace najdete v tématu [vytváření a používání IntelliSense – fragmenty kódu](https://msdn.microsoft.com/en-us/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="60c6a-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/en-us/library/ms165392.aspx)</span></span>
- <span data-ttu-id="60c6a-226">Další informace o funkcích specifické pro Visual Basic IntelliSense – fragmenty kódu, například přizpůsobení fragmentů kódu a řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="60c6a-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="60c6a-227">Další informace najdete v tématu [jazyka Visual Basic IntelliSense – fragmenty kódu](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="60c6a-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/en-us/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="60c6a-228">Další informace o jazyka C# – konkrétní funkce technologie IntelliSense, jako je například refaktoring a fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="60c6a-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="60c6a-229">Další informace najdete v tématu [Visual C# IntelliSense](https://msdn.microsoft.com/en-us/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="60c6a-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/en-us/library/43f44291.aspx).</span></span>
