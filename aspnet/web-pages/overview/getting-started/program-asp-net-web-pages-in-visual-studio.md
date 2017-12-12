---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "Programování ASP.NET Web Pages (Razor) pomocí sady Visual Studio | Microsoft Docs"
author: tfitzmac
description: "Tento dodatek vysvětluje, jak můžete pomocí sady Visual Studio 2010 nebo Visual Web Developer 2010 Express programu rozhraní ASP.NET Web Pages se syntaxí Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="54ee3-103">Programování webových stránek ASP.NET (Razor) pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54ee3-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="54ee3-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="54ee3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="54ee3-105">Tento článek vysvětluje, jak používat Visual Studio nebo Visual Web Developer Express programu weby technologie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="54ee3-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="54ee3-106">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="54ee3-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="54ee3-107">Co potřebujete k instalaci pro práci s webové stránky ASP.NET ve vaší verzi sady Visual Studio (Pokud se nic).</span><span class="sxs-lookup"><span data-stu-id="54ee3-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="54ee3-108">Jak přidat podporu pro ASP.NET Web Pages Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="54ee3-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="54ee3-109">Jak používat funkce v sadě Visual Studio pro práci s stránky ASP.NET Razor, včetně technologie IntelliSense a ladicí program.</span><span class="sxs-lookup"><span data-stu-id="54ee3-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="54ee3-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="54ee3-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="54ee3-111">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="54ee3-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="54ee3-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54ee3-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="54ee3-113">Služba WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="54ee3-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="54ee3-114">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 a službě WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="54ee3-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="54ee3-115">Můžete naprogramovat ASP.NET Web pages se syntaxí Razor pomocí služby WebMatrix nebo mnoha jiných editory kódu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="54ee3-116">Můžete také použít Microsoft Visual Studio, která je plně funkční integrované vývojové prostředí (IDE), které poskytuje výkonnou sadu nástrojů pro vytváření mnoho typů aplikací (nikoli pouze weby).</span><span class="sxs-lookup"><span data-stu-id="54ee3-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="54ee3-117">Chcete-li pracovat s stránky ASP.NET Razor, můžete buď pomocí jedné z úplné vydání sady Visual Studio nebo bezplatnou [Visual Studio Express pro Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span><span class="sxs-lookup"><span data-stu-id="54ee3-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="54ee3-118">Jsou dva zvláště užitečné funkce, které Visual Studio poskytuje pro programování s ASP.NET Razor webové stránky:</span><span class="sxs-lookup"><span data-stu-id="54ee3-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="54ee3-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="54ee3-119">*IntelliSense*.</span></span> <span data-ttu-id="54ee3-120">Funkci IntelliSense, která je součástí sady Visual Studio je komplexnější než IntelliSense ve službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="54ee3-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="54ee3-121">*Ladicí program*.</span><span class="sxs-lookup"><span data-stu-id="54ee3-121">*Debugger*.</span></span> <span data-ttu-id="54ee3-122">Ladicí program umožňuje odstraňovat kódu po zastavení program je spuštěn, prozkoumání proměnné a procházení řádky kódu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="54ee3-123">Pomocí sady Visual Studio s různými verzemi nástroje ASP.NET – webové stránky</span><span class="sxs-lookup"><span data-stu-id="54ee3-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="54ee3-124">Visual Studio 2012 a Visual Studio 2013 zahrnují podporu pro ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="54ee3-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="54ee3-125">(Balíčky, které jsou požadovány pro podporu rozhraní ASP.NET Web Pages jsou nainstalovány při instalaci sady Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="54ee3-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="54ee3-126">Visual Studio 2010 nezahrnuje podpora ve výchozím nastavení pro rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="54ee3-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="54ee3-127">Použít rozhraní ASP.NET Web Pages pomocí sady Visual Studio 2010, je nutné nainstalovat balíček ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="54ee3-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="54ee3-128">Získat ASP.NET Web Pages 2, nainstalujte technologii ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="54ee3-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="54ee3-129">Následující tabulka shrnuje podporu pro ASP.NET Web Pages v různých verzích sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54ee3-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="54ee3-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="54ee3-130">Visual Studio 2010</span></span> | <span data-ttu-id="54ee3-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="54ee3-131">Visual Studio 2012</span></span> | <span data-ttu-id="54ee3-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54ee3-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="54ee3-133">**Rozhraní ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="54ee3-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="54ee3-134">Instalace technologie ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="54ee3-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="54ee3-135">(Zahrnout)</span><span class="sxs-lookup"><span data-stu-id="54ee3-135">(Included)</span></span> | <span data-ttu-id="54ee3-136">(Zahrnout)</span><span class="sxs-lookup"><span data-stu-id="54ee3-136">(Included)</span></span> |
| <span data-ttu-id="54ee3-137">**Rozhraní ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="54ee3-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="54ee3-138">Aktualizace pro rozhraní ASP.NET Web Pages 3 až NuGet</span><span class="sxs-lookup"><span data-stu-id="54ee3-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="54ee3-139">(Zahrnout)</span><span class="sxs-lookup"><span data-stu-id="54ee3-139">(Included)</span></span> |

<span data-ttu-id="54ee3-140">Chcete-li pracovat s Visual Studio 2010, přečtěte si téma [instalaci podporu pro webové stránky ASP.NET v sadě Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="54ee3-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="54ee3-141">Spuštění sady Visual Studio ze služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="54ee3-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="54ee3-142">Pokud jste spustili projektu ve službě WebMatrix a chcete přepnout do sady Visual Studio, služba WebMatrix poskytuje tlačítko snadno otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54ee3-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="54ee3-143">Musíte mít nainstalovanou sadu Visual Studio v počítači pro toto tlačítko Povolit.</span><span class="sxs-lookup"><span data-stu-id="54ee3-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="54ee3-144">Následující obrázek ukazuje tlačítko ve službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="54ee3-144">The following image shows the button in WebMatrix.</span></span>

![Spusťte sadu Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="54ee3-146">Když kliknete na tlačítko, otevření projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54ee3-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="54ee3-147">Můžete přepnout přepínat mezi WebMatrix a Visual Studio bez problémů.</span><span class="sxs-lookup"><span data-stu-id="54ee3-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="54ee3-148">Pokud soubory změnily v jiném prostředí a musí být znovu k získání nejnovějších změn, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="54ee3-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="54ee3-149">Vytváření webu Razor rozhraní ASP.NET v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54ee3-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="54ee3-150">Vytvoření webu k syntaxe Razor rozhraní ASP.NET v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="54ee3-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="54ee3-151">Spusťte Visual Studio nebo Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="54ee3-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="54ee3-152">V **soubor** nabídky, klikněte na tlačítko **nový web**.</span><span class="sxs-lookup"><span data-stu-id="54ee3-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Vytvořit nový web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="54ee3-154">V **nový web** dialogovém okně vyberte jazyk, který chcete použít (Visual C# nebo Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="54ee3-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="54ee3-155">Vyberte **webové stránky ASP.NET (Razor)** šablony.</span><span class="sxs-lookup"><span data-stu-id="54ee3-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![lokality Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="54ee3-157">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="54ee3-157">Click **OK**.</span></span>

<span data-ttu-id="54ee3-158">Nový projekt existuje a je naplněna některé výchozí webové stránky můžete začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="54ee3-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="54ee3-159">Používání atributu IntelliSense</span><span class="sxs-lookup"><span data-stu-id="54ee3-159">Using IntelliSense</span></span>

<span data-ttu-id="54ee3-160">Teď, když jste vytvořili lokalitu, uvidíte, jak funguje technologie IntelliSense v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54ee3-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="54ee3-161">Na webu, kterou jste právě vytvořili, otevřete *Default.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="54ee3-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="54ee3-162">Po `<h3>` značky na stránce zadejte `@ServerInfo.` (včetně tečky).</span><span class="sxs-lookup"><span data-stu-id="54ee3-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="54ee3-163">Všimněte si, jak technologie IntelliSense zobrazí dostupné metody pro `ServerInfo` pomocné rutiny v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="54ee3-165">Vyberte `GetHtml` metoda ze seznamu a potom stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="54ee3-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="54ee3-166">IntelliSense automaticky vyplní metodu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="54ee3-167">(Jako u jakékoli metody v jazyce C#, je nutné přidat `()` znaků po metodě.)</span><span class="sxs-lookup"><span data-stu-id="54ee3-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
 <span data-ttu-id="54ee3-168">Dokončený kód `GetHtml` metoda vypadá jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="54ee3-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="54ee3-169">Stisknutím kombinace kláves Ctrl + F5 a spusťte stránky.</span><span class="sxs-lookup"><span data-stu-id="54ee3-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="54ee3-170">Je to, co bude stránka vypadat, když se zobrazí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="54ee3-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![výchozí stránku v prohlížeči](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="54ee3-172">Zavřete prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="54ee3-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="54ee3-173">Používání ladicího programu</span><span class="sxs-lookup"><span data-stu-id="54ee3-173">Using the Debugger</span></span>

1. <span data-ttu-id="54ee3-174">V horní části *Default.cshtml* stránka po řádek, který začíná `Page.Title`, přidejte následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="54ee3-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="54ee3-175">Šedé okraji editoru nalevo od kódu, klikněte na tlačítko Další tento nový řádek. Chcete-li přidat *zarážek*.</span><span class="sxs-lookup"><span data-stu-id="54ee3-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="54ee3-176">Zarážka je značku, která říká službě ladicí program na zastavení od tohoto okamžiku spuštění programu, abyste viděli, co se děje.</span><span class="sxs-lookup"><span data-stu-id="54ee3-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Sada zarážek](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="54ee3-178">Odeberte volání `ServerInfo.GetHtml` metoda a přidejte volání `@myTime` proměnné na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="54ee3-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="54ee3-179">Toto volání se zobrazí aktuální čas hodnotu, která vrátí nový řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="54ee3-180">Stisknutím klávesy F5 spusťte stránku v ladicím programu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="54ee3-181">Stránce zastaví na zarážce, kterou nastavíte.</span><span class="sxs-lookup"><span data-stu-id="54ee3-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="54ee3-182">Následující obrázek znázorňuje, co bude stránka vypadat v editoru se zarážkou (v žlutý).</span><span class="sxs-lookup"><span data-stu-id="54ee3-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![Breakpoint – ladění](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="54ee3-184">Na panelu nástrojů ladění, klikněte **Krokovat s vnořením** tlačítko (nebo stiskněte klávesu F11) ke spuštění na další řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="54ee3-185">Pokaždé, když na toto tlačítko přechodu provádění na dalším řádku kódu.</span><span class="sxs-lookup"><span data-stu-id="54ee3-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Krokovat s vnořením tlačítko](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="54ee3-187">Zkontrolujte hodnotu `myTime` proměnné tak, že podržíte ukazatel myši nad ním nebo zkontrolováním hodnoty zobrazené na **místní hodnoty –** a **zásobníkem volání** systému windows.</span><span class="sxs-lookup"><span data-stu-id="54ee3-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="54ee3-188">Visual Studio zobrazí hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="54ee3-188">Visual Studio display the value of the variable.</span></span>

    ![Hodnota času zobrazení](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="54ee3-190">Po dokončení zkoumání proměnnou a procházení kód, stiskněte klávesu F5, aby kontinuálně běžely stránce bez zastavení na každém řádku.</span><span class="sxs-lookup"><span data-stu-id="54ee3-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="54ee3-191">Pokud jste dokončili procházení všech kód, prohlížeč zobrazí stránku.</span><span class="sxs-lookup"><span data-stu-id="54ee3-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="54ee3-192">Další informace o ladicího programu a o tom, jak ladění kódu v sadě Visual Studio najdete v tématu [návod: ladění webové stránky v aplikaci Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="54ee3-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="54ee3-193">Pomocí syntaxe Razor rozhraní ASP.NET MVC projektů pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54ee3-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="54ee3-194">Také se používá hojně používá syntaxi Razor, v projekty ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="54ee3-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="54ee3-195">MVC je výkonný, na základě vzory způsob, jak vytvářet dynamické weby.</span><span class="sxs-lookup"><span data-stu-id="54ee3-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="54ee3-196">Pokud váš web ASP.NET Web Pages bude obtížné, můžete převod na aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="54ee3-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="54ee3-197">Příklad vytvoření aplikace MVC, naleznete v části [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="54ee3-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="54ee3-198">Instalace podpory pro webové stránky ASP.NET v sadě Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="54ee3-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="54ee3-199">V této části ukazuje, jak nainstalovat Visual Web Developer Express 2010 a nástroje webové stránky ASP.NET pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54ee3-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="54ee3-200">Pokud ještě nemáte služby instalace webové platformy, můžete ji stáhněte z následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="54ee3-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [<span data-ttu-id="54ee3-201">https://www.microsoft.com/web/downloads/Platform.aspx</span><span class="sxs-lookup"><span data-stu-id="54ee3-201">https://www.microsoft.com/web/downloads/platform.aspx</span></span>](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="54ee3-202">Spuštění instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="54ee3-202">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="54ee3-203">Klikněte **produkty** kartě.</span><span class="sxs-lookup"><span data-stu-id="54ee3-203">Click the **Products** tab.</span></span>

    ![Karta produkty WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="54ee3-205">Vyhledejte **ASP.NET MVC 4** (pro ASP.NET Web Pages 2) a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="54ee3-205">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="54ee3-206">Zahrnout tyto produkty Visual Studio tools pro vytváření webů ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="54ee3-206">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Možnosti instalace WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="54ee3-208">Klikněte na tlačítko **nainstalovat** k dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="54ee3-208">Click **Install** to complete the installation.</span></span>
