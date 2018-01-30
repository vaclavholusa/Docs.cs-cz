---
title: "Pomocníci značky ve formulářích v ASP.NET Core"
author: rick-anderson
description: "Popisuje předdefinované značky Pomocníci použít s formuláři."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/working-with-forms
ms.openlocfilehash: 805c2ba5b3a9669d5547e1c595883436eea0d11a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="f8cc5-103">Základní informace o použití značky Pomocníci ve formulářích v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8cc5-103">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="f8cc5-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), a [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="f8cc5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="f8cc5-105">Tento dokument ukazuje práci s formuláři a elementů HTML běžně používají na formuláři.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="f8cc5-106">HTML [formuláře](https://www.w3.org/TR/html401/interact/forms.html) element poskytuje primární mechanismus webové aplikace využívají k odesílání dat zpět na server.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="f8cc5-107">Většina tento dokument popisuje [značky Pomocníci](tag-helpers/intro.md) a jak se vám může pomoct efektivní vytvořit robustní formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="f8cc5-108">Doporučujeme, abyste si přečetli [Úvod do pomocné rutiny značky](tag-helpers/intro.md) před čtením tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="f8cc5-109">V mnoha případech pomocné objekty HTML poskytnout alternativní způsob konkrétní pomocné rutiny značky, ale je důležité vědět, že pomocné rutiny značky není nahradit pomocné rutiny HTML a neexistuje žádná značka Pomocník pro každý pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="f8cc5-110">Pokud existuje alternativu pomocné rutiny HTML, je uvedený.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="f8cc5-111">Pomocník značku formuláře</span><span class="sxs-lookup"><span data-stu-id="f8cc5-111">The Form Tag Helper</span></span>

<span data-ttu-id="f8cc5-112">[Formuláře](https://www.w3.org/TR/html401/interact/forms.html) značky pomocné rutiny:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="f8cc5-113">Generuje kód HTML [ \<formuláře >](https://www.w3.org/TR/html401/interact/forms.html) `action` hodnotu atributu pro akce kontroleru MVC nebo pojmenovanou trasu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="f8cc5-114">Vytváří skrytý [žádosti o ověření tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) při ochraně před paděláním požadavku posílaného mezi weby (při použití s `[ValidateAntiForgeryToken]` atribut v metodě akce HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="f8cc5-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="f8cc5-115">Poskytuje `asp-route-<Parameter Name>` atribut, kde `<Parameter Name>` se přidá do hodnoty trasy.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="f8cc5-116">`routeValues` Parametry, které `Html.BeginForm` a `Html.BeginRouteForm` poskytuje podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="f8cc5-117">Má alternativu pomocné rutiny HTML `Html.BeginForm` a`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="f8cc5-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="f8cc5-118">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-118">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="f8cc5-119">Pomocné rutiny formuláře značka výše generuje následující HTML:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="f8cc5-120">Modul runtime rozhraní MVC vygeneruje `action` hodnotu atributu ze atributů pomocná značku formuláře `asp-controller` a `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="f8cc5-121">Pomocník značku formuláře také vytváří skrytý [žádosti o ověření tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) při ochraně před paděláním požadavku posílaného mezi weby (při použití s `[ValidateAntiForgeryToken]` atribut v metodě akce HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="f8cc5-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="f8cc5-122">Ochrana proti padělání požadavku posílaného mezi weby čistý formuláře HTML je obtížné, pomocná značku formuláře poskytuje tuto službu můžete.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="f8cc5-123">Pomocí pojmenovanou trasu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-123">Using a named route</span></span>

<span data-ttu-id="f8cc5-124">`asp-route` Značky pomocný atribut můžete také vygenerovat kód pro kód HTML `action` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="f8cc5-125">Aplikace s [trasy](../../fundamentals/routing.md) s názvem `register` použít následující kód pro stránku registrace:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="f8cc5-126">Řadu zobrazení *zobrazení nebo účet* složky (vygeneruje, když vytvoříte novou webovou aplikaci s *jednotlivé uživatelské účty*) obsahují [asp. trasy returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atribut:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="f8cc5-127">Pomocí integrované šablony `returnUrl` je pouze naplněny automaticky při pokusu o přístup k autorizovaným prostředků, ale není ověřený nebo oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="f8cc5-128">Když zkusíte neoprávněným přístupem, middleware zabezpečení vás přesměruje na přihlašovací stránku s `returnUrl` nastavit.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="f8cc5-129">Pomocník vstupní značky</span><span class="sxs-lookup"><span data-stu-id="f8cc5-129">The Input Tag Helper</span></span>

<span data-ttu-id="f8cc5-130">Váže vstupní značka pomocné rutiny HTML [ \<vstupní >](https://www.w3.org/wiki/HTML/Elements/input) element modelu výrazu v zobrazení syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="f8cc5-131">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="f8cc5-132">Pomocník vstupní značky:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="f8cc5-133">Generuje `id` a `name` atributy HTML pro název výraz zadaný v `asp-for` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="f8cc5-134">`asp-for="Property1.Property2"`je ekvivalentní `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="f8cc5-135">Název výrazu se bude používat pro `asp-for` hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="f8cc5-136">Najdete v článku [názvy výrazů](#expression-names) části Další informace.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="f8cc5-137">Nastaví HTML `type` atribut hodnotu na základě typu modelu a [datové poznámky](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributy použité na vlastnost modelu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="f8cc5-138">Nedojde k přepsání HTML `type` hodnota atributu, pokud byl zadán jeden</span><span class="sxs-lookup"><span data-stu-id="f8cc5-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="f8cc5-139">Generuje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributy ověření ze [datové poznámky](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributy použité na vlastnosti modelu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="f8cc5-140">Obsahuje funkci pomocné rutiny HTML, které se překrývají s `Html.TextBoxFor` a `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="f8cc5-141">Najdete v článku **pomocné rutiny HTML alternativy vstupní značka pomocná** podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="f8cc5-142">Poskytuje silného typování.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-142">Provides strong typing.</span></span> <span data-ttu-id="f8cc5-143">Pokud název změny vlastností a neaktualizovat pomocná značky budete dojde k chybě podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="f8cc5-144">`Input` Nastaví značku pomocné rutiny HTML `type` atribut založený na typu .NET.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="f8cc5-145">Následující tabulka uvádí některé běžné typy .NET a vygenerovaný typ HTML (ne každý typ formátu .NET je uvedena).</span><span class="sxs-lookup"><span data-stu-id="f8cc5-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="f8cc5-146">Typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="f8cc5-146">.NET type</span></span>|<span data-ttu-id="f8cc5-147">Typ vstupu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="f8cc5-148">BOOL</span><span class="sxs-lookup"><span data-stu-id="f8cc5-148">Bool</span></span>|<span data-ttu-id="f8cc5-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-149">type=”checkbox”</span></span>|
|<span data-ttu-id="f8cc5-150">String</span><span class="sxs-lookup"><span data-stu-id="f8cc5-150">String</span></span>|<span data-ttu-id="f8cc5-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-151">type=”text”</span></span>|
|<span data-ttu-id="f8cc5-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="f8cc5-152">DateTime</span></span>|<span data-ttu-id="f8cc5-153">type=”datetime”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-153">type=”datetime”</span></span>|
|<span data-ttu-id="f8cc5-154">Byte</span><span class="sxs-lookup"><span data-stu-id="f8cc5-154">Byte</span></span>|<span data-ttu-id="f8cc5-155">typ = "number"</span><span class="sxs-lookup"><span data-stu-id="f8cc5-155">type=”number”</span></span>|
|<span data-ttu-id="f8cc5-156">celá čísla</span><span class="sxs-lookup"><span data-stu-id="f8cc5-156">Int</span></span>|<span data-ttu-id="f8cc5-157">typ = "number"</span><span class="sxs-lookup"><span data-stu-id="f8cc5-157">type=”number”</span></span>|
|<span data-ttu-id="f8cc5-158">Jednoduché, Double</span><span class="sxs-lookup"><span data-stu-id="f8cc5-158">Single, Double</span></span>|<span data-ttu-id="f8cc5-159">typ = "number"</span><span class="sxs-lookup"><span data-stu-id="f8cc5-159">type=”number”</span></span>|


<span data-ttu-id="f8cc5-160">Následující tabulka uvádí některé běžné [datových poznámek](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributy, které pomocné rutiny vstupní značka se mapování na konkrétní typy vstupu (ne každý atribut ověření je uvedena):</span><span class="sxs-lookup"><span data-stu-id="f8cc5-160">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="f8cc5-161">Atribut</span><span class="sxs-lookup"><span data-stu-id="f8cc5-161">Attribute</span></span>|<span data-ttu-id="f8cc5-162">Typ vstupu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="f8cc5-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-163">[EmailAddress]</span></span>|<span data-ttu-id="f8cc5-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-164">type=”email”</span></span>|
|<span data-ttu-id="f8cc5-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-165">[Url]</span></span>|<span data-ttu-id="f8cc5-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-166">type=”url”</span></span>|
|<span data-ttu-id="f8cc5-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-167">[HiddenInput]</span></span>|<span data-ttu-id="f8cc5-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-168">type=”hidden”</span></span>|
|<span data-ttu-id="f8cc5-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-169">[Phone]</span></span>|<span data-ttu-id="f8cc5-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-170">type=”tel”</span></span>|
|<span data-ttu-id="f8cc5-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="f8cc5-172">typ = "password"</span><span class="sxs-lookup"><span data-stu-id="f8cc5-172">type=”password”</span></span>|
|<span data-ttu-id="f8cc5-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="f8cc5-174">type=”date”</span><span class="sxs-lookup"><span data-stu-id="f8cc5-174">type=”date”</span></span>|
|<span data-ttu-id="f8cc5-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="f8cc5-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="f8cc5-176">typ = "čas"</span><span class="sxs-lookup"><span data-stu-id="f8cc5-176">type=”time”</span></span>|


<span data-ttu-id="f8cc5-177">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-177">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="f8cc5-178">Výše uvedený kód generuje následující HTML:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="f8cc5-179">U anotací dat `Email` a `Password` vlastnosti generovat metadata na modelu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="f8cc5-180">Pomocník značky vstup využívá metadat modelu a vytváří [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributy (najdete v části [ověření modelu](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="f8cc5-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="f8cc5-181">Tyto atributy popisují validátory pro připojení k vstupních polí.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="f8cc5-182">To umožňuje nerušivý HTML5 a [jQuery](https://jquery.com/) ověření.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="f8cc5-183">Nerušivý atributy mají formát `data-val-rule="Error Message"`, kde pravidlo je název pravidla ověření (například `data-val-required`, `data-val-email`, `data-val-maxlength`atd.) Pokud atribut je součástí chybovou zprávu, zobrazí se jako hodnota `data-val-rule` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="f8cc5-184">Existují také atributy ve formátu `data-val-ruleName-argumentName="argumentValue"` které poskytují další informace o pravidle, například `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="f8cc5-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="f8cc5-185">Pomocné rutiny HTML alternativy vstupní značka pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="f8cc5-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="f8cc5-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` a `Html.EditorFor` překrývajících se funkce s pomocnou rutinou vstupní značka.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="f8cc5-187">Pomocné rutiny vstupní značka automaticky nastaví `type` atribut; `Html.TextBox` a `Html.TextBoxFor` nebude.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="f8cc5-188">`Html.Editor`a `Html.EditorFor` zpracování kolekcí, komplexní objekty a šablony; není pomocný vstupní značka.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="f8cc5-189">Pomocné rutiny vstupní značka, `Html.EditorFor` a `Html.TextBoxFor` jsou silného typu (jejich použití výrazů lambda); `Html.TextBox` a `Html.Editor` nejsou (používají názvy výrazů).</span><span class="sxs-lookup"><span data-stu-id="f8cc5-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="f8cc5-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="f8cc5-190">HtmlAttributes</span></span>

<span data-ttu-id="f8cc5-191">`@Html.Editor()`a `@Html.EditorFor()` použít speciální `ViewDataDictionary` položka s názvem `htmlAttributes` při provádění jejich výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="f8cc5-192">Toto chování je volitelně rozšířen pomocí `additionalViewData` parametry.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="f8cc5-193">Klíč "htmlAttributes" nerozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="f8cc5-194">Klíč "htmlAttributes" se zpracovávají podobně jako `htmlAttributes` byl předán objekt pomocné rutiny jako vstup `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="f8cc5-195">Názvy výrazů</span><span class="sxs-lookup"><span data-stu-id="f8cc5-195">Expression names</span></span>

<span data-ttu-id="f8cc5-196">`asp-for` Hodnota atributu je `ModelExpression` a pravé straně výrazu lambda.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="f8cc5-197">Proto `asp-for="Property1"` stane `m => m.Property1` generovaného kódu, proto nemusíte předpony s `Model`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="f8cc5-198">Můžete použít "@" znak spustit výraz vložené a přesuňte před `m.`:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="f8cc5-199">Generuje následující:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="f8cc5-200">Pomocí vlastnosti kolekce `asp-for="CollectionProperty[23].Member"` vygeneruje stejný název jako `asp-for="CollectionProperty[i].Member"` při `i` má hodnotu `23`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="f8cc5-201">Navigace vlastnosti podřízené</span><span class="sxs-lookup"><span data-stu-id="f8cc5-201">Navigating child properties</span></span>

<span data-ttu-id="f8cc5-202">Můžete také přejít na vlastnosti podřízené pomocí cesty k vlastnosti zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-202">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="f8cc5-203">Vezměte v úvahu složitější třídu modelu, která obsahuje podřízenou `Address` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-203">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="f8cc5-204">V zobrazení, můžeme vytvořit vazbu na `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-204">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="f8cc5-205">Následující kód HTML se generuje pro `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-205">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="f8cc5-206">Názvy výrazů a kolekce</span><span class="sxs-lookup"><span data-stu-id="f8cc5-206">Expression names and Collections</span></span>

<span data-ttu-id="f8cc5-207">Ukázkový model obsahující pole `Colors`:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-207">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="f8cc5-208">Metoda akce:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-208">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="f8cc5-209">Následující syntaxe Razor ukazuje, jak přistupovat k konkrétní `Color` element:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-209">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="f8cc5-210">*Views/Shared/EditorTemplates/String.cshtml* šablony:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-210">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="f8cc5-211">Ukázka použití `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-211">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="f8cc5-212">Následující syntaxe Razor ukazuje, jak iterace v kolekci:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-212">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="f8cc5-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="f8cc5-214">Vždy používat `for` (a *není* `foreach`) k iterace v seznamu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-214">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="f8cc5-215">Vyhodnocení indexer v dotazu LINQ výraz může být nákladné a by měl být minimální.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-215">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="f8cc5-216">Výše uvedené komentáři vzorový kód ukazuje, jak by nahraďte výraz lambda s `@` operátor pro přístup k každý `ToDoItem` v seznamu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-216">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="f8cc5-217">Pomocník Textarea značky</span><span class="sxs-lookup"><span data-stu-id="f8cc5-217">The Textarea Tag Helper</span></span>

<span data-ttu-id="f8cc5-218">`Textarea Tag Helper` Pomocná značky je podobná pomocné rutiny vstupní značka.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-218">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="f8cc5-219">Generuje `id` a `name` a atributů ověření dat z modelu pro [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-219">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="f8cc5-220">Poskytuje silného typování.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-220">Provides strong typing.</span></span>

* <span data-ttu-id="f8cc5-221">Alternativní pomocné rutiny HTML:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="f8cc5-221">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="f8cc5-222">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-222">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="f8cc5-223">Následující kód HTML se vygeneruje:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-223">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="f8cc5-224">Pomocník značky popisek</span><span class="sxs-lookup"><span data-stu-id="f8cc5-224">The Label Tag Helper</span></span>

* <span data-ttu-id="f8cc5-225">Generuje popisek popisek a `for` atributu u [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) element pro název výrazu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-225">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="f8cc5-226">Alternativní pomocné rutiny HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-226">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="f8cc5-227">`Label Tag Helper` Přes čistý element popisku HTML poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-227">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="f8cc5-228">Automaticky získána hodnota popisné označení `Display` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-228">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="f8cc5-229">Určený zobrazovaný název mohou změnit čas a kombinace `Display` atribut a popisek značky pomocná budou platit `Display` všude, kde se používá.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-229">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="f8cc5-230">Menší značek ve zdrojovém kódu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-230">Less markup in source code</span></span>

* <span data-ttu-id="f8cc5-231">Silné typování s vlastností modelu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-231">Strong typing with the model property.</span></span>

<span data-ttu-id="f8cc5-232">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-232">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="f8cc5-233">Následující kód HTML se generuje pro `<label>` element:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-233">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="f8cc5-234">Pomocné rutiny značky popisek vygenerované `for` hodnotu atributu "E-mailu", což je Identifikátor přidružený `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-234">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="f8cc5-235">Pomocníci značky generovat konzistentní `id` a `for` elementy, aby mohly být správně přidružena.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-235">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="f8cc5-236">Titulek v této ukázce pochází z `Display` atribut.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-236">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="f8cc5-237">Pokud model neobsahovalo `Display` atribut popisek by být název vlastnosti výrazu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-237">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="f8cc5-238">Pomocníci značky ověření</span><span class="sxs-lookup"><span data-stu-id="f8cc5-238">The Validation Tag Helpers</span></span>

<span data-ttu-id="f8cc5-239">Existují dvě značky Pomocníci ověření.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-239">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="f8cc5-240">`Validation Message Tag Helper` (Které zobrazí zprávu ověření pro jedinou vlastnost v modelu) a `Validation Summary Tag Helper` (zobrazuje souhrn chyb ověřování).</span><span class="sxs-lookup"><span data-stu-id="f8cc5-240">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="f8cc5-241">`Input Tag Helper` Přidá HTML5 klientské straně ověření atributy jako vstup elementy na základě dat atributy poznámky na třídách modelu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-241">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="f8cc5-242">Ověření je také provést na serveru.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-242">Validation is also performed on the server.</span></span> <span data-ttu-id="f8cc5-243">Pomocná rutina pro ověření značky zobrazí tyto chybové zprávy, když dojde k chybě ověření.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-243">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="f8cc5-244">Pomocná rutina pro ověření zprávy značky</span><span class="sxs-lookup"><span data-stu-id="f8cc5-244">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="f8cc5-245">Přidá [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atribut [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, který připojí chybové zprávy ověření na vlastnosti zadaného modelu vstupní pole.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-245">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="f8cc5-246">Když dojde k chybě ověření straně klienta, [jQuery](https://jquery.com/) zobrazí chybovou zprávu ve `<span>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-246">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="f8cc5-247">Ověření také probíhá na serveru.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-247">Validation also takes place on the server.</span></span> <span data-ttu-id="f8cc5-248">Klienti mohou mít JavaScript zakázán a některé ověření provést pouze na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-248">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="f8cc5-249">Alternativní pomocné rutiny HTML:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="f8cc5-249">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="f8cc5-250">`Validation Message Tag Helper` Se používá s `asp-validation-for` atribut HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elementu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-250">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="f8cc5-251">Pomocná rutina pro ověření zprávy značky se generují následující kód HTML:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-251">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="f8cc5-252">Obvykle použijete `Validation Message Tag Helper` po `Input` značky Pomocník pro stejnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-252">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="f8cc5-253">Díky tomu se zobrazí chybové zprávy ověření téměř vstupu, který chybu způsobil.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-253">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="f8cc5-254">Musíte mít zobrazení s správný jazyk JavaScript a [jQuery](https://jquery.com/) skript odkazy na místě pro ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-254">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="f8cc5-255">V tématu [ověření modelu](../models/validation.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-255">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="f8cc5-256">Když se stane Chyba ověření straně serveru (například pokud máte ověřování na straně serveru vlastní nebo je zakázáno ověřování na straně klienta), MVC umístí tuto chybovou zprávu jako text `<span>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-256">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="f8cc5-257">Pomocná rutina pro ověření Summary – značka</span><span class="sxs-lookup"><span data-stu-id="f8cc5-257">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="f8cc5-258">Cíle `<div>` elementů s hodnotou `asp-validation-summary` atribut</span><span class="sxs-lookup"><span data-stu-id="f8cc5-258">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="f8cc5-259">Alternativní pomocné rutiny HTML:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="f8cc5-259">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="f8cc5-260">`Validation Summary Tag Helper` Se používá k zobrazení Souhrn ověřovacích zpráv.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-260">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="f8cc5-261">`asp-validation-summary` Hodnota atributu může být libovolná z následujících:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-261">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="f8cc5-262">ASP souhrnu ověření</span><span class="sxs-lookup"><span data-stu-id="f8cc5-262">asp-validation-summary</span></span>|<span data-ttu-id="f8cc5-263">Zobrazí ověřovacích zpráv</span><span class="sxs-lookup"><span data-stu-id="f8cc5-263">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="f8cc5-264">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="f8cc5-264">ValidationSummary.All</span></span>|<span data-ttu-id="f8cc5-265">Vlastnost a model úroveň</span><span class="sxs-lookup"><span data-stu-id="f8cc5-265">Property and model level</span></span>|
|<span data-ttu-id="f8cc5-266">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="f8cc5-266">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="f8cc5-267">Model</span><span class="sxs-lookup"><span data-stu-id="f8cc5-267">Model</span></span>|
|<span data-ttu-id="f8cc5-268">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="f8cc5-268">ValidationSummary.None</span></span>|<span data-ttu-id="f8cc5-269">Žádné</span><span class="sxs-lookup"><span data-stu-id="f8cc5-269">None</span></span>|

### <a name="sample"></a><span data-ttu-id="f8cc5-270">Ukázka</span><span class="sxs-lookup"><span data-stu-id="f8cc5-270">Sample</span></span>

<span data-ttu-id="f8cc5-271">V následujícím příkladu je model dat označených pomocí `DataAnnotation` atributy, které generuje chybové zprávy ověření na `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-271">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="f8cc5-272">Když dojde k chybě ověření, zobrazí pomocná rutina pro ověření značky chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-272">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="f8cc5-273">Vygenerovaný HTML (když je model platný):</span><span class="sxs-lookup"><span data-stu-id="f8cc5-273">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="f8cc5-274">Vyberte značku pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="f8cc5-274">The Select Tag Helper</span></span>

* <span data-ttu-id="f8cc5-275">Generuje [vyberte](https://www.w3.org/wiki/HTML/Elements/select) a přidružených [možnost](https://www.w3.org/wiki/HTML/Elements/option) prvky pro vlastnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-275">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="f8cc5-276">Má alternativu pomocné rutiny HTML `Html.DropDownListFor` a`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="f8cc5-276">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="f8cc5-277">`Select Tag Helper` `asp-for` Určuje název vlastnosti modelu pro [vyberte](https://www.w3.org/wiki/HTML/Elements/select) elementu a `asp-items` Určuje [možnost](https://www.w3.org/wiki/HTML/Elements/option) elementy.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-277">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="f8cc5-278">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-278">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="f8cc5-279">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-279">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="f8cc5-280">`Index` Metoda inicializuje `CountryViewModel`, nastaví vybrané země a předává jej do `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-280">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="f8cc5-281">HTTP POST `Index` metoda zobrazí výběr:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-281">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="f8cc5-282">`Index` Zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-282">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="f8cc5-283">Které generuje následující HTML (písmeny "CA" vybraná):</span><span class="sxs-lookup"><span data-stu-id="f8cc5-283">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="f8cc5-284">Nedoporučujeme používat `ViewBag` nebo `ViewData` Pomocník vyberte značky.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-284">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="f8cc5-285">Model zobrazení je robustnější na poskytování metadat MVC a obecně menší problém.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-285">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="f8cc5-286">`asp-for` Hodnota atributu je zvláštní případ a nevyžaduje `Model` předpony, další atributy do pomocné rutiny značky (například `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="f8cc5-286">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="f8cc5-287">Vazba výčtu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-287">Enum binding</span></span>

<span data-ttu-id="f8cc5-288">Často je možné použít `<select>` s `enum` vlastnost a generovat `SelectListItem` elementy z `enum` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-288">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="f8cc5-289">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-289">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="f8cc5-290">`GetEnumSelectList` Metoda generuje `SelectList` objekt výčtu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-290">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="f8cc5-291">Můžete uspořádání seznamu enumerátor s `Display` atribut získat bohatší uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-291">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="f8cc5-292">Následující kód HTML se vygeneruje:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-292">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="f8cc5-293">Možnost skupiny</span><span class="sxs-lookup"><span data-stu-id="f8cc5-293">Option Group</span></span>

<span data-ttu-id="f8cc5-294">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) element se vygeneruje, když model zobrazení obsahuje jeden nebo více `SelectListGroup` objekty.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-294">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="f8cc5-295">`CountryViewModelGroup` Skupin `SelectListItem` elementy do skupiny "Severní Americe" a "Evropa":</span><span class="sxs-lookup"><span data-stu-id="f8cc5-295">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="f8cc5-296">Níže jsou uvedeny dvě skupiny:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-296">The two groups are shown below:</span></span>

![Příklad skupiny možnost](working-with-forms/_static/grp.png)

<span data-ttu-id="f8cc5-298">Generovaný kód HTML:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-298">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="f8cc5-299">Více vyberte</span><span class="sxs-lookup"><span data-stu-id="f8cc5-299">Multiple select</span></span>

<span data-ttu-id="f8cc5-300">Značka pomocná vyberte automaticky vygeneruje [více = "více"](http://w3c.github.io/html-reference/select.html) -li zadána vlastnost v atributu `asp-for` atribut je `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-300">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="f8cc5-301">Například uděleno model následující:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-301">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="f8cc5-302">Pomocí následující zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-302">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="f8cc5-303">Generuje následující HTML:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-303">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="f8cc5-304">Žádná výběru</span><span class="sxs-lookup"><span data-stu-id="f8cc5-304">No selection</span></span>

<span data-ttu-id="f8cc5-305">Pokud se přistihnete pomocí možnosti "nebyl zadán" na více stránkách, můžete vytvořit šablonu k odstranění opakovaných HTML:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-305">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="f8cc5-306">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* šablony:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-306">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="f8cc5-307">Přidání HTML [ \<možnost >](https://www.w3.org/wiki/HTML/Elements/option) není omezen na elementy *žádná výběru* případu.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-307">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="f8cc5-308">Například následující metodu zobrazení a akce se generují kód HTML podobně jako výše uvedený kód:</span><span class="sxs-lookup"><span data-stu-id="f8cc5-308">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="f8cc5-309">Správný `<option>` element bude vybrána (obsahovat `selected="selected"` atribut) v závislosti na aktuální `Country` hodnota.</span><span class="sxs-lookup"><span data-stu-id="f8cc5-309">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="f8cc5-310">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f8cc5-310">Additional resources</span></span>

* [<span data-ttu-id="f8cc5-311">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="f8cc5-311">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f8cc5-312">Element formuláře HTML</span><span class="sxs-lookup"><span data-stu-id="f8cc5-312">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="f8cc5-313">Žádost o ověření tokenu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-313">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* [<span data-ttu-id="f8cc5-314">Vazby modelu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-314">Model Binding</span></span>](xref:mvc/models/model-binding)
* [<span data-ttu-id="f8cc5-315">Ověření modelu</span><span class="sxs-lookup"><span data-stu-id="f8cc5-315">Model Validation</span></span>](xref:mvc/models/validation)
* [<span data-ttu-id="f8cc5-316">IAttributeAdapter rozhraní</span><span class="sxs-lookup"><span data-stu-id="f8cc5-316">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="f8cc5-317">Fragmenty kódu pro tento dokument</span><span class="sxs-lookup"><span data-stu-id="f8cc5-317">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)
