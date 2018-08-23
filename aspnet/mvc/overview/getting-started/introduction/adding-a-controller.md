---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Přidání Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d5cc9db7b1eec139a37afb6427fd761342fcc1f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752642"
---
<a name="adding-a-controller"></a><span data-ttu-id="56b0c-102">Přidání Kontroleru</span><span class="sxs-lookup"><span data-stu-id="56b0c-102">Adding a Controller</span></span>
====================
<span data-ttu-id="56b0c-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="56b0c-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="56b0c-104">MVC jsou zahrnovaného *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="56b0c-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="56b0c-105">MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou.</span><span class="sxs-lookup"><span data-stu-id="56b0c-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="56b0c-106">Aplikace využívající architekturu MVC obsahují:</span><span class="sxs-lookup"><span data-stu-id="56b0c-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="56b0c-107">**M** odels: třídy, které představují data aplikace a logiku ověřování, který slouží k vynucení obchodní pravidla pro tato data.</span><span class="sxs-lookup"><span data-stu-id="56b0c-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="56b0c-108">**V** iews: souborů šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="56b0c-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="56b0c-109">**C** ontrollers: třídy, které zpracovávají příchozí požadavky prohlížeče, načíst datový model a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="56b0c-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="56b0c-110">Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v této řadě kurzů ukazují, jak se dají použít k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="56b0c-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="56b0c-111">V předchozím kroku MVC výchozí šablona byla vybrána.</span><span class="sxs-lookup"><span data-stu-id="56b0c-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="56b0c-112">Domů, vytvoří účet a správa Kontrolérů ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="56b0c-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="56b0c-113">Začněme tím, že vytvoříte třídu kontroleru.</span><span class="sxs-lookup"><span data-stu-id="56b0c-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="56b0c-114">V **Průzkumníka řešení**, klikněte pravým tlačítkem na *řadiče* složku a pak klikněte na tlačítko **přidat**, pak **řadič**.</span><span class="sxs-lookup"><span data-stu-id="56b0c-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="56b0c-115">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 – prázdný**a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="56b0c-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="56b0c-116">Pojmenujte nový kontroler "HelloWorldController" a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="56b0c-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Přidání kontroleru](adding-a-controller/_static/image3.png)

<span data-ttu-id="56b0c-118">Všimněte si, že v **Průzkumníka řešení** zda nový soubor byl vytvořen pojmenovaný *HelloWorldController.cs* a novou složku *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="56b0c-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="56b0c-119">Kontroler je otevřen v integrovaném vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="56b0c-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="56b0c-120">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="56b0c-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="56b0c-121">Metody kontroleru vrátí řetězec HTML jako příklad.</span><span class="sxs-lookup"><span data-stu-id="56b0c-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="56b0c-122">Název kontroleru `HelloWorldController` a první metoda se jmenuje `Index`.</span><span class="sxs-lookup"><span data-stu-id="56b0c-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="56b0c-123">Pojďme ho vyvolat z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="56b0c-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="56b0c-124">Spusťte aplikaci (stisknutím klávesy F5 nebo Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="56b0c-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="56b0c-125">V prohlížeči, připojte &quot;HelloWorld&quot; na cestu v panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="56b0c-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="56b0c-126">(Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) na stránce v prohlížeči bude vypadat jako na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="56b0c-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="56b0c-127">Ve výše uvedené metody kód vrátil řetězec přímo.</span><span class="sxs-lookup"><span data-stu-id="56b0c-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="56b0c-128">Jste uvedli jako systém právě vrátí kód HTML a udělal!</span><span class="sxs-lookup"><span data-stu-id="56b0c-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="56b0c-129">ASP.NET MVC vyvolá jiný kontroler třídy (a různé metody akcí v nich) v závislosti na příchozí adrese URL.</span><span class="sxs-lookup"><span data-stu-id="56b0c-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="56b0c-130">Výchozí adresy URL směrování logiky používá ASP.NET MVC používá k určení jaký kód, který má být vyvolán formátu tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="56b0c-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="56b0c-131">Nastavení formátu pro směrování *aplikace\_Start/RouteConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="56b0c-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="56b0c-132">Při spuštění aplikace a nechcete zadat všechny segmenty adres URL, použije se výchozí "Domovskou" kontroleru a metody akce "Index" definováno v sekci výchozí hodnoty z výše uvedený kód.</span><span class="sxs-lookup"><span data-stu-id="56b0c-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="56b0c-133">První část adresy URL určuje třída kontroleru k provedení.</span><span class="sxs-lookup"><span data-stu-id="56b0c-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="56b0c-134">Takže */HelloWorld* mapuje `HelloWorldController` třídy.</span><span class="sxs-lookup"><span data-stu-id="56b0c-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="56b0c-135">Druhá část adresy URL určí metodu akce v třídě ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="56b0c-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="56b0c-136">Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="56b0c-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="56b0c-137">Všimněte si, že jsme museli procházet */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="56b0c-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="56b0c-138">Důvodem je, že metodu s názvem `Index` představuje výchozí metodu, která bude volána na řadiči, pokud není explicitně zadaná.</span><span class="sxs-lookup"><span data-stu-id="56b0c-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="56b0c-139">Třetí část segment adresy URL ( `Parameters`) je pro data trasy.</span><span class="sxs-lookup"><span data-stu-id="56b0c-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="56b0c-140">Uvidíme data trasy, která dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="56b0c-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="56b0c-141">Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="56b0c-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="56b0c-142">`Welcome` Metoda spustí a vrátí řetězec &quot;Toto je metoda úvodní akce... &quot;.</span><span class="sxs-lookup"><span data-stu-id="56b0c-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="56b0c-143">Výchozí mapování MVC `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="56b0c-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="56b0c-144">Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda akce.</span><span class="sxs-lookup"><span data-stu-id="56b0c-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="56b0c-145">Nebyly použity `[Parameters]` část adresy URL ještě.</span><span class="sxs-lookup"><span data-stu-id="56b0c-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="56b0c-146">Pojďme mírně upravte příklad tak, aby některé informace o parametrech z adresy URL můžete předat do kontroleru (například */HelloWorld/uvítací? název = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="56b0c-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="56b0c-147">Změna vašeho `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="56b0c-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="56b0c-148">Všimněte si, že kód používá volitelný parametr funkce jazyka C# k označení, že `numTimes` parametr by ve výchozím nastavení 1-li pro tento parametr není předána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="56b0c-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="56b0c-149">Poznámka k zabezpečení: Kódu nad používá [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) k ochraně aplikace před zlými úmysly vstup (konkrétně JavaScript).</span><span class="sxs-lookup"><span data-stu-id="56b0c-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="56b0c-150">Další informace najdete v části [postupy: ochrana proti skript zneužije ve webové aplikaci pomocí použitím kódování HTML na řetězce](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="56b0c-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="56b0c-151">Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="56b0c-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="56b0c-152">Můžete vyzkoušet různé hodnoty pro `name` a `numtimes` v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="56b0c-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="56b0c-153">[Systém vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapují pojmenované parametry z řetězce dotazu do adresního řádku parametrům ve své metodě.</span><span class="sxs-lookup"><span data-stu-id="56b0c-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="56b0c-154">V ukázce výše, segment adresy URL ( `Parameters`) se nepoužívá, `name` a `numTimes` parametry jsou předány jako [řetězce dotazu](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="56b0c-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="56b0c-155">Na?</span><span class="sxs-lookup"><span data-stu-id="56b0c-155">The ?</span></span> <span data-ttu-id="56b0c-156">(otazník) v adrese URL výše je oddělovač a postupujte podle řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="56b0c-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="56b0c-157">&amp; Odděluje řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="56b0c-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="56b0c-158">Úvodní metoda nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="56b0c-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="56b0c-159">Spusťte aplikaci a zadejte následující adresu URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="56b0c-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="56b0c-160">Tentokrát třetí segment adresy URL odpovídající parametr trasa `ID.` `Welcome` metody akce obsahuje parametr (`ID`), který odpovídá specifikaci adresy URL v `RegisterRoutes` metody.</span><span class="sxs-lookup"><span data-stu-id="56b0c-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="56b0c-161">V aplikacích ASP.NET MVC je obvyklejší a zajistěte tak předání parametrů jako směrování dat (jak jsme to udělali s ID výše), než byly předány jako řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="56b0c-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="56b0c-162">Můžete také přidat trasu k předání i `name` a `numtimes` v parametrech jako data trasy v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="56b0c-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="56b0c-163">V *aplikace\_Start\RouteConfig.cs* přidejte trasy "Hello":</span><span class="sxs-lookup"><span data-stu-id="56b0c-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="56b0c-164">Spusťte aplikaci a přejděte do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="56b0c-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="56b0c-165">Výchozí trasu pro mnoho aplikací MVC funguje správně.</span><span class="sxs-lookup"><span data-stu-id="56b0c-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="56b0c-166">Dozvíte později v tomto kurzu k předávání dat pomocí vazače modelu a nebudete už muset upravit výchozí trasy pro daný.</span><span class="sxs-lookup"><span data-stu-id="56b0c-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="56b0c-167">V těchto příkladech je to kontroleru &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práce.</span><span class="sxs-lookup"><span data-stu-id="56b0c-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="56b0c-168">Kontroler přímo vrací HTML.</span><span class="sxs-lookup"><span data-stu-id="56b0c-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="56b0c-169">Obvykle nechcete řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód.</span><span class="sxs-lookup"><span data-stu-id="56b0c-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="56b0c-170">Místo toho obvykle použijeme soubor šablony samostatným zobrazením ke generování odpovědi HTML.</span><span class="sxs-lookup"><span data-stu-id="56b0c-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="56b0c-171">Pojďme se podívat na tom, jak to můžeme udělat.</span><span class="sxs-lookup"><span data-stu-id="56b0c-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="56b0c-172">[Předchozí](getting-started.md)
> [další](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="56b0c-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
