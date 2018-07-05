---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Úprava kódu webových formulářů ASP.NET v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 322a9aa6fb366eb9d5be33838fd6ac7ea51047f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371791"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="248f6-102">Kód úpravy webových formulářů ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="248f6-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="248f6-103">podle [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="248f6-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="248f6-104">V mnoha stránkách webový formulář ASP.NET napíšete kód v jazyce Visual Basic, C# nebo jiném jazyce.</span><span class="sxs-lookup"><span data-stu-id="248f6-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="248f6-105">Editor kódu v sadě Visual Studio vám umožňují psát kód rychle a pomáhá předejít chybám.</span><span class="sxs-lookup"><span data-stu-id="248f6-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="248f6-106">Kromě toho nabízí editor způsoby, jak vytvořit opakovaně použitelný kód snížit množství práce, kterou je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="248f6-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="248f6-107">Tento návod ukazuje různé funkce editoru kódu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="248f6-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="248f6-108">V tomto návodu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="248f6-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="248f6-109">Opravte chyby kódování vložené.</span><span class="sxs-lookup"><span data-stu-id="248f6-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="248f6-110">Refaktorovat a přejmenovat kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-110">Refactor and rename code.</span></span>
- <span data-ttu-id="248f6-111">Přejmenujte proměnných a objektech.</span><span class="sxs-lookup"><span data-stu-id="248f6-111">Rename variables and objects.</span></span>
- <span data-ttu-id="248f6-112">Vložte fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="248f6-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="248f6-113">Prerequisites</span></span>


<span data-ttu-id="248f6-114">K dokončení tohoto návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="248f6-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="248f6-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="248f6-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="248f6-116">Rozhraní .NET Framework se instaluje automaticky.</span><span class="sxs-lookup"><span data-stu-id="248f6-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="248f6-117">Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 for Web bude často se označuje jako Visual Studio v celé této sérii kurzů.</span><span class="sxs-lookup"><span data-stu-id="248f6-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="248f6-118">Pokud používáte Visual Studio, Tento názorný průvodce předpokládá, že jste vybrali **vývoj pro Web** kolekce nastavení při prvním spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="248f6-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="248f6-119">Další informace najdete v tématu [postupy: Výběr nastavení prostředí vývoje webu](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="248f6-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

  <span data-ttu-id="248f6-120">Úvod do sady Visual Studio a technologie ASP.NET, naleznete v tématu [vytvoření stránky základních webových formulářů ASP.NET 4.5 v sadě Visual Studio 2013](creating-a-basic-web-forms-page.md).</span><span class="sxs-lookup"><span data-stu-id="248f6-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="248f6-121">Vytvoření projektu webové aplikace a na stránce</span><span class="sxs-lookup"><span data-stu-id="248f6-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="248f6-122">V této části tohoto návodu vytvoříte projekt webové aplikace a přidejte novou stránku do ní.</span><span class="sxs-lookup"><span data-stu-id="248f6-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="248f6-123">Chcete-li vytvořit projekt webové aplikace</span><span class="sxs-lookup"><span data-stu-id="248f6-123">To create a Web application project</span></span>

1. <span data-ttu-id="248f6-124">Otevřete Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="248f6-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="248f6-125">Na **souboru** nabídce vyberte možnost **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="248f6-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="248f6-126">![Nabídka Soubor](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="248f6-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="248f6-127">**Nový projekt** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="248f6-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="248f6-128">Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** šablony skupiny na levé straně.</span><span class="sxs-lookup"><span data-stu-id="248f6-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="248f6-129">Zvolte **webová aplikace ASP.NET** šablon v prostředním sloupci.</span><span class="sxs-lookup"><span data-stu-id="248f6-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="248f6-130">Pojmenujte svůj projekt ***BasicWebApp*** a klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="248f6-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="248f6-131">![Dialogové okno Nový projekt](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="248f6-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="248f6-132">V dalším kroku vyberte **webových formulářů** šablony a kliknutím **OK** tlačítko pro vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="248f6-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="248f6-133">![Dialogové okno Nový projekt ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="248f6-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="248f6-134">Visual Studio vytvoří nový projekt, který obsahuje předem připravených funkce na základě šablony webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="248f6-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="248f6-135">Vytváří se nové technologie ASP.NET webové stránky s formuláři</span><span class="sxs-lookup"><span data-stu-id="248f6-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="248f6-136">Při vytváření nové aplikace webových formulářů pomocí **webová aplikace ASP.NET** šablony projektu, Visual Studio přidá stránky ASP.NET (webové formuláře – stránka) s názvem *Default.aspx*, stejně jako několik dalších souborů a složky.</span><span class="sxs-lookup"><span data-stu-id="248f6-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="248f6-137">Můžete použít *Default.aspx* stránku jako domovské stránky pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="248f6-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="248f6-138">Ale v tomto návodu vytvoříte a pracovat s novou stránku.</span><span class="sxs-lookup"><span data-stu-id="248f6-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="248f6-139">Přidání stránky do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="248f6-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="248f6-140">V **Průzkumníka řešení**, klikněte pravým tlačítkem na název webové aplikace (v tomto kurzu je název aplikace **BasicWebSite**) a potom klikněte na tlačítko **přidat**  - &gt; **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="248f6-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="248f6-141">**Přidat novou položku** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="248f6-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="248f6-142">Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně.</span><span class="sxs-lookup"><span data-stu-id="248f6-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="248f6-143">Vyberte **webový formulář** uprostřed seznamu a pojmenujte ho *FirstWebPage.aspx*.</span><span class="sxs-lookup"><span data-stu-id="248f6-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="248f6-144">![Přidat novou položku – dialogové okno](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="248f6-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="248f6-145">Klikněte na tlačítko **přidat** přidat na stránku webové formuláře do projektu.</span><span class="sxs-lookup"><span data-stu-id="248f6-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="248f6-146">Visual Studio vytvoří novou stránku a otevře jej.</span><span class="sxs-lookup"><span data-stu-id="248f6-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="248f6-147">Dále nastavte tuto novou stránku jako výchozí úvodní stránka.</span><span class="sxs-lookup"><span data-stu-id="248f6-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="248f6-148">V **Průzkumníka řešení**, klikněte pravým tlačítkem na novou stránku s názvem *FirstWebPage.aspx* a vyberte **nastavit jako úvodní stránku**.</span><span class="sxs-lookup"><span data-stu-id="248f6-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="248f6-149">Při příštím spuštění této aplikace pro testování náš postup, se automaticky zobrazí tuto novou stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="248f6-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="248f6-150">Oprava chyby kódování vložené</span><span class="sxs-lookup"><span data-stu-id="248f6-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="248f6-151">Editor kódu v sadě Visual Studio vám umožní zabránit chybám při psaní kódu, a pokud jste chybu, editor kódu umožňuje k opravě chyby.</span><span class="sxs-lookup"><span data-stu-id="248f6-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="248f6-152">V této části tohoto návodu budete psát jediného řádku kódu, které ilustrují funkce opravy chyb v editoru.</span><span class="sxs-lookup"><span data-stu-id="248f6-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="248f6-153">Chcete-li opravit chyby jednoduché psaní kódu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="248f6-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="248f6-154">V **návrhu** dvakrát klikněte na prázdnou stránku vytvořit obslužnou rutinu události pro **zatížení** události pro danou stránku.</span><span class="sxs-lookup"><span data-stu-id="248f6-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="248f6-155">Obslužná rutina události používáte pouze jako bod kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="248f6-156">Uvnitř obslužné rutiny, zadejte následující řádek, který obsahuje chybu a stisknutím klávesy **ENTER**:</span><span class="sxs-lookup"><span data-stu-id="248f6-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="248f6-157">Když stisknete klávesu **ENTER**, editor kódu umístí červenou a zelenou vlnovkou (obvykle volání &quot;podtržení&quot; řádky) v rámci oblasti kódu, které mají problémy.</span><span class="sxs-lookup"><span data-stu-id="248f6-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="248f6-158">Zelená podtržení zobrazuje varování.</span><span class="sxs-lookup"><span data-stu-id="248f6-158">A green underline indicates a warning.</span></span> <span data-ttu-id="248f6-159">Červené podtržení označuje chybu, která je potřeba opravit.</span><span class="sxs-lookup"><span data-stu-id="248f6-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="248f6-160">Podržte ukazatel myši nad `myStr` zobrazíte popisek, který vás informuje o upozornění.</span><span class="sxs-lookup"><span data-stu-id="248f6-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="248f6-161">Také podržte ukazatel myši nad červenou vlnovkou, chcete-li zobrazit chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="248f6-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="248f6-162">Následující obrázek ukazuje kód podtržení.</span><span class="sxs-lookup"><span data-stu-id="248f6-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="248f6-163">![Úvodní text v návrhovém zobrazení](code-editing-in-web-forms-pages/_static/image5.png "uvítací text v návrhovém zobrazení")</span><span class="sxs-lookup"><span data-stu-id="248f6-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="248f6-164">Chyba je opravit přidáním středníkem `;` na konec řádku.</span><span class="sxs-lookup"><span data-stu-id="248f6-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="248f6-165">Upozornění jednoduše vás upozorní, že jste nepoužili `myStr` ještě proměnné.</span><span class="sxs-lookup"><span data-stu-id="248f6-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="248f6-166">Zobrazení aktuálního kódu formátování nastavení v sadě Visual Studio tak, že vyberete **nástroje**  - &gt; **možnosti**  - &gt; **písma a Barvy**.</span><span class="sxs-lookup"><span data-stu-id="248f6-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="248f6-167">Refaktoring a přejmenování</span><span class="sxs-lookup"><span data-stu-id="248f6-167">Refactoring and Renaming</span></span>

<span data-ttu-id="248f6-168">Refaktoring je metodologie softwaru, která zahrnuje restrukturalizaci váš kód, aby bylo snazší porozumět a chcete zachovat, při zachování její funkčnost.</span><span class="sxs-lookup"><span data-stu-id="248f6-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="248f6-169">Jednoduchým příkladem může být, že napíšete kód v obslužné rutině události k získání dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="248f6-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="248f6-170">Při vývoji vaší stránce, zjistíte, že potřebujete přístup k datům z několika různých obslužných rutin.</span><span class="sxs-lookup"><span data-stu-id="248f6-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="248f6-171">Proto se Refaktorovat kód na stránce tak, že vytvoření metody přístupu k datům stránky a vložením volání metody v obslužných rutinách.</span><span class="sxs-lookup"><span data-stu-id="248f6-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="248f6-172">Editor kódu obsahuje nástroje, které můžete provádět různé úlohy refaktoringu.</span><span class="sxs-lookup"><span data-stu-id="248f6-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="248f6-173">V tomto návodu budete pracovat se dvěma refaktoringu technikami: přejmenováním proměnné a extrahování metod.</span><span class="sxs-lookup"><span data-stu-id="248f6-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="248f6-174">Další možnosti refaktorování zahrnují zapouzdření polí, zvyšuje se úroveň lokálních proměnných na parametry metod a Správa parametrů metody.</span><span class="sxs-lookup"><span data-stu-id="248f6-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="248f6-175">K dispozici tyto možnosti refaktorování, závisí na umístění v kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="248f6-176">Refaktoring kódu</span><span class="sxs-lookup"><span data-stu-id="248f6-176">Refactoring Code</span></span>

<span data-ttu-id="248f6-177">Je běžným scénářem refaktoringu vytvoření (extract) metodu z kódu, který se nachází uvnitř jiného člena, jako je například metody.</span><span class="sxs-lookup"><span data-stu-id="248f6-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="248f6-178">To snižuje velikost původního člena a díky opakovaně použitelného kódu byl extrahován.</span><span class="sxs-lookup"><span data-stu-id="248f6-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="248f6-179">V této části Průvodce jednoduchého kódu a pak z něj extrahovat metodu.</span><span class="sxs-lookup"><span data-stu-id="248f6-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="248f6-180">Refaktoring je podporována pro jazyk C#, takže vytvoříte stránky, která používá C# jako programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="248f6-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="248f6-181">Extrahovat metodu na stránce jazyka C#</span><span class="sxs-lookup"><span data-stu-id="248f6-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="248f6-182">Přepnout na **návrhu** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="248f6-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="248f6-183">V **nástrojů**, z **standardní** kartu tak, že přetáhnete [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku na stránku.</span><span class="sxs-lookup"><span data-stu-id="248f6-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="248f6-184">Dvakrát klikněte **tlačítko** ovládacího prvku k vytvoření obslužné rutiny pro jeho [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) události a poté přidejte následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="248f6-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="248f6-185">Kód vytvoří **ArrayList** objektu, používá smyčku k načtení hodnoty a pak používá další smyčky k zobrazení obsahu **ArrayList** objektu.</span><span class="sxs-lookup"><span data-stu-id="248f6-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="248f6-186">Stisknutím klávesy **CTRL + F5** spuštění stránky a pak klikněte na tlačítko **tlačítko** abyste měli jistotu, že se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="248f6-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="248f6-187">Vraťte se do editoru kódu a pak vyberte následující řádky v obslužné rutině události.</span><span class="sxs-lookup"><span data-stu-id="248f6-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="248f6-188">Klikněte pravým tlačítkem na výběr, klikněte na tlačítko **Refaktorovat**a klikněte na tlačítko **extrahovat metodu**.</span><span class="sxs-lookup"><span data-stu-id="248f6-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="248f6-189">**Extrahovat metodu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="248f6-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="248f6-190">V **nový název metody** zadejte **DisplayArray**a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="248f6-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="248f6-191">Editor kódu vytvoří novou metodu s názvem `DisplayArray`a vloží do nové metody ve volání **klikněte na tlačítko** obslužnou rutinu, ve kterém bylo původně určené smyčky.</span><span class="sxs-lookup"><span data-stu-id="248f6-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="248f6-192">Stisknutím klávesy **CTRL + F5** stránce spustit znovu, a klikněte na tlačítko **tlačítko**.</span><span class="sxs-lookup"><span data-stu-id="248f6-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="248f6-193">Na stránce funguje stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="248f6-193">The page functions the same as it did before.</span></span> <span data-ttu-id="248f6-194">`DisplayArray` Metoda může být nyní volání z libovolného místa ve třídě stránky.</span><span class="sxs-lookup"><span data-stu-id="248f6-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="248f6-195">Přejmenováním proměnné</span><span class="sxs-lookup"><span data-stu-id="248f6-195">Renaming Variables</span></span>

<span data-ttu-id="248f6-196">Při práci s proměnných a také objekty, můžete chtít poté, co je již odkazováno v kódu je přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="248f6-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="248f6-197">Přejmenování proměnných a objektech však může způsobit kód pro přerušení promeškali přejmenování jeden z odkazů.</span><span class="sxs-lookup"><span data-stu-id="248f6-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="248f6-198">Proto vám pomůže refaktoring přejmenování provést.</span><span class="sxs-lookup"><span data-stu-id="248f6-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="248f6-199">Chcete-li použít refaktoring pro přejmenování proměnné</span><span class="sxs-lookup"><span data-stu-id="248f6-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="248f6-200">V **klikněte na tlačítko** obslužná rutina události, vyhledejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="248f6-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="248f6-201">Klikněte pravým tlačítkem na název proměnné `alist`, zvolte **Refaktorovat**a klikněte na tlačítko **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="248f6-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="248f6-202">**Přejmenovat** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="248f6-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="248f6-203">V **nový název** zadejte **ArrayList1** a ujistěte se, že **náhled změn odkazu** vybral zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="248f6-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="248f6-204">Pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="248f6-204">Then click **OK**.</span></span>

    <span data-ttu-id="248f6-205">**Náhled změn** dialogové okno se zobrazí a zobrazí strom, který obsahuje všechny odkazy na proměnné, kterou chcete přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="248f6-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="248f6-206">Klikněte na tlačítko **použít** zavřete **náhled změn** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="248f6-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="248f6-207">Proměnné, které se odkazují na instance, kterou jste vybrali, budou přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="248f6-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="248f6-208">Mějte na paměti, ale který proměnnou `alist` není přejmenována na následujícím řádku.</span><span class="sxs-lookup"><span data-stu-id="248f6-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="248f6-209">Proměnná `alist` v tomto řádku není přejmenovat, protože nepředstavuje stejnou hodnotu jako proměnnou `alist` přejmenovaný.</span><span class="sxs-lookup"><span data-stu-id="248f6-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="248f6-210">Proměnná `alist` v `DisplayArray` deklarace je lokální proměnná pro tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="248f6-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="248f6-211">To ukazuje, že pomocí refaktoring přejmenování proměnné se liší od jednoduše provedením akce najít a nahradit v editoru; Refaktoring přejmenování proměnné se znalostí sémantiku proměnné, která funguje s.</span><span class="sxs-lookup"><span data-stu-id="248f6-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="248f6-212">Vkládání fragmentů kódu</span><span class="sxs-lookup"><span data-stu-id="248f6-212">Inserting Snippets</span></span>

<span data-ttu-id="248f6-213">Vzhledem k tomu, že existují mnoho úkolů kódování, které vývojáři webové formuláře se často potřeba provést, editor kódu poskytuje knihovnu fragmenty kódu nebo bloky předepsaného kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="248f6-214">Tyto fragmenty kódu můžete vložit do stránky.</span><span class="sxs-lookup"><span data-stu-id="248f6-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="248f6-215">Každý jazyk, který použijete v sadě Visual Studio má mírné rozdíly tak, jak vložit fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="248f6-216">Informace o vkládání fragmentů kódu, naleznete v tématu [fragmenty kódu technologie IntelliSense jazyka Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="248f6-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="248f6-217">Informace o vkládání fragmentů kódu v jazyce Visual C# najdete v tématu [fragmenty kódu Visual C#](https://msdn.microsoft.com/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="248f6-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="248f6-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="248f6-218">Next Steps</span></span>

<span data-ttu-id="248f6-219">Tento návod znázorňuje základní funkce editoru kódu sady Visual Studio 2010 pro opravu chyb v kódu, refaktoring kódu, přejmenováním proměnné a vkládání fragmentů kódu do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="248f6-220">Další funkce v editoru můžete vytvořit vývoj aplikací rychlý a snadný.</span><span class="sxs-lookup"><span data-stu-id="248f6-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="248f6-221">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="248f6-221">For example, you might want to:</span></span>

- <span data-ttu-id="248f6-222">Další informace o funkcích technologie IntelliSense, jako je například úprava možnosti technologie IntelliSense, Správa fragmentů kódu a fragmenty kódu online hledání.</span><span class="sxs-lookup"><span data-stu-id="248f6-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="248f6-223">Další informace najdete v tématu [pomocí technologie IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span><span class="sxs-lookup"><span data-stu-id="248f6-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="248f6-224">Zjistěte, jak vytvořit své vlastní fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="248f6-225">Další informace najdete v tématu [vytváření a používání fragmenty kódu technologie IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="248f6-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="248f6-226">Další informace o funkcích specifické pro jazyk Visual Basic fragmenty kódu technologie IntelliSense, jako je například přizpůsobení fragmenty kódu a řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="248f6-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="248f6-227">Další informace najdete v tématu [fragmenty kódu technologie IntelliSense jazyka Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="248f6-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="248f6-228">Další informace o jazyce C#-konkrétní funkce technologie IntelliSense, jako je Refaktoring a fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="248f6-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="248f6-229">Další informace najdete v tématu [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="248f6-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
