---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: "Použití ViewData a implementace třídy ViewModel | Microsoft Docs"
author: microsoft
description: "Krok 6 ukazuje jak povolit podporu pro širší formulář Úpravy scénářů a taky popisuje dva přístupy, které slouží k předávání dat z řadičů pro zobrazení:..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a><span data-ttu-id="1f719-103">Použití ViewData a implementujte ViewModel třídy</span><span class="sxs-lookup"><span data-stu-id="1f719-103">Use ViewData and Implement ViewModel Classes</span></span>
====================
<span data-ttu-id="1f719-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1f719-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1f719-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="1f719-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="1f719-106">Toto je krok 6 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="1f719-106">This is step 6 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="1f719-107">Krok 6 ukazuje jak povolit podporu pro širší formulář Úpravy scénářů a taky popisuje dva přístupy, které slouží k předávání dat z řadičů pro zobrazení: ViewData a ViewModel.</span><span class="sxs-lookup"><span data-stu-id="1f719-107">Step 6 shows how enable support for richer form editing scenarios, and also discusses two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>
> 
> <span data-ttu-id="1f719-108">Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="1f719-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a><span data-ttu-id="1f719-109">NerdDinner krok 6: ViewData a ViewModel</span><span class="sxs-lookup"><span data-stu-id="1f719-109">NerdDinner Step 6: ViewData and ViewModel</span></span>

<span data-ttu-id="1f719-110">Jsme zahrnutých počet scénáře vystavení formuláře a popsané postupy pro implementaci vytvářet, aktualizovat a odstraňovat podporu (CRUD).</span><span class="sxs-lookup"><span data-stu-id="1f719-110">We've covered a number of form post scenarios, and discussed how to implement create, update and delete (CRUD) support.</span></span> <span data-ttu-id="1f719-111">Nyní jsme budete provádět naše implementace DinnersController další a povolit podporu pro širší formulář Úpravy scénářů.</span><span class="sxs-lookup"><span data-stu-id="1f719-111">We'll now take our DinnersController implementation further and enable support for richer form editing scenarios.</span></span> <span data-ttu-id="1f719-112">Při takovém probereme dva přístupy, které slouží k předávání dat z řadičů pro zobrazení: ViewData a ViewModel.</span><span class="sxs-lookup"><span data-stu-id="1f719-112">While doing this we'll discuss two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>

### <a name="passing-data-from-controllers-to-view-templates"></a><span data-ttu-id="1f719-113">Předávání dat z řadiče zobrazení šablon</span><span class="sxs-lookup"><span data-stu-id="1f719-113">Passing Data from Controllers to View-Templates</span></span>

<span data-ttu-id="1f719-114">Jednou z definující vlastností vzoru MVC je striktní "oddělení obavy" pomáhá vynutit mezi různými součástmi aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f719-114">One of the defining characteristics of the MVC pattern is the strict "separation of concerns" it helps enforce between the different components of an application.</span></span> <span data-ttu-id="1f719-115">Modely, Kontrolery a zobrazení jednotlivých mít dobře definované rolích a zodpovědnostech a komunikaci mezi vzájemně dobře definované způsoby.</span><span class="sxs-lookup"><span data-stu-id="1f719-115">Models, Controllers and Views each have well defined roles and responsibilities, and they communicate amongst each other in well defined ways.</span></span> <span data-ttu-id="1f719-116">To pomáhá zvýšit úroveň testovatelnosti a opakované použití kódu.</span><span class="sxs-lookup"><span data-stu-id="1f719-116">This helps promote testability and code reuse.</span></span>

<span data-ttu-id="1f719-117">Pokud se rozhodne třídy Kontroleru k vykreslení odpovědi HTML zpět do klienta, je zodpovědná za explicitně předávání do zobrazení šablony všechna data potřebná pro vykreslení odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1f719-117">When a Controller class decides to render an HTML response back to a client, it is responsible for explicitly passing to the view template all of the data needed to render the response.</span></span> <span data-ttu-id="1f719-118">Zobrazení šablony nikdy proveďte veškeré data načtení nebo aplikaci logiky – a místo toho měli omezit sami mít pouze vykreslování kódu, které vycházejí z modelu nebo data předaný řadičem.</span><span class="sxs-lookup"><span data-stu-id="1f719-118">View templates should never perform any data retrieval or application logic – and should instead limit themselves to only have rendering code that is driven off of the model/data passed to it by the controller.</span></span>

<span data-ttu-id="1f719-119">Nyní data modelu předávány podle našich DinnersController třída našich šablon zobrazení je jednoduchý a jednoduché – seznam objektů večeři v případě Index() a jeden večeři objektu v případě Details(), Edit(), Create() a Delete().</span><span class="sxs-lookup"><span data-stu-id="1f719-119">Right now the model data being passed by our DinnersController class to our view templates is simple and straight-forward – a list of Dinner objects in the case of Index(), and a single Dinner object in the case of Details(), Edit(), Create() and Delete().</span></span> <span data-ttu-id="1f719-120">Jak jsme naše aplikace přidat další možnosti uživatelského rozhraní, často budeme potřebovat předat více než jen tato data k vykreslení odpovědi HTML v našich šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f719-120">As we add more UI capabilities to our application, we are often going to need to pass more than just this data to render HTML responses within our view templates.</span></span> <span data-ttu-id="1f719-121">Můžeme například chtít změňte pole "Země" v rámci naší upravit a vytvořit zobrazení nebudou jazyka HTML textové pole k rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="1f719-121">For example, we might want to change the "Country" field within our Edit and Create views from being an HTML textbox to a dropdownlist.</span></span> <span data-ttu-id="1f719-122">Místo pevný kódu rozevíracího seznamu názvů země v šabloně zobrazení jsme chtít vygenerovat ze seznamu podporovaných zemích, které jsme naplnit dynamicky.</span><span class="sxs-lookup"><span data-stu-id="1f719-122">Rather than hard-code the dropdown list of country names in the view template, we might want to generate it from a list of supported countries that we populate dynamically.</span></span> <span data-ttu-id="1f719-123">Je nutné zadat způsob, jak předat objekt večeři *a* seznam podporovaných zemích z našich řadiče našich šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f719-123">We will need a way to pass both the Dinner object *and* the list of supported countries from our controller to our view templates.</span></span>

<span data-ttu-id="1f719-124">Podívejme se na jsme se dá dosáhnout dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="1f719-124">Let's look at two ways we can accomplish this.</span></span>

### <a name="using-the-viewdata-dictionary"></a><span data-ttu-id="1f719-125">Pomocí slovníku ViewData</span><span class="sxs-lookup"><span data-stu-id="1f719-125">Using the ViewData Dictionary</span></span>

<span data-ttu-id="1f719-126">Základní třídy Kontroleru zpřístupňuje vlastnost slovníku "ViewData", která umožňuje předat další datové položky z řadiče zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f719-126">The Controller base class exposes a "ViewData" dictionary property that can be used to pass additional data items from Controllers to Views.</span></span>

<span data-ttu-id="1f719-127">Například pro podporu tohoto scénáře, kde chceme změnit textového pole "Země" v rámci naší upravit zobrazení nebudou jazyka HTML textové pole k rozevírací seznam, budeme aktualizovat naše metoda akce Edit() předat (kromě objekt večeři) SelectList objekt, který lze použít jako m odelu rozevírací seznam zemích.</span><span class="sxs-lookup"><span data-stu-id="1f719-127">For example, to support the scenario where we want to change the "Country" textbox within our Edit view from being an HTML textbox to a dropdownlist, we can update our Edit() action method to pass (in addition to a Dinner object) a SelectList object that can be used as the model of a countries dropdownlist.</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

<span data-ttu-id="1f719-128">V konstruktoru SelectList výše přijímá seznam okresech k naplnění rozevíracího downlist s, jakož i aktuálně vybrané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1f719-128">The constructor of the SelectList above is accepting a list of counties to populate the drop-downlist with, as well as the currently selected value.</span></span>

<span data-ttu-id="1f719-129">Potom jsme můžete aktualizovat naše Edit.aspx zobrazit šablonu pro použití pomocnou metodu Html.DropDownList() místo Html.TextBox() pomocnou metodu, kterou jsme použili dříve:</span><span class="sxs-lookup"><span data-stu-id="1f719-129">We can then update our Edit.aspx view template to use the Html.DropDownList() helper method instead of the Html.TextBox() helper method we used previously:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

<span data-ttu-id="1f719-130">Pomocná metoda Html.DropDownList() výše přebírá dva parametry.</span><span class="sxs-lookup"><span data-stu-id="1f719-130">The Html.DropDownList() helper method above takes two parameters.</span></span> <span data-ttu-id="1f719-131">První je název prvku formuláře HTML na výstup.</span><span class="sxs-lookup"><span data-stu-id="1f719-131">The first is the name of the HTML form element to output.</span></span> <span data-ttu-id="1f719-132">Druhá je model "SelectList", které jsme předány prostřednictvím ViewData slovníku.</span><span class="sxs-lookup"><span data-stu-id="1f719-132">The second is the "SelectList" model we passed via the ViewData dictionary.</span></span> <span data-ttu-id="1f719-133">Používáme jazyka C# – "klíčové slovo jako" přetypovat type v rámci slovníku jako SelectList.</span><span class="sxs-lookup"><span data-stu-id="1f719-133">We are using the C# "as" keyword to cast the type within the dictionary as a SelectList.</span></span>

<span data-ttu-id="1f719-134">A teď když jsme spustit naše aplikace a přístup */Dinners/Edit/1* adresy URL v rámci našeho prohlížeče uvidíme, že zobrazíte rozevírací seznam zemí, místo do textového pole. byl aktualizován naše upravit uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="1f719-134">And now when we run our application and access the */Dinners/Edit/1* URL within our browser we'll see that our edit UI has been updated to display a dropdownlist of countries instead of a textbox:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

<span data-ttu-id="1f719-135">Protože jsme také vykreslení zobrazení šablony upravit metodu HTTP POST upravit (ve scénářích při výskytu chyb), jsme budete chtít zajistit, jsme také aktualizovat tuto metodu za účelem přidání SelectList do ViewData při zobrazení šablony v chybě scénáře:</span><span class="sxs-lookup"><span data-stu-id="1f719-135">Because we also render the Edit view template from the HTTP-POST Edit method (in scenarios when errors occur), we'll want to make sure that we also update this method to add the SelectList to ViewData when the view template is rendered in error scenarios:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

<span data-ttu-id="1f719-136">A teď podporuje náš scénář upravit DinnersController rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="1f719-136">And now our DinnersController edit scenario supports a DropDownList.</span></span>

### <a name="using-a-viewmodel-pattern"></a><span data-ttu-id="1f719-137">Pomocí vzoru ViewModel</span><span class="sxs-lookup"><span data-stu-id="1f719-137">Using a ViewModel Pattern</span></span>

<span data-ttu-id="1f719-138">Přístup slovník ViewData má výhodu probíhá poměrně rychle a snadno implementovat.</span><span class="sxs-lookup"><span data-stu-id="1f719-138">The ViewData dictionary approach has the benefit of being fairly fast and easy to implement.</span></span> <span data-ttu-id="1f719-139">Někteří vývojáři nelíbí pomocí řetězcového slovníky, ale vzhledem k tomu, že překlepům může způsobit chyby, které nebudou zachyceny v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="1f719-139">Some developers don't like using string-based dictionaries, though, since typos can lead to errors that will not be caught at compile-time.</span></span> <span data-ttu-id="1f719-140">Beztypové slovníku ViewData také vyžaduje pomocí operátoru "jako" nebo přetypování při použití silného typu jazyk, jako je C#, v zobrazení šablony.</span><span class="sxs-lookup"><span data-stu-id="1f719-140">The un-typed ViewData dictionary also requires using the "as" operator or casting when using a strongly-typed language like C# in a view template.</span></span>

<span data-ttu-id="1f719-141">Alternativní způsob, který by mohl používáme je jedním často označuje jako "ViewModel" vzor.</span><span class="sxs-lookup"><span data-stu-id="1f719-141">An alternative approach that we could use is one often referred to as the "ViewModel" pattern.</span></span> <span data-ttu-id="1f719-142">Při použití tohoto vzoru vytvoříme silně typované třídy, který jsou optimalizované pro naše zobrazení konkrétní scénáře, a který vystavit vlastnosti pro dynamické hodnoty nebo obsah vyžaduje našich šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f719-142">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="1f719-143">Naše třídy kontroleru můžete naplnit a předat naše zobrazit šablonu použít tyto třídy optimalizované zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f719-143">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="1f719-144">To umožňuje bezpečnost typů, který kontroluje kompilaci a editor intellisense v rámci šablon zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f719-144">This enables type-safety, compile-time checking, and editor intellisense within view templates.</span></span>

<span data-ttu-id="1f719-145">Chcete-li například povolit večeři formuláře úpravy scénáře vytvoříme "DinnerFormViewModel" třídy jako níže, který zveřejňuje dvě vlastnosti silného typu: objekt večeři a model SelectList potřebné k naplnění rozevírací seznam zemí:</span><span class="sxs-lookup"><span data-stu-id="1f719-145">For example, to enable dinner form editing scenarios we can create a "DinnerFormViewModel" class like below that exposes two strongly-typed properties: a Dinner object, and the SelectList model needed to populate the countries dropdownlist:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

<span data-ttu-id="1f719-146">Můžeme aktualizujte naše Edit() akce metodu pro vytvoření DinnerFormViewModel pomocí večeři objekt, který načteme z našich úložiště a předejte ji do našich zobrazit šablonu:</span><span class="sxs-lookup"><span data-stu-id="1f719-146">We can then update our Edit() action method to create the DinnerFormViewModel using the Dinner object we retrieve from our repository, and then pass it to our view template:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

<span data-ttu-id="1f719-147">Jsme budete pak aktualizace našich zobrazení šablony, které se očekává "DinnerFormViewModel" místo "Večeři" objektu změnou atribut "dědí" v horní části stránky edit.aspx takto:</span><span class="sxs-lookup"><span data-stu-id="1f719-147">We'll then update our view template so that it expects a "DinnerFormViewModel" instead of a "Dinner" object by changing the "inherits" attribute at the top of the edit.aspx page like so:</span></span>

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

<span data-ttu-id="1f719-148">Jakmile jsme to provést, intellisense "Model" vlastnosti v rámci naší zobrazení šablony budou aktualizovány tak, aby odrážela objektový model, který jsme jsou předání typu DinnerFormViewModel:</span><span class="sxs-lookup"><span data-stu-id="1f719-148">Once we do this, the intellisense of the "Model" property within our view template will be updated to reflect the object model of the DinnerFormViewModel type we are passing it:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

<span data-ttu-id="1f719-149">Můžeme aktualizovat zobrazení kódu pracovat z ho.</span><span class="sxs-lookup"><span data-stu-id="1f719-149">We can then update our view code to work off of it.</span></span> <span data-ttu-id="1f719-150">Všimněte si níže jak jsme nejsou změna názvů elementů vstupu vytváříme (elementy form bude stále se s názvem "Title", "Země") – ale aktualizujeme metody pomocné rutiny HTML k načtení hodnoty pomocí třídy DinnerFormViewModel:</span><span class="sxs-lookup"><span data-stu-id="1f719-150">Notice below how we are not changing the names of the input elements we are creating (the form elements will still be named "Title", "Country") – but we are updating the HTML Helper methods to retrieve the values using the DinnerFormViewModel class:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

<span data-ttu-id="1f719-151">Také jsme budete aktualizovat naše metodu post upravit pro použití třídy DinnerFormViewModel při vykreslování chyby:</span><span class="sxs-lookup"><span data-stu-id="1f719-151">We'll also update our Edit post method to use the DinnerFormViewModel class when rendering errors:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

<span data-ttu-id="1f719-152">Můžete také aktualizujeme naše Create() metody akce k opětovnému použití přesné stejné *DinnerFormViewModel* třída povolit zemí rozevírací seznam v těch, které také.</span><span class="sxs-lookup"><span data-stu-id="1f719-152">We can also update our Create() action methods to re-use the exact same *DinnerFormViewModel* class to enable the countries DropDownList within those as well.</span></span> <span data-ttu-id="1f719-153">Dole je implementace HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="1f719-153">Below is the HTTP-GET implementation:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

<span data-ttu-id="1f719-154">Dole je implementace metodu Create HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="1f719-154">Below is the implementation of the HTTP-POST Create method:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

<span data-ttu-id="1f719-155">A teď naše upravit a vytvořit obrazovky podporují rozevíracích seznamů pro výběr země.</span><span class="sxs-lookup"><span data-stu-id="1f719-155">And now both our Edit and Create screens support drop-downlists for picking the country.</span></span>

### <a name="custom-shaped-viewmodel-classes"></a><span data-ttu-id="1f719-156">Ve tvaru vlastní třídy ViewModel</span><span class="sxs-lookup"><span data-stu-id="1f719-156">Custom-shaped ViewModel classes</span></span>

<span data-ttu-id="1f719-157">Ve výše uvedené scénáři Naše třída DinnerFormViewModel přímo zveřejňuje objekt modelu večeři jako vlastnost spolu s podpůrné SelectList vlastnost modelu.</span><span class="sxs-lookup"><span data-stu-id="1f719-157">In the scenario above, our DinnerFormViewModel class directly exposes the Dinner model object as a property, along with a supporting SelectList model property.</span></span> <span data-ttu-id="1f719-158">Tento postup funguje bez problémů pro scénáře, kde rozhraní HTML chceme vytvořit v rámci naší zobrazit šablonu odpovídá relativně úzce naše objekty modelu domény.</span><span class="sxs-lookup"><span data-stu-id="1f719-158">This approach works fine for scenarios where the HTML UI we want to create within our view template corresponds relatively closely to our domain model objects.</span></span>

<span data-ttu-id="1f719-159">Pro scénáře, kde to ale není je případ, jednou z možností, kterou můžete použít pro vytvoření třídy ViewModel ve tvaru vlastní jejichž objektový model více optimalizovaná pro používání v zobrazení – a který může vypadat zcela liší od základní objekt modelu domény.</span><span class="sxs-lookup"><span data-stu-id="1f719-159">For scenarios where this isn't the case, one option that you can use is to create a custom-shaped ViewModel class whose object model is more optimized for consumption by the view – and which might look completely different from the underlying domain model object.</span></span> <span data-ttu-id="1f719-160">Například může potenciálně vystavit, názvy jinou vlastnost nebo agregační vlastností shromážděných z více objektů modelu.</span><span class="sxs-lookup"><span data-stu-id="1f719-160">For example, it could potentially expose different property names and/or aggregate properties collected from multiple model objects.</span></span>

<span data-ttu-id="1f719-161">Může být ve tvaru vlastní třídy ViewModel používají k předávání dat z řadičů pro zobrazení k vykreslení, jakož i ke zpracování dat formuláře odeslat zpět na metodu akce kontroler.</span><span class="sxs-lookup"><span data-stu-id="1f719-161">Custom-shaped ViewModel classes can be used both to pass data from controllers to views to render, as well as to help handle form data posted back to a controller's action method.</span></span> <span data-ttu-id="1f719-162">Pro tento scénář novější můžete mít metody akce aktualizovat objekt ViewModel s daty odeslání formuláře a potom pomocí ViewModel instance mapy nebo načíst objekt modelu skutečné domény.</span><span class="sxs-lookup"><span data-stu-id="1f719-162">For this later scenario, you might have the action method update a ViewModel object with the form-posted data, and then use the ViewModel instance to map or retrieve an actual domain model object.</span></span>

<span data-ttu-id="1f719-163">Ve tvaru vlastní třídy ViewModel může poskytnout značnou flexibilitu a jsou něco prozkoumat kdykoli najít kód vykreslování v rámci zobrazit šablony nebo kód formuláře textu uvnitř vaší metody akce spuštění získat příliš složitý.</span><span class="sxs-lookup"><span data-stu-id="1f719-163">Custom-shaped ViewModel classes can provide a great deal of flexibility, and are something to investigate any time you find the rendering code within your view templates or the form-posting code inside your action methods starting to get too complicated.</span></span> <span data-ttu-id="1f719-164">Toto je často znaménkem zda modely domény není řádně odpovídají jsou generování uživatelského rozhraní, a že může pomoct zprostředkující třída ViewModel ve tvaru vlastní.</span><span class="sxs-lookup"><span data-stu-id="1f719-164">This is often a sign that your domain models don't cleanly correspond to the UI you are generating, and that an intermediate custom-shaped ViewModel class can help.</span></span>

### <a name="next-step"></a><span data-ttu-id="1f719-165">Další krok</span><span class="sxs-lookup"><span data-stu-id="1f719-165">Next Step</span></span>

<span data-ttu-id="1f719-166">Nyní podíváme, jak můžete použít částečné a stránky předlohy pro opakované využití a sdílet uživatelského rozhraní v rámci naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f719-166">Let's now look at how we can use partials and master-pages to re-use and share UI across our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1f719-167">[Předchozí](provide-crud-create-read-update-delete-data-form-entry-support.md)
[další](re-use-ui-using-master-pages-and-partials.md)</span><span class="sxs-lookup"><span data-stu-id="1f719-167">[Previous](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Next](re-use-ui-using-master-pages-and-partials.md)</span></span>