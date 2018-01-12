---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "Zkoumání upravit metody a zobrazení | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 84aadccc18e7fa0fb56c7a78e144a1bf1038aac5
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/19/2017
---
<a name="examining-the-edit-methods-and-edit-view"></a><span data-ttu-id="97397-102">Zkoumání upravit metody a zobrazení</span><span class="sxs-lookup"><span data-stu-id="97397-102">Examining the Edit Methods and Edit View</span></span>
====================
<span data-ttu-id="97397-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="97397-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="97397-104">V této části budete zkontrolujte vygenerovaného `Edit` zobrazení pro řadič film a metody akce.</span><span class="sxs-lookup"><span data-stu-id="97397-104">In this section, you'll examine the generated `Edit` action methods and views for the movie controller.</span></span> <span data-ttu-id="97397-105">Ale nejprve bude trvat krátké zneužívání aby datum vydání vypadat lépe.</span><span class="sxs-lookup"><span data-stu-id="97397-105">But first will take a short diversion to make the release date look better.</span></span> <span data-ttu-id="97397-106">Otevřete *Models\Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="97397-106">Open the *Models\Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

<span data-ttu-id="97397-107">Můžete provést také jazykovou verzi datum konkrétní takto:</span><span class="sxs-lookup"><span data-stu-id="97397-107">You can also make the date culture specific like this:</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

<span data-ttu-id="97397-108">Budeme se zabývat těmito tématy [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="97397-108">We'll cover [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) in the next tutorial.</span></span> <span data-ttu-id="97397-109">[Zobrazit](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayattribute.aspx) atribut určuje, co má být zobrazen pro název pole (v tomto případě "Datum vydání" místo "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="97397-109">The [Display](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayattribute.aspx) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="97397-110">[Datový typ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut určuje typ dat, v takovém případě je datum, a proto není zobrazit čas informace uložené v poli.</span><span class="sxs-lookup"><span data-stu-id="97397-110">The [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute specifies the type of the data, in this case it's a date, so the time information stored in the field is not displayed.</span></span> <span data-ttu-id="97397-111">[DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut je potřebný pro chyby v prohlížeči Chrome, který vykreslí dat. formáty dat nesprávně.</span><span class="sxs-lookup"><span data-stu-id="97397-111">The [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute is needed for a bug in the Chrome browser that renders date formats incorrectly.</span></span>

<span data-ttu-id="97397-112">Spusťte aplikaci a přejděte do `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="97397-112">Run the application and browse to the `Movies` controller.</span></span> <span data-ttu-id="97397-113">Podržte ukazatel myši nad **upravit** na adresu URL, která obsahuje odkazy na odkaz.</span><span class="sxs-lookup"><span data-stu-id="97397-113">Hold the mouse pointer over an **Edit** link to see the URL that it links to.</span></span>

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

<span data-ttu-id="97397-115">**Upravit** odkaz vygenerovalo `Html.ActionLink` metoda v *Views\Movies\Index.cshtml* zobrazení:</span><span class="sxs-lookup"><span data-stu-id="97397-115">The **Edit** link was generated by the `Html.ActionLink` method in the *Views\Movies\Index.cshtml* view:</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

<span data-ttu-id="97397-117">`Html` Objekt je pomocné rutiny, který je zveřejněný pomocí vlastnosti na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/en-us/library/gg402107(VS.98).aspx) základní třídy.</span><span class="sxs-lookup"><span data-stu-id="97397-117">The `Html` object is a helper that's exposed using a property on the [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/en-us/library/gg402107(VS.98).aspx) base class.</span></span> <span data-ttu-id="97397-118">`ActionLink` Metoda pomocníka, který usnadňuje dynamicky generovat hypertextové odkazy HTML, které odkazují na metody akce na řadiče.</span><span class="sxs-lookup"><span data-stu-id="97397-118">The `ActionLink` method of the helper makes it easy to dynamically generate HTML hyperlinks that link to action methods on controllers.</span></span> <span data-ttu-id="97397-119">První argument `ActionLink` metoda je text odkazu k vykreslení (například `<a>Edit Me</a>`).</span><span class="sxs-lookup"><span data-stu-id="97397-119">The first argument to the `ActionLink` method is the link text to render (for example, `<a>Edit Me</a>`).</span></span> <span data-ttu-id="97397-120">Druhý argument je název metody akce, která bude vyvolána (v takovém případě `Edit` akce).</span><span class="sxs-lookup"><span data-stu-id="97397-120">The second argument is the name of the action method to invoke (In this case, the `Edit` action).</span></span> <span data-ttu-id="97397-121">Konečný argument je [anonymní objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) který generuje data trasy (v tomto případě ID 4).</span><span class="sxs-lookup"><span data-stu-id="97397-121">The final argument is an [anonymous object](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) that generates the route data (in this case, the ID of 4).</span></span>

<span data-ttu-id="97397-122">Vygenerovaný odkaz viz předchozí obrázek je `http://localhost:1234/Movies/Edit/4`.</span><span class="sxs-lookup"><span data-stu-id="97397-122">The generated link shown in the previous image is `http://localhost:1234/Movies/Edit/4`.</span></span> <span data-ttu-id="97397-123">Výchozí trasa (nastavené v *aplikace\_Start\RouteConfig.cs*) trvá vzor adresy URL `{controller}/{action}/{id}`.</span><span class="sxs-lookup"><span data-stu-id="97397-123">The default route (established in *App\_Start\RouteConfig.cs*) takes the URL pattern `{controller}/{action}/{id}`.</span></span> <span data-ttu-id="97397-124">Proto ASP.NET překládá `http://localhost:1234/Movies/Edit/4` do požadavek na `Edit` metody akce `Movies` řadiče s parametrem `ID` rovna 4.</span><span class="sxs-lookup"><span data-stu-id="97397-124">Therefore, ASP.NET translates `http://localhost:1234/Movies/Edit/4` into a request to the `Edit` action method of the `Movies` controller with the parameter `ID` equal to 4.</span></span> <span data-ttu-id="97397-125">Zkontrolujte následující kód *aplikace\_Start\RouteConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="97397-125">Examine the following code from the *App\_Start\RouteConfig.cs* file.</span></span> <span data-ttu-id="97397-126">[MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda se používá ke směrování požadavků HTTP do správné metody kontroleru a akce a zadat volitelný parametr ID.</span><span class="sxs-lookup"><span data-stu-id="97397-126">The [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) method is used to route HTTP requests to the correct controller and action method and supply the optional ID parameter.</span></span> <span data-ttu-id="97397-127">[MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda také používány [HtmlHelpers](https://msdn.microsoft.com/en-us/library/system.web.mvc.htmlhelper(v=vs.108).aspx) například `ActionLink` k vygenerování adres URL daného kontroleru, metoda akce a všechna data trasy.</span><span class="sxs-lookup"><span data-stu-id="97397-127">The [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) method is also used by the [HtmlHelpers](https://msdn.microsoft.com/en-us/library/system.web.mvc.htmlhelper(v=vs.108).aspx) such as `ActionLink` to generate URLs given the controller, action method and any route data.</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

<span data-ttu-id="97397-128">Také můžete předat parametry metody akce pomocí řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="97397-128">You can also pass action method parameters using a query string.</span></span> <span data-ttu-id="97397-129">Například adresa URL `http://localhost:1234/Movies/Edit?ID=3` také předá parametr `ID` 3 k `Edit` metody akce `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="97397-129">For example, the URL `http://localhost:1234/Movies/Edit?ID=3` also passes the parameter `ID` of 3 to the `Edit` action method of the `Movies` controller.</span></span>

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

<span data-ttu-id="97397-131">Otevřete `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="97397-131">Open the `Movies` controller.</span></span> <span data-ttu-id="97397-132">Dva `Edit` metody akce, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="97397-132">The two `Edit` action methods are shown below.</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

<span data-ttu-id="97397-133">Všimněte si, druhý `Edit` předchází metody akce `HttpPost` atribut.</span><span class="sxs-lookup"><span data-stu-id="97397-133">Notice the second `Edit` action method is preceded by the `HttpPost` attribute.</span></span> <span data-ttu-id="97397-134">Tento atribut určuje, že přetížení `Edit` metoda může být volána pouze pro požadavky POST.</span><span class="sxs-lookup"><span data-stu-id="97397-134">This attribute specifies that the overload of the `Edit` method can be invoked only for POST requests.</span></span> <span data-ttu-id="97397-135">Je možné aplikovat `HttpGet` atribut prvního upravit metodu, ale který není nutné protože je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="97397-135">You could apply the `HttpGet` attribute to the first edit method, but that's not necessary because it's the default.</span></span> <span data-ttu-id="97397-136">(Budeme označovat metody akce, které jsou implicitně přiřazené `HttpGet` atribut jako `HttpGet` metody.) [Vazby](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) atribut je jiný mechanismus důležité zabezpečení, který udržuje hackery z typu overpost data modelu.</span><span class="sxs-lookup"><span data-stu-id="97397-136">(We'll refer to action methods that are implicitly assigned the `HttpGet` attribute as `HttpGet` methods.) The [Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute is another important security mechanism that keeps hackers from over-posting data to your model.</span></span> <span data-ttu-id="97397-137">Vlastnosti by měla obsahovat pouze vazby atributu, který chcete změnit.</span><span class="sxs-lookup"><span data-stu-id="97397-137">You should only include properties in the bind attribute that you want to change.</span></span> <span data-ttu-id="97397-138">Další informace o overposting a atribut vazby v mé [overposting Poznámka k zabezpečení](https://go.microsoft.com/fwlink/?LinkId=317598).</span><span class="sxs-lookup"><span data-stu-id="97397-138">You can read about overposting and the bind attribute in my [overposting security note](https://go.microsoft.com/fwlink/?LinkId=317598).</span></span> <span data-ttu-id="97397-139">V modelu jednoduché použili v tomto kurzu jsme závazné všechna data v modelu.</span><span class="sxs-lookup"><span data-stu-id="97397-139">In the simple model used in this tutorial, we will be binding all the data in the model.</span></span> <span data-ttu-id="97397-140">[ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atribut se používá k ochraně před paděláním požadavku a je spárován s `@Html.AntiForgeryToken()` v souboru zobrazení upravit (*Views\Movies\Edit.cshtml*), část je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="97397-140">The [ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attribute is used to prevent forgery of a request and is paired up with `@Html.AntiForgeryToken()` in the edit view file (*Views\Movies\Edit.cshtml*), a portion is shown below:</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

<span data-ttu-id="97397-141">`@Html.AntiForgeryToken()`generuje token proti padělání skrytého, který se musí shodovat s v `Edit` metodu `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="97397-141">`@Html.AntiForgeryToken()` generates a hidden form anti-forgery token that must match in the `Edit` method of the `Movies` controller.</span></span> <span data-ttu-id="97397-142">Další informace o webů požadavku padělání (také označované jako XSRF nebo proti útokům CSRF) v mé kurzu [XSRF proti útokům CSRF nebo zabránění v MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).</span><span class="sxs-lookup"><span data-stu-id="97397-142">You can read more about Cross-site request forgery (also known as XSRF or CSRF) in my tutorial [XSRF/CSRF Prevention in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).</span></span>

<span data-ttu-id="97397-143">`HttpGet` `Edit` Metoda přebírá parametr ID film, vyhledává film používající rozhraní Entity Framework `Find` metoda a vrátí vybraného videa na zobrazení pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="97397-143">The `HttpGet` `Edit` method takes the movie ID parameter, looks up the movie using the Entity Framework `Find` method, and returns the selected movie to the Edit view.</span></span> <span data-ttu-id="97397-144">Pokud nelze nalézt film, [HttpNotFound](https://msdn.microsoft.com/en-us/library/gg453938(VS.98).aspx) je vrácen.</span><span class="sxs-lookup"><span data-stu-id="97397-144">If a movie cannot be found, [HttpNotFound](https://msdn.microsoft.com/en-us/library/gg453938(VS.98).aspx) is returned.</span></span> <span data-ttu-id="97397-145">Při generování uživatelského rozhraní systému vytvoření zobrazení pro úpravy, zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy.</span><span class="sxs-lookup"><span data-stu-id="97397-145">When the scaffolding system created the Edit view, it examined the `Movie` class and created code to render `<label>` and `<input>` elements for each property of the class.</span></span> <span data-ttu-id="97397-146">Následující příklad ukazuje zobrazení úpravy, který byl vytvořen v sadě visual studio generování uživatelského rozhraní systému:</span><span class="sxs-lookup"><span data-stu-id="97397-146">The following example shows the Edit view that was generated by the visual studio scaffolding system:</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

<span data-ttu-id="97397-147">Všimněte si, jak se má zobrazit šablonu `@model MvcMovie.Models.Movie` příkaz v horní části souboru – tato hodnota určuje, že zobrazení očekává, že model pro zobrazení šablony být typu `Movie`.</span><span class="sxs-lookup"><span data-stu-id="97397-147">Notice how the view template has a `@model MvcMovie.Models.Movie` statement at the top of the file — this specifies that the view expects the model for the view template to be of type `Movie`.</span></span>

<span data-ttu-id="97397-148">Automaticky generovaný kód používá několik *pomocné metody* zefektivnění kód HTML.</span><span class="sxs-lookup"><span data-stu-id="97397-148">The scaffolded code uses several *helper methods* to streamline the HTML markup.</span></span> <span data-ttu-id="97397-149">[ `Html.LabelFor` ](https://msdn.microsoft.com/en-us/library/gg401864(VS.98).aspx) Pomocné rutiny zobrazuje název pole (&quot;název&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, nebo &quot;cena &quot;).</span><span class="sxs-lookup"><span data-stu-id="97397-149">The [`Html.LabelFor`](https://msdn.microsoft.com/en-us/library/gg401864(VS.98).aspx) helper displays the name of the field (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, or &quot;Price&quot;).</span></span> <span data-ttu-id="97397-150">[ `Html.EditorFor` ](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Vykreslí pomocné rutiny HTML `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="97397-150">The [`Html.EditorFor`](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper renders an HTML `<input>` element.</span></span> <span data-ttu-id="97397-151">[ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocník zobrazí všechny zprávy ověření přidružené k této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="97397-151">The [`Html.ValidationMessageFor`](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper displays any validation messages associated with that property.</span></span>

<span data-ttu-id="97397-152">Spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="97397-152">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="97397-153">Klikněte na tlačítko **upravit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="97397-153">Click an **Edit** link.</span></span> <span data-ttu-id="97397-154">V prohlížeči zobrazte zdroj pro stránku.</span><span class="sxs-lookup"><span data-stu-id="97397-154">In the browser, view the source for the page.</span></span> <span data-ttu-id="97397-155">Kód HTML pro form element jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="97397-155">The HTML for the form element is shown below.</span></span>

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

<span data-ttu-id="97397-156">`<input>` Prvky jsou v kódu HTML `<form>` element jejichž `action` k vystavování příspěvků na nastavený atribut *nebo filmy nebo upravit* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="97397-156">The `<input>` elements are in an HTML `<form>` element whose `action` attribute is set to post to the */Movies/Edit* URL.</span></span> <span data-ttu-id="97397-157">Data formuláře budou odeslány na server při **Uložit** po kliknutí na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97397-157">The form data will be posted to the server when the **Save** button is clicked.</span></span> <span data-ttu-id="97397-158">Druhý řádek ukazuje skrytého [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token vygenerované `@Html.AntiForgeryToken()` volání.</span><span class="sxs-lookup"><span data-stu-id="97397-158">The second line shows the hidden [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generated by the `@Html.AntiForgeryToken()` call.</span></span>

## <a name="processing-the-post-request"></a><span data-ttu-id="97397-159">Zpracování požadavku POST</span><span class="sxs-lookup"><span data-stu-id="97397-159">Processing the POST Request</span></span>

<span data-ttu-id="97397-160">Následující seznam uvádí `HttpPost` verzi `Edit` metody akce.</span><span class="sxs-lookup"><span data-stu-id="97397-160">The following listing shows the `HttpPost` version of the `Edit` action method.</span></span>

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

<span data-ttu-id="97397-161">[ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) ověří atribut [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token vygenerované `@Html.AntiForgeryToken()` volání v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="97397-161">The [ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attribute validates the [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generated by the `@Html.AntiForgeryToken()` call in the view.</span></span>

<span data-ttu-id="97397-162">[Vazač modelu ASP.NET MVC](https://msdn.microsoft.com/en-us/library/dd410405.aspx) přijímá hodnoty odeslaného formuláře a vytvoří `Movie` objekt, který se předá jako `movie` parametr.</span><span class="sxs-lookup"><span data-stu-id="97397-162">The [ASP.NET MVC model binder](https://msdn.microsoft.com/en-us/library/dd410405.aspx) takes the posted form values and creates a `Movie` object that's passed as the `movie` parameter.</span></span> <span data-ttu-id="97397-163">`ModelState.IsValid` Metoda ověří, že odeslaná data do formuláře lze použít k úpravě (upravit nebo aktualizace) `Movie` objektu.</span><span class="sxs-lookup"><span data-stu-id="97397-163">The `ModelState.IsValid` method verifies that the data submitted in the form can be used to modify (edit or update) a `Movie` object.</span></span> <span data-ttu-id="97397-164">Pokud jsou data platná, film data jsou uložena do `Movies` kolekce `db(MovieDBContext` instance).</span><span class="sxs-lookup"><span data-stu-id="97397-164">If the data is valid, the movie data is saved to the `Movies` collection of the `db(MovieDBContext` instance).</span></span> <span data-ttu-id="97397-165">Nová data film je uložit do databáze při volání `SaveChanges` metodu `MovieDBContext`.</span><span class="sxs-lookup"><span data-stu-id="97397-165">The new movie data is saved to the database by calling the `SaveChanges` method of `MovieDBContext`.</span></span> <span data-ttu-id="97397-166">Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, která zobrazuje kolekce film, včetně právě změny.</span><span class="sxs-lookup"><span data-stu-id="97397-166">After saving the data, the code redirects the user to the `Index` action method of the `MoviesController` class, which displays the movie collection, including the changes just made.</span></span>

<span data-ttu-id="97397-167">Při ověřování na straně klienta zjistí, že hodnoty pole nejsou platné, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="97397-167">As soon as the client side validation determines the values of a field are not valid, an error message is displayed.</span></span> <span data-ttu-id="97397-168">Pokud zakážete JavaScript, nebude mít ověřování na straně klienta, ale server bude zjišťovat odeslaných hodnot nejsou platné, a hodnot formuláře se zobrazí znovu s chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="97397-168">If you disable JavaScript, you won't have client side validation but the server will detect the posted values are not valid, and the form values will be redisplayed with error messages.</span></span> <span data-ttu-id="97397-169">Později v tomto kurzu jsme zkontrolujte ověření podrobněji.</span><span class="sxs-lookup"><span data-stu-id="97397-169">Later in the tutorial we examine validation in more detail.</span></span>

<span data-ttu-id="97397-170">`Html.ValidationMessageFor` Pomocné rutiny v *Edit.cshtml* zobrazení šablony postará o zobrazení příslušné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="97397-170">The `Html.ValidationMessageFor` helpers in the *Edit.cshtml* view template take care of displaying appropriate error messages.</span></span>

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

<span data-ttu-id="97397-172">Všechny `HttpGet` podobný Princip podle metody.</span><span class="sxs-lookup"><span data-stu-id="97397-172">All the `HttpGet` methods follow a similar pattern.</span></span> <span data-ttu-id="97397-173">Získají objekt film (nebo seznam objektů, v případě `Index`) a předat zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="97397-173">They get a movie object (or list of objects, in the case of `Index`), and pass the model to the view.</span></span> <span data-ttu-id="97397-174">`Create` Metoda předá objekt prázdný film zobrazení pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="97397-174">The `Create` method passes an empty movie object to the Create view.</span></span> <span data-ttu-id="97397-175">Takže v udělat všechny metody, které vytvořit, upravit, odstranit nebo jinak měnit data `HttpPost` přetížení metody.</span><span class="sxs-lookup"><span data-stu-id="97397-175">All the methods that create, edit, delete, or otherwise modify data do so in the `HttpPost` overload of the method.</span></span> <span data-ttu-id="97397-176">Úprava dat v metodu GET protokolu HTTP je bezpečnostním rizikem, jak je popsáno v příspěvku blogu [ASP.NET MVC Tip #46 – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span><span class="sxs-lookup"><span data-stu-id="97397-176">Modifying data in an HTTP GET method is a security risk, as described in the blog post entry [ASP.NET MVC Tip #46 – Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span> <span data-ttu-id="97397-177">Úprava dat v metodu GET také porušuje HTTP osvědčené postupy a architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) vzor, který určuje, že požadavky GET nesmí změnit stav vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="97397-177">Modifying data in a GET method also violates HTTP best practices and the architectural [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) pattern, which specifies that GET requests should not change the state of your application.</span></span> <span data-ttu-id="97397-178">Jinými slovy provádění operace GET by měl být bezpečné operace, která nemá žádné vedlejší účinky a nezmění trvalá data.</span><span class="sxs-lookup"><span data-stu-id="97397-178">In other words, performing a GET operation should be a safe operation that has no side effects and doesn't modify your persisted data.</span></span>

<span data-ttu-id="97397-179">Pokud používáte čeština – počítače, můžete tuto část přeskočte a přejděte na další kurz.</span><span class="sxs-lookup"><span data-stu-id="97397-179">If you are using a US-English computer, you can skip this section and go to the next tutorial.</span></span> <span data-ttu-id="97397-180">Globalize verzi tohoto kurzu si můžete stáhnout [zde](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475).</span><span class="sxs-lookup"><span data-stu-id="97397-180">You can download the Globalize version of this tutorial [here](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475).</span></span> <span data-ttu-id="97397-181">Vynikající kurzu dvě části na internacionalizace, najdete v části [Nadeem na ASP.NET MVC 5 internacionalizace](http://afana.me/post/aspnet-mvc-internationalization.aspx).</span><span class="sxs-lookup"><span data-stu-id="97397-181">For an excellent two part tutorial on Internationalization, see [Nadeem's ASP.NET MVC 5 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx).</span></span>


> [!NOTE]
> <span data-ttu-id="97397-182">Pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) pro desetinné čárky a formát data neanglických USA, musíte zahrnout *globalize.js* a konkrétní  *cultures/Globalize.cultures.js* souboru (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="97397-182">to support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="97397-183">Ověřování jiných než anglických jQuery můžete získat z NuGet.</span><span class="sxs-lookup"><span data-stu-id="97397-183">You can get the jQuery non-English validation from NuGet.</span></span> <span data-ttu-id="97397-184">(Neinstalujte Globalize Pokud používáte anglickou národního prostředí.)</span><span class="sxs-lookup"><span data-stu-id="97397-184">(Don't install Globalize if you are using a English locale.)</span></span>


1. <span data-ttu-id="97397-185">Z **nástroje** nabídce klikněte na tlačítko **Správce balíčků NuGetLibrary**a potom klikněte na **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="97397-185">From the **Tools** menu click **NuGetLibrary Package Manager**, and then click **Manage NuGet Packages for Solution**.</span></span>  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. <span data-ttu-id="97397-186">V levém podokně, vyberte  **Procházet*.* ** (Viz následující obrázek).</span><span class="sxs-lookup"><span data-stu-id="97397-186">On the left pane, select **Browse*.***(See the image below.)</span></span>
3. <span data-ttu-id="97397-187">Do vstupního pole zadejte *Globalize**.</span><span class="sxs-lookup"><span data-stu-id="97397-187">In the input box, enter *Globalize**.</span></span>  
  
    <span data-ttu-id="97397-188">![](examining-the-edit-methods-and-edit-view/_static/image6.png)Zvolte `jQuery.Validation.Globalize`, zvolte `MvcMovie` a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="97397-188">![](examining-the-edit-methods-and-edit-view/_static/image6.png) Choose `jQuery.Validation.Globalize`, choose `MvcMovie` and click **Install**.</span></span> <span data-ttu-id="97397-189">*Scripts\jquery.globalize\globalize.js* soubor bude přidán do projektu.</span><span class="sxs-lookup"><span data-stu-id="97397-189">The *Scripts\jquery.globalize\globalize.js* file will be added to your project.</span></span> <span data-ttu-id="97397-190">*Scripts\jquery.globalize\cultures\* bude obsahovat soubory JavaScript mnoho jazykovou verzi.</span><span class="sxs-lookup"><span data-stu-id="97397-190">The *Scripts\jquery.globalize\cultures\* folder will contain many culture JavaScript files.</span></span> <span data-ttu-id="97397-191">Poznámka: může trvat pět minut, chcete-li nainstalovat tento balíček.</span><span class="sxs-lookup"><span data-stu-id="97397-191">Note, it may take five minutes to install this package.</span></span>

 <span data-ttu-id="97397-192">Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml:</span><span class="sxs-lookup"><span data-stu-id="97397-192">The following code shows the modifications to the Views\Movies\Edit.cshtml file:</span></span> 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

<span data-ttu-id="97397-193">Abyste se vyhnuli opakující se tento kód v každé upravit zobrazení, můžete přesuňte jej do souboru rozložení.</span><span class="sxs-lookup"><span data-stu-id="97397-193">To avoid repeating this code in every Edit view, you can move it to the layout file.</span></span> <span data-ttu-id="97397-194">K optimalizaci stahování skriptu, najdete v části Moje kurzu [sdružování a Minifikace](../../performance/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="97397-194">To optimize the script download, see my tutorial [Bundling and Minification](../../performance/bundling-and-minification.md).</span></span>

<span data-ttu-id="97397-195">Další informace najdete v části [ASP.NET MVC 3 internacionalizace](http://afana.me/post/aspnet-mvc-internationalization.aspx) a [ASP.NET MVC 3 internacionalizace – část 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="97397-195">For more information see [ASP.NET MVC 3 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx) and [ASP.NET MVC 3 Internationalization - Part 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).</span></span>

<span data-ttu-id="97397-196">Jako dočasné opravu Pokud nelze získat ověření práce v národním prostředí, můžete vynutit počítače pro angličtinu, nebo můžete zakázat JavaScript v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="97397-196">As a temporary fix, if you can't get validation working in your locale, you can force your computer to use US English or you can disable JavaScript in your browser.</span></span> <span data-ttu-id="97397-197">Chcete-li vynutit počítače pro angličtinu, můžete přidat element globalizace do kořenového adresáře projektů *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="97397-197">To force your computer to use US English, you can add the globalization element to the projects root *web.config* file.</span></span> <span data-ttu-id="97397-198">Následující kód ukazuje element globalizace jazyková verze nastavena angličtina (Spojené státy).</span><span class="sxs-lookup"><span data-stu-id="97397-198">The following code shows the globalization element with the culture set to United States English.</span></span>

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><span data-ttu-id="97397-199"><a id="jQueryAjaxJSON"></a>V dalším kurzu jsme budete implementovat funkce vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="97397-199"><a id="jQueryAjaxJSON"></a> In the next tutorial, we'll implement search functionality.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="97397-200">[Předchozí](accessing-your-models-data-from-a-controller.md)
[další](adding-search.md)</span><span class="sxs-lookup"><span data-stu-id="97397-200">[Previous](accessing-your-models-data-from-a-controller.md)
[Next](adding-search.md)</span></span>