---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "Přidávání řadiče | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a><span data-ttu-id="00285-104">Přidání Kontroleru</span><span class="sxs-lookup"><span data-stu-id="00285-104">Adding a Controller</span></span>
====================
<span data-ttu-id="00285-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="00285-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="00285-106">Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="00285-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="00285-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="00285-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="00285-108">Zastupuje rozhraní MVC *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="00285-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="00285-109">MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou.</span><span class="sxs-lookup"><span data-stu-id="00285-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="00285-110">Aplikace založené na MVC obsahují:</span><span class="sxs-lookup"><span data-stu-id="00285-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="00285-111">**M** odels: třídy, které představují data, aplikace a které používají logiku ověření vynutit obchodní pravidla pro tato data.</span><span class="sxs-lookup"><span data-stu-id="00285-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="00285-112">**V** iews: soubory šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="00285-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="00285-113">**C** ontrollers: načíst data modelu třídy, které zpracovat příchozí požadavky prohlížeče a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="00285-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="00285-114">Jsme budete být pokrývajících tyto koncepty tento kurz řady a ukazují, jak je používat k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="00285-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="00285-115">Začněme vytvořením třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="00285-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="00285-116">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složku a potom vyberte **přidat kontroler**.</span><span class="sxs-lookup"><span data-stu-id="00285-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="00285-117">Pojmenujte nový kontroler &quot;HelloWorldController&quot;.</span><span class="sxs-lookup"><span data-stu-id="00285-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="00285-118">Ponechte výchozí šablony jako **kontroler MVC prázdný** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="00285-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![Přidání kontroleru](adding-a-controller/_static/image2.png)

<span data-ttu-id="00285-120">Všimněte si v **Průzkumníku řešení** aby nový soubor byla vytvořená s názvem *HelloWorldController.cs*.</span><span class="sxs-lookup"><span data-stu-id="00285-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="00285-121">Soubor je otevřen v prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="00285-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="00285-122">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="00285-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="00285-123">Metody kontroleru vrátí řetězec HTML jako příklad.</span><span class="sxs-lookup"><span data-stu-id="00285-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="00285-124">Název kontroleru `HelloWorldController` a první metoda výše jmenuje `Index`.</span><span class="sxs-lookup"><span data-stu-id="00285-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="00285-125">Budeme ji volat z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="00285-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="00285-126">Spusťte aplikaci (stiskněte F5 nebo Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="00285-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="00285-127">V prohlížeči připojit &quot;HelloWorld&quot; na cestu v panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="00285-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="00285-128">(Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) stránku v prohlížeči bude vypadat jako na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="00285-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="00285-129">V metodě výše uvedený kód vrátil řetězec přímo.</span><span class="sxs-lookup"><span data-stu-id="00285-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="00285-130">Je systém právě vrátí kód HTML v aplikaci, a to!</span><span class="sxs-lookup"><span data-stu-id="00285-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="00285-131">ASP.NET MVC volá jiné řadiče třídy (a různé metody akcí v nich), v závislosti na adresy URL příchozích.</span><span class="sxs-lookup"><span data-stu-id="00285-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="00285-132">Výchozí adresa URL směrování logiku používá ASP.NET MVC používá k určení jaký kód k vyvolání tento formát:</span><span class="sxs-lookup"><span data-stu-id="00285-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="00285-133">První část adresy URL určuje třídy kontroleru provést.</span><span class="sxs-lookup"><span data-stu-id="00285-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="00285-134">Proto */HelloWorld* se mapuje `HelloWorldController` třídy.</span><span class="sxs-lookup"><span data-stu-id="00285-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="00285-135">Druhá část adresy URL určuje metody akce v třídě provést.</span><span class="sxs-lookup"><span data-stu-id="00285-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="00285-136">Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třída spustí.</span><span class="sxs-lookup"><span data-stu-id="00285-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="00285-137">Všimněte si, že jsme museli vyhledejte */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="00285-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="00285-138">Důvodem je, že metodu s názvem `Index` je výchozí metodou, která bude volána na řadiči, pokud není explicitně určen.</span><span class="sxs-lookup"><span data-stu-id="00285-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="00285-139">Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="00285-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="00285-140">`Welcome` Metoda spustí a vrátí řetězec &quot;metodu úvodní akce... &quot;.</span><span class="sxs-lookup"><span data-stu-id="00285-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="00285-141">Výchozí mapování MVC je `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="00285-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="00285-142">Pro tuto adresu URL, že je řadič `HelloWorld` a `Welcome` je metoda akce.</span><span class="sxs-lookup"><span data-stu-id="00285-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="00285-143">Nebyly použity `[Parameters]` součástí ještě adresu URL.</span><span class="sxs-lookup"><span data-stu-id="00285-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="00285-144">Pojďme upravit v příkladu mírně tak, aby můžete předat některé informace o parametrech z adresy URL do kontroleru (například */HelloWorld/Vítá? name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="00285-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="00285-145">Změna vaší `Welcome` tak, aby zahrnoval dva parametry, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="00285-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="00285-146">Všimněte si, že kód používá volitelný parametr funkcí jazyka C# k označení, že `numTimes` parametr by měl výchozí na 1, pokud pro tento parametr není předána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="00285-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="00285-147">Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span><span class="sxs-lookup"><span data-stu-id="00285-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="00285-148">Můžete použít různé hodnoty pro `name` a `numtimes` v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="00285-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="00285-149">[Systému vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapuje pojmenované parametry z řetězce dotazu v panelu Adresa parametry ve své metodě.</span><span class="sxs-lookup"><span data-stu-id="00285-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="00285-150">V obou těchto příkladech se to řadičem &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práci.</span><span class="sxs-lookup"><span data-stu-id="00285-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="00285-151">Řadičem přímo vrací HTML.</span><span class="sxs-lookup"><span data-stu-id="00285-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="00285-152">Normálně nechcete, aby řadiče vrácení HTML přímo, vzhledem k tomu, který se stane velmi náročná kódu.</span><span class="sxs-lookup"><span data-stu-id="00285-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="00285-153">Místo toho obvykle použijeme oddělená zobrazení souboru šablony ke generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="00285-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="00285-154">Podíváme se na tom, jak jsme to lze provést další.</span><span class="sxs-lookup"><span data-stu-id="00285-154">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="00285-155">[Předchozí](intro-to-aspnet-mvc-4.md)
[další](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="00285-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
