---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Přidání Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 100f5623bafa33548f9c979e98765c92327eb369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373286"
---
<a name="adding-a-controller"></a><span data-ttu-id="fe115-104">Přidání Kontroleru</span><span class="sxs-lookup"><span data-stu-id="fe115-104">Adding a Controller</span></span>
====================
<span data-ttu-id="fe115-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="fe115-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="fe115-106">Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="fe115-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="fe115-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="fe115-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="fe115-108">MVC jsou zahrnovaného *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="fe115-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="fe115-109">MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou.</span><span class="sxs-lookup"><span data-stu-id="fe115-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="fe115-110">Aplikace využívající architekturu MVC obsahují:</span><span class="sxs-lookup"><span data-stu-id="fe115-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="fe115-111">**M** odels: třídy, které představují data aplikace a logiku ověřování, který slouží k vynucení obchodní pravidla pro tato data.</span><span class="sxs-lookup"><span data-stu-id="fe115-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="fe115-112">**V** iews: souborů šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="fe115-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="fe115-113">**C** ontrollers: třídy, které zpracovávají příchozí požadavky prohlížeče, načíst datový model a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fe115-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="fe115-114">Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v této řadě kurzů ukazují, jak se dají použít k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe115-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="fe115-115">Začněme tím, že vytvoříte třídu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="fe115-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="fe115-116">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *řadiče* složku a pak vyberte **přidat kontroler**.</span><span class="sxs-lookup"><span data-stu-id="fe115-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="fe115-117">Pojmenujte nový kontroler &quot;HelloWorldController&quot;.</span><span class="sxs-lookup"><span data-stu-id="fe115-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="fe115-118">Ponechte výchozí šablony jako **prázdný kontroler MVC** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="fe115-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![Přidání kontroleru](adding-a-controller/_static/image2.png)

<span data-ttu-id="fe115-120">Všimněte si, že v **Průzkumníka řešení** zda nový soubor byl vytvořen pojmenovaný *HelloWorldController.cs*.</span><span class="sxs-lookup"><span data-stu-id="fe115-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="fe115-121">Soubor je otevřen v integrovaném vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="fe115-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="fe115-122">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="fe115-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="fe115-123">Metody kontroleru vrátí řetězec HTML jako příklad.</span><span class="sxs-lookup"><span data-stu-id="fe115-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="fe115-124">Název kontroleru `HelloWorldController` a se jmenuje první výše uvedené metody `Index`.</span><span class="sxs-lookup"><span data-stu-id="fe115-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="fe115-125">Pojďme ho vyvolat z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="fe115-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="fe115-126">Spusťte aplikaci (stisknutím klávesy F5 nebo Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="fe115-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="fe115-127">V prohlížeči, připojte &quot;HelloWorld&quot; na cestu v panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="fe115-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="fe115-128">(Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) na stránce v prohlížeči bude vypadat jako na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="fe115-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="fe115-129">Ve výše uvedené metody kód vrátil řetězec přímo.</span><span class="sxs-lookup"><span data-stu-id="fe115-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="fe115-130">Jste uvedli jako systém právě vrátí kód HTML a udělal!</span><span class="sxs-lookup"><span data-stu-id="fe115-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="fe115-131">ASP.NET MVC vyvolá jiný kontroler třídy (a různé metody akcí v nich) v závislosti na příchozí adrese URL.</span><span class="sxs-lookup"><span data-stu-id="fe115-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="fe115-132">Výchozí adresy URL směrování logiky používá ASP.NET MVC používá k určení jaký kód, který má být vyvolán formátu tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="fe115-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="fe115-133">První část adresy URL určuje třída kontroleru k provedení.</span><span class="sxs-lookup"><span data-stu-id="fe115-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="fe115-134">Takže */HelloWorld* mapuje `HelloWorldController` třídy.</span><span class="sxs-lookup"><span data-stu-id="fe115-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="fe115-135">Druhá část adresy URL určí metodu akce v třídě ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="fe115-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="fe115-136">Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="fe115-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="fe115-137">Všimněte si, že jsme museli procházet */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fe115-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="fe115-138">Důvodem je, že metodu s názvem `Index` představuje výchozí metodu, která bude volána na řadiči, pokud není explicitně zadaná.</span><span class="sxs-lookup"><span data-stu-id="fe115-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="fe115-139">Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="fe115-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="fe115-140">`Welcome` Metoda spustí a vrátí řetězec &quot;Toto je metoda úvodní akce... &quot;.</span><span class="sxs-lookup"><span data-stu-id="fe115-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="fe115-141">Výchozí mapování MVC `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="fe115-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="fe115-142">Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda akce.</span><span class="sxs-lookup"><span data-stu-id="fe115-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="fe115-143">Nebyly použity `[Parameters]` část adresy URL ještě.</span><span class="sxs-lookup"><span data-stu-id="fe115-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="fe115-144">Pojďme mírně upravte příklad tak, aby některé informace o parametrech z adresy URL můžete předat do kontroleru (například */HelloWorld/uvítací? název = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="fe115-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="fe115-145">Změna vašeho `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="fe115-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="fe115-146">Všimněte si, že kód používá volitelný parametr funkce jazyka C# k označení, že `numTimes` parametr by ve výchozím nastavení 1-li pro tento parametr není předána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="fe115-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="fe115-147">Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span><span class="sxs-lookup"><span data-stu-id="fe115-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="fe115-148">Můžete vyzkoušet různé hodnoty pro `name` a `numtimes` v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="fe115-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="fe115-149">[Systém vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapují pojmenované parametry z řetězce dotazu do adresního řádku parametrům ve své metodě.</span><span class="sxs-lookup"><span data-stu-id="fe115-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="fe115-150">V obou těchto příkladech je to kontroleru &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práce.</span><span class="sxs-lookup"><span data-stu-id="fe115-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="fe115-151">Kontroler přímo vrací HTML.</span><span class="sxs-lookup"><span data-stu-id="fe115-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="fe115-152">Obvykle nechcete řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód.</span><span class="sxs-lookup"><span data-stu-id="fe115-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="fe115-153">Místo toho obvykle použijeme soubor šablony samostatným zobrazením ke generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="fe115-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="fe115-154">Pojďme se podívat na tom, jak to můžeme udělat.</span><span class="sxs-lookup"><span data-stu-id="fe115-154">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fe115-155">[Předchozí](intro-to-aspnet-mvc-4.md)
> [další](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="fe115-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
